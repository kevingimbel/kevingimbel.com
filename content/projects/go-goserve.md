---
date: 2016-09-30T15:23:17+02:00
title: goserve
language: "Go"
project_url: https://github.com/kevingimbel/goserve
---
Executable around the Go HTTP Server. Used to serve static files from a directory.

<!--more-->

### Install
1. [Install Go](https://golang.org/doc/install#install).
2. Clone the repo `git clone https://github.com/kevingimbel/goserve.git`
3. Run `go build goserve.go` from within the new directory.

If you would like to run the program from everywhere, link it into you `$PATH` variable, e.g.:
```bash
  $ sudo ln -s $(pwd)/goserve /usr/local/bin
```

### Usage
`goserve` can be used from the command line as follows:

```bash
  $ goserve [-port ""]
```
This will serve the current directory to `localhost:8000` or the specified port.

### Test it
To test the server run `goserve` from the project directory and open [localhost:8000/example](localhost:8000/example)
