## [[new(struct) vs &struct{}]]

	- There are no differences between them. With `new()` function it initializes with 0 values and returns pointer.
	- Whereas with `&struct{}` you can set initial values inside curly braces
- ## [[new() vs make() function]]
	- `new()` - creates value and returns pointer to it. Accepts only basic data types like struct, int etc
	- `make()` - accepts only slice, map and channel types and returns created value of it but not pointer
- ## [[pass-by-reference types]]
	- in Go **slices, maps, and channel** are always passed-by-reference not by-value
- ## [[Golang interfaces]]
	- Go interface internals by golang creator - https://www.airs.com/blog/archives/277
	- Go interfaces & pointers - https://web.archive.org/web/20170120032933/https://medium.com/@agileseeker/go-interfaces-pointers-4d1d98d5c9c6
	- How to user interfaces in Go - https://jordanorelli.com/post/32665860244/how-to-use-interfaces-in-go
	- Meaning of interface - https://stackoverflow.com/questions/23148812/whats-the-meaning-of-interface
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
- ## [[Time format]]
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