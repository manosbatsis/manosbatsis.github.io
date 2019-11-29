
# Corda Wishlist

A few things I'd like to see or develop around Corda.

<!-- TOC depthFrom:2 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Developer Experience](#developer-experience)
	- [Artifact Versioning and Publishing](#artifact-versioning-and-publishing)
		- [Use Snapshot Qualifiers](#use-snapshot-qualifiers)
		- [Provide Daily Snapshots](#provide-daily-snapshots)
		- [Guarantee Reproducible Builds](#guarantee-reproducible-builds)
		- [Versioning Strategy](#versioning-strategy)
	- [Cordapp Conventions](#cordapp-conventions)
		- [Standardise Configuration](#standardise-configuration)
		- [Flow APIs Consistency](#flow-apis-consistency)
- [Development Ideas](#development-ideas)

<!-- /TOC -->

## Developer Experience

### Artifact Versioning and Publishing

Help the Corda community's efforts to help Corda. There's plenty
of people trying to work with and provide feedback on the master branch
of Corda repos like corda-solutions, corda-accounts, tokens-sdk and so on.
Currently artifact releases are extremely inconsistent and break maven/gradle
and java/kotlin community conventions. Here's how to fix that.

#### Use Snapshot Qualifiers

Maven/Gradle versions can use the `-SNAPSHOT` qualifier to signify that a project is
currently under active development. When Corda developers depend on a Corda component
that is under active development, they can depend on a SNAPSHOT release,
and Maven/Gradle will periodically attempt to download the latest snapshot from a repository
when they run a build.

Similarly, if the next release of a Corda component is going to have a version "1.4",
a version "1.4-SNAPSHOT" should be used on the master or development branch
until it was formally released.

Use the SNAPSHOT qualifier during development of a release. This helps maven and gradle
understand they need to check for an updated artifact with the same version.


#### Provide Daily Snapshots

Setup CI to provide daily SNAPSHOT releases for all public Corda repositories. This will help
the community test a Corda library under development in their projects and provide feedback and fixes.

To clarify, the alternative for the individual developer is to manually clone, build and install the current
master locally or in a private repository, which requires too much effort.

#### Guarantee Reproducible Builds

Corda projects some times make multiple releases using the exact same version (e.g. 1.1-RC1)
to "emulate" what normally SNAPSHOT releases are for. This breaks builds by making them not
reproducible and confuses Maven/Gradle, as they that think the version is already installed locally.

This results in users submitting issues about missing classes or other API changes simply because
Corda teams break build tool conventions with their releases.

#### Versioning Strategy

Enforce a minimal versioning strategy troughout Corda repos where a minor version change does not
change the API or is backwards compatible. Increment major versions for incompatible API changes.
This will help Corda developers maintain dependencies in their projects.

### Cordapp Conventions

#### Standardise Configuration

Standardise `CordappContext.config` throughout cordapps provided by R3, see for example
https://github.com/corda/corda-solutions/issues/137

#### Flow APIs Consistency

Adopt the convention developed by the Tokens SDK, where the same functionality is available
in layers as appropriate:

- `StartableByRPC`/`InitiatingFlow` flow versions that can be used as is, i.e. with their own sessions and `TransactionBuilder`
- Inline flows that accept and reuse sessions but probably create their own `TransactionBuilder`
- Utility methods that accept a `TransactionBuilder` to add the appropriate items

## Development Ideas

TBD
