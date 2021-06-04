# Nested redirects for Laravel

- [Overview](#overview)   
- [Installation](#installation)   
- [Usage](#usage)   

# Overview

This package allows you to create simple or multiple nested redirects for your Laravel applications.   
   
This package can be useful from an SEO perspective, when in your application, you have URLs that have the potential of being modified.
   
**Example of the dynamic redirecting logic:**
* Let's assume you have an URL called `/original`   
   
* You create a redirect from `/original` to `/modified`
  > Accessing `/original` will redirect to `/modified`   
* You create another redirect from `/modified` to `/modified-again`   
  > Accessing `/modified` will redirect to `/modified-again` AND   
  > Accessoing `/original` will redirect to `/modified-again`   
* You create another redirect from `/modified-again` to `/modified-yet-again`   
  > Accessing `/modified-again` will redirect to `/modified-yet-again` AND      
  > Accessing `/modified` will redirect to `/modified-yet-again` AND   
  > Accessing `/original` will redirect to `/modified-yet-again`   
* You create another redirect from `modified-yet-again` to `/original`  
  > Accessing `/modified-yet-again` will redirect to `/original` AND   
  > Accessing `/modified-again` will redirect to `/original` AND   
  > Accessing `/modified` will redirect to `/original`
  
# Installation

Install the package via Composer:

```
composer require apexmuse/laravel-redirects
```

Publish the config file with:

```
php artisan vendor:publish --provider="ApexMuse\Redirects\ServiceProvider" --tag="config"
```

Publish the migration file with:

```
php artisan vendor:publish --provider="ApexMuse\Redirects\ServiceProvider" --tag="migrations"
```

After the migration has been published you can create the `redirects` table by running:

```
php artisan migrate
```

# Usage

### Add the middleware

In order for the redirecting functionality to actually happen, you need to add the `ApexMuse\Redirects\Middleware\RedirectRequests` middleware.

Go to `App\Http\Kernel` and add the `ApexMuse\Redirects\Middleware\RedirectRequests` middleware in your `$middlewareGroups` groups of choice.

```php
/**
 * The application's route middleware groups.
 *
 * @var array
 */
protected $middlewareGroups = [
    'web' => [
        ...
        \ApexMuse\Redirects\Middleware\RedirectRequests::class,
```

### Creating redirects

You should never use the `ApexMuse\Redirects\Models\Redirect` directly, as this is the default concrete implementation for the `ApexMuse\Redirects\Contracts\RedirectModelContract`.   
  
Using the `ApexMuse\Redirects\Models\Redirect` model class directly will prevent you from being able to extend the model's capabilities.

You can create redirects that will be stored inside the `redirects` table like this:   

```php
app('redirect.model')->create([
    'old_url' => '/your-old-url',
    'new_url' => '/your-new-url',
    'status' => 301
]);
```

To see how you can extend the `ApexMuse\Redirects\Models\Redirect` model's capabilities, please read the comments from `/config/redirects.php -> redirect_model`

# Credits

- [Andrei Badea](https://github.com/zbiller)
- [All Contributors](../../contributors)

# Security

If you discover any security related issues, please email andrei.badea@neurony.ro instead of using the issue tracker.

# License

The MIT License (MIT). Please see [LICENSE](LICENSE.md) for more information.

# Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently.

# Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.
