# Java Web Calculator – Manual CI/CD (Multi-Version Deployment)

This project demonstrates a **manual build–stage–deploy pipeline** for a **Java-based Web Calculator App**, progressively enhanced across **three versions**:

| Version | Features Included | WAR File Name | Description |
|----------|------------------|----------------|--------------|
| **v0.0.1** | Addition only | `webapp-add-0.0.1.war` | Basic calculator with addition feature |
| **v0.0.2** | Addition + Subtraction | `webapp-add-sub-0.0.2.war` | Enhanced version with subtraction functionality |
| **v0.0.3** | Addition + Subtraction + Multiplication | `webapp-add-sub-mul-0.0.3.war` | Final version with multiplication added |

---

## ⚙️ Infrastructure Overview

| Server | Purpose | Key Ports | Tools Installed |
|---------|----------|------------|-----------------|
| **Build Server** | Compiles and packages the code using Maven | 22 | OpenJDK 17, Maven, Git |
| **Nexus Server (Staging)** | Stores the `.war` artifacts | 22, 8081 | OpenJDK 17, Nexus Repository 3 |
| **Deploy Server** | Hosts the final app via Tomcat | 22, 8080 | OpenJDK 17, Apache Tomcat 9 |

---

## 🧱 1. Build Server Setup & Packaging
<img width="1920" height="1080" alt="1" src="https://github.com/user-attachments/assets/33b0dd78-72de-4971-874b-11e24cd2e46e" />
<img width="1920" height="1080" alt="11" src="https://github.com/user-attachments/assets/d268255f-e184-47e0-b690-82d974f7c53d" />
<img width="1920" height="1080" alt="12" src="https://github.com/user-attachments/assets/ccc80fe0-756b-4433-a9d2-9bbedaf05a32" />
<img width="1920" height="1080" alt="13" src="https://github.com/user-attachments/assets/d4fdfb4c-8fcf-4008-a0c1-983646ba32c9" />
<img width="1920" height="1080" alt="14" src="https://github.com/user-attachments/assets/d318fea3-3b9a-462d-b979-672b4d1ae038" />

### Steps
```bash
# 1️⃣ Hostname setup
sudo hostnamectl set-hostname build
sudo init 6

# 2️⃣ System update & Java installation
sudo apt -y update
sudo apt install openjdk-17-jre-headless -y

# 3️⃣ Install Maven
sudo apt install maven -y
java -version
mvn -version

# 4️⃣ Clone the repository
git clone https://github.com/mrtechreddy/Java-Web-Calculator-App.git
cd Java-Web-Calculator-App/

# 5️⃣ Validate & package source
mvn validate
mvn clean package

# 6️⃣ Verify WAR files in target/
cd target/
ls
````

### Multi-Version Packaging Process
<img width="1920" height="1080" alt="15" src="https://github.com/user-attachments/assets/3d19fd2b-a480-4551-98ca-5b8bf86568f9" />
<img width="1920" height="1080" alt="16" src="https://github.com/user-attachments/assets/74eb8739-4f36-4a66-b643-d2953b3aa829" />
<img width="1920" height="1080" alt="17" src="https://github.com/user-attachments/assets/e0a4be8f-8ab9-4139-9a7a-aa74a981e1ca" />
<img width="1920" height="1080" alt="18" src="https://github.com/user-attachments/assets/1d357e44-09e9-4728-ae45-c363dedd82f9" />
<img width="1920" height="1080" alt="19" src="https://github.com/user-attachments/assets/31f49672-a2f7-40cf-9e55-de7935bab7c3" />
<img width="1920" height="1080" alt="20" src="https://github.com/user-attachments/assets/7aefe448-d36b-4e30-87a0-23030eade9b9" />
<img width="1920" height="1080" alt="21" src="https://github.com/user-attachments/assets/ab2f3b44-0695-498d-b2f4-44c9d9d90b84" />
<img width="1920" height="1080" alt="22" src="https://github.com/user-attachments/assets/c8072766-7572-4b62-8801-43f00ebd2fc8" />

Each time a feature is added, the app is **rebuilt with a version tag**:

| Step | Feature Added          | Command                           | Output Artifact                |
| ---- | ---------------------- | --------------------------------- | ------------------------------ |
| 1    | Addition               | `mvn clean package`               | `webapp-add-0.0.1.war`         |
| 2    | Addition + Subtraction | Update code → `mvn clean package` | `webapp-add-sub-0.0.2.war`     |
| 3    | Addition + Sub + Mul   | Update code → `mvn clean package` | `webapp-add-sub-mul-0.0.3.war` |

### Deploy to Nexus

Update your `pom.xml` or `/etc/maven/settings.xml`:

```xml
<distributionManagement>
  <repository>
    <id>Java-Cal-App</id>
    <url>http://51.21.200.175:8081/repository/Java-Cal-App/</url>
  </repository>
</distributionManagement>
```

Then push build artifacts:

```bash
mvn deploy
```

---

## 🧩 2. Nexus Repository Server (Staging)

### Setup Steps
<img width="1920" height="1080" alt="3" src="https://github.com/user-attachments/assets/6e646768-e3bd-45df-a2ef-b6c9444a5e2e" />
<img width="1920" height="1080" alt="28" src="https://github.com/user-attachments/assets/af011878-9cd9-44b9-a048-da771cefe81b" />
<img width="1920" height="1080" alt="29" src="https://github.com/user-attachments/assets/b9ec9b66-c6d2-4670-b1fb-a16782311f0b" />
<img width="1920" height="1080" alt="30" src="https://github.com/user-attachments/assets/2ee32615-3077-44b2-86e9-d024d31587ba" />
<img width="1920" height="1080" alt="31" src="https://github.com/user-attachments/assets/4b4253ac-be08-4f17-97a5-422446e5c18e" />
<img width="1920" height="1080" alt="32" src="https://github.com/user-attachments/assets/9cae658c-bb86-4240-9c6f-125b98d466d5" />
<img width="1920" height="1080" alt="8" src="https://github.com/user-attachments/assets/685826b3-c4a3-476f-bb96-20f15fca2f70" />
<img width="1920" height="1080" alt="9" src="https://github.com/user-attachments/assets/71c399b1-d176-4c9e-9757-e1e9ae11374f" />
<img width="1920" height="1080" alt="10" src="https://github.com/user-attachments/assets/fca0a26b-e057-4eaf-997b-2840fc37bc9d" />


```bash
sudo hostnamectl set-hostname nexus
sudo init 6
sudo apt -y update
sudo apt install openjdk-17-jre-headless -y

# Download and extract Nexus
wget https://download.sonatype.com/nexus/3/nexus-3.85.0-03-linux-x86_64.tar.gz
tar -xvzf nexus-3.85.0-03-linux-x86_64.tar.gz
cd nexus-3.85.0-03/bin/

# Start Nexus service
./nexus start
sudo apt install net-tools -y
sudo netstat -ntpl
```

### Access Nexus

* **URL:** `http://51.21.200.175:8081`
* **Login:**

  * Username: `admin`
  * Password: `/home/ubuntu/sonatype-work/nexus3/admin.password`

Create a **hosted Maven repository** named `Java-Cal-App`.

After each deployment:

| Version | Artifact Path                                                                                |
| ------- | -------------------------------------------------------------------------------------------- |
| 0.0.1   | `/repository/Java-Cal-App/com/web/cal/webapp-add/0.0.1/webapp-add-0.0.1.war`                 |
| 0.0.2   | `/repository/Java-Cal-App/com/web/cal/webapp-add-sub/0.0.2/webapp-add-sub-0.0.2.war`         |
| 0.0.3   | `/repository/Java-Cal-App/com/web/cal/webapp-add-sub-mul/0.0.3/webapp-add-sub-mul-0.0.3.war` |

---

## 🧾 3. Deploy Server (Tomcat Deployment)

### Setup
<img width="1920" height="1080" alt="2" src="https://github.com/user-attachments/assets/3242a74a-2931-4071-9106-33eebceffa07" />
<img width="1920" height="1080" alt="4" src="https://github.com/user-attachments/assets/19c34068-7e95-4099-833d-e53c031cc148" />
<img width="1920" height="1080" alt="23" src="https://github.com/user-attachments/assets/ed80fc10-86eb-4f73-8bae-f042acbbfa48" />
<img width="1920" height="1080" alt="24" src="https://github.com/user-attachments/assets/72dbc10a-018c-46c8-8cc1-e4cc50b93988" />
<img width="1920" height="1080" alt="25" src="https://github.com/user-attachments/assets/751dc7b5-4f80-40ab-b481-80cfee4b1fd3" />


```bash
sudo hostnamectl set-hostname deploy
sudo init 6
sudo apt -y update
sudo apt install openjdk-17-jre-headless -y

# Download Apache Tomcat 9
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.111/bin/apache-tomcat-9.0.111.tar.gz
tar -xvzf apache-tomcat-9.0.111.tar.gz
cd apache-tomcat-9.0.111/

# Configure admin access
vi conf/tomcat-users.xml
# Add:
# <user username="admin" password="admin123" roles="manager-gui,admin-gui"/>

# Remove access restrictions
sudo vi ./webapps/manager/META-INF/context.xml
sudo vi ./webapps/host-manager/META-INF/context.xml
# Comment out or remove <Valve ...> line

# Start Tomcat
./bin/startup.sh
```

---

### Deploy WAR Files (Progressive Versions)
<img width="1920" height="1080" alt="26" src="https://github.com/user-attachments/assets/b0007f8c-ffd2-411d-b0e3-77d9c6148c91" />
<img width="1920" height="1080" alt="27" src="https://github.com/user-attachments/assets/19feeddc-48e9-450b-90f0-7862c4d9ee44" />

#### Deploy v0.0.1 (Addition Only)

```bash
cd apache-tomcat-9.0.111/webapps/
wget http://<NEXUS-IP>:8081/repository/Java-Cal-App/com/web/cal/webapp-add/0.0.1/webapp-add-0.0.1.war
```

Access → `http://<DEPLOY-IP>:8080/webapp-add`

#### Deploy v0.0.2 (Add + Sub)

```bash
wget http://<NEXUS-IP>:8081/repository/Java-Cal-App/com/web/cal/webapp-add-sub/0.0.2/webapp-add-sub-0.0.2.war
```

Access → `http://<DEPLOY-IP>:8080/webapp-add-sub`

#### Deploy v0.0.3 (Add + Sub + Mul)

```bash
wget http://<NEXUS-IP>:8081/repository/Java-Cal-App/com/web/cal/webapp-add-sub-mul/0.0.3/webapp-add-sub-mul-0.0.3.war
```

Access → `http://<DEPLOY-IP>:8080/webapp-add-sub-mul`

Restart Tomcat after each deployment:

```bash
./bin/shutdown.sh
./bin/startup.sh
```

---

## 🧮 Validation Summary

| Stage  | Output                 | Verification                      |
| ------ | ---------------------- | --------------------------------- |
| Build  | `.war` in `/target`    | `ls target/`                      |
| Stage  | Artifact in Nexus      | Nexus GUI → browse `Java-Cal-App` |
| Deploy | App running in browser | Access via Tomcat on port 8080    |

---

## 🧰 Troubleshooting

| Issue               | Cause                  | Fix                                       |
| ------------------- | ---------------------- | ----------------------------------------- |
| Nexus not reachable | Service not started    | `cd nexus-3.85.0-03/bin && ./nexus start` |
| Maven deploy fails  | Wrong repo credentials | Check `<servers>` in `settings.xml`       |
| Tomcat 403 error    | Context.xml restricted | Comment out `<Valve>` line                |
| WAR not visible     | Cached deployment      | Clear `/webapps` and redeploy             |

---

## 🧠 Tech Stack

* **Java 17**
* **Apache Maven 3**
* **Sonatype Nexus 3**
* **Apache Tomcat 9**
* **Ubuntu 22.04 LTS (AWS EC2)**

---

## 👨‍💻 Author

**Sivaiah Jagadapi**
*Power BI Developer | DevOps Learner | Cloud Automation Explorer*
📍 Hyderabad, India
🗂 [Glintsoft Infotech Pvt Ltd](https://www.glintsoft.com/)
📧 [sivaiah.jagadapi@example.com](mailto:sivaiah.jagadapi@example.com)

---

## 🏁 End-to-End Flow (All Versions)

```
Source Code v1 → Build (Maven) → WAR v0.0.1 → Nexus Repo → Tomcat Deploy (Addition)
        ↓
Source Code v2 → Build (Maven) → WAR v0.0.2 → Nexus Repo → Tomcat Deploy (Add + Sub)
        ↓
Source Code v3 → Build (Maven) → WAR v0.0.3 → Nexus Repo → Tomcat Deploy (Add + Sub + Mul)
```

---

✅ **Final Outcome:**
A fully working **3-tier manual CI/CD pipeline** demonstrating Java web app build, artifact staging in Nexus, and deployment to Tomcat — versioned incrementally as new calculator features are added.

```

---
