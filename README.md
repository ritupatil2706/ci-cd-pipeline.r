# CI/CD Pipeline — Flask App

Automated pipeline using Jenkins that builds a Docker image and deploys to an EC2 instance.

**How to use**
1. Create Docker Hub credentials and add to Jenkins as `dockerhub-creds`.
2. Add EC2 SSH key to Jenkins as `ec2-ssh` (username usually `ec2-user` or `ubuntu`).
3. Update `Jenkinsfile` with your GitHub repo and EC2 public IP.
4. Push code to GitHub and watch Jenkins run.
