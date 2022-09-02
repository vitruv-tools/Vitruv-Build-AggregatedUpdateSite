# Vitruv Aggregated Eclipse Update Site Generation
[![GitHub Action CI](https://github.com/vitruv-tools/Vitruv-Build-AggregatedUpdateSite/actions/workflows/aggregation.yml/badge.svg)](https://github.com/vitruv-tools/Vitruv-Build-AggregatedUpdateSite/actions/workflows/aggregation.yml)
[![Latest Release](https://img.shields.io/github/release/vitruv-tools/Vitruv-Build-AggregatedUpdateSite.svg)](https://github.com/vitruv-tools/Vitruv-Build-AggregatedUpdateSite/releases/latest)
[![Issues](https://img.shields.io/github/issues/vitruv-tools/Vitruv-Build-AggregatedUpdateSite.svg)](https://github.com/vitruv-tools/Vitruv-Build-AggregatedUpdateSite/issues)
[![License](https://img.shields.io/github/license/vitruv-tools/Vitruv-Build-AggregatedUpdateSite.svg)](https://raw.githubusercontent.com/vitruv-tools/Vitruv-Build-AggregatedUpdateSite/main/LICENSE)

## Purpose
This repository is responsible for generating and deploying the aggregated Eclipse update site for Vitruv artifacts.
Each Vitruv repository deploys its generated artifacts to a dedicated update site in the https://github.com/vitruv-tools/updatesite repository. Nightly artifact deployments are then accessible at `https://vitruv-tools.github.io/updatesite/nightly/${REPOSITORY_SUBFOLDER}` with `${REPOSITORY_SUBFOLDER}` being a (custom) path to a folder for a repository. Accordingly, release artifact deployments are accessible at `https://vitruv-tools.github.io/updatesite/release/${REPOSITORY_SUBFOLDER}/${VERSION}` with `${REPOSITORY_SUBFOLDER}` being a (custom) path to a folder for a repository (the same as for the nightly update site of that repository) and `${VERSION}` being the release version.

Those update sites only contain the artifacts generated from one repository. They do not contain the dependencies of those artifacts.
This repository builds an aggregated update site from the individual update sites, which contains all the artifacts of the individual update sites and references to update sites containing the required dependencies. In consequence, the aggregated update site generated in this repository can be used to install any Vitruv artifact with all its dependencies.

## Artifacts
This repository contains three kinds of artifacts:
1. The aggregated update site specification (`nightly.aggr`) with all aggregated artifacts and references to update site providing their dependencies.
2. A Maven build specification that executes the CBI aggregator validating the aggregation and building an aggregated update site from it. It can be run using `mvnw clean package` (using the provided Maven wrapper) to perform a build of nightly artifacts. To build the release artifacts, the update site urls have to be adapted manually, as done in [the GitHub actions](https://github.com/vitruv-tools/Vitruv-Build-AggregatedUpdateSite/blob/main/.github/workflows/aggregation.yml).
3. A GitHub Actions workflow that executes the Maven build and deploys the generated aggregated update site to the https://github.com/vitruv-tools/updatesite repository, which is then available at https://vitruv-tools.github.io/updatesite/nightly/aggregated. Releases can be triggered by creating a GitHub release which are then available at https://vitruv-tools.github.io/updatesite/release/aggregated/${VERSION}.

## Process
The GitHub Actions workflow is executed upon changes to this repository and can also be explicitly dispatched. The latter is, in particular, done whenever new individual update sites are pushed to the https://github.com/vitruv-tools/updatesite repository, such that the aggregated update site is build with the newly added artifacts. Additionally, it is triggered for a release build by creating a GitHub release.
