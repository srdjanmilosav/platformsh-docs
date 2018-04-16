# Magento 2

We recommend using Composer to load up dependencies and get the Magento vendor folders. A full example is available at https://github.com/platformsh/platformsh-example-magento.

## Minimum requirements for Magento 2 on Platform.sh

Magento 2 usually requires more resources than are available on our Development or Standard plans. We recommend choosing at least a Medium for basic, low traffic sites or development. [Submit a sizing request if you need help determining the best fit](https://platform.sh/quote).

## Setting up your project

The example project requires you to set up some things _before_ you push your code. Click through the initial setup wizard till you get to the _Continue later_ button, then go into your `master` environment configuration.

### 1. Project Variables

This project relies on the following [project variables](https://docs.platform.sh/configuration/app/variables.html) being set prior to initial deploy.

- ADMIN_USERNAME (defaults to “admin”)
- ADMIN_FIRSTNAME (defaults to “John”)
- ADMIN_LASTNAME (defaults to “Doe”)
- ADMIN_EMAIL (defaults to “john@example.com”)
- ADMIN_PASSWORD (defaults to “admin12”)
- ADMIN_URL (defaults to “admin”)
- APPLICATION_MODE (defaults to “production”)

The latter can be changed at any time adjust the Application Mode on the next deploy.

### 2. Magento Commerce credentials

This repository requires your Magento credentials to be added. You can do this via the project variables or an `auth.json` file in the repository. You can get those credentials in your [Magento Commerce account](https://www.magentocommerce.com/magento-connect/customerdata/accessKeys/list/).

**`auth.json` method**

```
"http-basic": {
      "repo.magento.com": {
         "username": "<public-key>",
         "password": "<private-key>"
      }
   }
```

**Project variables method**

Create a new environment variable called `env:COMPOSER_AUTH`, and paste in the same JSON structure as for the auth.json method.

Tick `JSON Value` and Save.

### 3. Push your code

You can now push up your fork of this repository to your Platform.sh project and run the installer.

## Example project setup

Here are the specific files for [this example](https://github.com/platformsh/platformsh-example-magento) to work on Platform.sh:

```
.platform/
         /routes.yaml
         /services.yaml
.platform.app.yaml
auth.json
composer.json
magento-vars.php
php.ini
```

### Application configuration

The `composer.json` will fetch Magento, and some configuration scripts to prepare your application
for Platform.sh.

`.platform.app.yaml` has the basic configuration of our application (we call it `mymagento`). This configures a
Composer based application, that we depend on a database called `database` and that we what to run a build script and a
deploy script during deployment, and also set up some crons and file mounts.

{% codesnippet "https://github.com/platformsh/platformsh-example-magento/blob/master/.platform.app.yaml", language="yaml" %}{% endcodesnippet %}

### Route configuration

In `.platform/routes.yaml` we just say that we will redirect www to the naked domain, and that the application that
will be serving HTTP will be the one we called `mymagento`.

{% codesnippet "https://github.com/platformsh/platformsh-example-magento/blob/master/.platform/routes.yaml", language="yaml" %}{% endcodesnippet %}

### Services

In `.platform/services.yaml` we say we want a MySQL instance, a Redis and a Solr. That would cover most basic Magento
needs.

{% codesnippet "https://github.com/platformsh/platformsh-example-magento/blob/master/.platform/services.yaml", language="yaml" %}{% endcodesnippet %}

