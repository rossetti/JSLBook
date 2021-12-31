# Introduction to Discrete Event Modeling {#introDEDS}

**[LEARNING OBJECTIVES]{.smallcaps}**

-   To be able to recognize and define the characteristics of a
    discrete-event dynamic system (DEDS)

-   To be able to explain how time evolves in a DEDS

-   To be able to develop and read an activity flow diagram

-   To be able to create, run, and examine the results of a JSL model of
    a simple DEDS

## Introduction {#introDEDS:Intro}

In Chapter \@ref(mcm), we
explored how to develop models in the JSL for which time is not a
significant factor. In the case of the news vendor problem, where we
simulated each day's demand, time advanced at regular intervals. In the
case of the area estimation problem, time was not a factor in the
simulation. These types of simulations are often termed static
simulations. In the next section, we begin our exploration of simulation
where time is an integral component in driving the behavior of the
system. In addition, we will see that time will not necessarily advance
at regular intervals (e.g. hour 1, hour 2, etc.). This will be the focus
of the rest of the textbook.

In this chapter, we explore the JSL simulation software platform for
developing and executing simulation models. We will begin our study of
the major emphasis of this textbook: modeling discrete-event dynamic
systems. Recall that a discrete-event dynamic system (DEDS) is a system
that evolves dynamically through time. This chapter will introduce how
time evolves for DEDSs and illustrate how the JSL can be used to develop
models for simple discrete-event system.

We can think of a system as a set of inter-related objects that work
together to accomplish a common objective. The objects within a system
are the concepts, abstractions, things, and processes with definable
boundaries and unique identity given our modeling perspective. Within
the traditional simulation parlance, objects of particular interest
within a system have often been called entities; however, this text
takes a broader view to include other modeling elements of interest
within the system (e.g. resources).

A discrete event system is a system in which the state of the system
changes only at discrete points in time. There are essentially two
fundamental viewpoints for modeling a discrete event system within
simulation: the event view and the process view. These views are simply
different representations for the same system. In the event view, the
system and its elements are conceptualized as reacting to events. In the
process view, the movement of entities through their processes implies
the events within the system in the system. This chapter focuses on the
event view.

The objects within the system change state because events occur within
the system. In addition, the changes in state for an object may cause
additional events to occur, affecting other objects within the system.
Through the propagation of events and object state changes, the entire
system state evolves through time. The state of the system is
essentially the collection of states associated with all the objects in
the system. The notion of modeling what happens to a system at
particular events in time is the basis of discrete event modeling.

## Discrete-Event Dynamic Systems {#introDEDS:deds}

Nelson (1995) states that the event-view "defines system events by
describing what happens to the system as it encounters entities". In
addition, Nelson (1995) states that the process view "implies system
events by describing what happens to an entity as it encounters the
system". One can consider the event-view as taking the perspective of
the system and the process-view as taking the perspective of the entity.
The process-view describes the life cycles of objects within the system.
Because of the way in which the process-view has been implemented
through a network transaction flow approach within many simulation
packages, the term entity has taken on the connotation of objects that
flow or move within the system. To avoid confusion, the more general
term, object, will be used to encompass non-flowing objects as well as
flowing objects within the system.

To understand the event-view, the concepts of event, activity, state,
and process must be clearly defined. Rumbaugh et al. (1991) defines
state and its relationship to events and activities as "an abstraction
of the attribute values and links of an object. Sets of values are
grouped together into a state according to properties that affect the
gross behavior of the object. A state corresponds to the interval
between two events. A state has duration; it occupies an interval of
time. A state is often associated with an activity that takes time to
complete or with the value of an object satisfying some condition. In
defining states, we ignore those attributes that do not affect the
behavior of the object and we lump together in a single state all
combinations of attribute values and links that have the same response
to events." Thus, the state of an object should entail only those
aspects that are required for the modeling.

An event is something that happens at an instant in time that
corresponds to a change in object state. An event is a one-way
transmission of information from one object to another that causes an
action resulting in a change in state. An event is said to be a
scheduled event (or timed, determined, bounded) if its occurrence can be
expressed as a function of system time and can thus be scheduled in
time. For example, suppose you have a doctor's appointment at 11 am. The
beginning of the appointment represents a scheduled event. An event is
said to be a conditional event if it is dependent upon the outcome of
certain conditions that cannot be predicted with certainty in advance
(e.g. the availability of a server). Conditional events are sometimes
called unbounded or contingent. If a scheduled event ends an activity,
then the activity is said to be a scheduled or timed activity;
otherwise, it is said to be a conditional activity.

Associated with an event is an action that is an instantaneous
operation, i.e. it takes zero simulated time to complete. In contrast,
an activity is an operation that takes simulated time to complete. An
activity can be associated with the state of an object over an interval.
Activities are defined by the occurrence of two events that represent
the activity's beginning time and ending time and mark the entrance and
exit of the state associated with the activity. In the simulation
literature, it is common to refer to activities that take a specified
duration of time as simply activities. To be more specific, it is useful
to classify activities of a specified duration as timed activities.
Getting your haircut is an example of a timed activity. There is a clear
beginning and a clear ending for the activity. An activity that
encompasses an interval of time that is unspecified or of indefinite
length is called a conditional activity. An example of a conditional
activity is a customer waiting in a queue for the barber to become
available. The length of the activity depends on when the barber becomes
available.

In the next section, we begin our exploration of simulation where time is an
integral component in driving the behavior of the system. In addition,
we will see that time will not necessarily advance at regular intervals
(e.g. hour 1, hour 2, etc.). As discussed, an event occurs at a specific point in time, thus we need to understand how the simulation time clock works.

## How the Discrete-Event Clock Works {#HowDEDSClockWorks}

This section introduces the concept of how time evolves in a discrete
event simulation. This topic will also be revisited in future chapters
after you have developed a deeper understanding for many of the
underlying elements of simulation modeling.

In discrete-event dynamic systems, an event is something that happens at
an instant in time which corresponds to a change in system state. An
event can be conceptualized as a transmission of information that causes
an action resulting in a change in system state. Let's consider a simple
bank which has two tellers that serve customers from a single waiting
line. In this situation, the system of interest is the bank tellers
(whether they are idle or busy) and any customers waiting in the line.
Assume that the bank opens at 9 am, which can be used to designate time
zero for the simulation. It should be clear that if the bank does not
have any customers arrive during the day that the state of the bank will
not change. In addition, when a customer arrives, the number of
customers in the bank will increase by one. In other words, an arrival
of a customer will change the state of the bank. Thus, the arrival of a
customer can be considered an event.

\begin{figure}

{\centering \includegraphics{./figures2/ch2/fig13ArrivalProcess} 

}

\caption{Customer Arrival Process}(\#fig:ArrivalProcess)
\end{figure}

::: {#tab:CustArrivals}
   Customer   Time of arrival   Time between arrival
  ---------- ----------------- ----------------------
      1              2                   2
      2              5                   3
      3              7                   2
      4             15                   8

  Table: (\#tab:CustArrivals) Customer Time of Arrivals
:::


Figure \@ref(fig:ArrivalProcess) illustrates a time line of customer
arrival events. The values for the arrival times in Table \@ref(tab:CustArrivals) have been rounded to
whole numbers, but in general the arrival times can be any real valued
numbers greater than zero. According to the figure, the first customer
arrives at time two and there is an elapse of three minutes before
customer 2 arrives at time five. From the discrete-event perspective,
*nothing* is happening in the bank from time $[0,2)$; however, at time 2, an
arrival event occurs and the subsequent actions associated with this
event need to be accounted for with respect to the state of the system. An event denotes an instance of time that changes the state of the system.  Since an event causes actions that result in a change of system state,
it is natural to ask: What are the actions that occur when a customer arrives to the
bank?

-   The customer enters the waiting line.

-   If there is an available teller, the customer will immediately exit
    the line and the available teller will begin to provide service.

-   If there are no tellers available, the customer will remain waiting
    in the line until a teller becomes available.

Now, consider the arrival of the first customer. Since the bank opens at
9 am with no customer and all the tellers idle, the first customer will
enter and immediately exit the queue at time 9:02 am (or time 2) and
then begin service. After the customer completes service, the customer
will exit the bank. When a customer completes service (and departs the
bank) the number of customers in the bank will decrease by one. It
should be clear that some actions need to occur when a customer
completes service. These actions correspond to the second event
associated with this system: the customer service completion event. What
are the actions that occur at this event?

-   The customer departs the bank.

-   If there are waiting customers, the teller indicates to the next
    customer that he/she will serve the customer. The customer will exit
    the waiting line and will begin service with the teller.

-   If there are no waiting customers, then the teller will become idle.

Figure \@ref(fig:ServiceProcess) contains the service times for each
of the four customers that arrived in
Figure \@ref(fig:ArrivalProcess). Examine the figure with respect to
customer 1. Based on the figure, customer 1 can enter service at time
two because there were no other customers present in the bank. Suppose
that it is now 9:02 am (time 2) and that the service time of customer 1
is known *in advance* to be 8 minutes as indicated in Table \@ref(tab:CustServiceTimes). From this information, the time that customer 1 is going to complete service can be pre-computed. From
the information in the figure, customer 1 will complete service at time
10 (current time + service time = 2 + 8 = 10). Thus, it should be
apparent, that for you to recreate the bank's behavior over a time
period that you must have knowledge of the customer's service times. The
service time of each customer coupled with the knowledge of when the
customer began service allows the time that the customer will complete
service and depart the bank to be computed in advance. A careful
inspection of Figure \@ref(fig:ServiceProcess) and knowledge of how banks operate
should indicate that a customer will begin service either immediately
upon arrival (when a teller is available) or coinciding with when
another customer departs the bank after being served. This latter
situation occurs when the queue is not empty after a customer completes
service. The times that service completions occur and the times that
arrivals occur constitute the pertinent events for simulating this
banking system.

\begin{figure}

{\centering \includegraphics{./figures2/ch2/fig14ServiceProcess} 

}

\caption{Customer Service Process}(\#fig:ServiceProcess)
\end{figure}

::: {#tab:CustServiceTimes}
   Customer   Service Time Started   Service Time   Service Time Completed
  ---------- ---------------------- -------------- ------------------------
      1                2                  8                   10
      2                5                  7                   12
      3                10                 9                   19
      4                15                 2                   17

  Table: (\#tab:CustServiceTimes) Service Time for First Four Customers
:::

If the arrival and the service completion events are combined, then the
time ordered sequence of events for the system can be determined.
Figure \@ref(fig:TimeOrderedProcess) illustrates the events ordered by time
from smallest event time to largest event time. Suppose you are standing
at time two. Looking forward, the next event to occur will be at time 5
when the second customer arrives. When you simulate a system, you must
be able to generate a sequence of events so that, at the occurrence of
each event, the appropriate actions are invoked that change the state of
the system.

\begin{figure}

{\centering \includegraphics{./figures2/ch2/fig15TimeOrderedProcess} 

}

\caption{Events Ordered by Time Process}(\#fig:TimeOrderedProcess)
\end{figure}

   Time         Event         Comment
  ------ -------------------- -------------------------------------------------------------------------------------------
    0         Bank 0pens      
    2          Arrival        Customer 1 arrives, enters service for 8 minutes, one teller becomes busy
    5          Arrival        Customer 2 arrives, enters service for 7 minutes, the second teller becomes busy
    7          Arrival        Customer 3 arrives, waits in queue
    10    Service Completion  Customer 1 completes service, customer 3 exits the queue and enters service for 9 minutes
    12    Service Completion  Customer 2 completes service, no customers are in the queue so a teller becomes idle
    15         Arrival        Customer 4 arrives, enters service for 2 minutes, one teller becomes busy
    17    Service Completion  Customer 4 completes service, a teller becomes idle
    19    Service Completion  Customer 3 completes service

\

The real system will simply evolve over time; however, a simulation of
the system must recreate the events. In simulation, events are created
by adding additional logic to the normal state changing actions. This
additional logic is responsible for scheduling future events that are
implied by the actions of the current event. For example, when a
customer arrives, the time to the next arrival can be generated and
scheduled to occur at some future time. This can be done by generating
the time until the next arrival and adding it to the current time to
determine the actual arrival time. Thus, all the arrival times do not
need to be pre-computed prior to the start of the simulation. For
example, at time two, customer 1 arrived. Customer 2 arrives at time 5.
Thus, the time between arrivals (3 minutes) is added to the current time
and customer 2's arrival at time 5 is scheduled to occur. Every time an
arrival occurs this additional logic is invoked to ensure that more
arrivals will continue within the simulation.

Adding additional scheduling logic also occurs for service completion
events. For example, since customer 1 immediately enters service at time
2, the service completion of customer 1 can be scheduled to occur at
time 12 (current time + service time = 2 + 10 = 12). Notice that other
events may have already been scheduled for times less than time 12. In
addition, other events may be inserted before the service completion at
time 12 actually occurs. From this, you should begin to get a feeling
for how a computer program can implement some of these ideas.

Based on this intuitive notion for how a computer simulation may
execute, you should realize that computer logic processing need only
occur at the event times. That is, the state of the system does not
change between events. Thus, it is *not necessary* to step incrementally
through time checking to see if something happens at time 0.1, 0.2, 0.3,
etc. (or whatever time scale you envision that is fine enough to not
miss any events). Thus, in a discrete-event dynamic system simulation the clock does not
"tick" at regular intervals. Instead, the simulation clock jumps from
event time to event time. As the simulation moves from the current event
to the next event, the current simulation time is updated to the time of
the next event and any changes to the system associated with the next
event are executed. This allows the simulation to evolve over time.

## Simulating a Queueing System By Hand

This section builds on the concepts discussed in the previous section in order to provide insights into how discrete event simulations operate.  In order to do this, we will simulate a simple queueing system by hand.  That is, we will process each of the events that occur as well as the state changes in order to trace the operation of the system through time.  

Consider again the simple banking system described in the previous
section. To simplify the situation, we assume that there is only one
teller that is available to provide service to the arriving customers.
Customers that arrive while the teller is already helping a customer
form a single waiting line, which is handled in a first come, first
served manner. Let's assume that the bank opens at 9 am with no
customers present and the teller idle. The time of arrival of the first
eight customers is provided in the following table.

  ----------------- ----------------- --------------
  Customer Number   Time of Arrival   Service Time
  1                 3                 4
  2                 11                4
  3                 13                4
  4                 14                3
  5                 17                2
  6                 19                4
  7                 21                3
  8                 27                2
  ----------------- ----------------- --------------

We are going to process these customers in order to recreate the
behavior of this system over from time 0 to 31 minutes. To do this, we
need to be able to perform the "bookkeeping" that a computer simulation
model would normally perform for us. Thus, we will need to define some
variables associated with the system and some attributes associated with
the customers. Consider the following system variables.

System Variables

-   Let $t$ represent the current simulation clock time.

-   Let $N(t)$ represent the number of customers in the system (bank) at
    any time $t$.

-   Let $Q(t)$ represent the number of customers waiting in line for the
    teller at any time $t$.

-   Let $B(t)$ represent whether or not the teller is busy (1) or
    idle (0) at any time $t$.

Because we know the number of tellers available, we know that the
following relationship holds between the variables:

$$N\left( t \right) = Q\left( t \right) + B(t)$$

Note also that, knowledge of the value of $N(t)$ is sufficient to
determine the values of $Q(t)$ and $B(t)$ at any time $t.$ For example,
if we know that there are 3 customers in the bank, then
$N\left( t \right) = 3$, and since the teller must be serving 1 of those
customers, $B\left( t \right) = 1$ and there must be 2 customers
waiting, $Q\left( t \right) = 2$. In this situation, since
$N\left( t \right)$, is sufficient to determine all the other system
variables, we can say that $N\left( t \right)$ is the system's *state
variable*. The state variable(s) are the minimal set of system
variables that are necessary to represent the system's behavior over
time. We will keep track of the values of all of the system variables
during our processing in order to collect statistics across time.

Attributes are properties of things that move through the system. In the parlance of simulation, we call the things that move through the system entity instances or entities.  In this situation, the entities are the customers, and we can define a number of attributes for each customer. Here, customer is a type of entity or entity type.  If we have other things that flow through the system, we identity the different types (or entity types).  The attributes of the customer entity type are as follows.  Notice that each attribute is subscripted by $i$, which indicates the $i^{th}$ customer instance.

Entity Attributes

-   Let $\mathrm{ID}_{i}$ be the identity number of the customer.
    $\mathrm{ID}_{i}$ is a unique number assigned to each customer that
    identifies the customer from other customers in the system.

-   Let $A_{i}$ be the arrival time of the $i^{\mathrm{th}}$ customer.

-   Let $S_{i}$ be the time the $i^{\mathrm{th}}$ customer started
    service.

-   Let $D_{i}$ be the departure time of the $i^{\mathrm{th}}$ customer.

-   Let $\mathrm{ST}_{i}$ be the service time of the $i^{\mathrm{th}}$
    customer.

-   Let $T_{i}$ be the total time spent in the system of the
    $i^{\mathrm{th}}$ customer.

-   Let $W_{i}$ be the time spent waiting in the queue for the
    $i^{\mathrm{th}}$ customer.

Clearly, there are also relationships between these attributes. We can
compute the total time in the system for each customer from their
arrival and departure times:

$$T_{i} = D_{i} - A_{i}$$

In addition, we can compute the time spent waiting in the queue for each
customer as:

$$W_{i} = T_{i} - \mathrm{ST}_{i} = S_{i} - A_{i}$$

As in the previous section, there are two types of events: arrivals and
departures. Let $E(t)$ be (A) for an arrival event at time $t$ and be
(D) for a departure event at time $t$. As we process each event, we will
keep track of when the event occurs and the type of event. We will also
keep track of the state of the system after each event has been
processed. To make this easier, we will keep track of the system
variables and the entity attributes within a table as follows.

\begin{table}

\caption{(\#tab:SQBH1)Hand Bank Simulation Bookkeeping Table.}
\centering
\fontsize{10}{12}\selectfont
\begin{tabular}[t]{rlrrrrrrrrrrl}
\toprule
\multicolumn{5}{c}{System Variables} & \multicolumn{7}{c}{Entity Attributes} & \multicolumn{1}{c}{ } \\
\cmidrule(l{3pt}r{3pt}){1-5} \cmidrule(l{3pt}r{3pt}){6-12}
t & E(t) & N(t) & B(t) & Q(t) & ID(i) & A(i) & S(i) & ST(i) & D(i) & T(i) & W(i) & Pending E(t)\\
\midrule
0 & NA & 0 & 0 & 0 & NA & NA & NA & NA & NA & NA & NA & NA\\
\bottomrule
\end{tabular}
\end{table}

In the table, we see that the initial state of the system is empty and
idle. Since there are no customers in the bank, there are no tabulated
attribute values within the table. Reviewing the provided information,
we see that customer 1 arrives at $t = 3$ and has a service time of 4
minutes. Thus,$\ \text{ID}_{1} = 1$, $A_{1} = 3$, and
$\text{ST}_{1} = 4$. We can enter this information directly into our
bookkeeping table. In addition, because the bank was empty and idle when
the customer arrived, we can note that the time that customer 1 starts
service is the same as the time of their arrival and that they did not
spend any time waiting in the queue. Thus, $S_{1} = 3$ and $W_{1} = 0$.
The table has been updated as follows.

\begin{table}

\caption{(\#tab:SQBH2)Hand Bank Simulation Bookkeeping Table.}
\centering
\fontsize{10}{12}\selectfont
\begin{tabular}[t]{rlrrrrrrrrrrl}
\toprule
\multicolumn{5}{c}{System Variables} & \multicolumn{7}{c}{Entity Attributes} & \multicolumn{1}{c}{ } \\
\cmidrule(l{3pt}r{3pt}){1-5} \cmidrule(l{3pt}r{3pt}){6-12}
t & E(t) & N(t) & B(t) & Q(t) & ID(i) & A(i) & S(i) & ST(i) & D(i) & T(i) & W(i) & Pending E(t)\\
\midrule
0 & NA & 0 & 0 & 0 & NA & NA & NA & NA & NA & NA & NA & NA\\
3 & A & 1 & 1 & 0 & 1 & 3 & 4 & 3 & NA & NA & 0 & E(7) = D(1), E(11) = A(2)\\
\bottomrule
\end{tabular}
\end{table}

Because customer 1 arrived to an empty system, they immediately started
service at time 3. Since we know the service time of the customer,
$\text{ST}_{1} = 4$, and the current time, $t = 3$, we can determine
that customer 1, will depart from the system at time 7
($t = 3 + 4 = 7$). That means we will have a departure event for
customer 1 at time 7. According to the provided data, the next customer,
customer 2, will arrive at time 11. Thus, we have two pending events, a
departure of customer 1 at time 7 and the arrival of customer 2 at time
11. This fact is noted in the column labeled "Pending E(t)" for pending events. Here 
E(7) = D(1), E(11) = A(2) indicates that customer 1 with
depart,$\ D_{1},$ at time 7, $E\left( 7 \right)$ and customer 2 will
arrive, $A_{2}$, at the event at time 11, $E(11)$. Clearly, the next
event time will be the minimum of 7 and 11, which will be the departure
of the first customer. Thus, our bookkeeping table can be updated as
follows.

\begin{table}

\caption{(\#tab:SQBH3)Hand Bank Simulation Bookkeeping Table.}
\centering
\fontsize{10}{12}\selectfont
\begin{tabular}[t]{rlrrrrrrrrrrl}
\toprule
\multicolumn{5}{c}{System Variables} & \multicolumn{7}{c}{Entity Attributes} & \multicolumn{1}{c}{ } \\
\cmidrule(l{3pt}r{3pt}){1-5} \cmidrule(l{3pt}r{3pt}){6-12}
t & E(t) & N(t) & B(t) & Q(t) & ID(i) & A(i) & S(i) & ST(i) & D(i) & T(i) & W(i) & Pending E(t)\\
\midrule
0 & NA & 0 & 0 & 0 & NA & NA & NA & NA & NA & NA & NA & NA\\
3 & A & 1 & 1 & 0 & 1 & 3 & 4 & 3 & NA & NA & 0 & E(7) = D(1), E(11) = A(2)\\
7 & D & 0 & 0 & 0 & 1 & 3 & 4 & 3 & 7 & 4 & 0 & E(11) = A(2)\\
\bottomrule
\end{tabular}
\end{table}

Since there are no customers in the bank and only the one pending event,
the next event will be the arrival of customer 2 at time 11. The table
can be updated as follows.

\begin{table}

\caption{(\#tab:SQBH4)Hand Bank Simulation Bookkeeping Table.}
\centering
\fontsize{10}{12}\selectfont
\begin{tabular}[t]{rlrrrrrrrrrrl}
\toprule
\multicolumn{5}{c}{System Variables} & \multicolumn{7}{c}{Entity Attributes} & \multicolumn{1}{c}{ } \\
\cmidrule(l{3pt}r{3pt}){1-5} \cmidrule(l{3pt}r{3pt}){6-12}
t & E(t) & N(t) & B(t) & Q(t) & ID(i) & A(i) & S(i) & ST(i) & D(i) & T(i) & W(i) & Pending E(t)\\
\midrule
0 & NA & 0 & 0 & 0 & NA & NA & NA & NA & NA & NA & NA & NA\\
3 & A & 1 & 1 & 0 & 1 & 3 & 4 & 3 & NA & NA & 0 & E(7) = D(1), E(11) = A(2)\\
7 & D & 0 & 0 & 0 & 1 & 3 & 4 & 3 & 7 & 4 & 0 & E(11) = A(2)\\
11 & A & 1 & 1 & 0 & 2 & 11 & 4 & 11 & NA & NA & 0 & E(13) = A(3), E(15) = D(2)\\
\bottomrule
\end{tabular}
\end{table}

Since the pending event set is E(13) = A(3), E(15) = D(2) the next event
will be the arrival of the third customer at time 13 before the
departure of the second customer at time 15. We will now have a queue
form. Updating our bookkeeping table, yields:

\begin{table}

\caption{(\#tab:SQBH5)Hand Bank Simulation Bookkeeping Table.}
\centering
\fontsize{10}{12}\selectfont
\begin{tabular}[t]{rlrrrrrrrrrrl}
\toprule
\multicolumn{5}{c}{System Variables} & \multicolumn{7}{c}{Entity Attributes} & \multicolumn{1}{c}{ } \\
\cmidrule(l{3pt}r{3pt}){1-5} \cmidrule(l{3pt}r{3pt}){6-12}
t & E(t) & N(t) & B(t) & Q(t) & ID(i) & A(i) & S(i) & ST(i) & D(i) & T(i) & W(i) & Pending E(t)\\
\midrule
0 & NA & 0 & 0 & 0 & NA & NA & NA & NA & NA & NA & NA & NA\\
3 & A & 1 & 1 & 0 & 1 & 3 & 4 & 3 & NA & NA & 0 & E(7) = D(1), E(11) = A(2)\\
7 & D & 0 & 0 & 0 & 1 & 3 & 4 & 3 & 7 & 4 & 0 & E(11) = A(2)\\
11 & A & 1 & 1 & 0 & 2 & 11 & 4 & 11 & NA & NA & 0 & E(13) = A(3), E(15) = D(2)\\
13 & A & 2 & 1 & 1 & 3 & 13 & 4 & 15 & NA & NA & 2 & E(14) = A(4), E(15) = D(2)\\
\bottomrule
\end{tabular}
\end{table}

Notice that in the last table update, we did not update $S_{i}$ and
$W_{i}$ because customer 3 had to wait in queue and did not start
service. Customer 3 will start service, when customer 2 departs.
Reviewing the pending event set, we see that the next event will be the
arrival of customer 4 at time 14, which yields the following bookkeeping
table.

\begin{table}

\caption{(\#tab:SQBH6)Hand Bank Simulation Bookkeeping Table.}
\centering
\fontsize{10}{12}\selectfont
\begin{tabular}[t]{rlrrrrrrrrrrl}
\toprule
\multicolumn{5}{c}{System Variables} & \multicolumn{7}{c}{Entity Attributes} & \multicolumn{1}{c}{ } \\
\cmidrule(l{3pt}r{3pt}){1-5} \cmidrule(l{3pt}r{3pt}){6-12}
t & E(t) & N(t) & B(t) & Q(t) & ID(i) & A(i) & S(i) & ST(i) & D(i) & T(i) & W(i) & Pending E(t)\\
\midrule
0 & NA & 0 & 0 & 0 & NA & NA & NA & NA & NA & NA & NA & NA\\
3 & A & 1 & 1 & 0 & 1 & 3 & 4 & 3 & NA & NA & 0 & E(7) = D(1), E(11) = A(2)\\
7 & D & 0 & 0 & 0 & 1 & 3 & 4 & 3 & 7 & 4 & 0 & E(11) = A(2)\\
11 & A & 1 & 1 & 0 & 2 & 11 & 4 & 11 & NA & NA & 0 & E(13) = A(3), E(15) = D(2)\\
13 & A & 2 & 1 & 1 & 3 & 13 & 4 & 15 & NA & NA & 2 & E(14) = A(4), E(15) = D(2)\\
\addlinespace
14 & A & 3 & 1 & 2 & 4 & 14 & 3 & 19 & NA & NA & 5 & E(15) = D(2), E(17) = A(5)\\
\bottomrule
\end{tabular}
\end{table}

Notice that we now have 3 customers in the system, 1 in service and 2
waiting in the queue. Reviewing the pending event set, we see that
customer 2 will finally complete service and depart at time 15.

\begin{table}

\caption{(\#tab:SQBH7)Hand Bank Simulation Bookkeeping Table.}
\centering
\fontsize{10}{12}\selectfont
\begin{tabular}[t]{rlrrrrrrrrrrl}
\toprule
\multicolumn{5}{c}{System Variables} & \multicolumn{7}{c}{Entity Attributes} & \multicolumn{1}{c}{ } \\
\cmidrule(l{3pt}r{3pt}){1-5} \cmidrule(l{3pt}r{3pt}){6-12}
t & E(t) & N(t) & B(t) & Q(t) & ID(i) & A(i) & S(i) & ST(i) & D(i) & T(i) & W(i) & Pending E(t)\\
\midrule
0 & NA & 0 & 0 & 0 & NA & NA & NA & NA & NA & NA & NA & NA\\
3 & A & 1 & 1 & 0 & 1 & 3 & 4 & 3 & NA & NA & 0 & E(7) = D(1), E(11) = A(2)\\
7 & D & 0 & 0 & 0 & 1 & 3 & 4 & 3 & 7 & 4 & 0 & E(11) = A(2)\\
11 & A & 1 & 1 & 0 & 2 & 11 & 4 & 11 & NA & NA & 0 & E(13) = A(3), E(15) = D(2)\\
13 & A & 2 & 1 & 1 & 3 & 13 & 4 & 15 & NA & NA & 2 & E(14) = A(4), E(15) = D(2)\\
\addlinespace
14 & A & 3 & 1 & 2 & 4 & 14 & 3 & 19 & NA & NA & 5 & E(15) = D(2), E(17) = A(5)\\
15 & D & 2 & 1 & 1 & 2 & 11 & 4 & 11 & 15 & 4 & 0 & E(17) = A(5), E(19) = D(3)\\
\bottomrule
\end{tabular}
\end{table}

Because customer 2 completes service at time 15 and customer 3 is
waiting in the line, we see that customer 3's attributes for $S_{i}$ and
$W_{i}$ were updated within the table. Since customer 3 has started
service and we know their service time of 4 minutes, we know that they
will depart at time 19. The pending events set has been updated
accordingly and indicates that the next event will be the arrival of
customer 5 at time 17.

\begin{table}

\caption{(\#tab:SQBH8)Hand Bank Simulation Bookkeeping Table.}
\centering
\fontsize{10}{12}\selectfont
\begin{tabular}[t]{rlrrrrrrrrrrl}
\toprule
\multicolumn{5}{c}{System Variables} & \multicolumn{7}{c}{Entity Attributes} & \multicolumn{1}{c}{ } \\
\cmidrule(l{3pt}r{3pt}){1-5} \cmidrule(l{3pt}r{3pt}){6-12}
t & E(t) & N(t) & B(t) & Q(t) & ID(i) & A(i) & S(i) & ST(i) & D(i) & T(i) & W(i) & Pending E(t)\\
\midrule
0 & NA & 0 & 0 & 0 & NA & NA & NA & NA & NA & NA & NA & NA\\
3 & A & 1 & 1 & 0 & 1 & 3 & 4 & 3 & NA & NA & 0 & E(7) = D(1), E(11) = A(2)\\
7 & D & 0 & 0 & 0 & 1 & 3 & 4 & 3 & 7 & 4 & 0 & E(11) = A(2)\\
11 & A & 1 & 1 & 0 & 2 & 11 & 4 & 11 & NA & NA & 0 & E(13) = A(3), E(15) = D(2)\\
13 & A & 2 & 1 & 1 & 3 & 13 & 4 & 15 & NA & NA & 2 & E(14) = A(4), E(15) = D(2)\\
\addlinespace
14 & A & 3 & 1 & 2 & 4 & 14 & 3 & 19 & NA & NA & 5 & E(15) = D(2), E(17) = A(5)\\
15 & D & 2 & 1 & 1 & 2 & 11 & 4 & 11 & 15 & 4 & 0 & E(17) = A(5), E(19) = D(3)\\
17 & A & 3 & 1 & 2 & 5 & 17 & 2 & 22 & NA & NA & 5 & E(19) = D(3), E(19) = A(6)\\
\bottomrule
\end{tabular}
\end{table}

Now, we have a very interesting situation with the pending event set. We
have two events that are scheduled to occur at the same time, the
departure of customer 3 at time 19 and the arrival of customer 6 at time
19. In general, the order in which events are processed that occur at
the same time may affect how future events are processed. That is, the
order of event processing may change the behavior of the system over
time and thus the order of processing is, in general, important.
However, in this particular simple system, the order of processing will
not change what happens in the future. We will simply have an update of
the state variables at the same time. In this instance, we will process
the departure event first and then the arrival event. If you are not
convinced that this will not make a difference, then I encourage you to
change the ordering and continue the processing. In more complex system
simulations, a priority indicator is attached to the events so that the
events can be processed in a consistent manner. Rather than continuing
the step-by-step processing of the events through time 31, we will
present the completed table. We leave it as an exercise for the reader
to continue the processing of the customers. The completed bookkeeping table at
time 31 is as follows.

\begin{table}

\caption{(\#tab:SQBHFull)Hand Bank Simulation Bookkeeping Table.}
\centering
\fontsize{10}{12}\selectfont
\begin{tabular}[t]{rlrrrrrrrrrrl}
\toprule
\multicolumn{5}{c}{System Variables} & \multicolumn{7}{c}{Entity Attributes} & \multicolumn{1}{c}{ } \\
\cmidrule(l{3pt}r{3pt}){1-5} \cmidrule(l{3pt}r{3pt}){6-12}
t & E(t) & N(t) & B(t) & Q(t) & ID(i) & A(i) & S(i) & ST(i) & D(i) & T(i) & W(i) & Pending E(t)\\
\midrule
0 & NA & 0 & 0 & 0 & NA & NA & NA & NA & NA & NA & NA & NA\\
3 & A & 1 & 1 & 0 & 1 & 3 & 4 & 3 & NA & NA & 0 & E(7) = D(1), E(11) = A(2)\\
7 & D & 0 & 0 & 0 & 1 & 3 & 4 & 3 & 7 & 4 & 0 & E(11) = A(2)\\
11 & A & 1 & 1 & 0 & 2 & 11 & 4 & 11 & NA & NA & 0 & E(13) = A(3), E(15) = D(2)\\
13 & A & 2 & 1 & 1 & 3 & 13 & 4 & 15 & NA & NA & 2 & E(14) = A(4), E(15) = D(2)\\
\addlinespace
14 & A & 3 & 1 & 2 & 4 & 14 & 3 & 19 & NA & NA & 5 & E(15) = D(2), E(17) = A(5)\\
15 & D & 2 & 1 & 1 & 2 & 11 & 4 & 11 & 15 & 4 & 0 & E(17) = A(5), E(19) = D(3)\\
17 & A & 3 & 1 & 2 & 5 & 17 & 2 & 22 & NA & NA & 5 & E(19) = D(3), E(19) = A(6)\\
19 & D & 2 & 1 & 1 & 3 & 13 & 4 & 15 & 19 & 6 & 2 & E(19) = A(6)\\
19 & A & 3 & 1 & 2 & 6 & 19 & 4 & 24 & NA & NA & 5 & E(21) = A(7), E(22) = D(4)\\
\addlinespace
21 & A & 4 & 1 & 3 & 7 & 21 & 3 & 28 & NA & NA & 7 & E(22) = D(4), E(24) = D(5)\\
22 & D & 3 & 1 & 2 & 4 & 14 & 3 & 19 & 22 & 8 & 5 & E(24) = D(5), E(27) = A(8)\\
24 & D & 2 & 1 & 1 & 5 & 17 & 2 & 22 & 24 & 7 & 5 & E(27) = A(8), E(28) = D(6)\\
27 & A & 3 & 1 & 2 & 8 & 27 & 2 & 31 & NA & NA & 4 & E(28) = D(6), E(31) = D(7)\\
28 & D & 2 & 1 & 1 & 6 & 19 & 4 & 24 & 28 & 9 & 5 & E(31) = D(7)\\
\addlinespace
31 & D & 1 & 1 & 0 & 7 & 21 & 3 & 28 & 31 & 10 & 7 & E(33) = D(8)\\
\bottomrule
\end{tabular}
\end{table}

Given that we have simulated the bank over the time frame from 0 to 31
minutes, we can now compute performance statistics for the bank and
measure how well it is operating. Two statistics that we can easily
compute are the average time spent waiting in line and the average time
spent in the system.

Let $n$ be the number of customers observed to depart during the
simulation. In this simulation, we had $n = 7$ customers depart during
the time frame of 0 to 31 minutes. The time frame over which we analyze
the performance of the system is call the simulation time horizon. In
this case, the simulation time horizon is fixed and known in advance.
When we perform a computer simulation experiment, the time horizon is
often referred to as the *simulation run length* (or simulation
*replication length*). To estimate the average time spent waiting in
line and the average time spent in the system, we can simply compute the
sample averages ( $\overline{T}$ and $\bar{W})$ of the observed
quantities ($T_{i}$ and $W_{i}$)for each departed customer.

$$\overline{T} = \frac{1}{7}\sum_{i = 1}^{7}T_{i} = \frac{4 + 4 + 6 + 8 + 7 + 9 + 10}{7} = \frac{48}{7} \cong 6.8571$$

$$\overline{W} = \frac{1}{7}\sum_{i = 1}^{7}W_{i} = \frac{0 + 0 + 2 + 5 + 5 + 5 + 7}{7} = \frac{24}{7} \cong 3.4286$$

Besides the average time spent in the system and the average time spent
waiting, we might also want to compute the average number of customers
in the system, the average number of customers in the queue, and the
average number of busy tellers. You might be tempted to simply average
the values in the $N(t)$, $B(t)$, and $Q(t)$ columns of the bookkeeping table.
Unfortunately, that will result in an incorrect estimate of these values
because a simple average will not take into account how long each of the
variables persisted with its values over time. In this situation, we are
really interested in computed a time average. This is because the
variables, $N(t)$, $B(t)$, and $Q(t)$ are time-based variables. This type of
variable is always encountered in discrete event simulations.

To make the discussion concrete, let's focus on the number of customers
in the queue, $Q(t)$. Notice the number of customers in the queue, $Q(t)$
takes on constant values during intervals of time corresponding to when
the queue has a certain number of customers.
$Q(t) = \{ 0,1,2,3,\ldots\}$. The values of $Q(t)$ form a step
function in this particular case. The following figure illustrates the
step function nature of this type of variable. A realization of the
values of variable is called a sample path.

\begin{figure}

{\centering \includegraphics[width=0.7\linewidth]{06-introDEDS_files/figure-latex/NCInQ-1} 

}

\caption{Number of Customers Waiting in Queue for the Bank Simulation}(\#fig:NCInQ)
\end{figure}
That is, for a given (realized) sample path, $Q(t)$ is a function that
returns the number of customers in the queue at time $t$. The mean value
theorem of calculus for integrals states that given a function,
$f( \bullet )$, continuous on an interval $(a, b)$, there exists a
constant, c, such that

$$\int_{a}^{b}{f\left( x \right)\text{dx}} = f(c)(b - a)$$

$$f\left( c \right) = \frac{\int_{a}^{b}{f\left( x \right)\text{dx}}}{(b - a)}$$

The value, $f(c)$, is called the mean value of the function. A similar
function can be defined for $Q(t)$ This function is called the
time-average (and represents the *mean value* of the $Q(t)$
function):

$$\overline{Q}\left( n \right) = \frac{\int_{t_{0}}^{t_{n}}{Q\left( t \right)\text{dt}}}{t_{n} - t_{0}}$$

This function represents the *average with respect to time* of the given
state variable. This type of statistical variable is called time-based
because $Q(t)$ is a function of time.

In the particular case where $Q(t)$ represents the number of customers
in the queue, $Q(t)$ will take on constant values during intervals of
time corresponding to when the queue has a certain number of customers. Let $Q\left( t \right) = \ q_{k}\ $for$\ t_{k - 1} \leq t \leq t_{k}$. Then, the time-average can be rewritten as follows:

$$\overline{Q}\left( t \right) = \frac{\int_{t_{0}}^{t_{n}}{Q\left( t \right)\text{dt}}}{t_{n} - t_{0}} = \sum_{k = 1}^{n}\frac{q_{k}(t_{k} - t_{k - 1})}{t_{n} - t_{0}}$$
Note that $q_{k}(t_{k} - t_{k - 1})$ is the area under the curve, $Q\left( t \right)$ over the interval $t_{k - 1} \leq t \leq t_{k}$ and because $$t_{n} - t_{0} = \left( t_{1} - t_{0} \right) + \left( t_{2} - t_{1} \right) + \left( t_{3} - t_{2} \right) + \ \cdots + \left( t_{n - 1} - t_{n - 2} \right) + \ \left( t_{n} - t_{n - 1} \right)$$
we can write, 
$$t_{n} - t_{0} = \sum_{k = 1}^{n}{t_{k} - t_{k - 1}}$$

The quantity $t_{n} - t_{0}$ represents the total time over which the variable is observed. Thus, the time average is simply the area under the curve divided by the amount of time
over which the curve is observed. From this equation, it should be noted
that each value of $q_{k}$ is weighted by the length of time that the
variable has the value. If we define, $w_{k} = (t_{k} - t_{k - 1})$,
then we can re-write the time average as:

$$\overline{Q}\left( t \right) = \frac{\int_{t_{0}}^{t_{n}}{Q\left( t \right)\text{dt}}}{t_{n} - t_{0}} = \sum_{k = 1}^{n}\frac{q_{k}(t_{k} - t_{k - 1})}{t_{n} - t_{0}} = \frac{\sum_{k = 1}^{n}{q_{k}w_{k}}}{\sum_{i = 1}^{n}w_{k}}$$

This form of the equation clearly shows that each value of $q_{k}$ is
weighted by:

$$\frac{w_{k}}{\sum_{i = 1}^{n}w_{k}} = \frac{w_{k}}{t_{n} - t_{0}} = \frac{(t_{k} - t_{k - 1})}{t_{n} - t_{0}}$$

This is why the time average is often called the time-weighted average.
If $w_{k} = 1$, then the time-weighted average is the same as the sample
average.

Now we can compute the time average for
$Q\left( t \right),\ N\left( t \right)$ and $B(t)$. Using the following
formula and noting that $t_{n} - t_{0} = 31$

$$\overline{Q}\left( t \right) = \sum_{k = 1}^{n}\frac{q_{k}(t_{k} - t_{k - 1})}{t_{n} - t_{0}}$$

We have that the numerator computes as follows:

$$\sum_{k = 1}^{n}{q_{k}\left( t_{k} - t_{k - 1} \right)} = 0\left( 13 - 0 \right) + \ 1\left( 14 - 13 \right) + \ 2\left( 15 - 14 \right) + \ 1\left( 17 - 15 \right) + \ 2\left( 19 - 17 \right)$$

$$\  + 1\left( 19 - 19 \right) + \ 2\left( 21 - 19 \right) + \ 3\left( 22 - 21 \right) + \ 2\left( 24 - 22 \right) + \ $$

$$1\left( 27 - 24 \right) + \ 2\left( 28 - 27 \right) + \ 1\left( 31 - 28 \right) = 28$$

And, the final time-weighted average number in the queue ss:

$$\overline{Q}\left( t \right) = \frac{28}{31} \cong 0.903$$

The average number in the system and the average number of busy tellers
can also be computed in a similar manner, resulting in:

$$\overline{N}\left( t \right) = \frac{52}{31} \cong 1.677$$

$$\overline{B}\left( t \right) = \frac{24}{31} \cong 0.7742$$

The value of $\overline{B}\left( t \right)$ is most interesting for this
situation. Because there is only 1 teller, the fraction of the tellers
that are busy is 0.7742. This quantity represents the *utilization* of
the teller. The utilization of a resource represents the proportion of
time that the resource is busy. Let c represent the number of units of a
resource that are available. Then the utilization of the
resource is defined as:

$$\overline{U} = \frac{\overline{B}\left( t \right)}{c} = \frac{\int_{t_{0}}^{t_{n}}{B\left( t \right)\text{dt}}}{c(t_{n} - t_{0})}$$

Notice that the numerator of this equation is simply the total time that
the resource is busy. So, we are computing the total time that the
resource is busy divided by the total time that the resource could be
busy, $c(t_{n} - t_{0})$, which is considered the utilization.

In this simple example, when an arrival occurs, you must determine
whether or not the customer will enter service or wait in the queue.
When a customer departs the system, whether or not the server will
become idle must be determined, by checking the queue. If the queue is
empty, then the server becomes idle; otherwise the next customer in the
queue begins service. In order to develop a computer simulation model of
this situation, the actions that occur at an event must be represented
within code. For example, the pseudo-code for the arrival event would
be:

*Pseudo-code for Arrival Event*

1.  schedule arrival time of next customer

    1.  AT = generate time to next arrival according to inter-arrival
        distribution

    2.  schedule arrival event at, t + AT

2.  check status of the servers (idle or busy)

    1.  if idle, do

        1.  allocate a server to the customer

        2.  ST = generate service time according to the service time
            distribution

        3.  schedule departure event at, t + ST

    2.  if busy, do

        1.  increase number in queue by 1 (place customer in queue)

*Pseudo-code for Departure Event*

1.  if queue is not empty

    1.  remove next customer from the queue

    2.  allocate the server to the next customer in the queue

    3.  ST = generate service time according to the service time
        distribution

    4.  schedule departure event at, t + ST

2.  else the queue is empty

    1.  make the customer's server idle

Notice how the arrival event schedules the next arrival and the
departure event may schedule the next departure event (provided the
queue is not empty). Within the JSL, this pseudo-code must be
represented in a class method that will be called at the appropriate
event time.

To summarize, discrete-event modeling requires two key abilities:

-   The ability to represent the actions associated with an event in
    computer code (i.e. in a class method).

-   The ability to schedule events so that they will be called at the
    appropriate event time, i.e. the ability to have the event's logic
    called at the appropriate event time.

Thus, the scheduling and execution of events is critical to
discrete-event modeling. The scheduling of additional events within an
event is what makes the system progress through time (jumping from event
to event). The JSL supports the scheduling and execution of events
within its calendar and model packages. The next section overviews the
libraries available within the JSL for discrete-event modeling.

## Modeling DEDS in the JSL {#introDEDS:dedsJSL}

Discrete event modeling within the JSL is facilitated by two packages:
1) the `jsl.simulation` package and 2) the `jsl.calendar` package. The `jsl.simulation`
package has classes that implement behavior associated with simulation
events and models.  The `jsl.calendar` package has classes that facilitate the
scheduling and processing of events.

### Event Scheduling

The following classes within the `jsl.simulation` package work together to
provide for the scheduling and execution of events:

-   `Simulation` - An instance of Simulation holds the model and
    facilitates the running of the simulation according to run
    parameters such as simulation run length and number of replications.

-   `Executive` - The Executive controls the execution of the events and
    works with the calendar package to ensure that events are executed
    and the appropriate model logic is called at the appropriate event
    time. This class is responsible for placing the events on a
    calendar, allowing events to be canceled, and executing the events
    in the correct order.

-   `JSLEvent` -- This class represents a simulation event. The
    attributes of JSLEvent provide information about the name, time,
    priority, and type of the event. The user can also check whether or
    not the event is canceled or if it has been scheduled. In addition,
    a general attribute of type Object is associated with an event and
    can be used to pass information along with the event.

-   `ModelElement` -- This class serves as a base class for all classes
    that are within a simulation model. It provides access to the
    scheduler and ensures that model elements participate in common
    simulation actions (e.g. warm up, initialization, etc.).

-   `Model` - An instance of a Model is required to serve as the parent
    to any model elements within a simulation model. It is the top-level
    container for model elements.

-   `SchedulingElement` -- SchedulingElement is a sub-class of
    ModelElement and facilitates the scheduling of events within a
    simulation model. Classes that require significant event scheduling
    should sub-class from SchedulingElement.

Figure \@ref(fig:jslModeling) illustrates the relationships between the
classes `Model`, `ModelElement`, `SchedulingElement`, `Simulation`, and
`Executive.` The `ModelElement` class represents the primary building block
for JSL models. A `ModelElement` represents something (an element) that
can be placed within an instance of a `Model`. The `Model` class subclasses
`ModelElement`. Every `ModelElement` can contain many other instances of
`ModelElement`. As such, an instance of a `Model` can also contain model
elements. There can only be one instance of the `Model` class within the
simulation. It acts as the parent (container) for all other model
elements. Model elements in turn also hold other model elements.
Instances arranged in this pattern form an object hierarchy that
represents the simulation model. The instance of a Simulation holds
(references) the instance of the `Model` class. `Simulation` also references
the `Executive.` The instance of the `Executive` uses an instance of a class
that implements the `CalendarIfc` interface. The simulation also
references an instance of the `Experiment` class. The `Experiment` class
allows the specification and control of the run parameters for the
simulation. Every instance of a `ModelElement` must be a child of another
`ModelElement` or the `Model`. This implies that instances of `ModelElement`
have access to the main model, which has access to an instance of
`Simulation` and thus the instance of the `Executive`. Therefore sub-classes
of `ModelElement` have access to the `Executive` and can schedule events.

![(\#fig:jslModeling)jsl.simulation Package and Relationships](./figures/ch4/jslmodeling.png) 

The `SchedulingElement` class is a special kind of `ModelElement` that
facilitates the scheduling of events. `SchedulingElement` is an abstract
base class for building other classes that schedule events. Sub-classes
of the `SchedulingElement` class will have a set of methods that
facilitate the scheduling of events.

![(\#fig:SchedulingElement)SchedulingElement and its scheduling methods](./figures/ch4/SchedulingElement.png) 

Figure \@ref(fig:SchedulingElement) illustrates the protected methods of
the `SchedulingElement` class. Since these methods are protected,
sub-classes will have them available through inheritance. There are two
major types of scheduling methods: one for rescheduling already existing
events and another for scheduling new events. The scheduling of new
events results in the creation of a new `JSLEvent` and the placement of
the event on the event calendar via the `Executive.`

The following code listing illustrates the key method for
scheduling events within the class SchedulingElement. Notice that the instance of the `Executive` is called via `getExecutive()`. In addition, the user can supply information for the
creation of an event such as its time, name, and priority. The user can
also provide an instance of classes that implement the `EventActionIfc`
interface. This interface promises to have an `action()` method. Within
the `action()` method the user can provide the code that is necessary to
represent the state changes associated with the event.

```java
/** Creates an event and schedules it onto the event calendar.  This is the main scheduling method that
 * all other scheduling methods call.  The other methods are just convenience methods for this method.
 *
 * @param <T> the type associated with the attached message
 * @param action represents an ActionListener that will handle the change of state logic
 * @param time represents the inter-event time, i.e. the interval from the current time to when the
 *        event will need to occur
 * @param priority is used to influence the ordering of events
 * @param message is a generic Object that may represent data to be transmitted with the event
 * @param name the name of the event
 * @return a valid JSLEvent
 */
protected final <T> JSLEvent<T> scheduleEvent(EventActionIfc<T> action, double time, int priority, T message, String name) {
	JSLEvent<T> event = getExecutive().scheduleEvent(action, time, priority, message, name, this);
	return (event);
}
```

There are two ways that the user can
provide event action code: 1) provide a class that implements the
`EventActionIfc` interface and supply it when scheduling the event or 2)
by treating the `EventActionIfc` as a functional interface and using Java 8's functional method representation.  Providing an inner class that implements the `EventActionIfc` interface will be illustrated here.

### Simple Event Scheduling Examples

This section presents two simple examples to illustrate event
scheduling. The first example illustrates the scheduling of events using the `EventActionIfc` interface. The second example shows how to simulate a Poisson process and collect
simple statistics.

#### Implementing Event Actions Using the EventActionIfc Interface

In the first example, there will be two events scheduled with actions.
The time between the events will all be deterministic. The specifics of
the events are as follows:

1.  Event Action One: This event occurs only once at time 10.0 and schedules the Action Two event to occur 5.0 time units later. It also prints out a message.
2.  Event Action Two: This event prints out a message. Then, it schedules an action one event 15 time units into the future. It also reschedules itself to reoccur in 20 minutes.

The following code listing provides the code for this simple event
example. Let's walk carefully through the construction and execution of
this code. 

First, the class sub-classes from `SchedulingElement`. This
enables the class to have access to all the scheduling methods within
`SchedulingElement` and provides one method that needs to be overridden:
`initialize()`. Every `ModelElement` has an `initialize()`
method. The base class, ModelElement's `initialize()` method does nothing.
However, the `initialize()` method is *critical* to properly modeling
using instances of `ModelElement` within the JSL architecture. The purpose of the
`initialize()` method is to provide code that can occur once at the
beginning of each replication of the simulation, prior to the execution
of any events. Thus, the `initialize()` method is the perfect location to
schedule initial events onto the event calendar so that when the
replications associated with the simulation are executed initial events
will be on the calendar ready for execution. Notice that in this example
the `initialize()` method does two things:

1.  schedules the first event action one event at time 10.0 via the call:
    scheduleEvent(myEventActionOne, 10.0)
2.  schedules the first action two event at time 20.0 via the call:
    scheduleEvent(myEventActionTwo, 20.0)

```java
public class SchedulingEventExamples extends SchedulingElement {

    private final EventActionOne myEventActionOne;
    private final EventActionTwo myEventActionTwo;

    public SchedulingEventExamples(ModelElement parent) {
        this(parent, null);
    }

    public SchedulingEventExamples(ModelElement parent, String name) {
        super(parent, name);
        myEventActionOne = new EventActionOne();
        myEventActionTwo = new EventActionTwo();
    }

    @Override
    protected void initialize() {
        // schedule a type 1 event at time 10.0
        scheduleEvent(myEventActionOne, 10.0);
        // schedule an event that uses myEventAction for time 20.0
        scheduleEvent(myEventActionTwo, 20.0);
    }

    private class EventActionOne extends EventAction {
        @Override
        public void action(JSLEvent event) {
            System.out.println("EventActionOne at time : " + getTime());
        }
    }

    private class EventActionTwo extends EventAction {
        @Override
        public void action(JSLEvent jsle) {
            System.out.println("EventActionTwo at time : " + getTime());
            // schedule a type 1 event for time t + 15
            scheduleEvent(myEventActionOne, 15.0);
            // reschedule the EventAction event for t + 20
            rescheduleEvent(jsle, 20.0);
        }
    }

    public static void main(String[] args) {
        Simulation s = new Simulation("Scheduling Example");
        new SchedulingEventExamples(s.getModel());
        s.setLengthOfReplication(100.0);
        s.run();
    }
}
```
The call `scheduleEvent(myEventAction, 20.0)` schedules an event 20 time
units into the future where the event will be handled via the instance
of the class `EventActionTwo`, which implements the `EventActionIfc` interface.
The reference `myEventActionTwo` refers to an object of type `EventActionTwo`,
which is an instance of the inner classes defined within
`SchedulingEventExamples`. This variable is defined as as class attribute
on line 4 and an instance is created in the constructor on line 11. To
summarize, the `initialize()` method is used to schedule the initial occurances of the two types of events. The `initialize()` method occurs right before time 0.0. That is, it occurs right before the simulation clock starts.

Now, let us examine the actions that occur for the two types of events.  Within the `action()` method of `EventActionOne`, we see the following code:

```java
System.out.println("EventActionOne at time : " + getTime());
```
Here a simple message is printed that includes the simulation time via the inherited `getTime()` method of the `ModelElement` class. Thus, by implementing the `action()` method of the `EventActionIfc` interface, you can supply the logic that occurs when the event is executed by the simulation executive. In the implemented `EventActionTwo` class, a simple message is printed and event action one is scheduled. In addition, the `rescheduleEvent()` method is used to reschedule the event that was supplied as part of the `action()` method. Since the attributes of the event remain the same,
the event is "recycled" to occur at the new scheduled time.

The main method associated with the `SchedulingEventExamples` class
indicates how to create and run a simulation model. The first line of the main method creates an instance of a `Simulation`. The next line makes an instance of
`SchedulingEventExamples` and attaches it to the simulation model. The
method call, `s.getModel()` returns an instance of the `Model` class that is
associated with the instance of the `Simulation`. The next line sets up the
simulation to run for 100 time units and the last line tells the simulation to
begin executing via the `run()` method. The output of the code is as follows:

```
EventActionOne at time : 10.0
EventActionTwo at time : 20.0
EventActionOne at time : 35.0
EventActionTwo at time : 40.0
EventActionOne at time : 55.0
EventActionTwo at time : 60.0
EventActionOne at time : 75.0
EventActionTwo at time : 80.0
EventActionOne at time : 95.0
EventActionTwo at time : 100.0
```

Notice that event action one output occurs at time 10.0. This is due to the event that was scheduled within the `initialize()` method. Event action two occurs for the first time at time 20.0 and then every 20 time units. Notice that event action one occurs at time 35.0. This is due to the event being scheduled in the action method of event action two.

#### Overview of Simuation Run Context

When the simulation runs, much underlying code is executed. At this
stage it is not critically important to understand how this code works;
however, it is useful to understand, in a general sense, what is
happening. The following outlines the basic processes that are occurring
when `s.run()` occurs:

1.  Setup the simulation experiment

2.  For each replication of the simulation:

    a.  Initialize the replication

    b.  Initialize the executive and calendar

    c.  Initialize the model and all model elements

    d.  While there are events on the event calendar or the simulation
        is not stopped

        i.  Determine the next event to execute

        ii.  Update the current simulation time to the time of the next
            event

        iii.  Call the action() method of the instance of `EventActionIfc` that was attached to the next event.

        iv.  Execute the actions associated with the next event

    e.  Execute end of replication model logic

3.  Execute end of simulation experiment logic

Step 2(b) initializes the executive and calendar and ensures that there
are no events at the beginning of the simulation. It also resets the
simulation time to 0.0. Then, step 2(c) initializes the model. In this
step, the initialize() methods of all of the model elements are
executed. This is why it was important to implement the `initialize()`
method in the example and have it schedule the initial events. Then,
step 2(c) begins the execution of the events that were placed on the
calendar. In looking at the code listings, it is not possible to ascertain how the action() methods are actually invoked unless you
understand that during step 2(c) each scheduled event is removed from
the calendar and its associated action called. In the case of the event action one
and two events in the example, these actions are specified in the action() method of EventActionOne and EventActionTwo. After all the events in the
calendar are executed or the simulation is not otherwise stopped, the
replication is ended. Any clean up logic (such as statistical
collection) is executed at the end of the replication. Finally, after
all replications have been executed, any logic associated with ending
the simulation experiment is invoked. Thus, even though the code does
not directly call the event logic it is still invoked by the simulation
executive because the events are scheduled. Thus, if you schedule
events, you can be assured that the logic associated with the events
will be executed.

#### Simulating a Poisson Process

The second simple example illustrates how to simulate a Poisson process.
Recall that a Poisson process models the number of events that occur
within some time interval. For a Poisson process the time between events
is exponentially distributed with a mean that is the reciprocal of the
rate of occurrence for the events. For simplicity, this example
simulates a Poisson process with rate 1 arrival per unit time. Thus, the
mean time between events is 1.0 time unit.  In this case the action is very simple, incrementing a counter that is tracking the number of events that have
occurred.

The code for this example is as follows. 

```java
public class SimplePoissonProcess extends SchedulingElement {

    private final RandomVariable myTBE;
    private final Counter myCount;
    private final EventHandler myEventHandler;

    public SimplePoissonProcess(ModelElement parent) {
        this(parent, null);
    }

    public SimplePoissonProcess(ModelElement parent, String name) {
        super(parent, name);
        myTBE = new RandomVariable(this, new ExponentialRV(1.0));
        myCount = new Counter(this, "Counts events");
        myEventHandler = new EventHandler();
    }

    @Override
    protected void initialize() {
        scheduleEvent(myEventHandler, myTBE.getValue());
    }

    private class EventHandler extends EventAction {
        @Override
        public void action(JSLEvent evt) {
            myCount.increment();
            scheduleEvent(myEventHandler, myTBE.getValue());
        }
    }

    public static void main(String[] args) {
        Simulation s = new Simulation("Simple PP");
        new SimplePoissonProcess(s.getModel());
        s.setLengthOfReplication(20.0);
        s.setNumberOfReplications(50);
        s.run();
        SimulationReporter r = s.makeSimulationReporter();
        r.printAcrossReplicationSummaryStatistics();
        System.out.println("Done!");
    }
}
```

There are a few new elements of this code to
note. First, this example uses two new JSL model elements:
`RandomVariable` and `Counter`. A `RandomVariable` is a sub-class of
`ModelElement` that is used to represent random variables within a
simulation model. The `RandomVariable` class must be supplied an instance
of a class that implements the `RandomIfc` interface. Recall that
implementations of the `RandomIfc` interface have a `getValue()` method that
returns a random value and permit random number stream control. The
supplied stream control is important when utilized advanced simulation
statistical methods. For example, stream control is used to advance the
state of the underlying stream to the next substream at the end of every
replication of the model. This helps in synchronizing the use of random
numbers in certain types of experimental setups. 

A `Counter` is also a sub-class of `ModelElement` which facilitates the incrementing and
decrementing of a variable and the statistical collection of the
variable across replications. The value of the variable associated with
the instance of a `Counter` is automatically reset to 0.0 at the beginning
of each replication. Lines 2 and 3 within the constructor create the
instances of the `RandomVariable` and the `Counter`.

Since we are modeling a Poisson process, the `initialize()` method is used
to schedule the first event using the random variable that represents
the time between events. This occurs on the only line of the `initialize()` method. The event logic, found in the inner class `EventHandler`,
causes the counter to be incremented. Then, the next arrival is scheduled
to occur. Thus, it is very easy to model an arrival process using this
pattern. The last items to note are in the `main()` method of the class,
where the simulation is created and run. In setting up the simulation,
the run length is set to 20 time units and the number of
replications associated with the simulation is set to 50.

A replication represents a sample path of the simulation that starts and
ends under the same conditions. Thus, statistics collected on each
replication represent independent and identically distributed
observations of the simulation model's execution. In this example, there
will be 50 observations of the counter observed. Since we have a Poisson
process with rate 1 event per time unit and we are observing the process
for 20 time units, we should expect that about 20 events should occur on
average.

Right after the `run()` method is called, an instance of a `SimulationReporter` is created for the
simulation. A `SimulationReporter` has the ability to write out
statistical output associated with the simulation. The code uses the
`printAcrossReplicationSummaryStatistics()` method to write out a simple
summary report across the 50 replications for the `Counter`. Note that
using the `Counter` to count the events provided for automatically
collected statistics across the replications for the counter. As you can
see from the output, the average number of events is close to the
theoretically expected amount.

```
Across Replication Statistical Summary Report
Sat Dec 31 13:02:53 EST 2016
Simulation Results for Model: Simple PP_Model

Number of Replications: 50
Length of Warm up period: 0.0
Length of Replications: 20.0

-----------------------------------------------------------
Counters
-----------------------------------------------------------
Name                  Average       Std. Dev.    Count
-----------------------------------------------------------
Counts events        20.500000      4.870779     50.000000 
-----------------------------------------------------------
```

### Up and Down Component Example

This section further illustrates DEDS modeling with a component of a
system that is subject to random failures. The component has two states
UP and DOWN. The time until failure is random and governed by an
exponential distribution with a mean of 1.0 time units. This represents
the time that the component is in the UP state. Once the component
fails, the component goes into the DOWN state. The time that the
component spends in the DOWN state is governed by an exponential
distribution with a mean of 2.0 time units. In this model, we are
interested in estimating the proportion of time that the component is in
the UP state and tracking the number of failures over the running time
of the simulation. In addition, we are interested in measuring the cycle
length of the component. The cycle length is the time between entering
the UP state. The cycle length should be equal to the sum of the time
spent in the up and down states.  

![(\#fig:UpDownComponent)Up and Down Component Process](./figures/ch4/UpDownComponent.png) 

Figure \@ref(fig:UpDownComponent) illustrates the dynamics of the
component over time. 

The following steps are useful in developing this model:

1.  Conceptualize the system/objects and their states
2.  Determine the events and the actions associated with the events
3.  Determine how to represent the system and objects as ModelElements
4.  Determine how to initialize and run the model

The first step is to conceptualize how to model the system and the state
of the component. A model element, UpDownComponent, will be used to
model the component. To track the state of the component, it is
necessary to know whether or not the component is UP or DOWN. A variable
can be used to represent this state. However, since we need to estimate
the proportion of time that the component is in the UP state, a
`TimeWeighted` variable will be used. `TimeWeighted` is a sub-class of
`ModelElement` that facilitates the observation and statistical collection
of time-based variables. Time-bases variables, which are discussed
further in the next Chapter, are a type of variable that changes values
at particular instants of time. Time-based variables must have
time-weighted statistics collected. Time-weighted statistics weights the
value of the variable by the proportion of time that the variable is set
to a value. To collect statistics on the cycle length we can use a
`ResponseVariable`. `ResponseVariable` is a sub-class of `ModelElement` that
can take on values within a simulation model and allows easy statistical
observation of its values during the simulation run. This class provides
observation-based statistical collection. Further discussion of
observation-based statistics will be presented in the next Chapter.

Because this system is so simple the required performance measures can
be easily computed theoretically. According to renewal theory, the
probability of being in the UP state in the long run should be equal to:

$$P(UP) = \frac{\theta_{u}}{\theta_{u}+\theta_{d}} = \frac{1.0}{1.0+2.0}=0.\overline{33}$$

where $\theta_{u}$ is the mean of the up-time distribution and
$\theta_{d}$ is the mean of the down-time distribution. In addition, the
expected cycle length should be $\theta_{u}+\theta_{d} = 3.0$.

The `UpDownComponent` class extends the `SchedulingElement` class and has
object references to instances of `RandomVariable`, `TimeWeighted`,
`ResponseVariable`, and `Counter` classes. Within the constructor of
`UpDownComponent`, we need to create the instances of these objects for
use within the class, as shown in the following code fragment.

```java
public class UpDownComponent extends SchedulingElement {

    public static final int UP = 1;
    public static final int DOWN = 0;
    private RandomVariable myUpTime;
    private RandomVariable myDownTime;
    private TimeWeighted myState;
    private ResponseVariable myCycleLength;
    private Counter myCountFailures;
    private final UpChangeAction myUpChangeAction = new UpChangeAction();
    private final DownChangeAction myDownChangeAction = new DownChangeAction();
    private double myTimeLastUp;

    public UpDownComponent(ModelElement parent) {
        this(parent, null);
    }

    public UpDownComponent(ModelElement parent, String name) {
        super(parent, name);
        RVariableIfc utd = new ExponentialRV(1.0);
        RVariableIfc dtd = new ExponentialRV(2.0);
        myUpTime = new RandomVariable(this, utd, "up time");
        myDownTime = new RandomVariable(this, dtd, "down time");
        myState = new TimeWeighted(this, "state");
        myCycleLength = new ResponseVariable(this, "cycle length");
        myCountFailures = new Counter(this, "count failures");
    }
```

Lines 3 and 4 define two constants to represent
the up and down states. Lines 5-9 declare additional references
needed to represent the up and down time random variables and the
variables that need statistical collection (myState, myCycleLength, and
myCountFailures). Lines 10 and 11 define and create the event actions
associated with the end of the up-time and the end of the down time. The
variable `myTimeLastUp` is used to keep track of the time that the
component last changed into the UP state, which allows the cycle length
to be collected. In lines 20-23, the random variables for the up and
downtime are constructed using exponential distributions.

As show in the following code listing, the `initialize()` method sets up the component.The variable `myTimeLastUp` is set to 0.0 in order to assume that the last time the component was in the UP state started at time 0.0. Thus,
we are assuming that the component starts the simulation in the UP
state. Finally, in line 7 the initial event is scheduled to cause the
component to go down according to the uptime distribution. This is the
first event and then the component can start its regular up and down
pattern. In the action associated with the change to the UP state,
line 18 sets the state to UP. Line 22 schedules the time until the
component goes down. Line 16 causes statistics to be collected on the
value of the cycle length. The code `getTime()-myTimeLastUp` represents
the elapsed time since the value of `myTimeLastUp` was set (in line 20),
which represents the cycle length. The `DownChangeAction` is very similar.
Line 31 counts the number of failures (times that the component has gone
down). Line 32 sets the state of the component to DOWN and line 34
schedules when the component should next transition into the UP state.

```java
public void initialize() {
	// assume that the component starts in the UP state at time 0.0
	myTimeLastUp = 0.0;
	myState.setValue(UP);
	// schedule the time that it goes down
	scheduleEvent(myDownChangeAction, myUpTime.getValue());
	//schedule(myDownChangeAction).name("Down").in(myUpTime).units();
}

private class UpChangeAction extends EventAction {

	@Override
	public void action(JSLEvent event) {
		// this event action represents what happens when the component goes up
		// record the cycle length, the time btw up states
		myCycleLength.setValue(getTime() - myTimeLastUp);
		// component has just gone up, change its state value
		myState.setValue(UP);
		// record the time it went up
		myTimeLastUp = getTime();
		// schedule the down state change after the uptime
		scheduleEvent(myDownChangeAction, myUpTime.getValue());
	}
}

private class DownChangeAction extends EventAction {

	@Override
	public void action(JSLEvent event) {
		// component has just gone down, change its state value
		myCountFailures.increment();
		myState.setValue(DOWN);
		// schedule when it goes up afer the down time
		scheduleEvent(myUpChangeAction, myDownTime.getValue());
	}
}
```

The following listing presents the code to construct and execute the
simulation. This code is very similar to previously presented code for
running a simulation. In line 3, the simulation is constructed. Then, in
line 5, the model associated with the simulation is accessed. This model
is then used to construct an instance of the UpDownComponent in line 7.
Finally, lines 9-15 represent getting a SimulationReporter, running the
simulation, and causing output to be written to the console. 

```java
public static void main(String[] args) {
	// create the simulation
	Simulation s = new Simulation("UpDownComponent");
	s.turnOnDefaultEventTraceReport();
	s.turnOnLogReport();
	// get the model associated with the simulation
	Model m = s.getModel();
	// create the model element and attach it to the model
	UpDownComponent tv = new UpDownComponent(m);
	// make the simulation reporter
	SimulationReporter r = s.makeSimulationReporter();
	// set the running parameters of the simulation
	s.setNumberOfReplications(5);
	s.setLengthOfReplication(5000.0);
	// tell the simulation to run
	s.run();
	r.printAcrossReplicationSummaryStatistics();
}
```

The results for the average time in the up state and the cycle length are consistent
with the theoretically computed results.

    ---------------------------------------------------------
    Across Replication Statistical Summary Report
    Sat Dec 31 18:31:27 EST 2016
    Simulation Results for Model: UpDownComponent_Model

    Number of Replications: 30
    Length of Warm up period: 0.0
    Length of Replications: 5000.0
    ---------------------------------------------------------
    Response Variables
    ---------------------------------------------------------
    Name              Average       Std. Dev.       Count 
    ---------------------------------------------------------
    state             0.335537       0.005846       30.000000 
    cycle length      2.994080       0.040394       30.000000 
    ---------------------------------------------------------

    ---------------------------------------------------------
    Counters
    ---------------------------------------------------------
    Name             Average        Std. Dev.    Count 
    ---------------------------------------------------------
    count failures   1670.066667    22.996152    30.000000 
    ---------------------------------------------------------

This example only scratches the surface of what is possible. Imagine if
there were 20 components. We could easily create 20 of instances and add
them to the model. Even more interesting would be to define the state of
the system based on which components were in the up state. This would be
the beginning of modeling the reliability of a complex system. This type
of modeling can be achieved by making the individual model elements
(e.g. `UpDownComponent`) more reusable and allow the modeling objects to
interact in complex ways. More complex modeling will be the focus of the
next chapter.

### Modeling a Simple Queueing System {#introDEDS:pharmacy}

This example considers a small pharmacy that has a single line for
waiting customers and only one pharmacist. Assume that customers arrive
at a drive through pharmacy window according to a Poisson distribution
with a mean of 10 per hour. The time that it takes the pharmacist to
serve the customer is random and data has indicated that the time is
well modeled with an exponential distribution with a mean of 3 minutes.
Customers who arrive to the pharmacy are served in the order of arrival
and enough space is available within the parking area of the adjacent
grocery store to accommodate any waiting customers.

\begin{figure}
\includegraphics[width=0.8\linewidth,height=0.8\textheight]{./figures/ch4/ch4fig8} \caption{Drive Through Pharmacy}(\#fig:DriveThruPharmacy)
\end{figure}

The drive through pharmacy system can be conceptualized as a single
server waiting line system, where the server is the pharmacist. An
idealized representation of this system is shown in Figure \@ref(fig:DriveThruPharmacy). If the pharmacist is busy serving a
customer, then additional customers will wait in line. In such a
situation, management might be interested in how long customers wait in
line, before being served by the pharmacist. In addition, management
might want to predict if the number of waiting cars will be large.
Finally, they might want to estimate the utilization of the pharmacist
in order to ensure that he or she is not too busy.

When modeling the system first question to ask is: *What is the system*?
In this situation, the system is the pharmacist and the potential
customers as idealized in Figure \@ref(fig:DriveThruPharmacy). Now you should consider the entities of
the system. An entity is a conceptual thing of importance that flows
through a system potentially using the resources of the system.
Therefore, one of the first questions to ask when developing a model is:
*What are the entities*? In this situation, the entities are the
customers that need to use the pharmacy. This is because customers are
discrete things that enter the system, flow through the system, and then
depart the system.

Since entities often use things as they flow through the system, a
natural question is to ask: *What are the resources that are used by the
entities*? A resource is something that is used by the entities and that
may constrain the flow of the entities within the system. Another way to
think of resources is to think of the things that provide service in the
system. In this situation, the entities "use" the pharmacist in order to
get their medicine. Thus, the pharmacist is a resource.

Another useful conceptual modeling tool is the *activity diagram*. An
*activity* is an operation that takes time to complete. An activity is
associated with the state of an object over an interval of time.
Activities are defined by the occurrence of two events which represent
the activity's beginning time and ending time and mark the entrance and
exit of the state associated with the activity. An activity diagram is a
pictorial representation of the process (steps of activities) for an
entity and its interaction with resources while within the system. If
the entity is a temporary entity (i.e. it flows through the system) the
activity diagram is called an activity flow diagram. If the entity is
permanent (i.e. it remains in the system throughout its life) the
activity diagram is called an activity cycle diagram. The notation of an
activity diagram is very simple, and can be augmented as needed to
explain additional concepts:

Queues: shown as a circle with queue labeled inside

Activities: shown as a rectangle with appropriate label inside

Resources: shown as small circles with resource labeled inside

Lines/arcs: indicating flow (precedence ordering) for engagement of entities in
    activities or for obtaining resources. Dotted lines are used to
    indicate the seizing and releasing of resources.
    
zigzag lines: indicate the creation or destruction of entities

Activity diagrams are especially useful for illustrating how entities
interact with resources. In addition, activity diagrams help in finding
the events and in identifying some the state changes that must be
modeled. Activity diagrams are easy to build by hand and serve as a
useful communication mechanism. Since they have a simple set of symbols,
it is easy to use an activity diagram to communicate with people who
have little simulation background. Activity diagrams are an excellent
mechanism to document a conceptual model of the system before building
the model.

\begin{figure}
\includegraphics[width=0.8\linewidth,height=0.8\textheight]{./figures/ch4/ch4fig9} \caption{Activity Diagram of Drive through Pharmacy}(\#fig:DTPActivityDiagram)
\end{figure}

Figure \@ref(fig:DTPActivityDiagram) shows the activity diagram for the
pharmacy situation. The diagram describes the life of an entity within
the system. The zigzag lines at the top of the diagram indicate the
creation of an entity. Consider following the life of the customer
through the pharmacy. Following the direction of the arrows, the
customers are first created and then enter the queue. Notice that the
diagram clearly shows that there is a queue for the drive-through
customers. You should think of the entity flowing through the diagram.
As it flows through the queue, the customer attempts to start an
activity. In this case, the activity requires a resource. The pharmacist
is shown as a resource (circle) next to the rectangle that represents
the service activity.

The customer requires the resource in order to start its service
activity. This is indicated by the dashed arrow from the pharmacist
(resource) to the top of the service activity rectangle. If the customer
does not get the resource, they wait in the queue. Once they receive the
number of units of the resource requested, they proceed with the
activity. The activity represents a delay of time and in this case the
resource is used throughout the delay. After the activity is completed,
the customer releases the pharmacist (resource). This is indicated by
another dashed arrow, with the direction indicating that the units of the resource aare
being put back or released. After the customer completes its service
activity, the customer leaves the system. This is indicated with the
zigzag lines going to no-where and indicating that the object leaves the
system and is disposed The conceptual model of this system can be
summarized as follows:

System:   The system has a pharmacist that acts as a resource, customers that
    act as entities, and a queue to hold the waiting customers. The
    state of the system includes the number of customers in the system,
    in the queue, and in service.
    
Events:   Arrivals of customers to the system, which occur within an
    inter-event time that is exponentially distributed with a mean of 6
    minutes.
    
Activities:   The service time of the customers are exponentially distributed with
    a mean of 3 minutes.
    
Conditional delays:   A conditional delay occurs when an entity has to wait for a
    condition to occur in order to proceed. In this system, the customer
    may have to wait in a queue until the pharmacist becomes available.

With an activity diagram and pseudo-code such as this available to
represent a solid conceptual understanding of the system, you can begin
the model development process.

In the current example, pharmacy customers arrive according to a Poisson
process with a mean of $\lambda$ = 10 per hour. According to probability
theory, this implies that the time between arrivals is exponentially
distributed with a mean of (1/$\lambda$). Thus, for this situation, the
mean time between arrivals is 6 minutes.

$$\frac{1}{\lambda} = \frac{\text{1 hour}}{\text{10 customers}} \times \frac{\text{60 minutes}}{\text{1 hour}} = \frac{\text{6 minutes}}{\text{customers}}$$

Let's assume that the pharmacy is open 24 hours a day, 7 days a week. In
other words, it is always open. In addition, assume that the arrival
process does not vary with respect to time. Finally, assume that
management is interested in understanding the long term behavior of this
system in terms of the average waiting time of customers, the average
number of customers, and the utilization of the pharmacist.

To simulate this situation over time, you must specify how long to run
the model. Ideally, since management is interested in long run
performance, you should run the model for an infinite amount of time to
get long term performance; however, you probably don't want to wait that
long! For the sake of simplicity, assume that 10,000 hours of operation
is long enough.

The logic of this model follows very closely the discussion of the bank
teller example.  The following code listing presents the definition of the variables and their creation. 

```java
public class DriveThroughPharmacy extends SchedulingElement {

    private int myNumPharmacists;
    private Queue<QObject> myWaitingQ;
    private RandomIfc myServiceRS;
    private RandomIfc myArrivalRS;
    private RandomVariable myServiceRV;
    private RandomVariable myArrivalRV;
    private TimeWeighted myNumBusy;
    private TimeWeighted myNS;
    private ResponseVariable mySysTime;
    private ArrivalEventAction myArrivalEventAction;
    private EndServiceEventAction myEndServiceEventAction;
    private Counter myNumCustomers;

    public DriveThroughPharmacy(ModelElement parent) {
        this(parent, 1,
                new ExponentialRV(1.0), new ExponentialRV(0.5));
    }

    public DriveThroughPharmacy(ModelElement parent, int numServers) {
        this(parent, numServers, new ExponentialRV(1.0), new ExponentialRV(0.5));
    }

    public DriveThroughPharmacy(ModelElement parent, int numServers, RandomIfc ad, RandomIfc sd) {
        super(parent);
        setNumberOfPharmacists(numServers);
        setServiceRS(sd);
        setArrivalRS(ad);
        myWaitingQ = new Queue<>(this, "PharmacyQ");
        myNumBusy = new TimeWeighted(this, 0.0, "NumBusy");
        myNS = new TimeWeighted(this, 0.0, "# in System");
        mySysTime = new ResponseVariable(this, "System Time");
        myNumCustomers = new Counter(this, "Num Served");
        myArrivalEventAction = new ArrivalEventAction();
        myEndServiceEventAction = new EndServiceEventAction();
    }
```

The `RandomVariable` class is used to model the time between
arrivals and the service time random variables. The `TimeWeighted` class
is used to model the number of busy servers and the number of customers
in the system. A `ResponseVariable` is used to model the time spent in the
system. There are two events represented by implementing the
`EventActionIfc` interface to model the arrival event and the end of
service event. Finally, a new JSL class, the `Queue` class, is used to
model the waiting line within the system. The `Queue` class is a sub-class
of `ModelElement` that is able to hold instances of the class `QObject` and
will automatically collect statistics on the number in the queue and the
time spent in the queue. The following code listing shows the logic required to model the
arrivals to the pharmacy.

```java
    protected void initialize() {
        super.initialize();
        // start the arrivals
        scheduleEvent(myArrivalEventAction, myArrivalRV);
    }

    private class ArrivalEventAction extends EventAction {
        @Override
        public void action(JSLEvent event) {
            //	 schedule the next arrival
            scheduleEvent(myArrivalEventAction, myArrivalRV);
            enterSystem();
        }
    }

    private void enterSystem() {
        myNS.increment(); // new customer arrived
        QObject arrivingCustomer = new QObject(getTime());

        myWaitingQ.enqueue(arrivingCustomer); // enqueue the newly arriving customer
        if (myNumBusy.getValue() < myNumPharmacists) { // server available
            myNumBusy.increment(); // make server busy
            QObject customer = myWaitingQ.removeNext(); //remove the next customer
            // schedule end of service, include the customer as the event's message
            scheduleEvent(myEndServiceEventAction, myServiceRV, customer);
        }
    }
```

In line 4 the first arrival event is scheduled
within the `initialize()` method. The `ArrivalEventAction` implementation
schedules the next arrival using the time between arrival random
variable and calls a private method named `enterSystem()`. This method
handles the logic for when a customer enters the system. First, the
number in the system is incremented and then the customer is enqueued.
In line 17, an instance of a `QObject` is created and its creation time is
supplied as the current simulation time using `getTime()`. Then, the
instance of the `Queue` class, `myWaitingQ`, is used to place the customer
in the queue using the `enqueue()` method. Note that even if the customer
receives immediate service, we still need to place the customer in the
queue because we need to correctly record that there was a zero wait
time. In lines 19-24, the number of busy servers is checked againts the
number of pharmacists. If a server is available, then the server is made
busy, the customer is removed from the queue, and the end of service for
the customer is scheduled. The scheduling of the end of service is
particularly important to note. In line 23, the reference to the `QObject`
is supplied when calling the `scheduleEvent()` method. This method has
signature:

```java
    /** Creates an event and schedules it onto the event calendar
     * @param <T> the type associated with the attached message
     * @param action represents an ActionListener that will handle the change of state logic
     * @param time represents the inter-event time, i.e. the interval from the current time to when the
     *        event will need to occur
     * @param message is a generic Object that may represent data to be transmitted with the event
     * @return a valid JSLEvent
     */
    protected final <T> JSLEvent<T> scheduleEvent(EventActionIfc<T> action, GetValueIfc time, T message) {
        return (scheduleEvent(action, time.getValue(), JSLEvent.DEFAULT_PRIORITY, message, getName()));
    }
```

Notice that the last argument of the method is of type `T`, which is defined as a generic parameter for the method. Thus, using this method, any instance of any class can be supplied.
Notice also that the `GetValueIfc` interface is used to supply the time.
This is why just the name of the service time random variable can be
used. The method automatically called the `getValue()` method of the
argument in order to determine the time until the event's occurrence.

The following code fragment presents the logic associated with the end
of service event. 

```java
    private class EndServiceEventAction implements EventActionIfc<QObject> {

        @Override
        public void action(JSLEvent<QObject> event) {
            myNumBusy.decrement(); // customer is leaving server is freed
            if (!myWaitingQ.isEmpty()) { // queue is not empty
                QObject customer = myWaitingQ.removeNext(); //remove the next customer
                myNumBusy.increment(); // make server busy
                // schedule end of service
                scheduleEvent(myEndServiceEventAction, myServiceRV, customer);
            }
            departSystem(event.getMessage());
        }
    }

    private void departSystem(QObject departingCustomer) {
        mySysTime.setValue(getTime() - departingCustomer.getCreateTime());
        myNS.decrement(); // customer left system
        myNumCustomers.increment();
    }
```

First the number of busy servers is decremented
because the service is becoming idle. Then, the queue is
checked to see if it is not empty. If the queue is not empty, then the
next customer must be removed, the server made busy again
and the customer scheduled into service. Finally, the
private method, `departSystem()` is called. The argument to this method is
`event.getMessage()`. The `getMessage()` method of `JSLEvent` will
return the object that was scheduled with the event. Since the `JSLEvent` was provided a `QObject` for the generic type in the signature of the `action()` method, we do not need to cast this object to type `QObject.` The `departSystem()` method simply collects
statistics using the `ResponseVariable`, `mySysTime` and the `TimeWeighted`,
`myNS.` These objects represent the system time and the number in the
system, respectively.

The following method can be used to run the model based on a desired number of servers.

```java
    public static void runModel(int numServers) {
        Simulation sim = new Simulation("Drive Through Pharmacy");
        sim.setNumberOfReplications(30);
        sim.setLengthOfReplication(20000.0);
        sim.setLengthOfWarmUp(5000.0);
        // add DriveThroughPharmacy to the main model
        DriveThroughPharmacy dtp = new DriveThroughPharmacy(sim.getModel(), numServers);
        dtp.setArrivalRS(new ExponentialRV(6.0));
        dtp.setServiceRS(new ExponentialRV(3.0));

        sim.run();
        sim.printHalfWidthSummaryReport();
    }
```

The reports indicate that customers wait about 3 minutes on average in
the line. The utilization of the pharmacist is about 50%. This means
that about 50% of the time the pharmacist was busy. For this type of
system, this is probably not a bad utilization, considering that the
pharmacist probably has other in-store duties. The reports also indicate
that there was less than one customer on average waiting for service.

    Across Replication Statistical Summary Report
    Mon Jan 02 16:05:23 CST 2017
    Simulation Results for Model: Drive Through Pharmacy_Model


    Number of Replications: 30
    Length of Warm up period: 5000.0
    Length of Replications: 20000.0
    -------------------------------------------------------------------------------
    Response Variables
    -------------------------------------------------------------------------------
    Name                                  Average       Std. Dev.    Count 
    -------------------------------------------------------------------------------
    PharmacyQ : Number In Q              0.490898        0.065510       30.000000 
    PharmacyQ : Time In Q                2.960012        0.351724       30.000000 
    NumBusy                              0.495274        0.016848       30.000000 
    # in System                          0.986173        0.080988       30.000000 
    System Time                          5.950288        0.403270       30.000000 
    -------------------------------------------------------------------------------

This single server waiting line system is a very common situation in
practice. In fact, this exact situation has been studied mathematically
through a branch of operations research called queuing theory. For
specific modeling situations, formulas for the long term performance of
queuing systems can be derived. This particular pharmacy model happens
to be an example of an M/M/1 queuing model. The first M stands for
Markov arrivals, the second M stands for Markov service times, and the 1
represents a single server. Markov was a famous mathematician who
examined the exponential distribution and its properties. According to
queuing theory, the expected number of customer in queue, $L_q$, for the
M/M/1 model is:

$$
\begin{aligned}
\label{ch4:eq:mm1}
L_q & = \dfrac{\rho^2}{1 - \rho} \\
\rho & = \lambda/\mu \\
\lambda & = \text{arrival rate to queue} \\
\mu & = \text{service rate}
\end{aligned}
$$

In addition, the expected waiting time in queue is given by
$W_q = L_q/\lambda$. In the pharmacy model, $\lambda$ = 1/6, i.e. 1
customer every 6 minutes on average, and $\mu$ = 1/3, i.e. 1 customer
every 3 minutes on average. The quantity, $\rho$, is called the
utilization of the server. Using these values in the formulas for $L_q$
and $W_q$ results in:

$$\begin{aligned}
\rho & = 0.5 \\
L_q  & = \dfrac{0.5 \times 0.5}{1 - 0.5} = 0.5  \\
W_q & = \dfrac{0.5}{1/6} = 3 \: \text{minutes}\end{aligned}$$

In comparing these analytical results with the simulation results, you
can see that they match to within statistical error. Later in this text,
the analytical treatment of queues and the simulation of queues will be
developed. These analytical results are available for this special case
because the arrival and service distributions are exponential; however,
simple analytical results are not available for many common
distributions, e.g. lognormal. With simulation, you can easily estimate
the above quantities as well as many other performance measures of
interest for wide ranging queuing situations. For example, through
simulation you can easily estimate the chance that there are 3 or more
cars waiting.

## Summary {#introDEDS:Summary}

This chapter introduced how to model discrete event dynamic systems
using the JSL. The JSL facilitates the model building process, the model
running process, and the output analysis process.

The model elements covered included:

`Model`:   Used to hold all model elements. Automatically created by the
    Simulation class.
    
`ModelElement`:   Used as an abstract base class for creating new model elements for a
    simulation.
    
`SchedulingElement`:   Used as an abstract base class for creating new model elements for a simulation that need to be able to schedule events.
    
`RandomVariable`:   A sub-class of ModelElement used to model randomness within a
    simulation.
    
`ResponseVariable`:   A sub-class of ModelElement used to collect statistics on
    observation-based variables.
    
`TimeWeighted`:   A sub-class of ModelElement used to collect statistics on
    time-weighted variables in the model.
    
`Counter`:   A sub-class of ModelElement used to count occurrences and collect
    statistics.
    
`Simulation`:   Used to create and control a simulation model.

`SimulationReporter`:   Used to gather and report statistics on a simulation model.

`JSLEvent`:   Used to model different events scheduled in time during a
    simulation.
    
`EventActionIfc`:   An interface used to define an action() method that represents event
    logic within the simulation.

The JSL has many other facets that have yet to be touched upon. Not only
does the JSL allow the modeler to build and analyze simulation models,
but it also facilitates data collection, statistical analysis, and
experimentation.

The next chapter will dive deeper into how to use the JSL to model
discrete-event oriented simulation situations.
