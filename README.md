# Use Linux Commands to Manage File Permissions

## Step 1 & Step 2: Project Description and Scenario
[cite_start]As a security professional working with a research team, my task is to examine existing permissions on the organization's file system to ensure users are authorized with appropriate access[cite: 59]. [cite_start]This helps keep the system secure and aligns with the principle of least privilege[cite: 61]. [cite_start]If permissions do not match the organization's security baseline, they must be modified to authorize appropriate users and remove unauthorized access[cite: 60].

* [cite_start]**Project Name:** Use Linux commands to manage file permissions [cite: 57]
* **Environment:** Linux Bash Shell
* [cite_start]**Tools Used:** `ls -la` (File auditing), `chmod` (Access control modification) [cite: 66, 94]

---

## Step 3: Check File and Directory Details
* [cite_start]**The Problem:** The active permissions set for files and subdirectories in the `projects` directory need to be audited to identify unauthorized permissions or access anomalies[cite: 63].
* [cite_start]**Solution Method:** I used the `ls` command with the `-la` option to display a detailed long listing of the directory contents, including hidden files (files starting with a period `.`)[cite: 66].

```bash
researcher2@5d738f0f927b:~/projects$ ls -la
```

#### 📸 Output Verification Screenshot:
![Initial Directory Audit](images/01_directory_audit.png)

* [cite_start]**Final Solution:** The command output revealed the following 10-character permission strings for the files and subdirectory[cite: 67, 68]:

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
* [cite_start]**The Problem:** The 10-character metadata string needs to be analyzed to understand the precise access rights granted to different user classifications[cite: 70].  
* [cite_start]**Solution Method:** I deconstructed the permission string by breaking down what each character position represents[cite: 71]:  
  * [cite_start]**1st character:** Indicates the file type (`d` for directory, `-` for a regular file)[cite: 72, 73].  
  * [cite_start]**2nd-4th characters:** Indicate the read (`r`), write (`w`), and execute (`x`) permissions for the **User (owner)**[cite: 76].  
  * [cite_start]**5th-7th characters:** Indicate the read (`r`), write (`w`), and execute (`x`) permissions for the **Group**[cite: 78].
  * [cite_start]**8th-10th characters:** Indicate the read (`r`), write (`w`), and execute (`x`) permissions for **Other** (anyone else on the system)[cite: 80, 81].
* [cite_start]**Final Solution (Example Analysis):** For example, the permissions for `project_t.txt` are `-rw-rw-r--`[cite: 83]. [cite_start]The first character (`-`) indicates it is a regular file[cite: 83]. [cite_start]The user, group, and other all have read permissions (`r`)[cite: 84]. [cite_start]Only the user and group have write permissions (`w`)[cite: 85]. [cite_start]No one has execute permissions (`-`)[cite: 86].

---

## Step 5: Change File Permissions
* [cite_start]**The Problem:** The organization's policy strictly prohibits "other" from having write access to any files[cite: 88]. [cite_start]The file `project_k.txt` is misconfigured with public write access (`-rw-rw-rw-`)[cite: 90].
* [cite_start]**Solution Method:** I used the `chmod` command to modify the permissions[cite: 94]. [cite_start]The argument `o-w` explicitly removes (`-`) the write permission (`w`) from other (`o`) for `project_k.txt`[cite: 96, 97].

```bash
researcher2@5d738f0f927b:~/projects$ chmod o-w project_k.txt
```

#### 📸 Output Verification Screenshot:
![Remediating Regular File](images/02_change_file_permissions.png)

* [cite_start]**Final Solution:** Verifying the update with `ls -la project_k.txt` confirms that the write access for other has been successfully removed[cite: 97]:

```text
-rw-rw-r-- 1 researcher2 research_team   46 Dec  2 15:27 project_k.txt
```

---

## Step 6: Change File Permissions on a Hidden File
* [cite_start]**The Problem:** The research team archived `.project_x.txt` (a hidden file)[cite: 99, 110]. [cite_start]This file should not have write permissions for anyone, but the user and group must be able to read it[cite: 100]. [cite_start]Its initial permissions were incorrectly set to `-rw--w----`[cite: 89, 104].
* [cite_start]**Solution Method:** I executed the `chmod` command with a compound argument: `u-w` to remove write from user, `g-w` to remove write from group, and `g+r` to add read to the group[cite: 112].

```bash
researcher2@3213bbc1d047:~/projects$ chmod u-w,g-w,g+r .project_x.txt
```

#### 📸 Output Verification Screenshot:
![Remediating Hidden File](images/03_change_hidden_file.png)

* [cite_start]**Final Solution:** The updated permission string is now compliant with the archiving restrictions, removing all write privileges[cite: 111]:

```text
-r--r----  1 researcher2 research_team   46 Dec 20 15:36 .project_x.txt
```

---

## Step 7: Change Directory Permissions
* [cite_start]**The Problem:** The organization requires that only the `researcher2` user can access the `drafts` directory and its contents[cite: 114]. [cite_start]The group currently holds unauthorized execute permissions (`drwx--x--`)[cite: 132].
* [cite_start]**Solution Method:** I used the `chmod` command with the `g-x` argument to remove (`-`) execute permissions (`x`) from the group (`g`)[cite: 132].

```bash
researcher2@5d738f0f927b:~/projects$ chmod g-x drafts
```

#### 📸 Output Verification Screenshot:
![Remediating Directory Access](images/04_change_directory_permissions.png)

* [cite_start]**Final Solution:** A final file system check shows that the directory is fully isolated to the owner[cite: 133]:

```text
drwx------ 2 researcher2 research_team 4096 Dec  2 15:27 drafts
```

---

## Step 8: Summary
[cite_start]I changed multiple permissions to match the level of authorization my organization wanted for files and directories in the `projects` directory[cite: 135]. [cite_start]The first step in this was using `ls -la` to check the permissions for the directory[cite: 136]. [cite_start]This informed my decisions in the following steps[cite: 137]. [cite_start]I then used the `chmod` command multiple times to change the permissions on files and directories to properly secure the system assets[cite: 137].
