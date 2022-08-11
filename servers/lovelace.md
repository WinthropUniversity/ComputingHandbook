# The Lovelace Compute Cluster

Lovelace (`lovelace.winthrop.edu`) -- named after [Ada Lovelace](https://en.wikipedia.org/wiki/Ada_Lovelace), the first programmer -- is a Linux-baed compute cluster available for students and faculty to explore parallel and distributed programming projects.  It consists of a login node and 45 compute nodes, each with 16 GB of memory and 4 cores.  In total, it offers 180 CPU cores, and 720 GB of memory for distributed tasks.  A wide variety of software pacakges are installed on the cluster, including multiple implementations of the MPI 4 standard, various compilers and languages, and a variety of linear algebra pacakges.  Currently, it is only available from the campus network.

## How is Lovelace Different from Hopper and Lab Machines?
Unlike our other Linux servers, users will not ssh to individual compute nodes in the Lovelace cluster.  Instead, they will use a *resource manager* to request specific resources (cores, memory, computers) for a specific amount of time that are provisioned to them on the fly.  Such requests can be interactive or batch scheduled.  While you have those resources, other users will not have access to them.

Additionally, though all machines in the Lovelace cluster share the same user space, the cluster does *not* use the same shared drive space as the Hopper server or its labs.  Hopper and the lab machines are intended for students to log directly into and be able to have access to a linux machine (possibly even a desktop) for project work.  Lovelace is intended for compute-intensive tasks, particularly those that run in parallel or are distributed across multiple computers.  It's best to think of Lovelace more like that shared printer on your floor:  You send a printout to it, which queues until the printer is ready and can be printed, then you go a pick up your printout.  In this case:  You setup a compute job (via a script) that specifies exactly what resources your job needs, submit it to a queue, then go get the output when the job is done.


# Accessing Lovelace

Users access Lovelace via the *secure shell* (SSH) protocol from *on campus*.  Currently, Lovelace is not available from outside the campus network.  So, regardless of what platform you are using, you will need at least three things:  

1.	An SSH client;
1.	Winthrop *single signon* (SSO) credentials;
1.	To have been granted access (more on this, below).


### Windows Users

For terminal sessions, Windows users can use [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) `lovelace.winthrop.edu` or [MoabXterm](https://mobaxterm.mobatek.net/)  to connect using the SSH protocol to the host.  For moving files to/from the cluster, Windows users can use [WinSCP](https://winscp.net/), [Filezilla](https://filezilla-project.org/), or [MoabXterm](https://mobaxterm.mobatek.net/).  

When prompted for a username, please use Winthrop SSH username (the username you use for your Winthrop email, Wingspan, etc.).  Likewise, the password will be your Winthrop SSH password.  PuTTY users should know that when you are prompted for your password, you will not see what you are typing -- nor will the cursor advance.  This is a security precaution (so no one over your should can see what you are doing); however, you *are* entering your password.

### Mac/Linux Users

Mac and Linux users have a built in SSH client, most likely [OpenSSN](https://www.openssh.com/) available from any *Terminal* session.  Simply open a terminal and type `ssh YOURUSERNAME@lovelace.winthrop.edu`, Where `YOURUSERNAME` is your Winthrop SSH username (the username you use for your Winthrop email, Wingspan, etc.).  Likewise, the password will be your Winthrop SSH password.  To transfer files, Mac and Linux users can use SCP at the command line from within a Terminal session, like this:

```
scp mylocalfile.txt YOURUSERNAME@lovelace.winthrop.edu:someremotedirectory/.
```

If Mac and Linux users prefer a graphical, drag-and-drop program for transfering files then programs like [Filezilla](https://filezilla-project.org/) are also available.

## Where Am I Once I Log In?

Lovelace is a cluster containing *many* computers.  So you may wonder:  Which computer am I on when I login?  You are on what we call the *user* or *login* node (hostname: `lovelace-user`).  You wont want to do any serious computation on that node because you might be sharing that resouce with many other users.  To access the *compute nodes*, you'll need to use the resource manage.  More on that, below.

## So I Can't Access Lovelace from Home?

The Lovelace cluster is not directly accessible from outside the campus network.  Faculty and staff who have access to a *virtual private networking* (VPN) client can use that to connect into the Winthrop network then access Lovelace from home.  Students do not yet have that capability.  However, you *can* connect to Hopper using SSH and Google Authenticator.  From Hopper, you can SSH over to Lovelace.  So remote access is indirectly possible.  If this becomes a challenge, let Paul Wiegand know.


# Lovelace's Module System

The software you will use on Lovelace will not be "system software" (not the software in `/user`).  Instead, Lovelace maintains a large list of available software specially built to be efficient in parallel / distributed settings.  Because some of these pieces of software can conflict in terms of versions and cross-library compatibility, you will have to *decide* what software you want to use then "load" that software.  To do that, we will use a *module system* called [LMod](readthedocs).

## Seeing What Software is Available

To see a list of what software is available, simply type the following at the command line prompt on Lovelace:

```
module avail
```

You'll see a very long list of confusing looking strings.  For example, one thing you might see is `openblas/0.3.20-gcc-11.2.0-module-i7ai7hu`.  This is the *OpenBLAS library* (a C/Fortran library for doing linear algebra computations) version 0.3.0 that was compiled using the 11.2.0 version of the *GNU Compiler Collection* (gcc).  The module name tells you how that library was built to make it easier to avoid cross-compiler issues.  The strange string at the end is just a 7-character random hash to make sure modules are unique.  If there are multiple libraries available with different hashes, then they probably rely on different underlying libraries.  For example, there are two OpenBLAS libraries, one that depends on OpenMPI and another that depends on MVAPICH2 (these are different implementations of MPI). 

LMod is smart enough to winnow the list using a string.  So, suppose you only want to see modules with the word "python" in them, then the command `module avail python` will give something like the following:

```
   py-cython/0.29.30-python-3.9.13-gcc-11.2.0-module-zadtqg4
   py-docutils/0.18.1-python-3.9.13-gcc-11.2.0-module-ozwglns
   py-wheel/0.37.0-python-2.7.18-gcc-11.2.0-module-fk5edog
   py-wheel/0.37.0-python-3.9.13-gcc-11.2.0-module-mfqhpoi      (D)
   python/2.7.18-gcc-11.2.0-module-6fuhson
   python/3.9.13-gcc-11.2.0-module-z7wwvgl                      (D)
```

## Loading / Unloading a Module

You will load a software module using the command `module load SOFTWARENAME`.  LMod is smart enough to load any dependent modules that may be needed.  Once you load that module, that software will be in path and accessible.  You do not have to type the entire long module name, LMod is smart enough to match the closest substring.  When there are conflicts it will defer to modules that are marked as *default*, those that have the `(D)` designation when you run `module avail`.

You can unload a specific module using the the command `module unload SOFTWARENAME`, and you can unload *all* loaded modules using the command `module purge`.  

An example will make things clearer.  Suppose you want to use *Python*.  Of course, there is a *system level* Python.  Indeed, if you type `which python3` at the command line before loading a module you'll see something like `/usr/bin/python3`; however, you will not want to use this version.  For one, none of the subordinate Python pacakges you may want to use will be installed on the system Python (e.g., numpy).

Instead, you'll want to load one of our python libraries:  `module load python/3`.  Now is you type `which python3` you'll see something like `/thur115/spack/opt/spack/linux-ubuntu22.04-haswell/gcc-11.2.0/python-3.9.13-z7wwvglqlmycvuk5gczrx4ftc4ooutze/bin/python3`.  And if you *run* `python3` you'll run the cluster software stack version.

FYI: You can list all modules that are currently loaded using `module list`.


# The Lovelace Resource Manager

As mentioned above, Lovelace is actually a big cluster of 45 different compute nodes.  It's important to understand how to see what resources are available, request resources, and what jobs are running.  We use a tool called [SLURM](https://slurm.schedmd.com/documentation.html) for this.

## Job Management

To see what computing resources are available, use the command `sinfo`.  For instance, you might see the following:

```
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
normal*      up   infinite     15  drain n[001-005,011-015,021-025]
normal*      up   infinite      4    mix n[101-104]
normal*      up   infinite     26   idle n[105,111-115,121-125,201-205,211-215,221-225]
```

"Partition" is just fancy SLURM-speak for *queue*, and we only have one queue (`normal`).  We are told with this output that several of the nodes are offline (`drain`), several are in mixed use -- meaning that some of their resources are being used but not all (`mix`), and some are available for use (`idle`).

You can see what jobs are running or pending  using the `squeue` command.  We can get a bit more information from that command with the `-l` switch ("long form").  For example, the command `squeue -l` shows that there are two jobs in the queue.  One is running over every node in the cluster, and the other is waiting to run until resources become available.

```
Thu Aug 11 15:41:44 2022
             JOBID PARTITION     NAME     USER    STATE       TIME TIME_LIMI  NODES NODELIST(REASON)
               159    normal SmallJob wiegandr  PENDING       0:00     30:00      1 (Resources)
               158    normal MyLargeJ  thur115  RUNNING       0:20     10:00     45 n[001-005,011-015,021-025,101-105,111-115,121-125,201-205,211-215,221-225]
```

## Direct and Interactive Jobs

You can request resources to run a program using the `srun` command.  That program basically says, "*Go run this command across the resources I request*".  It will run these concurrently on all resources.  For example, the `hostname` command in Linux simply prints the name of the host computer.  If I type `srun --nodes=3 --ntasks-per-node=2 hostname`, the cluster will run the hostname command in six places, twice per computer across three computers.  The result might be something like this:

```
n002
n003
n002
n003
n001
n001
```

Notice that the output did not occur in any particular order.  Parallel / distributed code makes no guarantees as to which tasks complete first.  Two CPUs apiece on three different computers were simultaneously running the `hostname` command, and that just happened to be the order of the output.  If you run it again, the order might be different.

You can use srun to request an *interactive* session.  This means that you are assigned the computer(s) and resources you request to use directly.  This is sort of like ssh'ing to the computer (do not ever connect directly to compute nodes by explicitly using SSH).  To do this, you basically tell srun that you want a terminal session (`--pty`) and you want to run a shell (`bash`).  So the following command asks the resource manager to give you all 4 cores of any available computer for 30 miuntes:  `srun --nodes=1 --ntasks-per-node=4 --time=00:30:00 --pty bash`.  If you issue that command, you'll see your prompt change from something like `thur115@lovelace-user` to something like `thur115@n001`.  You are now *on* a compute node and may use it as you wish -- for 30 minutes.  Type `exit` to stop your job.  If you haven't by the end of your time limit, the resource manager will kick you off the computer and free the resources.  I you want to see what it looks like to get kicked off, type `srun --nodes=1 --ntasks-per-node=4 --time=00:01:00 --pty bash` then wait for a little over a minute.

To learn more about srun, type `srun --help` at the command line.

## Batch Jobs

There are some disadvantages to using srun to execute jobs.  For one, the command will sit and wait until resources are available, execute and give you back results to your terminal.  In the mean time, your terminal is locked up waiting to run.  For large jobs that may require a long time to run or might wait for resources, which is tedious.  For another, it assumes a specific model of computing:  Running the same program multiple times (as opposed to, say, running one program across multiple resources).  Normally when we are running computationally intensive *parallel* or *distributed* code, we'd rather just script how a job should work then *submit* that script to the queuing system.  We get our prompt back to do what we want, and meanwhile the script waits to run until resources are available, then runs logging the output to a file.  We can come back later to get the results when the job is done (like the shared printer example discussed above).  This is the most natural and common way to interact with the compute cluster like Lovelace.  To do so, we'll use a different command: `sbatch`.

To use that command, you first have to write a Linux shell script that executes your job and specifies what resources you need.  You will give the sbatch command the name of that script (I usually just name mine `submit.slurm`, but you can call it anything you like) to submit your job.  For example, suppose you have a program written in C++ called `primes` that uses the MPI library to compute primes in parallel on 12 cores spread across three different computers.  Your submit script might look like this:

```
#!/bin/bash
#SBATCH --nodes=3
#SBATCH --ntasks-per-node=4
#SBATCH --time=00:10:00

# Load the MPI module
module load openmpi

# Run job
mpirun primes
```

If this script is in a file called `submit.slurm` then you would submit this job by typing `sbatch submit.slurm`.  When the job is done, there will be a new file in the same directory as where you submitted called `slurm-JOBID.out`, where `JOBID` is the number assigned to your job

You can learn more about sbatch by typing `sbatch --help`.  Additionally, we have placed a number of examples (in C, C++, and Python) in the `/thur115/examples` directory on Lovelace.

In the old days, using MPI would require that we build a special file called a *machine file* that told MPI what computers a job was to run on.  Our resource manager handles that for you.  If you correctly use SLURM as described, the `mpirun` command will know what computers to run on.  Users shouldn't need to know or care what computer(s) they are using in the cluster.

## Cancelling Jobs

Sometimes, once we submit a job and change our minds.  To stop a job or remove it from the queue (whether it is pending to run or actually executing), we use `scancel`.  We give scancel the job id, for example:  `scancel 165`.  Don't worry, you can't cancel other people's jobs!


## Getting Access

Access to Lovelace (and all Linux machines) is restricted to students and faculty who need access.  Typically access is granted through enrollment as part of a CSCI, DIFD, or MATH course. The current list of linux eligible courses is `'CSCI101','CSCI207','CSCI208','CSCI243','CSCI271','CSCI290','CSCI311','CSCI355','CSCI365','CSCI411','CSCI431','CSCI466','CSCI475','CSCI476','CSCI460', 'CSCI440','DIFD451','MATH370','MATH570'`.  Please contact the chair of the CS department if you feel your course should be added to this list.

Once you are granted access you will maintain that access until you leave the university. If you believe you should have access but do not please contact [helpdesk@winthrop.edu](mailto://helpdesk@winthrop.edu) to request it.  The request will need be approved.
