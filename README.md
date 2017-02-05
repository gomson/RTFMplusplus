# RTFM++:  A C++ implementation of the SRP based Real-Time For the Masses (RTFM) kernel

## Design

The RTFM++ is based on the papers by T.P. Baker **"A Stack-Based Resource
Allocation Policy for Realtime Processes"**, Johan Eriksson et. al. **"Real-Time
For the Masses, Step 1: Programming API and Static Priority SRP Kernel
Primitives"** and Per Lindgren et. al. **"Abstract Timers and their Implementation
onto the ARM Cortex-M family of MCUs"**, which utilizes the Nested Vector
Interrupt Controller (NVIC) in Cortex-M processors to implement a Stack
Resource Policy (SRP) based kernel. In this implementation of RTFM, heavy
use of C++ Metaprogramming and C++14 allows the priorities to be automatically
calculated at compile time, while resource locks are handled through RAII. This
minimizes user error without the need for an external extra compile step, as is
currently being investigated in the RTFM-core language (www.rtfm-lang.org).

More description will come...

### Contributors

* Emil Fresk


## Definitions
**Job:**

* A function that runs to completion in a finite time (no forever loop),
 not as a "normal" thread that has a forever loop.
* Has a settable priority which indicates the urgency of the Job.

**Resource:**

* An entity symbolizing something lockable, i.e. any locked Resource may
only be accessed by a single Job at a time.

**Lock (SRP):**

* A lock on a resource keeps other jobs, that will also take said resource,
from running through manipulation of the systems ISR settings. A lock can only
be held within a job and must be released before the exit of a job.

**Lock (PRP):**

* A lock on a resource keeps other jobs, that will also take said resource,
from running through manipulation of the systems ISR settings. In contrary
to the SRP lock, the PRP lock can exist over Job boundaries, this gives the
added advantage of less waits in the system, but the locks are more costly.

---

# TODO

## Kernel primitives needing implementation:

### Job handling:
* `pend(Job)`
    * Request a job for direct execution
* `clear(Job)`
    * Remove pending job request
* `async(Job, ...)`
    * Pend a job after a prescribed time

### Resource handling:

**Stack Resource Policy (SRP):**

* `lock(Resource)`
    * Local lock (within a Job) of a Resource, i.e. the lock must be released
    before the end of the Job.
* `release(Resource)`
    * Release of local lock

**Per Resource Policy (PRP) extensions:**

* `boundary_lock(Resource)`
    * Lock between jobs, i.e. a lock which can live across Job executions.
* `boundary_release(Resource)`
    * Release of lock between jobs.

### Timer handling
* Re-implement the timers/queue for async, details in "Real-Time For the Masses: Abstract Timers and Their Implementation onto the ARM Cortex family of MCUs"