communityid-js
==============

This package provides a JavaScript implementation of the open
[Community ID](https://github.com/corelight/community-id-spec)
flow tuple hashing standard.

It targets Node 18 and newer and has no additional dependencies.

[![Tests](https://github.com/corelight/communityid-js/actions/workflows/test.yml/badge.svg)](https://github.com/corelight/communityid-js/actions/workflows/test.yml)

Installation
------------

This package is available [via NPM](https://npmjs.com/package/communityid), therefore:

    $ npm install -g communityid

To install locally from a git clone, you can also use npm, e.g. by saying

    $ npm install /path/to/the/clone

The package works with local or global installation. It ships with an executable
to provide command-line Community ID calculation (see below), for which global
installation is recommended.

Usage
-----

The API is very simple:

    const communityid = require('communityid');
    
    console.log(communityid.calc('tcp', '127.0.0.1', '10.0.0.1', 1234, 80));

This will print "1:mgRgpIZSu0KHDp/QrtcWZpkJpMU=". For details on the arguments
and their types, please see the `calc` function's docstring. Seeding is
supported, and base64 encoding can optionally be disabled. In case of problems
with the input arguments, the function returns `null` and reports an error
message through the package's `communityid.error()` function, which defaults to
`console.error()`.

The package includes a sample application,
[community-id](https://github.com/corelight/communityid-js/blob/master/cli.js),
which calculates the ID for given flow tuples. See its `--help` output
for details. An example:

    $ community-id tcp 10.0.0.1 10.0.0.2 10 20
    1:9j2Dzwrw7T9E+IZi4b4IVT66HBI=

For troubleshooting, the `communityid.calc()` implementation can report the
hashed data to stderr. To enable, set (or add) the `communityid` debug stream in
your `NODE_DEBUG` environment variable:

    $ NODE_DEBUG=communityid community-id tcp 10.0.0.1 10.0.0.2 10 20
    COMMUNITYID 1182249:    seed 00:00
    COMMUNITYID 1182249:   saddr 0a:00:00:01
    COMMUNITYID 1182249:   daddr 0a:00:00:02
    COMMUNITYID 1182249:   proto 06
    COMMUNITYID 1182249: padding 00
    COMMUNITYID 1182249:   sport 00:0a
    COMMUNITYID 1182249:   dport 00:14
    1:9j2Dzwrw7T9E+IZi4b4IVT66HBI=

Testing
-------

The package includes a testsuite in the `test` folder, via Node's test
runner. To execute, run the following from the toplevel or the `test` folder:

    $ node --test
    ✔ icmp 1 (3.838516ms)
    ✔ icmp 2 (0.338138ms)
    ✔ icmp 3 (0.446812ms)
    ...

Browser support
---------------

The package currently does not support browser-side operation. Contributions
very welcome!
