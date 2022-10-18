# Network-Hawkeye
Application for Detection and Analysis of Network Vulnerabilities
from tkinter import *
import os
from PIL import Image, ImageTk
import tkinter.ttk as ttk
import csv
from filter import *

r = Tk()
r.title("Network Hawkeye")
r.geometry("1000x1000")

button_1 = Button(r,text = "Exit",command=exit)
button_1.place(x=50, y=20)
button_1.pack()

options = [
    "HyperText Transfer Protocol (HTTP)",
    "File Transfer Protocol (FTP)",
    "Transmission Control Protocol (TCP)",
    "User Datagram Protocol (UDP)",
    "Domain Name System (DNS)"
]

def show():
    label.config( text = clicked.get() )
    
    if clicked.get()==options[0]:
        http_filter()
        csv_func()        
    elif clicked.get()==options[1]:
        ftp_filter()
        csv_func()
    elif clicked.get()==options[2]:
        tcp_filter()
        csv_func()
    elif clicked.get()==options[3]:
        udp_filter()
        csv_func()
    elif clicked.get()==options[4]:
        dns_filter()
        csv_func()
        

# datatype of menu text
clicked = StringVar()  
# initial menu text
clicked.set( "Filter by Protocol" )
  
# Create Dropdown menu
drop = OptionMenu( r , clicked , *options,command=all_filter )
drop.pack()
  
# Create button, it will change label text
button = Button( r , text = "Apply Filter" , command = show ).pack()

label = Label( r , text = " " )
label.pack()

width = 530
height = 400
screen_width = r.winfo_screenwidth()
screen_height = r.winfo_screenheight()
x = (screen_width/2) - (width/2)
y = (screen_height/2) - (height/2)
r.geometry("%dx%d+%d+%d" % (width, height, x, y))
r.resizable(0, 0)
TableMargin = Frame(r, width=500)
TableMargin.pack(side=TOP)
scrollbarx = Scrollbar(TableMargin, orient=HORIZONTAL)
scrollbary = Scrollbar(TableMargin, orient=VERTICAL)
tree = ttk.Treeview(TableMargin, columns=("frame.number", "ip.src", "ip.dst"), height=400, selectmode="extended", yscrollcommand=scrollbary.set, xscrollcommand=scrollbarx.set)
scrollbary.config(command=tree.yview)
scrollbary.pack(side=RIGHT, fill=Y)
scrollbarx.config(command=tree.xview)
scrollbarx.pack(side=BOTTOM, fill=X)
tree.heading('frame.number', text="frame.number", anchor=W)
tree.heading('ip.src', text="Source IP", anchor=W)
tree.heading('ip.dst', text="Destination IP", anchor=W)
#tree.heading('ip.proto',text="IP Proto", anchor=W)
tree.column('#0', stretch=NO, minwidth=0, width=0)
tree.column('#1', stretch=NO, minwidth=0, width=200)
tree.column('#2', stretch=NO, minwidth=0, width=200)
#tree.column('#3', stretch=NO, minwidth=0, width=300)
#tree.column('#4', stretch=NO, minwidth=0, width=300)

def csv_func():
    with open(r'C:\Users\anany\Desktop\testtfile.csv') as f:
        reader = csv.DictReader(f, delimiter=',')
        for row in reader:
            frame_number = row['frame.number']
            Source = row['ip.src']
            Destination = row['ip.dst']
            #Info=row['ip.proto']
            tree.insert("", 0, values=(frame_number,Source,Destination))

tree.pack()
r.mainloop()
