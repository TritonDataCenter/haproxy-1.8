# HAProxy

This repository represents the version of [haproxy](https://www.haproxy.org/)
that is used as part of the [Manta project](https://github.com/joyent/manta).

The changes specific to this repo are:

- backport of event port poller implementation
- work-around for spurious event port wakeups breaking thread sync
- implementation of max-old-workers

## Repository Management

To better understand and maintain our differences from HAProxy, we try to
manage branches and tags in a specific fashion. First and foremost, all
branches and tags from the upstream HAProxy repository are mirrored here.
Anything that is Joyent-specific begins with a `joyent/` prefix.

Branches with Joyent modifications are named `joyent/<version>`, such as
`joyent/v1.8.8`. This is a branch that starts from the HAProxy
`v1.8.8` tag. These branches will have all of our patches
rebased on top of them. Currently, this repository is consumed by
`muppet`, which contains a git submodule for this repository.

When it comes time to update to a newer version of HAProxy, we would take
the following steps:

* Ensure that we have pushed all changes from the upstream repository and
  synced all of our branches and tags.
* Identify the release tag that corresponds to the point release. For
  this example, we'll say that's `v1.12.3`.
* Create a new branch named `joyent/<version>` from the tag. In this
  case we would name the branch `joyent/v1.12.3`.
* Rebase all of our patches on to that new branch, removing any patches
  that are no longer necessary.
* Test the new HAProxy binary.
* Review and Commit all relevant changes.
* Update the [muppet](https://github.com/joyent/muppet)
  submodule to point to the branch.

## Building and testing

As mentioned, this version of haproxy is built and used via
[muppet](https://github.com/joyent/muppet). However, for testing purposes, a
local build will suffice:

```
make -j TARGET=solaris
```

The resulting binary can be placed in the `loadbalancer` zone.

For more general details on haproxy, see the README.
