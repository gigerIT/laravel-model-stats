# Laravel Model Stats
Model statistics dashboard for your Laravel Application

[![Latest Version on Packagist](https://img.shields.io/packagist/v/jhumanj/laravel-model-stats.svg?style=flat-square)](https://packagist.org/packages/jhumanj/laravel-model-stats)
[![GitHub Tests Action Status](https://img.shields.io/github/workflow/status/jhumanj/laravel-model-stats/run-tests?label=tests)](https://github.com/jhumanj/laravel-model-stats/actions?query=workflow%3Arun-tests+branch%3Amain)
[![GitHub Code Style Action Status](https://img.shields.io/github/workflow/status/jhumanj/laravel-model-stats/Check%20&%20fix%20styling?label=code%20style)](https://github.com/jhumanj/laravel-model-stats/actions?query=workflow%3A"Check+%26+fix+styling"+branch%3Amain)
[![Total Downloads](https://img.shields.io/packagist/dt/jhumanj/laravel-model-stats.svg?style=flat-square)](https://packagist.org/packages/jhumanj/laravel-model-stats)

---
<img style="margin:auto;" src="https://i.ibb.co/hV6dt8L/Capture-d-e-cran-2021-09-11-a-10-54-59.png" alt="Screenshot of sample dashboard" border="0">

This Laravel packages gives you a statistic dashboard for you Laravel application. Think of it as a light version of 
[Grafana](https://grafana.com/), but built-in your Laravel application, and much easier to get started with. 
No code knowledge is required to use Laravel Model Stats, users can do everything from the web interface. It also 
optionally supports custom-code widgets, allowing you to define your widget data with
code, just like you would do with tinker.

---

## Installation

You can install the package via composer:

```bash
composer require jhumanj/laravel-model-stats
```

You can install the package and run the migrations with:

```bash
php artisan model-stats:install
php artisan migrate
```


## Available No-Code Widgets

Different type of widgets (daily count, daily average, etc.) are available. When creating a widget,
you choose a Model, an aggregation type and the column(s) for the graph. You can then resize and move the widgets around on your dashboard.

The aggregation types currently available:
- Daily Count (Number of records per day during selected period).
- Cumulated Daily Count (Cumulated Total record count during selected period).
- Period Total (Number of new records during selected period).
- Group By Count (Count per group for a given column during selected period).
- ... (more to come soon)

For each widget type, date can be any column: `created_at`,`updated_at`,`custom_date`.

## Custom Code Widgetn

You can also use custom code widgets, allowing you to define your widget data with
code, just like you would do with tinker.

Your code must define a `$result` variable containing the data to return to the choosen chart. You can use the `$dateFrom` and `$dateTo` variable.

Example custom code for a bar chart:

```php 
$result = [
    'a' => 10,
    'b' => 20
];
```

Note that for safety reasons, your code won't be allowed to perform any write operations on the database.
You can only use the code to query data and transform it in-memory.
Even if disabling write operations makes things sage, **remote code execution is always a 
very risky operation**. Be sure that your dashboard authorization is properly configured. You may want to disable custom code widgets by setting the `MODEL_STATS_CUSTOM_CODE` env variable to `false`. 

## Dashboard Authorization

The ModelStats dashboard may be accessed at the `/stats` route. By default, you will only be able to access this 
dashboard in the local environment. Within your `app/Providers/ModelStatsServiceProvider.php` file, there is an 
authorization gate definition. This authorization gate controls access to ModelStats in non-local environments. 
You are free to modify this gate as needed to restrict access to your ModelStats installation: 

```php 
/**
 * Register the ModelStats gate.
 *
 * This gate determines who can access ModelStats in non-local environments.
 *
 * @return void
 */
protected function gate()
{
    Gate::define('viewModelStats', function ($user) {
        return in_array($user->email, [
            'taylor@laravel.com',
        ]);
    });
}
```

## Upgrading

Be sure to re-publish the front-end assets when upgrading ModelStats:
```
php artisan model-stats:publish
```

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently.

## Contributing

Please see [CONTRIBUTING](.github/CONTRIBUTING.md) for details.

## Security Vulnerabilities

Please review [our security policy](../../security/policy) on how to report security vulnerabilities.

## Credits (Contributors)

- [Julien Nahum](https://github.com/JhumanJ)

## Inspiration 
- [Grafana](https://grafana.com/): for the dashboard/widget aspect
- [Laravel/Telescope](https://github.com/laravel/telescope): for many things in the package structure (front-end, authorization...)
- [Spatie/Laravel-Web-Tinker](https://github.com/spatie/laravel-web-tinker): for their web tinker implementation, which is used for custom code widgets

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
