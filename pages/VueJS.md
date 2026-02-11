## [[Env variables]]
	- Env variables can be defined in `.env` file located in the root directory of the project. They are automatically resolved and can be used in your app.
	- Env variables are exported differently depending how you are starting or building your app.
	- If you are using old Vue CLI app then variables must start with `VUE_APP`. For example:
	  ```
	  VUE_APP_REST_API=http://my-rest-api.com/v1
	  ```
	- If you are using Vite then variables must start with `VITE`. For example:
	  ```
	  VITE_REST_API=http://my-rest-api.com/v1
	  ```