GopherJS - A transpiler from Go to JavaScript
---------------------------------------------

### Useful Links
- Try GopherJS on the [GopherJS Playground](http://neelance.github.io/gopherjs-playground/)
- Get help in the [Google Group](https://groups.google.com/d/forum/gopherjs)

### What is GopherJS?
GopherJS translates [Go code](http://golang.org/) to pure JavaScript code. Its main purpose is to give you the opportunity to write front-end code in Go which will still run in all browsers.

You can take advantage of Go's elegant type system and other compile-time checks that can have a huge impact on bug detection and the ability to refactor, especially for big projects. Just think of how often a JavaScript method has extra handling of some legacy parameter scheme, because you don't know exactly if some other code is still calling it in that old way or not. GopherJS will tell you and if it does not complain, you can be sure that this kind of bug is not present any more.

### Installation and Usage
Get GopherJS and dependencies with: 
```
go get github.com/neelance/gopherjs
```
Now you can use  `./bin/gopherjs build` and `./bin/gopherjs install` which behave similar to the `go` tool. The generated JavaScript files can be used as usual in a website.

*Note: GopherJS will try to write compiled object files of the core packages to your $GOROOT/pkg directory. If that fails, it will fall back to $GOPATH/pkg.*

If you want to run the generated code with Node.js instead, get the latest 0.11 release from [here](http://blog.nodejs.org/release/). Then compile and install a Node module that is required for syscalls (e.g. writing output to terminal):
```
npm install node-gyp
cd src/github.com/neelance/gopherjs/node-syscall/
node-gyp rebuild
mkdir -p ~/.node_libraries/
cp build/Release/syscall.node ~/.node_libraries/syscall.node
cd ../../../../../
```

### What is supported?
The transpiler is able to turn itself (and all packages it uses) into pure JavaScript code that runs in all major browsers. It also passes a lot of the tests that are shipped with the Go source. This suggests a quite good coverage of Go's specification. However, there are some known exceptions listed below and some unknown exceptions that I would love to hear about when you find them.

Packages with passing test suites:

- archive/tar
- archive/zip
- bufio
- bytes
- compress/bzip2
- compress/gzip
- container/heap
- container/list
- crypto/aes
- crypto/cipher
- crypto/des
- crypto/dsa
- crypto/ecdsa
- crypto/elliptic
- crypto/hmac
- crypto/md5
- crypto/rand
- crypto/rc4
- crypto/sha1
- crypto/sha256
- crypto/sha512
- encoding/ascii85
- encoding/base32
- encoding/base64
- encoding/csv
- encoding/hex
- encoding/pem
- errors
- hash/adler32
- hash/crc32
- hash/crc64
- hash/fnv
- html
- image
- image/color
- image/draw
- image/gif
- image/jpeg
- io
- io/ioutil
- math
- math/cmplx
- math/rand
- mime
- path
- regexp
- sort
- strconv
- strings
- text/scanner
- text/tabwriter
- unicode
- unicode/utf8
- unicode/utf16

### Roadmap
These features are not implemented yet, but on the roadmap:

- reflection (already partially done)
- exact runtime type assertions for compound types without a name
- goroutines, channels, select
- goto
- output minification
- source maps

### Deviations from Go specification
Some tradeoffs had to be made in order to avoid huge performance impacts. Please get in contact if those are deal breakers for you.

- int and uint do not overflow, but have a maximum precision of 53 bits, after which rounding errors might occur (all other integer types have simulated overflow)
- float32 and complex64 have the same precision as float64 and complex128
- calls on nil cause a panic except for slice types

### Interface to external JavaScript
A function's body can be written in JavaScript by putting the code in a string constant with the name `js_[function name]` for package functions and `js_[type name]_[method name]` for methods. In that case, GopherJS disregards the Go function body and instead generates `function(...) { [constant's value] }`. This allows functions to have a Go signature that the type checker can use while being able to call external JavaScript functions.

### Libraries
[go-angularjs](https://github.com/neelance/go-angularjs) - a wrapper for [AngularJS](http://angularjs.org)
