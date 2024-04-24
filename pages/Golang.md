## [[Serialization using gob.Encoder/gob.Decoder]]
	- To serialize and deserialize data it is possible to use standard `gob.Encoder` and `gob.Decoder` library (for sending data over the network or saving/restoring data). When deserializing data keep in mind the target structure must comply to source data structure according to the rule https://pkg.go.dev/encoding/gob#hdr-Types_and_Values. Also it only serializes exported data (i.e. public fields).
	  ```
	  type author struct {
	  	Name         string
	  	MobilePhones []string
	  }
	  
	  type Document struct {
	  	Author *author
	  	Data    string
	  	History *Document
	  	Version int
	  }
	  
	  // DeepCopy main function that makes deep clone of the document using gob.Encoder and gob.Decoder
	  func (d *Document) DeepCopy() *Document {
	  	fn := func() *Document {
	  		buffer := new(bytes.Buffer)
	  		encoder := gob.NewEncoder(buffer)
	  		err := encoder.Encode(d)
	  		if err != nil {
	  			panic(err)
	  		}
	  
	  		decoder := gob.NewDecoder(buffer)
	  		result := new(Document)
	  		err = decoder.Decode(result)
	  		if err != nil {
	  			panic(err)
	  		}
	  
	  		return result
	  	}
	  
	  	result := fn()
	  	result.History = fn()
	  	result.Version = result.Version + 1
	  
	  	return result
	  }
	  
	  // NewDocumentWithHistory creates Document with history using DeepCopy (Prototype pattern).
	  // Each history is the clone of the previous version
	  func NewDocumentWithHistory() *Document {
	  	docVersion1 := Document{}
	  	docVersion1.Author = &author{"New author", []string{"1111"}}
	  	docVersion1.Version = 1
	  	docVersion1.Data = "Document data. Newly written with version 1"
	  
	  	docVersion2 := docVersion1.DeepCopy()
	  	docVersion2.Data = "Document data. Newly written with version 2"
	  	docVersion2.History.Author.Name = fmt.Sprintf(docVersion2.History.Author.Name + " (Archived in version %d)", docVersion2.History.Version)
	  	docVersion2.AddPhoneNumber("222")
	  
	  	docVersion3 := docVersion2.DeepCopy()
	  	docVersion3.Data = "Document data. Newly written with version 3"
	  	docVersion3.History.Author.Name = fmt.Sprintf(docVersion3.History.Author.Name + " (Archived in version %d)", docVersion3.History.Version)
	  	docVersion3.AddPhoneNumber("333")
	  
	  	return docVersion3
	  }
	  ```
- ## [[Time format in golang]]
	- Docs:
		- SO: https://stackoverflow.com/questions/42217308/go-time-format-how-to-understand-meaning-of-2006-01-02-layout
		- Godoc: https://pkg.go.dev/time#example-Time.Format
	- ```
	  # Parse format
	  Jan 2 15:04:05 2006 MST
	  1   2  3  4  5    6  -7
	  
	  # Example
	  t, err := time.Parse(time.UnixDate, "Wed Feb 25 11:06:39 PST 2015")
	  
	  // The time zone attached to the time value affects its output.
	  fmt.Println("Same, in UTC:", t.UTC().Format(time.UnixDate))
	  fmt.Println("in Shanghai with seconds:", 
	  					t.In(tz).Format("2006-01-02T15:04:05 -070000"))
	  fmt.Println("in Shanghai with colon seconds:", 
	  					t.In(tz).Format("2006-01-02T15:04:05 -07:00:00"))
	                      
	  // time.Time's Stringer method is useful without any format.
	  fmt.Println("default format:", t)
	  
	  // Predefined constants in the package implement common layouts.
	  fmt.Println("Unix format:", t.Format(time.UnixDate))
	  
	  ```
- ## [[Install app or package globally]]
	- Source: https://stackoverflow.com/questions/36650052/golang-equivalent-of-npm-install-g/68559728#68559728
		- Starting with Go >= **1.16** the recommended way to install an executable is to use
		  ```
		  go install package@version
		  ```
		- For example, `go install github.com/fatih/gomodifytags@latest`.
		- Executables (main packages) are installed to the directory named by the `GOBIN` environment variable, which defaults to `$GOPATH/bin` or `$HOME/go/bin` if the `GOPATH` environment variable is not set. You need to add this directory to your `PATH` variable to run executables globally. In my case, I've added this line to my `~/.zshrc` file. (if you are using bash, add it to the `~/.bash_profile` file):
		- ```
		  export PATH="$HOME/go/bin:$PATH"
		  ```
		- Go team published a [blog post](https://blog.golang.org/go116-module-changes#TOC_4.) about this change, here's the explanation quote:
		- > We used to recommend go get -u program to install an executable, but this use caused too much confusion with the meaning of go get for adding or changing module version requirements in go.mod.
		- Refer to [`go install` documentation](https://golang.org/ref/mod#go-install) for more details
		- `go get` will not install packages "globally". It will add them to your project's go.mod file instead.
- ## [[Generics in golang]]
	- ```
	  // Generics in interface
	  type List[T comparable] interface {
	  	Add(item T) T
	  	Get(i int) (*T, error)
	  	IsEmpty() bool
	  	Length() int
	  	Remove(i int) (*T, error)
	  	Items() []T
	  }
	  
	  // Generics in struct
	  type ArrayList[T comparable] struct {
	  	items []T
	  }
	  
	  // Generics in method
	  func (a *ArrayList[T]) Add(item T) T {
	  	a.items = append(a.items, item)
	  	return item
	  }
	  
	  // Generic builder function
	  func NewArrayList[T comparable]() ArrayList[T] {
	  	return ArrayList[T]{}
	  }
	  
	  // Instantiation with builder
	  arrayList := NewArrayList[int]()
	  
	  // Generics in function
	  func BuildMap[KV comparable](kv ...KV) (map[KV]KV, error) {
	  	result := map[KV]KV{}
	  	if len(kv)%2 != 0 {
	  		return nil, fmt.Errorf("odd number of key/value pairs")
	  	}
	  
	  	for i := 0; i < len(kv); i += 2 {
	  		result[kv[i]] = kv[i+1]
	  	}
	  
	  	return result, nil
	  }
	  ```