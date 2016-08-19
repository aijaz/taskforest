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

### Why Text Files are a Good Idea

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

The above does not apply to the files avilable on the website under the ```/yui``` url, which is licensed under the [BSD License](https://github.com/yui/yui3/blob/master/LICENSE.md).

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

## Documentation

