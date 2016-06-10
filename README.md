Charcoal Admin Cookbook
=======================

This guide is a loose collection of tips and tricks to configure, manage and customize a Charcoal admin installation (aka. the _backend_ or _control panel_).

> 👉For the _standard_ doc, view [`locomotivemtl/charcoal-admin` on github](https://github.com/locomotivemtl/charcoal-admin).

# Introduction

- [About this guide](#about-this-guide)
- [How to configure charcoal-admin](#how-to-configure-charcoal-admin)

[☟ Skip to **recipes**](#recipes)

## About this guide

Some assumptions are made in this guide:

- When a ★ shell command is evoqued, it is assumed to be ran from the project's root directory.
- The project's root directory is outsite of the web root.
	+ The web root is in the `www/` folder. 
	+ A standard charcoal _front controller_ should exist as `www/index.php`.
- A working database is properly configured.
- A default charcoal configuration is setup and working.
	+ The configuration should all be called from `config/config.php`.
	+ Typically, admin configuration itself is in `config/admin.json`.
- Admin routes are setup and reachable.

## How to configure charcoal-admin

The first thing to do is to make sure that `charcoal-admin` has been properly installed and configured:

Install it in your project, along all required dependencies.

```shell
★ composer require locomotivemtl/charcoal-admin
```

> 👉 A project can be created easily with `composer create-project` and the [charcoal project boilerplate](https://github.com/locomotivemtl/charcoal-project-boilerplate).

If you don't have an admin user yet, create one from the command line. This requires a properly setup database in your charcoal project:

```shell
★ ./vendor/bin/charcoal admin/user/create
```

> 👉 More info in the "[Adding an administrator from the command line](#adding-an-administrator-from-the-command-line)" recipe.

The charcoal control panel (backend) should be available at `http://{{URL}}/admin/login`.

To customize charcoal-admin, all is done in configuration file (and object metadata).

It is recommended to put all admin-related configuration options in a single file, `config/admin.json` and load it from `config/config.php`:

```php
$this->addFile('admin.json');
```

--

# Recipes


Required customizations:

- [Customizing the main menu](#customizing-the-main-menu)
- [Setting a custom home page](#setting-a-custom-home-page)

Object customizations

- ~~Setting a custom object title~~
- ~~Setting an image maximum width or height~~
- ~~Custom object dashboards~~
- ~~Disallowing object delete~~
- ~~Setting a custom order on an object list~~
- [Allowing sortable on an object list](#allowing-sortable-on-an-object-list)

Theme customizations:

- [Setting a custom logo](#setting-a-custom-logo)
- [Setting a background in the login section](#setting-a-background-in-the-login-section)

Scripts:

- [Adding an administrator from the command line](#adding-an-administrator-from-the-command-line)
- ~~Creating a database table from metadata~~
- ~~Altering a database table from metadata~~

<small>Note: ~~striked items~~ are just ideas; not yet written.</small>

## Customizing the main menu

The main menu, also called _header menu_, even though its is actually on the side, is a "simple" menu configuration.

> UI Items are defined in the `charcoal-ui` module. The `Menu` (`\Charcoal\Ui\Menu\MenuInterface`) is a simple collection of `MenuItem` `items`.
> 
> Menu items are `\Charcoal\Ui\MenuItem\MenuItemInterface`
> 
> 👉 Menu documentation is available on `locomotivemtl/charcaoal-ui` on github.

Edit the menu items in the `admin.json` file. For example:

```json
{
	"admin": {
		"header_menu": {
			"items": {
				"
			}
		}
	}
}
```

[☝ Recipes menu](#recipes)

## Setting a custom home page

Setting a custom homepage is a matter of _routing_.

> Charcoal-admin is based on charcoal-app, which itself is based on Slim.
>
>Slim is, at its core, a powerful routeur, provided by FastRoute..

charcoal-app makes routing easy through a configuration file. The admin routes are defined within the admin config so can be set in the `admin.json` file:

```json
{
	"admin": {
		"routes": {
			"templates": {
				"home": {
					"redirect": "object/collection?obj_type=foo/bar"
				}
			}
		}
	}
}
```

[☝ Recipes menu](#recipes)

## Allowing sortable on an object list

By default, it is not possible to reorder objects directly from the object table widget.

It is, however, really easy to enable this feature from the object's admin metadata. This is done by setting `"sortable":true` on the `charcoal/admin/widget` options:

```json
{
	"admin":{
		...
		"dashboards":{
			...
			"table":{
				"widgets": {
					"table": {
						"type": "charcoal/admin/widget/table",
						"collection_ident": "default",
						"obj_type": "foo/object/bar",
						"sortable":true
					}
				}
			}
		}
		"default_collection_dashboard":"table"
		...
	}
}

```

> Note that the object (in this example, `foo/object/bar`) must have a `position` property.

> Also note that the frontend should probably order by `position` for this feature to make any sense.

[☝ Recipes menu](#recipes)

## Setting a background in the login section

There are 2 different background types that can be set: _image_ or _video_. They are configured exactly the same way:

To set a background image, edit the `admin.json` file:

```json
{
	"admin": {
		"login": {
			"background_image": "https://cdn.example.com/image.png"
		}
	}
}
```

To set a background video (overkill but nice):

```json
{
	"admin": {
		"login": {
			"background_video": "https://cdn.example.com/video.mp4"
		}
	}
}
```

> 👉 Make sure to use `https` resource for the background media; otherwise browsers will throw a SSL warning / error.

[☝ Recipes menu](#recipes)

## Adding an administrator from the command line

The charcoal-admin module has a dependency on `charcoal-app`, which provides charcoal's scripting tools.

The admin module provides a few script that can be ran from the command line. To add an administrator user:

```shell
★ ./vendor/bin/charcoal admin/user/create
```

And follow the instructions:

- Enter your login name
- Enter your email (a confirmation email should be sent)
- Enter your password

[☝ Recipes menu](#recipes)

--

# Conclusion

How to contribute to this guide:

- Validate that the information in this guide is true, and current (updated). 
- Write unit tests to validate against assumptions made in this document. 
- Take screenshots and add them to relevant section(s). 
- Write recipes. This document is on github.
