Linux Commands
=========================
pwd - displays present working directory
clear - clears the screen
date - displays current date
history - displays history of commands being used
cal - displays calendar
whoami - displays current user name
touch - to create a new file
vi --> :wq , :q! - create, read, write file
cat - create , read , write file 
mkdir - creates a directorty
rmdir - deletes a directory
rm -f - removes files
rm - remove
mv - rename
cp - copy
cd - change directory
cat file - displays file contents from top to bottom
tac file - displays file contents from bottom to top 
head file => By default shows first 10 lines of the file
tail file => By default shows last 10 lines of the file
tail -f file --> displays last 10 lines, waits for next lines to be added
head -n 50 file => Shows first 50 lines of the file
tail -n 50 file => Shows last 50 lines of the file
wc file --> line, words, chars ==> Word Count

grep 'test' file - search text from a file - case sensitive (Global Regular Expression Print)
grep -i 'test' file - not case sensitive
grep -i 'test' * - searches in entire directory

echo test > file
echo test >> file
