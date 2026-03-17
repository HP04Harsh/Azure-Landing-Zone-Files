Azure Landing Zone Automation Framework
=======================================

A streamlined, event-driven DevOps solution for automating **Azure Landing Zone** deployments. This project connects business users to cloud infrastructure by converting **Power App** inputs into **Terraform** configurations via **Power Automate**, providing a true "No-Code to Infrastructure-as-Code" experience.

📖 Project Description
----------------------

The goal of this project is to simplify cloud resource provisioning for non-technical users. Instead of writing complex HCL code, users interact with a simple interface. The backend logic handles the conversion of these requirements into a machine-readable format that Terraform can immediately deploy to Microsoft Azure.

🏗 Architecture Overview
------------------------

The workflow is entirely automated and follows these core stages:

1.  **Intake**: Users provide details (VM size, Region, Department) in a **Power App**.
    
2.  **Processing**: **Power Automate** captures the response and generates a .tfvars.json file.
    
3.  **Delivery**: The JSON file is pushed directly to this GitHub repository.
    
4.  **Deployment**: A **GitHub Action** monitors the repo, detects the new file, and runs terraform apply.
    
5.  **Completion**: Upon a successful build, an automated **Email Notification** is sent to the requester.
    
📂 Project Structure
--------------------
```Azure-Landing-Zone-Files/
├── .github/workflows/      # GitHub Actions automation scripts
│   └── deploy.yml          # Core logic for running Terraform on push
├── terraform/              # Infrastructure code directory
│   ├── main.tf             # Azure Resource definitions
│   ├── variables.tf        # Variable declarations
│   └── requests/           # Target folder for Power Automate JSON files
└── README.md               # Project documentation
```

🚦 Prerequisites
----------------

*   **Azure Subscription**: Access to a tenant where resources will be created.
    
*   **GitHub Personal Access Token (PAT)**: Used by Power Automate to push files.
    
*   **Terraform CLI**: Required for local testing.
    
*   **Power Platform Account**: To host the Power App and Flow.
    

🚀 The Workflow (Step-by-Step)
------------------------------

1.  **User Submission**: User opens the Power App and submits a request.
    
2.  **Power Automate Trigger**: The flow starts, parses the dynamic data, and formats it as JSON.
    
3.  **GitHub Commit**: Power Automate uses the GitHub API to create a new file in /terraform/requests/.
    
4.  **Auto-Init**: GitHub Actions detects the commit and runs terraform init.
    
5.  **Apply**: The pipeline runs terraform apply -var-file="requests/new\_file.json".
    
6.  **Email**: A final API call sends a "Deployment Successful" email with resource details.
    

💻 Sample Commands
------------------

**Local Initialization:**
```
cd terraform
terraform init
```

**Testing a Generated JSON File:**
```
terraform plan -var-file="requests/powerapp_input_123.json"
```

🛠 Troubleshooting
------------------

*   **Permission Denied**: Check if the GitHub PAT has repo write access.
    
*   **Validation Errors**: Ensure the Power App dropdowns match the allowed values in variables.tf.
    
*   **State Locks**: If two users submit simultaneously, the GitHub Action might fail. Ensure "concurrency" is set in your workflow file.

 <img width="1408" height="768" alt="image" src="https://github.com/user-attachments/assets/3f30822d-2c4b-4183-81af-588c44b6ce1e" />
   

👥 Contributors
---------------

*   **Harsh Pardhi** ([HP04Harsh](https://github.com/HP04Harsh)) - DevOps Engineer
