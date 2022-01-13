# Quick Start

{% hint style="info" %}
**Good to know:** You can quickly and easily test the APIs using Postman
{% endhint %}

Your API requests are authenticated using API keys. Any request that doesn't include an API key will return an error.

You can generate an API key from your Dashboard at any time.

## Authorization

Flex Hub uses OAuth2 Client Credentials to allow access to the API. As part of onboarding the FlexID platform, you will be provided with the credentials required to make calls to our authorization provider and receive an access token. The access token is used as an `Authorization` header on all endpoints with the authorization scheme `Bearer.`

### Get Access Token

`POST /oauth/token`

| Parameter      | Description                                                          |
| -------------- | -------------------------------------------------------------------- |
| client\_id     | Client ID for your organization (Provided during onboarding)         |
| client\_secret | Client Secret for your organization (Provided during onboarding)     |
| audience       | Base URL for your Flex Hub Organization (Provided during onboarding) |
| grant\_type    | Set to `client_credentials`                                          |

> Request Body

{% tabs %}
{% tab title="Shell" %}
```shell
curl --request POST \
    --url https://flexfintx.us.auth0.com/oauth/token \
    --data '{
        "client_id": "9uKHFvrIwkJGb0U73GRyalb6emEWAf92",
        "client_secret": "X1uYb9hR7KL-NH5RkkJFFhYnH-9hcgR4o-W21FDFfHjzVu8HyY3b3FRmLE0-aj3D",
        "audience": "https://organization.hub.flexfintx.com/v1/",
        "grant_type": "client_credentials"
    }'

```
{% endtab %}

{% tab title="Javascript" %}
```javascript
var myHeaders = new Headers();
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
  client_id: "9uKHFvrIwkJGb0U73GRyalb6emEWAf92",
  client_secret:
    "X1uYb9hR7KL-NH5RkkJFFhYnH-9hcgR4o-W21FDFfHjzVu8HyY3b3FRmLE0-aj3D",
  audience: "https://organization.hub.flexfintx.com/v1/",
  grant_type: "client_credentials",
});

var requestOptions = {
  method: "POST",
  headers: myHeaders,
  body: raw,
  redirect: "follow",
};

fetch("https://flexfintx.us.auth0.com/oauth/token", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error))
```



> Sample JS Response (200 OK)

```json
{
  "access_token": "eyJhbHziOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6InVEa1lXa0UwOGN4NkZENE9lS01LWiJ9.eyJpc3MiOiJodHRwczovL2ZsZXhmaW50eC51cy5hdXRoMC5jb20vIiwic3ViIjoiOXVLSEZ2ckl3a0pHYjBVNzNHUnlhbGI2ZW1FV0FMMDVAY2xpZW50cyIsImF1ZCI6Imh0dHFwOi8vc2FuZGJveC5odWIuZmxleGZpbnR4LmNvbS92MS8iLCJpYXQiOjE2MDc1MTM2NTgsImV4cCI6MTYwNzYwMDA1OCwiYXpwIjoiOXVLSEZ2ckl3a0pHYjBVNzNHUnlhbGI2ZW1FV0FMMDUiLCJndHkiOiJjbGllbnQtY3JlZGVudGlhbHMifQ.tLlgYdAROgYgFJw6tzdOJrhybQcUcrpswsUsczLFssBx1EHGUTbvBTzrHjWN2QIJ_XyEcAVHPzPyEjOJrVfkZj2l1nsGjZwmU6Gx6tq38FwH70aEVbwZw_xFMuLR2VofJHtk0bIeSkAHe532-yw3tUyPmJlAWIIIPM5DCWARFPNslMw4dqg7WIMHvhXcRattdIfCeN5XOKFBey0brZtI4kMN0-KWsblWx3lOSL90QXoMGav1u2dIgKYafuKhHBSkSae_5OSjhIpDPQ2QuKtmIzBabVRmxWpqotxV-GYoykMWQFZzdTbqGb0eFgO8Y18COvdQ_2v5I_aEFns7GQTfsf",
  "expires_in": 86400,
  "token_type": "Bearer"
}
```



> Sample Response (401 Unauthorized)

```json
{
  "error": "access_denied",
  "error_description": "Unauthorized"
}
```

####
{% endtab %}
{% endtabs %}

## DIDs



All DID management API's require an `Authorization` header with scheme `Bearer` with an access token obtained from the `Get Access Token` endpoint.

### Create a DID

Generates keys and creates a new DID for the organization based on the provided DID Method type and Cryptographic Key type. Currently only `key` method is supported with key type `Ed25519`.

#### Request Body

`POST /dids`

| Parameter | Description                        |
| --------- | ---------------------------------- |
| method    | DID Method to create the DID for   |
| keyType   | Cryptographic key type for the DID |

Returns the DID Document that is created.

Unsupported methods and key types result in `400 Bad Request` errors with meaningful error descriptions.

> Request Body

{% tabs %}
{% tab title="shell" %}

{% endtab %}

{% tab title="Javascript" %}
```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer <ACCESS_TOKEN>");
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
  method: "key",
  keyType: "Ed25519",
});

var requestOptions = {
  method: "POST",
  headers: myHeaders,
  body: raw,
  redirect: "follow",
};

fetch("https://organization.hub.flexfintx.com/v1/dids", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```



> Sample Response (200 OK)

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    {
      "@base": "did:key:z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE"
    }
  ],
  "id": "did:key:z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE",
  "verificationMethod": [
    {
      "id": "#z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE",
      "type": "Ed25519VerificationKey2018",
      "controller": "did:key:z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE",
      "publicKeyBase58": "CymfNguWwC4bsUTS3E38xBxs1Tg8K8iWCKPtxuSAYBSr"
    },
    {
      "id": "#z6LSdPfuDZn2Dt1Hw5BDLJ3Au1W929rZrBU6zH3mP5nYgCZG",
      "type": "X25519KeyAgreementKey2019",
      "controller": "did:key:z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE",
      "publicKeyBase58": "2iVjhFyA8RHYqgoSoeXDaRHfB1KT9aHx7JL5td91xpnW"
    }
  ],
  "authentication": ["#z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE"],
  "assertionMethod": ["#z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE"],
  "capabilityInvocation": ["#z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE"],
  "capabilityDelegation": ["#z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE"],
  "keyAgreement": ["#z6LSdPfuDZn2Dt1Hw5BDLJ3Au1W929rZrBU6zH3mP5nYgCZG"]
}
```
{% endtab %}
{% endtabs %}

### Get all DIDs

Returns the DID Documents of all DIDs owned by the organization.

#### Request Body

`GET /dids`

{% tabs %}
{% tab title="shell" %}
```shell
curl --request GET \
    --url https://organization.hub.flexfintx.com/v1/dids \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer <ACCESS_TOKEN>");

var requestOptions = {
  method: "GET",
  headers: myHeaders,
  redirect: "follow",
};

fetch("https://organization.hub.flexfintx.com/v1/dids", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```



> Sample Response (200 OK)

```json
[
  {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      {
        "@base": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"
      }
    ],
    "authentication": ["#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"],
    "assertionMethod": ["#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"],
    "capabilityDelegation": [
      "#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"
    ],
    "capabilityInvocation": [
      "#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"
    ],
    "keyAgreement": ["#z6LSejq2UEYZFuEWuCohJCk1tEJtch4XuKCK4zToNMsy6qBZ"],
    "id": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
    "verificationMethod": [
      {
        "id": "#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
        "type": "Ed25519VerificationKey2018",
        "controller": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
        "publicKeyBase58": "5u3wGZwUxbwgGEFFbpnZQjfyBzNcAVTcgWvDhvzgquPP"
      },
      {
        "id": "#z6LSejq2UEYZFuEWuCohJCk1tEJtch4XuKCK4zToNMsy6qBZ",
        "type": "X25519KeyAgreementKey2019",
        "controller": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
        "publicKeyBase58": "44erwvjhASWmopRvmZE4Ze6QmYXRCi2AC1k7suESPTQo"
      }
    ]
  }
]
```


{% endtab %}
{% endtabs %}

### Delete a DID

Deletes a DID managed by the organization.

#### URL Parameters

`DELETE /dids/:id`

| Parameter | Description                  |
| --------- | ---------------------------- |
| id        | The DID identifier to delete |

{% tabs %}
{% tab title="shell" %}
```shell
curl --request DELETE \
    --url https://organization.hub.flexfintx.com/v1/dids/<DID>\
    --header 'Authorization: Bearer <ACCESS_TOKEN>'
```


{% endtab %}

{% tab title="javascript" %}
```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer <ACCESS_TOKEN>");

var requestOptions = {
  method: "DELETE",
  headers: myHeaders,
  redirect: "follow",
};

fetch("https://organization.hub.flexfintx.com/v1/dids/<DID>", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```
{% endtab %}
{% endtabs %}

### Resolve a DID

Resolves the given DID based on it's DID Method to return the DID Document. The DID Document contains additional information related to the DID.

#### URL Parameters

`GET /dids/:id`

| Parameter | Description                   |
| --------- | ----------------------------- |
| id        | The DID identifier to resolve |

{% tabs %}
{% tab title="shell" %}
```shell
curl --request GET \
    --url https://organization.hub.flexfintx.com/v1/dids/did:key:z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE \
    --header 'Authorization: Bearer <ACCESS_TOKEN>s
```
{% endtab %}

{% tab title="javascript" %}
```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer <ACCESS_TOKEN>");

var requestOptions = {
  method: "GET",
  headers: myHeaders,
  redirect: "follow",
};

fetch(
  "https://organization.hub.flexfintx.com/v1/dids/did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
  requestOptions
)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```

> Sample Response (200 OK)

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    {
      "@base": "did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C"
    }
  ],
  "id": "did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
  "verificationMethod": [
    {
      "id": "#z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
      "type": "Ed25519VerificationKey2018",
      "controller": "did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
      "publicKeyBase58": "EvtEQdUgGL8nc5izdL5X6aremUwNRGhmuqS6k5CmVbEp"
    },
    {
      "id": "#z6LStpYjLUfF46hW9UMbgDQXJgBbWvBDoWHX4rvb1uca8JXX",
      "type": "X25519KeyAgreementKey2019",
      "controller": "did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
      "publicKeyBase58": "J9NZpArNxdym45yq9ZtZz5y7fme76u7NBtCuXSy3Qvkm"
    }
  ],
  "authentication": ["#z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C"],
  "assertionMethod": ["#z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C"],
  "capabilityInvocation": ["#z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C"],
  "capabilityDelegation": ["#z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C"],
  "keyAgreement": ["#z6LStpYjLUfF46hW9UMbgDQXJgBbWvBDoWHX4rvb1uca8JXX"]
}
```


{% endtab %}
{% endtabs %}

## Credentials

All Credential management API's require an `Authorization` header with scheme `Bearer` with an access token obtained from the `Get Access Token` endpoint.

### Create a Credential

Creates a new Verifiable Credential signed by the organization's DID that is specified in the `issuer` field. Any fields under `credentialSubject` are allowed as long as they exist as a `https://schema.org/` specification.

#### Request Body

`POST /credentials`

| Parameter          | Description                                                                                                           |
| ------------------ | --------------------------------------------------------------------------------------------------------------------- |
| @context           | The JSON-LD context for the credential                                                                                |
| type               | Type of the Verifiable Credential                                                                                     |
| name               | Name of the Credential to display in the Wallet                                                                       |
| image?             | Optional link to a background image for the credential card. If not provided, a default one is applied                |
| credentialSubject  | Specific details contained within the credential. `id` field must be the DID of the receiver/holder of the credential |
| issuer             | A DID owned by the organization used to sign the credential                                                           |
| issuerOrganization | Name of the issuer organization to display in the wallet                                                              |
| saveToBank         | `true` if the created credential should be stored in the bank, `false` if not                                         |
| isRevocable        | `true` if the credential can be revoked later, `false` if not                                                         |

Returns the signed Credential

Unsupported or invalid request bodies result in `400 Bad Request` errors with meaningful error descriptions.

{% tabs %}
{% tab title="shell" %}
```shell
curl --request POST \
    --url https://organization.hub.flexfintx.com/v1/credentials \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
    --data '{
      "@context": ["https://www.w3.org/2018/credentials/v1", "https://schema.org"],
      "type": ["VerifiableCredential", "AlumniCredential"],
      "name": "Alumni of EXU",
      "issuerOrganization": "Example University",
      "credentialSubject": {
        "id": "did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
        "givenName": "John",
        "familyName": "Doe",
        "alumniOf": "Example University",
      },
      "issuer": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
      "saveToBank": true,
      "isRevocable": true,
    }'s
```
{% endtab %}

{% tab title="javascript" %}
```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer <ACCESS_TOKEN>");
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
  "@context": ["https://www.w3.org/2018/credentials/v1", "https://schema.org"],
  type: ["VerifiableCredential", "AlumniCredential"],
  name: "Alumni of EXU",
  issuerOrganization: "Example University",
  credentialSubject: {
    id: "did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
    givenName: "John",
    familyName: "Doe",
    alumniOf: "Example University",
  },
  issuer: "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
  saveToBank: true,
  isRevocable: true,
});

var requestOptions = {
  method: "POST",
  headers: myHeaders,
  body: raw,
  redirect: "follow",
};

fetch("https://organization.hub.flexfintx.com/v1/credentials", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```
{% endtab %}
{% endtabs %}

> Sample Response (200 OK)

```json
{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://schema.org",
    "https://w3id.org/vc-revocation-list-2020/v1"
  ],
  "id": "dc557bfe-4257-4a5a-b1f8-f3cd8dadd10a",
  "type": ["VerifiableCredential", "AlumniCredential"],
  "name": "Alumni of EXU",
  "image": "https://www.pngrepo.com/png/226672/512/rectangular-identity.png",
  "issuerOrganization": "Example University",
  "issuer": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
  "issuanceDate": "2021-01-25T00:59:15.857Z",
  "credentialSubject": {
    "id": "did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
    "givenName": "John",
    "familyName": "Doe",
    "alumniOf": "Example University"
  },
  "credentialStatus": {
    "id": "https://sandbox.hub.flexfintx.com/v1/revocations/list/74268682-176c-427f-ace2-b0b88d415bd8#1",
    "type": "RevocationList2020Status",
    "revocationListIndex": 1,
    "revocationListCredential": "https://sandbox.hub.flexfintx.com/v1/revocations/list/74268682-176c-427f-ace2-b0b88d415bd8"
  },
  "proof": {
    "type": "Ed25519Signature2018",
    "created": "2021-01-25T00:59:15.926Z",
    "jws": "eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..63hx3XYtdJc2jG3lkjayZrwPSV-8N5_KWvoSP5H5d7rwy0hgkE1ptkIlcyy9fAo0G_mUFCb7I1ylNREKP92cDg",
    "proofPurpose": "assertionMethod",
    "verificationMethod": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"
  }
}
```

### Get all Credentials

Returns the credentials issued by the organization. Credentials that were not saved to the bank will be returned without the `credentialSubject` field.

#### Request Body

`GET /credentials`

{% tabs %}
{% tab title="shell" %}
```shell
curl --request GET \
    --url https://organization.hub.flexfintx.com/v1/credentials \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
```
{% endtab %}

{% tab title="javascript" %}
```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer <ACCESS_TOKEN>");

var requestOptions = {
  method: "GET",
  headers: myHeaders,
  redirect: "follow",
};

fetch("https://organization.hub.flexfintx.com/v1/credentials", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```



> Sample Response (200 OK)

```json
[
  {
    "@context": [
      "https://www.w3.org/2018/credentials/v1",
      "https://schema.org",
      "https://w3id.org/vc-revocation-list-2020/v1"
    ],
    "type": ["VerifiableCredential", "AlumniCredential"],
    "id": "46831e51-0e7d-47b9-b1e2-36ebf93bd95d",
    "issuer": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
    "issuanceDate": "2020-11-30T18:22:49.378Z",
    "credentialSubject": {
      "id": "did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
      "givenName": "John",
      "familyName": "Doe",
      "alumniOf": "Example University"
    },
    "credentialStatus": {
      "id": "https://sandbox.hub.flexfintx.com/v1/revocations/list/74268682-176c-427f-ace2-b0b88d415bd8#0",
      "type": "RevocationList2020Status",
      "revocationListIndex": 0,
      "revocationListCredential": "https://sandbox.hub.flexfintx.com/v1/revocations/list/74268682-176c-427f-ace2-b0b88d415bd8"
    },
    "proof": {
      "type": "Ed25519Signature2018",
      "created": "2020-11-30T18:22:49.402Z",
      "jws": "eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..tPH_SGm65wGlD8KXKos_fGozAqXB3xSoTVAvhyJaudi8pvTehWAnHcVNxFr42DWf39Y1P7FenEW8rMu_EFASDQ",
      "proofPurpose": "assertionMethod",
      "verificationMethod": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"
    }
  },
  {
    "@context": [
      "https://www.w3.org/2018/credentials/v1",
      "https://schema.org"
    ],
    "type": ["VerifiableCredential", "AlumniCredential"],
    "id": "1758b393-f701-4844-b223-09fa95d2dc6b",
    "issuer": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
    "issuanceDate": "2021-01-25T01:07:11.455Z",
    "proof": {
      "type": "Ed25519Signature2018",
      "created": "2021-01-25T01:07:11.467Z",
      "jws": "eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..GZKL35qIn0njHGYnbnyXpMkJ8ey3NusjZnobA9A0XlB5TOQggyikuJonvGmi4WYoKD2lVNJ66VMbe6K58rU5CQ",
      "proofPurpose": "assertionMethod",
      "verificationMethod": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"
    }
  }
]
```
{% endtab %}
{% endtabs %}

### Get all Persisted Credentials

Returns the credentials issued by the organization that were saved to the bank. All returned credentials from this endpoint will include the `credentialSubject` field.

#### Request Body

`GET /credentials/persisted`

{% tabs %}
{% tab title="shell" %}
```shell
curl --request GET \
    --url https://organization.hub.flexfintx.com/v1/credentials/persisted \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
javar myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer <ACCESS_TOKEN>");

var requestOptions = {
  method: "GET",
  headers: myHeaders,
  redirect: "follow",
};

fetch(
  "https://organization.hub.flexfintx.com/v1/credentials/persisted",
  requestOptions
)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```

> Sample Response (200 OK)

```json
[
  {
    "@context": [
      "https://www.w3.org/2018/credentials/v1",
      "https://w3id.org/vc-revocation-list-2020/v1"
    ],
    "type": ["VerifiableCredential", "RevocationList2020Credential"],
    "id": "https://sandbox.hub.flexfintx.com/v1/revocations/list/74268682-176c-427f-ace2-b0b88d415bd8",
    "issuer": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
    "issuanceDate": "2020-11-30T18:21:38.105Z",
    "credentialSubject": {
      "id": "https://sandbox.hub.flexfintx.com/v1/revocations/list/74268682-176c-427f-ace2-b0b88d415bd8#list",
      "type": "RevocationList2020",
      "encodedList": "H4sIAAAAAAAAA-3BMQEAAADCoPVPbQsvoAAAAAAAAAAAAAAAAP4GcwM92tQwAAA"
    },
    "proof": {
      "type": "Ed25519Signature2018",
      "created": "2020-11-30T18:21:38.106Z",
      "jws": "eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..4cjdI8CvBb2kEl9cARWs4__kFtkkMOwcmxYbMf1vacj1mEM0khC7NUTfQgH4uOXd07SulS3xRIsYbQjGpcIJDg",
      "proofPurpose": "assertionMethod",
      "verificationMethod": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"
    }
  },
  {
    "@context": [
      "https://www.w3.org/2018/credentials/v1",
      "https://schema.org",
      "https://w3id.org/vc-revocation-list-2020/v1"
    ],
    "type": ["VerifiableCredential", "AlumniCredential"],
    "id": "dc557bfe-4257-4a5a-b1f8-f3cd8dadd10a",
    "issuer": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
    "issuanceDate": "2021-01-25T00:59:15.857Z",
    "credentialSubject": {
      "id": "did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
      "givenName": "John",
      "familyName": "Doe",
      "alumniOf": "Example University"
    },
    "credentialStatus": {
      "id": "https://sandbox.hub.flexfintx.com/v1/revocations/list/74268682-176c-427f-ace2-b0b88d415bd8#1",
      "type": "RevocationList2020Status",
      "revocationListIndex": 1,
      "revocationListCredential": "https://sandbox.hub.flexfintx.com/v1/revocations/list/74268682-176c-427f-ace2-b0b88d415bd8"
    },
    "proof": {
      "type": "Ed25519Signature2018",
      "created": "2021-01-25T00:59:15.926Z",
      "jws": "eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..63hx3XYtdJc2jG3lkjayZrwPSV-8N5_KWvoSP5H5d7rwy0hgkE1ptkIlcyy9fAo0G_mUFCb7I1ylNREKP92cDg",
      "proofPurpose": "assertionMethod",
      "verificationMethod": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"
    }
  }
]
```


{% endtab %}
{% endtabs %}

### Get Credential by ID

Returns the credential issued by the organization which matches the provided ID. The returned credential may or may not contain the `credentialSubject` field based on whether it was saved to the bank or not.

#### URL Parameters

`GET /credentials/:id`

| Parameter | Description                                |
| --------- | ------------------------------------------ |
| id        | The identifier of the Credential to return |

{% tabs %}
{% tab title="shell" %}
```shell
curl --request DELETE \
    --url https://organization.hub.flexfintx.com/v1/credentials/<ID> \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \sh
```
{% endtab %}

{% tab title="javascript" %}
```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer <ACCESS_TOKEN>");

var requestOptions = {
  method: "DELETE",
  headers: myHeaders,
  redirect: "follow",
};

fetch(
  "https://organization.hub.flexfintx.com/v1/credentials/<ID>",
  requestOptions
)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```

> Sample Response (200 OK)

```
OK
```


{% endtab %}
{% endtabs %}

*

## Revocations

### Get Revocation Status of Credential

This API endpoint requires an `Authorization` header with scheme `Bearer` with an access token obtained from the `Get Access Token` endpoint.

Returns the current revocation status of the credential which matches the provided ID. Returns `true` if it has been revoked, returns `false` otherwise.

#### URL Parameters

`GET /revocations/:id`

| Parameter | Description                                                       |
| --------- | ----------------------------------------------------------------- |
| id        | The identifier of the Credential to get the Revocation Status for |

> Request Body

{% tabs %}
{% tab title="shell" %}
```shell
curl --request GET \
    --url https://organization.hub.flexfintx.com/v1/revocations/<ID> \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
```
{% endtab %}

{% tab title="javascript" %}
```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer <ACCESS_TOKEN>");

var requestOptions = {
  method: "GET",
  headers: myHeaders,
  redirect: "follow",
};

fetch(
  "https://organization.hub.flexfintx.com/v1/revocations/<ID>",
  requestOptions
)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```

> Sample Response (200 OK)

```
{
  "isRevoked": true
}
```


{% endtab %}
{% endtabs %}

### Set Revocation Status of Credential

This API endpoint requires an `Authorization` header with scheme `Bearer` with an access token obtained from the `Get Access Token` endpoint.

Used to set the revocation status of the credential which matches the provided ID.

#### URL Parameters

`POST /revocations/:id`

| Parameter | Description                                                          |
| --------- | -------------------------------------------------------------------- |
| id        | The identifier of the Credential to update the Revocation Status for |

#### Request Body

| Parameter  | Description                                                                                         |
| ---------- | --------------------------------------------------------------------------------------------------- |
| setRevoked | Set the revocation status of the credential. `true` to revoke it, `false` to take back a revocation |

> Request Body

{% tabs %}
{% tab title="shell" %}
```shell
// Some curl --request GET \
    --url https://organization.hub.flexfintx.com/v1/revocations/<ID> \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
```


{% endtab %}

{% tab title="javascript" %}
```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer <ACCESS_TOKEN>");

var raw = JSON.stringify({
  setRevoked: true,
});

var requestOptions = {
  method: "GET",
  headers: myHeaders,
  redirect: "follow",
};

fetch(
  "https://organization.hub.flexfintx.com/v1/revocations/<ID>",
  requestOptions
)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```


{% endtab %}
{% endtabs %}

### Get Revocation List

Gets the RevocationList Credential with the provided ID. This API is public and is mostly used internally to get or set the revocation status of a credential.

#### URL Parameters

`GET /revocations/list/:id`

| Parameter | Description                                               |
| --------- | --------------------------------------------------------- |
| id        | The identifier of the RevocationList Credential to return |

> Request Body

{% tabs %}
{% tab title="shell" %}
```shell
curl --request GET \
    --url https://organization.hub.flexfintx.com/v1/revocations/list/<ID> \shs
```
{% endtab %}

{% tab title="javascript" %}
```shell
var requestOptions = {
  method: "GET",
  redirect: "follow",
};

fetch(
  "https://organization.hub.flexfintx.com/v1/revocations/list/<ID>",
  requestOptions
)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```

> Sample Response (200 OK)

```json
{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://w3id.org/vc-revocation-list-2020/v1"
  ],
  "type": ["VerifiableCredential", "RevocationList2020Credential"],
  "id": "https://sandbox.hub.flexfintx.com/v1/revocations/list/74268682-176c-427f-ace2-b0b88d415bd8",
  "issuer": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
  "issuanceDate": "2020-11-30T18:21:38.105Z",
  "credentialSubject": {
    "id": "https://sandbox.hub.flexfintx.com/v1/revocations/list/74268682-176c-427f-ace2-b0b88d415bd8#list",
    "type": "RevocationList2020",
    "encodedList": "H4sIAAAAAAAAA-3BMQEAAADCoPVPbQsvoAAAAAAAAAAAAAAAAP4GcwM92tQwAAA"
  },
  "proof": {
    "type": "Ed25519Signature2018",
    "created": "2020-11-30T18:21:38.106Z",
    "jws": "eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..4cjdI8CvBb2kEl9cARWs4__kFtkkMOwcmxYbMf1vacj1mEM0khC7NUTfQgH4uOXd07SulS3xRIsYbQjGpcIJDg",
    "proofPurpose": "assertionMethod",
    "verificationMethod": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"
  }
}
```


{% endtab %}
{% endtabs %}

## Presentations

All Presentation management API's, except Presentation Submission, require an `Authorization` header with scheme `Bearer` with an access token obtained from the `Get Access Token` endpoint.

### Create Presentation Request Template

Creates a new template for the organization to request presentations from holders of credentials. Used to provide information to the identity wallet about why the presentation is being requested, and what types of credentials are acceptable.

#### Request Body

`POST /presentations/templates`

| Parameter         | Description                                                                                                                                                                                                                              |
| ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| name              | Descriptive name for the presentation request template                                                                                                                                                                                   |
| reason            | Explanatory reason for the presentation request                                                                                                                                                                                          |
| credentialType    | `type` of credential that is accepted                                                                                                                                                                                                    |
| credentialIssuers | Array of DID's representing issuers whose credentials are considered valid submissions for this presentation request. `self` is also allowed as a special case to allow for self-issued credentials - this is used for DIDAuth purposes. |

Returns the Presentation Request Template that is created.

Unsupported requests result in `400 Bad Request` errors with meaningful error descriptions.

> Request Body

{% tabs %}
{% tab title="shell" %}
```shell
curl --request POST \
    --url https://organization.hub.flexfintx.com/v1/dids \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
    --data '{
        "name": "Alumni Credential Verification 3",
        "reason": "We need this to verify you hold a Bachelor'\''s Degree",
        "credentialType": "AlumniCredential",
        "credentialIssuers": [
            "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Af"
        ]
    }'
```
{% endtab %}

{% tab title="javascript" %}
```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer <ACCESS_TOKEN>");
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
  name: "Alumni Credential Verification 3",
  reason: "We need this to verify you hold a Bachelor's Degree",
  credentialType: "AlumniCredential",
  credentialIssuers: [
    "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Af",
  ],
});

var requestOptions = {
  method: "POST",
  headers: myHeaders,
  body: raw,
  redirect: "follow",
};

fetch(
  "https://organization.hub.flexfintx.com/v1/presentations/templates",
  requestOptions
)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```

> Sample Response (200 OK)

```json
{
  "id": "34fad1c3-42be-494b-9c60-23e174bc6c95",
  "name": "Alumni Credential Verification",
  "reason": "We need this to verify you hold a Bachelor's Degree",
  "credentialType": "AlumniCredential",
  "credentialIssuers": [
    "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Af"
  ]
}
```


{% endtab %}
{% endtabs %}

### List Presentation Request Templates

Returns all Presentation Request Templates that were created by the organization.

`GET /presentations/templates`

### Get Presentation Request Template by ID

Returns the Presentation Request template created by the organization whose ID matches the one provided.

#### URL Parameters

`GET /presentations/templates/:id`

| Parameter | Description                                                   |
| --------- | ------------------------------------------------------------- |
| id        | The identifier of the Presentation Request Template to return |

### Delete Presentation Request Template by ID

#### URL Parameters

`DELETE /presentations/templates/:id`

| Parameter | Description                                            |
| --------- | ------------------------------------------------------ |
| id        | The Presentation Request Template identifier to delete |

### Create Presentation Request

Generates an instance of a Presentation Request from previously created Presentation Request Template(s). Returns the created Presentation Request.

#### URL Parameters

`POST /presentations/templates/request`

| Parameter | Description                                                                                    |
| --------- | ---------------------------------------------------------------------------------------------- |
| templates | The Presentation Request Template identifiers to create the Presentation Request instance from |

### Submit Presentation

**This endpoint does not require an `Authorization` header and is public**

Accepts a Presentation Submission from a Credential Holder. This endpoint will mostly be used by the identity wallet of the credential holder.

#### Request Body

| Parameter    | Description                                                          |
| ------------ | -------------------------------------------------------------------- |
| requestId    | The Presentation Request identifier for which this is the submission |
| presentation | The signed Presentation which is the submission to this request      |

### Fetch Presentation Submission

Returns the submitted Presentation for a given `requestId`. This endpoint should be polled by the Verifier to know when a Holder has shared a Presentation for their request.

#### URL Parameters

| Parameter | Description                                                     |
| --------- | --------------------------------------------------------------- |
| requestId | The Presentation Request identifier to check the submission for |

## Metadata

The Metadata API requires an `Authorization` header with scheme `Bearer` with an access token obtained from the `Get Access Token` endpoint.

### Get Hub Metadata

Gets metadata information about the hub including the identifier for the Revocation List that is currently being updated (i.e. still has empty spots in it's `revocationList`) along with the current `revocationListIndex` which is the first index in the `revocationList` that is available to be assigned to a credential.
