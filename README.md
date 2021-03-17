# Multiregion Deployments of nginx with Packer and Terraform

To better demonstrate immutable deployments, I'm using Packer to bundle-up an AMI which can easily be added to any AWS region as an AMI.  From there, I can use Terraform to deploy my AMI to each region.

## Procedure

Start with bundling up the Packer image:

```
cd images
packer build -var 'region=us-east-1' image.pkr.hcl
```

Once the AMI is created for `us-east-1`, copy the AMI ID and populate the ami field in `../instances/terraform.tfvars`.  Now run Terraform:

```
cd ../instances
terraform init && terraform apply 
```

You can test your deployment by hitting the public IP output by Terraform.  If all is well, you should see the nginx screen.  To deploy to other regions, just update the Packer command and `terraform.tfvars` and re-run the above.
