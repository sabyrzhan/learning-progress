## [[REST API in using plain Golang 1.22 http.ServeMux]]

	- Building REST APIs With Go 1.22 http.ServeMux
		- https://shijuvar.medium.com/building-rest-apis-with-go-1-22-http-servemux-2115f242f02b
	- Reddit: **Let’s say you want to build a Go REST API. Should you use the standard library, a router, or a full-blown framework?**
		- https://www.reddit.com/r/golang/comments/15y5wiq/lets_say_you_want_to_build_a_go_rest_api_should/
	- Better HTTP server routing in Go 1.22
		- https://eli.thegreenplace.net/2023/better-http-server-routing-in-go-122
	- Go's 1.22+ ServeMux vs Chi Router
		- https://www.calhoun.io/go-servemux-vs-chi/
	- ```
	  package main
	  
	  import (
	  	"encoding/json"
	  	"fmt"
	  	"io"
	  	"log"
	  	"net/http"
	  	"time"
	  )
	  
	  func main() {
	  	router := initializeRoutes()
	  
	  	server := &http.Server{
	  		Addr:    ":8080",
	  		Handler: router,
	  	}
	  	log.Println("Listening...")
	  	_ = server.ListenAndServe() // Run the http server
	  }
	  
	  func initializeRoutes() *http.ServeMux {
	  	mux := http.NewServeMux()
	  	mux.HandleFunc("GET /api/hello", func(writer http.ResponseWriter, request *http.Request) {
	  		response := `
	  {
	  	"hello": "world"
	  }
	  `
	  		_, _ = writer.Write([]byte(response))
	  	})
	  	mux.HandleFunc("GET /api/{param}", func(writer http.ResponseWriter, request *http.Request) {
	  		param := request.PathValue("param")
	  		response := `
	  {
	  	"helloWithParam": "%s"
	  }
	  `
	  		_, _ = writer.Write([]byte(fmt.Sprintf(response, param)))
	  	})
	  	mux.HandleFunc("POST /api/data", func(writer http.ResponseWriter, request *http.Request) {
	  		// Reading and writing raw string data
	  		data, _ := io.ReadAll(request.Body)
	  		strData := string(data)
	  		strData += "\nUpdated data"
	  		_, _ = writer.Write([]byte(strData))
	  	})
	  	mux.HandleFunc("POST /api/json/data", func(writer http.ResponseWriter, request *http.Request) {
	  		var jsonData map[string]interface{}
	  		_ = json.NewDecoder(request.Body).Decode(&jsonData)
	  		jsonData["updatedAt"] = time.Now().UTC()
	  		_ = json.NewEncoder(writer).Encode(jsonData)
	  	})
	  
	  	return mux
	  }
	  
	  ```