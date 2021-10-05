# Setting up a Nuxt PWA

This guide although includes configuration for Nuxt is more of a deep dive into the nuts and bolts of PWA's. However this section covers the basic structure of a Nuxt PWA.

1. Manifest

The `manifest` is located within `nuxt.config.js`:

```js

pwa: {
    manifest: {
        name: "Main APP Name",
        short_name: "Used with Icon",
        start_url: "path that opens when user clicks the icon",
        display: "standalone",
        background_color: "#123456",
        theme_color: "#123456",
        scope: ".", // Include specific pages, the dot includes the whole app
        description: "information for the browser and user about the application",
        lang: "en",
        icons: [
            {
            src: "/icons/icon-48x48.png",
            type: "image/png",
            sizes: "48x48"
            },
            // Other icons omitted
        ]
    },
    workbox: {
      runtimeCaching: [
          {
            urlPattern: "https://dev-test-api-one.herokuapp.com/todos",
            handler: "cacheFirst",
            method: "GET",
            strategyOptions: { cacheableResponse: { statuses: [0, 200] } }
          },
          {
            urlPattern: "https://dev-test-api-two.herokuapp.com/todos",
            handler: "cacheFirst",
            method: "GET",
            strategyOptions: { cacheableResponse: { statuses: [0, 200] } }
          }
      ],
      cachingExtensions: "@/plugins/workbox-sync.js",
    }
  },
```

2. Background sync plugin

A special plugin is created to register endpoints where backsync requests are made.

**Note**: This plugin is not added to plugins within `nuxt.config.js` as we do not want the code to be injected.

```js
const bgSyncPlugin = new workbox.backgroundSync.BackgroundSyncPlugin(
  "formQueue",
  {
    maxRetentionTime: 24 * 60, // Retry for max of 24 Hours (specified in minutes)
  }
);

workbox.routing.registerRoute(
  /https:\/\/dev\-test\-api\-one\.herokuapp\.com\/todos/,
  // /https:\/\/jsonplaceholder\.typicode\.com\/posts/,
  new workbox.strategies.NetworkOnly({
    plugins: [bgSyncPlugin],
  }),
  "POST"
);

workbox.routing.registerRoute(
  /https:\/\/dev\-test\-api\-two\.herokuapp\.com\/todos/,
  new workbox.strategies.NetworkOnly({
    plugins: [bgSyncPlugin],
  }),
  "POST"
);
```

3. sw.js

Nuxt automatically creates a `sw.js` file within the static folder, we do not need to touch this file. When the project is built/generated for production Nuxt creates the public `dist` folder. As this happens Nuxt creates all service workers required and adds them to the `sw.js` file. This new file for production is located at the root of the `dist` folder.
