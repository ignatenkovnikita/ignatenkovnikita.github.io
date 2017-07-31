---
layout: post
title: Yii2 route suffix .php nginx template!
---

On my problem have url route with suffix .php

I fix this problem nex configuration

```bash
location ^~ /exchange/ {
        try_files $uri $uri/ /index.php?$args;

}
```




