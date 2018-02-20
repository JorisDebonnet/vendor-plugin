## SilverStripe Vendor Plugin

When installing SilverStripe modules in the vendor directory it may also be necessary
to ensure certain module assets are exposed to the webroot, as the 'vendor' url prefix
is blocked from web-access by default.

## Example

For example, given the below module composer.json:

```json
{
    "name": "tractorcow/anothermodule",
    "description": "a test module",
    "type": "silverstripe-vendormodule",
    "extra": {
        "expose": [
            "client"
        ]
    },
    "require": {
        "silverstripe/vendor-plugin": "^1.0",
        "silverstripe/framework": "^4.0"
    }
}
```

This will be installed into the `vendor/tractorcow/anothermodule` folder, and a
symlink will be created in `resources/tractorcow/anothermodule/client`, allowing
the web server to serve resources from the vendor folder without exposing any
code or other internal server files.

Note: The module type `silverstripe-vendormodule` is mandatory, as this behaviour
is not enabled for libraries of other types.

## Customising behaviour

By default the plugin will attempt to perform a symlink, failing back to a full
filesystem copy.

If necessary, you can force the behaviour to one of the below using the
`SS_VENDOR_METHOD` environment variable (set in your system environment prior to install):

  - `none` - Disables all symlink / copy
  - `copy` - Performs a copy only
  - `symlink` - Performs a symlink only
  - `junction` - Uses a junction (windows only)
  - `auto` - Try symlink (and junction on windows), but fail over to copy.

Note: to use `symlink` on Windows, run composer commands with Administrator privileges.

## Updating all exposed folders

A custom composer command can be run at any time to update / refresh all 

`composer vendor-expose [<method>]`

You can pass in one of the above methods to force a specific behaviour, otherwise
the default will be chosen based on either a previously used method,
or the `SS_VENDOR_METHOD` environment argument. 
