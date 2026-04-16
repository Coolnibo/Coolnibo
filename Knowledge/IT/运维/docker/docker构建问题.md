




```shell

docker buildx build --platform linux/arm64 
  --build-arg http_proxy=http://127.0.0.1:7890 
  --build-arg https_proxy=http://127.0.0.1:7890 
  -t kkfileview-base-arm:4.4.0 --load .

```