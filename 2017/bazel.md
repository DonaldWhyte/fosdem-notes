### Bazel Notes

Date: Sat 4th Feb
Time: 14:00

Bazel is optimised for coee in a single large mono-repository -- 10^7 files.

This means it makes assumptions about which dependencies have Bazel support.

What It's good for:

* aggressive caching without losing correctness
    - required for large mono-repos
* caching ensures that the final binaries/artefacts are byte-by-byte identical
* declarative style of BUILD files
    - separates concerns? how to build vs. compilation strategy for optimisation?

How does it work?

* load all BUILD files needed
* construct dependency graph of targets in all BUILD fiels
* generate action graph for deps/rules
* execute action graph (not executing cached nodes)

On subsequent builds, update the graphs. Keeps graph in memory, so the whole graph doesn't need to be rebuilt on every build. Only updates the subtree needed.

Bazel is client-server. Local server on machine caches the graph in memory.

What is needed in a repo:

* WORKSPACE file -- 
    - empty for simple repos
    - this sets up CC link optinos, host/target arch, etc.
    - also brings in the wider environment, e.g. standard repos in github, etc.
* BUILD files (one in each directory, one in each subdir, etc.)
    - targets, libs, srcs, deps, etc.
    - NOT CC link options, host/target arch -- this concern is separated OUTSIDE of the build/dep graph declarations
* Special BUILD files (e.g. `gmock.BUILD`) can be used to build external un-BAZELLED deps retrieved from tars/github repos

To properly deduce which actions are required, it needs to know ALL inputs/outputs used. This means all system/external deps need to be declared! NO implicitly pulled in shared objects, libraries that the dev doesn't realise they're using when build locally.

Bazel has a "sandbox" tool to make it correct build files. They build files in isolated environments, ensuring all inputs and outputs are known.

^--- full knowledge ensures correctnes, caching, remote execution, parallelism

This also allows exection of build on a remote environment. You can use a build cluster for faster execution. This even allows for **shared caches**!

#### Extending Bazel

Built-in rules for certain languages.

Generic rules for arbitrary building -- use `genrule`.

However, using custom specialised rules for every language doesn't scale. Instead, we need to extend the BUILD language.

We can use Python-like Skylark language for this. However, it's restriced to a simple core set of features. This removes global state and complicated that make deterministic builds and caching difficult/incorrect.

Load extensions using `load()` in BUILD file. `.bzl` extension for extensions.

#### Creating Bazel Packages

Currently support for tar and debian packages. It's possible to extend, so could be used to package Python packages and so on.  

#### Open-sourcing Bazel

Problems open sourcing:

* hard-coded paths
* tailored towards a single repo, but multiple (which is how msot other orgs work)
* tailored towards Google's specific languages (e.g. Python, C++, Java, etc.)
* dependencies have to be open-sourced

#### Roadmap

1.0 not here yet. Will likely be 2018.

Goals include stable build language and APIs, remote execution APIs (for shared caching, build farms, etc.)

More community projects w/ Skylark rules, so Bazel becomes a more language agnostic tool.
