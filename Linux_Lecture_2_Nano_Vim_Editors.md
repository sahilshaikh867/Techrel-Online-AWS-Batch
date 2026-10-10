# Linux Lecture 2 — Text Editors: Nano & Vi/Vim

## Lecture Goal

By the end of this lecture, students should be able to:

- Understand what a text editor is.
- Understand why text editors are important in Linux.
- Understand why Linux administrators and DevOps engineers use terminal editors.
- Create and edit a file using **Nano**.
- Understand the basic idea of **Vi/Vim**.
- Understand Vim modes.
- Perform basic editing, copying, deleting, searching, saving, and exiting in Vim.
- Complete a small hands-on Linux editing task.

> **Source basis:** This lecture follows the sequence and commands in the provided `Linux-Notes.pdf`, especially **Chapter 2: Nano & Vi/Vim Editors**. The PDF specifically covers Nano creation/save/exit and Vi/Vim modes and commands.

---

# 1. What Is a Text Editor?

A **text editor** is a program used to create, open, modify, and save text files.

In Linux, we frequently work with text files such as:

- Configuration files
- Shell scripts
- Application configuration
- Service configuration
- Documentation
- Source code
- Log-related text

A text editor allows us to change the content of these files.

### Simple Example

Suppose we want to create:

```text
hello.txt
```

We can open the file in a text editor, type:

```text
Hello Linux
```

and save it.

---

# 2. Why Do We Need Text Editors in Linux?

Linux is heavily used through the **Command Line Interface (CLI)**.

When working on a Linux server, we may not have a graphical desktop environment.

For example, a DevOps engineer may connect to a remote server using SSH:

```bash
ssh user@server-ip
```

After connecting, we may need to edit a configuration file or create a script directly from the terminal.

This is where terminal-based editors become important.

### Common situations

We may need an editor to:

1. Create a shell script.
2. Edit configuration files.
3. Modify application settings.
4. Write commands into a script.
5. Update server configuration.
6. Make quick changes on a remote Linux server.

---

# 3. GUI Editor vs Terminal Editor

There are two common ways to edit files.

## GUI Editor

Examples:

- VS Code
- Notepad
- Other graphical editors

These provide buttons, menus, and a visual interface.

## Terminal-Based Editor

Examples:

- Nano
- Vi
- Vim

These work directly inside the terminal.

### Why Terminal Editors Matter

Imagine:

```text
Your Laptop
     |
     | SSH
     v
Remote Linux Server
     |
     +-- No Desktop GUI
     |
     +-- Terminal
```

You cannot depend on a graphical editor on every server.

So knowing a terminal editor is an important Linux skill.

---

# 4. Editors Covered in This Lecture

According to the Linux notes, this chapter focuses on:

1. **Nano**
2. **Vi/Vim**

We will learn Nano first because it is beginner-friendly, then move to Vim.

---

# 5. Nano Editor

## What is Nano?

**Nano** is a simple, beginner-friendly terminal text editor.

The notes describe Nano as simple and usable directly with normal typing.

---

# 6. Why Use Nano?

Nano is useful when we want a simple editing experience from the terminal.

For beginners, Nano is easier to understand because:

- You can start typing directly.
- It works inside the terminal.
- Common save and exit shortcuts are simple.
- It is useful for quick file editing.

---

# 7. Creating a File Using Nano

The basic command from the notes is:

```bash
nano example.txt
```

This opens `example.txt` in Nano.

If the file does not already exist, Nano can be used to create it.

### Example

```bash
nano hello.txt
```

Then type:

```text
Hello Linux
This is my first Nano file.
```

---

# 8. Saving and Exiting Nano

According to the notes:

### Step 1 — Exit / Save Prompt

Press:

```text
Ctrl + X
```

Nano will ask about saving changes.

### Step 2 — Save

Press:

```text
Y
```

### Step 3 — Confirm Filename

Press:

```text
Enter
```

### Remember

```text
Ctrl + X
   ↓
Y
   ↓
Enter
```

---

# 9. Nano Practical

Run:

```bash
nano student.txt
```

Type:

```text
Name: Student
Course: Linux
Topic: Text Editors
```

Then:

```text
Ctrl + X
Y
Enter
```

Verify the file:

```bash
cat student.txt
```

---

# 10. What is Vi?

**Vi** is a terminal-based text editor.

It is different from simple editors because it uses different modes for different operations.

The Linux notes introduce Vi/Vim through its modes.

---

# 11. What is Vim?

**Vim** is a powerful text editor based on the Vi editor.

One important concept for beginners is:

> Vim does not behave like a normal editor where every key simply types text.

Instead, the keyboard behaves differently depending on the current **mode**.

---

# 12. Why Is Vim Important?

Vim is useful when working directly from a Linux terminal.

For example:

```text
Developer
   |
   | SSH
   v
Linux Server
   |
   +-- Terminal
         |
         +-- Vim
```

A developer or administrator can edit files without leaving the terminal.

---

# 13. Vim Modes

According to the notes, Vim has four important modes:

1. **Command Mode**
2. **Insert Mode**
3. **Execution Mode**
4. **Visual Mode**

Understanding modes is the most important beginner concept in Vim.

---

# 14. Command Mode

Command Mode is the default mode.

It is used for actions such as:

- Navigation
- Delete
- Copy
- Paste
- Other editing operations

Examples:

```text
dd
yy
p
u
```

---

# 15. Insert Mode

Insert Mode is used to type text.

Common ways to enter Insert Mode from Command Mode:

```text
i
```

Insert at the cursor.

```text
o
```

Create a new line below.

```text
O
```

Create a new line above.

### Important

Press:

```text
Esc
```

to return to Command Mode.

---

# 16. Execution Mode

Execution Mode is activated using:

```text
:
```

It is used for commands such as saving, quitting, searching, and other operations.

Examples:

```text
:w
:q
:wq
:q!
```

---

# 17. Visual Mode

Visual Mode is used to select text before performing an action.

The notes cover:

```text
v
```

Character-wise selection.

```text
V
```

Line-wise selection.

```text
Ctrl + v
```

Block/column selection.

After selecting text, we can perform actions such as:

```text
y
```

Copy

```text
d
```

Delete

```text
c
```

Cut/change

---

# 18. The Most Important Vim Rule

For beginners, remember:

```text
ESC
```

If you are confused about what mode you are in, press:

```text
Esc
```

This returns you to Command Mode.

---

# 19. Vim Basic Navigation

The notes include:

```text
gg
```

Go to the beginning of the file.

```text
G
```

Go to the end of the file.

```text
ngg
```

Go to line number `n`.

Example:

```text
10gg
```

Go to line 10.

---

# 20. Delete Commands

### Delete Current Line

```text
dd
```

### Delete Current Word

```text
dw
```

### Delete Multiple Lines

```text
ndd
```

Example:

```text
3dd
```

Delete 3 lines.

---

# 21. Copy / Yank Commands

In Vim, copying is commonly called **yanking**.

### Copy Current Line

```text
yy
```

### Copy Current Word

```text
yw
```

### Copy Multiple Lines

```text
nyy
```

Example:

```text
3yy
```

Copy 3 lines.

---

# 22. Paste

The notes use:

```text
P
```

to paste copied/selected content.

Practice copying a line and then pasting it.

---

# 23. Undo and Redo

### Undo

```text
u
```

### Redo

```text
Ctrl + r
```

---

# 24. Save and Exit Vim

These are extremely important commands.

## Quit

```text
:q
```

Quit without saving when there are no unsaved changes.

## Force Quit Without Saving

```text
:q!
```

Quit and discard changes.

## Save

```text
:w
```

Save the file.

## Save and Quit

```text
:wq
```

or:

```text
:x
```

## Force Save and Quit

```text
:wq!
```

---

# 25. Vim Command Summary

| Command | Purpose |
|---|---|
| `i` | Enter Insert Mode |
| `o` | New line below |
| `O` | New line above |
| `Esc` | Return to Command Mode |
| `dd` | Delete line |
| `dw` | Delete word |
| `yy` | Copy line |
| `yw` | Copy word |
| `p/P` | Paste |
| `u` | Undo |
| `Ctrl + r` | Redo |
| `gg` | Beginning of file |
| `G` | End of file |
| `ngg` | Go to line n |
| `:w` | Save |
| `:q` | Quit |
| `:q!` | Quit without saving |
| `:wq` | Save and quit |
| `:x` | Save and quit |

---

# 26. Searching in Vim

The notes include searching using:

```text
:/word
```

Example:

```text
:/Linux
```

This searches for the word:

```text
Linux
```

---

# 27. Find and Replace

The notes provide:

```text
:%s/old/new/g
```

Meaning:

- `old` → text to find
- `new` → replacement text
- `g` → replace all occurrences

Example:

```text
:%s/Linux/Ubuntu/g
```

This replaces occurrences of `Linux` with `Ubuntu`.

---

# 28. Display Line Numbers

Show line numbers:

```text
:set nu
```

Hide line numbers:

```text
:set nonu
```

---

# 29. Running a Terminal Command from Vim

The notes also include:

```text
:!command
```

This can be used to run a terminal command from Vim.

Example:

```text
:!ls
```

---

# 30. Nano vs Vim

| Feature | Nano | Vim |
|---|---|---|
| Beginner friendly | Very easy | Takes practice |
| Interface | Simple | Mode-based |
| Typing | Direct | Insert Mode required |
| Editing power | Basic | Powerful |
| Modes | No complex modes | Multiple modes |
| Quick editing | Excellent | Excellent |
| Advanced editing | Limited | Very powerful |

---

# 31. Beginner Practice — Nano

### Task 1

Create:

```bash
nano linux.txt
```

Write:

```text
Linux is an operating system.
Linux is widely used in servers.
Linux is important for DevOps.
```

Save and exit.

Verify:

```bash
cat linux.txt
```

---

# 32. Beginner Practice — Vim

Create:

```bash
vim linux.txt
```

### Step 1

Press:

```text
i
```

### Step 2

Type:

```text
Linux
DevOps
Cloud
Docker
AWS
```

### Step 3

Press:

```text
Esc
```

### Step 4

Try:

```text
dd
```

Delete a line.

### Step 5

Try:

```text
u
```

Undo.

### Step 6

Try:

```text
yy
```

Copy a line.

### Step 7

Try:

```text
p
```

Paste.

### Step 8

Save and exit:

```text
:wq
```

---

# 33. Mini Lab

Complete the following without using a GUI editor.

### Requirement

Create a file:

```bash
vim employee.txt
```

Add:

```text
Employee Name: Rahul
Department: DevOps
Skill: Linux
Skill: AWS
Skill: Docker
```

Then:

1. Add one more line.
2. Delete one line.
3. Copy one line.
4. Paste it.
5. Undo one change.
6. Search for `AWS`.
7. Show line numbers.
8. Save and exit.

Useful commands:

```text
i
Esc
dd
yy
p
u
:/AWS
:set nu
:wq
```

---

# 34. Real-World DevOps Example

Imagine you are working on a Linux server.

You connect:

```bash
ssh user@server
```

You need to modify a configuration file.

You can use:

```bash
vim config.txt
```

Edit the file:

```text
i
```

Make the required changes.

Then:

```text
Esc
:wq
```

The file is saved and Vim exits.

### Mental Model

```text
SSH
 ↓
Linux Server
 ↓
Terminal
 ↓
Vim
 ↓
Edit Configuration
 ↓
Save
 ↓
Application Uses Configuration
```

---

# 35. Common Beginner Mistakes in Vim

### Mistake 1 — Typing does not work

You may be in Command Mode.

Solution:

```text
i
```

Then type.

### Mistake 2 — You cannot exit

Press:

```text
Esc
```

Then:

```text
:q
```

or:

```text
:q!
```

### Mistake 3 — You accidentally changed something

Use:

```text
u
```

### Mistake 4 — You want to save

Use:

```text
:w
```

### Mistake 5 — You want to save and exit

Use:

```text
:wq
```

---

# 36. Interview Questions

### Q1. What is a text editor?

A text editor is a program used to create and modify text files.

### Q2. Why are terminal editors important in Linux?

They allow users to edit files directly from the terminal, which is especially useful on remote Linux servers.

### Q3. What is Nano?

Nano is a simple, beginner-friendly terminal text editor.

### Q4. What is Vim?

Vim is a powerful terminal text editor based on Vi.

### Q5. What are the main Vim modes covered in this lecture?

- Command Mode
- Insert Mode
- Execution Mode
- Visual Mode

### Q6. How do you enter Insert Mode?

Use:

```text
i
```

### Q7. How do you return to Command Mode?

Press:

```text
Esc
```

### Q8. How do you save a Vim file?

```text
:w
```

### Q9. How do you save and exit?

```text
:wq
```

### Q10. How do you exit without saving?

```text
:q!
```

---

# 37. Quick Revision

## Nano

```bash
nano file.txt
```

Save/exit:

```text
Ctrl + X
Y
Enter
```

## Vim

Open:

```bash
vim file.txt
```

Modes:

```text
Command
Insert
Execution
Visual
```

Important:

```text
i       → Insert
Esc     → Command Mode
dd      → Delete line
yy      → Copy line
p       → Paste
u       → Undo
:w      → Save
:q      → Quit
:q!     → Quit without saving
:wq     → Save + Quit
```

---

# 38. Final Takeaway

The main idea of today's lecture is:

> **Linux administrators and DevOps engineers often work directly from the terminal, so knowing how to edit files from the terminal is an essential Linux skill.**

Start with:

```text
Nano → Easy editing
```

Then learn:

```text
Vim → Powerful, mode-based editing
```

The most important Vim concept for beginners is:

```text
Command Mode
     ↕
Insert Mode
     ↕
Esc
```

Do not try to memorize every Vim command in one day.

First become comfortable with:

```text
i
Esc
dd
yy
p
u
:w
:q
:q!
:wq
```

---

# 39. Homework

### Task 1 — Nano

Create:

```bash
nano student.txt
```

Write 5 lines about yourself and save the file.

### Task 2 — Vim

Create:

```bash
vim linux.txt
```

Practice:

```text
i
Esc
dd
yy
p
u
:wq
```

### Task 3 — Explain

Answer in your own words:

1. What is a text editor?
2. Why do we need terminal editors?
3. What is Nano?
4. What is Vim?
5. What is Insert Mode?
6. What is Command Mode?
7. What does `Esc` do?
8. Difference between `:q` and `:q!`.
9. Difference between `:w` and `:wq`.
10. Why is Vim useful on remote Linux servers?

---

# 40. Next Lecture

According to the Linux notes, the next chapter after Nano & Vi/Vim is:

## Linux Users & Groups

Topics include:

- Root user
- System users
- Local users
- `useradd`
- `adduser`
- `passwd`
- `userdel`
- `/etc/passwd`
- `/etc/shadow`
- Groups
- `groupadd`
- User and group management

