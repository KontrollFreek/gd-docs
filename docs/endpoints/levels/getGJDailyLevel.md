# getGJDailyLevel.php

Gets which daily level we're on and gets how much time is left.

## Parameters

### Required Parameters

**secret** - Wmfd2893gb7

### Optional Parameters

**gameVersion** - 22

**binaryVersion** - 47

**dvs** - 3

**accountID** - The user's account ID

**gjp2** - The user's [GJP2](/topics/gjp.md)

**type** - 0 for daily, 1 for weekly, 2 for event level.

**chk** - (required for type 2) 5 random chars appended to the beginning of a random number [XOR](/topics/encryption/xor.md)'d and [URL-Safe Base64](/topics/encryption/base64.md) encoded. However, currently any string longer than 5 characters just works

**weekly** - 0 for daily, 1 for weekly. Defaults to 0 if not sent. This parameter is outdated since 2.207

## Response

Returns the index of the current daily level and the time left in seconds, separated by a pipe `|`.

Event levels also return the reward chest data and an integrity hash. Time left will always be `10` for event levels.

## Example

<!-- tabs:start -->

### **Python**

```py
import requests
url = "http://www.boomlings.com/database/getGJDailyLevel.php"
data = {
    "secret": "Wmfd2893gb7",
    "type": "2"
}
headers = {
    "User-Agent": ""  # Empty User-Agent
}

response = requests.post(url, data=data, headers=headers)
print(response.text)
```

### **curl**

```plain
curl -X POST http://www.boomlings.com/database/getGJDailyLevel.php -d "secret=Wmfd2893gb7&type=2" -A ""
```

<!-- tabs:end -->

**Response**
```py
2959|19186
```

<!-- tabs:end -->
