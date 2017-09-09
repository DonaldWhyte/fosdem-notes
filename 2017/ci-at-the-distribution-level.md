### Continuous Integration at a Distribution Level

Date: Sat 4th Feb
Time: 15:00

Shepherding 30K Ubuntu packages that never break.

Old distro model -- release without care, break build, freeze release, panic and fix tinhs (e.g. remove packages, etc.) and then release.

New distro model -- devs are responsible for testing the package builds correctly, doesn't break dependencies and so on.

#### `autopkgtest`

`autopkgtest` can be used to provide automated tests in DPKG:

`debian/tests/control` -- enumerates all test drivers for package
`debian/tests/*` -- actual individual test drivers (written in any language, return 0 if success, non-zero otherwise).

Running test:

```
autopkgtest <packageName> --
autopkgtest <directoryToLocalDebiabPackage> -- schroot
autopkgtest <gitHubRepoOrDebURL> -- \ lxd <lxdImageName>
```

You can use containers for isolated builds.

Specify environment restrictions for package tests, e.g.

```
Restrictions: needs-root, isolation-container, isolation-machine
```

e.g. you can ensure the tests runs in its own container (if it needs networking, services, etc.) or its own VM (if it needs its own kernel, only really for low-level packages, e.g. `systemd`).

#### Gating

devel-proposed -> proposed-migrarion -> devel

Package tests and builds occur between `devel-propsoed` and `proposed-migration`? Reject packages if tests fail. All tests run in together, so testing a whole DISTRIBUTION!

Means by the time a package reaches `devel`, we know its own tests pass and its tests pass correctly with ALL other packages on machine.

Auto-generated HTML status pages.

#### Upstream/Downstream Tests

Make sure your tests are not dependent on the debian (or whatever env)'s environment. Ensure the tests can be build just using the upstream source -- allows PR builds, local integration testing and so on.

Means developers already *know* their package works on all environments (providing PR builds test them all) before publishing. Publishing is just a matter of updating the `changelog` (no cumbersome back-and-forth of publishing, receiving error a few mins later, etc).
