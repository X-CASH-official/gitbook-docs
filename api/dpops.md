# X-Cash DPOPS API

Note zachys (atomic units) are 10^6 in X-Cash

## Stats <a id="stats"></a>

This method gets the stats

**URL**: [https://api.xcash.foundation/v1/xcash/dpops/unauthorized/stats/](https://api.xcash.foundation/v1/xcash/dpops/unauthorized/stats/)

**Method**: GET

**Inputs**: _None_.

**Results**:

* _mostTotalRoundsDelegateName_ - string; The delegate name that has participated in the most X-Cash DPOPS rounds.
* _mostTotalRounds_ - unsigned int; The most total rounds.
* _bestBlockVerifierOnlinePercentageDelegateName_ - string; The delegate with the best online percentage.
* _bestBlockVerifierOnlinePercentage_ - unsigned int; The best online percentage.
* _mostBlockProducerTotalRoundsDelegateName_ - string; The delegate will the most block producer rounds.
* _mostBlockProducerTotalRounds_ - unsigned int; Total most block producer rounds.
* _totalVotes_ - unsigned long long; Total votes in zachys (atomic units).
* _totalVoters_ - unsigned int; Total voters.
* _averageVote_ - unsigned int; Average vote amount in zachys (atomic units).
* _votePercentage_ - unsigned int; The percentage of the amount of xcash ciruclating compared to how much is being voted.
* _roundNumber_ - unsigned int; The round number of X-Cash DPOPS.
* _totalRegisteredDelegates_ - unsigned int; Total registered delegates.
* _totalOnlineDelegates_ - unsigned int; Total online delegates.
* _currentBlockVerifiersMaximumAmount_ - unsigned int; The current amount of delegates that the system will use.
* _currentBlockVerifiersValidAmount_ - unsigned int; The current amount of delegates that is needed for verification.

```bash
$ curl -X GET https://api.xcash.foundation/v1/xpayment-twitter/twitter/unauthorized/stats/ -H 'Accept: application/json'
{
  "mostTotalRoundsDelegateName": "europe3_xcash_foundation",
  "mostTotalRounds": 121528,
  "bestBlockVerifierOnlinePercentageDelegateName": "OnlyOne",
  "bestBlockVerifierOnlinePercentage": 100,
  "mostBlockProducerTotalRoundsDelegateName": "snakeway",
  "mostBlockProducerTotalRounds": 2899,
  "totalVotes": 38436931311445482,
  "totalVoters": 1000,
  "averageVote": 38436931311445,
  "votePercentage": 57,
  "roundNumber": 131677,
  "totalRegisteredDelegates": 150,
  "totalOnlineDelegates": 100,
  "currentBlockVerifiersMaximumAmount": 50,
  "currentBlockVerifiersValidAmount": 27
}
```

## Registered Delegates <a id="registered-delegates"></a>

This method gets all of the registered delegates

**URL**: [https://api.xcash.foundation/v1/xcash/dpops/unauthorized/delegates/registered](https://api.xcash.foundation/v1/xcash/dpops/unauthorized/delegates/registered)

**Method**: GET

**Inputs**: _None_.

**Results**:

Array of objects with the following structure:

* _votes_ -  unsigned long long; Total votes in zachys (atomic units).
* _voters_ -  unsigned int; Total voters.
* _IPAdress_ - string; The IP address or domain name.
* _delegateName_ - string; The delegate name.
* _sharedDelegate_ - bool;
* _seedNode_ - bool;
* _online_ - bool;
* _fee_ - unsigned int; The fee.
* _totalRounds_ - unsigned int; Total rounds.
* _totalBlockProducerRounds_ - unsigned int; Total block producer rounds.
* _onlinePercentage_ - unsigned int; The online percentage.

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/dpops/unauthorized/delegates/registered/ -H 'Accept: application/json'
[
  {
    "votes": 100000000,
    "voters": 10,
    "IPAdress": "us1.xcash.foundation",
    "delegateName": "us1_xcash_foundation",
    "sharedDelegate": false,
    "seedNode": true,
    "online": true,
    "fee": 0,
    "totalRounds": 100,
    "totalBlockProducerRounds": 10,
    "onlinePercentage": 67
  },
  {
    "votes": 0,
    "voters": 10,
    "IPAdress": "europe1.xcash.foundation",
    "delegateName": "europe1_xcash_foundation",
    "sharedDelegate": true,
    "seedNode": false,
    "online": true,
    "fee": 5,
    "totalRounds": 100,
    "totalBlockProducerRounds": 10,
    "onlinePercentage": 99
  }
]
```

## Online Delegates <a id="online-delegates"></a>

This method gets all of the online delegates

**URL**: [https://api.xcash.foundation/v1/xcash/dpops/unauthorized/delegates/online](https://api.xcash.foundation/v1/xcash/dpops/unauthorized/delegates/online)

**Method**: GET

**Inputs**: _None_.

**Results**:

Array of objects with the following structure:

* _votes_ -  unsigned long long; Total votes in zachys (atomic units).
* _voters_ -  unsigned int; Total voters.
* _IPAdress_ - string; The IP address or domain name.
* _delegateName_ - string; The delegate name.
* _sharedDelegate_ - bool;
* _seedNode_ - bool;
* _online_ - bool;
* _fee_ - unsigned int; The fee.
* _totalRounds_ - unsigned int; Total rounds.
* _totalBlockProducerRounds_ - unsigned int; Total block producer rounds.
* _onlinePercentage_ - unsigned int; The online percentage.

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/dpops/unauthorized/delegates/online/ -H 'Accept: application/json'
[
  {
    "votes": 100000000,
    "voters": 10,
    "IPAdress": "us1.xcash.foundation",
    "delegateName": "us1_xcash_foundation",
    "sharedDelegate": false,
    "seedNode": true,
    "online": true,
    "fee": 0,
    "totalRounds": 100,
    "totalBlockProducerRounds": 10,
    "onlinePercentage": 67
  },
  {
    "votes": 0,
    "voters": 10,
    "IPAdress": "europe1.xcash.foundation",
    "delegateName": "europe1_xcash_foundation",
    "sharedDelegate": true,
    "seedNode": false,
    "online": true,
    "fee": 5,
    "totalRounds": 100,
    "totalBlockProducerRounds": 10,
    "onlinePercentage": 99
  }
]
```

## Active Delegates <a id="active-delegates"></a>

This method gets the current active delegates (top 50) during the round

**URL**: [https://api.xcash.foundation/v1/xcash/dpops/unauthorized/delegates/active](https://api.xcash.foundation/v1/xcash/dpops/unauthorized/delegates/active)

**Method**: GET

**Inputs**: _None_.

**Results**:

Array of objects with the following structure:

* _votes_ -  unsigned long long; Total votes in zachys (atomic units).
* _voters_ -  unsigned int; Total voters.
* _IPAdress_ - string; The IP address or domain name.
* _delegateName_ - string; The delegate name.
* _sharedDelegate_ - bool;
* _seedNode_ - bool;
* _online_ - bool;
* _fee_ - unsigned int; The fee.
* _totalRounds_ - unsigned int; Total rounds.
* _totalBlockProducerRounds_ - unsigned int; Total block producer rounds.
* _onlinePercentage_ - unsigned int; The online percentage.

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/dpops/unauthorized/delegates/active/ -H 'Accept: application/json'
[
  {
    "votes": 100000000,
    "voters": 10,
    "IPAdress": "us1.xcash.foundation",
    "delegateName": "us1_xcash_foundation",
    "sharedDelegate": false,
    "seedNode": true,
    "online": true,
    "fee": 0,
    "totalRounds": 100,
    "totalBlockProducerRounds": 10,
    "onlinePercentage": 67
  },
  {
    "votes": 0,
    "voters": 10,
    "IPAdress": "europe1.xcash.foundation",
    "delegateName": "europe1_xcash_foundation",
    "sharedDelegate": true,
    "seedNode": false,
    "online": true,
    "fee": 5,
    "totalRounds": 100,
    "totalBlockProducerRounds": 10,
    "onlinePercentage": 99
  }
]
```

## Delegates <a id="delegates"></a>

This method gets the delegates data

**URL**: [https://api.xcash.foundation/v1/xcash/dpops/unauthorized/delegates/{delegateName}](https://api.xcash.foundation/v1/xcash/dpops/unauthorized/delegates/{delegateName})

**Method**: GET

**Resources**:
* _delegateName_ - **Required** - The delegates name.

**Inputs**: _None_.

**Results**:

* _votes_ -  unsigned long long; Total votes in zachys (atomic units).
* _voters_ -  unsigned int; Total voters.
* _IPAdress_ - string; The IP address or domain name.
* _delegateName_ - string; The delegate name.
* _publicAddress_ - string; The public address.
* _about_ - string; about.
* _website_ - string; website.
* _team_ - string; team.
* _specifications_ - string; server Specifications.
* _sharedDelegate_ - bool;
* _seedNode_ - bool;
* _online_ - bool;
* _fee_ - unsigned int; The fee.
* _totalRounds_ - unsigned int; Total rounds.
* _totalBlockProducerRounds_ - unsigned int; Total block producer rounds.
* _onlinePercentage_ - unsigned int; The online percentage.
* _rank_ - unsigned int; The delegates current rank.

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/dpops/unauthorized/delegates/us1_xcash_foundation/ -H 'Accept: application/json'
{
  "votes": 100000000,
  "voters": 10,
  "IPAdress": "us1.xcash.foundation",
  "delegateName": "us1_xcash_foundation",
  "publicAddress": "XCA1a9usG2UKajV1Dqzp8fL1BbN3hzuaaJMYjCo7qDoC4C3Vvc5owiLAqKbVw2cRbwRqx3mgrau1Z7LkX6cxR2NC4ZmFBLe2Mf",
  "about": "",
  "website": "",
  "team": "",
  "specifications": "",
  "sharedDelegate": false,
  "seedNode": true,
  "online": true,
  "fee": 0,
  "totalRounds": 100,
  "totalBlockProducerRounds": 10,
  "onlinePercentage": 67,
  "rank": 1
}
```

## Delegate Rounds Stats <a id="delegate-round-stats"></a>

This method gets the stats about the blocks the delegate produced

**URL**: [https://api.xcash.foundation/v1/xcash/dpops/unauthorized/delegates/rounds/{delegateName}/{start}/{limit}](https://api.xcash.foundation/v1/xcash/dpops/unauthorized/delegates/rounds/{delegateName}/{start}/{limit})

**Method**: GET

**Resources**:
* _delegateName_ - **Required** - The delegate name.
* _start_ - **Required** - The start item to return.
* _limit_ - Not required - The maximum amount of items to return (Default is 288, Maximum is 288).

**Inputs**: _None_.

**Results**:

* _totalBlocksProduced_ - unsigned int; The total blocks produced by the delegate
* _totalBlockRewards_ - unsigned long long; The total xcash from the blocks produced in zachys (atomic units).
* _averagePercentage_ - unsigned int; The average the delegate has produced a block. (100 is average, 200 is twice as good etc etc)
* _averageTime_ - unsigned int; The average time (in minutes) it takes for the delegate to produce a block.
* _blocksProduced_ - an arrray with the following structure:
  * _blockHeight_ - unsigned int; The block height. 
  * _blockReward_ - unsigned long long; The block reward in zachys (atomic units).
  * _time_ - unsigned int; The time stamp.

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/dpops/unauthorized/delegates/rounds/us1_xcash_foundation/1/2/ -H 'Accept: application/json'
{
  "totalBlocksProduced": 100,
  "totalBlockRewards": 1000000000,
  "averagePercentage": 100,
  "averageTime": 500,
  "blocksProduced": [
    {
      "blockHeight": 810000,
      "blockReward": 100000000,
      "time": 1654228489 
    },
    {
      "blockHeight": 811000,
      "blockReward": 100000000,
      "time": 1654228489
    }
  ]
}
```

## Delegates Votes <a id="delegates-votes"></a>

This method gets the vote data for a delegate

**URL**: [https://api.xcash.foundation/v1/xcash/dpops/unauthorized/delegates/votes/{delegateName}/{start}/{limit}](https://api.xcash.foundation/v1/xcash/dpops/delegates/votes/{delegateName}/{start}/{limit})

**Method**: GET

**Resources**:
* _delegateName_ - **Required** - The delegate name.
* _start_ - Not required - The start item to return data (Default is 0, the first item).
* _limit_ - Not required - The maximum amount of items to return (Default is all).

**Inputs**: _None_.

**Results**:

An array of objects with the following structure:

* _publicAddress_ - string; The public address who created the vote.
* _amount_ - unsigned long long; The vote amount in zachys (atomic units).
* _reserveProof_ - string; The reserve proof.

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/dpops/unauthorized/delegates/votes/{delegateName}/1/2/ -H 'Accept: application/json'
[
  {
    "publicAddress": "XCA1a9usG2UKajV1Dqzp8fL1BbN3hzuaaJMYjCo7qDoC4C3Vvc5owiLAqKbVw2cRbwRqx3mgrau1Z7LkX6cxR2NC4ZmFBLe2Mf",
    "amount": 1000000000,
    "reserveProof": "ReserveProofV1"
  },
  {
    "publicAddress": "XCA1a9usG2UKajV1Dqzp8fL1BbN3hzuaaJMYjCo7qDoC4C3Vvc5owiLAqKbVw2cRbwRqx3mgrau1Z7LkX6cxR2NC4ZmFBLe2Mf",
    "amount": 1000000000,
    "reserveProof": "ReserveProofV1"
  }
]
```

## Vote Details <a id="vote-details"></a>

This method gets the vote data from a specific address

**URL**: [https://api.xcash.foundation/v1/xcash/dpops/unauthorized/votes/{address}](https://api.xcash.foundation/v1/xcash/dpops/unauthorized/votes/{address})

**Method**: GET

**Resources**:
* _address_ - **Required** - The public address.

**Inputs**: _None_.

**Results**:

* _delegateName_ - string; The delegate name voted for.
* _publicAddress_ - string; The public address of the delegate.
* _amount_ - unsigned long long; The vote amount in zachys (atomic units).

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/dpops/unauthorized/votes/XCA1a9usG2UKajV1Dqzp8fL1BbN3hzuaaJMYjCo7qDoC4C3Vvc5owiLAqKbVw2cRbwRqx3mgrau1Z7LkX6cxR2NC4ZmFBLe2Mf/ -H 'Accept: application/json'
{
  "delegateName": "us1_xcash_foundation",
  "publicAddress": "XCA1a9usG2UKajV1Dqzp8fL1BbN3hzuaaJMYjCo7qDoC4C3Vvc5owiLAqKbVw2cRbwRqx3mgrau1Z7LkX6cxR2NC4ZmFBLe2Mf",
  "amount": 1000000000
}
```

## Round Details <a id="round-details"></a>

This method gets the round details

**URL**: [https://api.xcash.foundation/v1/xcash/dpops/unauthorized/rounds/{blockHeight}](https://api.xcash.foundation/v1/xcash/dpops/unauthorized/rounds/{blockHeight})

**Method**: GET

**Resources**:
* _blockHeight_ - **Required** - The block height for the round.

**Inputs**: _None_.

**Results**:

An array of delegate names that verified the block

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/dpops/unauthorized/rounds/810000/ -H 'Accept: application/json'
{
  "delegates": [
    "us1_xcash_foundation",
    "europe1_xcash_foundation"
  ]
}
```

## Last Block Producer <a id="last-block-producer"></a>

This method gets the last block producer

**URL**: [https://api.xcash.foundation/v1/xcash/dpops/unauthorized/lastBlockProducer](https://api.xcash.foundation/v1/xcash/dpops/unauthorized/lastBlockProducer)

**Method**: GET

**Inputs**: _None_.

**Results**:

* _lastBlockProducer_ - The last block producer.

```bash
$ curl -X GET https://api.xcash.foundation/v1/xcash/dpops/unauthorized/lastBlockProducer/ -H 'Accept: application/json'
{
  "lastBlockProducer": "us1_xcash_foundation"
}
```
