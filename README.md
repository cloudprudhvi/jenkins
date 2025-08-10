# Jenkins Installation and Setup on Ubuntu EC2

This guide will walk you through installing Jenkins on an Ubuntu EC2 instance, configuring it, and running your first Jenkins job to clone a repository and initialize Terraform.

## Prerequisites
- An AWS EC2 instance running Ubuntu (20.04/22.04/24.04 LTS)
- SSH access to the instance
- `sudo` privileges

## Jenkins Dependencies
Jenkins requires:
- Java (OpenJDK 11 or 17 recommended)
- `wget` and `curl` (for downloading packages)
- `git` (for cloning repositories)

## Jenkins Installation Steps


## Jenkins Installation Steps

Use the following commands to install Java and Jenkins:

1. **Update the system:**
   ```bash
   sudo apt update
   ```

2. **Install Java (default JRE and JDK):**
   ```bash
   sudo apt install default-jre
   sudo apt install default-jdk
   ```

3. **Add Jenkins repository and key:**
   ```bash
   wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo gpg --dearmor -o /usr/share/keyrings/jenkins.gpg
   sudo sh -c 'echo deb [signed-by=/usr/share/keyrings/jenkins.gpg] http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
   sudo apt update
   ```

4. **Install Jenkins:**
   ```bash
   sudo apt install jenkins
   ```

1. **Update the system:**
   ```bash
   sudo apt update
   ```

2. **Install Java (default JRE and JDK):**
   ```bash
   sudo apt install default-jre
   sudo apt install default-jdk
   ```

3. **Add Jenkins repository and key:**
   ```bash
   wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo gpg --dearmor -o /usr/share/keyrings/jenkins.gpg
   sudo sh -c 'echo deb [signed-by=/usr/share/keyrings/jenkins.gpg] http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
   sudo apt update
   ```

4. **Install Jenkins:**
   ```bash
   sudo apt install jenkins
   ```

## Jenkins Default Port
- Jenkins runs on port **8080** by default.
- Make sure to allow inbound traffic on port 8080 in your EC2 security group.

## Access Jenkins
- Open your browser and go to: `http://<EC2-PUBLIC-IP>:8080`

## Initial Setup
1. **Unlock Jenkins:**
   - Get the initial admin password:
     ```bash
     sudo cat /var/lib/jenkins/secrets/initialAdminPassword
     ```
   - Copy and paste this password into the Jenkins web UI.

2. **Install Suggested Plugins:**
   - Choose "Install suggested plugins" when prompted.

3. **Create Admin User:**
   - Set up your first admin user as prompted.

## What is a Jenkins Job?
A Jenkins job is a task or set of tasks (build, test, deploy, etc.) that Jenkins runs. Jobs can be configured to run automatically or manually.

### Freestyle Job Example: Clone a Repo and Run Terraform Init

1. **Create a New Freestyle Job:**
   - Go to Jenkins dashboard > "New Item"
   - Enter a name (e.g., `terraform-init-job`)
   - Select "Freestyle project" and click OK

2. **Configure Source Code Management:**
   - Select "Git"
   - Enter your repository URL

3. **Add Build Step:**
   - Choose "Execute shell"
   - Enter:
     ```bash
     terraform init
     ```

4. **Save and Build the Job:**
   - Click "Save"
   - Click "Build Now"

#### Why Does It Fail?
- The job will fail with an error like `terraform: command not found` because Terraform is not installed on the Jenkins server.

## Installing Terraform
1. **Install Terraform:**
   ```bash
   sudo apt-get update && sudo apt-get install -y gnupg software-properties-common curl
   curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
   echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
   sudo apt update && sudo apt install -y terraform
   terraform -version
   ```

2. **Re-run the Jenkins Job:**
   - Go back to Jenkins
   - Click on your job > "Build Now"
   - The job should now succeed and initialize Terraform in your repo.

---

## Summary
- Jenkins is a powerful automation server.
- It runs on port 8080 by default.
- The initial admin password is required for first login.
- Always install suggested plugins for a basic setup.
- Jenkins jobs automate tasks; a freestyle job can run any shell command.
- If a job fails due to missing tools (like Terraform), install them on the Jenkins server and re-run the job.

For more details, visit the [official Jenkins documentation](https://www.jenkins.io/doc/).
# jenkins