# Self-Managed Qovery Helm Chart for GCP

Requirements 
- Having a GKE cluster using the default config and in particular: Public Cluster (we didn't test private but it should work if you have a NAT GW?), kube version <= 1.28

Steps to install:
- Retrieve Kubeconfig with the command gcloud container clusters get-credentials CLUSTER_NAME --region=CLUSTER_REGION
- Create a cluster in Qovery with GCP credentials: ("roles/artifactregistry.admin" "roles/artifactregistry.writer" "roles/compute.networkAdmin" "roles/container.admin" "roles/container.clusterAdmin" "roles/artifactregistry.createOnPushRepoAdmin" "roles/iam.serviceAccountUser" "roles/source.admin" "roles/storage.admin")
- Retrieve the values from the qovery console (the one you get at the end of the flow)
- Pull this repository locally (note, this repository is temporary!)
- edit the qovery-chart-gcp/charts/qovery/values-demo-gcp.yaml by adding the values retrieved from the qovery console (remember to update acmeEmailAddr with your email)
- Enter the folder qovery-chart-gcp/charts/qovery/ and launched these two helm commands:

`helm upgrade --install --debug --wait --create-namespace -n qovery -f values-demo-gcp.yaml --set services.certificates.cert-manager-configs.enabled=false,services.certificates.qovery-cert-manager-webhook.enabled=false --set services.qovery.qovery-cluster-agent.enabled=false qovery .`

`helm upgrade --install --debug --wait --create-namespace -n qovery -f values-demo-gcp.yaml qovery .`
