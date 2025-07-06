# AUTOMATED-CONTAINER-DEPLOYMENT-AND-ADMINISTRATION-IN-THE-CLOUD

## ✅ Part 1: Provision EC2 via CloudFormation

**File**: `cloudformation-template.yaml`

- Creates VPC, Subnet, Route Table, Internet Gateway
- Launches EC2 Instance with SSH (port 22) and HTTP (port 80) access

**Steps**:
1. Launch stack via AWS CloudFormation → Upload the template.
2. Provide existing EC2 key pair name.
3. Note the EC2 **public IP** from Outputs.

---

## Part 2: Install Docker via Ansible

**Files**: `inventory.ini`, `install_docker.yml`

**Steps**:
```bash
sudo apt update && sudo apt install -y ansible
chmod 400 ~/.ssh/ec2key.pem
ansible-playbook -i inventory.ini install_docker.yml
Tasks performed:

Update packages

Install Docker

Enable/start Docker service

Add user to docker group

## Part 3: Deploy Flask in Docker via Ansible
Files: app.py, Dockerfile, run_flask_in_docker.yml

bash
Copy
ansible-playbook -i inventory.ini run_flask_in_docker.yml
Opens app in browser:

cpp
Copy
http://<your-ec2-public-ip>
Workflow:

Copies code to EC2

Builds Docker image

Stops old container

Runs Flask container on port 80

## Part 4: CI/CD with GitHub Actions
File: .github/workflows/deploy.yml

Auto deploys Flask app on every push to main branch.

🔐 Required GitHub Secrets:
Name	Value
EC2_HOST	Your EC2 Public IP
EC2_USER	ec2-user (Amazon Linux)
EC2_SSH_KEY	Content of your .pem file

Steps:
bash
Copy
git add .
git commit -m "Trigger pipeline"
git push origin main
