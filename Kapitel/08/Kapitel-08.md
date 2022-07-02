## 4 Multi-Level Feedback Queue (MLFQ) (Kapitel 8)

### 4.1 Warum MLFQ?

Das erste Ziel der MLFQ ist, die Turnaround Zeit zu reduzieren. Das geschieht in dem man kürzere Prozesse zuerst laufen lässt. Das zweite Ziel ist die Reaktionszeit zu verringern.

### 4.2 Gaming the Scheduler

Wenn ein Programm kurz vor ende seiner time slice einen I/O request stellt bbleibt seine Priorität die selbe und der Prozess kann so die CPU Zeit für sich beanspruchen da er nicht seinen ganzen time-slice aufgebraucht hat. Dies kann umgangen werden in dem "Rule 4" angewandt wird.

### 4.3 Starvation

Lange jobs verhungern in der MLFQ weil ihre priorität stetig sinkt. Dies kann verhindert werden in dem man alle Prozesse periodisch auf eine höhere Priorät boostet (Rule 5).

### 4.4 MLFQ Rules

* **Rule 1:** If Priority(A) > Priority(B), A runs (B doesnt)

* **Rule 2:** If Priority(A) = Priority(B), A& B run in round-robin fashion suing the time slice (quantum length) of the given queue

* **Rule 3:** When a job enters the system, it is placed at the highest priority (the tompomst queue)

* **Rule 4:** Once a job uses up its tim e allotment at a given level (regardless of how many times it has given up the CPU), its priority is reduced (i.e., it moves down one queue.)

* **Rule 5:** After some time period S, move all the jobs in the system to the topmost queue.

### 4.5 Homework

Der Simulator mlfq.py demonstriert das verhalten des mlfq Schedulers.


* Erstelle einen Workload mit 2 Jobs so dass ein Prozess 99% der CPU Zeit bekommt

```bash
python2 mlfq.py -l 0,200,19:0,200,0 -q 20 -n 2 -i 1 -S -c
```

Basic Output looks like this

```bash
prompt> ./mlfq.py -j 3

What you would then see is the specific problem definition:

Here is the list of inputs:
OPTIONS jobs 3
OPTIONS queues 3
OPTIONS quantum length for queue  2 is  10
OPTIONS quantum length for queue  1 is  10
OPTIONS quantum length for queue  0 is  10
OPTIONS boost 0
OPTIONS ioTime 0
OPTIONS stayAfterIO False


For each job, three defining characteristics are given:
  startTime : at what time does the job enter the system
  runTime   : the total CPU time needed by the job to finish
  ioFreq    : every ioFreq time units, the job issues an I/O
              (the I/O takes ioTime units to complete)

Job List:
  Job  0: startTime   0 - runTime  84 - ioFreq   7
  Job  1: startTime   0 - runTime  42 - ioFreq   2
  Job  2: startTime   0 - runTime  51 - ioFreq   4


Execution Trace:

[ time 0 ] JOB BEGINS by JOB 0
[ time 0 ] JOB BEGINS by JOB 1
[ time 0 ] JOB BEGINS by JOB 2
[ time 0 ] Run JOB 0 at PRIORITY 2 [ TICKS 9 ALLOT 1 TIME 83 (of 84) ]
[ time 1 ] Run JOB 0 at PRIORITY 2 [ TICKS 8 ALLOT 1 TIME 82 (of 84) ]
[ time 2 ] Run JOB 0 at PRIORITY 2 [ TICKS 7 ALLOT 1 TIME 81 (of 84) ]
[ time 3 ] Run JOB 0 at PRIORITY 2 [ TICKS 6 ALLOT 1 TIME 80 (of 84) ]
[ time 4 ] Run JOB 0 at PRIORITY 2 [ TICKS 5 ALLOT 1 TIME 79 (of 84) ]
[ time 5 ] Run JOB 0 at PRIORITY 2 [ TICKS 4 ALLOT 1 TIME 78 (of 84) ]
[ time 6 ] Run JOB 0 at PRIORITY 2 [ TICKS 3 ALLOT 1 TIME 77 (of 84) ]
[ time 7 ] IO_START by JOB 0
IO DONE
[ time 7 ] Run JOB 1 at PRIORITY 2 [ TICKS 9 ALLOT 1 TIME 41 (of 42) ]
[ time 8 ] Run JOB 1 at PRIORITY 2 [ TICKS 8 ALLOT 1 TIME 40 (of 42) ]
[ time 9 ] Run JOB 1 at PRIORITY 2 [ TICKS 7 ALLOT 1 TIME 39 (of 42) ]
...

Final statistics:
  Job  0: startTime   0 - response   0 - turnaround 175
  Job  1: startTime   0 - response   7 - turnaround 191
  Job  2: startTime   0 - response   9 - turnaround 168

  Avg  2: startTime n/a - response 5.33 - turnaround 178.00

  ```


