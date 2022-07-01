# Dev Guide to migrate Legacy to Symfony

## Introduction

The OpenCATS legacy code is stored in the root of the OpenCATS project, while the code of OpenCATS Symfony it's on the src/ folder.

The legacy application was intended to be light and conceptually to understand. It separated the code in three main areas:

* **Modules:** A module consists of the user interface logic and one or more “templates” to render the HTML page. Some of the modules in OpenCATS are Home, Candidates, Contacts, Calendar, etc.
* **Library components:** Library components are PHP objects which encapsulate lower-level functionality, such as interfacing with a database, parsing addresses, sending e-mail, etc. For each module, there is a roughly corresponding library component but not all libraries directly correspond to a module.
* **Templates:** HTML/CSS/js code with some light logic.

### OpenCATS Legacy Request Flow

Most of OpenCATS request went through index.php and ajax.php files which acted as “router” or delegator to the modules.

1. A page request is sent to index.php or ajax.php file
2. index.php parses the URL and sends the request to the corresponding module (specified by m= in the URL) for further processing
3. A module parses the “action” (specified by a= in the URL) and invokes the corresponding method within the module.
4. The method processes the request, often using library components
5. The function displays a template file, if necessary, and fills in the appropriate data and renders the HTML page

### OpenCATS Legacy URLs

OpenCATS Legacy URLs were designed to be intuitive and easy to use for developers. E.g. HTTP://OpenCATS.org/index.php?m=clients\&a=show\&clientID=239 means:

m = clients a = show clientID = 329

Translation: index.php sends the URL to the “clients” module. The clients module processes the action “show” for clientID 329. We want to display details of client 239.

## Version 0.9.4

This version aims to provide a bridge between Symfony and the legacy code so that the legacy code can be migrated little by little without requiring to rewrite the whole app at a single shot.

In this version, there are no significant changes to the Legacy URLs and the way they are managed from a user point of view. From the request flow, there is an important change. In this version all requests go through a Symfony's front controller.

## What's changed?

* A new docker image: this is required to load the symfony requirements + nginx configuration for the FrontController pattern
* config.php was changed to get variables from environment, to allow configuring symfony app and legacy configuration from the docker image
* Changes to all include paths: A new constant called LEGACY\_ROOT is prepended to all paths for compatibility
* Removal of index.php and ajax.php entry points, they are now functions in LegacyController
* All assets (css, images, javascripts) were relocated into the public assets folder
* Base composer.json and composer.lock are removed (all migrated to composer.json and composer.lock in Symfony structure)
* Behat and PHPUnit tests were standardized into a unified testing framework: Codeception and moved out of their legacy structure to Symfony's.

## Decisions

In order to simplify deployment and installation, the default mode for running OpenCATS will be docker (on all platforms).

## Migration challenges

### Configuration

Symfony configuration is managed by a set of yml files while the legacy application utilizes config.php. The first attempt was to migrate all config.php variables to the parameters.yml but it resulted to be a huge task. In order to solve this issue and also following the recommendations of https://12factor.net/, all values were replace with environment variables which are now injectable through docker-compose by replacing the .env file in the folder where the docker-compose file is executed.

### Functional test code

90% of the time is spent on updating test code. What's causing the issue? Mainly selenium and the PHP frameworks being used do not play well nor work reliable with:

* code that's using blocking javascript alert and confirm functions
* code that's using iframes

## Further performance improvements?

1. https://tideways.io/profiler/blog/5-ways-to-optimize-symfony-baseline-performance
2. http://symfony.com/doc/current/performance.html
