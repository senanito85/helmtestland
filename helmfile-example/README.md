# helmfile-example ☸️🔧
Example Helmfile repo for [Kubes&Clouds article](https://kubesandclouds.com/index.php/2020/12/16/helmfile/)



EXAMPLE

repositories:
- name: stable
  url: https://kubernetes-charts.storage.googleapis.com

environments:
prod:
test:
minikube:

helmDefaults:
{{ if eq .Environment.Name "prod" }}
kubeContext: gke_myprodproject_us-west1-a_myprodcluster
{{ end }}
{{ if eq .Environment.Name "test" }}
kubeContext: gke_mydevproject_us-west1-a_mydevcluster
{{ end }}
{{ if eq .Environment.Name "minikube" }}
kubeContext: minikube
{{ end }}

releases:
- name: cert-manager
  namespace: kube-system
  installed: true
  {{ if eq .Environment.Name "prod" }}
  chart: stable/cert-manager
  version: 0.5.0
  {{ end }}
  {{ if eq .Environment.Name "test" }}
  chart: stable/cert-manager
  version: 0.5.1
  {{ end }}
  {{ if eq .Environment.Name "minikube" }}
  chart: stable/cert-manager
  version: 0.5.2
  {{ end }}

#################################

###Update: I've came up with the following but I'd love to see someone come up with something simpler

#####***helmfile.yaml:

environments:
dev:
values:
- env/dev.yaml
prod:
values:
- env/prod.yaml

releases:
- name: some-name
  namespace: some-namespace
  chart: some/thing
  version: {{ index .Environment.Values.version "some-name" }}

env/dev.yaml:

version:
some-name: "0.1"

env/prod.yaml:

version:
some-name: "1.0"

###Please let me know what you think.