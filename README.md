# crossplane-aws-provider-bootstrap

Prerequisites
- kubectl
- helm
- Existing k8s cluster or kind
- aws account with access key and secret key and sufficient permissions

If using kind:

`kind create cluster`

May need to run this as well:

`kubectl cluster-info --context kind-kind`

If used kind delete the cluster in the end:

`kind delete cluster`

## Article #1

Install crossplane:

`helm repo add crossplane-stable https://charts.crossplane.io/stable`

`helm install crossplane crossplane-stable/crossplane --namespace crossplane-system --create-namespace`

Insert your AWS credentials to the creds file and run the following from the same folder:

`kubectl create secret generic aws-credentials -n crossplane-system --from-file=creds=./creds`

Install aws provider bootstrap:

`kubectl apply -f crossplane-provider-bootstrap.yaml`

Wait for provider to be ready:

`kubectl get provider`

Install aws provider config:

`kubectl apply -f crossplane-provider-conf.yaml`

Create a bucket:

`kubectl apply -f bucket-example.yaml`

Check that bucket is created:

`kubectl get bucket`

Clean, delete the bucket:

`kubectl delete -f bucket-example.yaml`

### Useful Debugging Commands

`kubectl get provider`

`kubectl logs -n crossplane-system deploy/crossplane -c crossplane`

`kubectl logs -n crossplane-system -l pkg.crossplane.io/provider=provider-aws`

## Article #2

Insert your AWS credentials to the creds file and run the following from the same folder:

```bash
kubectl create secret generic aws-creds \
--from-literal=aws_access_key_id=$(grep -i aws_access_key_id creds | awk -F' = ' '{print $2}') \
--from-literal=aws_secret_access_key=$(grep -i aws_secret_access_key creds | awk -F' = ' '{print $2}')
```
Install k8s provider bootstrap:

`kubectl apply -f k8s-provider-bootstrap.yaml`

Wait for k8s provider to be ready:

`kubectl get providers`

Install k8s provider config:

`kubectl apply -f k8s-provider-conf.yaml`

Apply composite app definitions:

`kubectl apply -f composite-app-definitions.yaml`

Create a composite app:

`kubectl apply -f composite-app-example.yaml`

Check that composite app is created:

`kubectl get K8sApplication`

Clean, delete the composite app:

`kubectl delete -f composite-app-example.yaml`
