Here’s a **professional, GitHub-ready `README.md`** for your **Java Web Calculator project (Manual Build → Nexus Staging → Tomcat Deploy)** — based directly on the server command histories you provided.
You can copy-paste it as-is into your repo root.

---

````markdown
# Java Web Calculator – Manual Build, Stage, and Deploy (3-Tier CI/CD Setup)

This project demonstrates a **manual CI/CD pipeline** for a Java-based web calculator application using three EC2 servers:

- **Build Server** – Compiles and packages the Java web app using Maven.  
- **Nexus Staging Server** – Hosts the artifacts using Sonatype Nexus Repository Manager.  
- **Deploy Server** – Runs Apache Tomcat to serve the deployed WAR file.

---

## 🚀 Architecture Overview

| Server | Purpose | Key Ports | Tools Installed |
|---------|----------|------------|-----------------|
| `build` | Build & package source code | 22 | OpenJDK 17, Maven, Git |
| `nexus` | Stage & host artifacts | 22, 8081 | OpenJDK 17, Nexus Repository 3 |
| `deploy` | Deploy WAR to Tomcat | 22, 8080 | OpenJDK 17, Apache Tomcat 9 |

---

## 🧱 1. Build Server Setup

### Steps
```bash
# Set hostname
sudo hostnamectl set-hostname build
sudo init 6

# System update & Java installation
sudo apt -y update
sudo apt install openjdk-17-jre-headless -y

# Verify Java installation
java -version

# Install Maven
sudo apt install maven -y
mvn -version

# Clone the Java web calculator project
git clone https://github.com/mrtechreddy/Java-Web-Calculator-App.git
cd Java-Web-Calculator-App/

# Validate & package the application
mvn validate
mvn clean package

# Check generated WAR in target folder
cd target/
ls
````

### Deploy Configuration (pom.xml)

Edit your `pom.xml` to include Nexus repository credentials and URLs:

```xml
<distributionManagement>
  <repository>
    <id>nexus</id>
    <url>http://<NEXUS-IP>:8081/repository/Java-Cal-App/</url>
  </repository>
</distributionManagement>
```

Then deploy:

```bash
mvn deploy
```

---

## 🧩 2. Nexus Server (Staging Repository)

### Steps

```bash
# Set hostname
sudo hostnamectl set-hostname nexus
sudo init 6

# System update & Java installation
sudo apt -y update
sudo apt install openjdk-17-jre-headless -y

# Download and extract Nexus
wget https://download.sonatype.com/nexus/3/nexus-3.85.0-03-linux-x86_64.tar.gz
tar -xvzf nexus-3.85.0-03-linux-x86_64.tar.gz
cd nexus-3.85.0-03/bin/

# Start Nexus service
./nexus start

# Check running ports
sudo apt install net-tools -y
sudo netstat -ntpl
```

### Nexus Access

* **URL:** `http://<NEXUS-IP>:8081`
* **Default credentials:**

  * Username: `admin`
  * Password: Found in
    `/home/ubuntu/sonatype-work/nexus3/admin.password`

Login → Create a hosted Maven repository named `Java-Cal-App`.

---

## 🧾 3. Deploy Server (Tomcat Hosting)

### Steps

```bash
# Set hostname
sudo hostnamectl set-hostname deploy
sudo init 6

# System update & Java installation
sudo apt -y update
sudo apt install openjdk-17-jre-headless -y

# Download Apache Tomcat 9
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.111/bin/apache-tomcat-9.0.111.tar.gz
tar -xvzf apache-tomcat-9.0.111.tar.gz
cd apache-tomcat-9.0.111/

# Configure Tomcat users (manager & admin roles)
vi conf/tomcat-users.xml
# Add inside <tomcat-users> tag:
# <user username="admin" password="admin123" roles="manager-gui,admin-gui"/>

# Allow remote access (edit these files):
sudo vi ./webapps/manager/META-INF/context.xml
sudo vi ./webapps/host-manager/META-INF/context.xml
# Comment out or remove the <Valve ...> restriction line.

# Start Tomcat
./bin/startup.sh
```

### Deploy WAR from Nexus

```bash
cd apache-tomcat-9.0.111/webapps/

# Download the WAR directly from Nexus repository
wget http://<NEXUS-IP>:8081/repository/Java-Cal-App/com/web/cal/webapp-add-sub-mul/0.0.3/webapp-add-sub-mul-0.0.3.war
```

Restart Tomcat:

```bash
./bin/shutdown.sh
./bin/startup.sh
```

Access application:

```
http://<DEPLOY-IP>:8080/webapp-add-sub-mul
```

---

## 🧮 Final Verification

* **Build success:** WAR generated in `/target`
* **Staging success:** WAR uploaded to Nexus repository
* **Deployment success:** App accessible on Tomcat via port 8080

---

## 🧰 Troubleshooting

| Issue               | Possible Cause                      | Fix                                        |
| ------------------- | ----------------------------------- | ------------------------------------------ |
| Nexus not reachable | Service not started                 | `cd nexus-3.85.0-03/bin && ./nexus start`  |
| WAR not uploaded    | Wrong credentials in `settings.xml` | Verify `<servers>` section in Maven config |
| Tomcat 403 error    | Access restriction in `context.xml` | Comment out the `<Valve>` tag              |
| WAR not deploying   | Filename mismatch                   | Ensure correct WAR version from Nexus      |

---

## 📦 Tech Stack

* **Java 17**
* **Apache Maven 3**
* **Sonatype Nexus 3**
* **Apache Tomcat 9**
* **Ubuntu 22.04 LTS (EC2)**

---

## 🧑‍💻 Author

**Sivaiah Jagadapi**
*Power BI & DevOps Enthusiast | Java Build Automation Learner*
📍 Hyderabad, India
🗂 [Glintsoft Infotech Pvt Ltd](https://www.glintsoft.com/)
📧 *[sivaiah.jagadapi@example.com](mailto:sivaiah.jagadapi@example.com)*

---

### 🏁 End-to-End Flow Summary

```
Developer → Build Server → Nexus Repository → Deploy Server (Tomcat)
      |            |              |                     |
      |  mvn build |  mvn deploy  |  wget WAR & deploy  |
```

---

```

---

Would you like me to **add command outputs and screenshots placeholders** (e.g., Maven build success, Nexus dashboard, Tomcat manager UI) to make the README more visually rich for GitHub? It’ll look professional and ready for your portfolio.
```
