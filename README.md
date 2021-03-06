# Grovo
Laravel 5.1 Grovo API Service Provider

Grovo API Documentation

http://docs.grovo.apiary.io

### Required setup

In the `repositories` key of `composer.json` file add the following

```php
"require": {
  "php": ">=5.5.9",
  "laravel/framework": "5.1.*",
  ...
  "upwebdesign/grovo": "dev-master",
},
...
"repositories": [ {
  "type": "vcs",
  "url": "https://github.com/upwebdesign/grovo"
}],
```
Run the Composer update comand

    $ composer update

or

    $ composer update upwebdesign/grovo

In your `config/app.php` add `'Upwebdesign\Grovo\GrovoServiceProvider'` to the end of the `$providers` array

```php
'providers' => array(

    'Illuminate\Foundation\Providers\ArtisanServiceProvider',
    'Illuminate\Auth\AuthServiceProvider',
    ...
    'Upwebdesign\Grovo\GrovoServiceProvider',

),
```

Don't forget to dump composer autoload

    $ composer dump-autoload

### Configuration

In `config/filesystems.php`, add new disk

```php
'config' => [
    'driver' => 'local',
    'root' => config_path()
],
```

Publish the grovo.config file

    $ php artisan vendor:publish

In order to make requests to Grovo, we need to obtain an API Token. You can do so by running:

    $ php artisan grovo:requestToken

When this command finishes, it will update your `config/grovo.config` file with the new token. It is recommended to store your token in your `.env`. Example:

```php
APP_ENV=local
APP_DEBUG=true
...
GROVO_TOKEN=xxxxxxxx
```

In your `config/grovo.php` config file, update

```php

  'client_id' => '',
  'client_secret' => '',
  ...
  'token' => env(GROVO_TOKEN),
```

Now, access to your Grovo Token is securely stored in your `.env` file.

## Usage

So, we have our Grovo API Token and are ready to start making requests.

In case of an error a exception, `HttpException`, will be thrown and can be caught.

```php
use Upwebdesign\Grovo\Grovo
```

Type-hint

```php
public function method(Grovo $grovo)
```

Get User

```php
$grovo->user()->get($id);
```

Creat User
```php
$grovo->user()->create([
  'email': 'jimmys@grovo.com',
  'first_name': 'Jon',
  'last_name': 'Sales',
  'groups': [
    'Engineering',
    'Platform',
    'API'
  ],
  'office_location': 'New York',
  'department': 'Engineering',
  'job_title': 'Senior Engineer',
  'employee_id': 8,
  'employment_type': 'fulltime',
  'hire_date': '2014-11-17 22:36:59',
  'status': 'active'
]);
```

Update User
```php
$grovo->user()->update($id, [
  'first_name': 'Feddy',
  'groups': 'Architecture'
]);
```

Delete User
```php
$grovo->user()->delete($id);
```

A full example

```php
<?php

namespace App\Http\Controllers;

use Upwebdesign\Grovo\Grovo

class UserController extends Controller
{
    public function show(Grovo $grovo)
    {
        $user = $grovo->user()->get(1);
        return view('user.show', compact('user'));
    }
}
```
