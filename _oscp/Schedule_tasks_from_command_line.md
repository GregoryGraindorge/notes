Schedule tasks from command line

# Schedule tasks from command line

 *by*  Srini\a

‘*Schedule tasks*‘ is a GUI application using which we can schedule tasks, There is an equivalent utility which provides the same functionality but with the advantage that it can be used from windows command line. This command is *Schtasks*. This is an in-built windows command supported by XP, Vista and Windows 7. Let’s see the syntax of this command with the help of few examples.

**How to schedule a task ?**

If you are logged into the same computer where you want to run the scheduled task, then you can use the below command to create the task.

 **  **

Schtasks create /RU username /RP password /SC schedule_frequency /MO Schedule_modifier /D days /M months /TN taskname /TR Task_command /ST start_time /SD start_day /ED end_date

Now let’s see few examples.

Example 1: Schedule disk defragmentation on every Saturday at 10AM. User credentials are administrator/password.

Schtasks /create /RU administrator /RP adminpassword /SC weekly /D SAT /TN defrag /TR c:\windows\system32\defrag.exe /ST 10:00:00

If the specified username and password are correct, then you would get the below message when you run the above command.

SUCCESS: The scheduled task "defrag" has successfully been created.
If the credentials are not correct, you may get a warning like below.

WARNING: The Scheduled task "defrag5" has been created, but may not run because the account information could not be set.

If there exists a scheduled task with the same name then the error would be:
specified task name already exists in the system.

If you need to use a domain user account to run the task you can specify *domainname\username* with /RU option.

**How to get the list of scheduled tasks?**
Just run Schtasks command and you can see the list of scheduled commands.
C:\>schtasks
TaskName                             Next Run Time            Status
==================================== ======================== ===============
defrag                               10:00:00, 3/12/2011
GoogleUpdateTaskUserS-1-5-21-3567637 11:14:00, 3/6/2011
GoogleUpdateTaskUserS-1-5-21-3567637 13:14:00, 3/5/2011

If you want complete details about each of the tasks you can run the command ‘*Schtasks /query /v*

**Delete  a scheduled task**

We can delete a schedule task using ‘*schtasks /delete /TN task_name*‘ command.  For example, to delete the task we created in the example 1 we can run the below command.

Schtasks /delete /TN defrag
**Delete all the scheduled tasks**
You can run the below command to delete all the scheduled tasks.
schtasks /delete /TN *
**Disable a scheduled task**

There does not seem to be a way to disable a scheduled task from command line. We can delete the tasks as mentioned above.

**Modify  a scheduled task:**

We can change a scheduled task using ‘schtasks /change’ command. Run ‘schtasks /change /?’ for the syntax.