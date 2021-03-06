# Yet Another DKIM Verifier

## Why not?
I found some DKIM solution for go, but I don’t like untested C binding and I have some time (thank you mr.Putin)
I wrote this code for my pet project.

## What can we do?

We can verify DKIM headers very fast.
It is about 44mb per second on my laptop.
We support custom resolving and custom cache logic. 

## TODO
- Support Length tag
- Support Time
- Support ExpireTime
- Support Copied header fields (z=)
- Sign

## How to use it?
```
package main

import (
    "bufio"
    "flag"
    "fmt"
    "github.com/kalloc/dkim"
    "os"
)

// ./test path/to/emls/*.eml

func main() {
    var filename string
    var fp *os.File
    var err error
    var dk *dkim.DKIM
    flag.Parse()

    for _, filename = range flag.Args() {
        fmt.Printf("Check: %s — ", filename)
        if fp, err = os.Open(filename); err != nil {
            fmt.Printf("ERR-WRONG_FILE")
        } else if dk = dkim.ParseEml(bufio.NewReader(fp)); dk == nil {
            fmt.Printf("ERR-DKIM_NOT_FOUND")
        } else if _, err = dk.GetPublicKey(); err != nil {
            fmt.Printf("ERR-DKIM_PK_NOT_FOUND")
        } else if dk.Verify() == false {
            fmt.Printf("ERR-DKIM_NOT_VERIFIED (Body is %v, Sig is %v)", dk.Status.ValidBody, dk.Status.Valid)
        } else {
            fmt.Printf("OK")
        }
        fmt.Printf("\n")
    }

}
```
