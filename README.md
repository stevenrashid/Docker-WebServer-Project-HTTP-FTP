# Java HTTP Server with FTP Fallback

A Java networking project that serves local files over HTTP and attempts to retrieve missing text files from a local FTP server.

It demonstrates TCP sockets, request parsing, thread-per-connection handling, and FTP control/data channels using Java's standard library.

## How it works

1. `WebServer` listens on port **6789** and starts a thread for each accepted connection.
2. The request handler maps the requested path to a file in its working directory.
3. Existing files are returned with an HTTP/1.0 response and a basic content type.
4. For a missing `.txt` file, `FtpClient` connects to **127.0.0.1:21**, authenticates, enters passive mode, and attempts to download the file.
5. Other missing file types receive a 404 response.

## Files

| File | Purpose |
| --- | --- |
| [WebServer.java](WebServer.java) | Server entry point, request handling, file responses, and FTP fallback. |
| [FtpClient.java](FtpClient.java) | FTP login, passive-port parsing, retrieval, and local file writing. |

## Run a local HTTP example

Use a JDK with `javac` and `java` available.

```bash
git clone https://github.com/stevenrashid/Docker-WebServer-Project-HTTP-FTP.git
cd Docker-WebServer-Project-HTTP-FTP
javac WebServer.java FtpClient.java
```

Create an `index.html` file in the same directory, then run:

```bash
java WebServer
```

Open [http://localhost:6789/index.html](http://localhost:6789/index.html). Request the filename explicitly: the server does not automatically map `/` to an index page.

## Exercise the FTP fallback

Configure a local FTP server listening on port 21 with passive-mode support and a text file accessible from its root. Update the hard-coded login arguments in `WebServer.java` for that local test account, recompile, and request a matching `.txt` file that is absent from the HTTP working directory.

The FTP client also uses loopback for its data connection. If you run the FTP service in Docker, map the control and passive ports so they are reachable from the Java process.

## Current scope

Despite the repository name, no Dockerfile, Compose configuration, FTP-server setup, or sample web content is checked in. The Java sources can be compiled directly.

This is a local networking exercise. Request paths are not sanitized, FTP is unencrypted, and transfer/error handling is minimal. Run it in an isolated development environment; it is not ready for public hosting.
