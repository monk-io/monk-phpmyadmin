# phpMyAdmin meets Monk

This repository contains Monk.io template to deploy phpMyAdmin either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

- [phpMyAdmin meets Monk](#phpmyadmin-meets-monk)
  - [Prerequisites](#prerequisites)
    - [Make sure monkd is running](#make-sure-monkd-is-running)
    - [Clone Repository](#clone-repository)
    - [Load Template](#load-template)
    - [Verify if it's loaded correctly](#verify-if-its-loaded-correctly)
  - [Deploy basic phpMyAdmin](#deploy-basic-phpmyadmin)
    - [Start runnable](#start-runnable)
    - [Check if it works](#check-if-it-works)
  - [Variables](#variables)
    - [Setting variables](#setting-variables)
    - [Default variables](#default-variables)
  - [Stop, remove and clean up workloads and templates](#stop-remove-and-clean-up-workloads-and-templates)

## Prerequisites

- [Install Monk](https://docs.monk.io/docs/get-monk)
- [Register and Login Monk](https://docs.monk.io/docs/acc-and-auth)
- [Add Cloud Provider](https://docs.monk.io/docs/cloud-provider)
- [Add Instance](https://docs.monk.io/docs/multi-cloud)

### Make sure monkd is running

```bash
$ monk status
daemon: ready
auth: logged in
not connected to cluster
```

### Clone Repository

```bash
git clone git@github.com:monk-io/monk-phpmyadmin.git
```

### Load Template

```bash
cd monk-phpmyadmin
$ monk load MANIFEST
```

### Verify if it's loaded correctly

```bash
$ monk list phpmyadmin
✔ Got the list
Type      Template               Repository  Version  Tags
runnable  phpmyadmin/phpmyadmin  local       -        phpmyadmin, mysqldb, mariadb, database
```

## Deploy basic phpMyAdmin

### Start runnable

```bash
$ monk run phpmyadmin/phpmyadmin
✔ Starting the job: local/phpmyadmin/phpmyadmin... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% phpmyadmin:latest local
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ New container 36f419b259a020b64b2d833ab59c8bd5-pmyadmin-phpmyadmin-phpmyadmin created DONE
✔ Started local/phpmyadmin/phpmyadmin

🔩 templates/local/phpmyadmin/phpmyadmin
 └─🧊 Peer local
    └─🔩 templates/local/phpmyadmin/phpmyadmin
       └─📦 36f419b259a020b64b2d833ab59c8bd5-pmyadmin-phpmyadmin-phpmyadmin running
          ├─🧩 phpmyadmin:latest
          └─🔌 open 1.1.1.1:80 -> 80

💡 You can inspect and manage your above stack with these commands:
        monk logs (-f) local/phpmyadmin/phpmyadmin - Inspect logs
        monk shell     local/phpmyadmin/phpmyadmin - Connect to the container's shell
        monk do        local/phpmyadmin/phpmyadmin/action_name - Run defined action (if exists)
💡 Check monk help for more!
```

### Check if it works

```bash
$ curl 1.1.1.1:80
<!doctype html>
<html lang="en" dir="ltr">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="referrer" content="no-referrer">
  <meta name="robots" content="noindex,nofollow">
  <style id="cfs-style">html{display: none;}</style>
  <link rel="icon" href="favicon.ico" type="image/x-icon">
  <link rel="shortcut icon" href="favicon.ico" type="image/x-icon">
...
```

## Variables

### Setting variables

You can set variable in either one of three ways:

1) Directly by editing this template
2) By using `monk run/update --set` command
3) By using `monk run/update --variables-file` command

### Default variables

The variables are stored in `manifest.yaml` file.
You can quickly setup by editing the values there.

| Variable                  | Description                                                                                                                | Default |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------- | ------- |
| PMA_ARBITRARY             | when set to 1 connection to the arbitrary server will be allowed                                                           |         |
| PMA_HOST                  | define address/host name of the MySQL server                                                                               |         |
| PMA_VERBOSE               | define verbose name of the MySQL server                                                                                    |         |
| PMA_PORT                  | define port of the MySQL server                                                                                            |         |
| PMA_HOSTS                 | define comma separated list of address/host names of the MySQL servers                                                     |         |
| PMA_VERBOSES              | define comma separated list of verbose names of the MySQL servers                                                          |         |
| PMA_PORTS                 | define comma separated list of ports of the MySQL servers                                                                  |         |
| PMA_USER and PMA_PASSWORD | define username and password to use only with the config authentication method                                             |         |
| PMA_ABSOLUTE_URI          | the full URL to phpMyAdmin. Sometimes needed when used in a reverse-proxy configuration. Don't set this unless needed.     |         |
| PMA_CONFIG_BASE64         | if set, this option will override the default config.inc.php with the base64 decoded contents of the variable              |         |
| PMA_USER_CONFIG_BASE64    | if set, this option will override the default config.user.inc.php with the base64 decoded contents of the variable         |         |
| PMA_CONTROLHOST           | when set, this points to an alternate database host used for storing the phpMyAdmin Configuration Storage database         |         |
| PMA_CONTROLPORT           | if set, will override the default port (3306) for connecting to the control host for storing the phpMyAdmin Configuration  | 3306    |
| PMA_PMADB                 | define the name of the database to be used for the phpMyAdmin Configuration Storage database. When not set, the advanced   |         |
| PMA_CONTROLUSER           | define the username for phpMyAdmin to use for advanced features (the controluser)                                          |         |
| PMA_CONTROLPASS           | define the password for phpMyAdmin to use with the controluser                                                             |         |
| PMA_QUERYHISTORYDB        | when set to true, enables storing SQL history to the phpMyAdmin Configuration Storage database. When false, history is     |         |
| PMA_QUERYHISTORYMAX       | when set to an integer, controls the number of history items. See documentation. Defaults to 25.                           | 25      |
| MAX_EXECUTION_TIME        | if set, will override the maximum execution time in seconds (default 600) for phpMyAdmin ($cfg['ExecTimeLimit']) and       | 600     |
| MEMORY_LIMIT              | if set, will override the memory limit (default 512M) for phpMyAdmin ($cfg['MemoryLimit']) and PHP memory_limit (format as | 512M    |
| UPLOAD_LIMIT              | if set, this option will override the default value for apache and php-fpm (format as [0-9+](K,M,G))                       | 10M     |
| HIDE_PHP_VERSION          | if defined, this option will hide the PHP version (expose_php = Off). Set to any value (such as HIDE_PHP_VERSION=true).    | true    |

## Stop, remove and clean up workloads and templates

```bash
monk stop     phpmyadmin/phpmyadmin
monk purge    phpmyadmin/phpmyadmin
monk purge -x phpmyadmin/phpmyadmin
```
