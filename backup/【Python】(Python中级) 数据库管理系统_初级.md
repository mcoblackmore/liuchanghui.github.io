# 项目要点

Python tkinter界面 + sqlite3数据库

# 项目文件

- lab.py: 主程序
- database.py: sqlite3数据库基本操作，包含建表、查表、插入、删除、更新、查询id存在性
- Employees.db:默认操作的sqlite3数据库

# 项目界面

<img width="677" alt="sf" src="https://github.com/mcoblackmore/liuchanghui.github.io/assets/49425642/2b2d38fd-67ce-4563-a7cf-7a0f72a8f51d">

# 项目文件
[lab.py.pdf](https://github.com/user-attachments/files/15956051/lab.py.pdf)
[database.py.pdf](https://github.com/user-attachments/files/15956057/database.py.pdf)

# 项目代码

## database.py

```Python
import sqlite3
def create_table():
    conn = sqlite3.connect('Employees.db')
    cursor = conn.cursor()
    cursor.execute('''CREATE TABLE IF NOT EXISTS Employees(
                   id TEXT PRIMARY KEY,
                   name TEXT,
                   role TEXT,
                   gender TEXT,
                   status TEXT)''')
    conn.commit()
    conn.close()

def fetch_employees():
    conn = sqlite3.connect('Employees.db')
    cursor = conn.cursor()
    cursor.execute("SELECT * FROM Employees")
    employees = cursor.fetchall()
    conn.close()
    return employees
def insert_employee(id, name, role, gender, status):
    conn = sqlite3.connect('Employees.db')
    cursor = conn.cursor()
    cursor.execute("INSERT INTO Employees(id, name, role, gender, status) VALUES(?,?,?,?,?)", 
                   (id, name, role, gender, status))
    conn.commit()
    conn.close()

def delete_employee(id):
    conn = sqlite3.connect('Employees.db')
    cursor = conn.cursor()
    cursor.execute("DELETE FROM Employees WHERE id=?", (id,))
    conn.commit()
    conn.close()
def update_employee(new_name, new_role, new_gender, new_status, id):
    conn = sqlite3.connect('Employees.db')
    cursor = conn.cursor()
    cursor.execute("UPDATE Employees SET name=?, role=?, gender=?, status=? WHERE id=?", 
                   (new_name, new_role, new_gender, new_status, id))
    conn.commit()
    conn.close()
def id_exists(id):
    conn = sqlite3.connect('Employees.db')
    cursor = conn.cursor()
    cursor.execute("SELECT count(*) FROM Employees WHERE id=?", (id,))
    result = cursor.fetchone()
    conn.close()
    return result[0] > 0

create_table()

```

## lab.py

```Python
import customtkinter # 导入customtkinter库,这个好看的库
from tkinter import *
from tkinter import messagebox
from tkinter import ttk
import tkinter as tk
import database
from PIL import ImageTk,Image

app = customtkinter.CTk()  # 这个是创建了一个CTk对象，用来创建窗口
app.title("Employee Management System")
app.geometry("900x420")
app.config(bg='#161c25')
app.resizable(False, False)

font1 = ('Arial',20,'bold')
font2 = ('Arial',12,'bold')
# 添加到树视图
def add_to_treeview():
    emplyees = database.fetch_employees()
    tree.delete(*tree.get_children())
    for employee in emplyees:
        tree.insert('', END, values=employee)
# 清空树视图输入框
def clear(*clicked):
    if clicked:
        tree.selection_remove(tree.focus())
        tree.focus('') # 取消选中状态
    id_entry1.delete(0, END)
    name_entry.delete(0, END)
    role_entry.delete(0, END)
    variable1.set('男')
    status_entry.delete(0, END)

def display_data(event):#等待树视图双击事件
    selected_item = tree.focus()
    if selected_item:
        row = tree.item(selected_item)['values']
        clear() # 清空输入框所有内容
        id_entry1.insert(0, row[0])
        name_entry.insert(0, row[1])
        role_entry.insert(0, row[2])
        variable1.set(row[3])
        status_entry.insert(0, row[4])
    else:
        pass
def delete():
    selected_item = tree.focus()
    if not selected_item:
        messagebox.showerror("Error", "请选择要删除的员工")
    else:
        id = id_entry1.get()
        database.delete_employee(id) # 删除数据库中的数据
        add_to_treeview() # 更新树视图
        clear() # 清空输入框所有内容
        messagebox.showinfo("Success", "删除成功")

def update():
    selected_item = tree.focus()
    if not selected_item:
        messagebox.showerror("Error", "请选择要更新的员工")
    else:
        id = id_entry1.get()
        name = name_entry.get()
        role = role_entry.get()
        gender = variable1.get()
        status = status_entry.get()
        database.update_employee(name, role, gender, status,id) # 更新数据库中的数据
        add_to_treeview() # 更新树视图
        clear() # 清空输入框所有内容
        messagebox.showinfo("牛逼", "更新成功")


# 插入数据
def insert():
    id = id_entry1.get()
    name = name_entry.get()
    role = role_entry.get()
    gender = variable1.get()
    status = status_entry.get()
    if not(id and name and role and gender and status):
        messagebox.showerror("Error", "请填充所有的值")
    elif database.id_exists(id):
        messagebox.showerror("Error", "ID已存在")
    else:
        database.insert_employee(id, name, role, gender, status)
        add_to_treeview()
        clear()
        messagebox.showinfo("Success", "添加成功")
        #clear_input()
        
################################################


top = PhotoImage(file="lfcg1.png")
top_image = Label(app, image=top, bg="#f0f1f5")
top_image.place(x=30, y=30)

title = customtkinter.CTkLabel(app, font=font1, text="精神病院员工管理系统", 
                               text_color="#fff",bg_color="#161c25")
title.place(x=60, y=30)

id_label1 = customtkinter.CTkLabel(app, font=font1, text="编  号:", 
                                   text_color="#fff",bg_color="#161c25")
id_label1.place(x=20, y=80)

id_entry1 = customtkinter.CTkEntry(app, font=font1, text_color="#000",
                                   fg_color="#fff",border_color="#0c9295",
                                   border_width=2,width=180)
id_entry1.place(x=100, y=80)

name_label = customtkinter.CTkLabel(app, font=font1, text="姓  名:", 
                                    text_color="#fff",bg_color="#161c25")
name_label.place(x=20, y=120)

name_entry = customtkinter.CTkEntry(app, font=font1, text_color="#000",
                                   fg_color="#fff",border_color="#0c9295",
                                   border_width=2,width=180)
name_entry.place(x=100, y=120)

role_label = customtkinter.CTkLabel(app, font=font1, text="角  色:", 
                                    text_color="#fff",bg_color="#161c25")
role_label.place(x=20, y=160)

role_entry = customtkinter.CTkEntry(app, font=font1, text_color="#000",
                                   fg_color="#fff",border_color="#0c9295",
                                   border_width=2,width=180)
role_entry.place(x=100, y=160)

gender_label = customtkinter.CTkLabel(app, font=font1, text="性  别:", 
                                      text_color="#fff",bg_color="#161c25")
gender_label.place(x=20, y=200)

options = ['男', '女', '跨性别']
variable1=StringVar()

gender_options = customtkinter.CTkComboBox(app,font=font1,
                                           text_color="#000",fg_color="#fff",
                                           dropdown_hover_color="#0c9295",button_color="#0c9295",
                                           button_hover_color="#0c9295",border_color="#0c9295",
                                           width=180,variable=variable1,values=options,state="readonly")
gender_options.set('男')
gender_options.place(x=100, y=200)

status_label = customtkinter.CTkLabel(app, font=font1, text="当前状态:", 
                                       text_color="#fff",bg_color="#161c25")
status_label.place(x=20, y=240)

status_entry = customtkinter.CTkEntry(app, font=font1, text_color="#000",
                                      fg_color="#fff",border_color="#0c9295",
                                      border_width=2,width=180)
status_entry.place(x=100, y=240)
#############################BUTTON按钮###################################
add_button = customtkinter.CTkButton(app,command=insert, text="添加员工", font=font1, 
                                     text_color="#fff",fg_color="#05a312",
                                     hover_color="#00850b",bg_color="#161c25",
                                     cursor="hand2",corner_radius=15,width=260
                                     )
add_button.place(x=20, y=310)

clear_button = customtkinter.CTkButton(app, command=lambda:clear(True), text="清空输入框", font=font1, 
                                       text_color="#fff",fg_color="#05a312",
                                       hover_color="#00850b",bg_color="#161c25",
                                       cursor="hand2",corner_radius=15,width=260
                                       )
clear_button.place(x=20, y=360)

update_button = customtkinter.CTkButton(app, command=update, text="更新信息", font=font1, 
                                         text_color="#fff",fg_color="#05a312",
                                          hover_color="#00850b",bg_color="#161c25",
                                           cursor="hand2",corner_radius=15,width=260)
update_button.place(x=300, y=360)

delete_button = customtkinter.CTkButton(app,command=delete, text="删除员工", font=font1, 
                                         text_color="#fff",fg_color="red",
                                          hover_color="#00850b",bg_color="#161c25",
                                           cursor="hand2",corner_radius=15,width=260)
delete_button.place(x=580, y=360)
#####################################
style = ttk.Style(app)
style.theme_use('clam')
style.configure("Treeview", font=font2, rowheight=30, 
                fieldbackground="#161c25", background="#fff", foreground="#000")
style.map("Treeview", background=[("selected", "#1a8f2d")])
tree = ttk.Treeview(app, height=15)
tree['columns'] = ('ID', 'Name', 'Role', 'Gender', 'Status')
tree.column('#0', width=0, stretch=tk.NO) # 这个是设置第一列的宽度
tree.column('ID', anchor=tk.CENTER, width=80)# 这个是设置第二列的宽度
tree.column('Name', anchor=tk.CENTER, width=170)
tree.column('Role', anchor=tk.CENTER, width=170)
tree.column('Gender', anchor=tk.CENTER, width=170)
tree.column('Status', anchor=tk.CENTER, width=170)

tree.heading('#0', text='ID')  #这个是设置表头的名字
tree.heading('ID', text='ID')
tree.heading('Name', text='Name')
tree.heading('Role', text='Role')
tree.heading('Gender', text='Gender')
tree.heading('Status', text='Status')

tree.place(x=500, y=20)
tree.bind('<ButtonRelease>',display_data)
add_to_treeview()

app.mainloop()

```