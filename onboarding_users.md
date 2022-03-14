# **AUX PROJECT 1: SHELL SCRIPTING** #

In this project, we will onboard 20 new Linux users onto a server. A shell script that reads a csv file that contains the first name of the users to be onboarded will be created.

Create Project Folder
~~~
mkdir Shell
~~~

Create a .csv file and insert 20 names
~~~
sudo nano Shell/names.csv
~~~

Insert these names, one per line
~~~
australia
austria
belarus
belgium
chile
chad
cameroon
denmark
england
egypt
finland
faroeisland
ghana
hungary
isreal
jamaica
kenya
malta
nigeria
niger
~~~

Create a user group, developers
~~~
sudo groupadd developers
~~~

In your current user account, update the public and priate keys

Create .ssh directory in /home
~~~~
mkdir .ssh
~~~~
Copy and paste the public key into .ssh/id_rsa.pub
~~~
sudo nano .ssh/id_rsa.pub
~~~~

Copy and paste the private key into .ssh/id_rsa
~~~
sudo nano .ssh/id_rsa
~~~

Create a script file in /usr/sbin
~~~
sudo nano /usr/sbin/onboarding_users.sh
~~~

Copy this script into the onboarding_users.sh file
~~~
#!/bin/bash

# read the outputs of cut and cat into arrays list1 and list2

readarray -t list1  < <(`echo cut -d: -f1 /etc/passwd`)
readarray -t list2  < <(sudo cat /home/ubuntu/Shell/names.csv)

# create a temporary array from list1 to list3 to hold the content of list1 temporarily

list3=("${list1[@]}")


# declare -p list3

# Checks if users already exits. If value of list3 can be found in list2 forget this value

for j in ${list2[*]}; do
        if [[ $j -eq ${list3[$j]} ]]; then
                unset list3[$j]
        fi
done

# Create users, folders, files and keys

for k in ${list2[*]};  do

        sudo useradd $k
        sudo usermod -a -G developers $k
        sudo mkhomedir_helper $k
        sudo mkdir /home/$k/.ssh/
        sudo touch /home/$k/.ssh/authorized_keys
        sudo echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCzKZyicHxIkklSrNlxsJyyTrcIdBIt84Z0cQb3R4k0jH53kxkaT5hP8tfWTe62LXi7vV86fY+SX7TBNM76XGCbw/6vrMGegm6J1x2i1AiLNwq5nqTjOGn0AIwku4IlCCLAB7tdfRyVuCarmBlwny3lzRyybIUAWXR/D6vpN09MsDILbKdhay+Q/p9OUBMSLPqXdY/QIh/Oe3rVv1lwY3AohNfq7V3tO88zKswfA5iiexNiSYX1myT0OrX8cBE771j9quoNZhQgaLI1mIMtAvnHQChrn9k2nUaO/BMBCQGol5XzGv1ado7hgoVPoluIUD+FGNo/pH4zcmDLICH6drXY/C9MESnkMUPLFxBXKO/OitApY71vRao9nAhAwpVMsy6FqiOb5uawhvhoHYIHTV/f4EtagVagRMP2PxYMYR6jykIV4MPJTkCm+lGhTyMlRu+qRQjdLn8AAtHf4aEV8dIkoGh088DI7eA/4o0wz4OV4upH5ewSFS+5IHmRECEW5Nc=" > /home/$k/.ssh/authorized_keys

done
~~~

Make the script executable
~~~
sudo chmod +x /usr/sbin/onboarding_users.sh
~~~

To check the current users on the server, run
~~~
ls -l /home
~~~

Change directory to /usr/sbin
~~~
cd /usr/sbin
~~~

Run the script
~~~
sudo ./onboarding_users.sh
~~~
The script runs in the background and create all the accounts and necessary files

Check with
~~~
ls -l /home
~~~
You should see a list of all the accounts and their corresponding /home directories
