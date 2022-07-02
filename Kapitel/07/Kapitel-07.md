## 3 Scheduler (Kaptiel 7)

### 3.1 Scheduling Metriken

$$T_{turnaround}=T_{completion} - T_{arrival}$$

* $T_{turnaround}$ ist die Dauer bis ein Prozess fertig gelaufen ist.

* $T_{completion}$ ist die Zeit an welcher ein Prozess fertig geworden ist.

* $T_{arrival}$ ist die Ankuftszeit eines Prozesses.

Die Turnaroundzeit ist eine Performance Metrik und kann oft mit der Fairnessmetrik in Konflikt stehen.

$$T_{responce}=T_{firstrun} - T_{arrival}$$

* $T_{responce}$ ist die Zeit bis ein Prozess zum ersten mal gescheduled wird. 

* $T_{firstrun}$ ist die Zeit wenn der Prozess das erste mal gescheduled wird.

* $T_{arrival}$ ist die Ankuftszeit eines Prozesses.


### 3.2 Algorithmen

#### 3.2.1 First In, First Out (FIFO)

Wer zu erst kommt, wird zuerst bedient. Das hat den Nachteil, das sehr kurze Prozesse auf einen langen warten müssen. Nennt man auch den **convoy effect**.

#### 3.2.2 Shortest Job First (SJF)

Die kürzesten Prozesse laufen zuerst, das erhöt die Responce Time aber ist schlecht für die Reaktionszeit.

#### 3.2.3 Shortest Time to Completion First (STCF)

Hier können Prozesse auch unterbrochen werden. Der Job mit der noch kürzesten laufzeit kommt zuerst drann.

#### 3.2.4 Round Robin (RR)

Für eine festgelegte Zeit (auch **time-slice** genannt), wird jeder Prozess gescheduled. Die Prozess wechseln sich nach dieser Zeit ab. Der Vorteil ist das Die Reaktionszeit jetzt gleich der festgelegten Zeit ist.


### 3.3 Preemptive vs. Non-Preemptive

Non-Preemptive Scheduler waren eher in alten Systemen gängig. Sie lassen jeden Prozess laufen bis er fertig ist. Preemptive Scheduler werden in nahezu jedem modernen OS verwendet. Hier können Prozesse während sie laufen auch unterbrochen werden.

### Homework: Scheduler

Der Simulator "scheduler.py" erlaubt es einem verschiedene Scheduling Algorithmen auszuprobieren. 

```bash
arnold% ./scheduler.py -s 0 -c
ARG policy FIFO
ARG jobs 3
ARG maxlen 10
ARG seed 0

Here is the job list, with the run time of each job: 
  Job 0 ( length = 9 )
  Job 1 ( length = 8 )
  Job 2 ( length = 5 )


** Solutions **

Execution trace:
  [ time   0 ] Run job 0 for 9.00 secs ( DONE at 9.00 )
  [ time   9 ] Run job 1 for 8.00 secs ( DONE at 17.00 )
  [ time  17 ] Run job 2 for 5.00 secs ( DONE at 22.00 )

Final statistics:
  Job   0 -- Response: 0.00  Turnaround 9.00  Wait 0.00
  Job   1 -- Response: 9.00  Turnaround 17.00  Wait 9.00
  Job   2 -- Response: 17.00  Turnaround 22.00  Wait 17.00

  Average -- Response: 8.67  Turnaround 16.00  Wait 8.67
```


