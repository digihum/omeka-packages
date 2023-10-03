# Omeka Packages (for composer)

## Usage

Use this as a compser package repository that provides composer.json representations for omeka plugins that don't have one within the repository and therefore cannot be added to packagist. The URL is `https://digihum.github.io/omeka-packages/`. This can be added to a composer.json file.

```
            {
                        "type": "composer",
                        "url": "https://digihum.github.io/omeka-packages/"
            },
```

All packages are of type `omeka-plugin`.

Composer https://getcomposer.org/ is a dependency manager for PHP, but it's not widely used with Omeka. It's one way for us to automate the deployment of Omeka classic sites as we manage many of them with various common and custom plugin configurations.

## Coverage

This is a working project that will grow as dependencies and dependency versions are added. Contributions are welcomed. Please provide a pull request. Currently there is often only one version of a package per plugin. 

## Contributions

To make a contribution of a new plugin please follow the following process and submit a pull request:
  - Create a folder for the `[VENDOR]`.
    - The convention is to use github repo organisation for this.
    - Must be lowercase to work with Composer > 2.0.
  - Create a file for the plugin `PLUGIN-NAME.json`
    - Internal convention is to use the same as the plugin folder name.
    - Must be lowercase to work with Composer > 2.0.
  - Populate the file with package information conforming to the composer.json schema
    ```
    {
    "packages": {
        "VENDOR/PLUGIN-NAME": [
                @composer.json
                "extras": {
                            "installer-name": "FOLDER-NAME"
                }
         ]
    }
    ```
    - https://getcomposer.org/doc/04-schema.md and tutorial https://www.w3resource.com/php/composer/composer-json-schema.php
    - Add the plugin's folder name (also it's php ClassName) that Omeka expects to an 'extras' section to be used with `composer/installers` to ensure the plugin is seen as valid.
    - Add your newly created patth to `includes` in packages.json in the root folder of the repository.

To make a contribution of a new version to an existing plugin please add your complete contribution to the json array for the plugin in reverse order as below:

    ```
    {
    "packages": {
        "[VENDOR]/[PLUGIN-NAME]": [
                @composer.json (newer)
                @composer.json 
                @composer.json (older)
         ]
    }
    ```

## Using Composer with Omeka

A few tweaks are necessary to have composer working nicely with your Omeka setup. Plugin folder names are 
  - `composer require composer/installers` for the folder renaming
  - `composer require oomphinc/composer-installers-extender` or `composer require neclimdul/composer-installers-extender` if that doesn't work for you to enable the use of the `omeka-plugin` custom type for setting the custom location.
  - If your Omeka installation sits at `/var/www/html` your extra configuration will be:

Add an extras section:
```
    "extra": {
        "installer-types": ["omeka-plugin", "omeka-theme", "library"],
        "installer-paths": {
            "/var/www/html/": ["omeka/omeka"],
            "/var/www/html/plugins/{$name}/": ["type:omeka-plugin"],
            "/var/www/html/themes/{$name}/": ["type:omeka-theme"]
        }
    },
    "config": {
        "allow-plugins": {
            "composer/installers": true,
            "neclimdul/composer-installers-extender": true,
            "php-http/discovery": true,
            "cweagans/composer-patches": false
        },
    "vendor-dir": "/LOCATION/TO/PUT/OTHER/GENERAL/DEPENDENCIES"
````

## Todo
  - [ ] Look into using minified format (for only needing diffs).
  - [ ] Develop a converter to take information from plugin.ini automatically.

## License

This repository is being made freely available by the Research Software Engineering team at University of Warwick under a MIT License.
