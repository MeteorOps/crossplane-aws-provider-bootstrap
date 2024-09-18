# crossplane-aws-provider-bootstrap

### Deploy Crossplane

`helm repo add crossplane-stable https://charts.crossplane.io/stable`

`helm install crossplane crossplane-stable/crossplane --namespace crossplane-system --create-namespace`

### Deploy the AWS Provider

Insert your AWS credentials to the creds file and run the following from the same folder:

`kubectl create secret generic aws-credentials -n crossplane-system --from-file=creds=./creds`

Install aws provider bootstrap:

`kubectl apply -f crossplane-provider-bootstrap.yaml`

Wait for provider to be ready:

`kubectl get provider`

Install aws provider config:

`kubectl apply -f crossplane-provider-conf.yaml`

### Create an AWS Resource using Crossplane (S3 Bucket Example)

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
