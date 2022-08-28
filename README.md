# Managing GCP Infrastructure using Terraform

The purpose of this project is to create an entire GCP infrastructure using Terraform. We will create VPC, subnets, network interfaces, and AMI's using GCP.



## Installation

Install Terraform

```bash
  brew tap hashicorp/tap
```
```bash
  brew install hashicorp/tap/terraform
```

```bash
  brew update
```

```bash
  brew upgrade hashicorp/tap/terraform
```

## Setup

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

1. Select your service account from the list.
2. Select the "Keys" tab.
3. In the drop down menu, select "Create new key".
4. Leave the "Key Type" as JSON.
5. Click "Create" to create the key and save the key file to your system.

## Write configuration

You will now write your first configuration to create a network.

Create a directory for your configuration.
```bash
mkdir learn-terraform-gcp
```

Change into the directory.
```bash
cd learn-terraform-gcp
```

Create a main.tf file for your configuration.
```bash
touch main.tf
```

Open main.tf in your text editor, and paste in the configuration below. Replace <NAME> with the service account key file you created and downloaded and <PROJECT_ID> with your GCP project's ID, and save the file.

```bash
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
```

### Initialize the directory

```bash
terraform init
```

### Format and validate the configuration

```bash
terraform fmt
```

```bash
terraform validate
```

## Create infrastructure

```bash
terraform apply
```

```bash
terraform show
```
# Change Infrastructure

## Create a new resource

```bash
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
```

```bash
terraform apply
```

## Modify configuration

```bash
 resource "google_compute_instance" "vm_instance" {
   name         = "terraform-instance"
   machine_type = "f1-micro"
+  tags         = ["web", "dev"]
   ## ...
 }
 ```

 ```bash
terraform apply
 ```

 ```bash
 Tip: The below snippet is formatted as a diff to give you context about what in your configuration should change. Replace the content displayed in red with the content displayed in green (exclude the leading + and - signs).
 ```

 ```bash
    boot_disk {
     initialize_params {
-      image = "debian-cloud/debian-9"
+      image = "cos-cloud/cos-stable"
     }
   }
   ```

   ```bash
   terraform apply
   ```

   ## Destroy Infrastructure

  ```bash
  terraform destroy
  ```
## Documentation

[Hashicorp Documentation](https://learn.hashicorp.com/collections/terraform/gcp-get-started)


## Authors

- [@johngeorge](https://github.com/johngeorge142/)

