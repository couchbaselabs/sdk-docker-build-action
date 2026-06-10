# FIT Performer Docker Image Conventions

Conventions for naming, tagging, and labeling FIT performer Docker images.

> [!TIP]
> The custom `couchbaselabs/sdk-docker-build-action` GitHub action creates images that follow these conventions.


## Repository

FIT performer images live in the GitHub Container Registry (ghcr.io) for convenient publication and consumption by GHA workflows.

An image should have `internal` visibility (the most permissive allowed by our GitHub policy) so it can be accessed by workflows in any Couchbase organization on GitHub.

An image should be associated with the SDK's GitHub repository; if this is set up correctly, the image appears in the GitHub UI in the repository's "Packages" section (right sidebar) and also as one of the `couchbase` organization's packages at https://github.com/orgs/couchbase/packages?tab=packages&q=*-fit-performer

> [!NOTE]
> The association with the repository is only cosmetic; the real namespace is the GitHub organization.


## Image name and tag

To distinguish FIT performer images from other kinds of images, the image name starts with the SDK's name and ends with `-fit-performer`.

This table shows how different types of performer images should be tagged:

| Image Type            | Image Tag Format                          | Example Tag                 |
|:----------------------|:------------------------------------------|:----------------------------|
| Release               | `major.minor.patch` *(no `v` prefix)*     | `3.12.0`                    |
| Snapshot              | Name of the branch the SDK was built from | `main`, `master`, `3.10.x`  |
| On-Demand (Git Hash)  | `sha-` plus first 12 digits of hash       | `sha-a1b2c3d4e5f6`          |
| On-Demand (GitHub PR) | Full git ref                              | `refs-pull-473-merge`       |
| On-Demand (Gerrit PR) | Full git ref                              | `refs-changes-58-240858-6`  |

> [!NOTE]
> Invalid characters, like the slashes in git refs, are replace by hyphens (-).


## Image metadata

At a minimum, an image should have the following labels as defined in [The OpenContainers Annotations Spec](https://specs.opencontainers.org/image-spec/annotations/?v=v1.1.1):

* `org.opencontainers.image.created` -- Timestamp, useful for ordering snapshot perf results.
* `org.opencontainers.image.revision` -- Full git commit hash the SDK was built from.
* `org.opencontainers.image.source` --  URL of the SDK GitHub repo. The GitHub Container Registry uses this to associate the image with the repository. Example: `https://github.com/couchbase/couchbase-jvm-clients`

If you want to add labels pointing to a pull request or GHA run, please use these label names:

* `com.couchbase.pr` -- URL of the PR the image was built from, if applicable.
* `com.couchbase.github.actions.run` -- URL of the GitHub Actions run that built the image, if applicable.
