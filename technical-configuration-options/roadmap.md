# Roadmap

## Current documented baseline

This documentation targets OpenCATS 0.12.0. The Composer requirement is PHP 8.4.1 or later within PHP 8; the release CI matrix uses PHP 8.4.27 and 8.5.12 on Linux with MariaDB 10.7 for Docker-backed tests. Deployments should use current security-patched PHP packages.

## Completed in 0.12.0

* Bootstrap 5.3 styling across the application, login, installer and Career Portal.
* Locally bundled Chart.js and SunEditor replacing Artichow and CKEditor.
* Stronger authorization, administrator-only CSV exports and default administrator password-change enforcement.
* Career Portal CAPTCHA and server-side candidate authentication sessions.
* ISO date support alongside existing US and UK formats.
* Status-history indexes, dependency updates and expanded automated tests.

See the [release notes](https://github.com/opencats/OpenCATS/releases/tag/v0.12.0) for the shipped changes.

## Where to track future work

The [OpenCATS Modernisation & Contributor Roadmap](https://github.com/orgs/opencats/projects/2) is the maintainer planning source. Use it alongside [GitHub issues](https://github.com/opencats/OpenCATS/issues), [pull requests](https://github.com/opencats/OpenCATS/pulls) and [CONTRIBUTING.md](https://github.com/opencats/OpenCATS/blob/master/CONTRIBUTING.md).

Planned recruitment, search, API and dashboard enhancements are not automatically part of the 0.12.0 release. Check the relevant issue and release notes before relying on a feature.
