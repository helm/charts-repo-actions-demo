# Charts Repo Test

This git project tests Helm actions for testing and hosting chart repositories on GitHub Pages.

## Actions

* [@helm/kind-action](https://github.com/scottrigby/kind-action)
* [@helm/chart-testing-action](https://github.com/scottrigby/chart-testing-action)
* [@helm/chart-releaser-action](https://github.com/scottrigby/chart-releaser-action)

## Project Status

Currently `master` tests [apiVersion: v1](https://helm.sh/docs/topics/charts/#the-apiversion-field) charts (installable by both Helm 2 and 3), and not for `apiVersion: v2` charts (installable by Helm 3 only).

## Chart Sources

* `charts/example`: Generated from Helm 2 `helm create charts/example`
* `charts/alpine`: Copied from Helm [testcharts/alpine](https://github.com/helm/helm/tree/master/cmd/helm/testdata/testcharts/alpine)
