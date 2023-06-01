
# Lab 4: Using Essential Tools

> *This lab is based on [Red Hat Certified System Administrator (RHCSA), 3/e by Sander van Vugt](https://www.oreilly.com/library/view/red-hat-certified/9780135656495/)*

For the this lab and the succeeding labs, we'll be using our RHEL 8 launched on Amazon EC2 instances. Feel free to do this lab in VirtualBox as well.

## Tasks

1. Locate the **man** page that shows how to set the password.
2. Search the **man** page on how create a user with name **ted**.
3. Set the password for user **ted** to "password".
4. Display **ted**'s group and then delete user **ted**.
5. Create user **robin** with a primary group of "IT" and the secondary group of "Devops".
6. Lock robin's account and then unlock it.
4. Make /etc your current working directory.
5. Use globbing to show all files in /etc that have a number in their name.
6. Still in /etc, display the contents page by page. Then switch to your home directory.
7. Create a file named **users** that contains the name of all four teenage mutant ninja turtles on separate lines
8. Display the version of your RHEL.


## Solutions

<details><summary> Task 1: Solution </summary>

#### Task 1: Locate the man page that shows how to set the password

Use *man -k <keyword>* to search the entire manual for the term "password".

<details><summary>man -k password </summary>

```bash
$ man -k password

chage (1)            - change user password expiry information
chgpasswd (8)        - update group passwords in batch mode
chpasswd (8)         - update passwords in batch mode
cracklib-check (8)   - Check passwords using libcrack2
create-cracklib-dict (8) - Check passwords using libcrack2
grpconv (8)          - convert to and from shadow passwords and groups
grpunconv (8)        - convert to and from shadow passwords and groups
grub2-setpassword (8) - Generate the user.cfg file containing the hashed grub bootloader password.
grub2-mkpasswd-pbkdf2 (1) - Generate a PBKDF2 password hash.
grub2-set-password (8) - Generate the user.cfg file containing the hashed grub bootloader password.
lchage (1)           - Display or change user password policy
login.defs (5)       - shadow password suite configuration
lpasswd (1)          - Change group or user password
openssl-passwd (1ssl) - compute password hashes
openssl-srp (1ssl)   - maintain SRP password file
pam_cracklib (8)     - PAM module to check the password against dictionary words
pam_pwhistory (8)    - PAM module to remember last passwords
pam_pwquality (8)    - PAM module to perform password quality checking
pam_unix (8)         - Module for traditional password authentication
password-auth (5)    - Common configuration file for PAMified services
pwck (8)             - verify integrity of password files
pwconv (8)           - convert to and from shadow passwords and groups
pwhistory_helper (8) - Helper binary that transfers password hashes from passwd or shadow to opasswd
pwmake (1)           - simple tool for generating random relatively easily pronounceable passwords
pwscore (1)          - simple configurable tool for checking quality of a password
pwunconv (8)         - convert to and from shadow passwords and groups
secret-tool (1)      - Store and retrieve passwords
shadow (3)           - encrypted password file routines
shadow (5)           - shadowed password file
smbpasswd (5)        - The Samba encrypted password file
smbpasswd (8)        - change a user's SMB password
srp (1ssl)           - maintain SRP password file
sslpasswd (1ssl)     - compute password hashes
systemd-ask-password (1) - Query the user for a system password
systemd-ask-password-console.path (8) - Query the user for system passwords on the console and via wall
systemd-ask-password-console.service (8) - Query the user for system passwords on the console and via wall
systemd-ask-password-wall.path (8) - Query the user for system passwords on the console and via wall
systemd-ask-password-wall.service (8) - Query the user for system passwords on the console and via wall
systemd-tty-ask-password-agent (1) - List or process pending systemd password requests
unix_chkpwd (8)      - Helper binary that verifies the password of the current user
unix_update (8)      - Helper binary that updates the password of a given user
vigr (8)             - edit the password, group, shadow-password or shadow-group file
vipw (8)             - edit the password, group, shadow-password or shadow-group file
```

</details>
<br>

See how many results are using **wc**. Column 1 is number of lines, column 2 is number of words, and column 3 is file size in bytes.

```bash
$ man -k password | wc
     43     426    3064
```

The first result looks promising. We check **man chage** and read it. Click Shift-g to go to the bottom of it, on the **SEE ALSO**. Here we see **passwd**.

```bash
$ man chage

SEE ALSO
       passwd(5), shadow(5).
```

We could also simply type **man pass** and press tab twice to see what auto completion will return.

```bash
$ man pass

passphrase-encoding  passwd               password-auth
```

If we check **man passwd**, we see the correct way to set a user's password

```bash
$ man passwd

PASSWD(1)                                             User utilities                                            PASSWD(1)

NAME
       passwd - update user's authentication tokens

SYNOPSIS
       passwd [-k] [-l] [-u [-f]] [-d] [-e] [-n mindays] [-x maxdays] [-w warndays] [-i inactivedays] [-S] [--stdin] [-?]
       [--usage] [username]
```

</details>

<details><summary> Task 2: Solution </summary>

#### 2. Search the **man** page on how create a user with name **ted**

Type in  man user then press tab twice to see what it will return.

```bash
$ man user

useradd        user_caps      user.conf.d    user_contexts  userdel        userhelper     usermod        users
```

We could then use *--help* to see a shorter version for man page of useradd.

<details><summary> useradd --help</summary>

```bash
$ sudo useradd --help

Usage: useradd [options] LOGIN
       useradd -D
       useradd -D [options]

Options:
  -b, --base-dir BASE_DIR       base directory for the home directory of the
                                new account
  -c, --comment COMMENT         GECOS field of the new account
  -d, --home-dir HOME_DIR       home directory of the new account
  -D, --defaults                print or change default useradd configuration
  -e, --expiredate EXPIRE_DATE  expiration date of the new account
  -f, --inactive INACTIVE       password inactivity period of the new account
  -g, --gid GROUP               name or ID of the primary group of the new
                                account
  -G, --groups GROUPS           list of supplementary groups of the new
                                account
  -h, --help                    display this help message and exit
  -k, --skel SKEL_DIR           use this alternative skeleton directory
  -K, --key KEY=VALUE           override /etc/login.defs defaults
  -l, --no-log-init             do not add the user to the lastlog and
                                faillog databases
  -m, --create-home             create the user's home directory
  -M, --no-create-home          do not create the user's home directory
  -N, --no-user-group           do not create a group with the same name as
                                the user
  -o, --non-unique              allow to create users with duplicate
                                (non-unique) UID
  -p, --password PASSWORD       encrypted password of the new account
  -r, --system                  create a system account
  -R, --root CHROOT_DIR         directory to chroot into
  -P, --prefix PREFIX_DIR       prefix directory where are located the /etc/* files
  -s, --shell SHELL             login shell of the new account
  -u, --uid UID                 user ID of the new account
  -U, --user-group              create a group with the same name as the user
  -Z, --selinux-user SEUSER     use a specific SEUSER for the SELinux user mapping
```

</details>
<br>

As a root, create the username **ted**.

```bash
$ sudo useradd ted
```

</details>

<details><summary> Task 3: Solution </summary>

#### 3. Set the password for user **ted** to "@Admin123"

This is the normal way to set passwords for a user account.

```bash
$ passwd ted

Changing password for user ted.
New password:
Retype new password:
passwd: all authentication tokens updated successfully.
```

Another method is to use the "echo".

```bash
echo "@Admin123" | passwd --stdin ted 
```

</details>

<details><summary> Task 4: Solution </summary>

#### 4. Display ted's group and then delete user ted

To display ted's group,

```bash
id ted
groups ted
```

To delete ted's account,

```bash
sudo userdel -r ted 
```

</details>

<details><summary> Task 5: Solution </summary>

#### 5. Create user **robin** with a primary group of "IT" and the secondary group of "Devops"

Check first if the groups are created.

```bash
grep -e it -e devops /etc/group 
```

If they don't exist yet, create them.

```bash
groupadd it
groupadd devops
```

Create the user and use "-g" to specify the primary group and "-G" to specify the secondary group.

```bash
useradd -g it -G devops robin
```

</details>

<details><summary> Task 6: Solution </summary>

#### 6. Lock robin's account and then unlock it

To lock a user account,

```bash
sudo usermod -L robin
```

When an account is locked, user cannot log into his/her account even if the password is known. The first exclamation mark (!) in the /etc/shadow file will confirm that the account is locked.


To unlock a user account,

```bash
sudo usermod -U robin
```

</details>

<details><summary> Task 7: Solution </summary>

#### 7. Make /etc your current working directory.

Use **pwd** to print your current working directory and **cd** to change directory.

```bash
$ pwd
/root
$ cd /etc/
$ pwd
/etc
```

</details>

<details><summary> Task 8: Solution </summary>

#### 8. Use globbing to show all files in /etc that have a number in their name.

This will list all files, directories, and files in those directories that have a "number" in their names.

```bash
$ ls *[0-9]*
DIR_COLORS.256color  grub2.cfg  krb5.conf  mke2fs.conf

dbus-1:
session.conf  session.d  system.conf  system.d

iproute2:
bpf_pinning  ematch_map  group  nl_protos  rt_dsfield  rt_protos  rt_realms  rt_scopes  rt_tables

krb5.conf.d:
crypto-policies  kcm_default_ccache

pkcs11:
modules

polkit-1:
localauthority  localauthority.conf.d  rules.d
```

To list just the directories,

```bash
$ ls *[0-9]* -d
dbus-1               grub2.cfg  krb5.conf    mke2fs.conf  polkit-1  rc1.d  rc3.d  rc5.d  sasl2
DIR_COLORS.256color  iproute2   krb5.conf.d  pkcs11       rc0.d     rc2.d  rc4.d  rc6.d  X11
```

</details>

<details><summary> Task 9: Solution </summary>

#### 9. Still in /etc, display the contents page by page. Then switch to your home directory.

Pipe with **less** t display results page by page. Press spacebar to check each pages.

```bash
$ ls -l | less
```

To switch to your home directory,

```bash
$ cd
$ pwd
/root
```

</details>

<details><summary> Task 10: Solution </summary>

#### 10. Create a file named "ninjas" that contains the name of all four

Use vim to create the file.

```bash
$ vim ninjas

# inside vim
tedangelo
donatello
leonardo
raphael
```

Another way is the command below. Type the names at the next line then to save, hit Ctrl-D.

```bash
$ cat > ninjas-2
tedangelo
donatello
leonardo
raphael
```

</details>

<details><summary> Task 11: Solution </summary>


#### 11. Display the version of your RHEL.
```bash
$ cat /etc/redhat-release

Red Hat Enterprise Linux release 8.5 (Ootpa)
```

</details>
<br>



As always, happy learning! 😀





