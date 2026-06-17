# Use Linux Commands to Manage File Permissions

## Step 1 & Step 2: Project Description and Scenario
As a security professional working with a research team, my task is to examine existing permissions on the organization's file system to ensure users are authorized with appropriate access.This helps keep the system secure and aligns with the principle of least privilege. If permissions do not match the organization's security baseline, they must be modified to authorize appropriate users and remove unauthorized access.

* **Project Name:** Use Linux commands to manage file permissions
* **Environment:** Linux Bash Shell
* **Tools Used:** `ls -la` (File auditing), `chmod` (Access control modification)

---

## Step 3: Check File and Directory Details
* **The Problem:** The active permissions set for files and subdirectories in the `projects` directory need to be audited to identify unauthorized permissions or access anomalies.
* **Solution Method:** I used the `ls` command with the `-la` option to display a detailed long listing of the directory contents, including hidden files (files starting with a period `.`).

```bash
researcher2@5d738f0f927b:~/projects$ ls -la
```

#### 📸 Output Verification Screenshot:
![Initial Directory Audit](images/01_directory_audit.png)

* **Final Solution:** The command output revealed the following 10-character permission strings for the files and subdirectory:

```text
total 32
drwxr-xr-x 3 researcher2 research_team 4096 Dec  2 15:27 .
drwxr-xr-x 3 researcher2 research_team 4096 Dec  2 15:27 ..
-rw--w---- 1 researcher2 research_team   46 Dec  2 15:27 .project_x.txt
drwx--x--  2 researcher2 research_team 4096 Dec  2 15:27 drafts
-rw-rw-rw- 1 researcher2 research_team   46 Dec  2 15:27 project_k.txt
-rw-r----  1 researcher2 research_team   46 Dec  2 15:27 project_m.txt
-rw-rw-r-- 1 researcher2 research_team   46 Dec  2 15:27 project_r.txt
-rw-rw-r-- 1 researcher2 research_team   46 Dec  2 15:27 project_t.txt
```

---

## Step 4: Describe the Permissions String
* **The Problem:** The 10-character metadata string needs to be analyzed to understand the precise access rights granted to different user classifications.  
* **Solution Method:** I deconstructed the permission string by breaking down what each character position represents:  
  * **1st character:** Indicates the file type (`d` for directory, `-` for a regular file)[cite: 72, 73].  
  * **2nd-4th characters:** Indicate the read (`r`), write (`w`), and execute (`x`) permissions for the **User (owner)**.  
  * **5th-7th characters:** Indicate the read (`r`), write (`w`), and execute (`x`) permissions for the **Group**.
  * **8th-10th characters:** Indicate the read (`r`), write (`w`), and execute (`x`) permissions for **Other** (anyone else on the system).
* **Final Solution (Example Analysis):** For example, the permissions for `project_t.txt` are `-rw-rw-r--`. The first character (`-`) indicates it is a regular file.The user, group, and other all have read permissions (`r`).Only the user and group have write permissions (`w`).No one has execute permissions (`-`).

---

## Step 5: Change File Permissions
* **The Problem:** The organization's policy strictly prohibits "other" from having write access to any files.The file `project_k.txt` is misconfigured with public write access (`-rw-rw-rw-`).
* **Solution Method:** I used the `chmod` command to modify the permissions.The argument `o-w` explicitly removes (`-`) the write permission (`w`) from other (`o`) for `project_k.txt`.

```bash
researcher2@5d738f0f927b:~/projects$ chmod o-w project_k.txt
```

#### 📸 Output Verification Screenshot:
![Remediating Regular File](images/02_change_file_permissions.png)

* **Final Solution:** Verifying the update with `ls -la project_k.txt` confirms that the write access for other has been successfully removed:

```text
-rw-rw-r-- 1 researcher2 research_team   46 Dec  2 15:27 project_k.txt
```

---

## Step 6: Change File Permissions on a Hidden File
* **The Problem:** The research team archived `.project_x.txt` (a hidden file).This file should not have write permissions for anyone, but the user and group must be able to read it. Its initial permissions were incorrectly set to `-rw--w----`.
* **Solution Method:** I executed the `chmod` command with a compound argument: `u-w` to remove write from user, `g-w` to remove write from group, and `g+r` to add read to the group.

```bash
researcher2@3213bbc1d047:~/projects$ chmod u-w,g-w,g+r .project_x.txt
```

#### 📸 Output Verification Screenshot:
![Remediating Hidden File](images/03_change_hidden_file.png)

* **Final Solution:** The updated permission string is now compliant with the archiving restrictions, removing all write privileges:

```text
-r--r----  1 researcher2 research_team   46 Dec 20 15:36 .project_x.txt
```

---

## Step 7: Change Directory Permissions
* **The Problem:** The organization requires that only the `researcher2` user can access the `drafts` directory and its contents.The group currently holds unauthorized execute permissions (`drwx--x--`).
* **Solution Method:** I used the `chmod` command with the `g-x` argument to remove (`-`) execute permissions (`x`) from the group (`g`).

```bash
researcher2@5d738f0f927b:~/projects$ chmod g-x drafts
```

#### 📸 Output Verification Screenshot:
![Remediating Directory Access](images/04_change_directory_permissions.png)

* **Final Solution:** A final file system check shows that the directory is fully isolated to the owner:

```text
drwx------ 2 researcher2 research_team 4096 Dec  2 15:27 drafts
```

---

## Step 8: Summary
I changed multiple permissions to match the level of authorization my organization wanted for files and directories in the `projects` directory.The first step in this was using `ls -la` to check the permissions for the directory.This informed my decisions in the following steps.I then used the `chmod` command multiple times to change the permissions on files and directories to properly secure the system assets.
