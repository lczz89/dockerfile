```
  ALPINE_VERSION:
    - 3.7
    - 3.9
```
docker build --build-arg ALPINE_VERSION=3.7 -t apline3.7 .