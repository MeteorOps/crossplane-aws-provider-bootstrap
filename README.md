# crossplane-aws-provider-bootstrap

Step-by-step guide and files to deploy Crossplane on AWS EKS and start using it to deploy AWS resources.  
This repository is used by the ["Deploy AWS Resources using Crossplane on Kubernetes"](https://www.meteorops.com/blog/deploy-aws-resources-using-crossplane-on-kubernetes) blog post.

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

Apply composite bucket definitions:

`kubectl apply -f bucket-definitions.yaml`

Apply composite bucket crd:

`kubectl apply -f bucket-crd.yaml`

### Create an AWS Resource using Crossplane (S3 Bucket Example)

Create a bucket:

`kubectl apply -f bucket-example.yaml`

Check that bucket is created:

`kubectl get mys3bucket`

or

`kubectl get bucket`

Clean, delete the bucket:

`kubectl delete -f bucket-example.yaml`

### Useful Debugging Commands

`kubectl get provider`

`kubectl logs -n crossplane-system deploy/crossplane -c crossplane`

`kubectl logs -n crossplane-system -l pkg.crossplane.io/provider=provider-aws`
