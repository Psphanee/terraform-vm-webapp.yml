# terraform-vm-webapp.yml
 trigger:
 branches:
 include:- main
 pool:
 vmImage: 'ubuntu-latest'
 variables:
 TF_VERSION: '1.6.0'
 steps:- task: UseTerraform@0
 inputs:
 terraformVersion: '$(TF_VERSION)'- script: |
 terraform init
 displayName: 'Terraform Init'
 env:
 ARM_CLIENT_ID: $(ARM_CLIENT_ID)
 ARM_CLIENT_SECRET: $(ARM_CLIENT_SECRET)
 ARM_SUBSCRIPTION_ID: $(ARM_SUBSCRIPTION_ID)
 ARM_TENANT_ID: $(ARM_TENANT_ID)- script: |
 terraform validate
 terraform plan -out=tfplan
 displayName: 'Terraform Plan'- script: |
 terraform apply -auto-approve tfplan
 displayName: 'Terraform Apply'
