# Custom interface_config.js

`interface_config.js` sets some of the runtime features for web clients. It is
located in `/etc/jitsi/meet/` folder and there is only one for each `Jitsi`
setup by default.

See
[interface_config.js](https://github.com/jitsi/jitsi-meet/blob/master/interface_config.js)
for the default template.

## Creating

Create your custom `interface_config.js` files by cloning this default one and
customize them according to needs.

```bash
cd /etc/jitsi/meet

PREFIX="moderator"
cp interface_config.js ${PREFIX}_interface_config.js

PREFIX="guest"
cp interface_config.js ${PREFIX}_interface_config.js
```

## Customizing

Update the custom `interface_config.js` files using an editor. Some parameters
which might be meaningful to update:

- `JITSI_WATERMARK_LINK: ''`
