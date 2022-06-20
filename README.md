# Vitruv Aggregated Update Site Generation

## Purpose
This repository is responsible for generating and deploying the aggregated update site for Vitruv artifacts.
Each Vitruv repository deploys its generated artifacts to a dedicated update site in the `https://vitruv-tools.github.com/updatesite` repository. Nightly artifact deployments are then accessible at `https://vitruv-tools.github.io/updatesite/nightly/${REPOSITORY_SUBFOLDER}` with `${REPOSITORY_SUBFOLDER}` being a (custom) path to a folder for a repository. Accordingly, release artifact deployments are accessible at `https://vitruv-tools.github.io/updatesite/release/${REPOSITORY_SUBFOLDER}/${VERSION}` with `${REPOSITORY_SUBFOLDER}` being a (custom) path to a folder for a repository (the same as for the nightly update site of that repository) and `${VERSION}` being the release version.

Those update sites only contain the artifacts generated from one repository. They do not contain the dependencies of those artifacts.
This repository builds an aggregated update site from the individual update sites, which contains all the artifacts of the individual update sites and references to update sites containing the required dependencies. In consequence, the aggregated update site generated in this repository can be used to install any Vitruv artifact with all its dependencies.

# Artifacts
This repository contains three kinds of artifacts:
1. The aggregated update site specification (`nightly.aggr`) with all aggregated artifacts and references to update site providing their dependencies
2. A Maven build specification that executes the CBI aggregator validating the aggregation and building an aggregated update site from it. It can be run using `mvnw clean package` (using the provided Maven wrapper) to perform a build of nightly artifacts and using `mvnw clean package -Prelease` to create a build of the latest release artifacts.
3. A GitHub Actions workflow that executes the Maven build and deploys the generated aggregated update site to the `https://vitruv-tools.github.com/updatesite` repository, which is then available at `https://vitruv-tools.github.io/updatesite/nightly/aggregated`. Currently, no release builds are supported by the GitHub Actions workflow (see #1).

# Process 
The GitHub Actions workflow is executed upon changes to this repository and can also be explicitly dispatched. The latter is, in particular, done whenever new individual update sites are pushed to the `https://vitruv-tools.github.com/updatesite` repository, such that the aggregated update site is build with the newly added artifacts.
