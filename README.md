# GoShare

A simple and light-weight ShareX image server designed to be "low-power". It's basically a Golang file server with an upload route built with an IP whitelist and token authentication. The server is designed to run headless and without any database requirements.

## Deploying an instance

Two commands will get this up and running. Firstly make sure you have [Docker](https://docker.io) installed on your machine. Once you've done that, edit the `main.go` file and change the variables as you please.

Now, you're ready to start the server, firstly, build the image for the server with the following command.
```
docker build . --tag goshare:latest
```

Wait for the image to build (even on a low-resource system like a Raspberry Pi, this should take no more than 5 minutes). Once complete, you can run the container with the command below.
```
docker run -d -v ~/goshare/uploads:/opt/site/uploads --name goshare --network="host" goshare:latest
```

You'll likely want to setup an Nginx Reverse Proxy in addition to CloudFlare to make this work best but I'm sure y'all can figure that out yourselves (plus there's like a million and 2 guides on it out there).

## Getting 419s when my MaxUploadSize is huge

If you're getting 419: Request Entity Too Large errors even though your file's size is within what is needed, then it's likely your Reverse Proxy. For Nginx users, the fix is simple, add the following to your config and reload.
```
client_max_body_size 100M;
```

**Note:** `100M` means 100 Megabytes, change it as you please.
