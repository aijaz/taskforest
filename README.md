# TaskForest
---
[![Build Status](https://travis-ci.org/aijaz/taskforest.svg?branch=master)](https://travis-ci.org/aijaz/taskforest)

**TaskForest** is a simple but expressive job scheduler that allows you to
chain jobs/tasks and create time dependencies. It uses text config files to
specify task dependencies.

### Executive Summary 

With the TaskForest Job Scheduler you can:

- schedule jobs run at predetermined times
- have jobs be dependent on each other
- rerun failed jobs
- mark jobs as succeeded or failed
- put jobs on hold and release the holds
- release all dependencies on a job
- automatically rerun jobs that fail
- receive custom emails or SMS when jobs fail, are retried, or succeed
- check the status of all jobs scheduled to run today
- interact with the included web service using your own client code
- interact with the included web server using your default browser
- express the relationships between jobs using a simple text-based format (a big advantage if you like using 'grep')
- browse job logs using the included web server

## About

### A Quick Introduction to Job Schedulers

A Job Scheduler is an application that is responsible for scheduling and running programs (also known as jobs or tasks). The user specifies dependencies between jobs, and the job scheduler runs each job only after all its dependencies have been met. Dependencies may be the successful completion of another job (the prerequisite job), or a certain time of day, or ownership of a limited resource or token.

The job scheduler is also responsible for allowing the operator to rerun failed (or even successful) jobs, mark jobs as having succeeded or failed, and querying the status of all the jobs that are scheduled to run.

Collections of jobs that are dependent on each other are known as Families. Some schedulers call Families "Job Streams."

### TaskForest Design Philosophy

The TaskForest Job Scheduler is designed with the philosophy that a program should be no more complicated than the minimum amount required to achieve its objectives. Complicated functions can be achieved by a conjunction of simpler functions.

To illustrate this, TaskForest has been designed to run as a single command that, once invoked from the command line, launches any jobs that need to be run, and then sleeps for a bit and then tries again. If you want, the taskforest script can also exit and be run repeatedly. It is a stateless application. You could launch taskforest once a day, and have it run all day, or you could run it once every minute (for example) using cron.

Since TaskForest does not need any graphical interface, it can be run on headless servers that don't have X installed. If you would prefer a graphical interface, it also comes with its own web server so that you can administer the application using any standard web browser. We also provide you a RESTful API that allow you to write your own web clients, so that you can control TaskForest from within your program. Everything you need to do to configure the system can be done by editing text files. 

### <a name="text_files"></a>Why Text Files are a Good Idea

One of the initial design requirements of TaskForest was that it be easily configuriable with just a shell prompt and your favorite text editor. Many of the servers I administer are old boxes which I administer by logging into them via ssh. So when it came to designing job dependencies in Family files, I chose a text file representation. The benefits of text files over a graphical user interface for this are many:

- **Easy Remote Access** - All you need is the ability to get to a command line and a text editor on the machine that runs taskforest. With the such low client access requirements, virtually any old machine that has internet access and an ssh client can be used to administer the system. I have often worked on our taskforestd server from a local internet cafe using a Putty.exe downloaded minutes earlier.
- **Mobile Access** - Text files also make work relatively easy using a mobile ssh client like Idokorro Mobile SSH. A dedicated mobile client would be ideal, but short of that, the text file approach assures low bandwidth usage and easy-to-make changes.
- **Flexibility** - The simple, easily parseable format of text files allows us to build richer clients later that would use a graphical interface to specify relationships between jobs.
- **Source Control** - The text based format makes it easy to place the Family files under source control. You can also easily diff different versions of the same family file.
- **Grep** - When you have dozens of family files and hundreds of jobs, you may need to answer questions like: "Are we still running Job J1? It needs to be decommissioned." This can easily be answered by grepping the Family files for job J1.

### Comparing TaskForest to cron

Cron is a very powerful tool, but lacks some important features that most job schedulers perform: It's not easy to specify complex dependencies between jobs. It's also not possible to use cron to rerun jobs or mark jobs as having succeeded so that dependent jobs may run.

### TaskForest Licensing and Pricing

TaskForest is distributed free of charge. It's open-source software that's distributed under the Apache License. For details, please read the LICENSE file.

The above does not apply to the files avilable on the website under the `/yui` url, which is licensed under the [BSD License](https://github.com/yui/yui3/blob/master/LICENSE.md).

### System Requirements

TaskForest is written in Perl. It requires Perl 5.8.3 or higher, and the following libraries (available from [CPAN](http://cpan.org/)):

- [DateTime](http://search.cpan.org/search?mode=all&query=DateTime)
- [Config::General](http://search.cpan.org/search?mode=all&query=Config%3A%3AGeneral) version 2.38 or higher
- [Log::Log4perl](http://search.cpan.org/search?mode=all&query=Log%3A%3ALog4perl) version 1.16 or higher
- [HTTP::Daemon](http://search.cpan.org/search?mode=all&query=HTTP%3A%3ADaemon) version 5.818 or higher
- [HTTP::Status](http://search.cpan.org/search?mode=all&query=HTTP%3A%3AStatus) version 5.817 or higher
- [HTTP::Response](http://search.cpan.org/search?mode=all&query=HTTP%3A%3AResponse) version 5.820 or higher
- [HTTP::Request](http://search.cpan.org/search?mode=all&query=HTTP%3A%3ARequest) version 5.818 or higher
- [Net::SMTP](http://search.cpan.org/search?mode=all&query=Net%3A%3ASMTP) version 2.31 or higher

Most of the testing has been performed on Unix-like operating systems like Linux, FreeBSD, Mac OSX. There has been some testing on Windows, but it is not as exhaustive as that on other operating systems. It is recommended that you run TaskForest on a Unix-like server.

### TaskForest Support

For support, please visit our website at http://www.taskforest.com/ or
create an issue on GitHub.

If you would like commercial support, please contact me on Twitter at <a href="http://twitter.com/@_aijaz_">@_aijaz_</a> or via email at ```taskforest``` ```@``` ```aijaz``` ```dot net```.

### Authors

[Aijaz A. Ansari](http://aijaz.net/)

The following developers have graciously contributed patches to enhance TaskForest:

-  Steve Hulet

Please see the 'Changes' file for details. If you have contributed code,
and your name is not on the above list, please accept my apologies and
let me know, so that I may give you credit.
If you're using this program, I would love to hear from you. Please contact me on Twitter at @\_aijaz\_ and let me know.

### Acknowledgements

Many thanks to the following for their help and support:

- GitHub
- Rosco Rouse
- Svetlana Lemeshov
- Teresia Arthur
- Steve Hulet

I would also like to thank Randal L. Schwartz for teaching the readers
of the Feb 1999 issue of Web Techniques how to write a pre-forking web
server, the code upon which the TaskForest Web server is built.

I would also like to thank the fine developers at Yahoo! for providing
yui to the open source community.

## Installing TaskForest

This program depends on DateTime, for timezone support. You can install DateTime as follows:

```
perl -MCPAN -e 'install DateTime'
```

Similarly, as of version 1.10, it also depends on Config::General and Log::Log4perl. You can install these as follows:

```
perl -MCPAN -e 'install Config::General'
perl -MCPAN -e 'install Log::Log4perl'
```

As of version 1.15, it also depends on the LWP libraries such as HTTP::Daemon, HTTP::Request, etc.

Download the latest TaskForest repository. You can install it the 'normal' way:

```
cd taskforest
perl Makefile.PL
make
make test
make install
```

If you don't have root access, and want to install in a local directory like ~/perl, replace the third line with

```
perl Makefile.PL prefix=~/perl
```

## Configuring TaskForest

There are two main parts to configuring TaskForest: configuring the Family files that specify job dependencies, and setting the options that determine TaskForest's behavior.

### Precedence of Different Options Sources

All settings (required and optional) may be specified in a variety of ways: command line, environment variable and config file. The order of preferences is this: Most options have default values. Settings specified in the config file override those defaults. Settings specified in environment variables take override those specified in the config file and the default values. Setting specified on the command line override those specified in envrionment variables, and those specified in the config files and the default values.

The names of the environment variable are the same as the names of the settings on the command line (or in the config file), but they should be in UPPER CASE, with `TF_` prepended. For example, the environment variable name for the [run_wrapper][] setting is `TF_RUN_WRAPPER`.

## Jobs

A job is defined as any executable program that resides on the file system. It is represented as a file in the files system whose name is the same as the job name. Jobs can depend on each other. Jobs can also have start times before which a job may not by run.

When a job is run by the run wrapper (`bin/run_with_log`), two status semaphore files are created in the [log directory][]. The first is created when a job starts and has a name of `$FamilyName.$JobName.pid`. This file contains some attributes of the job. When the job completes, more attributes are written to this file.

When the job completes, another semaphore file is written to the [log directory][]. The name of this file will be `$FamilyName.$JobName.0` if the job ran successfully, and `$FamilyName.$JobName.1` if the job failed. In either case, the file will contain the exit code of the job (0 in the case of success and non-zero otherwise).

When a job is run by the `run_with_log` run wrapper, any output the job sends to stdout or stderr will be captured and stored in a file called `$FamilyName.$JobName.$pid.$start_time.stdout` in the [log directory][].

## Job Status

Within TaskForest, every job has a status, which is one of the following values:

- **Waiting** - One or more dependencies of the job have not been met.
- **Ready** - All dependencies have been met; the job will run during the next iteration of the taskforest [main loop][].
- **Running** - The job is currently running
- **Success** - The job has run successfully
- **Failure** - The job was run, but the program exited with a non-zero return code

## Families

Jobs can be grouped together into **families**. A family has a start time associated with it before which none of its jobs may run. A family also has a either (a) a list of days-of-the-week or (b) a calendar associated with it. Jobs within a family may only run on the days specified by the days-of-the-week or the calendar.

Jobs and families are given simple names. A family is described in a family file whose name is the family name. Each family file is a text file that contains 1 or more job names. The layout of the job names within a family file determine the dependencies between the jobs (if any). There are [several reasons why text files are a good choice for Family files][].

Family names and job names should contain only the following characters:
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789_

Let's see a few examples. The main script expects environment variables or command line options or configuration file settings that specify the locations of the directory that contain family files, the directory that contains job files, and the directory where the logs will be written. The directory that contains family files should contain only family files.

#### EXAMPLE 1 - Family file named F_ADMIN

```
start => '02:00', tz => 'GMT', days => 'Mon,Wed,Fri'

 J_ROTATE_LOGS()
```
  
The first line in any family file always contains 3 bits of information about the family: the start time, the time zone, and the days on which this jobs in this family are run, or the calendar that specifies on which dates jobs in this family are run.

In this case, this family starts at 2:00 a.m. Chicago time. The time is adjusted for daylight savings time. This family 'runs' on Monday, Wednesday and Friday only. Pay attention to the format: it's important.

Valid days are 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'. Days must be separated by commas.

All start times (for families and jobs) are in 24-hour format. '00:00' is midnight, '12:00' is noon, '13:00' is 1:00 p.m. and '23:59' is one minute before midnight.

There is only one job in this family - J_ROTATE_LOGS. This family will start at 2:00 a.m., at which time J_ROTATE_LOGS will immediately be run. Note the empty parentheses [()]. These are required.

What does it mean to say that J_ROTATE_LOGS will be run? It means that the system will look for a file called J_ROTATE_LOGS in the directory that contains job files. That file should be executable. The system will execute that file (run that job) and keep track of whether it succeeded or failed. The J_ROTATE_LOGS script can be any executable file: a shell script, a perl script, a C program etc.

To run the program, the system actually runs a wrapper script that invokes the job script. The location of the wrapper script is specified on the command line or in an environment variable.

Now, let's look at a slightly more complicated example:

#### EXAMPLE 2 - Job Dependencies

This family file is named WEB_ADMIN.

```
start => '02:00', tz => 'GMT', calendar => 'Weekdays'

               J_ROTATE_LOGS()

 J_RESOLVE_DNS()       Delete_Old_Logs()

               J_WEB_REPORTS()      

    J_EMAIL_WEB_RPT_DONE()  # send me a notification
```
  
A few things to point out here:

- Blank lines are ignored.
- A hash (#) and anything after it, until the end of the line is treated as a comment and ignored.
- Job and family names do not have to start with J_ or be in upper case.
- Instead of specifying days of the week on which this Family may run, this Family specifies a calendar to which it adheres. The calendar used in this case is called 'Weekdays.' For a detailed description of calendars, please see the [calendars][] documentation.

It is possible to have a dependency on a job that's in another family. If, for example, J_ROTATE_LOGS was in the Family named LOGS, then the family above would look like this:

```
start => '02:00', tz => 'GMT', calendar => 'Weekdays'

            LOGS::J_ROTATE_LOGS()

 J_RESOLVE_DNS()       Delete_Old_Logs()

               J_WEB_REPORTS()      

    J_EMAIL_WEB_RPT_DONE()  # send me a notification
```

An external job dependency is different from 'normal' job dependencies, because unlike 'normal' dependencies, it specifies only the dependency, and not when the external job should run. This means that looking at the above family file, we cannot say when J_ROTATE_LOGS will run. More accurately, we cannot say when LOGS::J_ROTATE_LOGS will run. All we know is that after it runs, J_RESOLVE_DNS and Delete_Old_Logs can run (after 2:00 GMT).

This also means that external job dependencies may only be specified on the first line of a Family, or the first line of a group of jobs (see example 4). Therefore the following is not allowed:

```
start => '02:00', tz => 'GMT', calendar => 'Weekdays'

            LOGS::J_ROTATE_LOGS()

 J_RESOLVE_DNS()       Delete_Old_Logs()

            REPORTS::J_WEB_REPORTS()  # BAD!

    J_EMAIL_WEB_RPT_DONE()  # send me a notification
```
  
To see how this should be written, we need to know about Job Forests. Since that's described in example 4, we'll defer the solution until then.

One last thing about external job dependencies: just because we're waiting on a job in another family, that doesn't mean that the same job cannot be run in this family. For example, the following is permitted:

```
start => '02:00', tz => 'GMT', calendar => 'Weekdays'

            LOGS::J_ROTATE_LOGS()

 J_RESOLVE_DNS()       Delete_Old_Logs()

               J_WEB_REPORTS()

    J_EMAIL_WEB_RPT_DONE()  # send me a notification

                J_ROTATE_LOGS()   # This is a different
                                  # job!
```
  
The family will not start until J_ROTATE_LOGS has run from the LOGS family. The last job run by this family will be J_ROTATE_LOGS. It has nothing to do with the instance of the job that ran in the LOGS family. Line 11 will actually run the job, while line 3 only checks whether the job has run (by another family). That's what I mean when I say that external dependencies only specify the dependencies, while normal dependencies also specify when the job should run.

#### EXAMPLE 3 - Time Dependencies

Let's say that we don't want J_RESOLVE_DNS to start before 9:00 a.m. because it's very IO-intensive and we want to wait until the relatively quiet time of 9:00 a.m. In that case, we can put a time dependency of the job. This adds a restriction to the job, saying that it may not run before the time specified. We would do this as follows:

```
start => '02:00', tz => 'GMT', calendar => 'Weekdays'

               J_ROTATE_LOGS()

 J_RESOLVE_DNS(start => '09:00')  Delete_Old_Logs()

               J_WEB_REPORTS()      

    J_EMAIL_WEB_RPT_DONE()  # send me a notification
```

J_ROTATE_LOGS will still start at 2:00, as always. As soon as it succeeds, Delete_Old_Logs is started. If J_ROTATE_LOGS succeeds before 09:00, the system will wait until 09:00 before starting J_RESOLVE_DNS. It is possible that Delete_Old_Logs would have started and complete by then. J_WEB_REPORTS would not have started in that case, because it is dependent on two jobs, and both of them have to run successfully before it can run.

For completeness, you may also specify a timezone for a job's time dependency as follows:

```
J_RESOLVE_DNS(start=>'10:00', tz=>'America/New_York') ...
```

#### EXAMPLE 4 - Job Forests

You can see in the example above that line 03 is the start of a group of dependent jobs. No job on any other line can start unless the job on line 03 succeeds. What if you wanted two or more groups of jobs in the same family that start at the same time (barring any time dependencies) and proceed independently of each other?

To do this you would separate the groups with a line containing one or more dashes (only). Consider the following family:

```
start => '02:00', tz => 'GMT', calendar => 'Weekdays'

               J_ROTATE_LOGS()

 J_RESOLVE_DNS(start => '09:00')    Delete_Old_Logs()

               J_WEB_REPORTS()      

    J_EMAIL_WEB_RPT_DONE()  # send me a notification

-------------------------------------------------------

 J_UPDATE_ACCOUNTS_RECEIVABLE()

 J_ATTEMPT_CREDIT_CARD_PAYMENTS()

-------------------------------------------------------

 J_SEND_EXPIRING_CARDS_EMAIL()
```

Because of the lines of dashes on lines 11 and 17, the jobs on lines 03, 13 and 19 will all start at 02:00. These jobs are independent of each other. J_ATTEMPT_CREDIT_CARD_PAYMENT will not run if J_UPDATE_ACCOUNTS_RECEIVABLE fails. That failure, however will not prevent J_SEND_EXPIRING_CARDS_EMAIL from running.

Finally, you can specify a job to run repeatedly every 'n' minutes, as follows:

```
start => '02:00', tz => 'GMT', calendar => 'Weekdays'

 J_CHECK_DISK_USAGE(every=>'30', until=>'23:00')
```

This means that J_CHECK_DISK_USAGE will be called every 30 minutes and will not run on or after 23:00. By default, the 'until' time is 23:59. If the job starts at 02:00 and takes 25 minutes to run to completion, the next occurance will still start at 02:30, and not at 02:55. By default, every repeat occurrance will only have one dependency - the time - and will not depend on earlier occurances running successfully or even running at all. If line 03 were:

```
J_CHECK_DISK_USAGE(every=>'30', until=>'23:00', chained=>1)
```

...then each repeat job will be dependent on the previous occurance.

Now, let's get back to our discussion of external dependencies from example 3. I said that an external dependency may only be specified on the first line of the file or the first line of a group of jobs. This way of specifying a family is not allowed by TaskForest:

```
start => '02:00', tz => 'GMT', calendar => 'Weekdays'

            LOGS::J_ROTATE_LOGS()

 J_RESOLVE_DNS()       Delete_Old_Logs()

            REPORTS::J_WEB_REPORTS()  # BAD!

    J_EMAIL_WEB_RPT_DONE()  # send me a notification
```

With a few minor modifications, the family can be specified correctly:

```
start => '02:00', tz => 'GMT', calendar => 'Weekdays'

            LOGS::J_ROTATE_LOGS()

 J_RESOLVE_DNS()       Delete_Old_Logs()

--------------------------------------------

 J_RESOLVE_DNS() Delete_Old_Logs() REPORTS::J_WEB_REPORTS()
  
   J_EMAIL_WEB_RPT_DONE()  # send me a notification
```
  
We've moved the external dependency to the first line of it's own section. Now J_EMAIL_WEB_RPT_DONE relies on all 3 jobs, 2 that run in this Family, and one from the REPORTS family.

## Tokens

A token is a dependency. It is something that a job must 'possess' before it can run, if that job needs that token. You can create different types of tokens, giving each type a common name. You can also specify how many instances of tokens of each type are to exist. For example, if the configuration file contained the following lines:

```
 ...   
 <token T>
   number = 1
 </token>
 <token U>
   number = 2
 </token>
 ...
```

...it means that there are two types of tokens: 'T' and 'U'. There is only one instance of token type 'T', and two of type 'U'.

Given the above configuration, if your Family file looked as follows:

```
start => '00:00', tz => 'GMT', days => 'Mon,Wed,Fri'

 J1( token => 'T')  J2 ( token => 'T' ) J3()

-------------------------------------------------------

 J6(token => 'U') J5(token => 'U') J4(token => 'U')
 J8(token => 'T,U')
 
```

...then that means that job J1 and J2 both need a token of type 'T' to run. But, there's only one instance of token T, so J1 and J2 cannot both run at the same time (even though they would, if they didn't rely on tokens). The system will sort jobs alphabetically by name and choose the first in the list. In other words, in this case, J1 will run first and J2 will only run after J1 completes (if no other job has taken the token first). To be more accurate, J1 and J3 will run simultaneously, since J3 does not need any tokens.

To be even more accurate, J1, J3, J4 and J5 will run simultaneously. This is because J4, J5 and J6 all rely on token U, but there are only 2 instances of token U. Even though J6 appears on the line before J5 and J4, the system will choose J4 and J5 first, because they appear first in alphabetical order, and J6 will run after one of the other two have completed.

Because the system always chooses the job with the smallest name (alphabetically), it is possible to experience 'resource starvation' - where a job with a 'larger' name could never get an opportunity to run, because there are too many other jobs with smaller names that get to run first by virtue of their names. Future versions of TaskForest will implement heuristics to prevent resource starvations.

Note that J8 relies on two tokens: T and U. It will only run when it can acquire one of both tokens. If it can acquire one, but not the other, it will release the first and try to acquire both at a later time.

Finally, tokens can also be used to control the load on the machine on which taskforest is running. If you've got several independent jobs that don't depend on each other, but which use a fair amount of resources, you can have all the jobs use the same token. Then you can tweak the maximum number of instances of that token to a value that maximizes the number of simultaneous jobs without putting too much strain on the server.

## <a name="calendars"></a> Calendars

A calendar is a set of rules that defines on what days a job may run. The rules that make up a calendar are specified in the configuration file and the calendars themselves are associated with a Family in the Family file.

Calendar names should contain only the characters shown below:
ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789_.-

Let's start with the configuration file. You can have zero or more calendars in the configuration file. Each calendar may be associated with zero or more Families. A calendar consists of one or more rules. All rules belonging to a calendar are consulted in order to determine whether or not the Family should run today. The rules are consulted in the order in which they are specified in the configuration file. Rules may contradict each other. A rule may not conclusively determine whether or not the Family should run today. The last rule that determines whether or not a Family should run today will override any earlier rules. Rules are case insensitive.

Let's see some examples. First, let's see how a Family file specifies a calendar:

#### Example: One day only

```
start => '00:00', tz => 'GMT', calendar => 'NY2010'

 ...
```

You can see here that instead of the `days => '...'`, we have `calendar => 'NY2010'`. This tells the system that this family will rely on the calendar named `NY2010`. This calendar is defined in its own file. The file name should be the same as the calendar name. The directory in which the file exists is specified by the [calendar_dir][] option:

```
...
calendar_dir = "/foo/bar/calendars"
...
```

The file /foo/bar/calendars/NY2010 looks like this (the # symbols and anything after them are comments, just like in the [configuration file][]):

```
# NY2010
+ 2010/01/01  # Only valid on New Years Day, 2010
```

This calendar only has one rule. It is "+ 2010/01/01". The '+' in the rule says that if the date specified in this rule matches, then the Family must run on that day. The '+' is optional. This calendar will allow the Family that uses it to run on Jan 1, 2010, and on no other day. Dates in rules should be in the YYYY/MM/DD format.

#### Example: One month only
What if we want a job to run on every day in November 2010? You can use a rule like this:

```
# Nov2010
        
2010/11/*
```

The '*' in the DD part of the date is like a wildcard. It means that the DD part of the rule will match any number. In other words, if today's date is November DD, 2010, then this rule will match, for all values of DD. Note that the '+' is missing here. That's ok. It's optional, and if missing, the system will assume that you meant to put in a '+'.

#### Example: Rejecting days
If, on the other hand, you wanted this Families that use this calendar to run on all days in 2010 except all of November, you would use the '-' sign:

```
# All_But_Nov2010
        
 + 2010/*/*
 - 2010/11/*
```

The first line matches all days in 2010. The MM and DD parts are both wildcards. The optional plus tells the system that if the date matches this pattern, it should count as a valid run date. The next line, on the other, hand adds an exception to this rule: If the date falls within November 2010, the date should not be a valid run date - note the '-' sign that tells the system to exclude this date.

#### Example: Daily Calendar

To specify a daily calendar, use this:

```
# Daily

*/*/*
```

Of course, you don't have to name the calendar 'Daily.' You can name it whatever you want. Using this calendar is equivalent to having `days=>'Mon,Tue,Wed,Thu,Fri,Sat,Sun'` in the Family file.

#### Example: Specifying days of the week

You can also specify rules that specify days of the week with a qualifier. For example, to run a job on the first Monday of every month in 2009, you should use a rule like this:

```
# FirstMon09

+ first Mon 2009/*
```

A couple of points to mention here: First, the '+' is optional here as well. Second, the date part of the rule only has the year and the month (in YYYY/MM format). When you use a qualifier like 'first,' it makes no sense to say things like 'The first Monday of the 1st of every month.'

The word 'first' in line 2 above is called a qualifier. Valid qualifiers are:

- first
- second
- third
- fourth
- fifth
- last
- second last
- third last
- fourth last
- every

The qualifiers 'first last,', 'last last' and 'every last' also work in version 1.25, but they may stop working in a future version, so don't get into the habit of using those qualifiers.

Unlike the 'days' specifier in the family file, the days of the week in calendar rules may be spelled out. Only the first 3 characters are significant.

#### Calendar Recipes

The following 'recipes' show you some useful calendar rules:

```
# ############################################################
# Run every day
#
+ */*/*
```

```
# ############################################################
# Run on weekdays only
#
*/*/*
- every Saturday */*
- every Sunday */*

# You could also replace the 3 lines above
# with 5 '+' lines, one for each weekday.
```

```
# ############################################################
# 'Thanksgiving Day' observed in the U.S.
#
fourth Thursday */11
```

```
# ############################################################
# 'Thanksgiving Day' observed in Canada
#
second Monday */10
```

```
# ############################################################
# 'Memorial Day' observed in the U.S.
#
last Monday */5
```

```
# ############################################################
# The day Daylight Saving Time starts in the U.S.
#
second Sun */03  # this rule is valid for dates
                 # after 2007, but not earlier
```

```
# ############################################################
# The day Daylight Saving Time ends in the U.S.
#
first Sun */11   # this rule is valid for dates
                 # after 2007, but not earlier
```

```
# ############################################################
# The day Daylight Saving Time starts in Europe
#
last Sun */03    # tested for 2009
```

```
# ############################################################
# The day Daylight Saving Time ends in Europe
#
last Sun */10    # tested for 2009
```

## Automatic Retries

TaskForest can be configured so that when a job fails, it will automatically retry running the job. There is a system-wide option called [num_retries][] that specifies how many times the job will be retried. The [retry_sleep][] option specifies how many seconds the system will wait before trying to rerun the job.

The [num_retries][] and [retry_sleep][] options may optionally be specified for each job as well. For example, if the configuration file contains this...

```
# This is the number of times to automatically
# retry running a job that fails 
[num_retries](num_retries)              = 1

# Wait these many seconds before automatically
# retrying running a job that fails 
retry_sleep              = 300
```

...then if any job fails, the run wrapper will sleep for 300 seconds and then retry the job once. If the retry fails as well, then the job will be considered to have failed. If the retry succeeds, then job will have considered to have run successfully. During 300 second sleep period, and during the retries, the official status of the job will still be 'Running.'

The responsibility of implementing the auto-retries falls on the run wrapper. Even though TaskForest ships with two run wrappers, you really should use run_with_log and not run. When a job is being retried, run_with_log will note it the log file like this:

```
*****************************************************************
Start Time:   Mon Mar 22 17:57:21 2010
Family:       RETRY
Job:          J_Retry
Job File:     J_Retry
Log Dir:      logs/20090503
Script Dir:   jobs
Pid File:     logs/20090503/RETRY.J_Retry.pid
Success File: logs/20090503/RETRY.J_Retry.0
Failure File: logs/20090503/RETRY.J_Retry.1
Out/Err File: logs/20090503/RETRY.J_Retry.19258.1269298641.stdout

*****************************************************************

!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!! Current Time: Mon Mar 22 17:57:21 2010
!! Exit Code:    256
!! 
!! Job failed.  Sleeping 2 seconds and then retrying (retry 1 of 1).
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

*****************************************************************
Start Time: Mon Mar 22 17:57:21 2010
End Time:   Mon Mar 22 17:57:23 2010
Duration:   2 seconds
Exit Code:  0
*****************************************************************
```

You can see that in this case, TaskForest had been instructed to sleep for 2 seconds before retrying, and that there was to be only one retry.

You can also override these configuration settings for individual jobs. Let's say you have your configuration file set up as shown above, with one retry after 300 seconds. Now suppose you want job J_Retry to be retried 10 times, with a two-minute sleep between retries. You could specify the overrides directly in the family file as follows:

```
...
J_Retry (num_retries => 10, retry_sleep => 120)
...
```
  
This local specification of ten retries spaced two minutes apart will override what you have specified in the configuration file. You can override these configuration options on as many jobs as you wish.

## Emails

You can configure TaskForest to send you an email or SMS message (text) in any of these three situations:

- When a job fails
- When a job fails but will be automatically retried
- When a job that failed and was automatically retried finally succeeds

You can configure this feature system-wide via the configuration file, and you can also override it for individual jobs in the Family files (the same way retries are configured). You can also completely configure the emails or texts going out in each of the three cases, from the SMTP envelope information to MIME headers to body text. Again, this can be configured across the entire system and overridden for individual jobs and families. It really is a very powerful system that, with the proper email filters, can make managing your job workflow very easy. Let's see how it works.

In order to be able to send emails, you need to provide TaskForest some information in the configuration file:

```
# This is the SMTP server that will be used to send 
# emails out when a job fails, for example
smtp_server              = "localhost"

# The default SMTP port is 25.
smtp_port                = 25

# In a production environment this should be 60 or 120
smtp_timeout             = 60

# This is the SMTP envelope sender 
# (the text after "MAIL FROM:")
smtp_sender                = "user1@example.com"

# This is the email address that appears in the From: 
# mail header
mail_from                = "user1@example.com"

# If a user replies to a received email, the reply
# will go to this address instead of the From: address. 
# This address is set in the Reply-To mail header.
mail_reply_to            = "user2@example.com"

# This is the address to which bounces will be sent if 
# they occur at the SMTP server (as opposed to the 
# receiving Mail Transfer Agent).
mail_return_path         = "user3@example.com"

# This is the directory that stores the contents of
# the emails that are sent by the system. 
instructions_dir         = "instructions"
```

Then there are the following three settings that control who receives the emails. They can be set system-wide in the configuration file:

```
# When a job fails, emails are sent to this address
email                    = "test@example.com"

# When a job fails, but is being automatically retried,
# emails are sent to this address, as opposed to the 
# one stored in the 'email' setting.  If no_retry_email 
# is set, then no email will be sent in this case
retry_email              = "test2@example.com"

# When a job fails, is automatically retried one or more 
# times and then suceededs, emails are sent to this 
# address, as opposed to any of the others.  If 
# no_retry_success_email is set, then no email will be sent
# in this case.
<a href="#calendars">retry_success_email]</a>      = "test3@example.com"
```

Given the setup shown above, the email generated when a job is being retried is shown below. What you see below is a transcript of a SMTP session between TaskForest and a fake SMTP server used for testing. Lines that start with 'S: >' denote text sent from the SMTP server to the client (TaskForest). Lines that start with 'C: <' denote text sent from the SMTP client (TaskForest) to the SMTP server.

```
S: > 200 OK TaskForest Fake SMTP Server
C: < EHLO user1@example.com
S: > 200 OK
C: < MAIL FROM:<user1@example.com>
S: > 200 OK
C: < RCPT TO:<test2@example.com>
S: > 200 OK
C: < DATA
S: > 200 OK
C: < From: user1@example.com
C: < Return-Path: user3@example.com
C: < Reply-To: user2@example.com
C: < To: test2@example.com
C: < Subject: RETRY RETRY::J_Retry
C: < 
C: < This is the TaskForest system at your_machine_name
C: < ------------------------------------------------------
C: < 
C: < 
C: < The following job failed and will be rerun automatically.
C: < 
C: < Family:         RETRY
C: < Job:            J_Retry
C: < Exit Code:      256
C: < Retry After:    2 seconds
C: < No. of Retries: 1 of 1 
C: < 
C: < Instructions that apply to all jobs in the Family named RETRY.
C: < 
C: < Instructions that apply to the jobs named J_Retry.
C: < 
C: < 
C: < 
C: < ------------------------------------------------------------
C: < For help, please see http://www.taskforest.com/
C: < .
S: > 200 OK
C: < QUIT
```

TaskForest also supports use of the [Mailgun][] email service. 

```
# This is your Mailgun API key
mailgun_api_key                = "key-XXXXX"

# This is your mailgun domain name
mailgun_domain                  = "example.com"

# Please be advised: when using Mailgun, the following
# variables are NOT used: 
# * mail_reply_to
# * mail_return_path

# If mailgun_api_key is specified, then
# mail will only be sent via Mailgun, and not over SMTP.
```

TaskForest also supports use of the <a href="https://twilio.com">Mailgun</a> SMS service: 

```
# This is your Twilio Account SID
twilio_account_sid                = "ACXXXX"

# This is your Twilio Auth Token
twilio_auth_token                  = "abcdefj"

# This is your Twilio Phone number
#twilio_phone_number                  = "+13125551212"
```

What's left to be explained is how TaskForest chose the subject and the body of the email or text. The subject of the email will contain the Family name and Job name preceded by 'RETRY', 'FAIL' or 'RETRY_SUCCESS', depending on whether the job failed and is about to be retried (after the sleep time), or failed for the final time, or succeeded after failing and being retried one or more times.

As for the body, TaskForest concatenates the contents of several files found in instructions_dir. If the appropriate file does not exist, it will ignore it and move on to the next one. As it retrieves the contents of each file, TaskForest does some simple substitutions to replace placeholders found within the file's contents with the the value of that variable in the run_wrapper's environment. TaskForest makes all of the environment variables available to the taskforest program available to the run wrapper (run_with_log) and also adds the following variables to the environment:

- **TASKFOREST_FAMILY_NAME** - This is the name of the family in which this job was run.
- **TASKFOREST_JOB_NAME** - This is the name of the job that was run.
- **TASKFOREST_LOG_DIR** - This is the full path of the TaskForest log directory
- **TASKFOREST_JOB_DIR** - This is the full path of the TaskForest job directory
- **TASKFOREST_PID_FILE** - This is the name of the pid file that's used internally by TaskForest
- **TASKFOREST_SUCCESS_FILE** - This is the name of the file that's created if the job succeeded.
- **TASKFOREST_FAILURE_FILE** - This is the name of the file that's created if the job failed.
- **TASKFOREST_UNIQUE_ID** - This is an internal identifier used by TaskForest to refer to the job.
- **TASKFOREST_NUM_RETRIES** - This is the value of the num_retries configuration variable.
- **TASKFOREST_RETRY_SLEEP** - This is the value of the retry_sleep configuration variable.
- **TASKFOREST_EMAIL** - This is the value of the email configuration variable.
- **TASKFOREST_RETRY_EMAIL** - This is the value of the retry_email configuration variable.
- **TASKFOREST_NO_RETRY_EMAIL** - This is the value of the no_retry_email configuration variable.
- **TASKFOREST_INSTRUCTIONS_DIR** - This is the value of the instructions_dir configuration variable.
- **TASKFOREST_MAILGUN_API_KEY** - This is the value of the mailgun_api_key configuration variable.
- **TASKFOREST_MAILGUN_API_KEY** - This is the value of the mailgun_domain configuration variable.
- **TASKFOREST_SMTP_SERVER** - This is the value of the smtp_server configuration variable.
- **TASKFOREST_SMTP_PORT** - This is the value of the smtp_port configuration variable.
- **TASKFOREST_SMTP_SENDER** - This is the value of the smtp_sender configuration variable.
- **TASKFOREST_MAIL_FROM** - This is the value of the mail_from configuration variable.
- **TASKFOREST_MAIL_REPLY_TO** - This is the value of the mail_reply_to configuration variable.
- **TASKFOREST_MAIL_RETURN_PATH** - This is the value of the mail_return_path configuration variable.
- **TASKFOREST_SMTP_TIMEOUT** - This is the value of the smtp_timeout configuration variable.
- **TASKFOREST_RETRY_SUCCESS_EMAIL** - This is the value of the retry_success_email configuration variable.
- **TASKFOREST_NO_RETRY_SUCCESS_EMAIL** - This is the value of the no_retry_success_email configuration variable.
- **TASKFOREST_TWILIO_ACCOUNT_SID** - This is the value of the twilio_account_sid configuration variable.
- **TASKFOREST_TWILIO_AUTH_TOKEN** - This is the value of the twilio_auth_token configuration variable.
- **TASKFOREST_TWILIO_PHONE_NUMBER** - This is the value of the twilio_phone_number configuration variable.

Let `$instructions_dir` refer to the value of the `instructions_dir` option. Let's call the reason for the email `$reason`. It's value is one of `retry`, `fail`, or `retry_success` (as described above). Let's also refer to the job name as `$job_name`, and the family name as `$family_name`. TaskForest will look for the following files in the following order, inserting their contents into the **email** body.

#### `$instructions_dir/HEADER`

This is a header that will show up in every email. In the example that generated the above email, the contents of this file were:

```
This is the TaskForest system at $HOSTNAME
------------------------------------------------------
```

#### `$instructions_dir/HEADER.$reason`

This is a header that will be displayed just for that reason. In other words, it will be one of three files: HEADER.retry, HEADER.fail, and HEADER.retry_success. In the example that generated the above email, the name of this file was HEADER.retry and it's contents were:

```
The following job failed and will be rerun automatically.

Family:         $TASKFOREST_FAMILY_NAME
Job:            $TASKFOREST_JOB_NAME
Exit Code:      $TASKFOREST_RC
Retry After:    $TASKFOREST_RETRY_SLEEP seconds
No. of Retries: $TASKFOREST_RETRY of $TASKFOREST_NUM_RETRIES
```

#### `$instructions_dir/FAMILY.$family_name.$reason`

This is file that can contain specific instructions for that family and reason. In the example that generated the above email, the name of this file was `FAMILY.RETRY.retry` and its contents were:

```
Instructions that apply to all jobs in the Family named RETRY.
```

#### `$instructions_dir/JOB.$job_name.$reason`

This is file that can contain specific instructions for that job and reason. In the example that generated the above email, the name of this file was `JOB.J_Retry.retry` and its contents were:

```
Instructions that apply to the jobs named J_Retry.
```

#### `$instructions_dir/FOOTER.$reason`

This is a footer that will be displayed just for that reason. In other words, it will be one of three files: FOOTER.retry, FOOTER.fail, and FOOTER.retry_success. In the example that generated the above email, the name of this file would have been FOOTER.retry, but that file did not exist, so TaskForest skipped over it.

#### `$instructions_dir/FOOTER`

This is a footer that will show up in every email. In the example that generated the above email, the contents of this file were:

```
------------------------------------------------------------
For help, please see http://www.taskforest.com/
```

#### SMS Text Messages

TaskForest will look for the following files in the following order, inserting their contents into the **SMS text** body.

#### `$instructions_dir/SMS_HEADER`

This is a header that will show up in every SMS text.

#### `$instructions_dir/SMS_HEADER.$reason`

This is a header that will be displayed just for that reason. In other words, it will be one of three files: SMS_HEADER.retry, SMS_HEADER.fail, and SMS_HEADER.retry_success. 

#### `$instructions_dir/SMS_FAMILY.$family_name.$reason`

This is file that can contain specific instructions for that family and reason. 

#### `$instructions_dir/SMS_JOB.$job_name.$reason`

This is file that can contain specific instructions for that job and reason. 

#### `$instructions_dir/SMS_FOOTER.$reason`

This is a footer that will be displayed just for that reason. In other words, it will be one of three files: SMS_FOOTER.retry, SMS_FOOTER.fail, and SMS_FOOTER.retry_success. 

#### `$instructions_dir/SMS_FOOTER`

This is a footer that will show up in every email. 

## Options

The following options are required. If they are not specified on the command line, the environment will be searched for corresponding environment variables or look for them in the configuration file. I recommend that you specify all options in the configuration file.

**--run_wrapper=/a/b/r [or environment variable TF_RUN_WRAPPER]** - 
This is the location of the run wrapper that is used to execute the job files. The run wrapper is also responsible for creating the semaphore files that denote whether a job ran successfully or not. The system comes with two run wrappers: bin/run and bin/run_with_log

The first provides the most basic functionality, while the second also captures the stdout and stderr from the invoked job and saves it to a file in the log directory. You may use either run wrapper. If you need additional functionality, you can create your own run wrapper, as long as it preserves the functionality of the default run_wrapper.

**You are encouraged to use run_with_log** because of the extra functionality available to you. If you also use the included web server to look at the status of today's job, or to browser the logs from earlier days, clicking on a the status of a job that's already run will bring up the log file associated with that job. This is very convenient if you're trying to investigate a job failure.

**--log_dir=/a/b/l [or environment variable TF_LOG_DIR]** - 
This is called the root log directory. Every day a dated directory named in the form YYYYMMDD will be created and the semaphore files will be created in that directory.

**--job_dir=/a/b/j [or environment variable TF_JOB_DIR]** - 
This is the location of all the job files. Each job file should be an executable file (e.g.: a binary file, a shell script, a perl or python script). The file names are used as job names in the family configuration files. Job names may only contain the characters a-z, A-Z, 0-9 and _. You may create aliases to jobs within this directory.

If a job J1 is present in a family config file, any other occurrance of J1 in that family refers TO THAT SAME JOB INSTANCE. It does not mean that the job will be run twice.

If you want the same job running twice, you will have to put it in different families, or make soft links to it and have the soft link(s) in the family file along with the actual file name.

If a job is to run repeatedly every x minutes, you could specify that using the 'repeat/every' syntax shown above.

**--family_dir=/a/b/f [or environment variable TF_FAMILY_DIR]** - 
This is the location of all the family files. As is the case with jobs, family names are the file names. Family names may only contain the characters a-z, A-Z, 0-9 and _.

The following options are optional

**--once_only** - 
If this option is set, the system will check each family once, run any jobs in the Ready state and then exit. This is useful for testing, or if you want to invoke the program via cron or some similar system, or if you just want the program to run on demand, and not run and sleep all day.

**--end_time=HH:MM** - 
If once_only is not set, this option determines when the main program loop should end. This refers to the local time in 24-hour format. By default it is set to 23:55. This is the recommended maximum.

**--wait_time** - 
This is the amount of seconds to sleep at the end of every loop. The default value is 60.

**--verbose** - 
Print a lot of debugging information

**--help** - 
Display help text

**--log** -
Log stdout and stderr to files. Before the logging start, the program will print onto stdout the names of the log file and error file. The program logs incidents at different levels ("debug", "info", "warning", "error" and "fatal"). The log_threshold option sets the level at which logs are written to the stdout file. If the value of log_threshold is "info", then only those log messages with a level of "info" or above ("warning", "error" or "fatal") will be written to the stdout log file. The stderr log file always has logs printed at level "error" or above, as well as anything printed explicitly to STDERR.

**--log_threshold=t** - 
Log messages at level t and above only will be printed to the stdout log file. The default value is "warn".

**--log_file=o** - 
Messages printed to stdout are saved to file o in the log_directory (if --log is set). The default value is "stdout".

**--err_file=e** - 
Messages printed to stderr are saved to file e in the log_directory (if --log is set). The default value is "stderr".

**--config_file=f** - 
Read configuration settings from config file f.

**--chained** - 
If this is set, all recurring jobs will have the chained attribute set to 1 unless specified explicitly in the family file.

**--collapse** - 
If this option is set then the status command will behave as if the --collapse options was specified on the command line.

**--ignore_regex=r** - 
If this option is set then the family files whose names match the perl regular expression r will be ignored. You can specify this option more than once on the command line or in the configuration file, but if you use the environment to set this option, you can only set it to one value. Look at the included configuration file taskforest.cfg for examples.

**--default_time_zone** - 
This is the time zone in which jobs that ran on days in the past will be displayed. When looking at previous days' status, the system has no way of knowing what time zone the job was originally scheduled for. Therefore, the system will choose the time zone denoted by this option. The default value for this option is "America/Chicago".

**token** - 
This is a configuration option that can only be set in the configuration file. A token is a dependency. It is something that a job must 'possess' before it can run, if that job needs that token. You can create different types of tokens, giving each type a common name. You can also specify how many instances of tokens of each type are to exist. The syntax for specifying the different types of tokens and the number of instances each type can have is as follows:

```
...   
<token T>
  number = 1
</token>
<token U>
  number = 2
</token>
...
```

For more details on tokens, please see the tokens documentation above.

**--calendar_dir=/a/b/f** -
This is the location of all the calendar files. As is the case with jobs, calendar names are the file names. Calendar names may only contain the characters a-z, A-Z, 0-9 and _.

**--num_retries=n** -
This is the number of times to automatically retry running a job that fails. The default value is 0.

**--retry_sleep=n** -
Wait these many seconds before automatically retrying running a job that fails. The default value is 60.

**--smtp_server=server_name** -
This is the SMTP server that will be used to send emails out when a job fails, for example

**--smtp_port=p** -
This is the SMTP server port. If not defined it defaults to 25.

**--smtp_timeout=t** -
This is the SMTP timeout in seconds. If not defined it defaults to 60.

**--smtp_sender=s** -
This is the SMTP envelope sender (the text after "MAIL FROM:")

**--mail_from=f** -
This is the email address that appears in the From: mail header

**--mail_reply_to=r** -
If a user replies to a received email, the reply will go to this address instead of the From: address. This address is set in the Reply-To mail header.

**--mail_return_path=p** -
This is the address to which bounces will be sent if they occur at the SMTP server (as opposed to the receiving Mail Transfer Agent).

**--instructions_dir=i** -
This is the directory that stores the contents of the emails that are sent by the system.

**--email=e** -
When a job fails, emails are sent to this address.

**--retry_email=e** -
When a job fails, but is being automatically retried, emails are sent to this address, as opposed to the one stored in the 'email' setting. If no_retry_email is set, then no email will be sent in this case.

**--retry_success_email=e** -
When a job fails, is automatically retried one or more times and then suceeds, emails are sent to this address, as opposed to any of the others. If no_retry_success_email is set, then no email will be sent in this case.

**--no_retry_email=n** -
If this is set to 1, then an email will not be sent when a job fails and is being automatically retried. The default value is 0 (retry emails will be sent).

**--no_retry_success_email=n** -
If this is set to 1, then an email will not be sent when a job fails, was automatically retried one or more times and then finally succeeded. The default value is 0 (retry_success emails will be sent).

**--mailgun_api_key=k** - If you want to use the Mailgun service to send emails (instead of an SMTP server) this should be your secret Mailgun api key.

**--mailgun_domain=d** - This is the Mailgun domain to be used with the Mailgun API key.

**--twilio_account_sid=s** - If you want to use the Twilio service to send an SMS when a job fails this should be your secret Twilio Account SID.

**--twilio_auth_token=t** - This is your Twilio Auth Code.

**--twilio_phone_number=p** - This is your Twilio outbound phone number.

**--phone=p** - When a job fails, SMS texts are sent to this phone number.

**--retry_phone=e** -
When a job fails, but is being automatically retried, SMS texts are sent to this phone number, as opposed to the one stored in the 'phone' setting. If no_retry_phone is set, then no sms will be sent in this case.

**--retry_success_phone=e** -
When a job fails, is automatically retried one or more times and then suceeds, SMS texts are sent to this phone, as opposed to any of the others. If no_retry_success_phone is set, then no text will be sent in this case.

**--no_retry_phone=n** -
If this is set to 1, then an SMS text will not be sent when a job fails and is being automatically retried. The default value is 0 (retry emails will be sent).

**--no_retry_success_phone=n** -
If this is set to 1, then an SMS text will not be sent when a job fails, was automatically retried one or more times and then finally succeeded. The default value is 0 (retry_success texts will be sent).

## Configuration File

The 'taskforest' and 'status' commands accept a ``--config_file=f'' option. You can now specify commonly used options in the config file, so you do not have to include them on the command line. The config file should contain one option per command line. The following sample config file shows the list of all supported options, and documents their usage.


```

# ########################################
# SAMPLE CONFIG FILE
# ########################################
# These are the four required command line
# arguments to taskforest
log_dir          = "t/logs"
family_dir       = "/usr/local/taskforest/families"
job_dir          = "/usr/local/taskforest/jobs"
run_wrapper      = "/usr/local/bin/run_with_log"


# the name of the directory that contains calendar files
calendar_dir    = "/usr/local/taskforest/calendars"

# wait this many seconds between iterations of
# the main loop
wait_time       = 60

# stop taskforest at this time of day
end_time        = 2355

# if set to 1, run taskforest once only and
# exit immediately after that
once_only       = 0

# print out extra logs - may be redundant,
# due to log_threshold, below
# THIS OPTION WILL BE DEPRECATED SOON.
verbose         = 0

# by default assume that the --collapse option
# was given to the status command
collapse        = 1  # change from previously
                     # default behavior

# by default assume that all repeating jobs
# have the --chained=>1 attribute set
chained         = 1  # change from previously
                     # default behavior

# log stdout and stderr to files
log             = 1

# by default, log stdout messages with status >= this value.
# This only effects stdout
# The sequence of thresholds (smallest to largest) is:
# debug, info, warn, error, fatal
log_threshold   = "warn"

# The log_file and err_file names should NOT end with
# '.0' or '.1' because then they will be mistaken for
# job log files
log_file        = "stdout"  
err_file        = "stderr"  

# currently unused
log_status      = 0

# ignore family files whose names match these regexes
ignore_regex    = "~$"
ignore_regex    = ".bak$"
ignore_regex    = '\$'

# There are two types of tokens in this example:
# T - with 1 instance
# U - with 2 instance
<token T>
  number = 1
</token>
<token U>
  number = 2
</token>


# This is the number of times to automatically
# retry running a job that fails 
num_retries              = 1

# Wait these many seconds before automatically
# retrying running a job that fails 
retry_sleep              = 300


# This is the SMTP server that will be used to send 
# emails out when a job fails, for example
smtp_server              = "localhost"

# The default SMTP port is 25.
smtp_port                = 25

# In a production environment this should be 60 or 120
smtp_timeout             = 60

# This is the SMTP envelope sender 
# (the text after "MAIL FROM:")
smtp_sender                = "user1@example.com"

# This is the email address that appears in the From: 
# mail header
mail_from                = "user1@example.com"

# If a user replies to a received email, the reply
# will go to this address instead of the From: address. 
# This address is set in the Reply-To mail header.
mail_reply_to            = "user2@example.com"

# This is the address to which bounces will be sent if 
# they occur at the SMTP server (as opposed to the 
# receiving Mail Transfer Agent).
mail_return_path         = "user3@example.com"

# If you want to use the Mailgun service instead of SMTP 
# to send emails out, for example, when a job fails
# this will be your Mailgun API key
mailgun_api_key         = "key-XXXXX"

# And this will be domain corresponding to tha mailgun domain 
mailgun_domain          = "example.com"

# When a job fails, emails are sent to this address
email                    = "test@example.com"

# When a job fails, but is being automatically retried,
# emails are sent to this address, as opposed to the 
# one stored in the 'email' setting.  If no_retry_email 
# is set, then no email will be sent in this case
retry_email              = "test2@example.com"

# When a job fails, is automatically retried one or more 
# times and then suceeds, emails are sent to this 
# address, as opposed to any of the others.  If 
# no_retry_success_email is set, then no email will be sent
# in this case.
retry_success_email      = "test3@example.com"

# If this is set to 1, then an email will not be sent 
# when a job fails and is being automatically retried.
no_retry_email           = 0

# If this is set to 1, then an email will not be sent 
# when a job fails, was automatically retried one or more 
# times and then finally succeeded.
no_retry_success_email   = 0

# This is the directory that stores the contents of
# the emails and SMS texts that are sent by the system. 
instructions_dir         = "instructions"

# Your Twilio Account SID
twilio_account_sid      = "ACXXXX"

# Your Twilio Auth Token
twilio_auth_token      = "abcdefg"

# Your Twilio Phone Number
twilio_phone_number    = "+13125551212"

# When a job fails, SMS texts are sent to this phone number
phone                  = "+13125551212"

# When a job fails, but is being automatically retried,
# SMS are sent to this address, as opposed to the
# one stored in the 'phone' setting. If no_retry_phone
# is set, then no SMS will be sent in this case
retry_phone = "+13125551213"

# When a job fails, is automatically retried one or more 
# times and then suceeds, SMS are sent to this 
# address, as opposed to any of the others.  If 
# no_retry_success_phone is set, then no SMS will be sent
# in this case.
retry_success_phone      = "+13125551214"

# If this is set to 1, then an SMS will not be sent 
# when a job fails and is being automatically retried.
no_retry_phone           = 0

# If this is set to 1, then an SMS will not be sent 
# when a job fails, was automatically retried one or more 
# times and then finally succeeded.
no_retry_success_phone   = 0

```
