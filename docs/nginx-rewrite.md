```
-> https://jitsi.mydomain.corp/f1/f2/f3/f4/myroom  ->  path = "/f1/f2/f3/f4/myroom"
                                                       subdomain = ""

-> location ~ ^/([^/?&:'"]+)/(.*)$                 ->  path = "/f2/f3/f4/myroom"
                                                       subdomain = "f1/"
```

# vim: tw=140
