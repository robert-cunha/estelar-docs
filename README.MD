# Estelar API

## Get Started

First of all, you should ask for the administrator of the system to release the API Key for you. This API Key will allow you to call all the requests and including the one to generate the bearer token. The following document will explain wich key we should use in each route.

## Create an user

To create your user, you should call the following route. Replace the x-api-key header with the key the administrator gived to you.

```
curl --location --request POST '{{ESTELAR_API}}/user' \
--header 'x-api-key: c21dd17d7f786c5d2869aae9a32ca1a3' \
--header 'Content-Type: application/json' \
--data-raw '{
    "firstName": "Robert",
    "lastName": "Cunha",
    "email": "robert.cunha@estrelabet.com",
    "password": "kekekekekekeke"
}'
```

This will allow you to auth and get a token to make other requests.

## Authentication

After the user was created, you can authenticate on the system. Here is no need for the x-api-key.

```
curl --location -g --request POST '{{ESTELAR_API}}/auth' \
	--header 'Content-Type: application/json' \
	--data-raw '{
			"email": "robert.cunha@estrelabet.com",
			"password": "testetesteteste"
	}'
```

When you make this request, you should receive a response like this.

```
{
    "data": {
        "bearerToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InJvYmVydC5jdW5oYUBlc3RyZWxhYmV0LmNvbSIsImRhdGUiOjE2NzI3NjEzMzg5ODEsImlhdCI6MTY3Mjc2MTMzOCwiZXhwIjoxNjcyODQ3NzM4fQ.7cBFFxZuS0cum7f8hzBSMgEZ81-pMRrHJzSoTaH3WFk",
        "expiresIn": 86400
    },
    "message": null,
    "statusCode": 200
}
```

The most important thing here, is the bearerToken. It will be used to list the games.

## Create a round

This route should be used only for the webhook. It uses the x-api-key instead of bearer token.

```
curl --location --request POST '{{ESTELAR_API}}/round' \
--header 'x-api-key: c21dd17d7f786c5d2869aae9a32ca1a3' \
--header 'Content-Type: application/json' \
--data-raw '{
    "roundId": 3,
    "roundStart": "2023-01-05T13:46:27.787Z",
    "roundEnd": "2023-01-05T13:46:27.787Z",
    "multiplier": 3,
    "winners": 3,
    "losers": 9,
    "totalBets": 10,
    "totalWins": 3,
    "playerInfo": [
        {
            "playerId": 32,
            "crash": true,
	    "latency": 232,
	    "oddHalf": 2.1,
	    "oddFinal": 4.17,
	    "betValue": 10,
	    "totalBetWin": 15,
	    "halfBetWin": 1.3,
	    "fullBetWin": 1.4
        }
    ]
}'
```

## List rounds

You can list rounds and make some filters. Use the bearer toekn you've received before. On the Authorization header, put the word Bearer and paste the token.

```
curl --location --request GET '{{ESTELAR_API}}/round' \
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InJvYmVydC5jdW5oYUBlc3RyZWxhYmV0LmNvbSIsImRhdGUiOjE2NzI3NjEzMzg5ODEsImlhdCI6MTY3Mjc2MTMzOCwiZXhwIjoxNjcyODQ3NzM4fQ.7cBFFxZuS0cum7f8hzBSMgEZ81-pMRrHJzSoTaH3WFk'
```

### Query Parameters

> page: The page you want to get. The first page is 0.

> limit: How many items you want to get. The skip parameter will be calculated from this.

> query: A JSON object that can be used to filter items according to his attributes.

> sort: A JSON object that can be used to sort items according to his attributes. 1 is ascending and -1 is descending

### Examples with Query Parameters

```
curl --location --request GET '{{ESTELAR_API}}/round?page=0&limit=10&query={"winners":0}&sort={"roundId":-1}' \
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InJvYmVydC5jdW5oYUBlc3RyZWxhYmV0LmNvbSIsImRhdGUiOjE2NzI3NjEzMzg5ODEsImlhdCI6MTY3Mjc2MTMzOCwiZXhwIjoxNjcyODQ3NzM4fQ.7cBFFxZuS0cum7f8hzBSMgEZ81-pMRrHJzSoTaH3WFk'
```
