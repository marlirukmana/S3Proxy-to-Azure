download s3 jar:

wget https://repo1.maven.org/maven2/org/gaul/s3proxy/2.7.0/s3proxy-2.7.0-jar-with-dependencies.jar \
  -O /root/s3proxy/s3proxy.jar


docker-compose.yml :

```bash
version: "3.8"

services:
  s3proxy:
    image: openjdk:11-jre-slim
    container_name: s3proxy
    restart: unless-stopped
    volumes:
      - /root/s3proxy/s3proxy.conf:/opt/s3proxy/s3proxy.conf:ro
      - /root/s3proxy/s3proxy.jar:/opt/s3proxy/s3proxy.jar:ro
    working_dir: /opt/s3proxy
    command: ["java", "-jar", "/opt/s3proxy/s3proxy.jar", "--properties", "/opt/s3proxy/s3proxy.conf"]
    networks:
       marugo:
         ipv4_address: 10.8.1.8

networks:
   marugo:
     external: true
```bash
```bash
s3proxy.conf :

# Endpoint S3 Proxy
s3proxy.endpoint=http://0.0.0.0:8080
s3proxy.authorization=aws-v2-or-v4
s3proxy.identity=my-s3proxy-id
s3proxy.credential=my-s3proxy-secret

# Backend Azure Blob
jclouds.provider=azureblob
jclouds.identity=<AZURE_ACCOUNT_NAME>
jclouds.credential=<AZURE_ACCESS_KEY>
jclouds.endpoint=https://<AZURE_ACCOUNT_NAME>.blob.core.windows.net
```bash
