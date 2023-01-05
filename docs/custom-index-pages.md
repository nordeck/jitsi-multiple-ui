# Custom index pages

The default index page is `index.html` which is located in
`/usr/share/jitsi-meet`. The index page sets most of the runtime features by
including some config files (such as `config.js`, `interface_config.js`, etc.)
into itself.

See [index.html](https://github.com/jitsi/jitsi-meet/blob/master/index.html) for
the original copy.

## Creating

Create your custom index pages by cloning this default page and customize them
according to needs.

```bash
cd /usr/share/jitsi-meet

CODE="moderator"
cp index.html index-$CODE.html

CODE="guest"
cp index.html index-$CODE.html
```

## Customizing

Update the custom index pages using an editor. Some lines which might be
meaningful to update:

```html
<!--#include virtual="mycustom-head.html" -->
...
<!--#include virtual="mycustom-base.html" -->
...
<link rel="stylesheet" href="css/mycustom.css?v=6776">
...
var criticalFiles = [
  "mycustom-config.js",
  ...
  "mycustom_interface_config.js",
  ...
];
...
<script><!--#include virtual="/mycustom-config.js" --></script>
...
<script><!--#include virtual="/mycustom_interface_config.js" --></script>
...
<!--#include virtual="mycustom-title.html" -->
...
<!--#include virtual="mycustom-body.html" -->
...
```

Most critical one is `config.js` which sets most of the runtime features.
