[![License](http://img.shields.io/:license-apache-blue.svg)](http://www.apache.org/licenses/LICENSE-2.0.html) [![Build Status](https://travis-ci.org/simp/pupmod-simp-named.svg)](https://travis-ci.org/simp/pupmod-simp-named) [![SIMP compatibility](https://img.shields.io/badge/SIMP%20compatibility-4.2.*%2F5.1.*-orange.svg)](https://img.shields.io/badge/SIMP%20compatibility-4.2.*%2F5.1.*-orange.svg)

# named (BIND)


#### Table of Contents
1. [Module Description - What the module does and why it is useful](#module-description)
2. [Setup - The basics of getting started with named](#setup)
    * [What named affects](#what-named-affects)
    * [Setup requirements](#setup-requirements)
    * [Beginning with named](#beginning-with-named)
3. [Usage - Configuration options and additional functionality](#usage)
4. [Reference](#reference)
5. [Limitations - OS compatibility, etc.](#limitations)
6. [Development - Guide for contributing to the module](#development)
7. [Acceptance Tests](#acceptance-tests)


## Module Description

Installs, Configures and Manages a named service.

Options are available for caching and non-caching servers, and the choice
between placing named in chroot or non_chroot with selinux enabled.

### Caching

`simp/named` allows both the building of a non-caching named server via the
`named` class or a caching server utilizing the `named::caching` class.

### Chroot

This module will place named in a chroot at /var/named/chroot by default, but
can be overrided and selinux enforced by adding a `selinux_enforced` variable to
true in hiera or at the global variable level in the Puppet Console.

## Setup

Install `simp/named` to your modulepath. A SIMP rsync server must also be in
place to use the named module.

### What named affects

`simp/named` manages the bind packages, named services, named user/group,
named.conf, and the named directory and contents.

### Begging with named

To setup the basic named server in chroot:

```puppet
  class {'named':
    rsync_server => 'my.rsync.server',
  }
```

## Usage

### I want to use an selinux based named server not in chroot

Add the following to your Hiera File:

```
---
selinux_enforced: true
```

OR

Add selinux_enforced = true to the PE Console at the node or global level.

### I want to make a caching named server

```puppet
  class {'named::caching':
    rsync_server => 'my.rsync.server',
  }
```

## Reference

### Public Classes
* [named](https://github.com/simp/pupmod-simp-named/blob/master/manifests/init.pp): Main class for named.
* [named::chroot](https://github.com/simp/pupmod-simp-named/blob/master/manifests/chroot.pp): This class configures named in a chroot jail for execution on a system.
* [named::install](https://github.com/simp/pupmod-simp-named/blob/master/manifests/install.pp): Installs appropriate backages for BIND.
* [named::non_chroot](https://github.com/simp/pupmod-simp-named/blob/master/manifests/non_chroot.pp): This class configures named for execution on a system using seliunx.
* [named::service](https://github.com/simp/pupmod-simp-named/blob/master/manifests/service.pp): Manages named service.
* [named::caching](https://github.com/simp/pupmod-simp-named/blob/master/manifests/caching.pp): Configures a caching name server
* [named::hints](https://github.com/simp/pupmod-simp-named/blob/master/manifests/hints.pp): Determines what to add to named hints file.

### Private Classes
* [named::params](https://github.com/simp/pupmod-simp-named/blob/master/manifests/params.pp): Parameters for named.

### Defined Types
* [named::caching::forwarders](https://github.com/simp/pupmod-simp-named/blob/master/manifests/caching/forwarders.pp): Adds forwarders entries to your caching nameserver.

## Limitations

SIMP Puppet modules are generally intended to be used on a Red Hat Enterprise
Linux-compatible distribution.

## Development

Please read our [Contribution Guide](https://simp-project.atlassian.net/wiki/display/SD/Contributing+to+SIMP)
and visit our [Developer Wiki](https://simp-project.atlassian.net/wiki/display/SD/SIMP+Development+Home)

If you find any issues, they can be submitted to our
[JIRA](https://simp-project.atlassian.net).


## Acceptance tests

To run the system tests, you need `Vagrant` installed.

You can then run the following to execute the acceptance tests:

```shell
   bundle exec rake beaker:suites
```

Some environment variables may be useful:

```shell
   BEAKER_debug=true
   BEAKER_provision=no
   BEAKER_destroy=no
   BEAKER_use_fixtures_dir_for_modules=yes
```

*  ``BEAKER_debug``: show the commands being run on the STU and their output.
*  ``BEAKER_destroy=no``: prevent the machine destruction after the tests
   finish so you can inspect the state.
*  ``BEAKER_provision=no``: prevent the machine from being recreated.  This can
   save a lot of time while you're writing the tests.
*  ``BEAKER_use_fixtures_dir_for_modules=yes``: cause all module dependencies
   to be loaded from the ``spec/fixtures/modules`` directory, based on the
   contents of ``.fixtures.yml``. The contents of this directory are usually
   populated by ``bundle exec rake spec_prep``. This can be used to run
   acceptance tests to run on isolated networks.
