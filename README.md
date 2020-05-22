[![MIT license](http://img.shields.io/badge/license-MIT-brightgreen.svg)](http://opensource.org/licenses/MIT)
[![Packagist](https://img.shields.io/packagist/v/flownative/openidconnect-client.svg)](https://packagist.org/packages/flownative/openidconnect-client)
[![Maintenance level: Love](https://img.shields.io/badge/maintenance-%E2%99%A1%E2%99%A1%E2%99%A1-ff69b4.svg)](https://www.flownative.com/en/products/open-source.html)

# OpenID Connect Client for Flow Framework

This [Flow](https://flow.neos.io) package provides an [OpenID Connect](https://openid.net/connect/) client SDK.

## Configuration

    Flownative:
      OpenIdConnect:
        Client:
          services: []
            exampleService:
              options:
                discoveryUri: 'https://example.com/.well-known/openid-configuration'
                clientId: '@!EDD5.370D.8247.FED9!0001!B1C9.92C1!1008!13DB.54D8.65DE.2761'
                clientSecret: 'very-secret'
    
    Neos:
      Flow:
        http:
          chain:
            'postprocess':
              chain:
                'Flownative.OpenIdConnect.Client:setJwtCookie':
                  componentOptions:
                    cookieName: 'your-own-cookie-name-jwt'
                    secureCookie: false

        security:
          authentication:
            providers:
              'Flownative.OpenIdConnect.Client:OidcProvider':
                label: 'OpenID Connect'
                provider: 'Flownative\OpenIdConnect\Client\Authentication\OpenIdConnectProvider'
                providerOptions:
                  roles: ['Acme.MyPackage:User']
                  accountIdentifierTokenValueName: 'inum'
                  jwtCookieName: 'your-own-cookie-name-jwt'
                  serviceName: 'exampleService'
                token: 'Flownative\OpenIdConnect\Client\Authentication\OpenIdConnectToken'
                requestPatterns:
                  'Acme.MyPackage:Frontend':
                    pattern: 'Acme\MyPackage\Security\SiteRequestPattern'
                    patternOptions:
                      'siteNodeName': 'mysite'
                      'matchFrontend': true
                entryPoint: 'Flownative\OpenIdConnect\Client\Authentication\OpenIdConnectEntryPoint'
                entryPointOptions:
                  serviceName: 'acmeservice'
                  scopes: ['inum', 'user_name', 'sap_custno']

### Roles from Identity Token

Instead of specifying the Flow authentication roles directly, the
provider can extract the roles from the identity token values. The roles
provided by the token must have the same identifier which is used in
Flow's policy configuration.

Given that the indentity token provides a claim called
"https://flownative.com/roles", you may configure the provider as
follows:

```
…
        security:
          authentication:
            providers:
              'Flownative.OpenIdConnect.Client:OidcProvider':
                label: 'OpenID Connect'
                provider: 'Flownative\OpenIdConnect\Client\Authentication\OpenIdConnectProvider'
                providerOptions:
                  rolesFromClaims:
                    - 'https://flownative.com/roles'
                  …
 
```

When a user logs in and her identity token has a value
"https://flownative.com/roles" containing an array of Flow role
identifiers, the OpenID Connect provider will automatically assign these
roles to the transient account.

You may specify multiple claim names which are all considered for
compiling a list of roles.

Check logs for hints if things are not working as expected.

## Authentication Flow

Authentication works as follows:

...

## About OpenID Connect

See also:

https://openid.net/specs/openid-connect-basic-1_0.html
https://connect2id.com/learn/openid-connect
