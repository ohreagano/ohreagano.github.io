---
layout: post
Title: Assignment 5 - Databases
category: inls161
---

For this assignment, we set up a database with one table, and then wrote a script that accepts user input and 
puts it in the table. This is then exported via a dump into an SQL file so that it can be pulled into other 
workspaces. 

My fork of the repository can be found [here](https://github.com/ohreagano/mcrgirls-assignment-5).


# Setting up for the script

Before running the script, users have to check that they have already created the database and table in their 
workspace. This can be done with the following commands in the mysql> prompt.

`SHOW databases;`

If database5 is in the table of databases, great! If not, the user must run the setup code found in the README.md 
file to set up the database. To confirm that the table is set up, use the following commands, also in the mysql> prompt.

`USE database5;`

`SHOW tables;`

There should be just one table named table5. If it's not there, the user will have to go back and go through the setup code.

Now that the database and table are all setup, the script can take over and do most of the work. 


# How does the script work?

The script first asks for the user's cloud9 username. This is assigned to a variable and later used in mysql commands. 
Having a username and password is necessary to connect the mySQL server with the command-line client. This part won't work unless
the user enters their correct c9 username. We left the password blank in all instances where it is required.

Next the script imports the database from the dump in a SQL file. This pulls in the most up-to-date version of the database.

Then the script asks a series of questions and stores user input into variables. The script also generates a date stamp and unique
ID for each pass through the script and stores each in a variable. This information is then inserted directly into
the mysql database via the mysql 'insert' command.

After inserting new data, the script dumbs the updated database back into the sql file so other users can pull the data
into their workspace.


# Reflection

This has been the most challenging assignment for me all semester. We encountered so many problems trying to get the mysql 
commands to execute from the bash file. We kept recieving error messages related to the username and password and tried 
many strategies to work around them. I think we just had a lot of problems all piled into the script, so it was hard to 
isolate them and understand what our changes to the script were doing.

It was also challenging to work with three people off of one computer screen, especially when we were stuck on a problem. We also
could have done a better job splitting up tasks. I did a lot of reading about the user and password issues, and worked on the
code for the insert command and the database setup commands.

I learned that it's important to break down the problem into tiny pieces and test each before adding it into the big picture.
If I were to approach this problem again, I would start small and build onto it, making sure that the main script always runs,
so that when adding something new its easier to tell where the errors are.

