# X-payment API

Note zachys (atomic units) are 10^6 in X-Cash

## Stats <a id="stats"></a>

This method gets the stats

**URL**: [https://api.xcash.foundation/v1/xpayment/unauthorized/stats/](https://api.xcash.foundation/v1/xpayment/unauthorized/stats/)

**Method**: GET

**Inputs**: _None_.

**Results**:

* _totalUsers_ - unsigned int; Total users registered.
* _avgTipAmount_ - unsigned int; Average tip amount sent in zachys (atomic units).
* _totalDeposits_ - unsigned int; Total deposits.
* _totalWithdraws_ - unsigned int; Total withdraws.
* _totalTipsPublic_ - unsigned int; Total public tips sent.
* _totalTipsPrivate_ - unsigned int; Total private tips sent.
* _totalVolumeSentPublic_ - unsigned long long; Total volume sent for public tips, in zachys (atomic units).
* _totalVolumeSentPrivate_ - unsigned long long; Total volume sent for private tips, in zachys (atomic units).
* _totalTipsLastDayPublic_ - unsigned int; Total public tips sent in last day.
* _totalTipsLastDayPrivate_ - unsigned int; Total private tips sent in last day.
* _totalVolumeSentLastDayPublic_ - unsigned long long; Total volume sent for public tips in last day, in zachys (atomic units).
* _totalVolumeSentLastDayPrivate_ - unsigned long long; Total volume sent for private tips in last day, in zachys (atomic units).
* _totalTipsLastHourPublic_ - unsigned int; Total public tips sent in last hour.
* _totalTipsLastHourPrivate_ - unsigned int; Total private tips sent in last hour.
* _totalVolumeSentLastHourPublic_ - unsigned long long; Total volume sent for public tips in last hour, in zachys (atomic units).
* _totalVolumeSentLastHourPrivate_ - unsigned long long; Total volume sent for private tips in last hour, in zachys (atomic units).

```bash
$ curl -X GET https://api.xcash.foundation/v1/xpayment/unauthorized/stats/ -H 'Accept: application/json'
{
  "totalUsers": 7,
  "avgTipAmount": 10,
  "totalDeposits": 5,
  "totalWithdraws": 17,
  "totalTipsPublic": 7,
  "totalTipsPrivate": 10,
  "totalVolumeSentPublic": 500000000,
  "totalVolumeSentPrivate": 1000000000,
  "totalTipsLastDayPublic": 7,
  "totalTipsLastDayPrivate": 10,
  "totalVolumeSentLastDayPublic": 500000000,
  "totalVolumeSentLastDayPrivate": 1000000000,
  "totalTipsLastHourPublic": 7,
  "totalTipsLastHourPrivate": 10,
  "totalVolumeSentLastHourPublic": 500000000,
  "totalVolumeSentLastHourPrivate": 1000000000
}
```

## Stats Per Day <a id="stats-per-day"></a>

This method gets the daily amount of payments and volumes sent per day

**URL**: [https://api.xcash.foundation/v1/xpayment/unauthorized/statsPerDay/{start}/{limit}](https://api.xcash.foundation/v1/xpayment/unauthorized/statsPerDay/{start}/{limit})

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

**URL**: [https://api.xcash.foundation/v1/xpayment/unauthorized/topStats/{amount}](https://api.xcash.foundation/v1/xpayment/unauthorized/topStats/{amount})

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
{
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
}
```

## Recent Tips <a id="recent-tips"></a>

This method gets the recent tips

**URL**: [https://api.xcash.foundation/v1/xpayment/unauthorized/recentTips/{amount}](https://api.xcash.foundation/v1/xpayment/unauthorized/recentTips/{amount})

**Method**: POST

**Resources**:
* _amount_ - not required - The amount of items to return (Default is 10).

**Inputs**:

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
$ curl -X POST https://api.xcash.foundation/v1/xpayment/unauthorized/recentTips/2 -H 'Content-Type: application/json' -H 'Accept: application/json'  -H 'Content-Type: application/json'  -d '{"sort":"First","type":"All"}'
[
  {
    "tweetId": "",
    "fromUser": "",
    "toUser": "",
    "amount": 0,
    "time": 1654204410,
    "type": "private"
  },
  {
    "tweetId": "1531918830276075521",
    "fromUser": "test1",
    "toUser": "test",
    "amount": 5000000,
    "time": 1654195778,
    "type": "public"
  }
]
```
