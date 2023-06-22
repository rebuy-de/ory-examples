# ORY Kratos as Login Provider for ORY Hydra

**Warning: ** this is a preliminary example and will properly be implemented in
ORY Kratos directly.

For now, to run this example execute:

```shell script
$ docker-compose up --build
```

Next, create an OAuth2 Client

```shell script
code_client=$(docker-compose exec hydra \
    hydra create client \
    --endpoint http://127.0.0.1:4445 \
    --grant-type authorization_code,refresh_token \
    --response-type code,id_token \
    --format json \
    --scope openid --scope offline --scope email --scope customer_uuid \
    --redirect-uri http://127.0.0.1:8080/auth/callback)

echo export CODE_CLIENT_ID=$(echo $code_client | jq -r '.client_id')
echo export CODE_CLIENT_SECRET=$(echo $code_client | jq -r '.client_secret')
```

and perform an OAuth2 Authorize Code Flow

```shell script
$ docker-compose exec hydra \
    hydra perform authorization-code \
    --client-id $code_client_id \
    --client-secret $code_client_secret \
    --endpoint http://127.0.0.1:4444/ \
    --port 5555 \
    --scope openid --scope offline
```
