# TaskForest
---
[![Build Status](https://travis-ci.org/aijaz/taskforest.svg?branch=master)](https://travis-ci.org/aijaz/taskforest)

**TaskForest** is a simple but expressive job scheduler that allows you to
chain jobs/tasks and create time dependencies. It uses text config files to
specify task dependencies.

## Executive Summary 

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

## Quick Start

The following quick start assumes that: 

- you have `root` access
- you want to install the necessary files in `/usr/local/taskforest`
- you have write access to `/usr/local/taskforest`

Make sure you have the following Perl modules installed (available from [CPAN](http://cpan.org/)):

- [DateTime][]
- [Config::General][] version 2.38 or higher
- [Log::Log4perl][] version 1.16 or higher
- [HTTP::Daemon][] version 5.818 or higher
- [HTTP::Status][] version 5.817 or higher
- [HTTP::Response][] version 5.820 or higher
- [HTTP::Request][] version 5.818 or higher
- [Net::SMTP][] version 2.31 or higher

### 1. Install TaskForest

Fork or clone the repository. In the `master` branch enter: 

```
$ perl Makefile.PL
$ make
$ make test # this will take a few minutes
$ sudo make install

$ mkdir /usr/local/taskforest
$ mkdir /usr/local/taskforest/jobs
$ mkdir /usr/local/taskforest/families
$ mkdir /usr/local/taskforest/logs
```

### 2. Create a Config File

Save the following text into `/usr/local/taskforest/taskforest.cfg`:

```python
log_dir     = "/usr/local/taskforest/logs"
family_dir  = "/usr/local/taskforest/families"
job_dir     = "/usr/local/taskforest/jobs"
run_wrapper = "/usr/local/bin/run_with_log"
```

### 3. Create Job Files

When TaskForest runs a job, it executes a Job File. A Job File is a script or executable file that has the 'execute' (*x*) bit set. Let's create some very simple job files: one that always completes successfully, and one that always fails:

```bash
cd /usr/local/taskforeset/jobs

echo "#!/bin/bash" > ROTATE_LOGS
echo "exit 0" >> ROTATE_LOGS

cp ROTATE_LOGS RESOLVE_DNS
cp ROTATE_LOGS DELETE_OLD_LOGS
cp ROTATE_LOGS GENERATE_REPORTS
cp ROTATE_LOGS EMAIL_REPORTS
cp ROTATE_LOGS UPDATE_ACCOUNTS_RECEIVABLE
cp ROTATE_LOGS PROCESS_PENDING_CHARGES
cp ROTATE_LOGS BACKUP_DATABASE
cp ROTATE_LOGS SAVE_BACKUP_OFFSITE

chmod a+x *
```

### 4. Create Family Files

Jobs are grouped into Families. A *Family* has a name, a start time, a time zone, and a list of days of the week when it may run (or a calendar, but you don't know about that, yet). Let's create a Family that starts at 9:00 a.m. Chicago time three days a week. Save the following text into '/usr/local/taskforest/families/WEEKDAYS':

```
start => '09:00', tz => 'America/Chicago', days => 'Mon,Wed,Fri'

# Everything after the '#' character is ignored.
# As are blank lines

                ROTATE_LOGS()

  RESOLVE_DNS()            DELETE_OLD_LOGS()

                GENERATE_REPORTS()

                EMAIL_REPORTS(start => '13:00')

--------------------------------------------------------------

  UPDATE_ACCOUNTS_RECEIVABLE()

  PROCESS_PENDING_CHARGES()

--------------------------------------------------------------

  BACKUP_DATABASE()

  SAVE_BACKUP_OFFSITE(num_retries => 10, retry_sleep => 600)
```

Have a look at this file. There are a few things I want to tell you about it:

- There are three job groups, (separated by lines of dashes)
- The job groups run independent of each other.
- At 9:00 a.m. the following jobs will run at the same time (since they're the first in their respective groups): 
    + ROTATE_LOGS
    + UPDATE_ACCOUNTS_RECEIVABLE
    + BACKUP_DATABASE
- When ROTATE_LOGS completes successfully, RESOLVE_DNS and DELETE_OLD_LOGS will both run simultaneously, since they're on the same line.
- When both these jobs complete successfully, GENERATE_REPORTS will be run
- When GENERATE_REPORTS completes successfully, EMAIL_REPORTS will run. But not before 1:00 p.m. TaskForest will wait until 1:00 p.m. to run the job, as long as all its other dependencies are met
- Once UPDATE_ACCOUNTS_RECEIVABLE completes successfully, PROCESS_PENDING_CHARGES will be run
- If BACKUP_DATABASE fails, SAVE_BACKUP_OFFSITE will not run
- If SAVE_BACKUP_OFFSITE fails, it will be retried 10 additional times, with a 10 minute (600 second) wait between retries

### 5. Schedule TaskForest to run every day

There are many ways to run TaskForest. The simplest is to start it every day at a fixed time using `cron`:

```
05 0 * * * /usr/local/bin/taskforest --config_file=/usr/local/taskforest/taskforest.cfg
```

In the above example, TaskForest will start every day at 00:05 (12:05 a.m.). By default, TaskForest stops running every day at 23:55 (11:55 p.m.).  

### 6. Check Up on the Status of all the Jobs

To see the status of all jobs do the following: 

```
$ export TF_LOG_DIR=/usr/local/taskforest/logs
$ status
```

## Where to Go From Here

Now that you know how to get started with TaskForest, you can browse the [TaskForest][] documentation.

[DateTime]: http://search.cpan.org/search?mode=all&query=DateTime
[Config::General]: http://search.cpan.org/search?mode=all&query=Config%3A%3AGeneral
[Log::Log4perl]: http://search.cpan.org/search?mode=all&query=Log%3A%3ALog4perl
[HTTP::Daemon]: http://search.cpan.org/search?mode=all&query=HTTP%3A%3ADaemon
[HTTP::Status]: http://search.cpan.org/search?mode=all&query=HTTP%3A%3AStatus
[HTTP::Response]: http://search.cpan.org/search?mode=all&query=HTTP%3A%3AResponse
[HTTP::Request]: http://search.cpan.org/search?mode=all&query=HTTP%3A%3ARequest
[Net::SMTP]: http://search.cpan.org/search?mode=all&query=Net%3A%3ASMTP
[TaskForest]: http://taskforest.com
