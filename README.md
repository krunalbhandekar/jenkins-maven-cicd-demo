# 🚀 Jenkins Maven Java CI/CD - Simple Calculator Project

This project demonstrates how to build a simple Java calculator application using Maven and Jenkins. It supports **two build methods**:

- ✅ Freestyle Job using Shell
- ✅ Pipeline Job using Jenkinsfile

---

## 🧰 Project Structure

```bash
├── pom.xml
└── src/
└── main/
└── java/
└── com/
└── example/
└── Calculator.java
```

---

## 🛠️ Prerequisites

- Java 17 (Installed on Jenkins server)
- Maven (Manually installed at `/opt/maven`)
- GitHub repository containing this project
- Jenkins server up and running
- Git plugin installed in Jenkins

---

## 📦 Manual Installation of Maven on Jenkins Server

### 🔹 Step 1: Download Maven

```bash
cd /opt
```

```bash
sudo wget https://dlcdn.apache.org/maven/maven-3/3.9.10/binaries/apache-maven-3.9.10-bin.tar.gz
```

🔁 You can check the latest version from: https://maven.apache.org/download.cgi

### 🔹 Step 2: Extract Maven

```bash
sudo tar -xvzf apache-maven-3.9.10-bin.tar.gz
```

```bash
sudo mv apache-maven-3.9.10 maven
```

### 🔹 Step 3: Set Environment Variables

Edit the system profile:

```bash
vim /etc/profile
```

Paste the following:

```bash
export PATH=$PATH:'/opt/maven/bin/'
```

### 🔹 Step 4: Load Environment Variables

```bash
source /etc/profile
```

### 🔹 Step 5: Verify Maven Installation

```bash
mvn --version
```

Expected output:

```pgsql
Apache Maven 3.9.10
Java version: 17 (or whatever is installed)
```

---

## 🛠️ Step-by-Step: Run mvn clean package in Jenkins

### ✅ Option 1: Freestyle Project

**1. Create Jenkins Freestyle Job**

- Go to **Jenkins → New Item → Freestyle Project**
- Name: `build-calculator-manual`
- Under **Source Code Management**
  - Select **Git**
  - Add repo: `https://github.com/krunalbhandekar/jenkins-maven-cicd-demo.git`
  - Enter branch **\*/main**
- Under **Build Steps → Add build step → Execute shell**

**2. Add Shell Script**

```bash
/opt/maven/bin/mvn clean package
```

**3. Save & Build**

- Click **Apply**
- Click **Save**
- Click **Build Now** (left navigation)

**📁 Output**

After successful build: `.jar` will be generated in `target/`

```bash
ls /var/lib/jenkins/workspace/build-calculator-manual/target
```

---

### ✅ Option 2: Pipeline Job (Jenkinsfile)

**1. Create Jenkins Pipeline Job**

- Go to **Jenkins → New Item → Pipeline**
- Name: `build-calculator-pipeline`
- Under **Pipeline → Definition → Pipeline script**

**2. Pipeline Script**

Paste this:

```groovy
pipeline {
    agent any

    stages {
        stage('Pull') {
            steps {
                git branch: 'main', url: 'https://github.com/krunalbhandekar/jenkins-maven-cicd-demo.git'
            }
        }
        stage('Build') {
            steps {
               sh '/opt/maven/bin/mvn clean package'
            }
        }
    }
}
```

**3. Save & Build**

- Click **Apply**
- Click **Save**
- Click **Build Now** (left navigation)

**📁 Output**

After successful build: `.jar` will be generated in `target/`

```bash
ls /var/lib/jenkins/workspace/build-calculator-pipeline/target
```

---

### 👨‍💻 Author

Maintained by **[Krunal Bhandekar](https://www.linkedin.com/in/krunal-bhandekar/)**

---
