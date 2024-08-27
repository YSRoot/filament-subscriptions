![Screenshot](https://raw.githubusercontent.com/tomatophp/filament-subscriptions/master/arts/megoxv-tomato-subscriptions.jpg)

# Filament Subscriptions

[![Latest Stable Version](https://poser.pugx.org/tomatophp/filament-subscriptions/version.svg)](https://packagist.org/packages/tomatophp/filament-subscriptions)
[![License](https://poser.pugx.org/tomatophp/filament-subscriptions/license.svg)](https://packagist.org/packages/tomatophp/filament-subscriptions)
[![Downloads](https://poser.pugx.org/tomatophp/filament-subscriptions/d/total.svg)](https://packagist.org/packages/tomatophp/filament-subscriptions)

Manage subscriptions and feature access with customizable plans in FilamentPHP

## Screenshots

![Tenant Menu](https://raw.githubusercontent.com/tomatophp/filament-subscriptions/master/arts/tenant-menu.png)
![User Menu](https://raw.githubusercontent.com/tomatophp/filament-subscriptions/master/arts/user-menu.png)
![Billing Page](https://raw.githubusercontent.com/tomatophp/filament-subscriptions/master/arts/billing-page.png)
![Change Subscription Modal](https://raw.githubusercontent.com/tomatophp/filament-subscriptions/master/arts/change-subscription-modal.png)
![Plans](https://raw.githubusercontent.com/tomatophp/filament-subscriptions/master/arts/plans.png)
![Edit Plan](https://raw.githubusercontent.com/tomatophp/filament-subscriptions/master/arts/edit-plan.png)
![Create Feature](https://raw.githubusercontent.com/tomatophp/filament-subscriptions/master/arts/create-feature.png)
![Features](https://raw.githubusercontent.com/tomatophp/filament-subscriptions/master/arts/features.png)
![Subscriptions](https://raw.githubusercontent.com/tomatophp/filament-subscriptions/master/arts/subscriptions.png)
![Create Subscription](https://raw.githubusercontent.com/tomatophp/filament-subscriptions/master/arts/create-subscription.png)
![Cancel Modal](https://raw.githubusercontent.com/tomatophp/filament-subscriptions/master/arts/cancel-modal.png)


## Features

- [x] Manage plans
- [x] Manage features
- [x] Manage subscriptions
- [x] multi-tenancy support
- [x] Native Filament subscriptions support
- [x] Subscription Middleware
- [x] Subscription Page like Spark
- [x] Subscription Events
- [x] Subscription Facade Hook
- [ ] Subscription Webhooks
- [ ] Subscription Payments Integrations

## Installation

```bash
composer require tomatophp/filament-subscriptions
```

now you need to publish your migrations

```bash
php artisan vendor:publish --provider="Laravelcm\Subscriptions\SubscriptionServiceProvider"
```

after that please run this command

```bash
php artisan filament-subscriptions:install
```

finally register the plugin on `/app/Providers/Filament/AdminPanelProvider.php`

```php
->plugin(\TomatoPHP\FilamentSubscriptions\FilamentSubscriptionsPlugin::make())
```

## Using 

now on your User.php model or any auth model you like you need to add this trait

```php
namespace App\Models;

use Laravelcm\Subscriptions\Traits\HasPlanSubscriptions;
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable
{
    use HasPlanSubscriptions;
}
```

To configure the billing provider for your application, use the `FilamentSubscriptionsProvider`:

```php
use TomatoPHP\FilamentSubscriptions\Providers\FilamentSubscriptionsProvider;
use Filament\Panel;

public function panel(Panel $panel): Panel
{
    return $panel
        // ...
        ->tenantBillingProvider(new FilamentSubscriptionsProvider());
}
```

This setup allows users to manage their billing through a link in the tenant menu.

## Requiring a Subscription

To enforce a subscription requirement for any part of your application, use the `requiresTenantSubscription()` method:

```php
use Filament\Panel;

public function panel(Panel $panel): Panel
{
    return $panel
        // ...
        ->requiresTenantSubscription();
}
```


Users without an active subscription will be redirected to the billing page.

## Register New Subscriper Type

you can register new subscriber type by using this code

```php
use TomatoPHP\FilamentSubscriptions\Facades\FilamentSubscriptions;

public function boot()
{
    FilamentSubscriptions::register(
        \TomatoPHP\FilamentSubscriptions\Services\Contracts\Subscriber::make()
            ->name('User')
            ->model(\App\Models\User::class)
    );
}
```

## Use Events

we add events everywhere on the subscription process and here is the list of events

- `TomatoPHP\FilamentSubscriptions\Events\CancelPlan`
- `TomatoPHP\FilamentSubscriptions\Events\ChangePlan`
- `TomatoPHP\FilamentSubscriptions\Events\RequestPlan`
- `TomatoPHP\FilamentSubscriptions\Events\SubscribePlan`

all events have the same payload

```php
return [
    "old" => //Plan,
    "new" => //Plan,
    "subscription" => //Subscription,
]
```

## Use Facade Hook

you can use the facade hook to add your custom logic to the subscription process

```php

use TomatoPHP\FilamentSubscriptions\Facades\FilamentSubscriptions;

FilamentSubscriptions::afterSubscription(function (array $data){
    // your logic here
});

FilamentSubscriptions::afterRenew(function (array $data){
    // your logic here
});

FilamentSubscriptions::afterChange(function (array $data){
    // your logic here
});

FilamentSubscriptions::afterCanceling(function (array $data){
    // your logic here
});

```
## Publish Assets

you can publish config file by use this command

```bash
php artisan vendor:publish --tag="filament-subscriptions-config"
```

you can publish views file by use this command

```bash
php artisan vendor:publish --tag="filament-subscriptions-views"
```

you can publish languages file by use this command

```bash
php artisan vendor:publish --tag="filament-subscriptions-lang"
```

## Support

you can join our discord server to get support [TomatoPHP](https://discord.gg/vKV9U7gD3c)

## Docs

you can check docs of this package on [Docs](https://docs.tomatophp.com/filament/filament-withdrawals)

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information on what has changed recently.

## Security

Please see [SECURITY](SECURITY.md) for more information about security.

## Credits

- [Abdelmjid](https://wa.me/201091523908)

## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.


