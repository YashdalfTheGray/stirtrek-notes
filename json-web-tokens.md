# JSON Web Tokens to the Rescue

Ayan Dave (adave@fusionalliance.com)

## Cookies

* Reliance on the User Agent - meh
* Encourages separation of designation and authorization
* Reliance on DNS and other infrastructure
* Privacy concerns

## Building Stateless systems

HTTP Cookies aren't available to native devices or IoT type devices.

An idea can be using an Access Token. It solves the separation of designation and authorization concerns. It is usually a string and it can have an age to it. It is issued by the resource owner server and is enforced by the resource owner server and the authorization server.

Once you log in with a username and the password, the server validate and sends back an access token. Whenever authorization is needed, send the auth token with the request. At logout, the app code destroys the token.

## Good Access Tokens

* Well Structured
* Well defined
* URL Safe
* Self Contained
* Compact

This is where JWT comes in.

## JSON Web Tokens - RFC7519

There are three parts to a JWT. Header, Payload and Signature.

The header is base64 encoded and it looks like

```json
{
    "alg": "H256",
    "typ": "JWT"
}
```

The payload is whatever you want to put in the JWT and it is also base64 encoded. Combining the header and the payload will generate a signature for the token. The signature should be able verify a tamperproof JWT.

The header only has an `alg` field and a `typ` field. The `alg` field specifies which algorithm was used to sign this token and `typ` is what type the token is.

The Payload contains some Claims. There are some Registered Claims (JWT keywords, basically), public claims and private claims. The registered claims usually have things like issuer, subject, audience, expirations, etc.

Public Claims are claims registered with IANA JWT Claims Registry. They comprise of common datatypes and keywords.

Private claims are agreed upon by the producer and consumers. There can't be conflicts between public or registered.

## Building Use Cases

* Login - The server sends back a JWT. Authorization is done using the `Authorization: Bearer JWT` header. Token can be used to customize the expiry, limit API permissions and can have session data as the payload.
* Calling REST APIs - Similar structure as the login, the issuer and the subject are both services at this point. The payload can be the POST request body.
* Federated Authentication - Run to the authentication server and get a token. Send the token to another service that authorizes your actions with the same authentication server. This is OAuth 2 type use cases

## Some code

`npm install jsonwebtoken`

The Node.js library doesn't some checks that the Java/.NET versions do. These libraries also don't support rolling keys but that can be easily built out if needed.

```javascript
var jwt = require('jsonwebtoken');

var dataToEncode = {

}

var encodedToken = jwt.sign(dataToEncode, 'this is a secret');

// this works
var decodedToken = jwt.sign(encodedToken, 'this is a secret');

// this doesn't work
var decodedToken = jwt.sign(encodedToken, 'this is not a secret');
```

## JWT and Security

CIA - Confidentiality, Integrity, Availablity

JWT helps with the Integrity part of the equation. An option to increase confidentiality is to use a JWE which is an encrypted JWT. There are other specs like JWS too.

## Takeaways

* JWT is an open standard
* It's a simple data structure
* Compact, self-contained, well-structured
* Base64 encoded, URL safe
* Signed but not encrypted so HTTPS is required
* Meant for checking the integrity
* [JWT Website](https://jwt.io)

Cookies are still good to use in certain cases but JWTs can be a compelling alternative!
