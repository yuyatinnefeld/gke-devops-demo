# local deployment
```bash
cd app_1
docker build -t fastapi-img .
docker run -d --name fastapi -p 80:80 fastapi-img
open 0.0.0.0:80
```
