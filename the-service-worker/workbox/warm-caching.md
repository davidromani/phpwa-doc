# Warm Caching

Workbox provides various caching strategies to make it easy to manage how requests are handled by the service worker. One of the features is the ability to maintain a _warm cache_ for certain URLs.

A warm cache refers to pre-caching URLs during the service worker installation phase. This ensures that those URLs are cached and readily available even before they are requested by the user.

The URLs to be pre-cached are likely the pages that users will definitely navigate to. For example, the main functionality of your application, the pricing page or a page with the latest news that users will definitely read.

When the service worker is installed, Workbox will automatically cache the pages specified in the list. These files will remain cached and will be instantly available to the user, contributing to a swift user experience.

<mark style="color:green;">**This feature is enabled by default!**</mark> All assets (images, stylesheets or scripts) managed by Asset Mapper are cached.

Hereafter the main benefits of Precache:

* **Faster Load Times**: Since assets are stored locally, web applications can load faster, providing a better user experience.
* **Offline Support**: Precached assets ensure that the application is usable even without a network connection.
* **Consistency**: All users receive the same version of files, ensuring a consistent experience.
* **Background Updates**: Assets are updated in the background, preventing any interruption to the user experience.

### Page Caching

Developers can build more resilient and user-friendly web applications that perform reliably under various network conditions. Also, it is possible to warm cache a selection of pages. This is powerfull as it allows applications to partially work offline.

The strategy applied for page is Network First i.e. the page from the web server is fetched first. In case of failure, the cached data is served. By default, the service worker will wait 3 seconds before serving the cached version. This value can be configured using the `network_timeout_seconds` option.

{% code title="/config/packages/pwa.yaml" lineNumbers="true" %}
```yaml
pwa:
    serviceworker:
        enabled: true
        src: "sw.js"
        workbox:
            network_timeout_seconds: 2 # Wait only 2 seconds
            warm_cache_urls:
                - 'app_homepage' # Simple route name
                - path: 'app_feature1' # Simple route name without parameters
                - path: 'app_feature2' # Route name with parameters
                  params:
                      foo: 'bar'
                      param1: 'value1'
```
{% endcode %}

{% hint style="info" %}
Please note that you can refer to any URLs, but only URLs served by your application will be cached.
{% endhint %}

### Image Caching

By default, a maximum of 60 images are cached for 1 year. The supported image extensions extensions are as follows: `ico`, `png`, `jpeg`/`jpg`, `gif`, `svg`, `webp` and `bmp`. This can be changed using the next configuration options

<pre class="language-yaml" data-title="/config/packages/pwa.yaml" data-line-numbers><code class="lang-yaml">pwa:
<strong>    serviceworker:
</strong>        enabled: true
        src: "sw.js"
        workbox:
            max_image_age: 60 * 60 * 24 * 30 # 30 days
            max_image_cache_entries: 200
            image_regex: '/\.(png|jpe?g|svg|webp)$/'
</code></pre>

### Asset caching

Similary to images, e.g. CSS, JSON, XML or JS files are cached. The supported image extensions extensions are as follows: `css`, `js`, `json`, `xml`, `txt`, `map`, `webmanifest`.

This can be changed using the next configuration options:

{% code title="/config/packages/pwa.yaml" lineNumbers="true" %}
```yaml
pwa:
    serviceworker:
        enabled: true
        src: "sw.js"
        workbox:
            static_regex: '/\.(css|jsx?)$/'
```
{% endcode %}

### Font Caching

By default, 30 fonts can be cached for 1 year. This can be changed using the next configuration options:

{% code title="/config/packages/pwa.yaml" lineNumbers="true" %}
```yaml
pwa:
    serviceworker:
        enabled: true
        src: "sw.js"
        workbox:
            max_font_cache_entries: 10
            max_font_age: 60 * 60 * 24 * 30 # 30 days
```
{% endcode %}
