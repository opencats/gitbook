# Roadmap

## Summary

| Version | Feature                                                                                                                                                                                                                                                                                                                                                                                                                                   |    Status   |
| ------: | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :---------: |
|   0.9.2 | Docker                                                                                                                                                                                                                                                                                                                                                                                                                                    |     DONE    |
|   0.9.2 | Continuous Integration                                                                                                                                                                                                                                                                                                                                                                                                                    |     DONE    |
|   0.9.3 | Dependency management                                                                                                                                                                                                                                                                                                                                                                                                                     |     DONE    |
|   0.9.4 | <p><a href="https://github.com/opencats/OpenCATS/pull/206">Display number of candidates in job order pipeline</a><br><a href="https://github.com/opencats/OpenCATS/pull/209">Filled positions cannot hire</a><br><a href="https://github.com/opencats/OpenCATS/pull/210">Job Order Layout for extra fields</a><br><a href="https://github.com/opencats/OpenCATS/pull/212">Job Order pipeline with hot job and submitted job links</a></p> | In progress |
|   0.9.5 | Symfony, Replace Simpletest with Codeception                                                                                                                                                                                                                                                                                                                                                                                              | IN PROGRESS |
|   1.0.0 | First 100% OpenSource release                                                                                                                                                                                                                                                                                                                                                                                                             |   PLANNED   |

## Releases

### 1.0.0

### 0.9.6

### 0.9.5

#### Symfony

#### Replace Simpletest/Behat with Codeception

Migrate Simpletest unit tests and integration tests to PHPUnit, use PHPUnit for further unit testing code The latest [simpletest](http://simpletest.org/) release is from January 23rd, 2012. It's not longer maintained. It's not manageable by Composer. This is against our values and principles:

* We can't focus on writing the code that matters (business logic, product code) as we need to maintain our library for ourselves
* Does not encourage other developers collaboration as it uses legacy stack
* Is not managed by Composer

The solution is to use PHPUnit, which is stable, widely accepted, manageable by composer and plays well with other open source projects such as mocking libraries.

### 0.9.4

#### Content

* [Display number of candidates in job order pipeline](https://github.com/opencats/OpenCATS/pull/206)
* [Filled positions cannot hire](https://github.com/opencats/OpenCATS/pull/209)
* [Job Order Layout for extra fields](https://github.com/opencats/OpenCATS/pull/210)
* [Job Order pipeline with hot job and submitted job links](https://github.com/opencats/OpenCATS/pull/212)

#### Steps for migration from 0.9.3

1. (Draft)
2. Backup
3. Download code
4. How to patch code
5. How to execute DB updates
6. How to verify that's working
7. How to rollback
8. How to submit bugs and get support
