# X-payment Twitter API

Note zachys (atomic units) are 10^6 in X-Cash

## User Stats <a id="user-stats"></a>

This method gets the stats for a specific user

**URL**: [https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/{user}/stats/](https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/{user}/stats/)

**Method**: GET

**Resources**:
* _user_ - **Required** - the username.

**Inputs**: _None_.

**Results**:

* _totalSentTips_ - unsigned int; Total sent Tips.
* _totalReceivedTips_ - unsigned int; Total received tips.
* _totalPublicSentTips_ - unsigned int; Total Public sent Tips.
* _totalPrivateSentTips_ - unsigned int; Total Private sent tips.
* _totalDeposits_ - unsigned int; Total deposits.
* _totalWithdraws_ - unsigned int; Total withdraws.

```bash
$ curl -X GET https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/{user}/stats/ -H 'Accept: application/json'
{
  "totalSentTips": 7,
  "totalReceivedTips": 10,
  "totalPublicSentTips": 5,
  "totalPrivateSentTips": 2,
  "totalDeposits": 5,
  "totalWithdraws": 7
}
```

## Stats <a id="stats"></a>

This method gets the stats

**URL**: [https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/stats/](https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/stats/)

**Method**: GET

**Inputs**: _None_.

**Results**:

* _totalUsers_ - unsigned int; Total users registered.
* _totalTipsPublic_ - unsigned long long; Total public tips sent.
* _totalTipsPrivate_ - unsigned long long; Total private tips sent.
* _totalVolumeSent_ - unsigned long long; Total volume sent in zachys (atomic units).
* _avgTipAmount_ - unsigned int; Average tip amount sent in zachys (atomic units).
* _tipsSentLastHour_ - unsigned int; Total tips sent in the last hour from the current time.
* _tipsSentLast24Hours_ - unsigned int; Total tips sent in the last 24 hours from the current time.
* _volumeSentLastHour_ - unsigned long long; Total volume sent in the last hour from the current time.
* _volumeSentLast24Hours_ - unsigned long long; Total volume sent in the last 24 hours from the current time.
* _totalDeposits_ - unsigned int; Total deposits.
* _totalWithdraws_ - unsigned int; Total withdraws.

```bash
$ curl -X GET https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/stats/ -H 'Accept: application/json'
{
  "totalUsers": 7,
  "totalTipsPublic": 10,
  "totalTipsPrivate": 5,
  "totalVolumeSent": 17000000,
  "avgTipAmount": 130769231,
  "tipsSentLastHour": 7,
  "tipsSentLast24Hours": 7,
  "volumeSentLastHour": 100,
  "volumeSentLast24Hours": 1000,
  "totalDeposits": 100,
  "totalWithdraws": 100
}
```

## Stats Per Day <a id="stats-per-day"></a>

This method gets the daily amount of payments and volumes sent per day

**URL**: [https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/statsPerDay/{start}/{limit}](https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/statsPerDay/{start}/{limit})

**Method**: GET

**Resources**:
* _start_ - Not required - The start day to return data (Default is 1, the first start day).
* _limit_ - Not required - The maximum amount of days to return (Default is all).

**Inputs**: _None_.

**Results**:

Array of objects with the following structure:

* _time_ -  unsigned int; Total users registered.
* _amount_ - unsigned long long; Total tips sent.
* _volume_ - unsigned long long; Total volume sent in zachys (atomic units).

```bash
$ curl -X GET https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/statsPerDay/1/2/ -H 'Accept: application/json'
[
  {
    "time": 1654228489,
    "amount": 100,
    "volume": 100000000
  },
  {
    "time": 1654228489,
    "amount": 100,
    "volume": 100000000
  }
]
```

## Top Stats <a id="top-stats"></a>

This method gets the top users for tips and volume

**URL**: [https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/topStats/{amount}](https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/topStats/{amount})

**Method**: GET

**Resources**:
* _amount_ - not required - The amount of items to return (Default is 10).

**Inputs**: _None_.

**Results**:

* _topTips_ - Array of objects with the following structure:
  * _username_ -  string; The username.
  * _tips_ - unsigned int; Total tips sent.
* _topVolumes_ - Array of objects with the following structure:
  * _username_ -  string; The username.
  * _volume_ - unsigned long long; Total volume sent in zachys (atomic units).

```bash
$ curl -X GET https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/topStats/2 -H 'Accept: application/json'
"topTips": [
  {
    "username": "test1",
    "tips": 105
  },
  {
    "username": "test2",
    "tips": 100
  }
],
"topVolumes": [
  {
    "username": "test1",
    "volume": 105000000
  },
  {
    "username": "test2",
    "volume": 100000000
  }
]
```

## Recent Tips <a id="recent-tips"></a>

This method gets the recent tips

**URL**: [https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/recentTips/{amount}](https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/recentTips/{amount})

**Method**: POST

**Resources**:
* _amount_ - not required - The amount of items to return (Default is 10).

**Inputs (All are required)**:

* _sort_ - "First" for most recent tips, "Last" for the least recent tips.
* _type_ - "Public" for only public transactions, "Private" for only private transactions, "All" for both.

**Results**:

Array of objects with the following structure:

* _tweetId_ - string; The tweet id.
* _fromUser_ - string; The username who sent the tip.
* _toUser_ - string; The username who received the tip.
* _amount_ - unsigned long long; The anount of the tip in zachys (atomic units).
* _time_ -  unsigned int; The time.
* _type_ - string; The tip type.

```bash
$ curl -X POST https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/recentTips/2 -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'Content-Type: application/json'  -d '{"sort":"First","type":"All"}'
[
  {
    "tweetId": ""
    "fromUser": "",
    "toUser": "",
    "amount": 0,
    "time": 1654204410,
    "type": "private"
  },
  {
    "tweetId": "1531918830276075521"
    "fromUser": "test1",
    "toUser": "test",
    "amount": 5000000,
    "time": 1654195778,
    "type": "public"
  }
]
```

## Tips <a id="tips"></a>

This method gets the tips history

**URL**: [https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/tips/{start}/{limit}](https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/tips/{start}/{limit})

**Method**: POST

**Resources**:
* _start_ - Not required - The start tip to return (Default is 1, the first tip).
* _limit_ - Not required - The maximum amount of tips to return (Default is all).

**Inputs (None are required)**:

* _from_ - Filter by specific username.
* _to_ - Filter by specific username.
* _tweetId_ - Filter by specific tweetId. Note this will always return a maximum of 1 tip and override the amount parameter

**Results**:

Array of objects with the following structure:

* _tweetId_ - string; The tweet id.
* _fromUser_ - string; The username who sent the tip.
* _fromId_ - string; The user id who sent the tip.
* _toUser_ - string; The username who received the tip.
* _toId_ - string; The user id who received the tip.
* _amount_ - unsigned long long; The anount of the tip in zachys (atomic units).
* _time_ -  unsigned int; The time.
* _type_ - string; The tip type.

```bash
$ curl -X POST https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/tips/2 -H 'Content-Type: application/json' -H 'Accept: application/json' -H 'Content-Type: application/json'  -d '{"from":"x_payment"}'
[
  {
    "tweetId": ""
    "fromUser": "",
    "fromId": "",
    "toUser": "",
    "toId": "",
    "amount": 0,
    "time": 1654204410,
    "type": "private"
  },
  {
    "tweetId": "1531918830276075521"
    "fromUser": "test1",
    "fromId": "000000000",
    "toUser": "test",
    "toId": "000000000",
    "amount": 5000000,
    "time": 1654195778,
    "type": "public"
  }
]
```
