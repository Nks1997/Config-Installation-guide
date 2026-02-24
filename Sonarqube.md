# SonarQube Installation Commands

---

## 1. Linux Installation Commands
```bash
sudo apt update
sudo apt install -y openjdk-17-jdk unzip wget
java -version

sudo useradd -r -s /bin/false sonar

SONAR_VERSION="10.5.0.60888"
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-$SONAR_VERSION.zip
sudo unzip sonarqube-$SONAR_VERSION.zip -d /opt/
sudo mv /opt/sonarqube-$SONAR_VERSION /opt/sonarqube
sudo chown -R sonar:sonar /opt/sonarqube

sudo tee /etc/systemd/system/sonar.service > /dev/null <<EOF
[Unit]
Description=SonarQube service
After=network.target

[Service]
Type=forking
ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop
User=sonar
Group=sonar
Restart=always
LimitNOFILE=65536
LimitNPROC=4096
TimeoutSec=600

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable sonar
sudo systemctl start sonar
sudo systemctl status sonar --no-pager
```
---

## 2. Docker Installation Commands
```bash
docker pull sonarqube:lts-community
docker run -d --name sonarqube -p 9000:9000 sonarqube:lts-community

# Optional for low-memory environments
docker run -d --name sonarqube -p 9000:9000 -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true sonarqube:lts-community

docker ps
docker logs -f sonarqube
```
---

## 3. Windows Installation Commands

# Download and extract SonarQube manually
# Navigate to bin\windows-x86-64

StartSonar.bat
# Optional: install as a Windows service
InstallNTService.bat

# Edit sonar.properties if using a database
# Access at http://localhost:9000

---

## 4. Kubernetes / AKS Installation Commands

# Add SonarQube Helm repository
```bash
helm repo add oteemocharts https://oteemo.github.io/charts
helm repo update
```
# Install SonarQube in AKS cluster
```bash
helm install sonarqube oteemocharts/sonarqube --set service.type=LoadBalancer --set postgresql.postgresqlPassword=sonar
```
# Check LoadBalancer IP
```bash
kubectl get svc sonarqube
```
# List all Helm releases
```bash
helm list -A
```
# Check SonarQube pods
```bash
kubectl get pods -l app.kubernetes.io/instance=sonarqube
```
# Show Helm release values
```bash
helm get values sonarqube -n default
```
# Show all resources installed by Helm
```bash
kubectl get all -l app.kubernetes.io/instance=sonarqube -n default
```
# SonarQube Installation on Windows (Step-by-Step)

This guide shows how to install and configure SonarQube on Windows.

---

## Step 1: Download SonarQube

1. Go to [https://www.sonarqube.org/downloads/](https://www.sonarqube.org/downloads/)
2. Download the **SonarQube Community Edition** (or Developer/Enterprise if licensed).

---

## Step 2: Extract SonarQube

1. Extract the zip file to a folder, e.g., `C:\sonarqube`.

---

## Step 3: Configure database (optional for production)

1. Open `C:\sonarqube\conf\sonar.properties`.
2. For production database (PostgreSQL/MySQL/SQL Server), uncomment and set:

sonar.jdbc.username=your_db_user  
sonar.jdbc.password=your_db_password  
sonar.jdbc.url=jdbc:postgresql://localhost/sonarqube  

3. For development/testing, the embedded H2 database requires no changes.

---

## Step 4: Run SonarQube

1. Navigate to `C:\sonarqube\bin\windows-x86-64`.
2. Run `StartSonar.bat`.
3. Wait 1–2 minutes for startup.

---

## Step 5: Access SonarQube

1. Open a browser and go to:

http://localhost:9000

2. Default credentials:

Username: admin  
Password: admin

---

## Step 6: Optional – Install as Windows Service

1. Navigate to `C:\sonarqube\bin\windows-x86-64`.
2. Run `InstallNTService.bat` to install SonarQube as a Windows service.
3. The service will start automatically on system boot.
