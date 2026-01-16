# 🚀 DevOps Final Project: Automated Salary Calculator Pipeline

## 📌 Project Overview
This project simulates a real-world **DevOps CI/CD workflow** using Jenkins, GitHub, and Python.
The goal is to automate the process of calculating employee salaries and generating professional HTML reports across a distributed system.

The pipeline demonstrates a **Hybrid Architecture** where a **Linux Master** orchestrates tasks that are executed on a **Windows Agent**, controlled via a `Jenkinsfile`.

## ⚙️ Architecture & Prerequisites

### System Architecture
* **Jenkins Master:** Linux (Ubuntu/Debian) - Manages the pipeline and orchestrates the build.
* **Jenkins Agent:** Windows 10/11 - Connected via SSH/JNLP to execute the workload.
* **Version Control:** GitHub.

### Prerequisites
* **Python 3.x** installed on the Agent machine.
* **Git** installed and configured in the system PATH.
* **Java (JRE/JDK)** for Jenkins agent connectivity.

---

## 📂 Repository Structure
* `salary_calc.py` - The core logic script (Python). Calculates Net Salary, Tax, and Bonuses.
* `Jenkinsfile` - The Declarative Pipeline definition (Groovy).
* `salary_slip.html` - The generated output report (artifact).
* `salary_calc.log` - execution logs.

---

## 🛠️ How to Run

### 1. Running Manually (CLI)
You can run the script locally without Jenkins using the command line:

```bash
# Usage: python salary_calc.py <Name> <Rate> <Hours> <IsHoliday> <Date>
python salary_calc.py "Eliad" 50 10 true "01/01/2026"

2. Running via Jenkins (Pipeline)
The job is defined as a Parameterized Pipeline:

Open the Jenkins Job (Salary_Pipeline).

Click Build with Parameters.

Fill in the required fields:

RUN_ON_NODE: Choose where to run (e.g., windows or built-in).

EMPLOYEE_NAME: Enter the name.

HOURLY_RATE: Enter hourly wage (must be positive).

HOURS_WORKED: Enter hours (must be positive).

IS_HOLIDAY: Check if holiday bonus applies (+50%).

DATE: Date of the report.

Click Build.

🔄 Pipeline Stages Explained
The Jenkinsfile defines the following stages:

Checkout:

Connects to the GitHub repository.

Pulls the latest code (Script and Jenkinsfile) to the Agent's workspace.

Run Script:

Determines the OS of the node (Linux vs. Windows).

Executes salary_calc.py with the user-provided parameters.

Validation: The script checks if hours/rate are positive numbers. If not, the build fails.

Logging: Writes calculation steps to salary_calc.log.

Post-Build Actions:

HTML Publisher: Archives salary_slip.html and presents it on the Jenkins sidebar.

Ensures artifacts are saved even if the build is unstable.

📊 Outputs
HTML Report
The system generates a styled HTML salary slip including:

Employee Details

Gross Salary Calculation

Tax Deduction (10%)

Net Pay

Log File
A detailed log is created for audit purposes:
[2026-01-15 22:30:00] --- Starting Salary Calculation Script ---
[2026-01-15 22:30:00] Inputs received: Name=Eliad, Rate=50.0, Hours=10.0...
[2026-01-15 22:30:00] Holiday bonus applied (+50%).
[2026-01-15 22:30:00] Calculation success: Gross=750.0, Net=675.0
[2026-01-15 22:30:00] HTML report generated successfully.

📸 Screenshots


1. Jenkins Job Configuration
2. Parameter Input Screen
3. Pipeline Execution (Stage View)
4. Console Output (Showing Windows Execution)
5. Final HTML Report
6. Master/Agent Nodes

Submitted by: Eliad Hagag
