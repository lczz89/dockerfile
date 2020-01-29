```
  ALPINE_VERSION:
    - 3.7
    - 3.8
    - 3.9
```
docker build --build-arg ALPINE_VERSION=3.9 -t apline3.9 .