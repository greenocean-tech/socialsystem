# ai career system
ai career system

<!-- This is the markdown template for the new social system of the AI, 
created by Lo and University of Helsinki. 
Copy the template, paste it to your GitHub README and edit! -->

# Project Title

New career system instead of emotional human leaders.

## Summary

Many leaders promote friends become leaders but without solving issues capability or getting support.
Labor can reply on this ai system instead of interface terrible leaders and reduce office bully situation.


## Background

* Manager rejected labor internal transfer job opportunity in the company.
* Manager has no assign any task for labors then rate bad performance for labor that damaged labor career and bonus.
* Manager can not state clear task to labor but push all the responsibility to labors then asked labor resign.
* Manager yelling labor at the office but without helping solve the issue.
* Manager asked labor privacy questions without respectful.


## How is it used?

Labors can be the user.
Leaders instruction will be filter by ai system ,if it's ilegal then it won't ask labor to do but system will educate leader.
Labor has freedom to transfer to another job position without leader agree or reject.
AI can check the labor working result without emotional leader change task all the time. 
AI can be reporting line for labor instead of report to emotional leader directly.

![Cat](https://upload.wikimedia.org/wikipedia/commons/5/5e/Sleeping_cat_on_her_back.jpg)

This is how you create code examples:
```
def main():
   task = ['report', 'repair', 'operation', 'quality', 'year']
   pop = [low wage, reject transfer, HR and leader comment, bully behavior, yell]   # not actually needed in this exercise...
   fishers = [correct, incorrect, wage, complete, incomplete]

   totPop = sum(pop)
   totFish = sum(fishers)

   # write your solution here

   for i in range(len(task)):
      print("%s %.2f%%" % (task[i], 100.0))    # current just prints 100%

main()
```


## Data sources and AI methods

| Syntax      | Description |
| ----------- | ----------- |
| Task        | Daily production result  |
| Transfer opportunity   | fill division, job title        |

## Challenges

Managers and HR will initial a group to against this system.

## What next?

Implement this system inside company and allow labor to choose, I will need protection to against all of Managers and HR.

## 1. System Architecture Overview

```
+-------------------------+
|     User Interfaces     |
|-------------------------|
| - Manager Dashboard     |
| - Labor Dashboard       |
| - HR/Admin Dashboard    |
+-------------------------+
           |
           v
+----------------------------+
|       API Layer            |
| (RESTful or GraphQL APIs)  |
+----------------------------+
           |
           v
+-------------------------------------------------+
|            Application Logic Layer               |
|-------------------------------------------------|
| - Task Assignment Engine (AI/Rules-based)        |
| - Task Legality Checker & Policy Engine          |
| - Performance Rating Engine (AI/Rule-based)      |
| - Communication & Notification Module            |
| - Reporting & Audit Logging                      |
+-------------------------------------------------+
           |
           v
+------------------------------+
|         Database             |
|------------------------------|
| - Users (Labor, Managers)    |
| - Tasks & Assignments        |
| - Reports & Signatures       |
| - Audit Logs & Policy Events |
| - Notifications              |
+------------------------------+
           |
           v
+-------------------------------+
|    External Integrations      |
|-------------------------------|
| - HR System                   |
| - Email/SMS for Notifications |
| - Document Signing API        |
+-------------------------------+

---

## 2. Key Features Mapped to Components

- **Task Assignment Engine**: Assigns tasks based on skill, availability, legality, and deadlines. Prevents illegal/irregular tasks.
- **Legality Checker & Policy Engine**: Flags illegal tasks (e.g., copying customer data), educates managers, notifies HR/higher management, logs incident.
- **Performance Rating Engine**: Rates only on objective completion (verified test reports, customer signatures).
- **Communication Module**: All manager-labor communications are routed, filtered, and logged. Yelling or emotional language is flagged/neutralized.
- **Notification Module**: Reminds managers/labors of missing assignments, prevents unfair ratings/PIP moves.
- **Reporting/Audit**: Tracks actions, compliance, and provides a transparent log for HR and executives.

---

## 3. Prototype Codebase Outline

Below is a basic codebase structure in **Python (FastAPI)** for the backend logic, with placeholder code for the main modules. 

```python name=main.py
from fastapi import FastAPI
from routers import tasks, users, reports, notifications, compliance

app = FastAPI(title="AI Labor Performance & Task Management")

app.include_router(users.router)
app.include_router(tasks.router)
app.include_router(reports.router)
app.include_router(notifications.router)
app.include_router(compliance.router)
```

```python name=routers/tasks.py
from fastapi import APIRouter, HTTPException
from models import Task, User
from services import legality_checker, assignment_engine

router = APIRouter(prefix="/tasks", tags=["tasks"])

@router.post("/assign")
def assign_task(task: Task):
    # Check legality first
    legal, message = legality_checker.check(task)
    if not legal:
        # Log and notify HR/higher manager
        legality_checker.report_illegal(task, message)
        raise HTTPException(status_code=403, detail=message)
    # Assign task
    assigned = assignment_engine.assign(task)
    return {"status": "assigned", "to": assigned}
```

```python name=services/legality_checker.py
def check(task):
    # Example: Prevent illegal data-copying tasks
    if "copy" in task.description and "customer" in task.description:
        if "to another customer" in task.description:
            return False, "Copying data between customers is illegal!"
    # Add more policy checks as needed
    return True, ""

def report_illegal(task, message):
    # Log event and send notification to HR and higher management
    print(f"Illegal Task Attempt: {task.description} | {message}")
    # (Send notification logic)
```

```python name=services/assignment_engine.py
def assign(task):
    # Example: Assign based on skills, availability, and deadlines
    # (This would use AI/ML or rules-based selection)
    # Placeholder: assign to first available user
    return "labor_user_id"
```

```python name=routers/reports.py
from fastapi import APIRouter
from models import Report
from services import performance_rating

router = APIRouter(prefix="/reports", tags=["reports"])

@router.post("/submit")
def submit_report(report: Report):
    # AI reviews report and checks for customer signature
    completed = performance_rating.review(report)
    if completed:
        return {"status": "completed", "rating": "pass"}
    else:
        return {"status": "incomplete", "rating": "fail"}
```

```python name=services/performance_rating.py
def review(report):
    # Example: AI checks for required fields and customer signature
    if report.content and report.customer_signature:
        return True
    return False
```

```python name=routers/notifications.py
from fastapi import APIRouter
router = APIRouter(prefix="/notifications", tags=["notifications"])

@router.post("/remind")
def remind_managers():
    # Logic to remind managers if no task assigned monthly
    # (Pseudo code for notification)
    return {"status": "reminder sent"}
```

```python name=routers/compliance.py
from fastapi import APIRouter
router = APIRouter(prefix="/compliance", tags=["compliance"])

@router.post("/pip/prevent")
def prevent_unfair_pip(user_id: str):
    # Prevent PIP without notice or assigned tasks
    # (Pseudo logic)
    return {"status": "PIP prevention enforced"}
```

```python name=models.py
from pydantic import BaseModel

class User(BaseModel):
    user_id: str
    name: str
    role: str  # manager/labor/hr

class Task(BaseModel):
    task_id: str
    description: str
    assigned_to: str
    deadline: str

class Report(BaseModel):
    report_id: str
    task_id: str
    content: str
    customer_signature: bool
```

---

## 4. Next Steps

- **Frontend**: Build dashboards for manager, labor, and HR with clear views of tasks, reports, and compliance actions.
- **Integrate with HR**: For reporting illegal tasks and handling PIP moves.
- **AI/ML**: Upgrade assignment and report review logic with real AI models as needed.
- **Security & Compliance**: Add authentication, authorization, and audit logging.

---

## 5. Customization for Your Industry

- Templates for reports, task types, and legal policies can be adjusted for equipment suppliers.
- Integrate with document signing APIs for customer signatures.

## Acknowledgments

* office leader history sources of inspiration 
* do not use code, images, data etc. from others without permission
* when you have permission to use other people's materials, always mention the original creator and the open source / Creative Commons licence they've used
  <br>For example: [Sleeping Cat on Her Back by Umberto Salvagnin](https://commons.wikimedia.org/wiki/File:Sleeping_cat_on_her_back.jpg#filelinks) / [CC BY 2.0](https://creativecommons.org/licenses/by/2.0)
* etc
