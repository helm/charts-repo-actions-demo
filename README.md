# Charts Repo Actions Demo

[![](https://github.com/helm/charts-repo-actions-demo/workflows/Lint%20and%20Test%20Charts/badge.svg)](https://github.com/helm/charts-repo-actions-demo/actions)
[![](https://github.com/helm/charts-repo-actions-demo/workflows/Release%20Charts/badge.svg?branch=master)](https://github.com/helm/charts-repo-actions-demo/actions)

Example project to demo testing and hosting a chart repository with GitHub Pages and Actions.

## Actions

* [@helm/kind-action](https://github.com/helm/kind-action)
* [@helm/chart-testing-action](https://github.com/helm/chart-testing-action)
* [@helm/chart-releaser-action](https://github.com/helm/chart-releaser-action)

## Project Status

Currently `master` tests [apiVersion: v1](https://helm.sh/docs/topics/charts/#the-apiversion-field) charts (installable by both Helm 2 and 3), and not for `apiVersion: v2` charts (installable by Helm 3 only).

## Chart Sources

* `charts/example`: Generated from Helm 2 `helm create charts/example`
* `charts/alpine`: Copied from Helm [testcharts/alpine](https://github.com/helm/helm/tree/master/cmd/helm/testdata/testcharts/alpine)

## How-To

You can automatically test and host your own chart repository with GitHub Pages and Actions by following these steps.

### Prerequisites

* A GitHub project to use for your Charts repo (a clean project is most straightforward, as there won't be release cluttering or possible `gh-pages` conflicts)
* A branch to use for GitHub Pages (`gh-pages` is most straightforward, as it's the default and requires no configuration)
* A project [Secret](https://help.github.com/en/actions/automating-your-workflow-with-github-actions/creating-and-using-encrypted-secrets#creating-encrypted-secrets) named `CR_TOKEN` with the value of a GitHub [personal access token](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line#creating-a-token)
  * The token must have `repo` scope
  * The token's user must have write access to the project
  * To mitigate risk you may wish to limit the token to a single project by creating a [machine user](https://developer.github.com/v3/guides/managing-deploy-keys/#machine-users)
  * Please note the personal access token is required because of an [Actions bug](https://github.community/t5/GitHub-Actions/Github-action-not-triggering-gh-pages-upon-push/m-p/31266/highlight/true#M743), and will hopefully be unnecessary in the future

### Steps

The above [prerequisites](#prerequisites) _must_ be complete before the steps below, or your charts' initial versions won't be released.

1. Use the `master` branch for all of the below, if you wish to use the Actions workflow files as-is
1. Copy the `.github/workflows` files from this project to yours
1. Add your charts to a parent directory in the project (`/charts` is most straightforward, as it's the default. To change this see [helm/chart-testing > configuration > chart-dirs](https://github.com/helm/chart-testing#configuration))
1. Optional: To list your charts repo publicly on the [Helm Hub](https://hub.helm.sh), see [Helm Hub > How To Add Your Helm Charts](https://github.com/helm/hub#how-to-add-your-helm-charts)

### Results

* The [Lint and Test Charts](/.github/workflows/lint-test.yaml) workflow uses [@helm/kind-action](https://www.github.com/helm/kind-action) GitHub Action to spin up a [kind](https://kind.sigs.k8s.io/) Kubernetes cluster, and [@helm/chart-testing-action](https://www.github.com/helm/chart-testing-action) to lint and test your charts on every Pull Request and push
* The [Release Charts](/.github/workflows/release.yaml) workflow uses [@helm/chart-releaser-action](https://www.github.com/helm/chart-releaser-action) to turn your GitHub project into a self-hosted Helm chart repo. It does this – during every push to `master` – by checking each chart in your project, and whenever there's a new chart version, creates a corresponding [GitHub release](https://help.github.com/en/github/administering-a-repository/about-releases) named for the chart version, adds Helm chart artifacts to the release, and creates or updates an `index.yaml` file with metadata about those releases, which is then hosted on GitHub Pages
* You should now be able to add your charts repo with `helm repo add <owner> https://<owner>.github.io/<project>`
