# Lecture 3
## Compute Scaling
* ### <font color='red'>Vertical Computational Scaling</font>: 
    - Have faster processors: Switch n GHz CPU for a 2n GHz = two times faster
    - Limits of fundamental physics
* ### <font color='red'>Horizontal Computational Scaling</font>:
    - Have more processors:
        * Easy to add more; Cost increase.
        * Harder to design, develop, test, debug, deploy and understand.
* ### <font color='red'>Single machine multiple cores</font>:
    - Typical laptop/PC/servers
* ### <font color='red'>Loosely coupled collection/cluster of machines</font>:
    - Pooling/sharing of resources
        * Dedicated vs available only when not in use by others.
        * Web services: <font color='green'>Condor, Seti@home, Boinc</font>
* ### <font color='red'>Tightly coupled cluster of machines</font>:
    - Typical HPC/HTC set-up
        * Many servers in same rack/server room(Often with fast message passing interconnects)
* ### <font color='red'>Widely distributed clusters of machines</font>:
    - <font color='green'>UK NGS, EGEE, etc.</font> Distributed systems more generally.
* ### <font color='red'>Hybrid combinations of above</font>:
    * Leads to many challenges with distributed systems.
        * Shared state
        * Message passing paradigms <font color='pink'>(dangers of delayed/lost messages)</font>
## Limitations
* ### Add more processes
    * If n processors are thrown at a problem. How much faster will it go:
    * Terminologies:
        * *T(1)* = time for serial computation
        * *T(N)* = time for N parallel computations
        * *S(N)* = speed up
        > $$ S(N) = \frac{T(1)}{T(N)}$$
        * Proportion of speed up depend on parts of program that can't be parallelized.
## <font color='blue'>*Amdahl's Law*:</font>
* 
>$$ T(1) = \sigma + \pi$$
>$$ T(N) = \sigma + \frac{\pi}{N}$$
>$$ S = \frac{T(1)}{T(N)} = \frac{\sigma+\pi}{1+(\frac{\pi}{\sigma})*(\frac{1}{N})}$$
>$$ \frac{\pi}{\sigma} = \frac{1-\alpha}{\alpha} $$
>$$ where \ \alpha \ is \ fraction \ of \ running \ time \ that \ sequential \ program \ spends \ on \ $$ 
>$$non-parallel \ parts \ of \ a \ computation \ approximates \ to \ S = \frac{1}{\alpha}$$
>$$ Therefore \ S = \frac{1+\frac{(1-\alpha)}{\alpha}}{1+\frac{(1-\alpha)}{N\alpha}} = \frac{1}{\alpha + \frac{(1-\alpha)}{N}} \thickapprox \frac{1}{\alpha}$$
* E.g. If 95% of the program can be parallelized, the theoretical maximum speedup using parallel computing would be <font color='red'> 20 times</font>, no matter how many processors are used.     
    * <font color='pink'>If the non-parallelizable part takes 1 hour, then no matter how many cores you throw at it, it won't complete in less than 1 hour.</font>
### Over Simplifications of *Amdahl's Law*
* Consider a program that executes a single loop, where all iterations cna be computed independently. <font color='pink'> i.e. code can be parallelized by splitting the loop into several parts. </font>The overhead is replicated as many times as there are are processors. In effect, loop overhead acts as a further overhead in running the code. Also getting data to/from many processors overheads
    * <font color='red'>In a nutshell *Amdahl's Law* greatly simplifies the real world</font>
* It also assumes a fixed problem size - sometimes can't predict length of time required of jobs.
## <font color='blue'> *Gustafson-Barsis's Law*</font>
* Gives the 'scaled speed-up':
> $$ T(1) = \sigma + N \pi \quad T(N) = \sigma + \pi $$
> $$ S(N) = \frac{T(1)}{T(N)} = \frac{\sigma + N\pi}{\sigma + \pi} = \frac{\sigma}{\sigma + \pi} + \frac{N\pi}{\sigma + \pi}$$
> $$ Where \ \pi \ is \ fixed \ parallel \ time \ per \ process$$
> $$ And \ \alpha \ is \ fraction \ of \ running \ time \ sequential \ program \ spends \ on \ parallel \ parts$$
> $$ So: \ \frac{\pi}{\sigma} = \frac{1-\alpha}{\alpha}$$
> $$ Therefore: \ S(N) = \alpha + N(1-\alpha) = N - \alpha(N-1)$$
* E.g. Speed up *S* using *N* processes is given as a linear formula dependent on the number of processes and the fraction of time to urn sequential parts. *Gustafson's Law* processes that programmer tend to set the size of problems to use the available equipment to solve problems within a practical fixed time. Faster (more parallel) equipment available, larger problems can be solved in the same time. 
## Computer Architecture
* A computer comprises:
    * CPU for executing programs
        * ALU, FPU
        * Load/store unit
        * Registers <font color='pink'>(fast memory locations)</font>
        * Program counter <font color='pink'>(address of instruction that is executing)</font>
        * Memory interface
    * Memory that stores/executing programs and related data
    * I/O systems <font color='pink'>(keyboards, networks, etc)</font>
    * Permanent storage for read/writing data into out of memory
    * Of key importance is the balance of all of these:
        * Superfast CPU's starved of data
        * Jobs killing clusters
    * There are many different ways to design/architect computers
* ### <font color='blue'>*Flynn's Taxonomy*</font>
    * #### <font color='red'>Single Instruction, Single Data Stream(SISD)</font>
        * Sequential computer which exploits no parallelism in either the instruction or data streams
        * Single control unit (CU/CPU) fetches single Instruction Stream from memory,. The CU/CPU then generates appropriate control signals to direct to direct single processing element to operate on single Stream. <font color='pink'>i.e. one operation at a time.</font>
        * Obsolete
        * E.g. Example architecture of SISD
            ><img src='SISD.png' width='50%'>
    * #### <font color='red'>Multiple Instruction, Single Data Stream(MISD)</font>
        * Parallel computing architecture where many functional units(PU/CPU) perform different operations on the same data.
        * Examples include fault checking computer architectures. <font color='pink'>E.g. Running multiples error checking processes on same data stream.</font>
        * E.g. Example architecture of MISD
            ><img src='MISD.png' width='50%'>
    * #### <font color='red'>Single Instruction, Multiple Data Stream(SIMD)</font>
        * Multiple processing elements that perform the same operation on multiple data points simultaneously.
        * Focus is on data level parallelism. <font color='pink'>i.e. Many parallel computations, but only a single process at a given moment.</font>
        * Many modern computers use *SIMD* instructions. <font color='pink'>E.g. To improve performance of multimedia use such as for image processing.</font>
        * E.g. Example architecture for SIMD
            ><img src='SIMD.png' width='50%'>
    * #### <font color='red'>Multiple Instruction, Multiple Data Stream(MIMD)</font>
        * Number of processors that function asynchronously and independently.
        * At any time, different processors may be executing different instruction on different pieces of data.
        * Machines can be shared memory or distributed memory categories.
            * Depends on how *MIMD* processors access memory.
        * Most systems these days operate on *MIMD*. <font color='pink'>E.g. HPC</font>
        * E.g. Example architecture of MIMD
            ><img src='MIMD.png' width='50%'>
        
