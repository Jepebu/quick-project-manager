# Quick Project Manager (qpm)
### Manage locally stored projects in a repeatable and scalable fashion.

<b>qpm</b> is designed mostly for my own convenience. It functions as a sort of mini version control interface that handles basic functions such as:
- Creating temporary edited version of a project before submitting them to the source.
- Having multiple independently edited copies of the source.
- Maintaining locks for different edits and versions of a project.
- Creating new versions of projects which adhere to sem-ver.
- Zipping previous versions of a project to save space.
- Relatively strict error handling and over-write checks.

## Examples:
### Creating a project, then making a new version.
``` bash
jepebu@Ubuntu-Server:~$ qpm create test-project
[*] Project 'test-project' created!
jepebu@Ubuntu-Server:~$ ls -la ~/qpm/projects/test-project
test-project-0.0.0
jepebu@Ubuntu-Server:~$ qpm new test-project
[*] Version 0.0.1 created.
jepebu@Ubuntu-Server:~$ ls ~/qpm/projects/test-project
test-project-0.0.0.zip  test-project-0.0.1
```

### Editing a temporary version of a project, submitting the changes, then applying them to the origin.
``` bash
jepebu@Ubuntu-Server:~$ qpm edit test-project
[+] Editing copy of '/home/jepebu/qpm/projects/test-project/test-project-0.0.1/'. Use 'exit' to return to your original shell.
[ jepebu qpm test-project-0.0.1-main ]$ echo "Hello World!" >> my_file
[ jepebu qpm test-project-0.0.1-main ]$ exit
exit
[*] Done editing 'test-project-main'. Use 'qpm submit test-project --name main' to submit your changes back to the origin.
jepebu@Ubuntu-Server:~$ qpm submit test-project
[*] Edits submitted!
jepebu@Ubuntu-Server:~$ qpm apply test-project
sending incremental file list
./
my_file

sent 158 bytes  received 38 bytes  392.00 bytes/sec
total size is 13  speedup is 0.07
[*] Done applying updates!
```

### Making a new independent edit version
``` bash
jepebu@Ubuntu-Server:~$ qpm edit test-project --name new-edit-1
[+] Editing copy of '/home/jepebu/qpm/projects/test-project/test-project-0.0.1/'. Use 'exit' to return to your original shell.
[ jepebu qpm test-project-0.0.1-new-edit-1 ]$ echo "Hello from new-edit-1!" >> my_file
[ jepebu qpm test-project-0.0.1-new-edit-1 ]$ exit
exit
[*] Done editing 'test-project-new-edit-1'. Use 'qpm submit test-project --name new-edit-1' to submit your changes back to the origin.
jepebu@Ubuntu-Server:~$ qpm submit test-project --name new-edit-1
[*] Edits submitted!
```

### Opening an already existing edit to make further changes
``` bash
jepebu@Ubuntu-Server:~$ qpm edit test-project --name new-edit-1
[+] Editing copy of '/home/jepebu/qpm/.runtime/edits/test-project/test-project-0.0.1-new-edit-1/'. Use 'exit' to return to your original shell.
[ jepebu qpm test-project-0.0.1-new-edit-1 ]$ ls 
my_file
```

### Creating a version with a different sem-ver increment
``` bash
jepebu@Ubuntu-Server:~/qpm/projects/test-project$ qpm new test-project --version major
[*] Version 1.0.1 created.
jepebu@Ubuntu-Server:~/qpm/projects/test-project$ qpm new test-project --version minor
[*] Version 1.1.1 created.
jepebu@Ubuntu-Server:~/qpm/projects/test-project$ qpm new test-project --version patch
[*] Version 1.1.2 created.
```

### Trying to open an edit which is already opened elsewhere
```bash
jepebu@Ubuntu-Server:~$ Terminal 1 - qpm edit test-project
[+] Editing copy of '/home/jepebu/qpm/projects/test-project/test-project-1.1.2/'. Use 'exit' to return to your original shell.
[ jepebu qpm test-project-1.1.2-main ]$ 

jepebu@Ubuntu-Server:~$ Terminal 2 - qpm edit test-project
[!] Requested edit 'main' is currently locked.
jepebu@Ubuntu-Server:~$ Terminal 2 - 
```

#### There are many more niche examples for many more niche scenarios, but I think this gets the point across.

