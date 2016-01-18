# docker-librenms
A turnkey docker container for [librenms](http://www.librenms.org/)

Is still work in progress, but is relatively stable.

## Setup
Simply edit docker-compose.yml to your liking.

The following can (and probably should) be adjusted under "environment"

* LIBRENMS_USERNAME: The username you will use to login to librenms. User will not be created if it is removed (may be useful for existing database)
* LIBRENMS_PASSWORD: The password you will use to login to librenms
* LIBRENMS_EMAIL: The email you will use with your librenms username
* LIBRENMS_THREADS: The number of poller threads to have
* LIBRENMS_TZ: The timezone for librenms

## Notes:
* The db volume mounts, along with the rrd/logs mounts can be moved elsewhere, the folders are just there for convenience.
* The container does not do "replacements" of **any** files for compatibility with future releases. Instead, all files are modified in-place using augeas and sed (for uncommenting).
* rrdcached is included for reducing the load on the container
* performance_schema is disabled by default on the mysql container to reduce memory usage. It can be re-enabled by removing mysql/conf/performance_schema.cnf
* No HTTPS support, use [jwilder/nginx-proxy](https://github.com/jwilder/nginx-proxy) to proxy connections to the container
