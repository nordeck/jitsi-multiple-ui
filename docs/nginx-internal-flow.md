# Nginx internal flow

`Nginx` processes the request via some internal flow (`rewrite`) rules.

## Default flow

```
-> https://jitsi.mydomain.corp/f4/f3/f2/f1/myroom  ->  path = "/f4/f3/f2/f1/myroom"

-> location ~ ^/([^/?&:'"]+)/(.*)$                 ->  path = "/f3/f2/f1/myroom"
                                                       tenant = "f4/"

-> location ~ ^/([^/?&:'"]+)/(.*)$                 ->  path = "/f2/f1/myroom"
                                                       tenant = "f3/"

-> location ~ ^/([^/?&:'"]+)/(.*)$                 ->  path = "/f1/myroom"
                                                       tenant = "f2/"

-> location ~ ^/([^/?&:'"]+)/(.*)$                 ->  path = "/myroom"
                                                       tenant = "f1/"

-> location ~ ^/([^/?&:'"]+)$                      ->  path = "/myroom"
                                                       tenant = "f1/"

-> location @root_path                             ->  path = "/"
                                                       tenant = "f1/"
                                                       room="myroom"
                                                       index="index.html"
```

## Customized flow

```
-> https://jitsi.mydomain.corp/f4/f3/f2/f1/myroom  ->  path = "/f4/f3/f2/f1/myroom"

-> location ~ ^/([^/?&:'"]+)/(.*)$                 ->  path = "/f3/f2/f1/myroom"
                                                       tenant = "f4/"

-> location ~ ^/([^/?&:'"]+)/(.*)$                 ->  path = "/f2/f1/myroom"
                                                       tenant = "f3/"

-> location ~ ^/([^/?&:'"]+)/([^/?&:'"]+)/(.*)$    ->  path = "/f1/myroom"
                                                       tenant = "f3/"
                                                       index = "index-f2.html"

-> location ~ ^/([^/?&:'"]+)/(.*)$                 ->  path = "/myroom"
                                                       tenant = "f1/"
                                                       index = "index-f2.html"

-> location ~ ^/([^/?&:'"]+)$                      ->  path = "/myroom"
                                                       tenant = "f1/"
                                                       index = "index-f2.html"

-> location @root_path                             ->  path = "/"
                                                       tenant = "f1/"
                                                       room="myroom"
                                                       index = "index-f2.html"
```
