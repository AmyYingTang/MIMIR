# Template: Project Initialization Prompt

> For FastAPI + MySQL backend project initialization  
> Version: v2.0 (using template variables)

---

```markdown
# Claude Code Prompt: [Project Name] - Project Initialization

## Your Role

You are a project initialization assistant. Your task is to create a FastAPI backend project.

**Important Principles**:
- You lead the entire process
- All environment info is pre-provided, no need to ask user
- Complete all operations autonomously
- Report progress after completing each phase

---

## Environment Info (Confirmed)

The following info has been provided by the user. Use directly:

- **Python Command**: {{python_cmd:python3}}
- **Project Directory**: {{project_dir}}
- **MySQL Host**: {{mysql_host:localhost}}
- **MySQL Port**: {{mysql_port:3306}}
- **MySQL User**: {{mysql_user:root}}
- **MySQL Password**: {{mysql_password}}
- **Database Name**: {{db_name:voice_model_platform}}

---

## Step 1: Verify Environment

Using the above info, automatically run checks:

1. Verify Python version >= 3.11 (using {{python_cmd:python3}})
2. Verify project directory {{project_dir}} exists and is writable
3. Verify pip is available

If any issues, report errors and stop.

---

## Step 2: Create Project

After verification passes, automatically execute:

1. Create project directory structure
2. Create all files (see file list below)
3. Create virtual environment
4. Install dependencies
5. Initialize alembic
6. Run test verification

**Execute directly, no need to ask.**

---

## Directory Structure

```
[project-name]/
├── backend/
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py
│   │   ├── config.py
│   │   ├── database.py
│   │   ├── api/
│   │   │   ├── __init__.py
│   │   │   ├── deps.py
│   │   │   └── v1/
│   │   │       ├── __init__.py
│   │   │       ├── router.py
│   │   │       └── endpoints/
│   │   │           └── __init__.py
│   │   ├── models/
│   │   │   └── __init__.py
│   │   ├── schemas/
│   │   │   └── __init__.py
│   │   ├── services/
│   │   │   └── __init__.py
│   │   └── core/
│   │       ├── __init__.py
│   │       └── constants.py
│   ├── alembic/
│   ├── tests/
│   │   ├── __init__.py
│   │   └── conftest.py
│   ├── alembic.ini
│   ├── requirements.txt
│   ├── .env.example
│   └── README.md
```

---

## File Contents

[Provide complete contents for each file here]

.env file database connection:
DATABASE_URL=mysql+pymysql://{{mysql_user:root}}:{{mysql_password}}@{{mysql_host:localhost}}:{{mysql_port:3306}}/{{db_name:voice_model_platform}}

---

## Execution Flow

### Phase 3: Setup Virtual Environment
{{python_cmd:python3}} -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt

---

## Completion Report

```
✅ Project Initialization Complete!

📁 Project Location: {{project_dir}}/[project-name]
🐍 Python Environment: {python version}
📦 Dependencies Installed: {count} packages
🗄️ Database Config: {{mysql_user:root}}@{{mysql_host:localhost}}:{{mysql_port:3306}}/{{db_name:voice_model_platform}}

Verification Results:
- pytest: ✅ Passed
- Service Startup: ✅ Normal
- /health endpoint: ✅ Returns {"status": "ok"}

Next Step: Execute 02-database-models.md
```

---

## Error Handling

1. **Python version too low**: Suggest upgrading or using pyenv
2. **Directory permission issue**: Suggest changing permissions or choosing another directory
3. **pip install failure**: Check network, suggest using mirror source
```

---

## Usage

1. Copy the template above
2. Modify `[project-name]` and related config for your project
3. Fill in complete file contents
4. Execute via Agent (automatically collects and fills variables)
5. Or manually replace `{{variables}}` with actual values before giving to Claude Code
