# IOS Support

Apple do not support the PWA icons or theme color out of the box but we can fix this:

1. Add a `link` tag in the head to link to the app icon to be used on mobile and tablet. Multiple link tags can be used here:

```html
<link rel="apple-touch-icon" href="/icons/icon-96x96.png" />
```

2. Add a `meta` tag to the head for the theme colored banner:

```html
<meta name="apple-mobile-web-app-status-bar" content="#123456" />
```
