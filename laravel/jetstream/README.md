## qwik web services

```bash
service='my_service'
```

```bash
laravel new $service --jet --teams
```

```bash
ln -s $PWD/$service $HOME/Sites
```

```bash
cd $service
```

```bash 
mysql -u root -e "CREATE DATABASE $service"
```

```bash
php artisan migrate:fresh
```

```bash
code . && code config/jetstream.php
```

enable api feature

```php
    'features' => [
        // Features::termsAndPrivacyPolicy(),
        // Features::profilePhotos(),
        Features::api(),
        Features::teams(['invitations' => true]),
        Features::accountDeletion(),
    ],
```




## Forcing urls to use https

```php
<?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Register any application services.
     *
     * @return void
     */
    public function register()
    {
        //
    }

    /**
     * Bootstrap any application services.
     *
     * @return void
     */
    public function boot()
    {
        //if the config('app.url_scheme') is set to https, then we will force the scheme to be https
        if (config('app.url_scheme') === 'https') {
            \URL::forceScheme('https');
        }
    }
}
```
`config/app.php`
```php
return [
//..
'url_scheme' => env('APP_URL_SCHEME', 'http'),
//..
];
```

`.env`
```ini
APP_URL_SCHEME=https
```

## upgrading to use vite instead of mix

`vite.config.js`
```js
export default defineConfig({
    plugins: [
        laravel({
            input: [
                'resources/css/app.css',
                'resources/js/app.js',
            ],
            refresh: [
                ...refreshPaths,
                'app/Http/Livewire/**',
            ],
        }),
    ],
});
```

`postcss.config.js`
```js
module.exports = {
    plugins: {
        tailwindcss: {},
        autoprefixer: {},
    },
};
```
`resources/js/app.js`
```js
import './bootstrap';

import Alpine from 'alpinejs';
import focus from '@alpinejs/focus';
window.Alpine = Alpine;

Alpine.plugin(focus);

Alpine.start();
```
`resources/js/bootstrap.js`
```js
import _ from 'lodash';
window._ = _;

/**
 * We'll load the axios HTTP library which allows us to easily issue requests
 * to our Laravel back-end. This library automatically handles sending the
 * CSRF token as a header based on the value of the "XSRF" token cookie.
 */

import axios from 'axios';
window.axios = axios;

window.axios.defaults.headers.common['X-Requested-With'] = 'XMLHttpRequest';

/**
 * Echo exposes an expressive API for subscribing to channels and listening
 * for events that are broadcast by Laravel. Echo and event broadcasting
 * allows your team to easily build robust real-time web applications.
 */

// import Echo from 'laravel-echo';

// import Pusher from 'pusher-js';
// window.Pusher = Pusher;

// window.Echo = new Echo({
//     broadcaster: 'pusher',
//     key: import.meta.env.VITE_PUSHER_APP_KEY,
//     wsHost: import.meta.env.VITE_PUSHER_HOST ? import.meta.env.VITE_PUSHER_HOST : `ws-${import.meta.env.VITE_PUSHER_APP_CLUSTER}.pusher.com`,
//     wsPort: import.meta.env.VITE_PUSHER_PORT ?? 80,
//     wssPort: import.meta.env.VITE_PUSHER_PORT ?? 443,
//     forceTLS: (import.meta.env.VITE_PUSHER_SCHEME ?? 'https') === 'https',
//     enabledTransports: ['ws', 'wss'],
// });
```
`resources/css/app.css`
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

`package.json`
```json
{
    "private": true,
    "scripts": {
        "dev": "vite",
        "build": "vite build"
    },
    "devDependencies": {
        "@alpinejs/focus": "^3.10.5",
        "@tailwindcss/forms": "^0.5.2",
        "@tailwindcss/typography": "^0.5.0",
        "alpinejs": "^3.0.6",
        "autoprefixer": "^10.4.7",
        "axios": "^1.1.2",
        "laravel-vite-plugin": "^0.7.0",
        "lodash": "^4.17.19",
        "postcss": "^8.4.14",
        "tailwindcss": "^3.1.0",
        "vite": "^3.0.0"
    }
}
```
