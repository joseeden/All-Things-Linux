
# Lab xx: Write Simple Bash Scripts 

> *This lab is based on [O'Reilly Interactive Learning Challenge: Writing Simple Bash Scripts.](https://www.oreilly.com/online-learning/intro-interactive-learning.html)*

We'll be creating simple bash scripts that redirect standard output and standard error.

## Bash Fundamentals Challenges
This challenge is part of an interactive set:

<details><summary> 1. Write Simple Bash Scripts That Output Text </summary>

### 1. Write Simple Bash Scripts That Output Text

- Write a script called name.sh that outputs your user ID to the terminal. The file should be placed in the /root folder, and should be executable.

- Write a script called name2.sh in the /root folder that:
    * Creates a folder called name in the /root/ folder
    * Creates an executable script called name.sh in the folder /root/name that outputs your name to the terminal
    * Runs the /root/name/name.sh script
    * Returns to the original folder, and deletes the name folder and all its contents

</details>

2. Bash Loops and Arguments
3. Bash Tests
4. Create a File-Reporting Application
5. Write Simple Bash Scripts That Redirect Standard Output and Standard Error ← You are here
6. Bash Subshell
7. Bash Here-Docs
8. Bash Globs and Redirection
9. Bash Debugging
10. Waiting for Tasks to Complete
11. Trapping Signals in Shell Scripts

## Solutions

<details><summary> 1. Write Simple Bash Scripts That Output Text </summary>

### 1. Write Simple Bash Scripts That Output Text

- Write a script called name.sh that outputs your user ID to the terminal. The file should be placed in the /root folder, and should be executable.

    ```bash
    $ touch ~/name.sh
    $ chmod 700 ~/name.sh 
    ```
    ```bash
    $  vim ~/name.sh

    #!/bin/bash
    echo $USER
    ```
    ```bash 
    ./name.sh
    ```

- Write a script called name2.sh in the /root folder that:
    * Creates a folder called name in the /root/ folder
    * Creates an executable script called name.sh in the folder /root/name that outputs your name to the terminal
    * Runs the /root/name/name.sh script
    * Returns to the original folder, and deletes the name folder and all its contents

    ```bash
    $ touch ~/name2.sh
    $ chmod 700 ~/name2.sh 
    ```
    ```bash
    $  vim ~/name.sh

        #!/bin/bash

        mkdir /root/name
        touch /root/name/name.sh
        chmod 700 /root/name/name.sh

        echo '
        #!/bin/bash 
        echo $USER' > /root/name/name.sh

        /bin/bash /root/name/name.sh
        rm -rf name
    ```
    ```bash 
    ./name2.sh
    ```

</details>
