# kedis

A compact, educational Redis-like toy written in modern C++ (C++20). This repository contains a minimal proof-of-concept TCP server and client to explore building a key-value store and the fundamentals of a Redis-like server.

Status: prototype — a single-threaded demo that accepts a connection, reads a short message, and replies. This is an experimental codebase for learning and iterating on a storage engine and protocol.

## What this repo contains

- CMakeLists.txt — simple CMake build that produces two executables: `server` and `client`.
- server.cpp — minimal TCP server that accepts connections on port 1234 and responds with a constant message.
- client.cpp — minimal TCP client that connects to localhost:1234, sends a short message, and prints the server reply.
- .idea/ — IDE settings (can be ignored in builds).

## Quickstart — build & run

From a fresh clone:

1. Configure & build (recommended):

```bash
mkdir -p build
cd build
cmake ..            # generates build system (requires CMake >= 3.20)
cmake --build . --target server
cmake --build . --target client
```

Or a single-step CMake configure+build:

```bash
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build --target server
cmake --build build --target client
```

2. Run the server (in one terminal):

```bash
./build/server
```

3. Run the client (in another terminal):

```bash
./build/client
```

Expected behavior:
- The client sends "hello" to the server.
- The server prints a log like `client says: hello` to stderr and writes back `world`.
- The client prints `server says: world`.

## Implementation notes (observed from current code)

- The server listens on port 1234 and is single-threaded: it accepts a connection, reads up to ~63 bytes, logs the client's message, writes `"world"` back, and closes the connection.
- The client connects to the loopback address (127.0.0.1), sends `"hello"`, reads the response and exits.
- The project is intentionally small and minimal so you can iterate quickly on protocol and storage design.

Caveat: the current code uses ntohs/ntohl where typically htons/htonl are expected when writing port and address fields. You may want to review the socket byte-order usage if you see networking issues on some platforms.

## Roadmap / TODO ideas

- Implement a simple command protocol (RESP or a tiny custom format).
- Add an in-memory key-value store with basic commands: SET, GET, DEL, EXISTS, KEYS.
- Add persistence options (AOF/RDB-like snapshots).
- Support multiple clients (thread pool or evented I/O with epoll/kqueue).
- Add tests, CI (GitHub Actions), and code formatting checks (clang-format).
- Implement more Redis-like features: pub/sub, expiry, replication.

## Contributing

Contributions welcome. Suggested workflow:
1. Open an issue describing the change/feature.
2. Create a branch `feat/<topic>` or `fix/<issue>`.
3. Send a PR with a short description and tests where appropriate.

If you'd like, I can open a PR to replace the repository README with this version.

## License

Add a license of your choice (MIT is a common choice for small projects). This repository currently doesn't include a license file; consider adding `LICENSE` if you want to clarify reuse terms.

## Contact / author

See the repository owner: orignlkartik1
