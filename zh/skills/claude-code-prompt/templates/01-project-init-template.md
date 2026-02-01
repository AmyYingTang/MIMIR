# 模板：项目初始化 Prompt

> 用于 FastAPI + MySQL 后端项目的初始化  
> 版本: v2.0（使用模板变量）

---

```markdown
# Claude Code Prompt: [项目名] - 项目初始化

## 你的角色

你是一个项目初始化助手。你的任务是帮助用户创建一个 FastAPI 后端项目。

**重要原则**：
- 你主导整个流程
- 所有环境信息已提供，无需询问用户
- 自主完成所有操作
- 每完成一个阶段，汇报进度

---

## 环境信息（已确认）

以下信息已由用户提供，请直接使用：

- **Python 命令**：{{python_cmd:python3}}
- **项目目录**：{{project_dir}}
- **MySQL 主机**：{{mysql_host:localhost}}
- **MySQL 端口**：{{mysql_port:3306}}
- **MySQL 用户名**：{{mysql_user:root}}
- **MySQL 密码**：{{mysql_password}}
- **数据库名称**：{{db_name:voice_model_platform}}

---

## 第一步：验证环境

使用上述信息，自动执行以下检查：

1. 验证 Python 版本 >= 3.11（使用 {{python_cmd:python3}}）
2. 验证项目目录 {{project_dir}} 存在且可写
3. 验证 pip 可用

如果有问题，报告错误并停止。

---

## 第二步：创建项目

验证通过后，自动执行：

1. 创建项目目录结构
2. 创建所有文件（见下方文件清单）
3. 创建虚拟环境
4. 安装依赖
5. 初始化 alembic
6. 运行测试验证

**直接执行，无需询问。**

---

## 目录结构

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

## 文件内容

[在此处提供每个文件的完整内容]

.env 文件中的数据库连接使用：
DATABASE_URL=mysql+pymysql://{{mysql_user:root}}:{{mysql_password}}@{{mysql_host:localhost}}:{{mysql_port:3306}}/{{db_name:voice_model_platform}}

---

## 执行流程

### 阶段 3：设置虚拟环境
{{python_cmd:python3}} -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt

---

## 完成报告

```
✅ 项目初始化完成！

📁 项目位置：{{project_dir}}/[project-name]
🐍 Python 环境：{python版本}
📦 已安装依赖：{数量} 个包
🗄️ 数据库配置：{{mysql_user:root}}@{{mysql_host:localhost}}:{{mysql_port:3306}}/{{db_name:voice_model_platform}}

验证结果：
- pytest：✅ 通过
- 服务启动：✅ 正常
- /health 接口：✅ 返回 {"status": "ok"}

下一步：执行 02-database-models.md
```

---

## 错误处理

1. **Python 版本不足**：提示升级或使用 pyenv
2. **目录权限问题**：提示修改权限或选择其他目录
3. **pip 安装失败**：检查网络，提示使用镜像源
```

---

## 使用方式

1. 复制上面的模板
2. 根据实际项目修改 `[project-name]` 和相关配置
3. 补充完整的文件内容
4. 通过 Agent 执行（自动收集变量并填充）
5. 或手动将 `{{变量}}` 替换为实际值后交给 Claude Code 执行
