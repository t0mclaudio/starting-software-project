# How JWT Authentication Works

## Client request
1. Client logs in to the app
2. The App using the JWT module creates a token
3. Token is passed as a response to the client
4. Client saves token to `local storage/cookies`
5. Next time the client makes a request to a protected route, the token is passed as part of the `Header` as `Authorization` key

## Client makes a request to a protected route
1. User makes a request to a protected route
2. Request includes the `Authorization` key in the request `Headers`
3. **[Middleware]** App extracts the `Authorization` key from the request body
4. **[Middleware]** App extracts the `token` from the `Authorization` object using a split
5. **[Middleware]** Pass the `token` to the response payload
6. **[Middleware]** `Next()` is called
7. Using the `JWT.verify` method, it verifies if user is authenticated
8. If not send a 403
9. If autenticated continue processing the request and send response
