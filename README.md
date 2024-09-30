Mofified : proc.h proc.c
Implement it by replacing the proc.h and proc.c in /usr/src/minix/kerne/ with my proc.h and proc.c and recompile kernel

1) Intheinit_proc()everyprocess’ticketNumsisinitiallysetas5
2) Toincreaseadd1ticketwhenblock,IaddsetProcPriority(1,p)atthedequeue
function. And to decrease 1 ticket when use all time quantum, I add a statement in enqueue function, if a process p_cycles > 0, which mean cycles this process used > 0, it used all time quantum before so call setProcPriority(-1, p) at enqueue
3) Wheninterrupthappened,itwillcalldequeue,Ihaveaforlooptoloopover every priority level, print out count of process at each level schedule queue.


In proc.h I add a variable usnsigned ticketNums to record the ticket number a process has.

In proc.c I modified
1) Init_proc() add a line to set initial 5 ticket for a process,also srandom() is called
inside this function
2) I add a setProcTicketfunction setProcTicket(intnum,structproc*p) num
indicates the number of ticket is going to assign to the process p, and can judge if
the process’ ticketNums add num will exceed 100 or be less then 1, if exceed 100, the process’ ticket will remain 100, if less then 1, the process’ ticket num will remain 1.
3) Inthedequeuefunction,IaddsetProcTicket(1,p)becausewhendequeueis called means a process is blocked, and whenever a process is blocked I will increase it’s ticket number.
4) Inpick_proc()Idothelotteryinthisfunction,theprocesswhowonthelottery, will be return when this function ends.
