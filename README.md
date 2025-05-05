# Hello World Node.js App - CI/CD with Jenkins

## 📌 Objective

Deploy a simple Node.js "Hello World" application using a Jenkins CI/CD pipeline triggered by GitHub webhooks.

---

## 📁 Repository Structure

.
├── app.js
├── package.json
├── Jenkinsfile
├── screenshots/
│ └── .gitkeep
└── README.md

yaml
Copy
Edit

---

## 🔧 Technologies Used

- Node.js
- Jenkins
- GitHub
- Ubuntu EC2 (AWS)

---

## 🚀 Setup Instructions

### ✅ Step 1: Launch Ubuntu EC2 Instance

1. Open [AWS EC2 Console](https://console.aws.amazon.com/ec2/).
2. Launch a new Ubuntu 22.04 EC2 instance.
3. Open inbound ports **22 (SSH)** and **8080 (Jenkins)**.

---

### ✅ Step 2: Install Java & Jenkins on EC2

```bash
sudo apt update
sudo apt install -y openjdk-17-jdk
Add Jenkins repository:

bash
Copy
Edit
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update
sudo apt install -y jenkins
Start Jenkins:

bash
Copy
Edit
sudo systemctl enable jenkins
sudo systemctl start jenkins
Access Jenkins:
http://<your-ec2-public-ip>:8080

✅ Step 3: Unlock Jenkins and Install Plugins
Get the initial admin password:

bash
Copy
Edit
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Login to Jenkins UI.

Install suggested plugins.

Create admin user.

✅ Step 4: Set up Node.js on EC2
Install Node.js and npm:

bash
Copy
Edit
sudo apt install -y nodejs npm
Check version:

bash
Copy
Edit
node -v
npm -v
✅ Step 5: Create GitHub Repository
Create a new repository on GitHub (e.g. hello-node-jenkins).

Clone it into your EC2 instance or push your local code:

bash
Copy
Edit
git clone https://github.com/<your-username>/hello-node-jenkins.git
cd hello-node-jenkins
✅ Step 6: Create Jenkins Pipeline
In Jenkins UI, create a new Pipeline project.

Set GitHub repo URL in the pipeline config.

Add this Jenkinsfile in your repo:

groovy
Copy
Edit
pipeline {
  agent any
  stages {
    stage('Clone Repository') {
      steps {
        git 'https://github.com/<your-username>/hello-node-jenkins.git'
      }
    }
    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }
    stage('Start Application') {
      steps {
        sh 'nohup npm start &'
      }
    }
  }
}
✅ Step 7: Add GitHub Webhook
Go to GitHub repo → Settings > Webhooks → Add webhook

Set:

Payload URL: http://<your-ec2-public-ip>:8080/github-webhook/

Content type: application/json

Events: Just the push event

Save.

🌐 Application URL
After the Jenkins build, access your app at:
http://<your-ec2-public-ip>:3000

📷 Screenshots
All screenshots are placed in the screenshots/ folder.

📝 Assumptions
Jenkins is running on the same server as the app.

Node.js is installed and compatible with the app.

No SSL/HTTPS or reverse proxy is configured.

🙌 Author
Tharun Katta
GitHub: tharunkatta10

