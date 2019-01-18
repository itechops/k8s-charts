# kubernetes-charts

Helm is the package manager for Kubernetes. As an operating system package manager makes it easy to install tools on an OS, Helm makes it easy to install applications and resources into Kubernetes clusters.

* [Getting started guide](https://github.com/kubernetes/helm/blob/master/docs/chart_template_guide/getting_started.md)
* [Using Helm](https://github.com/kubernetes/helm/blob/master/docs/using_helm.md)
* [Blog post on kubernetes.io](http://blog.kubernetes.io/2016/10/helm-charts-making-it-simple-to-package-and-deploy-apps-on-kubernetes.html)

## Updating this repository
This is setup using github pages. It serves the contents of the `/docs` folder.

To add or update a chart, do the following:

```
$ helm create <chart-name>
$ helm package <chart-name>
$ mv <chart-name>-0.1.0.tgz docs
$ helm repo index docs --url https://itech.github.com/kubernetes-charts
$ git add -i
$ git commit -av
$ git push origin master
```

From there, I can do:
```
helm repo add itech https://itech.github.com/kubernetes-charts
helm install itech/<chart-name>
```


To release a new version of `generic-app`:
* Edit `charts/generic-app/Chart.yaml`, update version number
* `cd charts`
* `helm package generic-app`
* `mv generic-app-X.Y.Z.tgz ../docs/`
* `cd ..`
* `helm repo index docs --url https://itech.github.com/kubernetes-charts`
* Commit and push to the repo
* Tag the release `git tag vX.Y.X`
* Push tags `git push --tags`
* Deploy to Google Page could take some time
* Update your local repo `helm repo update`

## Customizing values and installing a chart

First step is to copy the `values.yaml` from the chart you're going to customize to a location
and customize the values. I.e. Ports, image, etc

First make sure that your chart is up to date
`helm repo update`

To install that chart
`helm install -f values.yaml --name boilerplate-api-dev --namespace boilerplate-dev itech/generic-app`

Once that is installed you can make changes to that chart, first edit the values.yaml

`helm upgrade -f values.yaml boilerplate-api-dev itech/generic-app`

You can list the charts using `helm list`
You can get the values for any existing chart using `helm get values <chartname>`

Any helm install or upgrade command takes a `--version` flag. This allows you to specify a specific chart version.
