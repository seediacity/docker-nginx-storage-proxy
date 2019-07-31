# docker-nginx-storage-proxy
A Docker image for running Nginx as a caching proxy for Google Cloud Storage or Azure Blob storage.

This is the Git repo for the Docker image built automatically at Docker Hub - 
[seedia/nginx-storage-proxy](https://hub.docker.com/r/seedia/nginx-storage-proxy/).

Based on [nginx-gcs-proxy](https://github.com/socialwifi/docker-nginx-gcs-proxy) by SocialWifi.

## Usage

```bash
# for GCS 

docker run -d \
 -e GCS_BUCKET_URL="[bucket/folder]" \
 -p 8080:8080 seediacity/nginx-storage-proxy

# or for Azure Blob Storage

docker run -d \
 -e AZ_STORAGE_ACCOUNT_NAME="[account-name]" \
 -e AZ_STORAGE_BLOB_URL="[container/blob]" \
 -p 8080:8080 seediacity/nginx-storage-proxy

```

## Configuration

The following tables lists the configurable environment variables of nginx-gcs-proxy and their default values.

Variable | Description | Default
--- | --- | ---
`GCS_BUCKET_URL` | Full URL to the bucket folder. `https://storage.googleapis.com/[GCS_BUCKET_URL]/index.html` | None - either GCS or AZ required
`AZ_STORAGE_ACCOUNT_NAME` | Name of the storage account. `https://[AZ_STORAGE_ACCOUNT_NAME].blob.core.windows.net/my-bucket/some-other-dir/index.html` | None - either GCS or AZ required
`AZ_STORAGE_BLOB_URL` | Container name with folder (blob) path. `https://mystorageaccount.blob.core.windows.net/[AZ_STORAGE_BLOB_URL]/index.html` | None - either GCS or AZ required
`LISTEN_PORT` | Server listen port | 8080
`NOT_FOUND_MEANS_INDEX` | When requested path is not found in the bucket, return index.html. Useful when serving single page apps, like Angular, React, Ember. Possible values: "true", "false". | false
`PROXY_HTTPS_ENABLED` | Use https to connect with remote storage, if disabled http is used. Possible values: "true", "false". | false

## Health-checking

```bash
curl -v http://127.0.0.1:8080/healthz/

```
```
*   Trying 127.0.0.1...
* TCP_NODELAY set
* Connected to 127.0.0.1 (127.0.0.1) port 8080 (#0)
> GET /healthz/ HTTP/1.1
> Host: 127.0.0.1:8080
> User-Agent: curl/7.55.1
> Accept: */*
> 
< HTTP/1.1 200 OK
< Server: nginx
< Date: Wed, 17 Jan 2018 14:22:23 GMT
< Content-Type: application/octet-stream
< Content-Length: 0
< Connection: keep-alive
< 
* Connection #0 to host 127.0.0.1 left intact
```

## Building

```bash
docker build nginx-storage-proxy -t seediacity/nginx-storage-proxy

```

## Testing

```bash
docker run --rm -e GCS_BUCKET_URL="dummy" seediacity/nginx-storage-proxy nginx -t
```

## Running with logs in console

```bash
docker run --rm -ti \
  -e PROXY_HTTPS_ENABLED=true 
  -e AZ_STORAGE_ACCOUNT_NAME="dummy-account" \
  -e AZ_STORAGE_BLOB_URL="dummy-container/dummy-dir" \
  -p 8088:8080 seediacity/nginx-storage-proxy
```
