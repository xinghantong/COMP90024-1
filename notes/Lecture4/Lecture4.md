# Lecture 4
## <font color=red>High Performance Computing(HPC)</font>
* <font color=red>High-Performance computing</font> is any computer system whose architecture allows for above average performance. 
* <font color=red>Clustered computing</font> is when two or more computers serve a single resource. This improves performance and provides redundancy; Typically a collection of smaller computers strapped together with a high-speed local network. 
* The clustered HPC is the most efficient, economical, and scalable method. 
## Parallel and Research Programming
* <font color=red>Parallel computing</font> refers to the submission of jobs or processes over multiple processors and by splitting up the data or tasks between them. <font color=pink>(Random number generation as data parallel, driving a vehicle as task parallel)</font>
## HPC Cluster Design
><img src='HPC_Cluster_Design.png' width='50%'>
## Important elements for HPC other than performance
* Correctness of code and signal
* Clarity of code and architecture
* Reliability of code and equipment
* Modularity of code and components
* Readability of code and hardware documentation
* Compatibility of code and hardware
- Common rule in parallel programming: <font color=pink>Develop a working serial version of code first and then work out what parts can be made parallel</font>
## Spartan Instructions
### Module Commands
```help```: Provides a list of the switches, subcommands, and subcommand arguments that are available through the environment modules package
```
module help
```
```avail```: Lists all the modules which are available to be loaded
```
module avail
```
```whatis```: Provides a description of the module listed
```
module whatis <modulefile>
```
```display```: Use this command to see exactly what a given modulefile will do to your environment
```
module display <modulefile>
```
```load```: Adds one or more modulefiles to the user's current environment
``` 
module load <modulefile>
```
```unload```: Removes any listed modules from the user's current environment
```
module unload <modulefile>
```
```switch```: Unload one modulefile and loads another.
```
module switch <modulefile1> <modulefile2>
```
```purge```: Removes all modules from the user's environment
```
module purge
```
### Submitting and Running Jobs
1. Setup and launch consists of writing a short script that initially makes resource requests and then commands, and optionally checking queueing system
>Core command for checking queue:
>```
>squeue | less
>```
> Alternative command for checking queue:
>```
>showq -p cloud | less
>```
>Core command for job submission:
>```
>sbatch[jobscript]
>```
>    
2. Check job status
>Core command for checking job in Slurm:
>```
>squeue -j [jobid]
>```
>Detailed command in Slurm:
>```
>scontrol show job [jobid]
>```
>Core command for deleting job in Slurm:
>```
>scancel [jobid]
>```
3. Slurm provides an error and output files.
### Example of submission script
```bash
#!/bin/bash
#SBATCH --partition=cloud
#SBATCH --time=01:00:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
module load my-app-complier/version
my-app data
```
After the script is written it can be submitted to the scheduler.
```bash
sbatch [filename].slurm
```
### Job Arrays
* Job arrays allow the same batch script and the same resources requests, and lauches multiple jobs simultaneouly. 
* E.g. Submits 10 batch jobs:
```bash
#SBATCH --array=1-10
myapp ${SLURM_ARRAY_TASK_ID}.csv
```
### Dependencies: Job Pipelines
* A dependency condition is established on which the launching of a batch script depends, creating a conditional pipeline.
* E.g.
```
#SBATCH --dependency=afterok:myfirstjobid mysecondjob
```
* Dependency types
    * ```after:jobid[:jobid]```: job can begin after the specified jobs have started
    * ``` afterany:jobid[:jobid]```: job can begin after the specified jobs have terminated
    * ```afternotok:jobid[:jobid]```: job can begin after the specified jobs have failed
    * ```afterok:jobid[:jobid]```: job can begin after the specified job have run to completion with an exit code of zero
    * ```singleton```: job can begin execution after all previously lauched jobs with the same name and user have ended
## Shared Memory Parallel Programming
* One form of parallel programming is <font color=red>multipthreading</font>, whereby a master thread forks a number of sub-threads and divides tasks between them. The threads will then run concurrently and are then joined at a subsequent point to resume normal serial application. 
* One implementation of multithreading is <font color=red>OpenMP(Open Multiple Threading)</font>. It is an Application Program Interface that includes directives for multi-threading, shared memory parallel programming. 
> Example for OpenMP:
>> <img src='OpenMP.png' width=50%>
Example for Shared Memory Parallel Programming
```c
#include <stdio.h> 
#include "omp.h" int main(void)
{
    int id;
    #pragma omp parallel num_threads(8) private(id) 
    {
    int id = omp_get_thread_num();
    printf("Hello world %d\n", id);
    }
return 0; 
}

program hello2omp
    include "omp_lib.h"
    integer :: id
    !$omp parallel num_threads(8) private(id)
        id = omp_get_thread_num() print *, "Hello world", id
    !$omp end parallel
end program hello2omp
```
## Distributed Memory Parallel Programming
* Moving from shared memory to parallel programming involves a conceptual change from multi-threaded programming to a message passing paradigm. In this case, <font color=red>MPI(Message Passing Interface)</font> is one of the most popular standards, along with a popular implementation as <font color=red>OpenMPI</font>.
* The core principle is that many processors should be able to cooperate to solve a problem by passing messages to each through a common communication networks.
* The programmer is responsible for identifying opportunities for parallelism and implementing algorithms for parallelization using MPI.
> Example for MPI:
>> <img src='MPI.png' width=50%>
```c
#include <stdio.h> 
#include "mpi.h"
int main( argc, argv ) 
int argc;
char **argv;
{
    int rank, size;
    MPI_Init( &argc, &argv );
    MPI_Comm_size( MPI_COMM_WORLD, &size ); MPI_Comm_rank( MPI_COMM_WORLD, &rank );
    printf( "Hello world from process %d of %d\n", rank, size ); 
    MPI_Finalize();
    return 0;
}
!   Fortran MPI Hello World 
    program hello
    include 'mpif.h'
    integer rank, size, ierror, tag, status(MPI_STATUS_SIZE)

    call MPI_INIT(ierror)
    call MPI_COMM_SIZE(MPI_COMM_WORLD, size, ierror) call MPI_COMM_RANK(MPI_COMM_WORLD, rank, ierror) 
    print*, 'node', rank, ': Hello world'
    call MPI_FINALIZE(ierror)
    end
```
# MPI Workshop
## MPI Programming Basics
* Many parallel programs can be written using just these six functions:
    * ```MPI_INIT```
    * ```MPI_FINALIZE```
    * ```MPI_COMM_SIZE```
    * ```MPI_COMM_RANK```
    * ```MPI_SEND```
    * ```MPI__RECV```
* ```MPI_SEND``` and ```MPI_RECV``` function can be substituted with collective operations such as ```MPI_BCAST``` and ```MPI_REDUCE```
### Collective Operations in MPI
* ```MPI_BCAST```: Distributed data from one process (the root) to all others in a communicator
* ```MPI_REDUCE```: Combines data from all processes in communicator and returns it to one process
## Example of MPI4Py
```python
from mpi4py import MPI
import sys

size = MPI.COMM_WORLD.Get_size()
rank = MPI.COMM_WORLD.Get_rank()
print("Helloworld! I am process %d of %d.\n"%(rank, size))
```
