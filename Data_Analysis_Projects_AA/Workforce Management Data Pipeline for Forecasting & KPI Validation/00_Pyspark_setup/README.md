
# PySpark Setup Guide (Windows)

This README explains how to properly set up PySpark on a Windows machine.

This setup is mandatory to run any PySpark code without errors.

---

## What is PySpark?

PySpark is the Python API for Apache Spark.

Important point:
- PySpark alone is NOT enough
- It needs Java, Spark engine, Hadoop utilities, and correct configuration

If any one of these is missing or misconfigured, PySpark code will fail.

---

## Mandatory Version Compatibility (Very Important)

It is mandatory that all versions match correctly.

If versions are mismatched, you will face runtime errors, gateway errors, or silent failures.

Recommended stable combination:

| Component | Recommended Version |
|--------|----------------------|
| Python | 3.9 or 3.10 |
| PySpark | 3.5.x |
| Spark | 3.5.x |
| Java (JDK) | 11 or 17 (LTS) |

Always check compatibility before installing.

---

## Step 1: Install Python

Download Python from:
https://www.python.org/downloads/

Recommended:
- Python 3.10

During installation:
- Check **Add Python to PATH**

Verify:
```powershell
python --version
````

---

## Step 2: Install Java (JDK)

Spark runs on JVM, so Java is mandatory.

Download Java (JDK 11 or 17 LTS):
[https://www.oracle.com/java/technologies/javase-downloads.html](https://www.oracle.com/java/technologies/javase-downloads.html)

Install and verify:

```powershell
java -version
```

---

## Step 3: Download Apache Spark (Manual Install)

Go to:
[https://spark.apache.org/downloads/](https://spark.apache.org/downloads/)](https://spark.apache.org/downloads.html)

Select:

* Spark version: 3.5.x
* Package: Pre-built for Apache Hadoop 3.3

Download the `.tgz` file.

---

## Step 4: Extract Spark

Extract Spark to:
Create folder named spark on C:\
```
C:\spark\
```

After extraction, structure should look like:

```
C:\spark\
 └── spark-3.5.1-bin-hadoop3\
     ├── bin\
     ├── conf\
     ├── jars\
     ├── python\
```

Note:

* If you do NOT see this folder, Spark is not installed correctly
* PySpark pip install does NOT create this folder

---

## Step 5: Install winutils.exe (Windows Only)

Windows needs Hadoop utilities to manage file permissions.

Download winutils:
[https://github.com/cdarlint/winutils](https://github.com/cdarlint/winutils)

Steps:

1. Open the repository
2. Choose Hadoop version 3.3.x
3. Download `winutils.exe`

Create folders:

```
C:\hadoop\bin\
```

Place file:

```
C:\hadoop\bin\winutils.exe
```

---

## Step 6: Set Environment Variables (Mandatory)

Open:
System Properties → Environment Variables

### Add System Variables

**JAVA_HOME**

```
C:\Program Files\Java\jdk-17
```

**SPARK_HOME**

```
C:\spark\spark-3.5.1-bin-hadoop3
```

**HADOOP_HOME**

```
C:\hadoop
```

---

### Update PATH Variable

Add these paths:
Under user variable click path -> click Edit -> New -> paste these paths given below-> click OK
```
%JAVA_HOME%\bin
%SPARK_HOME%\bin
%HADOOP_HOME%\bin
```

Restart the system after setting variables.

---

## Step 7: Verify Spark Installation

Open a new terminal:

```powershell
spark-submit --version
```

If Spark version prints, Spark is correctly installed.

---

## Step 8: Install PySpark (Python Side)

Install PySpark using pip:

```powershell
pip install pyspark==3.5.1
```

Verify:

```powershell
pip show pyspark
```

---

## What Are JAR Files and Why They Matter

Spark internally runs on JVM.
Python alone cannot talk directly to databases or Excel in Spark.

JAR files act as connectors.

Examples:

* Reading Excel files
* Connecting to SQL Server
* JDBC connections
* Other databases (Postgres, Oracle, etc.)

---

## Where to Place JAR Files

All external JAR files must be placed inside:

```
C:\spark\spark-3.5.1-bin-hadoop3\jars\
```

Spark automatically loads all JARs from this folder.

No extra configuration needed if placed correctly.

---

## Excel, JDBC, and Future Integrations

Using Excel or SQL later will NOT cause conflicts if:

* JAR versions match Spark version
* No duplicate JARs exist
* JARs are placed only inside `jars` folder

This is the standard enterprise approach.

---

## Common Errors and Their Meaning

### JAVA_GATEWAY_EXITED

Reason:

* Java not installed
* JAVA_HOME incorrect
* Version mismatch

### winutils.exe error

Reason:

* HADOOP_HOME not set
* winutils.exe missing

### Permission denied while writing files

Reason:

* winutils.exe not found
* Incorrect Hadoop setup

---

## Important Notes

* PySpark pip install does NOT install Spark engine
* Spark folder must exist manually
* Versions must always align
* Environment variables are mandatory
* This setup is one-time only

---

## Final Statement

If you follow this README exactly:

* PySpark will run without errors
* Large datasets will process correctly
* Future Excel, SQL, and cloud integrations will work smoothly

This is the same setup used in real enterprise projects.


