# Film Suggestions App Deployment on AWS with Jenkins

This project is a sample Node.js/React-style application for film suggestions. The README demonstrates how to deploy it on an AWS EC2 instance using a Jenkins pipeline.

## Project Structure

- `Jenkinsfile` — CI/CD pipeline for building and deploying the app
- `README.md` — deployment instructions and setup guide

## Prerequisites

Before starting, make sure the following are ready:

- AWS account with an EC2 instance running Ubuntu or Amazon Linux
- Jenkins installed and running
- GitHub repository connected to Jenkins
- SSH access to the EC2 instance
- Node.js and npm installed on the EC2 instance (for this sample app)
- PM2 or a similar process manager (optional but recommended)

## AWS EC2 Setup

1. Launch an EC2 instance (Ubuntu recommended).
2. Open the required ports:
   - 22 (SSH)
   - 80 (HTTP)
   - 443 (HTTPS)
3. Install dependencies on the EC2 instance:

```bash
sudo apt update
sudo apt install -y nodejs npm nginx git
sudo npm install -g pm2
```

4. Create a directory for the app:

```bash
sudo mkdir -p /var/www/film-suggestions
sudo chown -R ubuntu:ubuntu /var/www/film-suggestions
```

5. Configure NGINX as a reverse proxy if needed.

## Jenkins Setup

1. Install required Jenkins plugins:
   - GitHub Integration
   - Pipeline
   - SSH Agent or Publish Over SSH
2. Add your AWS EC2 SSH key in Jenkins.
3. Create a pipeline job pointing to this repository.
4. Ensure Jenkins has access to the EC2 server via SSH.

## Pipeline Behavior

The `Jenkinsfile` in this repository performs the following steps:

- Checkout source code
- Install dependencies
- Run tests
- Build the application
- Deploy the built artifact to the AWS EC2 instance
- Restart the app service

## Example Deployment Flow

A typical deployment flow:

```bash
npm install
npm run build
npm test
```

Then upload the built files to the EC2 instance and start the app using PM2 or systemd.

## Jenkinsfile Overview

The Jenkins pipeline contains stages such as:

- `Checkout`
- `Install Dependencies`
- `Build`
- `Test`
- `Deploy`

This is a basic production-ready example. You can modify it for your exact project stack, environment variables, and deployment target.

## Sample EC2 Deployment Commands

On the EC2 instance:

```bash
cd /var/www/film-suggestions
npm install
npm run build
pm2 start "npm run start" --name film-suggestions
```

If using NGINX:

```bash
sudo systemctl restart nginx
```

## Security Notes

- Do not expose AWS credentials in the repository.
- Use Jenkins credentials for secrets.
- Restrict SSH access to your IP or Jenkins agent only.
- Use HTTPS with a valid SSL certificate in production.

## Troubleshooting

If the Jenkins job fails:

- Verify GitHub webhook or repository credentials
- Check Jenkins environment variables
- Confirm the EC2 instance is running and reachable
- Check the SSH key and permissions
- Verify Node.js and npm versions on the EC2 machine

## Next Improvements

- Add Docker support
- Add environment-specific deployment (`dev`, `staging`, `prod`)
- Add Slack or email notifications
- Add infrastructure as code using Terraform or CloudFormation

## License

This project is provided for learning and deployment demonstration purposes.
