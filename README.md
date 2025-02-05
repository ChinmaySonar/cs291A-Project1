# cs291A-Project1
AWS Lambda and JSON Web Tokens (JWT)


A semi-trivial HTTP API


# API Specification
# GET /

This protected endpoint is used to simply reflect the content contained within the data field of a valid JWT. It is a demonstration of how authorization can be enforced on an endpoint.

Requires a valid token from POST /token passed via the HTTP Header Authorization whose value is Bearer <TOKEN>.

On success, returns a json document containing the contents of the data field from the provided token and responds with the status code 200.

Responds 401 if either the token is not yet valid, or if it is expired.

Responds 403 if a proper Authorization: Bearer <TOKEN> header is not provided.

# POST /token

This endpoint is used to obtain the JWT necessary to request /. Normally such an endpoint would be used to authenticate a user (e.g., verify a username and password) before returning an authorization token. For simplicity we’re skiping the authentication step and will always return a token for a valid request.

On success, returns a json document of the format {"token": <GENERATED_JWT>} with status code 201 where <GENERATED_JWT>:

uses a HS256 signature (the symmetric key to use is in the environment variable JWT_SECRET)

contains exactly three fields

data which includes the request body from the HTTP requst

exp which is set to 5 seconds after the generation time

nbf which is set to 2 seconds after the generation time

Responds 415 if the request content type is not application/json.

Responds 422 if the body of the request is not actually json.

# Other Specifications

All HTTP responses should have the content type application/json. You shouldn’t need to do anything to satisfy this requirement.

Requests to any other resources must respond with status code 404.

Requests to / or /token which do not use the appropriate HTTP method must respond with status code 405.
