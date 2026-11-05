These notes are organized into concepts, commands, examples, and important corrections for easy revision.
1. AWS EC2 & SSH Connection
Generate SSH Key Pair
ssh-keygen -f daws-86s


This generates two files:
File	Purpose
daws-86s	Private key
daws-86s.pub	Public key

- Private key: Keep it secure. Never share it.
- Public key: Can be placed on the EC2 instance.
Connect to EC2 Instance
ssh -i <private-key> ec2-user@<IP>


Example:
ssh -i daws-86s ec2-user@13.234.100.10


EC2 Security Group Rules
Direction	Protocol	Port	Source/Destination
Inbound	SSH	22	0.0.0.0/0
Outbound	All traffic	All	0.0.0.0/0

Security Note: Allowing SSH from 0.0.0.0/0 means SSH is accessible from anywhere. For production, restrict SSH access to your own IP address.

2. Git Bash & Linux Directory Structure
Git Bash
- Git Bash provides a Linux-like command-line environment on Windows.
- It allows us to practice Linux commands on Windows.
- It supports many Unix/Linux commands.
Windows Path vs Git Bash Path
Windows:
C:\devops\daws-86s


Git Bash:
/c/devops/daws-86s


Absolute Path vs Relative Path
Absolute Path	Relative Path
Starts from root /	Starts from current directory
Complete path	Path relative to current location
Example: /c/devops/daws-86s	Example: daws-86s

cd /c/devops/daws-86s


This is an absolute path.
cd daws-86s


This is a relative path.

Important: A relative path works only when the specified directory exists relative to the current working directory.

3. Linux Command Structure
General command syntax:
command <inputs> <options>


Example:
ls -l


Component	Meaning
ls	Command
-l	Option

Inputs and options may be mandatory or optional depending on the command.

Example:
cp source destination


- source: Input file
- destination: Target location

4. Linux Users & Root User
User Types
Symbol	Meaning
$	Normal user prompt
#	Root user prompt

Important Linux Directories
Directory	Meaning
/	Root directory; starting point of filesystem
/root	Root user's home directory
/home/ec2-user	EC2 user home directory
/home/<username>	Normal user's home directory

Switching to Root User
sudo su -


- Switches to root user.
- Changes directory to root user's home directory.
- Loads root login environment.
sudo su


- Switches to root user.
- Normally remains in the current working directory.
- Does not perform a full login-shell initialization.
pwd – Print Working Directory
pwd


Displays the current directory.

Example:
pwd


Output:
/home/ec2-user



5. Linux File & Directory Operations (CRUD)
CRUD stands for:
Operation	Meaning
C	Create
R	Read
U	Update
D	Delete

Create a File
touch file.txt


Creates an empty file if it does not exist.

If the file already exists, updates its timestamp.
Read a File
cat file.txt


Displays the file content.
Create a File and Write Content
cat > file.txt


Type the content:
This is Linux class
Learning DevOps


Press:
Enter
Ctrl + D


This saves the content.
Redirection Operators
Operator	Meaning
>	Overwrite file
>>	Append content

Example:
echo "Hello Linux" > file.txt


Overwrites the file content.
echo "Hello DevOps" >> file.txt


Appends content without removing existing content.

Important: > and >> are shell redirection operators.

6. Linux Directory Commands
mkdir – Create Directory
mkdir devops


Creates a directory named devops.
rmdir – Delete Empty Directory
rmdir devops


Deletes an empty directory.
rm – Remove Files or Directories
rm file.txt


Deletes a file.
rm -r devops


Deletes a directory recursively, including its contents.
Warning
rm -rf directory


This forcefully deletes a directory and its contents without prompting. Use carefully.
cd – Change Directory
cd /home/ec2-user


Moves into the specified directory.
cd ..


Moves one level up.
cd .


Refers to the current directory.

Linux is case-sensitive:
cd


is different from:
Cd



7. ls – List Files & Directories
Basic Command
ls


Lists files and directories.
Important ls Options
Command	Meaning
ls -l	Long listing format
ls -r	Reverse order
ls -t	Sort by modification time
ls -lr	Long listing, reverse sorting
ls -lt	Long listing, latest modified first
ls -ltr	Long listing, oldest modified first

Examples
ls -l


Displays permissions, ownership, size, date and filename.
ls -lt


Displays latest modified files first.
ls -ltr


Displays oldest modified files first.

Note: ls -l does not necessarily mean alphabetical sorting. Alphabetical sorting is the default behavior of ls.

8. cp & mv Commands
cp – Copy Files
Syntax:
cp <source> <destination>


Example:
cp file.txt backup.txt


Copies file.txt into backup.txt.

Copy a file into a directory:
cp file.txt /tmp/


mv – Move or Rename Files
Syntax:
mv <source> <destination>


Example:
mv file.txt /tmp/


Moves the file into /tmp.

Rename a file:
mv old.txt new.txt


Changes the filename from old.txt to new.txt.

9. grep – Search Text Inside Files
Basic Syntax
grep <word-to-search> <file-name>


Example:
grep Linux file.txt


Searches for lines containing Linux.
Important grep Options
Option	Meaning
-n	Display line numbers
-c	Count matching lines
-i	Ignore case
-v	Display non-matching lines

Examples
grep -n Linux file.txt


Displays matching lines with line numbers.
grep -c Linux file.txt


Counts the number of matching lines.
grep -i linux file.txt


Searches case-insensitively.
grep -v Linux file.txt


Displays lines that do not contain Linux.

Important: grep -c counts matching lines, not total occurrences of the word.

10. wget & curl Commands
wget – Download Files
wget <URL>


Example:
wget https://example.com/file.txt


Downloads the file to the current directory.
curl – Transfer Data
curl <URL>


Example:
curl https://example.com


Displays the response content on the terminal.

Download a file using curl:
curl -O https://example.com/file.txt


-O saves the file using the remote filename.

11. cut Command – Extract Fields
The cut command extracts specific fields or characters from text.
Syntax
cut -d "<delimiter>" -f <field-number>


Option	Meaning
-d	Specify delimiter
-f	Specify field number

Example URL:
https://raw.githubusercontent.com/daws-86s/notes/refs/heads/main/session-02.txt


Command:
echo "https://raw.githubusercontent.com/daws-86s/notes/refs/heads/main/session-02.txt" | cut -d "/" -f1


Output:
https:


Extract Different Fields
Field	Output
-f1	https:
-f2	Empty field
-f3	raw.githubusercontent.com
-f4	daws-86s
-f5	notes
-f6	refs
-f7	heads
-f8	main
-f9	session-02.txt

Concept: The cut command separates text using a delimiter and extracts the required field.

12. awk Command – Extract Fields
awk is a powerful text-processing command used to extract and manipulate data.
Syntax
awk -F "<delimiter>" '{print $N}' file


Component	Meaning
-F	Field separator
$1	First field
$2	Second field
$3	Third field
$NF	Last field

Example:
echo "https://raw.githubusercontent.com/daws-86s/notes/refs/heads/main/session-02.txt" | awk -F "/" '{print $3}'


Output:
raw.githubusercontent.com


Extract the first field:
echo "https://raw.githubusercontent.com/daws-86s/notes/refs/heads/main/session-02.txt" | awk -F "/" '{print $1}'


Output:
https:


Extract the last field:
echo "https://raw.githubusercontent.com/daws-86s/notes/refs/heads/main/session-02.txt" | awk -F "/" '{print $NF}'


Output:
session-02.txt


Difference Between cut and awk
cut	awk
Simple field extraction	Advanced text processing
Easy to use	More powerful
Mainly extracts columns/characters	Extracts, filters and processes fields

Correction to your class example:
awk -F "/" '{print $1F}'


This is not the correct syntax for extracting field 1.

Correct:
awk -F "/" '{print $1}'


For field 3:
awk -F "/" '{print $3}'



13. Linux Important Symbols
Symbol	Meaning
/	Root directory
.	Current directory
..	Parent directory
~	Current user's home directory
$	Normal user prompt
#	Root user prompt
>	Overwrite redirection
>>	Append redirection
\|	Pipe operator

Pipe Operator (|)
A pipe passes the output of one command as input to another command.

Example:
cat file.txt | grep Linux


Flow:
cat file.txt → grep Linux → Matching lines


Another example:
echo "Hello Linux" | cut -d " " -f1


Output:
Hello



14. Quick Revision – Linux Commands Cheat Sheet
Command	Purpose
pwd	Print working directory
ls	List files
ls -l	Long listing
cd	Change directory
cd ..	Move one level up
mkdir	Create directory
rmdir	Remove empty directory
touch	Create file
cat	Read file
cat > file	Create/write file
cp	Copy
mv	Move/rename
rm	Delete
rm -r	Delete recursively
grep	Search text
cut	Extract fields
awk	Process text
wget	Download file
curl	Transfer/display data
uname -a	Display system information
sudo su -	Switch to root login shell
