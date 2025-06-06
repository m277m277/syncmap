[![GitHub Workflow Status (branch)](https://img.shields.io/github/actions/workflow/status/yyle88/syncmap/release.yml?branch=main&label=BUILD)](https://github.com/yyle88/syncmap/actions/workflows/release.yml?query=branch%3Amain)
[![GoDoc](https://pkg.go.dev/badge/github.com/yyle88/syncmap)](https://pkg.go.dev/github.com/yyle88/syncmap)
[![Coverage Status](https://img.shields.io/coveralls/github/yyle88/syncmap/master.svg)](https://coveralls.io/github/yyle88/syncmap?branch=main)
![Supported Go Versions](https://img.shields.io/badge/Go-1.22%2C%201.23-lightgrey.svg)
[![GitHub Release](https://img.shields.io/github/release/yyle88/syncmap.svg)](https://github.com/yyle88/syncmap/releases)
[![Go Report Card](https://goreportcard.com/badge/github.com/yyle88/syncmap)](https://goreportcard.com/report/github.com/yyle88/syncmap)

# SyncMap - A Type-Safe Wrapper for `sync.Map`

`SyncMap` is a **type-safe** and **generic** wrapper around Go's `sync.Map`. It simplifies the use of `sync.Map` by allowing you to define the types for both keys and values.

It makes using `sync.Map` easier and safer by letting you define the types for keys and values when you create the `sync.Map`.

## README

[中文说明](README.zh.md)

## Why Choose SyncMap?

Using `SyncMap` has several advantages:

- **Generic Type**: Helps avoid type mismatches during runtime.
- **Clearer Code**: No need for type assertions from `interface{}`.
- **Simple Usage**: Works just same as `sync.Map`, same methods.

## Installation

```bash
go get github.com/yyle88/syncmap
```

## How to Use SyncMap

Here’s a simple example showing how you can use `SyncMap` to safely store and retrieve structured data.

### Example 1: Storing and Retrieving Data

```go
package main

import (
	"fmt"
	"github.com/yyle88/syncmap"
)

type User struct {
	Name string
	Age  int
}

func main() {
	// Create a SyncMap with string keys and User values
	users := syncmap.NewMap[string, *User]()

	// Add a user to the map
	users.Store("u1", &User{Name: "Alice", Age: 30})

	// Retrieve the user
	if user, ok := users.Load("u1"); ok {
		fmt.Printf("User: Name: %s, Age: %d\n", user.Name, user.Age) // Output: User: Name: Alice, Age: 30
	}
}
```

#### Explanation

1. **Defines Clear Types**: The key is a `string`, and the value is a `User` struct pointer, avoiding the use of `interface{}` for values.
2. **Simplifies Data Access**: No need for manual type assertions like `user = v.(*User)` when retrieving data.
3. **Works Same with `sync.Map`**: Functions like `Store` and `Load` are exactly the same as in `sync.Map`, so you don't need to learn new methods.

### Example 2: Storing, Deleting, and Iterating Over Data

```go
package main

import (
	"fmt"
	"github.com/yyle88/syncmap"
)

type Person struct {
	Name     string
	Age      int
	HomePage string
}

func main() {
	// Create a SyncMap with int keys and *Person values
	mp := syncmap.NewMap[int, *Person]()

	// Add some persons to the map
	mp.Store(1, &Person{
		Name:     "Kratos",
		HomePage: "https://go-kratos.dev/",
	})
	mp.Store(2, &Person{
		Name: "YangYiLe",
		Age:  18,
	})
	mp.Store(3, &Person{
		Name: "DiLiReBa",
		Age:  18,
	})

	// Delete the entry with key 3
	mp.Delete(3)

	// Iterate over all items in the map
	mp.Range(func(key int, value *Person) bool {
		fmt.Println(key, value.Name, value.Age, value.HomePage)
		return true
	})
}
```

#### Explanation

1. **Store and Delete**: This example shows how to add multiple entries and delete one using the `Delete` method.
2. **Iterate Over Data**: The `Range` method is used to iterate over all the key-value pairs in the map, which simplifies traversing the map compared to manually managing `sync.Map`'s iteration.
3. **Simplified Operations**: Just like with the previous example, no type assertions are required, and the usage is straightforward.

---

## SyncMap API

`SyncMap` provides the following functions:

| Function                  | Description                                                      |
|---------------------------|------------------------------------------------------------------|
| `Store(key, value)`       | Adds or updates a key-value.                                     |
| `Load(key)`               | Retrieves the value.                                             |
| `LoadOrStore(key, value)` | Returns the value if it exists; otherwise, adds a new key-value. |
| `Delete(key)`             | Removes the key-value pair from the map.                         |
| `Range(func)`             | Iterates over all key-value pairs in the map.                    |

Same as `sync.Map`.

---

## License

This project is open-source under the MIT License. You can find the full text of the license in the [LICENSE](LICENSE) file.

---

## Contributing

We welcome contributions of all kinds! Whether it’s reporting a bug, suggesting a feature, or submitting code improvements.

---

## Thank You

If you find this package valuable, give me some stars on GitHub! Thank you!!!

---

## GitHub Stars

[![starring](https://starchart.cc/yyle88/syncmap.svg?variant=adaptive)](https://starchart.cc/yyle88/syncmap)
