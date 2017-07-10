spree
=====

Project public development nonprofit

----
from ZeroBurner
http://forum.ragezone.com/f858/tutorial-9dragons-server-file-971094/

IMPORTANT:
From my tests what is not working right now is:

- You cannot login any GM character (This can be caused from several factors maybe an alternate client is needed)
- You cannot drop any item to the floor from your char, I suspect this can be solved starting the maps in a certain order but I'm not really sure about that.

All the rest seems to work right now.

Disclaimer: Since I'm a really busy person :D and I'm the person who's writing this tutorial I will post only the procedure using windows server 2003 Enterprise SP2 32 bit
I'm not going to provide any link for licensed software or any cracked item You will be responsible to find everything you need on your own, I will share only free items for your convenience

I've decided to use Windows 2003 Server Enterprise SP2 32bit since I think this windows version is the more robust and reliable I've found until now.


If you want to follow my tutorial you must use the paths as I've specified or the auto-build script will not work.
In any case you are free to change everything on your own :) Everything is free here and I'm not jealous of things I'm releasing!

What do you need:
VMWARE Workstation
Windows 2003 32 bit (you can use the one with SP2 integrated)
If you are using a clean Windows 2003 you will need also the SP2, you can download it from microsoft for free (The SP not the OS) (I've added the SP also to my share)
Python 2.7 32 bit (in my share)
PyODBC for this version of python (in my share)
Apache http sever 2.0 (in my share)
Microsoft Visual C++ 2010 runtime libraries (in my share)


My share link
https://mega.co.nz/#F!cMt0GJ6Y!K5NoaSPdajh893pqarxUZg

Step 1:
Install Windows Server 2003 SP2 on a virtual machine, I'm using VMWARE since I've an ESX server at home but 
you can chose whatever you want, I've tested it also on VMWARE Workstation without any problem.
I've tried on Oracle Virtual Box and IT DOES NOT WORK, at least for me....
I suggest you to create the VM with a BRIDGED network adapter, this will make things easier :)
Each Map works as a separate process, this means you will need a good machine to execute the software. 
I'm using 4 cores and 16Gb of memory and the server runs correctly with all the maps. 
I've not done any regression test to identify the minimum requirements, but I've successfully executed a lot of maps in a 2cores 8Gb of memory machine.
You will need around 60Gb to install everything but I suggest you to create a 100Gb disk if you have enough space.

IMPORTANT NOTE: Is mandatory to define an administrator password otherwise you will
have severe issues installing and using the Database
IMPORTANT NOTE2: You are obliged to use a static ip assignement, no DHCP, tested and is not working
IMPORTANT NOTE3: If you install Windows Server 2003 Standard you will have the 4GB limit, so is useless to give more than 4 GB of memory.

take note of the IP address you assigned to the server, you will need it.

Step 2:
Prepare the environment
REBOOT THE MACHINE EVERY TIME IS REQUESTED! Don't skip reboots or things can give you problems
remember is windows :)

Before all i suggest you to install a good text editor (like scite or Notepad++) on the virtual machine

0) If you are installing on a VM install the guest additions (like vmware tools or whatever)
1) Install SP2
2) Install Microsoft Visual C++ 2010 runtime libraries
3) Install Python 2.7.5 32 bit (install for all users with default options on c:\python27)
4) Install PyODBC (All default options, it should find automatically where to install)
5) Install Apache 2.0 
In Network Domain and in Server Name put your vM Ip address
Put an Email (is not necessarely a valid email) on Admin Email address
Install for all users!
Select Custom and Change the install directory to c:\httpd
Verify you can reach the web server from a web browser

Now we need to configure apache to execute python cgi
a) Remove c:\httpd\Apache2\cgi-bin\printenv.pl
b) Remove everything in c:\httpd\Apache2\htdocs (NOT THE DIRECTORY ITSELF! ONLY THE CONTENT)
c) Edit C:\httpd\Apache2\conf\httpd.conf
Search for uncommented line: Options Indexes FollowSymLinks (if you install from the binary I posted should be row 267)
Change it to: Options Indexes FollowSymLinks ExecCGI 
Search for: #AddHandler cgi-script .cgi (if you install from the binary I posted should be row 778)
Remove the # to uncomment the line and add .py to the end of the line (will become: AddHandler cgi-script .cgi .py)
Save the file
Restart apache 
Start -> All Programs -> Apache HTTP Server 2.0 -> Control Apache Server -> Restart 
copy the testenv.py script to c:\httpd\Apache2\cgi-bin\
Open your web browser to http://<yourvmip>/cgi-bin/testenv.py
you should see a simple web page that will demonstrate python is working and correctly configured
After that you can remove the file c:\httpd\Apache2\cgi-bin\testenv.py

6) Create an administrative account on your VM to run the MSSQL services, this will be the database owner account
I'm lazy to explain how to create it from graphical intefaces so I will guide you trough the old fashion command line style :D
Decide a password, I will reference it as yourpassword on the following lines
open a command prompt on your vm and give the following commands:

net user mssqladmin yourpassword /add
net localgroup Administrators mssqladmin /add

You should set password never expires flag for this user, to do that you can:
press start, right click on my computer click manage
click on Local users and groups
on the right pane dobule click on users
double click mssqladmin
tick password never expires
click ok

7) Install SQL Server 2008 R2 on your machine
For this phase your VM should be able to connect to the internet otherwise you will have to install your prerequisites manually (.net 3.51 and others)
This phase can require up two reboots of your virtual machine
After the setup support file is completed you will be promped for the installation type
Select SQL Server Feature Installation
Install Everything EXCEPT the Reporting Services
On the Server Configuration leave the default instance! This is mandatory or 9d will not work as well as the autoconf script.
Now the service accounts page should open, click on "Use the same account for all SQL Server services"
Insert the credentials of your mssqladmin user
click next
On the next page select Mixed Mode (SQL Server authentication and Windows Authentication) 
specify a password for your sa user (you will need this password really soon :))
Click on Add current user 
Click on Add -> on the box that will open write mssqladmin and click check names; click ok
Click NEXT
again, click Add current user 
Click on Add -> on the box that will open write mssqladmin and click check names; click ok
click next 
click next 
click next
click install
wait :D

8) Create a directory on c:\ called scripts
copy the content of the scripts directory in my share inside the directory c:\scripts

9) Extract the content of the file posted on the release topic (the 9dragons server rar) to c:\9dragons on your virtual machine

10) Copy the file ND_LOG_0_backup.bak from my share to c:\9dragons\DB
Expecially if you are using SQLExpress copy the file ND_HISTORY_0_backup_2012_04_15_021519_2283750.bak on to c:\9dragons\DB replacing the old one

11) Launch the c:\script\autoconf.py script (I suggest you to do from a command prompt)
c:
cd \scripts
autoconf.py

The script will ask you for some details, provide them
1st the IP address of your db server
2nd the password you defined for the SA user on step 7

During this phase your system can become unresponsive if you try to perform other tasks!

12) Copy the file c:\scripts\lump.dat onto C:\9dragons

13) On to the directory svrctl there is a link called LOG_SERVER right click on it and do properties
change the ip at the end of the target with your vm ip address


Step 3:
Import the web content
Under My share there is a web directory
Copy the content of cgi-bin to c:\httpd\Apache2\cgi-bin
Copy the content of htdocs into c:\httpd\Apache2\htdocs
edit the file c:\httpd\Apache2\htdocs\ND1\PATCH.PSC
Change the IP address 192.168.0.211 to your VM ip address

Step 4:

Configure your client to access this 9dragons server
Extract the 9dragons client from the release in your machine (not the VM)
on your machine edit the file C:\windows\system32\drivers\etc\hosts

add the following entries (replacing 192.168.0.211 with your vm ip address)

192.168.0.211	onnetnprotect.http.internapcdn.net
192.168.0.211	nprotect.cdnetworks.net
192.168.0.211	nprotect.pb.in.th
192.168.0.211	nprotect.unbbz.com
192.168.0.211	9dragons.http.internapcdn.net

Launch the ndreg Editor contained inside the client

write your vm ip address into:
Status server
Login Server

into patch server write (replacing 192.168.0.211 with your vm ip address)
http://192.168.0.211

Step 5:
Go back to your VM and launch the script CheckProcedures.py
This script must be executed everyday, I suggest you to schedule it to be executed every midnight and everytime you login...

Go into your VM and launch the Server using the links in your c:\scripts\svrctl folder
you need to execute the scripts in the following order:

1) LOG_SERVER
2) MS 
3) NDLOGIN_US
4) DS_SERVER

if you want to use it I've created a batch file that start the entire server, if you used my configuration script to install the server, and you have c:\scripts\dbauth.py you can use the batch file I'm attaching, this will allow you to start a single component or all the server components in the correct order. 
To use it you need to have the dbauth.py correctly configured, and this is done by the autoconf.py
This will start the server components but not the maps, for the maps I'll post something later today
To work the script needs to be copied into your c:\scripts directory and is contained in my share under the script directory (Start9dServer.bat)




once the DS Server is started completely (it will need something like 30 seconds normally) open the DS_SERVER window
press ESC
write 
set server open
press enter
(This will open the server)

Now launch the maps
There is one shortcurt for each map inside svrctl
launch at least bamboo

Step 6:

Open the client using ndlauncher
if you did everything correctly it should start to patch

You can login with any user that already exist in this db (check the Ninedragons_Accounts to find one) with the default password
On the next days I will post more info on how to change other things... like exp rate and other stuffs

Inside the scripts directory you have a changepassword script that can help a lot :)

To exit any component press esc on the window and write quit... this will close the item
if you close the first map you open everything will crash

Step 6: Additional Configurations

1) Server Rates

There is a file that is loaded everytime a map start, so if you make a change to the file you need to restart the maps that are running.
edit the file c:\9dragons\gs\nfofile\groupbonus.txt
change the multipliers for group 0 the multipliers are self explicative

2) Refine Rates

The file required to change the refine success rate is SERVER ONLY the same file is placed encrypted also on the client but for other purposes, not for the rates. the rates client side are IGNORED. let's say that if you want to change the item required to refine you need to change both the files but if you want just to change the refine rate you need to modify it just server side.

Edit the file c:\9dragons\GS\nfofile\InchantTable.TXT and...

each row represent a refine level, the first number represent the level the other two numbers the min and max success possibilities, change the min/max and you will change the success rate.

The refinement success rate is expressed in 1/1000000 (seven digits) so it goes from 0 that is 0% to 1000000 that is 100% change all the numbers to 10000000

3) Item Mall

Regarding the item mall there is a stored procedure in the database go under MSSQL Server Management, open the CIS_DB > Programmability > Stored Procedures > db.Sp_Purchase_Using right click and press execute Stored Procedure

on the user_id value you need to put the USERNAME
on cart_itemCode you need to write a valid item code
on the game_server write 0
on the item_price write what number you want, does not matter...

Click ok and you will have the item like you purchase something in 9d item mall inside your purchase history.

for a valid list of items, you can find it on c:\9dragons\gs\nfofiles\cashitempackage.txt

inside this file the first number is the valid id that you can use in the procedure, the other numbers represent info about the content. they reference to the item table and this values must match the encrypted version inside the client otherwise it will fail to deliver.

the last column is the description. if you scroll down you will found also the us items.

4) The Official Map management system

Based on the EU Release that shdoc1 has kindly signaled me I've modified the map control system to work on this server.
What you need to do:
Download the updated mapcontrol from my share


extract somewhere on your 9d VM

Let's assume you have extracted it onto c:\MAPCONTROL
To configure it you need to:
1)
edit the file c:\MAPCONTROL\Commander\env.ini
Replace the IP address with your VM ip address
2)
Copy the file c:\9dragons\env\ServerEnv.inf into c:\MAPCONTROL\Commander\
3)
Edit the file C:\MAPCONTROL\Ctrl\env.ini
Replace the IP address with your VM ip address


Now it's configured.

To use it, you need to execute first LocalServerCtl.exe from c:\MAPCONTROL\Ctrl
after is started you need to execute the LocalCommader.exe from c:\MAPCONTROL\Commander

A nice Interface will start... Click on the group 0 from the Group List
Now you can start the maps one by one or all together.

To start a single map you need to click on the Number of the map, the click is not really precise, so click the number until it become blue (Selected) then click ON on Server Command.

If you want to start all the maps (Is really CPU/Memory consuming so if you don't have a good machine you'll need to pay attention to that) you can use the group command, like all on etc etc

5) The Zero's create user stored procedure 

I've created a procedure to create a user into your 9d server

copy the file into your server, double click it and when management studio loads, click on the ! to execute query.

create_add_user.sql is in my share

This will create a stored procedure you can access from: NineDragons_Account > Programmability > Stored Procedures > dbo.pr_Create_Account

Right click and Execute Stored Procedure.

This will require you to insert a username and password and will create account and password data in the various tables.

You can integrate it with any webserver, if the user already exist the procedure will obviously fail.

Making the oper_tool working require two different phases,

one to configure the DB and one to configure the oper_tool itself.

Configuring the DB:

1) Open the MSSQL server Management Studio and expand the GMS db to go into tables
2) Right click on dboLocalServerData and select edit top 200 rows
3) Select the line 1(Yang) and right click on the row, select delete
4) Do the same for row 2 (Yin)
5) click on charac_addr value (should be 10.101.102.51) to edit it
write the ip of your db server there (the VM or wherever you've installed the MSSQL)
6) Move to charac_id and write sa
7) on charac_password write your sa password (the one you have defined during mssql installation and autoconf.py)
8) Leave Charac_dbname as is
9) Move to log_addr and write the ip of your db server there (the VM or wherever you've installed the MSSQL)
10)change log_id to sa
11)change log_password to your sa password (the one you have defined during mssql installation and autoconf.py)
12) Move to ds_addr and write the ip of your db server there (the VM or wherever you've installed the MSSQL)
13) Move to ms_addr and write the ip of your db server there (the VM or wherever you've installed the MSSQL)
14) Move to history_addr and write the ip of your db server there (the VM or wherever you've installed the MSSQL)
15)Move to history_id and write sa
16) Move to history_password and write your sa password (the one you have defined during mssql installation and autoconf.py)
17) Start pressing the TAB KEY until you go to the next line and all the red exclamation marks disappears
18) Right click on dbo.UserDBData and click Edit top 200 rows
19) on address write your vm db ip address
20) on id write sa
21) on password write your password
22) press tab until your red exclamation marks does not disappear
23) edit the top 200 rows of the dbo.OPER_list table
24) search the join date value for the first row and take note on how the date is stored, 
this depend on how your locale is set so will change from machine to machine
is important to take note of this format, you can also copy this value and reuse it
and reuse it for every new account you create
I suggest you to cleanup this table deleting row by row like we did on step 3 for the servers

You will need to repeat this tasks for each machine you want to enable
open the oper_tool on the machine where you want to run the gm_tool
take not of the HOST NAME exactly how is written in the Login table

25) edit the top 200 rows of the dbo.OPER_list table
26) start on the blank line and write the following values:
27) on oper_id write a username of your choice, like admin or gmbla or whatever you like, this user does not need to exist as a 9d user
28) on password write a password for this login
29) on oper_name write something like your name or whatever you want, is not important
30) on oper_nick leave the value as is (NULL)
31) on ip leave the value as is (NULL)
32) on pc_name write the hostname you noted
33) on oper_level write 1
34) on join_date write a date in the style noted on step 24

To configure the oper_tool open it
click on configuration
on DB address write the ip of your db server
on id write sa
on password write your sa password
on the DB write GMS

click apply setting

Now write the ID and PASSWORD you've created and... that's all... if you did everything correctly it should work like a charm.
