# Task-Manager
Python Based Program - Using Tkinter 
import tkinter as tk
from tkinter import ttk
from tkinter import messagebox
import matplotlib.pyplot as plt
from matplotlib.backends.backend_tkagg import FigureCanvasTkAgg

def login():
    username = entry_username.get()
    password = entry_password.get()

    if username == "admin" and password == "password":
        messagebox.showinfo("Login Successful", "Welcome, {}".format(username))
        radio_button_personal.config(state="normal")
        radio_button_academic.config(state="normal")
    else:
        messagebox.showerror("Login Failed", "Invalid username or password")

def select_task_type():
    selected_task = var.get()
    if selected_task == "Personal":
        notebook.select(1)  # Switch to the Personal tab
    elif selected_task == "Academic":
        notebook.select(2)  # Switch to the Academic tab

def add_personal_task():
    task = entry_personal_task.get()
    if task:
        listbox_personal_tasks.insert(tk.END, task)
        entry_personal_task.delete(0, tk.END)

def cross_off_personal_task():
    selected_task = listbox_personal_tasks.curselection()
    if selected_task:
        listbox_personal_tasks.itemconfig(selected_task, fg="#A6A6A6")

def uncross_personal_task():
    selected_task = listbox_personal_tasks.curselection()
    if selected_task:
        listbox_personal_tasks.itemconfig(selected_task, fg="#464646")

def delete_personal_task():
    selected_task = listbox_personal_tasks.curselection()
    if selected_task:
        listbox_personal_tasks.delete(selected_task)

def delete_crossed_personal_tasks():
    count = 0
    while count < listbox_personal_tasks.size():
        if listbox_personal_tasks.itemcget(count, "fg") == "#A6A6A6":
            listbox_personal_tasks.delete(count)
        else:
            count += 1
def cross_off_academic_task():
    selected_task = listbox_academic_tasks.curselection()
    if selected_task:
        listbox_academic_tasks.itemconfig(selected_task, fg="#A6A6A6")

def uncross_academic_task():
    selected_task = listbox_academic_tasks.curselection()
    if selected_task:
        listbox_academic_tasks.itemconfig(selected_task, fg="#464646")

def delete_academic_task():
    selected_task = listbox_academic_tasks.curselection()
    if selected_task:
        listbox_academic_tasks.delete(selected_task)

def delete_crossed_academic_tasks():
    count = 0
    while count < listbox_academic_tasks.size():
        if listbox_academic_tasks.itemcget(count, "fg") == "#A6A6A6":
            listbox_academic_tasks.delete(count)
        else:
            count += 1

def add_academic_task():
    subject = entry_subject.get()
    study_time = entry_study_time.get()
    study_material = entry_study_material.get()

    if subject and study_time and study_material:
        academic_task = f"Subject: {subject} \n Study Time: {study_time} hours\n Study Material: {study_material}"
        listbox_academic_tasks.insert(tk.END, academic_task)
        entry_subject.delete(0, tk.END)
        entry_study_time.delete(0, tk.END)
        entry_study_material.delete(0, tk.END) 

def create_study_time_graph():
    subjects = []
    study_times = []
    for item in listbox_academic_tasks.get(0, tk.END):
        parts = item.split("\n")
        study_time = parts[1].split(": ")[1]  # Extract the study time
        study_times.append(float(study_time.split()[0]))  # Convert study time to a float
        subjects.append(parts[0].split(": ")[1])

    if study_times:
        fig, ax = plt.subplots(figsize=(6, 6))
        ax.pie(study_times, labels=subjects, autopct='%1.1f%%')
        ax.set_title('Academic Study Time')

        canvas = FigureCanvasTkAgg(fig, master=frame_academic)
        canvas_widget = canvas.get_tk_widget()
        canvas_widget.grid(row=6, column=0, columnspan=3, padx=10, pady=10)

background_color = "#B4EEB4"
text_color = "#03030B"
completed_task_color = "#A6A6A6"

root = tk.Tk()
root.title("Task Manager")
root.geometry("1000x800")
root.configure(bg=background_color)

notebook = ttk.Notebook(root)

frame_login = tk.Frame(notebook)
notebook.add(frame_login, text="Login")
frame_login.config(width=800, height=600)

frame_personal = tk.Frame(notebook)
notebook.add(frame_personal, text="Personal")
frame_personal.config(width=400, height=300)

frame_academic = tk.Frame(notebook)
notebook.add(frame_academic, text="Academic")
frame_academic.config(width=600, height=500)
notebook.pack(pady=10)

label_username = tk.Label(frame_login, text="Username:", fg=text_color)
label_password = tk.Label(frame_login, text="Password:")
label_username.grid(row=0, column=0, padx=10, pady=5)
label_password.grid(row=1, column=0, padx=10, pady=5)

entry_username = tk.Entry(frame_login)
entry_password = tk.Entry(frame_login, show="*")
entry_username.grid(row=0, column=1, padx=10, pady=5)
entry_password.grid(row=1, column=1, padx=10, pady=5)

button_login = tk.Button(frame_login, text="Login", command=login)
button_login.grid(row=2, columnspan=2, pady=10)

var = tk.StringVar()
radio_button_personal = tk.Radiobutton(frame_login, text="Personal", variable=var, value="Personal", state="disabled", command=select_task_type)
radio_button_academic = tk.Radiobutton(frame_login, text="Academic", variable=var, value="Academic", state="disabled", command=select_task_type)
radio_button_personal.grid(row=3, column=0, padx=10, pady=5)
radio_button_academic.grid(row=4, column=0, padx=10, pady=5)

label_personal_task = tk.Label(frame_personal, text="Add Personal Task:", fg=text_color)
label_personal_task.pack(pady=5)
entry_personal_task = tk.Entry(frame_personal)
entry_personal_task.config(fg=text_color)
entry_personal_task.pack(pady=5)

button_add_personal_task = tk.Button(frame_personal, text="Add Personal Task", command=add_personal_task)
button_cross_off_personal_task = tk.Button(frame_personal, text="Cross Completed Personal Task", command=cross_off_personal_task)
button_uncross_personal_task = tk.Button(frame_personal, text="Uncross Personal Task", command=uncross_personal_task, fg= text_color)
button_delete_personal_task = tk.Button(frame_personal, text="Delete Personal Task", command=delete_personal_task)
button_delete_crossed_personal_tasks = tk.Button(frame_personal, text="Delete Crossed Personal Tasks", command=delete_crossed_personal_tasks)

button_add_personal_task.pack(pady=5)
button_cross_off_personal_task.pack(pady=5)
button_uncross_personal_task.pack(pady=5)
button_delete_personal_task.pack(pady=5)
button_delete_crossed_personal_tasks.pack(pady=5)

listbox_personal_tasks = tk.Listbox(frame_personal)
listbox_personal_tasks.pack(pady=10)

label_subject = tk.Label(frame_academic, text="Subject:", fg=text_color)
label_subject.grid(row=0, column=0, columnspan=2, padx=10, pady=5)
entry_subject = tk.Entry(frame_academic)
entry_subject.grid(row=0, column=2, padx=10, pady=5)

label_study_time = tk.Label(frame_academic, text="Study Time (hours):", fg=text_color)
label_study_time.grid(row=1, column=0, columnspan=2, padx=10, pady=5)
entry_study_time = tk.Entry(frame_academic)
entry_study_time.grid(row=1, column=2, padx=10, pady=5)

label_study_material = tk.Label(frame_academic, text="Study Material:", fg=text_color)
label_study_material.grid(row=2, column=0, columnspan=2, padx=10, pady=5)
entry_study_material = tk.Entry(frame_academic)
entry_study_material.grid(row=2, column=2, padx=10, pady=5)

button_add_academic_task = tk.Button(frame_academic, text="Add Academic Task", command=add_academic_task)
button_create_study_time_graph = tk.Button(frame_academic, text="Create Study Time Graph", command=create_study_time_graph)
button_cross_off_academic_task = tk.Button(frame_academic, text="Cross Completed Academic Task", command=cross_off_academic_task)
button_uncross_academic_task = tk.Button(frame_academic, text="Uncross Academic Task", command=uncross_academic_task, fg=text_color)
button_delete_academic_task = tk.Button(frame_academic, text="Delete Academic Task", command=delete_academic_task)
button_delete_crossed_academic_tasks = tk.Button(frame_academic, text="Delete Crossed Academic Tasks", command=delete_crossed_academic_tasks)

button_add_academic_task.grid(row=3, column=0, padx=10, pady=5)
button_create_study_time_graph.grid(row=3, column=2, padx=10, pady=5)
button_cross_off_academic_task.grid(row=4, column=0, padx=10, pady=5)
button_uncross_academic_task.grid(row=4, column=2, padx=10, pady=5)
button_delete_academic_task.grid(row=5, column=0, padx=10, pady=5)
button_delete_crossed_academic_tasks.grid(row=5, column=2, padx=10, pady=5)

listbox_academic_tasks = tk.Listbox(frame_academic, width=70)
listbox_academic_tasks.grid(row=6, column=0, columnspan=3, padx=10, pady=10)

root.mainloop() 
