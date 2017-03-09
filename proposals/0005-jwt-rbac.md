# Interoperable JWT RBAC

* Proposal: [MP0005-JWT_RBAC](MP0005-JWT_RBAC.md)
* Authors: [Scott Stark](https://github.com/starksm64), [Pedro Igor Silva](https://github.com/pedroigor)
* Status: **Awaiting review**

*During the review process, add the following fields as needed:*

* Decision Notes: [Discussion thread topic covering the  Rationale](https://groups.google.com/forum/#!topic/microprofile/gakCq7kSBsY), [Discussion thread topic with additional Commentary](https://groups.google.com/forum/#!forum/microprofile)

## Introduction

This specification outlines a proposal for using [OpenID Connect(OIDC)](http://openid.net/connect/) based [JSON Web Tokens(JWT)](https://tools.ietf.org/html/rfc7519) for role based access control(RBAC) of microservice endpoints.

Mailinglist thread: [Discussion thread topic for that proposal](https://groups.google.com/forum/#!forum/microprofile)

## Motivation

MicroProfile 1.1 is a baseline platform definition that optimizes Enterprise Java for a microservices architecture and delivers application portability across multiple MicroProfile runtimes. While Java EE is a very feature rich platform and is like a toolbox that can be used to to address a wide variety of application architectures, MicroProfile focuses on defining a small and a minimum set of Java EE standards that can be used to deliver applications based on a microservice architecture, are they:

* JAX-RS
* CDI
* JSON-P

The security requirements that involve microservice architectures are strongly related with RESTful Security. In a RESTful architecture style, services are usually stateless and any security state associated with a client is sent to the target service on every request in order to allow services to re-create a security context for the caller and perform both authentication and authorization checks.

One of the main strategies to propagate the security state from clients to services or even from services to services involves the use of security tokens. In fact, the main security protocols in use today are based on security tokens such as OAuth2, OpenID Connect, SAML, WS-Trust, WS-Federation and others. While some of these standards are more related with identity federation, they share a common concept regarding security tokens and token based authentication.

For RESTful based microservices, security tokens offer a very lightweight and interoperable way to propagate identities across different services, where:
* Services don’t need to store any state about clients or users
* Services can verify the token validity if token follows a well known format. Otherwise, services may invoke a separated service.
* Services can identify the caller by introspecting the token. If the token follows a well known format, services are capable to introspect the token by themselves, locally. Otherwise, services may invoke a separated service.
* Services can enforce authorization policies based on any information within a security token
* Support for both delegation and impersonation of identities

Today, the most common solutions involving RESTful and microservices security are based on [OAuth2](https://tools.ietf.org/html/rfc6749), [OpenID Connect](http://openid.net/connect/) and [JSON Web Token (JWT)](https://tools.ietf.org/html/rfc7519) standards.

### Token Based Authentication
Token Based Authentication mechanisms allow systems to authenticate, authorize and verify identities based on a security token. Usually, the following entities are involved:

* Issuer
  * Responsible for issuing security tokens as a result of successfully asserting an identity (authentication). Issuers are usually related with Identity Providers.
* Client
  * Represented by an application to which the token was issued for. Clients are usually related with Service Providers. A client may also act as an intermediary between a subject and a target service (delegation).
* Subject
  * The entity to which the information in a token refers to.
* Resource Server
  * Represented by an application that is going to actually consume the token in order to check if a token gives access or not to a protected resource.

Independent of the token format or protocol in use, from a service perspective, token based authentication is based on the following steps:

* Extract security token from the request
  * For RESTful services, this is usually achieved by obtaining the token from the Authorization header.
* Perform validation checks against the token
  * This step usually depends on the token format and security protocol in use. The objective is make sure the token is valid and can be consumed by the application. It may involve signature, encryption and expiration checks.
* Introspect the token and extract information about the subject
  * This step usually depends on the token format and security protocol in use. The objective is to obtain all the necessary information about the subject from the token.
* Create a security context for the subject
  * Based on the information extracted from the token, the application creates a security context for the subject in order to use the information wherever necessary when serving protected resources.

## Using JWT Bearer Tokens to Protect Services
For now, use cases are based on a scenario where services belong to the same security domain. This is an important note in order to avoid dealing with all complexities when you need to access services across different security domains. With that in mind, we assume that any information carried along with a token could be understood and processed (without any security breaches) by the different services involved.

The use case can be described as follows:

1. Client sends a HTTP request to Service A including the JWT as a bearer token:
```
    GET /resource/1 HTTP/1.1
    Host: example.com
    Authorization: Bearer mF_9.B5f-4.1JqM
```  
1. Token-Based Authentication Mechanism in front of Service A perform all steps described on the Token-Based Authentication Section.
1. Token-Based Authentication Mechanism perform a role and group mapping based on the JWT claims. Where the role and group mapping is fully configurable by the application.

[JWT](https://tools.ietf.org/html/rfc7519) tokens follow a well defined and known standard that is becoming the most common token format to protect services. It not only provides a token format but additional security aspects like signature and encryption based on another set of standards like JWS, JWE and others.

There are few reasons why JWT is becoming so widely adopted:
*Token validation doesn’t require an additional trip and can be validated locally by each service
*Given its JSON nature, it is solely based on claims or attributes to carry authentication and authorization information about a subject.
*Makes easier to support different types of access control mechanisms such as ABAC, RBAC, Context-Based Access Control, etc.
*Message-level security using signature and encryption as defined by both JWS and JWE standards
*Given its JSON nature, processing JWT tokens becomes trivial and lightweight. Especially if considering JEE standards such as JSON-P or the different third-party libraries out there such as Nimbus, Jackson, etc.
*Parties can easily agree on a specific set of claims in order to exchange both authentication and authorization information
*Widely adopted by different Single Sign-On solutions and well known standards such as OpenID Connect given its small overhead and ability to be used across different different security domains (federation)



## Recommendation for Interoperability

The decision about using JWT as token format also depends on the agreement between both identity providers and service providers. That means identity providers - responsible for issuing tokens - should be able to issue tokens using the JWT format in a way that service providers can understand in order to introspect the token and gather information about a subject.

As a recommendation, JWT tokens should follow the standard claims defined by [OpenID Connect Specification](http://openid.net/specs/openid-connect-core-1_0.html) to carry authentication related information about a subject. This specification defines a special type of token called ID Token, which defines a small set of required claims that can be used to identify the subject.
```json
  {
   		"iss": "https://server.example.com",
   		"sub": "24400320",
   		"aud": "s6BhdRkqt3",
   		"nonce": "n-0S6_WzA2Mj",
   		"exp": 1311281970,
   		"iat": 1311280970,
   		"auth_time": 1311280969,
	}
```
Please, note that OpenID Connect specification also defines a few other standard claims that can be used to identity the subject and improve the security of a token. The snippet above is just the bare minimum necessary to represent a subject with basic information about how tokens should be validated. Also note that the ID Token is not meant to be used as a bearer/access token to gain access to resources protected by a resource server or service. Here we propose a bearer/access token format, based on some claims from ID Token.

In addition to these standard claims, we also recommend a **preferred_username** claim to carry the username. For instance:
```json
  {
   		"iss": "https://server.example.com",
   		"sub": "24400320",
      "preferred_username": "jdoe",
   		"aud": "s6BhdRkqt3",
   		"nonce": "n-0S6_WzA2Mj",
   		"exp": 1311281970,
   		"iat": 1311280970,
   		"auth_time": 1311280969,
	  }
```

From an authorization perspective, we recommend two specific claims to carry role information about a subject.
```json
	{
   		"iss": "https://server.example.com",
   		"sub": "24400320",
     "preferred_username": "jdoe",
   		"aud": "s6BhdRkqt3",
   		"nonce": "n-0S6_WzA2Mj",
   		"exp": 1311281970,
   		"iat": 1311280970,
   		"auth_time": 1311280969,
      "realm_access": {
          "roles": ["role-in-realm", "user", "manager"]
      },
      "resource_access": {
          "my-service": {
            "roles": [
              "role-in-my-service"
            ]
          }
        },
      }
  }
```
The **realm_access** claim can be used to define which roles were granted to a subject at the realm or security domain level. On the other hand, the **resource_access** claim allows to define a different set of roles which are associated with a specific resource server or service.

## Impact on existing code (if applicable)

Existing services that desire to implement JWT RBAC must configure the container to support integration with an authorization implementation that is capable of parsing the JWT bearer token to validate the token and populate a security context with the caller and granted roles for use in ```getCallerPrincipal()/getUserPrincipal(), isCallerInRole(String)/isUserInRole(String)``` type of container methods.

## Alternatives considered

None

