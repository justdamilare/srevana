---
title: "Load CSV files in Go"
date: 2021-01-01T09:56:54+01:00
draft: false
---

This is a very easy way to open a CSV file line by line in Go. Suppose we have a CSV file `pops.csv` that looks like this 
| City | Population|
|--------| ---------|
| Lagos | 21000000 |
| Tokyo | 37400000 |
| Delhi | 28500000 |
| Rio de Janeiro | 13300000 |

To read the file line by line,

```go
package main

import (
	"encoding/csv"
	"fmt"
	"io"
	"os"
)

type CityPopulation struct {
	City  string
	Population string
}

func check(e error) {
	if e != nil {
		panic(e)
	}

}

func main() {
	fmt.Println("This is the entry point!")

	file, err := os.Open("pops.csv")
	check(err)
	println("Log: Opened CSV file")

	reader := csv.NewReader(file)

	var pops []CityPopulation

	for {
		line, err := reader.Read()

		if err == io.EOF {
			break
		}
		check(err)

		pops = append(pops, CityPopulation{
			City:  line[0],
			Population: line[1],
		})
	}
	for i := range pops {
        fmt.Printf("The city of %s is populated with %s people.\n" , pops[i].City, pops[i].Population)
	}
}
```

We created a struct `CityPopulation` to hold the values of the CSV. Then we created a function `check(e error)` to check for errors. The function only checks for errors in the file an panics. 

In the main function, we use `os.Open("pops.csv")` to open the CSV file and load it. We then use `csv.NewReader()` to create a reader. We then use a for loop to read the file line by line. Notice the `io.EOF`, it is emitted by reader.Read() when it gets to the end of the CSV file. We then load the contents of the line into the `pops` slice that was created earlier.