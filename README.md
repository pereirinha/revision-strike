# Revision Strike

[![Build Status](https://travis-ci.org/stevegrunwell/revision-strike.svg?branch=master)](https://travis-ci.org/stevegrunwell/revision-strike)
[![Code Climate](https://codeclimate.com/github/stevegrunwell/revision-strike/badges/gpa.svg)](https://codeclimate.com/github/stevegrunwell/revision-strike)
[![Test Coverage](https://codeclimate.com/github/stevegrunwell/revision-strike/badges/coverage.svg)](https://codeclimate.com/github/stevegrunwell/revision-strike/coverage)

Unless post revisions are explicitly limited, WordPress will build up a hefty sum of revisions over time. While it's great to have revision history for some recent content, the chances that old revisions will be necessary diminish the longer a post has been published. Revision Strike is designed to automatically remove these unneeded revisions on older, published posts.

**How does it work?**

First, a threshold is set, with a default of 30 days. Once a day, Revision Strike will run and find any post revisions in the database attached to **published** posts with a post date of at least 30 (or your custom threshold) days ago, and "strike" (tear-down and remove) them from the WordPress database.


## Usage

There are a number of ways to interact with Revision Strike:


### WP Cron

Upon plugin activation, a hook is registered to trigger the `revisionstrike_strike_old_revisions` action daily, which kicks off the striking process. This hook is then automatically removed upon plugin deactivation.


### WP-CLI

If you make use of [WP-CLI](http://wp-cli.org/) on your site you may trigger Revision Strike with the following command:

```
$ wp revision-strike clean
```

#### Arguments

<dl>
	<dt>--days=&lt;days&gt;</dt>
	<dd>Remove revisions on posts published at least &lt;days&gt; day(s) ago.</dd>
	<dt>--verbose</dt>
	<dd>Enable verbose logging of deleted revisions.</dd>
</dl>


## Filters

Revision Strike leverages the [WordPress Plugin API](https://codex.wordpress.org/Plugin_API) to let you customize its behavior without editing the codebase directly.

### revisionstrike_post_types

Controls the post types for which revisions should be automatically be purged.

<dl>
	<dt>(array) $post_types</dt>
	<dd>An array of post types.</dd>
</dl>

#### Example

By default, Revision Strike only works against posts of the "Post" post type. If you'd like to include other post types (pages, custom post types, etc.), this filter will allow you do so.

```php
/**
 * Include the "page" and "book" custom post type in Revision Strike.
 *
 * @param array $post_types An array of post types.
 */
function theme_set_post_types( $post_types ) {
	return array( 'post', 'page', 'book' );
}
add_filter( 'revisionstrike_post_types', 'theme_set_post_types' );
```


## Releases

### 0.1

Initial public release.