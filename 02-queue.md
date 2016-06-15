---
layout: page
title: Submitting jobs to HPC systems
subtitle: Selecting queue classes
minutes: 15
---

> ## Learning Objectives {.objectives}
>
> * Learn what queue classes are
> * Learn how to specify queue classes when submitting jobs
> * Learn how to specify resources such as nodes and processors

## What are queue classes

You can see the list of all queues on a cluster using the `qstat` command.

~~~ {.bash}
$ qstat -q
~~~
~~~ {.output}
server: srih0001.hpc.wvu.edu

Queue            Memory CPU Time Walltime Node  Run Que Lm  State
---------------- ------ -------- -------- ----  --- --- --  -----
tdmusho            --      --       --      --    1   2 --   E R
zbetienne          --      --       --      --    1  16 --   E R
spdifazio          --      --       --      --    0   0 --   E R
comm_mmem_week     --      --    168:00:0   --  165  31 --   E R
aelynam            --      --       --      --    0   0 --   E R
genomics_core      --      --       --      --    0   0 --   E R
standby            --      --    04:00:00   --   47  46 --   E R
stmcwilliams       --      --       --      --    0   0 --   E R
bjanderson         --      --       --      --    0   0 --   D S
dsmebane           --      --       --      --    7   0 --   E R
comm_256g_mem      --      --    168:00:0   --    0   1 --   E R
jplewis_large      --      --       --      --   16  18 --   E R
comm_mmem_day      --      --    24:00:00   --    8   5 --   E R
lmshare            --      --       --      --    0   0 --   E R
bvpopp             --      --       --      --    0   0 --   E R
gspirou            --      --       --      --    0   0 --   E R
debug              --      --    01:00:00   --    0   0 --   E R
galaxy             --      --       --      --    0   0 --   E R
hadoop             --      --       --      --    0   0 --   D S
vyakkerman         --      --       --      --    1   0 --   E R
jbmertz            --      --       --      --    6  11 --   E R
comm_gpu           --      --    168:00:0   --    0   0 --   E R
ahismail           --      --       --      --    0   0 --   E R
jshawkins          --      --       --      --    0   0 --   E R
jbmertz_gpu        --      --       --      --    8   0 --   E R
dllong             --      --       --      --    0   0 --   E R
gipiras            --      --       --      --    0   0 --   E R
batch              --      --    04:00:00   --    0   0 --   D S
comm_smp           --      --    168:00:0   --    0   0 --   E R
comm_large_mem     --      --    168:00:0   --    0   0 --   E R
mmshare            --      --       --      --    0   0 --   E R
mcwilliams_gpu     --      --       --      --    0   0 --   E R
jaspeir            --      --       --      --    0   0 --   E R
jplewis            --      --       --      --    8 193 --   E R
testqueue          --      --       --      --    0   0 --   E S
alromero           --      --       --      --    5   0 --   E R
training           --      --       --      --    0   0 --   E R
											   ----- -----
												 273   323
~~~

## Specifying queue classes when submitting jobs

Submit a job to the `standby` queue

~~~ {.bash}
$ qsub -q standby submit-stats.sh
~~~

This automatically sets a default walltime.  


## Specifying compute type resources

Compute type resources include the number of nodes, processors and/or memory.  
Sometimes you will assume the default of all of these (1 node, 1 processor, 4GB 
of RAM), other times you will need to specify a certain number of resources to 
match your workflow.  This is done with the `-l` option.

Request two nodes

~~~ {.bash}
$ qsub -l nodes=2 submit-stats.sh
~~~

> ## Requesting resources and utilization is not synonmous {.callout}
>
> `-l nodes=2` will give you access to two separate nodes, two total cores.  It 
> is important to realize the distinction between your code using two cores and 
> requesting resources in the scheduler.  The qsub command, will only gurantee 
> that nobody else will use the resources you request while your job is 
> running.  However, your submitted job code can utilize a different number of 
> resources.  It is important that the resources utilized by your code, matches 
> the resources requested.  A good general rule is if you do not explicitly 
> indicate in your command/code to use more than one processor or node, it 
> almost always only uses a single processor.

Request 5 processors

~~~ {.bash}
$ qsub -l procs=5 submit-stats.sh
~~~

Request 3 nodes, 2 processors on each node (6 total processors)

~~~ {.bash}
$ qsub -l nodes=3:ppn=2 submit-stats.sh
~~~

Request 15GB of RAM

~~~ {.bash}
$ qsub -l pvmem=15gb submit-stats.sh
~~~

Request 3 nodes, 5 processors a node (15 total), with 11gb of RAM per processor

~~~ {.bash}
$ qsub -l nodes=3:ppn=5,pvmem=11gb submit-stats.sh
~~~


