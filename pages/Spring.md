- ## Spring Boot vs Spring Cloud
	- [Spring boot vs Spring Cloud](https://www.educba.com/spring-cloud-vs-spring-boot/)
## Spring Security
	- ### Request flow order
		- `User -> Authentication filter -> Authentication manager -> Authentication provider (uses User details service and password encoder)`
			- **Authentication filter** - component that checks wether request is authenticated.
			- **Authentication manager** - holds needed authentication provider
			- **Authentication provider** - actual logic used to authenticate the request. Internally it uses `UserDetailsService` and `PasswordEncoder`:
				- `UserDetailsService` - interface that fetches user details from data store or other locations.
				- `PasswordEncoder` - used to validate user password if used.
			- After validating the response is propagated back to user in the following order:
				- `Authentication provider -> Authentication manager -> Authentication filter (uses AuthenticationSuccessHandler or AuthenticationFailureHandler depending on the result) -> User`. Here `AuthenticationFilter` invokes one of the following functions depending on the result:
					- `AuthenticationSuccessHandler` - if the authentication is successfull, it fills `SecurityContext` with authentication details.
					- `AuthenticationFailureHandler` - returns response about authentication failure.
- ## [[Spring How-To]]
	- **Enable CORS on method with annotation in Spring Boot 2**
		- ```
		  @CrossOrigin(origins = "http://localhost:9000")
		  @GetMapping("/greeting")
		  public Greeting greeting(@RequestParam(required=false) String name) {
		      System.out.println("==== in greeting ====");
		      return new Greeting(counter.incrementAndGet(), String.format(template, name));
		  }
		  ```
	- **Enable HTTPS for Tomcat in Spring Boot  2**
		- `keytool -genkeypair -alias <key_alias> -keyalg RSA -keysize 2048 -storetype PKCS12 -keystore <key_name>.p12 -validity 3650`
		- Put keystore into `resources` folder
		- In `application.properties`
			- ```
			  # The format used for the keystore. It could be set to JKS in case it is a JKS file
			  server.ssl.key-store-type=PKCS12
			  # The path to the keystore containing the certificate
			  server.ssl.key-store=classpath:keystore/baeldung.p12
			  # The password used to generate the certificate
			  server.ssl.key-store-password=password
			  # The alias mapped to the certificate
			  server.ssl.key-alias=baeldung
			  #trust store location
			  trust.store=classpath:keystore/baeldung.p12
			  #trust store password
			  trust.store.password=password
			  ```
-