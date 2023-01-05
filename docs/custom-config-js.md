# Custom config.js

`config.js` sets most of the runtime features. It is located in
`/etc/jitsi/meet/` folder and there is only one for each `Jitsi` setup by
default.

See [config.js](https://github.com/jitsi/jitsi-meet/blob/master/config.js) for
the default template.

## Creating

Create your custom `config.js` files by cloning this default one and customize
them according to needs.

```bash
cd /etc/jitsi/meet

PREFIX="moderator"
cp YOUR-DOMAIN-config.js $PREFIX-config.js

PREFIX="guest"
cp YOUR-DOMAIN-config.js $PREFIX-config.js
```

## Customizing

Update the custom `config.js` files using an editor. Some parameters which might
be meaningful to update:

- `disableModeratorIndicator: true`
- `disableReactions: true`
- `disablePolls: true`
- `requireDisplayName: true`
- `disableShortcuts: true`
- `prejoinConfig`

  ```json
  prejoinConfig: {
      enabled: true,
      hideDisplayName: false,
      hideExtraJoinButtons: ['no-audio', 'by-phone'],
  },
  ```

- `toolbarButtons`

  ```json
  toolbarButtons: [
     'camera',
     'chat',
     'desktop',
     'filmstrip',
     'hangup',
     'highlight',
     'microphone',
     'participants-pane',
     'raisehand',
     'tileview',
     'toggle-camera',
  ],
  ```

- `disableInviteFunctions: true`
- `breakoutRooms`

  ```json
  breakoutRooms: {
      hideAddRoomButton: true,
      hideAutoAssignButton: true,
      hideJoinRoomButton: true,
  },
  ```

- `hideConferenceTimer: true`
- `hideParticipantsStats: true`
