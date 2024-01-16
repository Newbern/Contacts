# Contacts
## Python
### Modules:
- tkinter
- ttkbootstrap
- json

# Main screen Setup
This is a contacts' booklet that helps organize all those people on your phone. This will help with business and helping
organize who you are working with and also just for information about a person
```bash
import json
import tkinter as tk
import ttkbootstrap as ttk

# Uploading data
with open("data.json", "r") as file:
    data = json.load(file)

# Uploading Display Screen
window = ttk.Window(themename="darkly")
window.wm_state("zoomed")
window.title("Contacts")

Font = "Comic Sans MS"
```
# ListBox Setup
as always get your data and your display window set up for the upcoming project.
```bash
# Search bar Frame
Search_Frame = tk.Frame(master=window)
Search_Frame.pack(anchor="nw", pady=10)

# Search bar
Search = tk.Entry(master=Search_Frame, font=(Font, 15))
Search.pack(side="left")

# Search bar Button
Search_Button = tk.Button(master=Search_Frame, text="Search", command=lambda: Detail(True))
Search_Button.pack(side="left")
```
Then the first thing to set up is our search bar, so we have quick access to who we are searching up we can do this by
typing in there `Name`,`LastName`,`Phone`,`Email`, and `Address`
```bash
# Frame
Frame = tk.Frame(master=window)
Frame.pack(side="left", anchor="n")

# Showing Contacts List
ListBox = tk.Listbox(master=Frame, width=50, height=25, font=(Font, 15))
ListBox.pack(side=tk.LEFT, fill=tk.BOTH)

# Inserting all the contacts
refresh()
```
Then we get us a frame, so we can insert a list full of contacts / data this will make it easier to see what is in front 
of us. 
```bash
# Refreshing window
def refresh():
    ListBox.delete(0, tk.END)
    # Inserting all the contacts
    for num in range(-1, len(data)):
        # Adding contacts list
        if num == -1:
            ListBox.insert(tk.END, "+ Add Contacts")
            continue
        # Inserts all the data in the list
        ListBox.insert(tk.END, f'{data[num]["Name"]} {data[num]["Last_Name"]}')
```
This function will actually upload and refresh our contacts data and just make the code look ka bit sharper
this only updates the `ListBox`.
I had also put this `+ Add Contacts` part into it, so we can add more contacts data

```bash
# Scroll bar
Scroll_bar = tk.Scrollbar(master=Frame, orient="vertical")
Scroll_bar.pack(side=tk.RIGHT, fill=tk.Y)

# Scroll_bar config with the ListBox
ListBox.configure(yscrollcommand=Scroll_bar.set)
Scroll_bar.configure(command=ListBox.yview)
```

Then we upload the Scroll bar, so we can see more of our data without it taking up a huge amount of space.

```bash
# Buttons
Button = tk.Button(master=Frame, text="Select", command=Detail, width=10, height=5)
Button.pack(pady=5)
Button = tk.Button(master=Frame, text="Edit", command=Edit, width=10, height=5)
Button.pack()
Button = tk.Button(master=Frame, text="Delete", command=Del, width=10, height=5)
Button.pack(pady=5)
```

Last but not least this is our buttons, so we can interact without contacts and make adjustments.

# Label Setup
```bash
# Label
lst = []
for _ in range(len(data[0])):
    lst.append(tk.Label(master=FrameLabel))

lst[0].pack(anchor="nw")
lst[1].pack(anchor="w")
lst[2].pack(anchor="ne")
lst[3].pack(anchor="center")
lst[4].pack(pady=25, anchor="sw")

# Frame Details
msg = tk.Message(master=FrameLabel, width=800)
msg.pack()

tk.mainloop()
```
This will on the right side of the screen will add nearly invisible labels that we will use to see our contacts' data.

# Details Setup
```bash
# Information
def Detail(*args):
    # Getting the Selected choice in the list box
    SELCETED = ListBox.get(tk.ANCHOR)

    # getting Search results
    SEARCH = Search.get()

    # Splitting the Word to get the Name of the Person
    try:
        if args:
            Phone = SEARCH
            Name = LastName = ""
        else:
            Name, *LastName = SELCETED.split()
            Phone = ""
            if Name == "+":
                Add()

        # Getting Person data information
        for n in range(len(data)):
            if args:
                for crud in data[n]:
                    if data[n][crud] == Phone:
                        lst[0].config(text=data[n]["Name"], font=(Font, 75))
                        lst[1].config(text=data[n]["Last_Name"], font=(Font, 15))
                        lst[2].config(text=data[n]["Phone"], font=(Font, 15))
                        lst[3].config(text=data[n]["Email"], font=(Font, 15))
                        lst[4].config(text=data[n]["Address"], font=(Font, 15))
                        msg.config(text=data[n]["Details"], font=(Font, 15))
                        break


            elif data[n]["Name"] == Name:
                if data[n]["Last_Name"] == LastName[0]:
                    lst[0].config(text=data[n]["Name"], font=(Font, 75))
                    lst[1].config(text=data[n]["Last_Name"], font=(Font, 15))
                    lst[2].config(text=data[n]["Phone"], font=(Font, 15))
                    lst[3].config(text=data[n]["Email"], font=(Font, 15))
                    lst[4].config(text=data[n]["Address"], font=(Font, 15))
                    msg.config(text=data[n]["Details"], font=(Font, 15))
                    break

    except:
        Name = "Empty"
        for i in range(len(lst)):
            if i == 0:
                lst[i].config(text=Name, font=(Font, 75))
                continue
            else:
                lst[i].config(text="", font=(Font, 75))
```
This right here does 2 things first it will grab the `ListBox` that is selected and then divide it by the `FirstName` 
and the `LastName` and scan the data to find it if it does not it will print the Label as Empty.
And if it does show something back then it will put the information in its label in the respected manner.
The second thing this function does is that it will use the search bar and find the data that way and if not then return
Empy back as well.

# Add Setup
```bash
# Add contacts Pop up screen
def Add():
    Add_box = tk.Toplevel(master=window)
    Add_box.title("Adding contacts")
    Add_box.geometry("500x500")

    # Name
    Add_box_Name = tk.Label(master=Add_box, text="Name:")
    Add_box_Name_text = tk.Entry(master=Add_box)
    Add_box_Name.pack()
    Add_box_Name_text.pack()

    # Last Name
    Add_box_Last_Name = tk.Label(master=Add_box, text="Last_Name:")
    Add_box_Last_Name_text = tk.Entry(master=Add_box)
    Add_box_Last_Name.pack()
    Add_box_Last_Name_text.pack()

    # Phone
    Add_box_Phone = tk.Label(master=Add_box, text="Phone:")
    Add_box_Phone_text = tk.Entry(master=Add_box)
    Add_box_Phone.pack()
    Add_box_Phone_text.pack()

    # Done Button
    Add_box_Button = tk.Button(master=Add_box, text="Done", width=10, height=2, command=lambda: Add_Contacts(
        Add_box_Name_text.get(),
        Add_box_Last_Name_text.get(),
        Add_box_Phone_text.get(),
        Add_box_Email_text.get(),
        Add_box_Address_text.get(),
        Add_box_Details_text.get("1.0", tk.END)))

    Add_box_Button.pack(padx=20, anchor="ne")

    # Email
    Add_box_Email = tk.Label(master=Add_box, text="Email:")
    Add_box_Email_text = tk.Entry(master=Add_box)
    Add_box_Email.pack()
    Add_box_Email_text.pack()

    # Address
    Add_box_Address = tk.Label(master=Add_box, text="Address:")
    Add_box_Address_text = tk.Entry(master=Add_box)
    Add_box_Address.pack()
    Add_box_Address_text.pack()

    # Details
    Add_box_Details = tk.Label(master=Add_box, text="Details:")
    Add_box_Details_text = tk.Text(master=Add_box)
    Add_box_Details.pack()
    Add_box_Details_text.pack()
```
So the First thing this does is actually throw a Pop-up screen, and you actually just fill in the information
and press done. 
It will transfer your input to its data function that sorts and sets up a new contact.
```bash
 # Add contacts Data
    def Add_Contacts(Name, Last_Name, Phone, Email, Address, Details):
        if Name != "":
            if Last_Name != "":
                data.append(
                    {
                        "Name": Name,
                        "Last_Name": Last_Name,
                        "Phone": "-".join([Phone[:3], Phone[3:6], Phone[6:]]),
                        "Email": Email,
                        "Address": Address,
                        "Details": Details
                    })
                with open("data.json", "w") as New_data:
                    json.dump(data, New_data, indent=3)
                Add_box.destroy()
                refresh()
```
This also destroys the pop-up window and refreshes the ListBox.

# Deleting Setup
```bash
# Deleting Contacts
def Del():
    # Getting Selected contacts
    SELECTED = ListBox.get(tk.ANCHOR)
    Name, *LastName = SELECTED.split()

    # Won’t run if "+ Add contacts" is selected
    if Name != "+":
        # Pop up screen
        Del_box = tk.Toplevel(master=window)
        Del_box.geometry("500x500")
        Del_box.title("Contacts to be Deleted")

        # Text Label
        Del_box_Label = tk.Label(master=Del_box, text=f"Are you sure \n you want to delete \n {SELECTED}?", font=(Font, 15))
        Del_box.configure()
        Del_box_Label.pack()

        # Yes_Button
        Del_box_Button = tk.Button(master=Del_box, text="Yes", width=15, height=10,
                                   command=lambda: Del_Contacts(Name, LastName[0]))
        Del_box_Button.pack(padx=75, side="left")

        # No_Button
        Del_box_Button = tk.Button(master=Del_box, text="No", width=15, height=10, command=Del_box.destroy)
        Del_box_Button.pack(padx=50, side="right")
```
This right here just throws up a pop-up screen and ask if you want to confirm deleting then just goes to the deleting data
and goes through with it.
```bash
    def Del_Contacts(First, Last):
        for n in range(len(data)):
            if data[n]["Name"] == First:
                if data[n]["Last_Name"] == Last:
                    data.remove(data[n])
                    break

        with open("data.json", "w") as New_data:
            json.dump(data, New_data, indent=3)

        Del_box.destroy()
        refresh()
```
# Editing Setup

```bash
# Edit
def Edit():
    # Pop-up Screen
    Edit_box = tk.Toplevel(master=window)
    Edit_box.geometry("500x500")
    Edit_box.title("Edit")

    SELCETED = ListBox.get(tk.ANCHOR)
    Name, *Last_Name = SELCETED.split()

    for n in range(len(data)):
        if data[n]["Name"] == Name:
            if data[n]["Last_Name"] == Last_Name[0]:
                SELCETED = data[n]
                break

    # Name
    Edit_box_Name = tk.Label(master=Edit_box, text=f"{SELCETED['Name']} to :")
    Edit_box_Name_text = tk.Entry(master=Edit_box)
    Edit_box_Name.grid(row=0, column=2)
    Edit_box_Name_text.grid(row=0, column=3)

    # Last Name
    Edit_box_Last_Name = tk.Label(master=Edit_box, text=f"{SELCETED['Last_Name']} to :")
    Edit_box_Last_Name_text = tk.Entry(master=Edit_box)
    Edit_box_Last_Name.grid(row=2, column=2)
    Edit_box_Last_Name_text.grid(row=2, column=3)

    # Phone
    Edit_box_Phone = tk.Label(master=Edit_box, text=f"{SELCETED['Phone']} to :")
    Edit_box_Phone_text = tk.Entry(master=Edit_box)
    Edit_box_Phone.grid(row=3, column=2)
    Edit_box_Phone_text.grid(row=3, column=3)

    # Email
    Edit_box_Email = tk.Label(master=Edit_box, text=f"{SELCETED['Email']} to :")
    Edit_box_Email_text = tk.Entry(master=Edit_box)
    Edit_box_Email.grid(row=5, column=2)
    Edit_box_Email_text.grid(row=5, column=3)

    # Address
    Edit_box_Address = tk.Label(master=Edit_box, text=f"{SELCETED['Address']} to :")
    Edit_box_Address_text = tk.Entry(master=Edit_box)
    Edit_box_Address.grid(row=6, column=2)
    Edit_box_Address_text.grid(row=6, column=3)

    # Details
    Edit_box_Details = tk.Label(master=Edit_box, text=f"Details:")
    Edit_box_Details_text = tk.Text(master=Edit_box, width=20, height=10)
    Edit_box_Details.grid(row=7, column=2)
    Edit_box_Details_text.grid(row=8, column=3)

    # Done Button
    Edit_box_Button = tk.Button(master=Edit_box, text="Done", width=10, height=2, command=lambda: Edit_Contacts(
        Name,
        Last_Name[0],
        [Edit_box_Name_text.get(),
         Edit_box_Last_Name_text.get(),
         Edit_box_Phone_text.get(),
         Edit_box_Email_text.get(),
         Edit_box_Address_text.get(),
         Edit_box_Details_text.get("1.0", tk.END)]
    ))
    Edit_box_Button.grid(row=9, column=2)
```
So all I did with this one was again, made a pop-up screen, and then I actually made a grid which went really well and made
it look nice.
But just like the other one this grabbed the data and went into another function.

```bash
    def Edit_Contacts(OG_Name, OG_LastName, info):

        for num in range(len(data)):
            if data[num]["Name"] == OG_Name:
                if data[num]["Last_Name"] == OG_LastName:
                    if info[0] != "":
                        data[num]["Name"] = info[0]

                    if info[1] != "":
                        data[num]["Last_Name"] = info[1]

                    if info[2] != "":
                        Phone = info[2]
                        data[num]["Phone"] = "-".join([Phone[:3], Phone[3:6], Phone[6:]])

                    if info[3] != "":
                        data[num]["Email"] = info[3]

                    if info[4] != "":
                        data[num]["Address"] = info[4]

                    if info[5] != "":
                        if info[5] != "\n":
                            data[num]["Details"] = info[5]
                break
        with open("data.json", "w") as New_data:
            json.dump(data, New_data, indent=3)

        Edit_box.destroy()
        refresh()
```
So the first thing this does check to see if you actually typed anything in those input entry's and then sets up the 
new data with that and then just closes then window and refreshes the `ListBox` again.

Keep losing that one friends phone number? having trouble sorting through business numbers and personal number? This is a very simple, very easy DIY project that will have you going crazy this is my personal Contacts list it helps me sort through friends and work numbers as well as giving extra details as to why im helping this person.
