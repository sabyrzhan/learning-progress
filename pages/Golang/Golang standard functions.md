## [[strconv.Quote() for quoting string]]
	- ```
	  str = `"Golang is statically typed "`
	  fmt.Println(strconv.Quote(str))
	  ```
- ## [[Serialization using gob]]
	- More info also here: https://stackoverflow.com/a/71237167
	- To serialize and deserialize data it is possible to use standard `gob.Encoder` and `gob.Decoder` library (for sending data over the network or saving/restoring data). When deserializing data keep in mind the target structure must comply to source data structure according to the rule https://pkg.go.dev/encoding/gob#hdr-Types_and_Values. Also it only serializes exported data (i.e. public fields). Functions and channels also treated not exported. If you want to export them you have handle them manually by implementing `DataEncoder` and `DataDecoder` called `MarshalBinary()` and `UnmarshalBinary()`
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