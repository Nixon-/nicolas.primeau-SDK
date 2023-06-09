# The One API SDK

- [The One API SDK](#the-one-api-sdk)
  * [Install](#install)
    + [Dev instructions](#dev-instructions)
  * [Usage](#usage)
    + [Authentication](#authentication)
    + [SDK Resources](#sdk-resources)
      - [Movie](#movie)
      - [Quote](#quote)
    + [CLI client](#cli-client)
  * [Test](#test)

## Install

Installation can be done via pip:

```
python -m pip install nicolas-primeau-sdk
```

### Dev instructions

To setup locally, first clone the repo, then install the requirements with this command:

```
python -m pip install -r requirements.txt
```

## Usage

The SDK can be used to retrieve movie and quote information from the https://the-one-api.dev/

### Authentication

The API requires authentication for most endpoints. A free auth token can be generated 
by signing up at this page: https://the-one-api.dev/sign-up

Once you have a token, it's strongly recommended to use an environment variable:
```
export THE_ONE_API_TOKEN={your token here}
```

It's also possible to specify the auth token directly in the SDK config:
```
from the_one_api_sdk.sdk import TheOneApiSdk
from the_one_api_sdk.config import SdkConfig

config = SdkConfig(auth_token="your token")
client = TheOneApiSdk(config=config)
```

### SDK Resources

The following resources are available

#### Movie

A movie contains these attributes:

    movie_id: str
    name: str
    runtimeInMinutes: Optional[int] = None
    budgetInMillions: Optional[int] = None
    boxOfficeRevenueInMillions: Optional[int] = None
    academyAwardNominations: Optional[int] = None
    academyAwardWins: Optional[int] = None
    rottenTomatoesScore: Optional[int] = None


Movies can be listed like so:
```
from the_one_api_sdk.sdk import TheOneApiSdk

client = TheOneApiSdk()
for movie in client.movies.list():
    print(movie)
```

Or a single movie resource can be fetched directly:

```
movie = client.movies("5cd95395de30eff6ebccde5d").fetch()
```

#### Quote

A quote contains these attributes

    quote_id: str
    dialog: str
    character_id: Optional[str] = None
    movie_id:  Optional[str] = None

Quotes can be listed like so:
```
for quote in client.quotes.list():
    print(quote)
```

Or a single quote resource can be fetched directly:

```
quote = client.quote("5cd96e05de30eff6ebcce7e9").fetch()
```


Quotes can also be retrieved for a movie like this:

```
movies_quotes = client.quotes.list(movie_id="5cd95395de30eff6ebccde5d").fetch()
```
Or, like this:
```
movies_quotes = client.movies("5cd95395de30eff6ebccde5d").quotes.fetch()
```

### CLI client

The simplest way to use this SDK is through the CLI client.

Below are example on how to use the CLI client.

Listing movies
```
the-one-api-sdk movies list
```
Listing movies, up to a limit
```
the-one-api-sdk movies list --limit 2
```

Get a specific movie
```
the-one-api-sdk movies get --movie-id <movie id>
```

List 50 quotes
```
the-one-api-sdk quotes list --limit 50
```

List quotes from a movie
```
the-one-api-sdk quotes list --movie-id <movie id>
```

Get a single quote
```
the-one-api-sdk quotes get --quote-id <quote id>
```

## Test

Tests are using the standard unittest library:

```
python -m unittest
```




