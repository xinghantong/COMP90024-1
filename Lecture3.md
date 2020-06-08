# Lecture 3
## Compute Scaling
* ### <font color='red'>Veritical Computational Scaling</font>: 
    - Have faster processors: Switch n GHz CPU for a 2n GHz = two times faster
    - Limits of fundemental physics
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
        * Many servers in same rack/server room(Often with fast message passing interconnecrs)
* ### <font color='red'>Widely distributed clusters of machines</font>:
    - <font color='green'>UK NGS, EGEE, etc.</font> Distributed sytstems more generally.
* ### <font color='red'>Hybrid combinations of above</font>:
    * Leads to many challenges with distributed systems.
        * Shared state
        * Message passing paradigms <font color='pink'>(dangers of delayed/lost messages)</font>
## Limitations
* Add more processes
    * If n processors are thrown at a problem. How much faster will it go:
    * Terminologies:
        * *T(1)* = time for serial computation
        * *T(N)* = time for N parallel computations
        * *S(N)* = speed up
        * $$ 
            S(N) = \frac{\partial T(1)}{\partial T(N)}
          $$
        * Proportion of speed up depend on parts of program that can't be parallelized.
## 