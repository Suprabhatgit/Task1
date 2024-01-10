# Task1
# Problem Statement And Solution:

 Scenario There is a customer who came to you with a problem to have a custom linux
command for his operations. Our task is to understand the problem and create a linux
command via bash script as per the instructions.

* Command name - internsctl
* Command version - v0.1.0
# Section A
1. I want a manual page of command so that I can see the full documentation of the command.
For example if you execute the command
man ls
as output we get the doc and usage guidelines. Similarly if I execute man internsctl I want
to see the manual of my command.
2. Each linux command has an option --help which helps the end user to understand the use
cases via examples. Similarly if I execute internsctl --help it should provide me the
necessary help

3. I want to see version of my command by executing
internsctl --version
---
* <b>Solution:</b>

<b>step-1:</b> #!/bin/bash

function display_help() {
    echo "Usage: internsctl [OPTIONS]"
    echo "Custom Linux Command for Operations"
    echo ""
    echo "Options:"
    echo "  --help"
    echo "  --version"
}

function display_version() {
    echo "internsctl v0.1.0"
}

case "$1" in
    --help)
        display_help
        ;;
    --version)
        display_version
        ;;
    *)
        echo "Invalid option. Use 'internsctl --help' for usage guidelines."
        exit 1
        ;;
esac

<b>step-2:</b> chmod +x internsctl.sh

<b>step-3:</b> ./internsctl.sh --help

<b>step-4:</b> ./internsctl.sh --version

---

# Section B
I want to execute the following command for -
* Part1 | Level Easy
I want to get cpu information of my server through the following command:

$ internsctl cpu getinfo
Expected Output -
I want similar output as we get from lscpu command

I want to get memory information of my server through the following command:
$ internsctl memory getinfo
Expected Output
I want similar output as we get from free command

---
* <b>Solution:</b>

<b>step-1:</b> Just add bash script in above program : 

  function display_help() {
    echo "  cpu getinfo"
    echo "  memory getinfo "
  }

<b>step-2:</b> function get_cpu_info() {
    lscpu
}

function get_memory_info() {
    free
}


case "$1" in
cpu)
        if [ "$2" == "getinfo" ]; then
            get_cpu_info
        else
            echo "Invalid subcommand for 'cpu'. Use 'internsctl cpu getinfo'."
            exit 1
        fi
        ;;
    memory)
        if [ "$2" == "getinfo" ]; then
            get_memory_info
        else
            echo "Invalid subcommand for 'memory'. Use 'internsctl memory getinfo'."
            exit 1
        fi
        ;;
    *)
        echo "Invalid option. Use 'internsctl --help' for usage guidelines."
        exit 1
        ;;
esac

<b>step-3:</b> chmod +x internsctl.sh

<b>step-4:</b> ./internsctl.sh cpu getinfo & 
                ./internsctl.sh memory getinfo

---

* Part2 | Level Intermediate
I want to create a new user on my server through the following command:
$ internsctl user create <username>
Note - above command should create user who can login to linux system and access his home
directory

I want to list all the regular users present on my server through the following command:
$ internsctl user list

If want to list all the users with sudo permissions on my server through the following command:
$ internsctl user list --sudo-only

---

* <b>Solution:</b>

<b>step-1:</b> 
function display_help() {
echo "  user create <username> Create a new user"
    echo "  user list"
    echo "  user list --sudo-only"
}

<b>step-2:</b> 
function create_user() {
    if [ -z "$2" ]; then
        echo "Error: Missing username. Usage: internsctl user create <username>"
        exit 1
    fi
sudo useradd -m "$2"
echo "User '$2' created successfully."
}

function list_users() {
    cut -d: -f1 /etc/passwd
}


function list_sudo_users() {
    getent group sudo | cut -d: -f4 | tr ',' '\n'
}

<b>step-3:</b>  
case "$1" in
user)
        if [ "$2" == "create" ]; then
            create_user "$@"
        elif [ "$2" == "list" ]; then
            if [ "$3" == "--sudo-only" ]; then
                list_sudo_users
            else
                list_users
            fi
        else
            echo "Invalid subcommand for 'user'. Use 'internsctl user create <username>' or 'internsctl user list [--sudo-only]'."
            exit 1
        fi
        ;;
    *)
        echo "Invalid option. Use 'internsctl --help' for usage guidelines."
        exit 1
        ;;
esac

<b>step-4:</b> chmod +x internsctl.sh

<b>step-5:</b> 
1. ./internsctl.sh user create testuser
2. ./internsctl.sh user list
3. ./internsctl.sh user list --sudo-only

---

* Part3 | Advanced Level
By executing below command I want to get some information about a file
$ internsctl file getinfo <file-name>
Expected Output [make sure to have the output in following format only]
xenonstack@xsd-034:~$ internsctl file getinfo hello.txt
File: hellot.txt
Access: -rw-r--r--
Size(B): 5448
Owner: xenonstack

Modify: 2020-10-07 20:34:44.616123431 +0530

In case I want only specific information then I must have a provision to use options
$ internsctl file getinfo [options] <file-name>
--size, -s to print size
--permissions, -p print file permissions
--owner, o print file owner
--last-modified, m

Expected Output with options
If I want to obtain the size of the specified file only, I should be able to use the following
command:
xenonstack@xsd-034:~$ internsctl file getinfo --size hello.txt
5448
If I want to obtain the permissions of the specified file only, I should be able to use the following
command:
xenonstack@xsd-034:~$ internsctl file getinfo --permissions hello.txt
-rw-r--r--
If I want to obtain the owner of the specified file only, I should be able to use the following
command:
xenonstack@xsd-034:~$ internsctl file getinfo --owner hello.txt
xenonstack
If I want to obtain the last modified time of the specified file only, I should be able to use the
following command:
xenonstack@xsd-034:~$ internsctl file getinfo --last-modified hello.txt
2024-10-07 14:34:44.616123431 +0530
![Screenshot (42)](https://github.com/Suprabhatgit/Task1/assets/141928640/9ba41294-f9d2-4702-932f-40719e6787e6)

![Screenshot (34)](https://github.com/Suprabhatgit/Task1/assets/141928640/01d36067-f142-4d2f-b34d-dfcad9bbbed7)

![Screenshot (35)](https://github.com/Suprabhatgit/Task1/assets/141928640/2db74ddc-50d9-4c46-ac8e-4dd8ca76c9d8)
![Screenshot (36)](https://github.com/Suprabhatgit/Task1/assets/141928640/5c3d5a1e-eafe-4209-958b-648588c7e0b2)
![Screenshot (37)](https://github.com/Suprabhatgit/Task1/assets/141928640/22bc1312-fe44-4f90-adba-b3386a36499d)
