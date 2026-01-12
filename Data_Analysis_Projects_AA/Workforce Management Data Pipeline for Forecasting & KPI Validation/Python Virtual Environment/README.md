# Python Virtual Environment (venv) – Setup & Usage Guide

---

## What is a Virtual Environment (.venv)?

A **virtual environment** is an isolated Python setup created specifically for a project.
It has its **own Python packages, versions, and dependencies**, separate from the system-wide Python installation.

This project uses a virtual environment named **`.venv`**.

---

## Why Do We Use a Virtual Environment?

Using a virtual environment is considered **best practice**, especially for data projects like PySpark.

### Key Benefits

* Prevents **version conflicts** between different projects
* Keeps project dependencies **isolated and clean**
* Avoids breaking system-level Python packages
* Ensures the project works the same on different machines
* Makes debugging and maintenance easier

> In short: **"Version conflict se bachaav"** aur stable development.

---

## Prerequisites

Make sure you have:

* Python 3.x installed
* PowerShell (Windows) or Terminal (Linux/Mac)

Check Python installation:

```bash
python --version
```

---

## Step 1: Create a Virtual Environment

Navigate to the project root directory:

```bash
cd path/to/your/project
```

Create the virtual environment:

```bash
python -m venv .venv
```

This will create a folder named **`.venv`** in the project directory.

---

## Step 2: Activate the Virtual Environment

### On Windows (PowerShell)

```powershell
.\.venv\Scripts\activate
```

After activation, your terminal will look like:

```text
(.venv) PS C:\Users\...\project-name>
```

This confirms the environment is active.

---

## Step 3: Install Project Dependencies

Once the environment is active, install required packages:

```bash
pip install pyspark
```

All packages will be installed **inside `.venv` only**, not globally.

---

## Step 4: Verify Installation

Check if a package is installed inside the virtual environment:

```bash
pip show pyspark
```

You should see a path similar to:

```text
...\.venv\Lib\site-packages
```

This confirms correct installation.

---

## How to Deactivate the Virtual Environment

When you are done working:

```bash
deactivate
```

The `(.venv)` prefix will disappear from the terminal.

---

## When Should You Use `.venv`?

* Always activate `.venv` before:

  * Running PySpark jobs
  * Installing Python packages
  * Running project scripts

* Do NOT install project libraries globally using system Python

---

## Common Mistakes to Avoid

* Mixing global Python packages with `.venv`
* Forgetting to activate `.venv` before running code
* Installing packages outside the virtual environment

These mistakes often lead to **version mismatch errors** and runtime failures.

---

## Summary

* `.venv` provides an isolated Python environment
* It protects the project from dependency and version conflicts
* Always activate `.venv` before working on the project
* Deactivate it once work is complete

---

## Recommended Practice

Keep `.venv` inside the project folder and add it to `.gitignore`:

```gitignore
.venv/
```

---

This setup ensures a **stable, conflict-free, and professional development workflow**.
