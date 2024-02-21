Scribble [![GoDoc](https://godoc.org/github.com/boltdb/bolt?status.svg)](http://godoc.org/github.com/saberder/nestedScribble) [![Go Report Card](https://goreportcard.com/badge/github.com/saberder/nestedScribble)](https://goreportcard.com/report/github.com/saberder/nestedScribble)
--------
This fork is to make scribble handle nested folder structures.
So you can have layoyts like bowls/{bowlname}/fish/{fishname}.json instead of the flat structure of only fish/{fishname}.json and no deeper.
ReadAll will still work by reading all the json files in bowls/{bowlname}/fish folder.

A tiny JSON database in Golang

### Installation

Install using `go get github.com/sdomino/scribble`.

### Usage

```go
// a new scribble driver, providing the directory where it will be writing to,
// and a qualified logger if desired
db, err := scribble.New(dir, nil)
if err != nil {
  fmt.Println("Error", err)
}

// Write a fish to the database
fish := Fish{}
if err := db.Write("fish", "onefish", fish); err != nil {
  fmt.Println("Error", err)
}

// Read a fish from the database (passing fish by reference)
onefish := Fish{}
if err := db.Read("fish", "onefish", &onefish); err != nil {
  fmt.Println("Error", err)
}

// Read all fish from the database, unmarshaling the response.
records, err := db.ReadAll("fish")
if err != nil {
  fmt.Println("Error", err)
}

fishies := []Fish{}
for _, f := range records {
  fishFound := Fish{}
  if err := json.Unmarshal([]byte(f), &fishFound); err != nil {
    fmt.Println("Error", err)
  }
  fishies = append(fishies, fishFound)
}

// Delete a fish from the database
if err := db.Delete("fish", "onefish"); err != nil {
  fmt.Println("Error", err)
}

// Delete all fish from the database
if err := db.Delete("fish", ""); err != nil {
  fmt.Println("Error", err)
}
```

## Documentation
- Complete documentation is available on [godoc](http://godoc.org/github.com/sdomino/scribble).
- Coverage Report is available on [gocover](https://gocover.io/github.com/sdomino/scribble)

## Todo/Doing
- Support for windows
- Better support for concurrency
- Better support for sub collections
- More methods to allow different types of reads/writes
- More tests (you can never have enough!)
