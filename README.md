# github-theme-updater

The theme-review process on wordpress.org can take months...

* The initial review is usually a coupe of months.
* Then if all goes well the theme proceeds to the final review which takes 2-4 months at the time of this writing.
* If you for some reason update your theme on WP's trac while awaiting in the final-review queue (for example upload a new version with fixes), then you lose your place in the queue and move to the last position.
* If your theme is tagged as accessibility-ready, then you have to wait another 2-4 months.

Overall a theme review can easily take more than 6 months.

In the meantime, life goes on. If your business depends on your themes, then you may want to implement an alternative method to provide updates for your theme so people can start using it and you can start growing.

This is a simple class that lets you use github releases in your repository to handle your theme updates.
A more robust solution would be to use the [github-updater](https://github.com/afragen/github-updater) plugin, but I'm in no mood to include a 540kb library in my theme when I can do the same in less than 200 lines of code.

## Implementing in the theme:

1. Download the php file from this repo and include it in your theme.
2. Change the namespace on the top of the file to match the one you use in your theme (you are using namespaces, right?).
3. Include the updater class in your functions.php file:

```php
add_action( 'after_setup_theme', function() {
	get_template_part( 'inc/classes/Updater' );
});
```

4. At the bottom of the `Updater.php` file init the updater:

```php
new Updater(
	[
		'name' => 'Gridd',                     // Theme Name.
		'repo' => 'wplemon/gridd',             // Theme repository.
		'slug' => 'gridd',                     // Theme Slug.
		'url'  => 'https://wplemon.com/gridd', // Theme URL.
		'ver'  => 1.2                          // Theme Version.
	]
);
```

## Releasing an update on Github

When you want to release an update you can simply go to your repository > Releases and create a new release.  
* The version shouldeither be formatted as `1.2`, `1.2.3`, `v1.2` or `v1.2.3` etc. The version comparison will remove the `v` and the script will be able to compare versions. However, if you call your release-tag `tomato` don't expect it to work.
* You **have to upload a zip file.**. When creating your release, upload a `.zip` file in there. The default packages created by github have the version number inside the folder-name, and that has the potential to break things on user sites when updating. This updater script will only detect the file uploaded as an asset manually by you.

## Releasing on w.org

Remember to remove the script when uploading an update on w.org. When your theme goes live you can remove it from the repository as well, this is only meant as a stepping stone.

Until your theme goes live you can ignore the class inclusion if you did it using `get_template_part` as in the example above. If the file doesn't exist nothing bad will happen.

## Advice

* Use at your own risk. If you don't follow the instructions above, things will break.
* This script is as simple as it can be. If you find a bug, pull-requests are more than welcomed.
* Personally I prefer using a `.gitattributes` file to exclude the development files like `.sass`, `.map`, `.editorconfig` etc. If you use such a file on your project, you can just add `Updater.php export-ignore` in there and it won't be included in your theme export when you get your build ready for w.org

That's all. I hope you enjoy using this and it makes your life somewhat easier.