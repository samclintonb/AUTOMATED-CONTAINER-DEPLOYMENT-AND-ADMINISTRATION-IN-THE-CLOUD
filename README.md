**AUTOMATED-CONTAINER-DEPLOYMENT-AND-ADMINISTRATION-IN-THE-CLOUD**

This project demonstrates how to automatically provision cloud infrastructure, configure a server, containerize a Python Flask web application, and deploy it using a complete DevOps pipeline. It integrates AWS CloudFormation, Ansible, Docker, and GitHub Actions to enable CI/CD workflows for scalable, repeatable deployments.

Project Structure cloudformation-template.yaml – Provisions EC2, VPC, subnet, and security groups on AWS

inventory.ini – Hosts file for Ansible to connect to EC2
install_docker.yml – Ansible playbook to install Docker on the EC2 instance
run_flask_in_docker.yml – Ansible playbook to deploy Flask app via Docker
Dockerfile – Instructions to containerize the Flask app
app.py – Flask web application code
requirements.txt – Python dependencies
.github/workflows/deploy.yml – GitHub Actions workflow for CI/CD pipeline

**Part 1: Provision Infrastructure with CloudFormation Open AWS CloudFormation console**
Upload cloudformation-template.yaml

Enter KeyPair name (for SSH access)

Launch the stack and note the EC2 Public IP from Outputs

**Part 2: Install Docker with Ansible Install Ansible on your local machine using:**

                sudo apt update && sudo apt install -y ansible

Replace the IP in inventory.ini with your EC2 public IP

Set key permissions: 
                chmod 400 ~/.ssh/ec2key.pem

Run the Ansible playbook: 
                ansible-playbook -i inventory.ini install_docker.yml

SSH into the instance to verify: 
                ssh -i ~/.ssh/ec2key.pem ec2-user@ docker --version

**Part 3: Run Flask App in Docker using Ansible Ensure app.py and Dockerfile are present locally**

Run the deployment playbook:

                ansible-playbook -i inventory.ini run_flask_in_docker.yml

Visit http:// in your browser

You should see: “Hello from Docker configured with Ansible!”

**Part 4: Set Up CI/CD with GitHub Actions Push app.py, Dockerfile, and .github/workflows/deploy.yml to your GitHub repository**

In GitHub, go to: Settings → Secrets and variables → Actions → New repository secret

Add the following secrets:
EC2_HOST: your EC2 public IP 
EC2_USER: ec2-user (Amazon Linux) or ubuntu (Ubuntu) 
EC2_SSH_KEY: content of your .pem file (as a single line)

Push any changes to the main branch. 
This will trigger the GitHub Actions workflow to: Connect to EC2 over SSH
Copy updated files

Stop old containers
Build and run new Docker container

**How It Works **
EC2 provisioned via CloudFormation 
Docker installed with Ansible 
Flask app containerized and deployed CI/CD pipeline with GitHub Actions auto-deploys on every push
