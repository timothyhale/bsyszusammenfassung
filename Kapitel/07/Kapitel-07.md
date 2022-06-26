## 3 Scheduler (Kaptiel 7)

### 3.1 Scheduling Metriken

$$T_{turnaround}=T_{completion} - T_{arrival}$$

* $T_{turnaround}$ ist die Dauer bis ein Prozess fertig gelaufen ist.

* $T_{completion}$ ist die Zeit an welcher ein Prozess fertig geworden ist.

* $T_{arrival}$ ist die Ankuftszeit eines Prozesses.

Die Turnaroundzeit ist eine Performance Metrik und kann oft mit der Fairnessmetrik in Konflikt stehen.

### 3.2 Algorithmen

#### 3.2.1 First In, First Out (FIFO)

Wer zu erst kommt, wird zuerst bedient. Das hat den Nachteil, das sehr kurze Prozesse auf einen langen warten müssen. Nennt man auch den **convoy effect**.

#### 3.2.2 Shortest Job First (SJF)
