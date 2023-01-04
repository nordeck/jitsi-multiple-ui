![Jitsi with multiple UI](docs/schema-multiple-ui.png)

# Jitsi with multiple UI

This repo contains documents/files/config to support multiple UIs on a single
`Jitsi` setup.

## Custom UI

Create a custom user interface for each user group. This UI may be a simple
`HTML` page or a dynamic page generated by a server-side service such as
`nodejs`, `PHP`, etc. No need to host these UI pages on the `Jitsi` server.

`Jitsi` will be embedded into custom user interfaces using
[Jitsi iFrame API](https://jitsi.github.io/handbook/docs/dev-guide/dev-guide-iframe).
This is the first layer of customization and mostly covers customization of
outer space, not Jitsi itself.

Check [custom-ui-a.html](templates/custom-ui/custom-ui-a.html) for a sample
page.

Click
[this link](https://nordeck.github.io/jitsi-multiple-ui/templates/custom-ui/custom-ui-a.html)
to see it in action.

## Custom Jitsi URL

![Custom Jitsi URL](docs/custom-jitsi-url.png)

We need a `code` section in `URL` to select a custom feature set and
configuration for `Jitsi`. The last two sections of `path` have a special
meaning for `Jitsi`. The last one is `room` and the other one is `tenant`
(_isolated room group_).

Therefore we need a third section in `path` to put the `code`. So we are using a
`path` with three subsections in our implementation.

## Custom Nginx config

`Nginx` redirects the user to `index.html` which is located in
`/usr/share/jitsi-meet/` by default and this index page determines most of the
runtime features of `Jitsi`. We don't want to use the same index page for all
users. Therefore we need to select different index pages for different user
groups depending on the `code` from the `URL`.

We are adding the following location block into the `Nginx` configuration to
catch the `code` from the `URL` and to select a custom index page for the
request:

```config
# nordeck: index selection
location ~ ^/([^/?&:'"]+)/([^/?&:'"]+)/(.*)$ {
    set $index "index-$1.html";
    rewrite ^/([^/?&:'"]+)/(.*)$ /$2;
}
```

And we are customizing the default `@root_path` block to apply redirection:

```config
# nordeck: customized @root_path
location @root_path {
    # rewrite ^/(.*)$ / break;
    rewrite ^/(.*)$ /$index break;
}
```

## Custom index pages

Create an additional index page in `/usr/share/jitsi-meet/` for each custom
group. The custom index page should be named as `index-CODE.hmtl`. You may copy
the original `index.html` file as a starting point.

```bash
CODE="bamberg"

cd /usr/share/jitsi-meet
cp index.html index-$CODE.html
```
