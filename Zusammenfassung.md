---
title: "BSYS Zusammenfassung für die Klausur"
subtitle: "Buch und Übungen"
author: Timothy Hale
date: 15.06.2022	
lang: de
---

\maketitle
\thispagestyle{empty}
\clearpage
\tableofcontents
\pagenumbering{roman}
\clearpage
\pagenumbering{arabic}
\setcounter{page}{1}

## 1 Einleitung Betriebssysteme (Kapitel 2)

The Crux of the Problem: Wie und warum virtualisiert das Betriebssystem seine Ressourcen.

### 1.1 von Neumann

Das von Neumann Modell beschreibt die Grundlagen eines Computersystems. Ein Prozessor holt sich Instruktionen vom Speicher, entschlüsselt sie und führt sie anschliessend aus. Wenn er fertig ist geht der Prozessor zur nächsten Instruktion, bis das Programm Fertig ist.

### 1.2 Was macht ein Betriebssystem?

Das Ziel ist es das System so Benutzerfreundlich wie möglich zu machen. Das mehrere Programme gleichzeitg laufen können, Programme Speicher teilen können und diese auch mit anderen Geräten interagieren können. Dieses Konstrukt nennt man Betriebssystem. Der hauptsächliche Weg wie das OS das umsetzt wird virtualisierung genannt. Da es die Ressourcen (CPU, Speicher etc.) verwaltet, ist das OS auch als Ressourcen Manager bekannt.

### 1.3 Virtualisierung

Das OS nimmt die physikalische Ressource wie beispielsweise den Prozessor, Speicher oder Laufwerke und transformiert sie in eine generische, leichter nutzbare virtuelle Form. Deswegen wird das OS manchmal auch virtuelle Maschine genannt.

### 1.4 Standard Bibliothek

Damit Benutzer dem OS Anweisungen geben können, stellt es interfaces (APIs) zur verfügung. Es exportiert hunderte von "system calls". Die Ansammlung dieser calls wird auch Die Standard Biliothek genannt.

### 1.5 System Calls

Der Unterschied zwischen einer Prozdedur und einem System Call ist, das letzteres die Kontrolle an das OS weitergibt und das hardware privilege level auf Kernel mode ändert. Normale Programme laufen nur mit User Mode Berechtigung. System calls werden durch eine spezielle hardware Instruktion names "trap" aufgerufen. Die Hardware gibt die Kontrolle weiter an einen vom OS initialisierten "trap handler". Wenn das OS die Anfrage bearbeitet hat, wird die Kontrolle über eine "return-from-trap" instruktion wieder dem User gegeben.

### 1.6 Virtueller Adressraum

Manchmal auch einfach Adressraum genannt, ist "Die Abstraktion des physischen Speichers"- Mächtel. Für jeden Prozess sieht es aus als gehöre ihm allein der gesamte Physikalische Speicher. Das OS mapped virtuellen Speicher auf den Physikalischen. 

### 1.7 Multiprogramming

Ist die fähigkeit auf einem Computer, mehrere Programme gleichezeitig auszuführen und nicht jedes Programm einzeln laufen zu lassen. Dies erlaubt es zum Beispiel ein weiteres Programm zu starten während ein anderes gerade auf I/O wartet.

## 2 Der Prozess (Kapitel 4)

The Crux of the Problem: Wie stellt man die Illusion mehrerer CPUs zur Verfügung?

### 2.1 Was ist ein Prozess?

Ein Prozess ist ein laufendes Programm. Das OS wechselt ständig zwischen Prozessen hin und her um die Illusion zu erzeugen das es mehrere CPUs gleichzeitig geben würde. Die Grundlegende Technik heisst **time sharing**.

### 2.2 Policy vs Mechansims

Ein Mechanismus ist die technische Umsetzung der vorliegenden Regeln (Policy). Im Betriebssystem ist ein solcher Mechanismus der **context switch**. Gesteuert wird er von der Scheduling policy welche entscheidet wann ein solcher context switch statt finden soll.

### 2.3 Machine state

Der **machine state** ist zusammengesetzt aus dem was ein Programm lesen oder updaten kann während es läuft. Ein wichtiger Teil des machine state welcher bestandteil eines Prozesses ist, ist der Adressraum. Ein weiter Teil sind die Register. Spezielle Register die für den aktuellen state wichtig sind, ist Beispielsweise der **program counter** oder auch instruction pointer genannt und der stack- und zugehörige framepointer. Ein weiterer bestandteil ist eine liste von dateien die gerade offen sind.

### 2.4 Prozess API

* **Create**: Eine Methode um neue Prozesse zu erzeugen. Im vergleich zu frühen Betriebssystemen, laden neue ihre Programmdaten **lazily**, also erst bei gebrauch.

* **Destroy**: Wenn Prozesse nicht selbstständig schliessen kann es nützlich sein sie zum beenden zu zwingen.

* **Wait**: Dieses Interface macht es möglich Prozesse anzuhalten.

* **Miscellaneous Control**: Andere Methoden erlauben es zum Beispiel Prozesse nur eine gewisse dauer schlafen zu legen.

* **Status**: Normalerweise gibt es auch Interfaces welche Inforamtionen über einen bestimmten Prozess zurückgeben. 

### 2.5 Prozess zustände

* **Running**: Der Prozess läuft gerade auf dem Prozessor.

* **Ready**: Der Prozess ist bereit zu laufen wurde aber noch nicht gescheduled.

* **Blocked**: Der Prozess ist blockiert, zum Beispiel wartet er gerade auf I/O und signalisiert so das ein anderer Prozess laufen kann. Hier ist der nächste Schritt Ready. Ein Prozess kann nie vom zustand Ready in den Zustand Blocked versetzt werden.

### 2.6 Prozessliste

Diese enthält Informationen über alle Prozess im System. Jeder Eintrag befindet sich im sogenannten **process control block (PCB)**. Eine Struktur welche die Inforamtionen eines bestimmten Prozesses enthält.

### 2.7 Homework: The Process

Simulator: "process-run-py". Erlaubt es einem den Status eines Prozesses zu beobachten. Entwerder nutzt er gerade die CPU oder I/O.

```bash
./process-run.py -l 4:100,1:0
```

Dieser Befehl würde zwei Prozesse starten, einen der zu 100 prozent die CPU verwendet und einen der nur I/O verwendet. Die Ausgabe sähe so aus:

```bash
Time     PID: 0     PID: 1        CPU        IOs 
  1     RUN:cpu      READY          1            
  2     RUN:cpu      READY          1            
  3     RUN:cpu      READY          1            
  4     RUN:cpu      READY          1            
  5        DONE     RUN:io          1            
  6        DONE    WAITING                     1 
  7        DONE    WAITING                     1 
  8        DONE    WAITING                     1 
  9        DONE    WAITING                     1 
 10*       DONE       DONE 
 ```	

 Da noch keine Flags gesetzt sind, läuft zuerst Prozess 1 und dann Prozess 2 obwohl beide gleichzeitig ausgeführt werden könnten.

Wann die Prozesse wechseln lässt sich mit dem -S Flag steuern:

 * **SWITCH_ON_IO**: Sobald ein I/O Vorgang auftritt kommt der nächste Prozess drann.

 * **SWITCH_ON_END**: Sobald der Prozess fertig ist, kommt der nächste dran.

 Was passiert wenn ein I/O Vorgang beendet ist lässt sich mit dem -I Flag steuern:

 * **IO_RUN_LATER**: Der Prozess wartet bis ein anderer Prozess fertig ist.

 * **IO_RUN_IMMEDIATE**: Sobald die I/O fertig ist, bekommt der Prozess sofort wieder CPU Zeit.


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

### 4.5 Voo-Doo Constants

Eine Voodoo Konstante wäre zum Beispiel die Periode in welcher ein Prozess auf eine höhere Priorität geboosted wird. Wenn sie zu hoch angesetzt wird, verhungern größere jobs, ist sie zu klein laufen interaktive jobs vielleicht nicht lange genug. Es gibt keinen eindeutig richtigen Wert.

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





## 7 Threads

### 7.1 Thread-API (Kaptitel 27)

#### 7.1.1 Was ist ein Thread

... 
	
#### 7.1.2 Basic-Befehle:

	pthread_t T1;	

	pthread_create(&T1, NULL, worker_T1, args_T1);

	void* worker(void* args)

	pthread_join(T1, retval);

	pthread_exit(NULL);



#### 7.1.3 Thread-Argumente übergeben:

Bei einem Argument:

	int* arg = malloc(sizeof(int)); 	//Argumente sollten auf den Heap falls der Eltern-Thread zuerst beendet wird
	
	pthread_create(&T1, NULL, worker_T1, arg);



Bei mehreren Argumenten:

	struct arguments{
		int arg1;
		int arg2;
	};

	struct arguments* args = malloc(sizeof(struct arguments)); 	//Auch hier: argumente auf den Heap = good practice

	
	pthread_create(&T1, NULL, worker_T1, args);
	




#### 7.1.4 Thread-return:

	void* worker(void* args) {

		//... do something ....

		int* retval = malloc(sizeof(int));    	//**Returnvalues müssen auf den HEAP, da der Thread beendet wird!!!**
		return (void*)retval;
	}




### 7.2 Locks (Kapitel 28)

#### 7.2.1 Was ist ein Lock

...

#### 7.2.2 Lock-API (mutex)

Es gibt zwei Möglichkeiten einen Lock zu initialisieren:

	pthread_mutex_t  mutex1  =  PTHREAD_MUTEX_INITIALIZER;  //direkte initialisierung

	pthread_mutex_t  mutex2;
	pthread_mutex_init( &mutex2,  NULL );		//indirekte initialisierung

	assert(pthread_mutex_init( &mutex2,  NULL ));	//assertion = good practice



Locks können immer nur von einem Thread gehalten werden:

	pthread_mutex_lock( &mutex1 );
		
		// .. do something in critical section ..

	pthread_mutex_unlock( &mutex1 );


#### 7.2.3 Spinlock

Ein Spinlock ist die einfachste Form des Locks. Allerdings hat er zwei Probleme: während Threads auf den Lock
warten, laufen sie mit ständigen Anfragen in der while-Schleife leer, ausserdem ist nicht sichergestellt, dass ein Thread
einen Lock überhaupt bekommt, wenn er immer im falschen Moment CPU-Zeit bekommt.

	typedef struct __lock_t {
		int flag;
 	} lock_t;

 	void init(lock_t *lock) {
 		lock->flag = 0;		//  0 = unlocked,   1 = locked
 	}



	void lock(lock_t *lock) {
 		while (TestAndSet(&lock->flag, 1) == 1); 	// solange gelockt spin mit while
 	}

 	void unlock(lock_t *lock) {
 		lock->flag = 0;
 	}



	int TestAndSet(int *old_ptr, int new) {	// ALLES HIER IST ATOMIC!!!
		int old = *old_ptr; 	
		*old_ptr = new; 	
		return old; 			
	}




#### 7.2.4  Ticket-Spinlock

Ein Ticket-Lock kann genau wie ein Spinlock funktionieren, allerdings ist klar geregelt, welcher Thread
den Lock als nächstes bekommt. Beim normalen Spinlock entscheidet der Scheduler, wer überhaupt die Möglichkeit bekommt,
einen freien Lock zu packen und sorgt im schlimmsten Fall dafür,
Das Threads ewig auf einen Lock warten. Mit dem Ticket-System ist zumindest dieses Problem gelöst.


	typedef struct __lock_t {
		int ticket;
		int turn;
	} lock_t;


 	void lock_init(lock_t *lock) {
 		lock->ticket = 0;
 		lock->turn = 0;
 	}


 	void lock(lock_t *lock) {
 		int myturn = FetchAndAdd(&lock->ticket);
 		while (lock->turn != myturn)			; // Auch hier kommt ein Spin zum einsatz
 	}


 	void unlock(lock_t *lock) {
 		lock->turn = lock->turn + 1;
 	}



	int FetchAndAdd(int *ptr) {		// ALLES HIER IST ATOMIC!!!
 		int old = *ptr;
 		*ptr = old + 1;
 		return old;
 	}



#### 7.2.5 yield statt spin

Locks können, statt zu spinnen auch einfach die CPU abgeben, um so unnötiges warten zu verhindern. In der Realität
Ist ein Spin aber meisstens trotzdem für kurze Zeit erwünscht, moderne Systeme nutzen daher ein hybrides System. Sogenannte **TWO-PHASED-LOCKS**

	void lock() {
 		while (TestAndSet(&flag, 1) == 1)
		yield(); 				// unter linux: sched_yield()
	}
	
 	void unlock() {
 		flag = 0;
 	}




#### 7.2.6 Lock-Queue

Eine Lock-Queue funktioniert ähnlich wie ein Ticket-System, und nutzt Funktionen wie 
	queue_add(id)
	park;
	unpark;
Um die Threads ähnlich einem Scheduler zu verwalten. (Code zu viel für Zusammenfassung)



#### 7.2.7 Pettersons-Algorithmus

Der Peterson-Alogrithmus ist ein bescheidener Alogrithmus. Threads geben hier die Kontrolle freiwillig an die anderen wartenden Threads ab.

	int turn;
	int flag[2];

	void thread_1(){
    		flag[0] = 1;
    		turn = 2;
   		 while(flag[0] && turn == 2);
    		//--Hier Critical Section--
    		flag[0] = 0;

	void thread_2(){
    		flag[1] = 1
    		turn = 1;
    		while(flag[1] && turn == 1);
    		//--Hier Critical Section--
    		flag[1] = 0;




#### 7.2.8 Homework (Simulation)


....



### 7.3 Condition-Variablen (Kapitel 30)


#### 7.3.1 Was ist eine Condition-Variable

Condition-Variablen helfen bei der Synchronisation von Threads. Über  wait(condition-variable)  können sich Threads "schlafen legen und auf   signal(condition-variable)  von anderen Threads warten. Damit lässt sich auch eine Barriere für Threads bauen, die mehere Threads an einer Stelle aufählt, bis eine globale Bedingung erfüllt ist. Solche Funktionen sind insbesondere bei Producer-Consumer-Verhältnissen wichtig. Ist der Buffer leer? wait! Ist der Buffer voll? signal!



#### 7.3.2 Cond-API
	

	pthread_cond_t   condition_var;

	condition_var  =  PTHREAD_COND_INITIALIZER;
 
	pthread_cond_signal(&condition_var);

	pthread_cond_wait(&condition_var, &lock);

**ACHTUNG: wait() geht davon aus, dass der Thread der wait aufruft einen Lock, der wait() lockt in besitz hat. Wait gibt diesen Lock während des wartens frei!!** 

	
#### 7.3.3 Simpler Condition-Join

Über eine Condition Variable kann eine simple Alternative zu pthread_join gebaut werden.


	void thread_join(){
    		pthread_mutex_lock(&lock);				// LOCKS SIND WICHTIG, DA SONST INTERRUPT IN DER WHILE SCHLEIFE
    		while(thread_end == false){                             // while statt if, good practice da if Probleme verursachen kann
        		pthread_cond_wait(&condition, &lock);   	// Solange end nicht true ist, wartet die Funktion join() die Condition-Variable stellt hier somit eine Barriere dar 
    		}
    		pthread_mutex_unlock(&lock);
	}

	void thread_exit(){
    		pthread_mutex_lock(&lock);
    		end = true;
    		pthread_cond_signal(&condition);			// Sobald der gewünschte Thread das Signal gibt, wird die "Barriere" mit wait() aufgehoben.
    		pthread_mutex_unlock(&lock);
	}



#### 7.3.4 Barriere

**Info - es gibt auch vorgefertigte Barrieren in POSIX: pthread_barrier_t  mit  pthread_barrier_wait()**


...... funktionierender Code fehlt noch .....




### 7.4 Semaphoren (Kapitel 31)

#### 7.4.1 Was ist eine Semaphore

Eine Semaphore steuert, wie viele "User" gleichzeitig auf eine Resource zugreifen können. Diese können unter Linux "einzeln eingelassen" werden. Mit einer Semaphore die den Wert 1 bekommt, lässt sich zudem ein simpler Lock bauen, da in diesem Fall ja nur ein User jeweils auf die Semaphore zugreift. Gesteuert wird die Semaphore über einen Internen Zähler. Ist dieser auf 0, werden alle Threads die *sem_wait* aufrufen vorerst schalfen gelegt. Nun kann der Zähler mit *sem_post* um 1 inkrementiert werden. Die Semaphore lässt nun einen Thread passieren.



#### 7.4.2. Sem-API

	sem_t semaphore1;
	
	sem_wait(&semaphore1);   

	sem_post(&semaphore1);



#### 7.4.3 Code einer Semaphore (aus ostep geklaut)

	typedef struct __Zem_t {
		int value;
 		pthread_cond_t cond;
 		pthread_mutex_t lock;
 	} Zem_t;


 	void Zem_init(Zem_t *s, int value) {
 		s->value = value;
 		Cond_init(&s->cond);
 		Mutex_init(&s->lock);
 	}

 	void Zem_wait(Zem_t *s) {
 		Mutex_lock(&s->lock);
 		while (s->value <= 0)
 		Cond_wait(&s->cond, &s->lock);
 		s->value--;
 		Mutex_unlock(&s->lock);
 	}

 	void Zem_post(Zem_t *s) {
 		Mutex_lock(&s->lock);
 		s->value++;
 		Cond_signal(&s->cond);
 		Mutex_unlock(&s->lock);
 	}


#### 7.4.4 Dining-Philosophers-Problem




