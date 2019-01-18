# Docker image for Nextcloud

Copyed from https://github.com/nextcloud/docker/tree/5f8fa19f04cb550993c5e470581d074aa8d05c63/.examples

The modified Dockerfile uses the default image as base image and adds dependencies for all optional packages suggested by nextcloud that may be needed for some specific apps and preview generation, as stated in the  [Prerequisites for manual installation](https://docs.nextcloud.com/server/15/admin_manual/installation/source_installation.html).

The configuration of the preview generation can be done in config.php, as explained in the  [Configuration Parameters](https://docs.nextcloud.com/server/15/admin_manual/configuration_server/config_sample_php_parameters.html#previews).

## Cron

The Dockerfile uses supervisor to run the cron job inside the container (so no extra container is needed). This image runs `supervisord` to start nextcloud and cron as two seperate processes inside the container.

## FPM pool directives

In the Nextcloud base image with PHP-FPM, I've found this message, which appears from time to time:

```
WARNING: [pool www] server reached pm.max_children setting (5), consider raising it
```

I've slightly modified the Dockerfile to copy a new `www.conf` (PHP-FPM default pool configuration) file that sets a higher value for `pm.max_children`.

## PHP memory_limit

Image processing requires a lot of memory. I've increased the value in the `memory_limit.ini` configuration file to prevent PHP from crashing when processing large images. For more details, see the [Nextcloud Gallery Wiki](https://github.com/nextcloud/gallery/wiki/Requirements).

The memory requirements when resizing to the default 2048x2048.

MegaPixels | MegaBytes
---------- | ---------
14.1 | 99
12.2 | 87
8.7 | 66

See [Defining the PHP memory_limit](https://github.com/nextcloud/gallery/wiki/Defining-the-PHP-memory_limit) wiki article for more details.
