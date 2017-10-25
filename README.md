# ligo
Useful go library

### KeySearch

```
  KeySearch(key string, json interface{}, allowInterfaces bool) interface{}

  key: Key to be searched
  json: Unmarshaled json byte array
  allowInterfaces: Allows the first interface matching the key to be returned

  Note: All JSON numbers will be returned as float64
```
```go
import (
    "fmt"
    "encoding/json"
    "github.com/pmpbar/ligo"
)

func main() {
  byteData := "{ \"number\": 123, \"object\": { \"child\": \"hello world!\" } }"
  var data interface{}
  err := json.Unmarshal([]byte(byteData), &data)

  // Ok to cast if you know the key will always be there
  integer := ligo.KeySearch("number", data, false).(float64)
  fmt.Println(integer) // 123

	obj := KeySearch("object", data, true).(map[string]interface{})
  fmt.Println(obj["child"])

	child := KeySearch("child", data, false)
  fmt.Println(child.(string)) // "hello world!"

	notThere := KeySearch("asdf", data, false)
  if notThere == nil {
    fmt.Println("Nothing was found")
  }
}
```
