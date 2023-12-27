# 说明

1. 构建镜像：
   ```shhell
   docker build -f Dockerfile -t helloworld-from-dockerfile .
   ```
2. 使用 `nuctl deploy` 部署服务：
   ```shell
   nuctl deploy helloworld --run-image helloworld-from-dockerfile:latest \
       --runtime golang \
       --handler main:Handler \
       --platform local
   ```
3. 使用 `nuctl invoke` 调用服务：
   ```shell
   nuctl invoke helloworld --platform local
   23.12.26 10:16:09.655 (I)    nuctl.platform.invoker Executing function {"method": "GET", "url": "http://0.0.0.0:32769", "bodyLength": 0, "headers": {"Content-Type":["text/plain"],"X-Nuclio-Log-Level":["info"],"X-Nuclio-Target":["helloworld"]}}
   23.12.26 10:16:09.656 (I)    nuctl.platform.invoker Got response {"status": "200 OK"}
   23.12.26 10:16:09.656 (I)                     nuctl >>> Start of function logs
   23.12.26 10:16:09.656 (I)                helloworld This is an unstructured log {"time": 1703556969656.5457}
   23.12.26 10:16:09.656 (I)                     nuctl <<< End of function logs

   > Response headers:
   Content-Length = 21
   Server = nuclio
   Date = Tue, 26 Dec 2023 02:16:09 GMT
   Content-Type = application/text

   > Response body:
   Hello, from Nuclio :]
   ```
