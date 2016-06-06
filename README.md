# What is msg.py?
Modflow Scalar Generation for Python

## Target use case
For use in generating new wel.dat input files based on predetermined scalar values.

# Parametric Launcher Usage
-------------------------------------------------------------------------
Parametric Job Launcher: a simple utility for submitting multiple
                         serial applications simultaneously.
-------------------------------------------------------------------------

## To use:

  Copy the example "paramlist" input file to your working directory
  and edit to supply the name of each serial application to
  run per allocated process (these can be the same for each process,
  but there needs to be a separate line specified for each desired task).
  Note that example paramlist file provided is setup
  to run 8 instances of the program named "hello"; the stdin
  for this application is a single integer. Source code in Fortran and
  a pre-built binary is provided for the hello world application which
  can be used to test the batch launcher.

## Available Environment Variables:

  The launcher defines the following environment variables for each job
  that is started:

    * $CONTROL_FLE is the file containing the jobs to run in your parametric
      submission.

    * $TACC_LAUNCHER_NPROCS contains the number of processes running
      simultaneously in your parametric submission.

    * $TACC_LAUNCHER_NHOSTS contains the number of hosts running
      simultaneously in your parametric submission.

    * $TACC_LAUNCHER_PPN contains the number of processes per node.

    * $TACC_LAUNCHER_NJOBS contains the number of jobs in your paramfile.

    * $TACC_LAUNCHER_TSK_ID is the particular processing core that the job is
      running on, from 0 to $TACC_LAUNCHER_NPROCS-1.

    * $TACC_LAUNCHER_JID represents the particular job instance currently
      running. $TACC_LAUNCHER_JID is numbered from 1 to $TACC_LAUNCHER_NJOBS.

  Example: If you want to redirect stdout to a file containing the unique ID
  of each line, you can specify the following in the paramlist file:
    a.out > out.o$TACC_LAUNCHER_JID
  If this particular execution instance of a.out was the first line in the
  paramlist file, the output would be placed in the file "out.o1".

  Note: you can also use the launcher to run a sequence of serial
  jobs when you have more jobs to run than the requested number of
  processors.

## Task Scheduling Behavior:

  The launcher has three available behaviors for scheduling jobs, available
  by setting the environment variable $TACC_LAUNCHER_SCHED:
  (descriptions below assume k = task, p = num. procs, n = num. jobs)

    * interleaved (default) - each task k executes every (k+p)th line
    * block - each task k executes lines [ k(n/p)+1, (k+1)(n/p) ]
    * dynamic - each task k executes first available unclaimed line

## Using the launcher with Intel Xeon Phi cards:

  The launcher has the ability to execute appropriately compiled executables
  natively on Intel Xeon Phi (MIC) cards.

  Available Environment Variables for Intel Xeon Phi execution:

    * $TACC_LAUNCHER_NPHI is the number of Intel Xeon Phi cards per node.
      This is set to zero (0) by default. Acceptable values are '1' and '2'.

    * $TACC_LAUNCHER_PHI_PPN is the number of processes per Intel Xeon Phi card.

    * $PHI_CONTROL_FILE is the file containing the jobs to run on the
      Intel Xeon Phi cards.

## Job Submission:

  Copy the example job submission script "launcher.<sched>" to your
  working directory to use as a starting point for interfacing with
  the desired batch system. Note that this script provides some simple
  error checking prior to the actual submission to aid in diagnosing
  missing executables and misconfiguration.

  The directory containing this README contains several example submission
  scripts:

    SGE:   launcher.sge
    SLURM: launcher.slurm

--
### Last Update: 10/9/2013



