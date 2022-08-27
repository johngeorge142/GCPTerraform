# GCP Terraform

This project is a tutorial from https://learn.hashicorp.com/tutorials/terraform/infrastructure-as-code?in=terraform/gcp-get-started

»Install Terraform

brew tap hashicorp/tap

brew install hashicorp/tap/terraform

brew update

brew upgrade hashicorp/tap/terraform

terraform -help

terraform -help plan

**Build Infrastructure - Terraform GCP Example**

Set up GCP

After creating your GCP account, create or modify the following resources to enable Terraform to provision your infrastructure:

A GCP Project: GCP organizes resources into projects. Create one now in the GCP console and make note of the project ID. You can see a list of your projects in the cloud resource manager.

Google Compute Engine: Enable Google Compute Engine for your project in the GCP console. Make sure to select the project you are using to follow this tutorial and click the "Enable" button.

A GCP service account key: Create a service account key to enable Terraform to access your GCP account. When creating the key, use the following settings:

Select the project you created in the previous step.
Click "Create Service Account".
Give it any name you like and click "Create".
For the Role, choose "Project -> Editor", then click "Continue".
Skip granting additional users access, and click "Done".
After you create your service account, download your service account key.

Select your service account from the list.
Select the "Keys" tab.
In the drop down menu, select "Create new key".
Leave the "Key Type" as JSON.
Click "Create" to create the key and save the key file to your system.

mkdir learn-terraform-gcp

cd learn-terraform-gcp

touch main.tf

--------------------

terraform {
  required_providers {
    google = {
      source = "hashicorp/google"
      version = "3.5.0"
    }
  }
}

provider "google" {
  credentials = file("<NAME>.json")

  project = "<PROJECT_ID>"
  region  = "us-central1"
  zone    = "us-central1-c"
}

resource "google_compute_network" "vpc_network" {
  name = "terraform-network"
}
  
 --------------------

terraform init
  
terraform fmt
  
terraform validate
  
terraform apply
  
terraform show
 
---------------------------

**Change Infrastructure**
 
Add the following configuration for a Google compute instance resource to main.tf.
  
-------------------------

resource "google_compute_instance" "vm_instance" {
  name         = "terraform-instance"
  machine_type = "f1-micro"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-9"
    }
  }

  network_interface {
    network = google_compute_network.vpc_network.name
    access_config {
    }
  }
}

----------------------------

terraform apply
  
**Modify configuration**

In addition to creating resources, Terraform can also make changes to those resources.

Add a tags argument to your vm_instance resource block.

Tip: The below snippet is formatted as a diff to give you context about what in your configuration should change. Add the content in green (exclude the leading + sign).
 resource "google_compute_instance" "vm_instance" {
   name         = "terraform-instance"
   machine_type = "f1-micro"
+  tags         = ["web", "dev"]
   ## ...
 }

  ------------------
  
  
terraform apply
  
terraform destroy
