slip 2
Q1. Write a C program to find file properties such as inode number, number of hard link, File 
permissions, File size, File access and modification time and so on of a given file using stat() 
system call.
#include <stdio.h>

#include <stdlib.h>

#include <sys/stat.h>

#include <unistd.h>

#include <errno.h>

#include <time.h>

int main(int argc, char *argv[]) {

 if (argc != 2) {
  printf("Usage: %s <filename>\n", argv[0]);

 return 1;
 
 }
 
 struct stat fileStat;
 

 if (stat(argv[1], &fileStat) < 0) {

 fprintf(stderr, "Error accessing file %s: %s\n", argv[1], strerror(errno));

 return 1;

 }

 printf("File: %s\n", argv[1]);

 printf("Inode Number: %ld\n", (long)fileStat.st_ino);

 printf("Number of Hard Links: %ld\n", (long)fileStat.st_nlink);
 
 printf("File Permissions: ");
 
 printf((S_ISDIR(fileStat.st_mode)) ? "d" : "-");

 printf((fileStat.st_mode & S_IRUSR) ? "r" : "-");

 printf((fileStat.st_mode & S_IWUSR) ? "w" : "-");


 printf((fileStat.st_mode & S_IXUSR) ? "x" : "-");

 printf((fileStat.st_mode & S_IRGRP) ? "r" : "-");


 printf((fileStat.st_mode & S_IWGRP) ? "w" : "-");

 printf((fileStat.st_mode & S_IXGRP) ? "x" : "-");

 printf((fileStat.st_mode & S_IROTH) ? "r" : "-");

 printf((fileStat.st_mode & S_IWOTH) ? "w" : "-");

 printf((fileStat.st_mode & S_IXOTH) ? "x\n" : "-\n");

 printf("File Size: %ld bytes\n", (long)fileStat.st_size);

 printf("Last Access Time: %s", ctime(&fileStat.st_atime));

 printf("Last Modification Time: %s", ctime(&fileStat.st_mtime));

 return 0;
 }

Output:

File: example.txt

Inode Number: 123456

Number of Hard Links: 1

File Permissions: -rw-r--r--

File Size: 1024 bytes


Last Access Time: Mon Jan 10 15:30:00 2023

Last Modification Time: Fri Dec 15 09:45:00 2023

Q2. Write a C program that catches the ctrl-c (SIGINT) signal for the first time and display 
the appropriate message and exits on pressing ctrl-c again.

#include <stdio.h>

#include <signal.h>
#include <stdlib.h>

int count = 0;

void handle_sigint(int signum) {

 if (signum == SIGINT) {

 if (count == 0) {

 printf("\nCaught SIGINT signal. Press Ctrl+C again to exit.\n");

count++;

 } else {


 printf("\nReceived second SIGINT. Exiting...\n");


 int main() {

 signal(SIGINT, handle_sigint);

 printf("Press Ctrl+C to trigger SIGINT signal.\n");
 while(1) {

 // Program continues running and waiting for SIGINT
 }

 return 0;
}exit(EXIT_SUCCESS);
 }
 }
}

slip 3 
Q1. Print the type of file and inode number where file name accepted through Command
Line.


#include <stdio.h>

 #include <stdlib.h>

 #include <sys/types.h>

 #include <sys/stat.h>

 #include <unistd.h>

  #include <errno.h>
 int main(int argc, char *argv[]) {

   if (argc != 2) {

  printf("Usage: %s <filename>\n", argv[0]);

  return 1;

  }

  struct stat fileStat;

 if (stat(argv[1], &fileStat) < 0) {

 fprintf(stderr, "Error accessing file %s: 
 
 %s\n", argv[1], strerror(errno));

 return 1;

 }
 
 printf("File: %s\n", argv[1]);


 printf("Inode Number: %ld\n", (long)fileStat.st_ino);

 if (S_ISREG(fileStat.st_mode)) {

 printf("File Type: Regular file\n");
 } 

 else if (S_ISDIR(fileStat.st_mode)) {

 printf("File Type: Directory\n");

 } else if (S_ISLNK(fileStat.st_mode)) {

 printf("File Type: Symbolic Link\n");

 } else if (S_ISCHR(fileStat.st_mode)) {

 printf("File Type: Character Device\n");

 } else if (S_ISBLK(fileStat.st_mode)) {

 printf("File Type: Block Device\n");

 } else if (S_ISFIFO(fileStat.st_mode)) {


 printf("File Type: FIFO/Named Pipe\n");

 } else if (S_ISSOCK(fileStat.st_mode)) {

 printf("File Type: Socket\n");

 } else {

 printf("File Type: Unknown\n");

 }

 return 0;
 }

Output :

example.txt is a regular file:


File: example.txt

Inode Number: 123456

File Type: Regular file

example.txt is a directory file:

File: example.txt

Inode Number: 123456

File Type: Directory file

Q2. Write a C program which creates a child process to run linux/ unix command or any user 
defined program. The parent process set the signal handler for death of child signal and 
Alarm signal. If a child process does not complete its execution in 5 second then parent 
process kills child process.

#include <stdio.h>

#include <stdlib.h>

#include <unistd.h>

#include <signal.h>

#include <sys/types.h>

#include <sys/wait.h>

void child_handler(int signum) {

 if (signum == SIGCHLD) {

 printf("Child process has terminated.\n");
 }

}

void alarm_handler(int signum) {


 if (signum == SIGALRM) {

 printf("Child process exceeded time limit. 
 
 Killing...\n");

 kill(getpid(), SIGKILL); // Kill the current 
 
 process
 }

}

int main(int argc, char *argv[]) {

 if (argc < 2) {

 printf("Usage: %s <command>\n", argv[0]);

 return 1;

 }

 signal(SIGCHLD, child_handler);

 signal(SIGALRM, alarm_handler);

 pid_t pid = fork();

 if (pid < 0) {

 perror("Fork failed");

 exit(EXIT_FAILURE);

 } else if (pid == 0) { // Child process

 execvp(argv[1], &argv[1]); // Execute the 
 
 command
 perror("Execution failed");


 exit(EXIT_FAILURE);

 } else { // Parent process

 alarm(5); // Set alarm for 5 seconds

 // Wait for the child process to terminate


 wait(NULL);

 alarm(0); // Cancel the alarm

 }

 return 0;

}

Output :

Child process has terminated.

slip4

Q.1) Write a C program to find whether a given files passed through command line 
arguments are present in current directory or not.

#include <stdio.h>

#include <stdlib.h>

#include <unistd.h>
int main(int argc, char *argv[]) {


 if (argc < 2) {

 printf("Usage: %s <file1> <file2> ... 
 
 <fileN>\n", argv[0]);

 return 1;

 }

 for (int i = 1; i < argc; i++) {

 if (access(argv[i], F_OK) != -1) {

 printf("%s exists in the current 
 
 directory.\n", argv[i]);

 } else {

 printf("%s does not exist in the current 

 directory.\n", argv[i]);

 }

 }

 pid_t pid = fork();

 if (pid < 0) {



 perror("Fork failed");

 exit(EXIT_FAILURE);


 } else if (pid == 0) { // Child process


 signal(SIGHUP, child_handler);

 signal(SIGINT, child_handler);

 signal(SIGQUIT, child_handler);

 while(1) {

 // Child process continues running and handling signals

 
 }

 } else { // Parent process


 int counter = 0;

 while (counter < 5) { // Sending signals for 
 15 seconds (5 * 3 seconds)

 sleep(3);
 if (!kill_child) {

 kill(pid, SIGHUP); // Send SIGHUP signal

 } else {

 kill(pid, SIGINT); // Send SIGINT signal

 
 }

 counter++;

 if (counter == 5) {

 kill(pid, SIGQUIT); // Send SIGQUIT signal 
  after 15 seconds

 }

 kill_child = !kill_child;

 }
 wait(NULL); // Wait for child to terminate
 }
 return 0;
}

Output :

Child received SIGHUP from parent.

Child received SIGINT from parent.

Child received SIGHUP from parent.

Child received SIGINT from parent.

Child received SIGHUP from parent.

Child received SIGINT from parent.

Child received SIGHUP from parent.
Child received SIGINT from parent.

Child received SIGHUP from parent.

My Papa has Killed me!!!





slip5 

Q.1) Read the current directory and display the name of the files, no of files in current 
directory.

#include <stdio.h>

#include <stdlib.h>

#include <dirent.h>

int main() {

 DIR *directory;

 struct dirent *entry;

 int file_count = 0;


 // Open the current directory

 directory = opendir(".");
 if (directory == NULL) {

 perror("Unable to open directory");

 return EXIT_FAILURE;
 }

 // Read directory entries and display file names

 while ((entry = readdir(directory)) != NULL) {

 printf("%s\n", entry->d_name);

 file_count++;

 }

 closedir(directory);

 printf("\nTotal files in current directory: 

 %d\n", file_count);


 return EXIT_SUCCESS;

}
Output :

file1.txt

file2.jpg

folder1

folder2

program.c
