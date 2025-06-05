## 🖥️ **Linux CLI – Screen Utility**

### **What Is `screen`?**

- A **terminal multiplexer**: allows multiple terminal sessions to run **simultaneously** and **independently**.
    
- Sessions **persist** even if the connection is dropped or terminal is closed.
    
- Prevents loss of long-running processes due to disconnection or accidental `CTRL+C`.
    

---

## 🔧 **Basic Usage**

bash

CopyEdit

`screen`

- Starts a new `screen` session.
    
- Add `-h` to see help and available options.
    

bash

CopyEdit

`screen -h`

---

## 🔍 **Listing Active Screens**

bash

CopyEdit

`screen -ls`

- Lists all running screen sessions.
    

**Example Output:**

nginx

CopyEdit

`There are screens on: 	123.screen1	(Detached) 	456.screen2	(Detached) 2 Sockets in /run/screen/S-linux.`

---

## 🆕 **Creating a Named Screen**

bash

CopyEdit

`screen -S [screen-name]`

- Creates a new screen session with a custom name.
    
- Helps in identifying sessions easily.
    

---

## 🔄 **Reconnecting to a Screen**

bash

CopyEdit

`screen -r [screen-name]`

- Reconnect to a detached or existing screen session.
    
- Tip: Run `screen -ls` first to get the correct session name.
    

---

## 📤 **Detaching from a Screen**

- Use this key combination:
    

objectivec

CopyEdit

`CTRL + A, then CTRL + D`

- Detaches screen session without killing it.
    
- Returns you to your original terminal window.
    

---

## ❌ **Killing a Screen Session**

- Key combo:
    

css

CopyEdit

`CTRL + A, then K, then Y`

- `K`: initiates kill request
    
- `Y`: confirms the kill
    

This permanently ends the screen session.

---

## 📌 **Commands Overview**

|Command|Description|
|---|---|
|`screen -h`|View all screen options|
|`screen -ls`|List all currently active screen sessions|
|`screen -S [name]`|Start a new session with a custom name|
|`screen -r [name]`|Reconnect to an existing screen session|
|`CTRL + A, CTRL + D`|Detach from a screen session|
|`CTRL + A, K, Y`|Kill a screen session|

---

## ⚠️ **Important Notes**

- **Case-sensitive** options! Run `screen -h` to check correct usage.
    
- Sessions **continue running** in the background even when detached.
    
- Always **detach or kill** sessions properly to prevent orphan processes.
    

---

## 🧪 **In This Lab**

You’ll practice:

- Listing active screens
    
- Creating and naming screens
    
- Reconnecting to sessions
    
- Detaching and killing screen sessions
    

Complete tasks successfully to receive tokens for submission.