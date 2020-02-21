#### Laravel

- **(Only for Laravel 5.4 or minor)** Add provider to config/app.php

```php
'providers' => [
    LaravelGoogleAds\LaravelGoogleAdsProvider::class,
],
```

- Run `$ php artisan vendor:publish` to publish the configuration file `config/google-ads.php` and insert:
    - developerToken
    - clientId & clientSecret
    - refreshToken

#### Lumen

- Add provider to `bootstrap/app.php`

- Copy `vendor/spotonlive/laravel-google-ads/config/config.php` to `config/google-ads.php` and insert:
    - developerToken
    - clientId & clientSecret
    - refreshToken

- Add config to `bootstrap/app.php`

```php
$app->configure('google-ads');
```

### Generate refresh token
*This requires that the `clientId` and `clientSecret` is from a native application.*

Run `$ php artisan googleads:token:generate` and open the authorization url. Grant access to the app, and input the
access token in the console. Copy the refresh token into your configuration `config/google-ads.php`

### Basic usage

The following example is for AdWords, but the general code applies to all
products.


```php
<?php

namespace App\Services;

use LaravelGoogleAds\Services\AdWordsService;
use Google\AdsApi\AdWords\AdWordsServices;
use Google\AdsApi\AdWords\AdWordsSessionBuilder;
use Google\AdsApi\AdWords\v201806\cm\CampaignService;
use Google\AdsApi\AdWords\v201806\cm\OrderBy;
use Google\AdsApi\AdWords\v201806\cm\Paging;
use Google\AdsApi\AdWords\v201806\cm\Selector;

class Service
{
    /** @var AdWordsService */
    protected $adWordsService;
    
    /**
     * @param AdWordsService $adWordsService
     */
    public function __construct(AdWordsService $adWordsService)
    {
        $this->adWordsService = $adWordsService;
    }

    public function campaigns()
    {
        $customerClientId = 'xxx-xxx-xx';

        $campaignService = $this->adWordsService->getService(CampaignService::class, $customerClientId);

        // Create selector.
        $selector = new Selector();
        $selector->setFields(array('Id', 'Name'));
        $selector->setOrdering(array(new OrderBy('Name', 'ASCENDING')));

        // Create paging controls.
        $selector->setPaging(new Paging(0, 100));

        // Make the get request.
        $page = $campaignService->get($selector);
    }
}
```

