---
title: Setup a dockerized Laravel project on Nginx without subdomains
description: 
published: true
date: 2020-07-10T07:35:44.554Z
tags: 
editor: undefined
---

Note that this is only for when hosting a web application using subdirectory structure like:

domain.com/projects/your-project-1-name
domain.com/projects/your-project-2-name

This structure is slightly more complicated to setup compared to the subdomain structure. The reason behind it is because:
  
- This introduces prefixes in route
- Laravel will only accept X-Forwarded headers from trusted proxies.
- Laravel's issue with signed route as two signatures are generated using completely different methods.

## Proxy Pass
To setup proxy pass, modify the `/etc/nginx/sites-enabled/default` config file using the following configuration:

```
location /projects/project_name/ {
    proxy_pass http://localhost:8000;
    proxy_redirect off;
    proxy_set_header   X-Real-IP $remote_addr;
    proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header   X-Forwarded-Proto https;
    proxy_set_header   X-Forwarded-Host $server_name;
}
```

In this configuration, we have configured:
- Proxy any route with prefix `projects/project_name` into the docker container `http://localhost:8000`
- Pass standard header for identifying the original host, protocol and IP address of client
- Pass the entire route `projects/project_name` into the container

## Laravel Trusted Proxy

Laravel has to be configured to trust a certain proxy. Here is how you can do it.

Run te following command:

```bash
docker inspect <containerID> --format='{{range .NetworkSettings.Networks}}{{.Gateway}}{{end}}'
```
This command will query the gateway IP of the docker application container.

To whitelist the IP in Laravel, modify `app/Http/Middleware/TrustProxies.php` just like below:

```php
<?php

namespace App\Http\Middleware;

use Fideloper\Proxy\TrustProxies as Middleware;
use Illuminate\Http\Request;

class TrustProxies extends Middleware
{
    /**
     * The trusted proxies for this application.
     *
     * @var array|string|null
     */
    protected $proxies = ['docker-gateway-ip-address'];

    /**
     * The headers that should be used to detect proxies.
     *
     * @var int
     */
    protected $headers = Request::HEADER_X_FORWARDED_ALL;
}

```

> Note that the IP address changes every time the container is restarted. The TrustProxies.php will need to be updated. To avoid this, please set the subnet inside your `docker-compose.yml` for your project.
{.is-info}

## Prefixed Routing

> First of all there are a few ways Laravel can generate a URL.
> - `route()` helper
> The helper checks the router config and generates the URL based on it. 
> 
> - `Request::getBaseUrl()` method
> This method uses the base path of the executing index.php to generate their URL
> 
> Laravel uses the `route()` helper to generate signed URL but validates using `Request::getBaseUrl()` method. The inconsistency between these two will fail the validation. 
> {.is-warning}

To create the prefix, use symbolic link in the `public` folder that points to itself. 

To create a symbolic link in public folder, use the command below:

```bash
cd public

# create as many subfolders as you need based on your prefix except
# for the very last subfolder
mkdir projects

# the final subfolder will be a symbolic link below
ln -s ../ ./projects/project_name
```

And that is how to setup a dockerized Laravel application served by Nginx using subdirectory structure instead of subdomains. 

## References
- https://serverfault.com/questions/607615/using-trailing-slashes-in-nginx-configuration
- http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass
- https://laravel.com/docs/7.x/requests#configuring-trusted-proxies