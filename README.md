# go-notes

Convert float64 to []byte and then convert back to float64: https://play.golang.org/p/tIZZJZhIFzq

    package main

    import (
      "fmt"
      "math"
      "encoding/binary"
    )

    func main() {

        var c float64
        c = 52.53
        fmt.Println(c)
        // Convert float64 to uint64 first
        bits := math.Float64bits(c)
        // bits is uint64
        fmt.Println(bits)
        bytes := make([]byte, 8 )
        fmt.Println(bytes)
        binary.BigEndian.PutUint64(bytes, bits)
        fmt.Println(bytes)

        // Conversion from []byte:
        tagInt := binary.BigEndian.Uint64(bytes) // prop.Value is of type []byte
        // tagInt is uint64
        fmt.Println(tagInt)
        tagVal := math.Float64frombits(tagInt)

        fmt.Println(tagVal)
    }

Use nested struct in message pack: 

    package main

    import (
        "fmt"
        "github.com/vmihailenco/msgpack"
        "time"
    )

    func main() {

        type SmallStruct struct {
            SS int `msgpack:"ss"`
        }

        type BigStruct struct {
            Timestamp   int64       `msgpack:"timestamp"`
            SmallStruct SmallStruct `msgpack:"smallStruct"`
        }

        s := SmallStruct{10}

        data := BigStruct{
            Timestamp:   time.Now().Unix(),
            SmallStruct: s,
        }

        dataBs, err := msgpack.Marshal(data)
        fmt.Println(dataBs)
        fmt.Println(err)

        var v BigStruct

        err = msgpack.Unmarshal(dataBs, &v)

        fmt.Println(v.SmallStruct.SS)

    }
