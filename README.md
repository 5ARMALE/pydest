# What is Pydest?

Pydest is an asynchronous API wrapper for Destiny 2 written in Python. The goal of the project is fully support the Destiny 2 API while abstracting the details of formulating and making the request away from the user.

Here's a quick example of Pydest in action (assuming this code is running in an event loop):

```
import aiohttp
import pydest

with pydest.API('your-api-key') as destiny:
    response = await destiny.search_destiny_player(1, 'slayer117')
```

## Supported Endpoints

- GET: Destiny2.GetDestinyManifest
- GET: Destiny2.SearchDestinyPlayer
- GET: Destiny2.GetProfile
- GET: Destiny2.GetCharacter
- GET: Destiny2.GetClanWeeklyRewardState
- GET: Destiny2.GetItem
- GET: Destiny2.GetPostGameCarnageReport
- GET: Destiny2.GetHistoricalStatsDefinition
- GET: Destiny2.GetPublicMilestoneContent
- GET: Destiny2.GetPublicMilestones

All GET endpoints that are not in a preview state are supported. I will add support for the other GET endpoints when they leave the preview state. I also plan to add the POST endpoints when I have a better understanding of how they work.

For a detailed description of each endpoint, refer to the [official documentation](https://bungie-net.github.io/multi/index.html).

## Installation

```
$ git clone https://github.com/jgayfer/pydest
$ cd pydest
$ python3 -m pip install -U .
```

## Usage

In the [official documentation](https://bungie-net.github.io/multi/index.html), the endpoints have two types of parameters: **Path Parameters** and **Querystring Parameters**. Path parameters should be used as arguments in the same order as they are presented in the official docs. While querystring parameters must be the last argument, given as a Python list object.

For example, say you want to call [Destiny.GetProfile](https://bungie-net.github.io/multi/operation_get_Destiny2-GetProfile.html#operation_get_Destiny2-GetProfile). The path parameters are **destinyMembershipId** of type int, and **membershipType** of type [BungieMembershipType](https://bungie-net.github.io/multi/schema_BungieMembershipType.html#schema_BungieMembershipType). The querystring parameter, **components**, is an array of type [Destiny.DestinyComponentType](https://bungie-net.github.io/multi/schema_Destiny-DestinyComponentType.html#schema_Destiny-DestinyComponentType).

We could make this request with Pydest as follows:
```
membership_id = '12345'
membership_type = 1
components = ['Characters', 'VendorReceipts']

with pydest.API('your-api-key') as destiny:
    res = await destiny.get_profile(membership_id, membership_type, components)
```

Special care have been taken to keep all arguments in the same order that they are presented in the [official documentation](https://bungie-net.github.io/multi/index.html). All of the API calls can be found in [api.py](./pydest/api.py), and I've added fairly descriptive docstring to each function to explain how to use them. But for the most part, you should be able to get by without diving into the code.

## Running Tests

There is a series of integration tests that can be run to verify that Pydest is working as intended. These tests will hit all supported Destiny 2 endpoints with well formed requests, and verify that a valid response is received. The main reason reason that these tests would fail, is if the Bungie.net servers are down, or the endpoints themselves have changed.

Before these tests can be run, you'll need to create a `credentials.json` file in the root directory of this project. This file will contain your api key.
```
{
  "api-key": "your-api-key"
}
```
