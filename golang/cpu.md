# Understanding the CPU Architecture

I needed to understand what CPU some code was running on so that different data could be used during some processing.


This can be done relatively easily using the inbuilt `runtime` package like this:

```golang
package main

import (
    "fmt"
    "runtime"
)

func main() {
    fmt.Printf("Running on %s/%s)", runtime.GOOS, runtime.GOARCH)
}
```

I did also find the [cpuid](https://github.com/klauspost/cpuid) package which provides much more information and could be useful in wider instances.


## References

* https://stackoverflow.com/questions/36024542/how-do-i-determine-whether-the-program-was-built-as-32-bit-or-64-bit-in-go
