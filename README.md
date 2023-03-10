# Laravel GridPane

This package provides integration with the GridPane API. It supports creating servers, retrieving and updating sites, deleting domains, etc.

The package simply provides a `GridPane` facade that acts as a wrapper to the [kylewlawrence/gridpane-api-client-php](https://github.com/kylewlawrence/gridpane-api-client-php) package.

**NB:** Currently only supports bearer token-based authentication.

## Installation

You can install this package via Composer using:

```bash
composer require kylewlawrence/gridpane-api-client-php
```

If you want to make use of the facade you must install it as well.

```php
// config/app.php
'aliases' => [
    ..
    'GridPane' => KyleWLawrence\GridPane\Facades\GridPane::class,
];
```

## Configuration

To publish the config file to `app/config/gridpane-laravel.php` run:

```bash
php artisan vendor:publish --provider="KyleWLawrence\GridPane\Providers\GridPaneServiceProvider"
```

Set your configuration using **environment variables**, either in your `.env` file or on your server's control panel:

- `GP_TOKEN`

The API access token. You can create one at: `https://my.gridpane.com/settings`

- `GP_DRIVER` _(Optional)_

Set this to `null` or `log` to prevent calling the GridPane API directly from your environment.

## Contributing

If you have any questions/problems/request with the SDK (the GridPaneService class), please go to the [GridPane PHP SDK repository](https://github.com/KyleWLawrence/gridpane-api-client-php) with those requests. Pull Requests for the Laravel wrapper are always welcome here. I'll catch-up and develop the contribution guidelines soon. For the meantime, just open and issue or create a pull request.

## Usage

### Facade

The `GridPane` facade acts as a wrapper for an instance of the `GridPane\API\Client` class. Any methods available on this class ([documentation here](https://github.com/kylewlawrence/gridpane-api-client-php#usage)) are available through the facade. for example:

```php
// Get all Servers
GridPane::server()->getAll();

// Create a new server
$newServer = $client->server()->create([
    'servername' => 'hal9000',                          
    'ip' => '199.199.199.199',                        
    'datacenter' => 'space-station-v',                     
    'webserver' => 'nginx',      
    'database' => 'percona'
]);
print_r($newServer);

// Delete a server
GridPane::server(12345)->delete();
```

### Dependency injection

If you'd prefer not to use the facade, you can skip adding the alias to `config/app.php` and instead inject `KyleWLawrence\GridPane\Services\GridPaneService` into your class. You can then use all of the same methods on this object as you would on the facade.

```php
<?php

use KyleWLawrence\GridPane\Services\GridPaneService;

class MyClass {

    public function __construct(GridPaneService $gridpane_service) {
        $this->gridpane_service = $gridpane_service;
    }

    public function getSite() {
        $this->gridpane_service->site()->get(12345);
    }

}
```

This package is available under the [MIT license](http://opensource.org/licenses/MIT).
