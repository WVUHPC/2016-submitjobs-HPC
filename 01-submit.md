---
layout: page
title: Submitting jobs to HPC systems
subtitle: Submit your first job
minutes: 30
---

> ## Learning Objectives {.objectives}
>
> * Learn to write a submit script
> * Learn to submit, check, and cancel jobs
> * Learn to add PBS directives to control jobs behavior


Download our intial submit script

[Download](https://wvuhpc.github.com/submitjobs-hpc/script-data.tar.gz)


Copy data to cluster

~~~ {.bash}
$ scp script-data.tar.gz spruce.hpc.wvu.edu:.
~~~

~~~ {.bash}
$ ssh spruce.hpc.wvu.edu
$ tar xzvf script-data.tar.gz
$ cd script-data
~~~

We want to submit the do-stats.sh script to the system.  We write a second 
script that tells the scheduler what to do:

1. Change to the script-data directory
2. Execute the do-stats.sh script

~~~
cd $HOME/script-data
bash do-stats.sh
~~~

Let's call this script `submit-stats.sh`. Notice that these two commands would 
be exactly how you would execute the script normally.  Now submit the job.

~~~ {.bash}
$ qsub submit-stats.sh
~~~
~~~ {.output}
546324.srih001.hpc.wvu.edu
~~~

> ## Your jobID will differ {.callout}
>
> Every job gets a unique jobID....

You can see a list of all your running jobs using `showq`.

~~~ {.bash}
$ showq -u training01
~~~
~~~ {.output}
active jobs------------------------
JOBID              USERNAME      STATE PROCS   REMAINING            STARTTIME

546324             training01  Running     1     1:59:02  Fri Jun 10 15:19:21

1 active job              1 of 384 processors in use by local jobs (0.26%)
						  21 of 32 nodes active      (65.62%)

eligible jobs----------------------
JOBID              USERNAME      STATE PROCS     WCLIMIT            QUEUETIME


0 eligible jobs   

blocked jobs-----------------------
JOBID              USERNAME      STATE PROCS     WCLIMIT            QUEUETIME


0 blocked jobs   

Total job:  1
~~~

> ## username difference {.callout}
>
> Here we use training01..


After a few minutes.  Two new files will appear in the script-data directory.

~~~ {.bash}
$ ls
~~~
~~~ {.output}
do-stats.sh     NENE01812A.txt  NENE02040B.txt        stats-NENE01751B.txt  stats-NENE02040B.txt
goodiff         NENE01843A.txt  NENE02040Z.txt        stats-NENE01812A.txt  stats-NENE02043A.txt
goostats        NENE01843B.txt  NENE02043A.txt        stats-NENE01843A.txt  stats-NENE02043B.txt
NENE01729A.txt  NENE01971Z.txt  NENE02043B.txt        stats-NENE01843B.txt  submit-stats.sh
NENE01729B.txt  NENE01978A.txt  stats-NENE01729A.txt  stats-NENE01978A.txt  submit-stats.sh.e546324
NENE01736A.txt  NENE01978B.txt  stats-NENE01729B.txt  stats-NENE01978B.txt  submit-stats.sh.o546324
NENE01751A.txt  NENE02018B.txt  stats-NENE01736A.txt  stats-NENE02018B.txt
NENE01751B.txt  NENE02040A.txt  stats-NENE01751A.txt  stats-NENE02040A.txt

~~~

Check our output

~~~ {.bash}
$ cat submit-stats.sh.o546324
~~~
~~~ {.output}
...
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
~~~

The 'e' file is for errors.  The 'o' file is for regular output.

We need to include canceling a job.


### Checkjob

~~~ {.bash}
$ checkjob 546324
~~~
~~~ {.output}
job 546324

AName: submit-stats.sh
State: Completed Completion Code: 0  Time: Fri Jun 10 15:35:08
Creds:  user:training01  group:trainingc  class:batch  qos:community
WallTime:   00:01:31 of 2:00:00
SubmitTime: Fri Jun 10 15:33:04
  (Time Queued  Total: 00:00:33  Eligible: 00:00:33)

TemplateSets:  DEFAULT
Total Requested Tasks: 1

Req[0]  TaskCount: 1  Partition: torque
Dedicated Resources Per Task: PROCS: 1  SWAP: 4096M

Allocated Nodes:
[compute-01-25:1]


SystemID:   Moab
SystemJID:  546324
Notification Events: JobFail  Notification Address: training01@theacademy.com

StartCount:     1
Execution Partition:  torque
Flags:          RESTARTABLE
Attr:           checkpoint
StartPriority:  100000
~~~



### Controlling some job behavior

The qsub command has a large number of options that can be given to it to 
control the behavior of the job.  For instance `-N` changes the jobName, and 
subsequently the names of the output files.

~~~ {.bash}
$ qsub -N north-stats submit-stats.sh
~~~

You will now notice that the output files are named north-stats.o546325 and 
north-stats.e546325.  You can also get an e-mail from the system when the job 
completes using the `-M` option.

~~~ {.bash}
$ qsub -M training01@theacademy.com submit-stats.sh
~~~

> ## Mail at different job status {.challenge}
>
> Each job goes through several status changes during it's life cycle.  If a 
> queue job is cancelled before starting the jod is said to be 'aborted' or 
> failed.  Of course, a status change occurs changing from being queued to 
> excuting.  And another status change occurs when the job completes execution 
> (whether successful or not).  You can control when the system sends you an 
> e-mail using the `-m` option.
>
> 1. Submit a job with `-m be -M youremail@server.com` options.  What kind of 
>    e-mails did you recieve from the system?
> 2. Submit a job with `-m n -M youremail@server.com` options.  Did you recieve 
>    any e-mails from the system about this job?

> ## Controlling the name of output files {.challenge}
>
> The output of `submit-stats.sh` produced two files, a `.e` file and a `.o` 
> file.  The name template for these files was the jobName.ejobID or 
> jobName.ojobID.  You can control the names of these output files.
>
> 1. Submit a job with `-o statOutput.txt` option.  What where the names of the 
>    output files?
> 2. Submit a job with `-e statError.txt` option.  What where the names of the 
>    output files?
> 3. Submit a job with `-j oe` option.  How many files where produced?  What 
>    where their names?
> 4. Can you submit a job with a single output file produced and named 
>    singleStream.txt?
> 5. How do you eliminate output files all together?  Hint: `/dev/null` can be 
>    used in place of output files to throw away output.

> ## Canceling a job {.challenge}
>
> The `sleep` command makes a job wait for the number of seconds specified.  
> For instance, `sleep 3600` would make a shell wait 1 hour before continuing.  
> Write a submit script that sleeps for 2 hours.  Submit the job, and verify 
> that it's running using the `showq` command.
>
> 1. Terminate the job using `canceljob jobID` command.  Where any output files 
>    produced?
> 2. Re-submit the job but now with `-m n -M youremail@server.com`.  When you 
>    cancelled the job, did you get any e-mails?
> 3. Can you submit the job, and cancel it without getting any e-mails?  Hint: 
>    trying using nomail@hpc.wvu.edu as your e-mail address.

> ## Placing qsub options inside a submit script {.challenge}
>
> Another way your can specify qsub options is by placing them inside the 
> submit script.  Any option that starts with `#PBS` and appears before 
> commands will be accepted by the system.  Re-submit your sleep command script 
> with `#PBS -M youremail@server.com` placed in the file *after* your sleep 
> command.  Cancel the job, did you get an e-mail?  Now put the `#PBS -M 
> youremail@server.com` *before* the sleep command.  Re-submit and cancel the 
> job.  Did you recieve an e-mail?  Now re-submit the job keeping the `-M` in 
> the submit script.  But place `-M noemail@hpc.wvu.edu` on the command-line in 
> your qsub command.  Did you receive an e-mail?  Use `checkjob` to verify 
> which e-mail address is attributed to the job.
