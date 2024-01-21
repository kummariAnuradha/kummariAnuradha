#Command-Line Interface (CLI):


import datetime

tasks = []

def add_task():
    """Adds a task to the list."""
    task = input("Enter task description: ")
    tasks.append({"task": task, "completed": False, "added_on": datetime.datetime.now()})
    print("Task added successfully!")

def view_tasks():
    """Displays all tasks in the list."""
    if not tasks:
        print("No tasks to display.")
        return
    for index, task in enumerate(tasks):
        print(f"{index + 1}. {task['task']} - {'Completed' if task['completed'] else 'Pending'}")

def mark_task_done():
    """Marks a task as completed."""
    view_tasks()
    if not tasks:
        return
    index = int(input("Enter task index to mark as done: ")) - 1
    if 0 <= index < len(tasks):
        tasks[index]["completed"] = True
        print(f"Task '{tasks[index]['task']}' marked as done!")
    else:
        print("Invalid task index.")

def delete_task():
    """Deletes a task from the list."""
    view_tasks()
    if not tasks:
        return
    index = int(input("Enter task index to delete: ")) - 1
    if 0 <= index < len(tasks):
        removed_task = tasks.pop(index)
        print(f"Task '{removed_task['task']}' deleted successfully!")
    else:
        print("Invalid task index.")

while True:
    print("\nTo-Do List")
    print("1. Add Task")
    print("2. View Tasks")
    print("3. Mark Task as Done")
    print("4. Delete Task")
    print("5. Exit")

    choice = input("Enter your choice: ")

    if choice == "1":
        add_task()
    elif choice == "2":
        view_tasks()
    elif choice == "3":
        mark_task_done()
    elif choice == "4":
        delete_task()
    elif choice == "5":
        break
    else:
        print("Invalid choice. Please try again.")

#Web-Based Application (using Flask):


from flask import Flask, render_template, request, redirect, url_for

app = Flask(_name_)
tasks = []

@app.route("/")
def home():
    return render_template("index.html", tasks=tasks)

@app.route("/add", methods=["POST"])
def add_task():
    task = request.form["task"]
    tasks.append({"task": task, "completed": False})
    return redirect(url_for("home"))

@app.route("/mark_done/<int:task_id>")
def mark_task_done(task_id):
    tasks[task_id]["completed"] = True
    return redirect(url_for("home"))

@app.route("/delete/<int:task_id>")
def delete_task(task_id):
    tasks.pop(task_id)
    return redirect(url_for("home"))

if _name_ == "_main_":
    app.run(debug=True)

#index.html (template for web app):

HTML
<!DOCTYPE html>
<html>
<head>
    <title>To-Do List</title>
</head>
<body>
    <h1>To-Do List</h1>
    <form action="/add" method="POST">
        <input type="text" name="task" placeholder="Enter task">
        <button type="submit">Add Task</button>
    </form>
    <ul>
        {% for task in tasks %}
        <li>
            {{ task["task"] }} -
            {% if task["completed"] %}
            <span style>
