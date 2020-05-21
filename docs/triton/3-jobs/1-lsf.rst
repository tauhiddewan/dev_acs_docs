.. _p-jobs: 

Job Scheduling with LSF
===============================

Triton currently uses the **LSF** resource manager to schedule all
compute resources. LSF (*load sharing facility*) supports over 1500
users and over 200,000 simultaneous job submissions. Jobs are submitted
to **queues**, the software categories we define in the scheduler to
organize work more efficiently. LSF distributes jobs submitted by users
to our over 340 compute nodes according to queue, user priority, and
available resources. You can monitor your job status, queue position,
and progress using :ref:`LSF commands <lsf-commands>`.

.. tip:: **Reserve an appropriate amount of resources through LSF for your jobs.** 

If you do not know the resources your jobs need, use the
**debug** queue to benchmark your jobs. More on :ref:`Pegasus
Queues <p-queues>` and :ref:`LSF Job Scripts <lsf-scripts>` 

.. warning:: Jobs with insufficient resource allocations interfere with cluster performance and the CCS account responsible for those jobs may be suspended (`Policies <https://ccs.miami.edu/ac/policies>`__).

.. tip:: **Stage data for running jobs exclusively in the** ``/scratch`` **file system,** which is optimized for fast data access. 

Any files used as input for your jobs must first be transferred to /scratch. See :ref:`Pegasus
Resource Allocations <projects>` for more information. The
/nethome file system is optimized for mass data storage and is therefore
slower-access. 

.. warning:: Using /nethome while running jobs degrades the performance of the entire system and the CCS account responsible may be suspended*** (`Policies <https://ccs.miami.edu/ac/policies>`__).

.. tip:: **Do not background processes with the** ``&`` **operator in LSF.** 

These spawned processes cannot be killed with **bkill** after the parent is
gone. 

.. warning:: Using the & operator while running jobs degrades the performance of the entire system and the CCS account responsible may be suspended (`Policies <https://ccs.miami.edu/ac/policies>`__).

LSF Batch Jobs
--------------

Batch jobs are self-contained programs that require no intervention to
run. Batch jobs are defined by resource requirements such as how many
cores, how much memory, and how much time they need to complete. These
requirements can be submitted via command line flags or a script file.
Detailed information about LSF commands and example script files can be
found later in this guide.

1. **Create a job scriptfile**

   Include a job name ``-J``, the information LSF needs to allocate
   resources to your job, and names for your output and error files.

   ::

       scriptfile
       #BSUB -J test
       #BSUB -q normal
       #BSUB -P myproject
       #BSUB -o %J.out
       ...

2. **Submit your job to the appropriate project and queue with**
   ``bsub < scriptfile``

   Upon submission, a ``jobID`` and the queue name are returned.

   ::

       [username@triton ~]$ bsub < scriptfile 
       Job <6021006> is submitted to queue <normal>.

3. **Monitor your jobs with** ``bjobs``

   Flags can be used to specify a single job or another user’s jobs.

   ::

       [username@triton ~]$ bjobs
       JOBID  USER   STAT  QUEUE    FROM_HOST  EXEC_HOST   JOB_NAME  SUBMIT_TIME
       4225   usernam   RUN   normal  m1       16*n060     testjob   Mar  2 11:53

4. **Examine job output files**

   Once your job has completed, view output information.

   ::

       [username@triton ~]$ cat test.out
       Sender: LSF System <lsfadmin@n069.triton.edu>
       Subject: Job 6021006: <test> in cluster <triton> Done
       Job <test> was submitted from host <login4.triton.edu> by user <username> in cluster <mk2>.
       Job was executed on host(s) <8*n069>, in queue <normal>, as user <username> in cluster <mk2>.
       ...
