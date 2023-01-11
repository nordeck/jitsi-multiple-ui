# Nginx internal flow

`Nginx` processes the request via some internal flow (`rewrite`) rules.

## Default flow

```
-> https://jitsi.mydomain.corp/f0/f1/moderator/kiel/myroom   ->  path   = /f0/f1/moderator/kiel/myroom

-> location ~ ^/([^/?&:'"]+)/(.*)$                           ->  path   = /f1/moderator/kiel/myroom
                                                                 tenant = f0/

-> location ~ ^/([^/?&:'"]+)/(.*)$                           ->  path   = /moderator/kiel/myroom
                                                                 tenant = f1/

-> location ~ ^/([^/?&:'"]+)/(.*)$                           ->  path   = /kiel/myroom
                                                                 tenant = moderator/

-> location ~ ^/([^/?&:'"]+)/(.*)$                           ->  path   = /myroom
                                                                 tenant = kiel/

-> location ~ ^/([^/?&:'"]+)$                                ->  path   = /myroom
                                                                 tenant = kiel/

-> location @root_path                                       ->  path   = /
                                                                 tenant = kiel/
                                                                 room   = myroom
                                                                 index  = index.html
```

## Customized flow

```
-> https://jitsi.mydomain.corp/f0/f1/moderator/kiel/myroom   ->  path   = /f0/f1/moderator/kiel/myroom

-> location ~ ^/([^/?&:'"]+)/(.*)$                           ->  path   = /f1/moderator/kiel/myroom
                                                                 tenant = f0/

-> location ~ ^/([^/?&:'"]+)/(.*)$                           ->  path   = /moderator/kiel/myroom
                                                                 tenant = f1/

-> location ~ ^/([^/?&:'"]+)/([^/?&:'"]+)/(.*)$              ->  path   = /kiel/myroom
                                                                 tenant = f1/
                                                                 index  = index-moderator.html

-> location ~ ^/([^/?&:'"]+)/(.*)$                           ->  path   = /myroom
                                                                 tenant = kiel/
                                                                 index  = index-moderator.html

-> location ~ ^/([^/?&:'"]+)$                                ->  path   = /myroom
                                                                 tenant = kiel/
                                                                 index  = index-moderator.html

-> location @root_path                                       ->  path   = /
                                                                 tenant = kiel/
                                                                 room   = myroom
                                                                 index  = index-moderator.html
```

## Examples

```
-> https://jitsi.mydomain.corp/moderator/bamberg/AZ2832      -> index  = index-moderator.html
                                                                tenant = bamberg
                                                                room   = AZ2832

-> https://jitsi.mydomain.corp/guest/bamberg/AZ2832          -> index  = index-guest.html
                                                                tenant = bamberg
                                                                room   = AZ2832

-> https://jitsi.mydomain.corp/guest/kiel/AZ2832             -> index  = index-guest.html
                                                                tenant = kiel
                                                                room   = AZ2832
```
