---
description: >-
  This documentation explains how to install, enhance and use OpenCATS, the free
  open-source applicant tracking system 'opencats' available at opencats.org
cover: .gitbook/assets/opencats-logo.png
coverY: -19.322916666666668
---

# opencats (current release 0.9.7)

### Release information[¶](broken-reference)

The current OpenCATS release is 0.9.6 and the most recent release will always be available at [https://github.com/opencats/opencats/releases](https://github.com/opencats/opencats/releases)

#### Dependencies
The main dependencies for this software are PHP and MariaDB. PHP 7.2 and MariaDB 10.6 are the most recent versions supported. 
**Higher levels may work, but are untested** 

**Note:** MySQL is now unsupported due to some divergence between MariaDB and MySQL. MariaDB is a drop-in replacement for MySQL. Additional utilities (antiword, html2text, unrtf, etc) are necessary for full-text indexing of uploaded resume's. These are not required for installing OpenCATS, and can be added in later.

#### Which package to install?
If you want a simple install, then please check you've downloaded and installed the *-full package on the releases page [https://github.com/opencats/opencats/releases](https://github.com/opencats/opencats/releases) and not the source code or have cloned the master repository. If you've  done either of the last two, you'll need to download and run composer [https://getcomposer.org/)](https://getcomposer.org/) to pick up the dependencies which are already bundled in the -*full package. 

### and finally - a gentle warning

This documentation is coming along nicely, albeit slowly. If you have corrections or requests for anything that should be included in this documentation, please submit a PR to this documentation on github (there's a link in the top right-hand corner of this page!).
