# Setting up a Nuxt PWA

This guide although includes configuration for Nuxt is more of a deep dive into the nuts and bolts of PWA's. However this section covers the basic structure of a Nuxt PWA.

Nuxt has a first-party PWA module that can be either added as the project is created or after the fact via npm/yarn:

#### [Link to setup page](https://pwa.nuxtjs.org/setup)

```
npm i --save-dev @nuxtjs/pwa
```

Add to buildModules within `nuxt.config.js`:

```js
buildModules: ["@nuxtjs/pwa"];
```

Next make sure there is a `sw.js` file within the `static` folder and ensure it is also added to the `.gitignore` file.

Lastly add home screen icon to be used to the `static` folder. By default the `pwa.icon` module automatically generates all sizes and adds the path to the compiled manifest within the `dist` folder.

Now we are ready to start configuring PWA provisions. Below is the basic structure, we deep dive into each area in specific sections:

1. Manifest

The `manifest` is located within `nuxt.config.js` and is not structured in the same way it would be when in JSON form in non-nuxt projects. Note the icon paths do not need to be added here as this is taken care of for us as long as there is a source icon in the static folder:

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

A special plugin is created to register endpoints where background sync requests are made.

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

Disregarding all workbox configuration in above examples the provisions in place so far should create a standalone pwa with a manifest that is able to be saved to the home screen with the home screen icon.
