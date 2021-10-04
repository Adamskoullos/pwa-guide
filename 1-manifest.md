# Application Manifest

The `manifest.json` file is the configuration settings that tell the browser how to treat and present the application.

Below is an example using some of the core properties that can be set. The icons array needs to have icons of specific sizes to suit devices and all must be square:

- 48px
- 72px
- 96px
- 128px
- 144px
- 152px
- 192px
- 384px
- 512px

```json
{
  "name": "Main APP Name",
  "short_name": "Used with Icon",
  "start_url": "path that opens when user clicks the icon",
  "display": "standalone",
  "background_color": "#123456",
  "theme_color": "#123456",
  //"orientation": "", If the app should be opened in either portrait or landscape, leave out if universal
  "scope": ".", // Include specific pages, the dot includes the whole app
  "description": "information for the browser and user about the application",
  "lang": "en",
  "icons": [
    {
      "src": "/icons/icon-48x48.png",
      "type": "image/png",
      "sizes": "48x48"
    },
    {
      "src": "/icons/icon-72x72.png",
      "type": "image/png",
      "sizes": "72x72"
    },
    {
      "src": "/icons/icon-96x96.png",
      "type": "image/png",
      "sizes": "96x96"
    },
    {
      "src": "/icons/icon-128x128.png",
      "type": "image/png",
      "sizes": "128x128"
    },
    {
      "src": "/icons/icon-144x144.png",
      "type": "image/png",
      "sizes": "144x144"
    },
    {
      "src": "/icons/icon-152x152.png",
      "type": "image/png",
      "sizes": "152x152"
    },
    {
      "src": "/icons/icon-192x192.png",
      "type": "image/png",
      "sizes": "192x192"
    },
    {
      "src": "/icons/icon-384x384.png",
      "type": "image/png",
      "sizes": "384x384"
    },
    {
      "src": "/icons/icon-512x512.png",
      "type": "image/png",
      "sizes": "512x512"
    }
  ]
}
```

For applications that do not use a front-end framework with configuration files a link to the `manifest.json` file needs to be added to each html page within the head. The file itself is placed at the root of the public application:

```html
<link rel="manifest" href="/manifest.json" />
```

**Nuxt**:

Nuxt houses the manifest within the `nuxt.config.js` file and auto adds settings to each page.

Once set up the manifest can be seen within the dev tools via the `application` tab within `Manifest`.
