---
tags:
  - windows
  - powershell
  - CLI
---
# PowerShell Mode Attributes

When running `ls` or `Get-ChildItem` in Windows PowerShell, the **Mode** column contains 6 positions representing file attributes:

| Position | Letter | Attribute | Description |
| :---: | :---: | :--- | :--- |
| **1** | `d` | **Directory** | The item is a folder. |
| **2** | `a` | **Archive** | The item is modified and needs a backup. |
| **3** | `r` | **Read-only** | The item is locked from edits. |
| **4** | `h` | **Hidden** | The item is invisible by default. |
| **5** | `s` | **System** | The item is a critical system file. |
| **6** | `l` | **Link** | The item is a shortcut or symbolic link. |

> [!TIP] View Hidden Files
> To display hidden (`h`) or system (`s`) files in your terminal, use the force flag:
> `ls -Force`
