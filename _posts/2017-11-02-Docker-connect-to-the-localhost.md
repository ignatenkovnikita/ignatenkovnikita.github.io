---
layout: post
title: Docker container connect to the localhost DB!
---

Sometimes need connect from docker container to the localhost, example db and other service

Docker support many network modes

But we need simple solutions
This solution is extra_hosts in docker-compose.yml

```yaml
app:
  build: docker/php
  working_dir: /app
  volumes_from:
    - data
  expose:
    - 9000
  links:
    - db
  environment:
    XDEBUG_CONFIG: "idekey=PHPSTORM remote_enable=On remote_connect_back=On"
  extra_hosts:
    - "dockerhost:172.17.0.1"
```

And use host dockerhost in application connect to db mysql
```bash
mysql -u root -p -h dockerhost
``` 

Find ip dockerhost
```bash
ifconfig | grep -E "([0-9]{1,3}\.){3}[0-9]{1,3}" | grep -v 127.0.0.1 | awk '{ print $2 }' | cut -f2 -d: | head -n1
```

