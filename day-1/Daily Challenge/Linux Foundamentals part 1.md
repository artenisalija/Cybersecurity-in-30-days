# Linux Fundamentals Part 1 - [Linux Fundamentals](https://tryhackme.com/room/linuxfundamentals)

## Interacting with the File System

| Command | Full Name |
|---------|-----------|
| ls | listing |
| cd | change directory |
| cat | concatenate |
| pwd | print working directory |

## Operators

| Symbol / Operator | Description |
|-------------------|-------------|
| & | This operator allows you to run commands in the background of your terminal. |
| && | This operator allows you to combine multiple commands together in one line of your terminal. |
| > | This operator is a redirector - meaning that we can take the output from a command (such as using cat to output a file) and direct it elsewhere. |
| >> | This operator does the same function of the > operator but appends the output rather than replacing (meaning nothing is overwritten). |

## Quiz

**1. Research: What year was the first release of a Linux operating system?**
```
1991
```

**2. If we wanted to output the text "TryHackMe", what would our command be?**
```
echo TryHackMe
```

**3. What is the username of who you're logged in as on your deployed Linux machine?**
```
tryhackme
```

**4. On the Linux machine that you deploy, how many folders are there?**
```
4
```

**5. Which directory contains a file?**
```
folder4
```

**6. What is the contents of this file?**
```
Hello World!
```

**7. Use the cd command to navigate to this file and find out the new current working directory. What is the path?**
```
/home/tryhackme/folder4
```

**8. Use grep on "access.log" to find the flag that has a prefix of "THM". What is the flag?**  
*Note: The "access.log" file is located in the "/home/tryhackme/" directory*
```
THM{ACCESS}
```

**9. If we wanted to run a command in the background, what operator would we want to use?**
```
&
```

**10. If I wanted to replace the contents of a file named "passwords" with the word "password123", what would my command be?**
```
echo password123 > passwords
```

**11. Now if I wanted to add "tryhackme" to this file named "passwords" but also keep "passwords123", what would my command be?**
```
echo tryhackme >> passwords
```