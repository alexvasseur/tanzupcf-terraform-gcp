# VMware Tanzu Application Service PCF on GCP

## What it is

- Terraforming PCF on GCP (2.11) per the doc
- will not create OpsManager - this has to be done manually per doc
- specific firewall allowing DNS UDP 53 for Redis Enterprise cluster and tile

## How To

- Create `terraform.tfvars` 
- Generate SSL cert and key with the PCF wildcard domains and use those so that the GCP LB is the SSL endpoint

## Limitations

DNS for Redis Cluster in the tile is not done thru this Terraform.
You would need to do per Redis Labs docs.

## TODO picture





# OLD NOTES FROM UPSTREAM

# GCP

Follow [these instructions](https://docs.pivotal.io/platform/ops-manager/2-8/gcp/prepare-env-terraform.html)
to create the service account that is needed to run the terraform templates.


### Prerequisites
- a pre-created zone in GCP (see the [google docs](https://cloud.google.com/dns/docs/zones#creating_managed_zones) for details)
    - the `Zone name` maps to `hosted_zone` in `terraform.tfvars`
    - The Fully Qualified Domain Name (FQDN) for the environment will be `environment_name.hosted-zone`

### Roles & Permissions

If you are looking to create a service account with more restrictive permissions,
you can follow these instructions.

The roles required:
- Compute Instance Admin (v1) - `compute.instanceAdmin`
- Compute Network Admin - `compute.networkAdmin`
- Compute Security Admin - `compute.securityAdmin`
- DNS Administrator - `dns.admin`
- Security Admin - `iam.securityAdmin`
- Service Account Admin - `iam.serviceAccountAdmin`
- Service Account Key Admin - `iam.serviceAccountKeyAdmin`
- Storage Admin - `storage.admin`
- Service Account User - `iam.serviceAccountUser`

For each role, you can run the following command:

```console
gcloud projects add-iam-policy-binding $PROJECT_ID --member serviceAccount:$SERVICE_ACCOUNT_EMAIL --role "roles/$ROLE"
```

To understand the mapping between permissions and roles, you can see [this document](https://cloud.google.com/iam/docs/understanding-roles).
