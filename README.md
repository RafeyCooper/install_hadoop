# 🔹 Big Data Project Setup Guide (Windows + WSL2 + Hadoop + Spark + Hive + Tableau + Flask)

This guide covers installation and setup of all required components for your Risk Sentinel project.

-------------------------------------------------
## 0. Install Ubuntu on Windows (WSL2)

### 1. Enable WSL & Virtualization
Open **PowerShell as Administrator** and run:
```powershell
wsl --install
```
👉 This will:
- Enable Windows Subsystem for Linux
- Enable Virtual Machine Platform
- Install Ubuntu (latest LTS) by default

If you already have WSL, update it:
```powershell
wsl --update
```

### 2. Reboot Windows

### 3. Open Ubuntu
From Windows search bar → type **Ubuntu** → launch it.  
On first run, it will:
- Ask you to create a username and password (e.g., `umair`).

### 4. Update Ubuntu
```bash
sudo apt update && sudo apt upgrade -y
```

### 5. Check WSL Version
```powershell
wsl -l -v
```
Output should show:
```
NAME      STATE   VERSION
Ubuntu    Running 2
```

-------------------------------------------------
## 1. Install Java (required for Hadoop & Spark)
```bash
sudo apt install openjdk-11-jdk -y
java -version
```

-------------------------------------------------
## 2. Install Hadoop
### Download & Extract
```bash
cd ~
wget https://downloads.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz
tar -xvzf hadoop-3.3.6.tar.gz
mv hadoop-3.3.6 hadoop
```

### Set Environment Variables
Edit `~/.bashrc`:
```bash
export HADOOP_HOME=$HOME/hadoop
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
```
Reload:
```bash
source ~/.bashrc
```

### Configure core-site.xml
`$HADOOP_HOME/etc/hadoop/core-site.xml`
```xml
<configuration>
  <property>
    <name>fs.defaultFS</name>
    <value>hdfs://localhost:9000</value>
  </property>
</configuration>
```

### Format Namenode
```bash
hdfs namenode -format
```

### Start Hadoop
```bash
start-dfs.sh
start-yarn.sh
jps
```
Expected processes: NameNode, DataNode, SecondaryNameNode, ResourceManager, NodeManager

-------------------------------------------------
## 3. Install Spark
### Download & Extract
```bash
cd ~
wget https://archive.apache.org/dist/spark/spark-3.5.1/spark-3.5.1-bin-hadoop3.tgz
tar -xvzf spark-3.5.1-bin-hadoop3.tgz
mv spark-3.5.1-bin-hadoop3 spark
```

### Set Environment Variables
Add to `~/.bashrc`:
```bash
export SPARK_HOME=$HOME/spark
export PATH=$PATH:$SPARK_HOME/bin
```
Reload:
```bash
source ~/.bashrc
```

### Test Spark
```bash
pyspark
```
Exit with:
```python
exit()
```

-------------------------------------------------
## 4. Install Hive
### Download & Extract
```bash
cd ~
wget https://downloads.apache.org/hive/hive-3.1.3/apache-hive-3.1.3-bin.tar.gz
tar -xvzf apache-hive-3.1.3-bin.tar.gz
mv apache-hive-3.1.3-bin hive
```

### Set Environment Variables
Add to `~/.bashrc`:
```bash
export HIVE_HOME=$HOME/hive
export PATH=$PATH:$HIVE_HOME/bin
```
Reload:
```bash
source ~/.bashrc
```

### Initialize Hive Metastore
```bash
schematool -initSchema -dbType derby
```

-------------------------------------------------
## 5. Install Python + Flask (for UI)
```bash
sudo apt install python3-pip -y
pip3 install flask pyspark pandas scikit-learn
```

Run Flask app:
```bash
python3 app.py
```

-------------------------------------------------
## 6. Install Tableau (Windows, not WSL)
1. Download Tableau Desktop (trial/student version).
2. Install on Windows host.
3. Connect Tableau to Hive via ODBC driver.

-------------------------------------------------
## 7. Workflow Summary
1. Start Hadoop: `start-dfs.sh && start-yarn.sh`
2. Upload dataset to HDFS:
   ```bash
   hdfs dfs -mkdir /fraud
   hdfs dfs -put ~/creditcard.csv /fraud/
   ```
3. Process with Spark (PySpark for rules & ML).
4. Save results in Hive tables.
5. Visualize with Tableau dashboard.
6. Provide real-time fraud check via Flask app.

-------------------------------------------------
## 8. Stop Services
```bash
stop-yarn.sh
stop-dfs.sh
```

-------------------------------------------------
✅ With this setup, you have the complete Big Data pipeline running on your laptop (Windows + WSL2, single-node).
