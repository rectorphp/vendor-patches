# Vendor Patches

Shared patches for Rector packages, for easier re-use.

This meta package makes use of [cweagans/composer-patches](https://github.com/cweagans/composer-patches).

<br>

## How to Use

The composer patches packages does not work as intuitively as you may expect. Yet, there is a combination, that clicks :)

<br>

### ~~1. External File~~

The [external patch file](https://github.com/cweagans/composer-patches#using-an-external-patch-file) works only for main `rector/rector-src` repository. It cannot be used e.g. for dev packages that depend on `rector/rector-src

:red_circle:

### 2. Define Patch Paths in `rector/rector-src`

The patches must be defined in [`rector/rector-src` in `composer.json`](https://github.com/rectorphp/rector-src/blob/0dd833b1e29ba665bbb3acad85a6359f701f2e18/composer.json#L154-L164):

```json
{
    "extra": {
        "patches": {
            "package-name": [
                "https://raw.github.com/.../patches/some.patch"
            ]
        }
    }
}
```

* The path to patch **file must be absolute**. Relative path fails when using `rector/rector-src` as dependency, as the `composer.json` is nested there

### 3. Add Patch File Here

The patch must be added to this repository in `/patches` directory.

### 4. Allow Patches in Dependent Package

The dependency package (e.g. [rector/rector-symfony](https://github.com/rectorphp/rector-symfony)) must allow to install patches from `rector/rector-src` in the package `composer.json`:

```json
{
    "require-dev": {
        "symplify/vendor-patches": "^10.0"
    },
    "extra": {
        "enable-patching": true
    }
}
```

It also must require `symplify/vendor-patches package`, to invoke the plugin on `composer install`.

<br>

This is the setup that works :heavy_check_mark:
