# Cognito JWKS

JSON Web Key Sets (JWKS) is a set of keys containing the public keys used to verify any JSON Web Token (JWT) issued by an Authorization Server and signed using the RS256 signing algorithm. These are exposed via the .well-known [1] folder on the servers, and Cognito is no different.

The JWKS configuration for a Cognito pool is available from: `https://cognito-idp.<region>.amazonaws.com/<pool_id>/.well-known/jwks.json`

## References

  1. [Well-known URI](https://en.wikipedia.org/wiki/Well-known_URI)
  2. [JSON Web Key (JWK)](https://datatracker.ietf.org/doc/html/rfc7517)
