# Securing API endpoints

Seth (@spetryjohnson)
[Github](https://github.com/spetryjohnson)

## Some exploits

* Twitter - AP account tweets about the White House
* The Nissan Leap API doesn't have any validation or authorization

## Information Overload

Two versions OAuth, federated systems, authentication and authorization, oh my.

Don't roll your own, don't be a rookie.

## Agenda

* Identity vs. Authentication vs. Authorization
* Compare and contrast
* Other stuff

This is a summary of the options available for security.

## Identity

* Identity - your app's concept of a user.
* Authentication - securely link an identity to a request
* Authorization - is the identity allowed to access the resource

Not all APIs care about all 3 things.

## Authentication built into the web server

* Client Certs
* HTTP Basic Authentication
* HTTP Digest Authentication

### Client Certificate

A client cert is "reverse SSL", proves client identity to server. No usernames/passwords and on IIS, only "low code" for AD.

### HTTP Basic Authentication

The client asks for some resource, the server responds with 401 and the client responds with a base64 username and password pair. This uses the `Authorization` header. The main problem is that username and password are in plain text. You have to use TLS with this!

Revoking access requires changing passwords.

### HTTP Digest Authentication

The browser asks for a resource, the server responds with 401 and the client is given a nonce. The client then creates a hash using the nonce, the password and the username. Sends the nonce, the username and the hash and the server compares it.

This means that TLS is not required for this. Every request makes 2 calls and it prevents the server from storing passwords with encryption.

## API Keys

API keys are used instead of username/password pairs. There isn't a standard format but is usually a GUID. Sometimes associated with specific permissions. The main account password is never stored or shared. You can revoke an API key easily. The implementation can be customized, it is not limited by server platform.

An API key can be passed with the request in a query string or as a header. You want to use TLS here and use a header field instead of a query string.

Anyone who has a key can get access and API Keys are only has secure as the TLS implementation. Should salt and hash keys on the server side.

## API Keys as Cryptographic keys: HMAC

Singed requests == sensitive values not transmitted and the server is ensured of the request. Don't have to use TLS but should anyway. This can act as a Hashed Message Authentication Code or HMAC.

Signed requests verify identity without bearer tokens and TLS is not required. It proves that request was not modified in transit.

HMAC is awesome but there is a lot of complexity in calculating the hash. The server and the client has to compute hashes exactly the same way.

Signed request has to have atleast 2 things, a public identifier and a signature. The public identifier tells the server what secret to look up to compute the signature.

### What to use as the "secret value"?

Has to be something that the app can retrieve in clear test. Salted/hashed passwords won't work. Should have some sort of timeout so that exposure is limited.

## HMAC Process

When someone logs in, the server sends back the private key to the client. Uses private keys to sign requests. Issuing temporary keys is a good idea and deactivate the key when the user logs out.

## OAuth

For both 2-party and 3-party authorization scenario. Was designed originally as a delegate for 3 party authorization scenarios. The resource owner grants access to a client at the server and the client then makes an authorized request to the server.

**NOTE** - OAuth is not an authentication protocol. OAuth access tokens also don't have any information about identity so can't be used as Authentication.

### OAuth 1.0a

Uses signed request (HMAC system) so it doesn't require TLS. OAuth 1 works best with browsers since you have to go to a website. It is complex to implement, even with libraries.

### OAuth 2.0

No HMAC signature so it's simpler to implement but it is not backwards compatible. It is also more conducive to mobile or enterprise scenarios. OAuth 2.0 is a framework, not a protocol. OAuth 2.0 is less interoperable than OAuth 1.0a.

Eran Hammer (the guy who helped write OAuth 2.0) doesn't like it.

## OpenID Connect

Open standard built on OAuth 2.0 and it has ID tokens that store information about the identity.

## Enterprisey options

* SAML - ideal for SSO with centralized identity
* WS-Security - king of complexity but accounts for a lot of things

## What should I choose?

* Client certs are good for
* HTTP Basic if you can use TLS for everything
* HTTP Digest...just don't.
* API Keys as bearer tokens if you're okay with Basic Auth but need more freedom. This needs TLS.
* Consider API keys as signed requests if you can't use TLS but be prepared to publish algorithms to create signature
* OAuth 1.0a if writing a web-based service or if you can't have TLS
* OAuth 2.0 if you need non-web clients trading off for reduced security
* OAuth 2.0 + OpenID Connect if you need to authenticate against identity
* SAML or WS-Security for enterprise hell

## Takeaways

* Requests must use TLS or be signed with MAC
* If you need to interface with 3rd party APIs, use OAuth but know that it's not authentication.
* If you need identity and authentication, use OAuth 2.0 + OpenID Connect
