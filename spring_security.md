# Spring Security
## Flow
Request flow looks following:
`User request -> Authentication filter -> Authentication manager -> Authentication provider (uses User details service and password encoder/decoder)`

1. Authentication filter - component that checks wether request is authenticated.
2. Authentication manager - holds needed authentication provider
3. Authentication provider - actual logic used to authenticate the request. Internally it uses `UserDetailsService` and `PasswordEncoder`:
    1. `UserDetailsService` - interface that fetches user details from data store or other locations.
    2. `PasswordEncoder` - used to validate user password if used.

After validating the response propogates back to user in the following flow:
`Authentication provider -> Authentication manager -> Authentication filter (uses AuthenticationSuccessHandler or AuthenticationFailureHandler depending on the result) -> User

Here `AuthenticationFilter` invokes one of the following functions depending on the result:
1. `AuthenticationSuccessHandler` - if the authentication is successfull, it fills `SecurityContext` with authentication details.
2. `AuthenticationFailureHandler` - returns response about authentication failure.
