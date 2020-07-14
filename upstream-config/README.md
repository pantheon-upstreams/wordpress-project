# Composer-enabled Wordpress Upstream template
## Experimental

This is Pantheon's recommended starting point for forking new upstreams that work with the Platform's integrated
Composer build process.

Because it is under active development, you should not create permanent/production sites using this repository
yet. We make no guarantees of backwards compatibility. Merging new updates from this repository may break existing
sites.

We've empowered Pantheon Upstreams to influence the Composer packages that are included in downstream sites
by including two `composer.json` files in this repository:
  * The root `/composer.json` should be regarded as owned by the downstream site. Upstream maintainers should avoid
    editing it as much as possible so that the downstream site maintainer may adjust it without creating potential
    for conflicts when merging upstream updates.
  * The `upstream-config/composer.json` should be regarded as owned by the upstream maintainer. It is included by the
    root `composer.json`, and it allows upstreams to add or remove packages on downstream sites. These changes will be
    automatically incorporated into the downstream site whenever upstream updates are applied.

The Composer build system that's integrated into the Pantheon Platform does not require you to use this method, but
it is Pantheon's recommendation. Upstreams with more complex needs could choose other methods to separate
the downstream composer.json from the upstream's, such as by using [wikimedia/composer-merge-plugin](https://github.com/wikimedia/composer-merge-plugin).

While this repository is designed to be forked, it is possible to use it directly as an upstream and
create sites from it.

## Documentation for upstream maintainers

### Create your fork

Fork this repository on GitHub, and use the new repository as your starting point for custom upstreams.

Once you've forked this repository, Pantheon's documentation can help you [connect your repository to
Pantheon.](https://pantheon.io/docs/create-custom-upstream#connect-repository-to-pantheon)

#### Edit the vendor name used in the upstream-config

Pantheon ships the `upstream-config/composer.json` file with the line 
`"name": "pantheon-upstreams/upstream-config"`. You should change `pantheon-upstreams` to a name
more befitting your organization, and update the `require` section of the root `/composer.json`
file to match it.

#### Add and remove packages

You can use `composer require` inside the `upstream-config` directory to easily edit the upstream composer.json.

A note about themes: This template places a theme in the upstream composer.json. This choice best fits situations
where the downstream sites will all use the same theme. If your upstream does not mean to lock downstream sites
into a particular theme, you should remove themes from the upstream composer.json. Sites are unable to remove packages
included by the upstream from those that are installed.

### Maintain your fork

When you make modifications to your upstream based on this repository, a few special considerations apply:
  1. When editing `upstream-config/composer.json`, always increment the `version` number listed in that file.  
     Depending on the contents of the root `/composer.json`, this is sometimes necessary for Composer to detect the
     changes to your upstream configuration.
  2. You can perform a quick sanity check of your changes to `upstream-config/composer.json` by running
    `composer install` or `composer update` in the `upstream-config` directory. Just take care not to rely on
    ["root-only" properties of composer.json](https://getcomposer.org/doc/04-schema.md).