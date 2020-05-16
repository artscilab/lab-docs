# Filesystem Setup

!> This documentation was originally created July 14, 2017. Some of it may now be outdated and needs updating. Proceed with caution. 

|     Directory    |                Use             |
|         -        |                 -              |
|     /var/www     |       NGiИX Virtual Hosts      |
|    /etc/nginx    |      NGiИX Configuration       |
| /etc/letsencrypt | TLS Certificates and LE Config |
|    /home/ftp-*   |     FTP User Authorizations    |

## /var/www

Each virtual host gets its own directory here, which is owned by `root`.

This is to enable the use of `chroot` to lock down each site.

Each virtual host directory contains within it an `html` directory, which is owned
either by the FTP user for that site if there is one, or `www-data` if not. This
`html` directory _or a subdirectory_ is where the site is served from by NGiИX

## /etc/nginx

Contains the web server (NGiИX) configuration files.

Outside of the `site-templates-custom` directory, this directory is managed
automatically by the server management software, so editing config files directly
is not a good idea since your changes may be overwritten at any time.

## /etc/letsencrypt

This directory is managed automatically by the installed Let's Encrypt ACME client, Certbot.

It contains all the TLS certificates used by NGiИX virtual hosts, and a configuration that
specifies how the certificates should be renewed. Those renewals are run by regularly by a
cron job &mdash; _it's technically a `systemd` service_.

## /home/ftp-*

These home directories contain only an SSH `authorized_keys` file which determines who can
access that account. Upon login, FTP users' sessions are moved to their respective chroot directory.

---

Contributors: Colin Campbell