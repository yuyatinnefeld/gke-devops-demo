# local deployment
```bash
cd app_2
docker build -t hello_go_http .
docker run -p 8888:8888 -t hello_go_http
open localhost:8888
```