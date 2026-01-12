### Configure PySpark to use Virtual Environment (Windows)

After creating and activating the virtual environment, run the following commands:

```powershell
setx PYSPARK_PYTHON "%CD%\.venv\Scripts\python.exe"
setx PYSPARK_DRIVER_PYTHON "%CD%\.venv\Scripts\python.exe"
Restart PowerShell after running these commands.
