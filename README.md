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

## ⚙️ Switch Java Version on Jenkins Server (System-Wide)

If your **project requires Java 17**, but **Jenkins is using Java 21** (or any other version), you can manually switch the default Java version using the following command:

### ✅ Use the following command:

```bash
sudo update-alternatives --config java
```

### 🧾 Output Example:

```bash
There are 2 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                            Priority   Status
------------------------------------------------------------
* 0            /usr/lib/jvm/java-21-openjdk-amd64/bin/java     1211      auto mode
  1            /usr/lib/jvm/java-17-openjdk-amd64/bin/java     1111      manual mode
  2            /usr/lib/jvm/java-21-openjdk-amd64/bin/java     1211      manual mode

Press enter to keep the current choice[*], or type selection number:
```

### ⌨️ What to Do:

- Enter the number (e.g., `1`) for Java 17 to switch.
- This will make Java 17 the **default Java version for all users**, including Jenkins.

### 🧪 To Verify the Current Java Version

```bash
java -version
```

Expected:

```bash
openjdk version "21.0.x" ...
```

### 💡 Tip: Don’t Want to Change Global Java?

If you don’t want to change Java system-wide, you can **override it in Jenkins job** using:

**Freestyle Job → Execute Shell Step:**

```bash
export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64
export PATH=$JAVA_HOME/bin:$PATH
/opt/maven/bin/mvn clean package
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

**📂 Test Folder Structure with `tree` Command**

```bash
sudo apt install tree
```

```bash
cd /var/lib/jenkins/workspace/build-calculator-manual
```

```bash
tree
```

Expected:

```pgsql
├── pom.xml
├── src
│   └── main
│       └── java
│           └── com
│               └── example
│                   └── Calculator.java
└── target
    ├── classes
    ├── generated-sources
    ├── maven-status
    └── SimpleCalculator-1.0-SNAPSHOT.jar
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

**📂 Test Folder Structure with `tree` Command**

```bash
sudo apt install tree
```

```bash
cd /var/lib/jenkins/workspace/build-calculator-pipeline
```

```bash
tree
```

Expected:

```pgsql
├── pom.xml
├── src
│   └── main
│       └── java
│           └── com
│               └── example
│                   └── Calculator.java
└── target
    ├── classes
    ├── generated-sources
    ├── maven-status
    └── SimpleCalculator-1.0-SNAPSHOT.jar
```

---

### 👨‍💻 Author

Maintained by **[Krunal Bhandekar](https://www.linkedin.com/in/krunal-bhandekar/)**

---
