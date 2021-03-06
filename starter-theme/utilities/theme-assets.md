# Theme\Assets

This class (it's actually a facade) provides tools to work with assets.

## Theme\Assets::getThemeUri()

Return the public URI of the theme root directory.
This is useful since `get_template_directory_uri()` will return the URI to the `theme/` directory inside your theme (instead of the root directory) since this is where the theme's `style.css` file is.

## Theme\Assets::getAssetUri( $asset )

Get the real relative path to an asset that was built using the Webpack pipeline.

Assets such as images and fonts which pass through the build pipeline have their content hashed and appended to their filenames to facilitate cache busting. Since filenames change depending on the contents, a `dist/manifest.json` file is **automatically** generated to act as a dictionary between the source filename and the final filename.

### Example

Having this `dist/manifest.json`:
```json
{
  // ...
  "images/favicon.ico": "images/favicon.3bb5e12219.ico",
  // ...
}
```
... calling:
```php
Theme\Assets::getAssetUri( 'images/favicon.ico' )
```
... will return:
```
images/favicon.3bb5e12219.ico
```

You can see this exact scenario used for the `Theme\Assets::addFavicon()` method here:

https://github.com/htmlburger/wpemerge-theme-core/blob/master/src/Assets/Assets.php#L142

## Theme\Assets::enqueueStyle( $handle, $src, $dependencies = [], $media = 'all' )

Enqueue a style like `wp_enqueue_style()` does but provide an automatic version number derived from the file's modified time.

Essentially provide a built-in, automatic and fool-proof cache buster for the enqueued style.

## Theme\Assets::enqueueScript( $handle, $src, $dependencies = [], $in_footer = false )

Enqueue a script like `wp_enqueue_script()` does but provide an automatic version number derived from the file's modified time.

Essentially provide a built-in, automatic and fool-proof cache buster for the enqueued script.

## Theme\Assets::addFavicon()

Output a &lt;link /&gt; tag for the site favicon pointing to `dist/images/favicon.ico` which is generated by default from `resources/images/favicon.ico`.

Will not output anything if you have already added a favicon through WordPress' Customizer so it does not override it.
