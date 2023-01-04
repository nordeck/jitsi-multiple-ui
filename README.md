![Jitsi with multiple UI](docs/schema-multiple-ui.png)

# Jitsi with multiple UI

- [1. About](#1-about)
- [2. Custom UI](#2-custom-ui)
- [3. Custom Jitsi URL](#3-custom-jitsi-ui)
- [4. Nginx customization (custom index pages)](#4-nginx-customization-custom-index-pages)

## 1. About

This repo contains documents/files/config to support multiple UIs on a single
`Jitsi` setup.

## 2. Custom UI

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

## 3. Custom Jitsi URL

![Custom Jitsi URL](docs/custom-jitsi-url.png)

We need a `code` section in `URL` to select a custom feature set and
configuration for `Jitsi`. The last two sections of `path` have a special
meaning for `Jitsi`. The last one is `room` and the other one is `tenant`
(_isolated room group_).

Therefore we need a third section in `path` to put the `code`. So we are using a
`path` with three subsections in our implementation.

## 4. Nginx customization (custom index pages)

`Nginx` redirects users to `index.html` which is located in
`/usr/share/jitsi-meet/` by default and this index page determines most of the
runtime features of `Jitsi`. We don't want to use the same index page for all
users. Therefore we will select different index pages for different user groups
depending on the `code` from the `URL`.

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

## 5. Custom index pages

Create an additional index page in `/usr/share/jitsi-meet/` for each custom
group. The custom index page should be named as `index-CODE.hmtl`. You may copy
the original `index.html` file as a starting point.

```bash
CODE="bamberg"

cd /usr/share/jitsi-meet
cp index.html index-$CODE.html
```

See [index.html](https://github.com/jitsi/jitsi-meet/blob/master/index.html) for
the original copy.

The index page determines most of the runtime features by including some config
files (_such as `config.js`, `interface_config.js`, etc._) into itself.
Customize the index pages according to the needs. For example set custom config
files in it.

## 6. Nginx customization (custom config.js)

`config.js` sets most of the runtime features and it is located in
`/etc/jitsi/meet/` folder. `Nginx` uses a hard-coded `alias` to point it. Since
there will be multiple `config.js` in our implementation, we need to update
`Nginx` config to point the right location for the additional `config.js` files.

Add the following location block into the `Nginx` configuration:

```config
# nordeck: custom config files
location ~ /([a-zA-Z0-9-]+)-config.js {
    alias /etc/jitsi/meet/$1-config.js;
}
```

## 7. Custom config.js

Create an additional `config.js` file in `/etc/jitsi/meet/` for each feature
set. The custom `config.js` file should be named as `PREFIX-config.js`. `PREFIX`
must be a combination of letters and digits and `-`. The file name must match
the one used in the index page. You may copy the original `config.js` file as a
starting point.

```bash
PREFIX="bamberg-moderator"

cd /etc/jitsi/meet
cp YOUR-DOMAIN-config.js $PREFIX-config.js
```

See [config.js](https://github.com/jitsi/jitsi-meet/blob/master/config.js) for
the default template.
