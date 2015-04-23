#WP Local Toolbox

Through constants defined in wp-config, you can disable plugins, disable the  loading of external files, set search engine visibility, display the server name and change the color of the admin bar, or literally anything else you can think of.

This is an invaluable tool if you often work in production, staging, and local servers at the same time. 

WP Local Toolbox uses the following constants defined in wp-config.php:

* **WPLT_SERVER**: The name of your server environment. It will be displayed in the admin bar at browser widths greater than 1030px. If left undefined, the plugin will make no changes to the admin bar. 

	If not defined as 'PRODUCTION' or 'LIVE', the plugin will enable 'Discourage search engines from indexing this site' to prevent your development and staging servers from being indexed. This option is not stored in the database, so your production server will still look to the actual setting on the Reading page.

* **WPLT_COLOR**: Determines the color of the admin bar. You can set this to any CSS color. If left undefined, will use the following defaults: 
	
	* Production / Live: red
	* Staging / Testing: orange
	* Local / Dev: green

* **WPLT_ADMINBAR**: Boolean to show or hide the admin bar on the frontend. `TRUE` will force the adminbar to display, `FALSE` will force it to be hidden. Will override `add_filter('show_admin_bar', '__return_false');` in functions.php, but doesn't attempt to overcome any CSS based hiding of the admin bar.

* **WPLT_DISABLED_PLUGINS**: An array of plugins to disable. This does not store any data in the database, so plugins that are manually deactivated or activated will stay so when undefined in this constant.

* **WPLT_NOTIFY**: Define this constant as the email address where you'd like to be notified of post updates. Helpful in production to see if a client has submitted a new post, or in development to see if data is being added to the staging environment so you know to pull before pushing. Especially helpful when combined with APIs like Zapier.

* **WPLT_AIRPLANE**: Control loading of external files when developing locally. WP loads certain external files (fonts, gravatar, etc) and makes external HTTP calls. This isn't usually an issue, unless you're working in an evironment without a web connection. This plugin removes / unhooks those actions to reduce load time and avoid errors due to missing files.

	On and Off: Can be toggled from the admin bar by clicking 'Airplane Mode'. A ✗ or ✓ will indicate if Airplane Mode is enabled or disabled. 

##Modification

You can add code that will be executed depending on server name by modifying the following in wp-local-toolbox.php.

I'd love a pull request if you come up with something useful.

```
if (strtoupper(WPLT_SERVER) != 'LIVE' && strtoupper(WPLT_SERVER) != 'PRODUCTION') {
	// Everything except PRODUCTION/LIVE SERVER

	// Hide from robots
	add_filter( 'pre_option_blog_public', '__return_zero' );

} else {
	// PRODUCTION/LIVE SERVER

}
```

##Example

```
define('WPLT_SERVER', 'local'); // set server environment to 'LOCAL'

define('WPLT_COLOR', 'purple'); // set admin bar color to #800080

define('WPLT_ADMINBAR', 'false'); // hide admin bar on front end

define('WPLT_DISABLED_PLUGINS', serialize(array( 'w3-total-cache/w3-total-cache.php', 'updraftplus/updraftplus.php', 'nginx-helper/nginx-helper.php', 'wpremote/plugin.php' ))); // deactive a set of plugins

define('WPLT_AIRPLANE', 'true'); // enable the Airplane Mode toggle

define('WPLT_NOTIFY','someone@somewhere.com') // send an email to someone@somewhere.com when any post or page is updated
```

##Notes

As a special thank you, this plugin will remove the ridiculous `Howdy, ` that is prepended to the username in the admin bar.

You're welcome.

##Credit

* Original WPLT Plugin by Joe Guilmette: https://github.com/joeguilmette/wp-local-toolbox 

* Plugin disabling from Mark Jaquith: https://gist.github.com/markjaquith/1044546

	* Using this fork: https://gist.github.com/Rarst/4402927

* Airplane Mode from Andrew Norcross: https://github.com/norcross/airplane-mode
