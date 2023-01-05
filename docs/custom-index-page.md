# Custom index page

The default index page is `index.html` which is located in
`/usr/share/jitsi-meet`. The index page determines most of the runtime features
by including some config files (such as `config.js`, `interface_config.js`,
etc.) into itself.

## Creating

Create your custom index pages by cloning this default page and customize them
according to needs.

```bash
CODE="bamberg"

cd /usr/share/jitsi-meet
cp index.html index-$CODE.html
```
