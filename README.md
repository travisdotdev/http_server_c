# C HTTP Server

A minimal HTTP/1.1 server written from scratch in C using the POSIX sockets API. It accepts TCP connections, parses incoming HTTP requests, and serves a static HTML file in response to `GET` requests.

This began as a learning project to understand network programming at the systems level how a web server actually works beneath the abstractions of higher-level frameworks.

## Features

- Listens for TCP connections on a configurable port (default `8080`)
- Parses the HTTP request line to extract the request method
- Serves a static `index.html` file for `GET` requests
- Returns an HTTP/1.1 response (status line, headers, and body)
- Built directly on the BSD sockets API no external libraries

## Concepts demonstrated

- TCP socket setup with `getaddrinfo()`, `socket()`, `bind()`, `listen()`, and `accept()`
- Protocol-agnostic address handling via `struct addrinfo` and `struct sockaddr_storage`
- Reusing a port across restarts with `SO_REUSEADDR`
- Reading from and writing to sockets with `recv()` and `send()`
- Basic HTTP request parsing and response construction

## Requirements

- A C compiler (`gcc` or `clang`)
- A POSIX-compliant system (Linux or macOS)

## Build

```bash
gcc -Wall -Wextra -std=c11 -o server server.c
```

(Adjust the source filename if yours differs.)

## Run

Make sure an `index.html` file exists in the same directory, then start the server:

```bash
./server
```

You should see:

```
server: waiting for connections...
```

Then open a browser and visit `http://localhost:8080`, or test it from the command line:

```bash
curl -v http://localhost:8080
```

## Project structure

```
.
├── server.c      # the HTTP server
├── index.html    # the page served on GET requests
└── README.md
```

## Limitations & roadmap

This is an early-stage learning project. It works, but it is intentionally minimal the following are known limitations and planned improvements:

- **Concurrency** — handles one connection at a time. Planned: serve concurrent clients via `fork()`, threads, or an event loop (`poll`/`epoll`).
- **HTTP methods** — only `GET` is handled. Planned: support additional methods and return `405 Method Not Allowed` for the rest.
- **Routing** — a single hardcoded file is served. Planned: map request paths to files and return `404 Not Found` for missing resources.
- **Request parsing** — minimal for now. Planned: robustly parse the full request line and headers.
- **File I/O** — the response body is written one byte at a time. Planned: buffered reads and a correct `Content-Length` header.
- **IP version** — IPv4 only. Planned: IPv6 support via `AF_UNSPEC`.
- **Robustness** — planned: comprehensive error handling and input validation.

## Acknowledgments

Built while working through [Beej's Guide to Network Programming](https://beej.us/guide/bgnet/).
