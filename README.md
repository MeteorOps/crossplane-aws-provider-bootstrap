# crossplane-aws-provider-bootstrap

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