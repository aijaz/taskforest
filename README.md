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
- check the status of all jobs scheduled to run today
- interact with the included web service using your own client code
- interact with the included web server using your default browser
- express the relationships between jobs using a simple text-based format (a big advantage if you like using 'grep')

### Synopsis

Over the years TaskForest has migrated from a collection of simple perl
modules to a full-fledged system. I have found that putting the
documentation in a single POD is getting much more difficult. You can
now find the latest documetation on the TaskForest website located at [taskforest.com](http://www.taskforest.com/). If you run the included web server, you will also find a complete copy
of the documentation on the included web site.

### Support 

For support, please visit our website at http://www.taskforest.com/ or
create an issue on GitHub

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

- SourceForge
- Rosco Rouse
- Svetlana Lemeshov
- Teresia Arthur
- Steve Hulet
- GitHub

I would also like to thank Randal L. Schwartz for teaching the readers
of the Feb 1999 issue of Web Techniques how to write a pre-forking web
server, the code upon which the TaskForest Web server is built.

I would also like to thank the fine developers at Yahoo! for providing
yui to the open source community.

### License

TaskForest is available under the Apache License. For more information, please see the LICENSE file.

