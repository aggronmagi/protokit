# protokit

[![Travis Build Status][travis-svg]][travis-ci]
[![codecov][codecov-svg]][codecov-url]
[![GoDoc][godoc-svg]][godoc-url]
[![Go Report Card][goreport-svg]][goreport-url]

A starter kit for building protoc-plugins. Rather than write your own, you can just use an existing one.

See the [examples](examples/) directory for uh...examples.

## Getting Started

```golang
package main

import (
	"github.com/golang/protobuf/protoc-gen-go/plugin"
	"github.com/pseudomuto/protokit"

	"log"
)

func main() {
	// all the reading, writing, and parsing is taken care of
	if err := protokit.RunPlugin(new(plugin)); err != nil {
		log.Fatal(err)
	}
}

type plugin struct{}

func (p *plugin) Generate(req *plugin_go.CodeGeneratorRequest) (*plugin_go.CodeGeneratorResponse, error) {
	// this will parse the requested proto files into `protokit.FileDescriptor` objects
	descriptors := protokit.ParseCodeGenRequest(req)

	resp := new(plugin_go.CodeGeneratorResponse)
	// resp.File = append(resp.File, &plugin_go.CodeGeneratorResponse_File{ ... }

	return resp, nil
}
```

Then invoke your plugin via `protoc`. For example (assuming your app is called `thingy`):

`protoc --plugin=protoc-gen-thingy=./thingy -I. --thingy_out=. rpc/*.proto`

[travis-svg]:
  https://travis-ci.org/pseudomuto/protokit.svg?branch=master
	"Travis CI build status SVG"
[travis-ci]:
  https://travis-ci.org/pseudomuto/protokit
  "protoc-gen-twagger at Travis CI"
[codecov-svg]: https://codecov.io/gh/pseudomuto/protokit/branch/master/graph/badge.svg
[codecov-url]: https://codecov.io/gh/pseudomuto/protokit
[godoc-svg]: https://godoc.org/github.com/pseudomuto/protokit?status.svg
[godoc-url]: https://godoc.org/github.com/pseudomuto/protokit
[goreport-svg]: https://goreportcard.com/badge/github.com/pseudomuto/protokit
[goreport-url]: https://goreportcard.com/report/github.com/pseudomuto/protokit
