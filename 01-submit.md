---
layout: page
title: Submitting jobs on HPC
subtitle: Submit your first job
minutes: 25
---

> ## Leaerning Objectives {.objectives}
>
> * Learn how to submit a job to a scheduler
> * Learn to see, manage, and cancel submitted jobs
> * Learn that PBS directives can be used to control scheduler's actions

We are going to submit the shell scripts to the scheduler

~~~ {.bash}
$ qsub script.sh
~~~

~~~ {.output}
541678.srih0001.hpc.wvu.edu
~~~

The output is the full jobID.  However, for commands, jobIDs can be shortened 
to the 6 digit number at the front.  In this case, 541678.

> ## jobIDs will differ {.callout}
>
> In this lesson, we will use `541678` for the remainder of the commands 
> showcased.  However, to follow along, your jobID will differ.  You need to 
> use the jobID that is outputted from your `qsub` command.

Show queued or running jobs

~~~ {.bash}
$ showq -u training01
~~~

~~~ {.output}
alkjdfadf
~~~

Show the estimated start time

~~~ {.bash}
$ showstart 541678
~~~

> ## `showstart` is not terribly accurate {.callout}
>
> `showstart` doesn't accurately tell you when a job will start.  Rather, it 
> informs you generally how many jobs might be in front of yours.  `showstart` 
> estimates job start times by assuming all jobs scheduled to run before yours 
> will run for total walltime.  So if showstart tells you your job will start 
> in 5 hours in the standby queue; which has a 4 hour walltime limit.  Then you 
> probably third in line: 1 hour remaining on the job current job running + 4 
> hours for the job in front of yours.  Keep this in mind.

Cancel a running or scheduled job

~~~ {.bash}
$ canceljob 541678
~~~

Checkjob gives a lot of info as well

~~~ {.bash}
$ checkjob 541678
~~~

~~~ {.output}
aldjalfjs
~~~

> ## `checkjob` is only available for 48 hours
>
> `checkjob` information is stored in a database and only available for 48 
> hours from the time of completion (or cancelling) of the job.

## PBS directives


