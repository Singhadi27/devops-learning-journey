# 🚀 DevOps with Jenkins – Learning Journey (Class 14–20)

Welcome to my DevOps journey focused on **Jenkins** — the industry-standard tool for automating builds, tests, and deployments using CI/CD pipelines. This document captures everything from scratch setup to production-ready pipelines with tools like Maven, GitHub, SonarQube, and Tomcat.

---

## 🧠 What is Jenkins?

> Jenkins is an open-source CI/CD tool written in Java that automates software development processes like building, testing, and deploying applications.

### 🔄 CI/CD Explained:
- **CI (Continuous Integration):** Auto-build & test code after every push.
- **CD (Continuous Delivery):** Manual deploy to production.
- **CD (Continuous Deployment):** Auto deploy to production.

---

## 🧱 Pipeline Flow

```
CODE → BUILD → TEST → DEPLOY
```

### Environments:
- 🧑‍💻 DEV: Developers
- 🧪 QA: Testers
- 👨‍💼 UAT: Clients
- 🌐 PROD: Live for Users

---

## 🛠 Jenkins Installation (on AWS EC2)

### ✅ Prerequisites:
- EC2 instance with port **8080** open
- Amazon Linux 2

### 🔧 Setup Steps:

```bash
# Step 1: Install tools
yum install git java-1.8.0-openjdk maven -y

# Step 2: Add Jenkins repo
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

# Step 3: Install Java 11 and Jenkins
amazon-linux-extras install java-openjdk11 -y
yum install jenkins -y

# Step 4: Start Jenkins
systemctl start jenkins
systemctl status jenkins
```

Access Jenkins at:  
```http://<your-ec2-public-ip>:8080```

Unlock it using:
```bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

## 📦 Creating Your First Jenkins Job

- Dashboard → New Item → Freestyle Project
- Name: `netflix-job`
- Source Code Management → Git
- Repo: `https://github.com/devopsbyraham/jenkins-java-project.git`

---

## 🔄 Automate Jenkins Setup with Script

```bash
vim jenkins.sh
# Paste your setup commands
sh jenkins.sh
```

### 💡 Tip:
If you face disk issues:
```bash
sudo rm -rf /tmp/*
sudo systemctl mask tmp.mount
sudo reboot
```

---

## 🔘 Jenkins Parameters

| Type     | Use Case                                 |
|----------|-------------------------------------------|
| Choice   | Select one option from a dropdown         |
| String   | Input text (rarely used)                  |
| Multiline| Input multiple lines                      |
| Boolean  | True/False switch                         |
| File     | Upload file for the build                 |

---

## 🌐 Changing Jenkins Port

```bash
vim /usr/lib/systemd/system/jenkins.service
# Go to line :67 and change 8080 to 8090

systemctl daemon-reload
systemctl restart jenkins
```

---

## ⚙️ Jenkins Configuration Tips

- **Executors**: Increase to run parallel builds  
  → Manage Jenkins → Nodes → Built-in Node → Configure → Set to 3

- **Restarted EC2?**  
  → Jenkins IP changes, update browser URL with new public IP.

---

## 🧯 Recover Deleted Jenkins Jobs

1. Manage Jenkins → Plugins → Install `Job Configuration History`
2. Go back to dashboard to view recovery options.

---

## ⏰ Scheduled Builds with CRON

- CRON format: `* * * * *` → `min hour day month weekday`
- Example:  
  `50 5 18 9 3` → Runs 5:50AM on 18th Sept (Wednesday)

Use [crontab.guru](https://crontab.guru) to generate schedules.

### ⚠️ Limitations:
- Will trigger builds even if code hasn’t changed

---

## ✅ Poll SCM – Trigger Only on Code Change

- Go to Jenkins job → Configure → Build Triggers → Check **Poll SCM**
- Add schedule: `* * * * *`

---

## 🌐 Webhooks – Trigger on Every GitHub Push

- GitHub → Repo → Settings → Webhooks → Add Webhook
- Payload URL: `http://<your-jenkins-url>:8080/github-webhook/`
- Content-Type: `application/json`

---

## ⛔ Throttle Concurrent Builds

Install plugin: **Throttle Concurrent Builds**

- Set maximum builds per node or project to avoid overload

---

## 🔗 Linked Jobs (Upstream & Downstream)

- Create Job1 and Job2
- Job1 → Configure → Post-Build Actions → Build Other Projects → Enter Job2

---

## 🔧 Jenkins Pipeline (Scripted vs Declarative)

> Pipeline: Step-by-step execution of build, test, deploy, written in **Groovy DSL**

### Types:
- 🧾 Scripted (legacy)
- ✅ Declarative (most commonly used)

### Pipeline Example Flow:

```
Stage('Build') → Stage('Test') → Stage('Deploy')
```

---

## 🧪 SonarQube Integration (Code Quality)

- Port: **9000**
- Required: **Java 11**, t2.medium EC2

### Setup:

```bash
# Run manually
sh /opt/sonarqube-10.4.1.88267/bin/linux-x86-64/sonar.sh start
```

**Login:**  
Username: `admin`  
Password: `admin`

> Embed quality check inside pipeline:  
`stage('Code Quality') { steps { ... } }`

### 💡 Memory Tip:
If SonarQube crashes, your server may need **Swap Memory** for more RAM.

---

## ☁️ Tomcat Deployment with Jenkins

### 1. Download & Extract Tomcat:

```bash
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.106/bin/apache-tomcat-9.0.106.tar.gz
tar -xzf apache-tomcat-9.0.106.tar.gz
```

### 2. Configure Tomcat Users:

Edit `tomcat-users.xml`:

```xml
<tomcat-users>
  <role rolename="manager-gui"/>
  <role rolename="admin-gui"/>
  <role rolename="manager-script"/>
  <user username="admin" password="admin123" roles="manager-gui,admin-gui,manager-script"/>
</tomcat-users>
```

### 3. Edit Context Restrictions:

```bash
vim apache-tomcat-9.0.106/webapps/manager/META-INF/context.xml
# Delete lines 21 and 22 to remove IP restrictions
```

### 4. Start Tomcat:

```bash
sh apache-tomcat-9.0.106/bin/startup.sh
```

### 5. Add Credentials in Jenkins:
- Go to Jenkins → Credentials → Add Tomcat user

### 6. Use in Jenkins Pipeline:
```bash
http://<public-ip>:8080/netflix/
```

---

## 🛠 Troubleshooting Jenkins

### ✅ Server-Level:
- Console logs
- Installed packages (Java, Maven, Git)
- CPU & Memory usage

### ✅ Job-Level:
- Git repo URL & branch
- Plugins & syntax

### ✅ Code-Level:
- Errors or dependency failures from developer-side

---

## 🎯 Change Tomcat Port

```bash
vim apache-tomcat-9.0.106/conf/server.xml
# Go to line :69 and change port

# Restart Tomcat
sh apache-tomcat-9.0.106/bin/shutdown.sh
sh apache-tomcat-9.0.106/bin/startup.sh
```

---

## 🏁 Conclusion

This marks the completion of my Jenkins hands-on learning journey — from EC2 setup to CI/CD pipelines, GitHub automation, Tomcat deployment, and SonarQube integration.

Thanks for reading 🙌  
Feel free to ⭐ the repo and follow for more DevOps content!
