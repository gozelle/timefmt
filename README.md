# timefmt-go
[![CI Status](https://github.com/gozelle/timefmt/workflows/CI/badge.svg)](https://github.com/gozelle/timefmt/actions)
[![Go Report Card](https://goreportcard.com/badge/github.com/gozelle/timefmt)](https://goreportcard.com/report/github.com/gozelle/timefmt)
[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/gozelle/timefmt/blob/main/LICENSE)
[![release](https://img.shields.io/github/release/itchyny/timefmt-go/all.svg)](https://github.com/gozelle/timefmt/releases)
[![pkg.go.dev](https://pkg.go.dev/badge/github.com/gozelle/timefmt)](https://pkg.go.dev/github.com/gozelle/timefmt)

### Efficient time formatting library (strftime, strptime) for Golang
This is a Go language package for formatting and parsing date time strings.

```go
package main

import (
	"fmt"
	"log"

	"github.com/gozelle/timefmt"
)

func main() {
	t, err := timefmt.Parse("2020/07/24 09:07:29", "%Y/%m/%d %H:%M:%S")
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println(t) // 2020-07-24 09:07:29 +0000 UTC

	str := timefmt.Format(t, "%Y/%m/%d %H:%M:%S")
	fmt.Println(str) // 2020/07/24 09:07:29

	str = timefmt.Format(t, "%a, %d %b %Y %T %z")
	fmt.Println(str) // Fri, 24 Jul 2020 09:07:29 +0000
}
```

Please refer to [`man 3 strftime`](https://linux.die.net/man/3/strftime) and
[`man 3 strptime`](https://linux.die.net/man/3/strptime) for formatters.
As an extension, `%f` directive is supported for zero-padded microseconds, which originates from Python.
Note that `E` and `O` modifier characters are not supported.

## Comparison to other libraries
- This library
  - provides both formatting and parsing functions in pure Go language,
  - depends only on the Go standard libraries not to grow up dependency.
- `Format` (`strftime`) implements glibc extensions including
  - width specifier like `%6Y %10B %4Z` (limited to 1024 bytes),
  - omitting padding modifier like `%-y-%-m-%-d`,
  - space padding modifier like `%_y-%_m-%_d`,
  - upper case modifier like `%^a %^b`,
  - swapping case modifier like `%#Z`,
  - time zone offset modifier like `%:z %::z %:::z`,
  - and its performance is very good.
- `AppendFormat` is provided for reducing allocations.
- `Parse` (`strptime`) allows to parse
  - composed directives like `%F %T`,
  - century years like `%C %y`,
  - week names like `%A` `%a` (parsed results are discarded).
- `ParseInLocation` is provided for configuring the default location.

![](https://user-images.githubusercontent.com/375258/88606920-de475c80-d0b8-11ea-8d40-cbfee9e35c2e.jpg)

## Bug Tracker
Report bug at [Issues・itchyny/timefmt-go - GitHub](https://github.com/gozelle/timefmt/issues).

## Author
itchyny (https://github.com/itchyny)

## License
This software is released under the MIT License, see LICENSE.
