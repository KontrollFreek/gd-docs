# Rewards CHK

Requests that involve rewards use a different style of CHK, which requires the server to respond to a challenge sent by the client.

When the game begins, a random integer between 0 and 1 million is generated and incremented by the result of dividing the player's user ID by 10 thousand. This value is stored globally in the GameManager, but is not saved between restarts.

```c++
this->m_chk = static_cast<int>(this->m_playerUserID/10000.0f + rand()/RAND_MAX * 1000000.0f);
```

When making a request to a rewards endpoint, this number is converted to an integer and [XOR'd](xor.md) by either the daily challenges or chest reward key, depending on endpoint. This value is converted into [base64](base64.md).

Afterwards, a random string of length 5 is generated, and the base64 encoded chk is appended to this string. This is the final chk sent to the servers.

This process is modeled with the following C++ code:

```c++
auto chkString = std::format("{}", GameManager::sharedState()->m_chk);
auto encodedChk = cocos2d::ZipUtils::base64EncodeEnc(chkString, "19847");

// gen_random(n) returns a random string of length n using the alphabet [a-zA-Z0-9]
auto chkParam = std::format("chk={}{}", gen_random(5), encodedChk);
```

## Verification

When the request is made, the server extracts the GameManager's chk number from the given chk parameter. It also generates a random 5 character string. The chk number and 5 character string is also used as part of the [hash](/resources/server/hashes.md). An unencoded "hash" is sent with only [base64](base64.md) and [XOR](xor.md) being applied (prior to SHA1/salting). This unencoded hash allows the client to verify the hash sent by the servers.
