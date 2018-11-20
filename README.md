# go-notes

Convert float64 to []byte and then convert back to float64: 

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
