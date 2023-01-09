# Nginx internal flow

`Nginx` processes the request via some internal flow (`rewrite`) rules.

## Default flow

```
-> https://jitsi.mydomain.corp/f1/f2/f3/f4/myroom  ->  path = "/f1/f2/f3/f4/myroom"

-> location ~ ^/([^/?&:'"]+)/(.*)$                 ->  path = "/f2/f3/f4/myroom"
                                                       tenant = "f1/"

-> location ~ ^/([^/?&:'"]+)/(.*)$                 ->  path = "/f3/f4/myroom"
                                                       tenant = "f2/"

-> location ~ ^/([^/?&:'"]+)/(.*)$                 ->  path = "/f4/myroom"
                                                       tenant = "f3/"

-> location ~ ^/([^/?&:'"]+)/(.*)$                 ->  path = "/myroom"
                                                       tenant = "f4/"

-> location ~ ^/([^/?&:'"]+)$                      ->  path = "/myroom"
                                                       tenant = "f4/"

-> location @root_path                             ->  path = "/"
                                                       tenant = "f4/"
                                                       room="myroom"
                                                       index="index.html"
```

## Customized flow

```
-> https://jitsi.mydomain.corp/f1/f2/f3/f4/myroom  ->  path = "/f1/f2/f3/f4/myroom"

-> location ~ ^/([^/?&:'"]+)/(.*)$                 ->  path = "/f2/f3/f4/myroom"
                                                       tenant = "f1/"

-> location ~ ^/([^/?&:'"]+)/(.*)$                 ->  path = "/f3/f4/myroom"
                                                       tenant = "f2/"

-> location ~ ^/([^/?&:'"]+)/([^/?&:'"]+)/(.*)$    ->  path = "/f4/myroom"
                                                       tenant = "f2/"
                                                       index = "index-f3.html"

-> location ~ ^/([^/?&:'"]+)/(.*)$                 ->  path = "/myroom"
                                                       tenant = "f4/"
                                                       index = "index-f3.html"

-> location ~ ^/([^/?&:'"]+)$                      ->  path = "/myroom"
                                                       tenant = "f4/"
                                                       index = "index-f3.html"

-> location @root_path                             ->  path = "/"
                                                       tenant = "f4/"
                                                       room="myroom"
                                                       index = "index-f3.html"
```
