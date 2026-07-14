
Thread Priority :
-----------------
It is possible in java to assign priority to a Thread. Thread class has provided two predefined methods setPriority(int newPriority) and getPriority() to set and get the priority of the thread respectively.

public final void setPriority(int newPriority)
{
}
public final int getPriority()
{
   return this.priority;
}

In java we can set the priority of the Thread in numbers from 1- 10 only where 1 is the minimum priority and 10 is the maximum priority.

Whenever we create a thread in java by default its priority would be 5 that is normal priority.

The user-defined thread created as a part of main thread will acquire the same priority of main Thread.

Thread class has also provided 3 final static variables which are as follows :-

   public static final int MIN_PRIORITY = 1;
   public static final int NORM_PRIORITY = 5;
   public static final int MAX_PRIORITY = 10;

Note :- We can't set the priority of the Thread beyond the limit(1-10) so if we set the priority beyond the limit (1 to 10) then it will generate an exception java.lang.IllegalArgumentException.
---------------------------------------------------------------------
package com.ravi.priority;

public class PriorityDemo1
{
    public static void main(String[] args)
    {
        int priority = Thread.currentThread().getPriority();
        System.out.println("Main thread priority is :"+priority);
       
        Thread t1 = new Thread();
        System.out.println("Child Thread priority is :"+t1.getPriority());
    }

}
---------------------------------------------------------------------
package com.ravi.priority;

class MyThread implements Runnable
{    
    public void run()
    {
       int priority = Thread.currentThread().getPriority();
       System.out.println("Child Thread priority is :"+priority);
    }    
}

public class PriorityDemo2
{
    public static void main(String[] args)
    {
        Thread t = Thread.currentThread();
        t.setPriority(Thread.MAX_PRIORITY);
        //t.setPriority(11);  //java.lang.IllegalArgumentException
       
        System.out.println("Main Thread priority is :"+t.getPriority());
       
        Thread t1 = new Thread(new MyThread());
        t1.start();
    }

}
--------------------------------------------------------------------
package com.ravi.priority;

class UserThread implements Runnable
{
    @Override
    public void run()
    {
        int priority = Thread.currentThread().getPriority();
        String name = Thread.currentThread().getName();
       
        int count = 0;
       
        for(int i=1; i<1000000; i++)
        {
            count++;
        }
       
        System.out.println("The thread name is :"+name);
        System.out.println("The thread priority is :"+priority);
       
    }
   
}
public class PriorityDemo3
{
    public static void main(String[] args)
    {
        UserThread ut = new UserThread();
       
        Thread t1 = new Thread(ut, "Last_Thread");
        Thread t2 = new Thread(ut, "First_Thread");

        t1.setPriority(Thread.MIN_PRIORITY); //Lowest Priority
        t2.setPriority(Thread.MAX_PRIORITY); //Highest Priority
       
        t1.start(); t2.start();
       
    }

}
--------------------------------------------------------------------
package com.ravi.priority;

class UserThread implements Runnable
{
    @Override
    public void run()
    {
        int priority = Thread.currentThread().getPriority();
        String name = Thread.currentThread().getName();
       
        int count = 0;
       
        for(int i=1; i<1000000; i++)
        {
            count++;
        }
       
        System.out.println("The thread name is :"+name);
        System.out.println("The thread priority is :"+priority);
       
    }
   
}
public class PriorityDemo3
{
    public static void main(String[] args)
    {
        UserThread ut = new UserThread();
       
        Thread t1 = new Thread(ut, "Last_Thread");
        Thread t2 = new Thread(ut, "First_Thread");

        t1.setPriority(Thread.MIN_PRIORITY); //Lowest Priority
        t2.setPriority(Thread.MAX_PRIORITY); //Highest Priority
       
        t1.start(); t2.start();
       
    }

}

Most of time the thread having highest priority will complete its task but we can't say that it will always complete its task first that means Thread schedular dominates Priority of the Thread.
---------------------------------------------------------------------
Thread.yield() : [To Prevent a thread from over utilization of CPU]
----------------
It is a static method of Thread class.

The currently executing thread will send a notification to thread schedular to stop the currently executing Thread (In Running state) and provide a chance to Threads which are in Runnable state to enter inside the running state having same priority or higher priority than currently executing Thread.

Here The running Thread will directly move from Running state to Runnable state.

The Thread schedular may accept OR ignore this notification message given by currently executing Thread.

Here there is no guarantee that  after using yield() method the running Thread will move to Runnable state and the thread which is running thread will move to Runnable state.

If the thread which is in runnable state is having low priority than the current executing thread in Running state,Running thread will continue its execution.

*It is mainly used to avoid the over-utilisation a CPU by the current Thread so all the threads will get equal chance of CPU
for execution as well as In industry we can use testing and debugging purposes.

package com.ravi.thread;

class Test implements Runnable
{
    @Override
    public void run()
    {
        String name = Thread.currentThread().getName();
       
        for(int i=1; i<=10; i++)
        {
            System.out.println(i+" by "+name+" thread");
       
            if(name.equals("child1"))
            {
                Thread.yield();  //Give the chance to Child2
            }        
        }        
    }    
}
public class YieldDemo
{
    public static void main(String[] args)
    {
        Test test = new Test();
       
        Thread t1 = new Thread(test, "child1");
        Thread t2 = new Thread(test, "child2");
       
        t1.start();
        t2.start();
    }

}
--------------------------------------------------------------------
New Thread life Cycle :
-----------------------
A thread is well known for independent execution, During the life cycle of a thread it passes through different states

Java has provided a new Thread life cycle from JDK 1.5V with the support of enum called Thread.State.

We have a predefined enum called State which is available inside Thread class and it represents life cycle constants which are as follows :

1) NEW (Born State)
2) RUNNABLE (Ready to Run)
3) BLOCKED (Waiting for Object lock to enter OR reenter in syn area)
4) WAITING (Waiting for another thread to complete, join() wait())
5) TIMED_WAITING (Waiting for time period to complete, sleep(1000))
6) TERMINATED (Dead)

NEW :
-----
Whenever we create a thread instance(Thread Object) a thread comes to new state OR born state. New state does not mean that the Thread has started yet only the object or instance of Thread has been created.

RUNNABLE :
-----------
Whenever we call start() method on thread object, A thread moves to Runnable state i.e Ready to run state. Here the thread is considered "alive," but it doesn’t immediately start execution unless the CPU scheduler assigns it time.

BLOCKED :
---------
If a thread is waiting for object lock OR monitor to enter inside synchronized area OR re-enter inside synchronized area then it is in blocked state.

WAITING :
---------
A thread in the waiting state is waiting for another thread to
perform a particular action but WITHOUT ANY TIMEOUT period.
A thread that has called wait() method on an object is waiting for another thread to call notify() or notifyAll() on the same object OR A thread that has called join() method is waiting for a specified thread to terminate.

TIMED_WAITING :
---------------
A thread in the timed_waiting state, if we call any method which put the thread into temporarly timed_waiting state but WITH POSITIVE TIMEOUT period like sleep(lons ms), join(long ms), wait(long ms) then the Thread is considered as Timed_Waiting state.

TERMINATED :
-------------
The thread has successuflly completed it's execution in the separate stack memory that means run() method is successfully completed.

========================================================================
ThreadGroup :
-------------
It is a predefined class available in java.lang Package.

By using ThreadGroup class we can put 'n' number of threads into a single group to perform some common/different operation.

By using ThreadGroup class constructor, we can assign the name of group under which all the thread will be executed.

ThreadGroup tg = new ThreadGroup(String groupName);

ThreadGroup class has provided the following methods :

public String getName() : To get the name of the Group

pubic int activeCount() : How many threads are alive and running under that particular group.

Thread class has provided constructor to put the thread into particular group.

Thread t1 = new Thread(ThreadGroup tg, Runnable target, String name);

By using ThreadGroup class, multiple threads will be executed under single group.

WAP to assign different threads which are runing into a particular
group.

package com.ravi.thread_group;

class Target implements Runnable
{
    @Override
    public void run()
    {
        String name = Thread.currentThread().getName();    
        for(int i=1; i<=30; i++)
        {            
           System.out.println(name+" is running for "+i+" time");
        }
    }    
}

public class ThreadGroupDemo1
{
    public static void main(String[] args)
    {
       ThreadGroup tg = new ThreadGroup("Batch_42_43");    
       
       Thread t1 = new Thread(tg, new Target(), "Child1");
       Thread t2 = new Thread(tg, new Target(), "Child2");
       Thread t3 = new Thread(tg, new Target(), "Child3");
       Thread t4 = new Thread(tg, new Target(), "Child4");
       
       t1.start();
       t2.start();
       t3.start();
       t4.start();
       
       System.out.println("Group Name is :"+tg.getName());
       System.out.println("Total Running Threads under this group :"+tg.activeCount());

    }
}
---------------------------------------------------------------------
package com.ravi.thread_group;

class Tatkal implements Runnable
{
    @Override
    public void run()
    {
        String name = Thread.currentThread().getName();
        System.out.println(name+" has booked the Ticket under Tatkal Scheme");
    }
   
}
class PremimumTatkal implements Runnable
{
    @Override
    public void run()
    {
        String name = Thread.currentThread().getName();
        System.out.println(name+" has booked the Ticket under Premimum Tatkal Scheme");
       
    }    
}
public class ThreadGroupDemo2 {

    public static void main(String[] args)
    {
        ThreadGroup tatkal = new ThreadGroup("Tatkal");
        ThreadGroup Premimumtatkal = new ThreadGroup("Premimum_Tatkal");
       
        Thread t1 = new Thread(tatkal,new Tatkal(), "Scott");
        Thread t2 = new Thread(tatkal,new Tatkal(), "Smith");
       
        Thread t3 = new Thread(Premimumtatkal,new PremimumTatkal(), "Alen");
        Thread t4 = new Thread(Premimumtatkal,new PremimumTatkal(), "John");
       
        t1.start(); t2.start(); t3.start(); t4.start();
       

    }

}

Note : By using this approach we can assign differnt target for different threads by uisng ThreadGroup.
--------------------------------------------------------------------
package com.ravi.thread_group;

public class ThreadGroupDemo3
{
    public static void main(String[] args)
    {
        Thread t = Thread.currentThread();
        System.out.println(t.toString());
    }

}

Note : From the above program It is clear that main thread is running under main group.
---------------------------------------------------------------------
Daemon Thread [Service Level Thread]
------------------------------------
Daemon thread is a low- priority thread which is used to provide background maintenance.  

The main purpose of of Daemon thread to provide services to the user thread.              

JVM can't terminate the program till any of the non-daemon (user) thread is active, once all the user thread will be completed then JVM will automatically terminate all Daemon threads, which are running in the background to support user threads.

The example of Daemon thread is Garbage Collection thread, which is running in the background for memory management.

In order to make a thread as a Daemon thread as well as to verify whether a thread is daemon thread or not, Java software people has provided the following two methods

1) public final void setDaemon(boolean on) : If the boolean value
   is true the thread will work as a Daemon thread.
   
2) public final boolean isDaemon() : Will verify whether the
   thread is daemon thread OR user thread.
---------------------------------------------------------------------
public class DaemonThreadDemo1
{
  public static void main(String[] args)
    {
        System.out.println("Main Thread Started...");

        Thread daemonThread = new Thread(() ->
        {
            while (true)
            {
                System.out.println("Daemon Thread is running...");
                try
                {
                    Thread.sleep(1000);
                }
                catch (InterruptedException e)
                {
                    e.printStackTrace();
                }
            }
        });

        daemonThread.setDaemon(true);
        daemonThread.start();

       
        Thread userThread = new Thread(() ->
        {
            for (int i=1; i<=15; i++)
            {
                System.out.println("User Thread: " + i);
                try
                {
                    Thread.sleep(2000);
                }
                catch (InterruptedException e)
                {
                    e.printStackTrace();
                }
            }
        });

        userThread.start();

        System.out.println("Main Thread Ended...");
    }
}

Note : In the above program, first user thread will be completed then only JVM will automatically terminate the daemon thread.
---------------------------------------------------------------------
package com.ravi.thread_group;

public class IdDaemon {

    public static void main(String[] args)
    {
        Thread main = Thread.currentThread();
        System.out.println(main.isDaemon());

       
        Thread t1 = new Thread();
        t1.setDaemon(true);
        System.out.println(t1.isDaemon());
       
    }

}
-------------------------------------------------------------------
public void interrupt() Method of Thread class :
--------------------------------------------------
It is a predefined non static method of Thread class. The main purpose of this method to disturb the execution of the Thread, if the thread is in waiting or sleeping state.

Whenever a thread is interupted then it throws InterruptedException so the thread (if it is in sleeping or waiting mode) will get a chance to come out from a particular logic.

Points :-
---------
If we call interrupt method and if the thread is not in sleeping or waiting state then it will behave normally.

If we call interrupt method and if the thread is in sleeping or waiting state then we can stop the thread  gracefully.

*Overall interrupt method is mainly used to interrupt the
thread safely so we can manage the resources easily.

Methods :
---------
1) public void interrupt () :- Used to interrupt the Thread but the thread must be in sleeping or waiting mode.

2) public boolean isInterrupted() :- Used to verify whether thread is interrupted or not.

//Program :
------------
class Interrupt extends Thread
{
   @Override
   public void run()
    {
       Thread t = Thread.currentThread();
       System.out.println("Is thread interrupted : "+t.isInterrupted());
       
       for(int i=1; i<=5; i++)
        {
           System.out.println(i);  
           
                   try
           {
            Thread.sleep(1000);  //Java.lang.InterruptedException
                                 //interrupt flag value will become false
           }                    
           catch (InterruptedException e)
           {
               System.err.println(e);
           }
           
        }
    }
}
public class  InterruptThread
{
    public static void main(String[] args)
    {
        Interrupt it = new Interrupt();
        System.out.println("Thread State is "+it.getState());  //NEW
        it.start();
        it.interrupt();  //main thread is interrupting the child thread
                         //interrupt flag value will become true
    }
}

Note : In the above program main thread is interrupting child thread so interrupt flag value will become true, now once the child thread(in this program) will go into sleeping OR waiting state then child thread
will throw InterruptedException and interrupt flag value will become
false hence catch block would be executed only one time.
-----------------------------------------------------------------------
class Interrupt extends Thread
{
   @Override
   public void run()
    {
       Thread t = Thread.currentThread();
       System.out.println("Is thread interrupted : "+t.isInterrupted());
       
       for(int i=1; i<=5; i++)
        {
           System.out.println(i);  
           
           try
           {
            Thread.sleep(1000);  //Java.lang.InterruptedException
                                 //interrupt flag value will become false
           }                    
           catch (InterruptedException e)
           {
               System.err.println("Thread has Interrupted");
               Thread.currentThread().interrupt(); //interrupt flag value will become true
           }
           
        }
    }
}
public class  InterruptThread
{
    public static void main(String[] args)
    {
        Interrupt it = new Interrupt();
        System.out.println("Thread State is "+it.getState());  //NEW
        it.start();
        it.interrupt();  //main thread is interrupting the child thread
                         //interrupt flag value will become true
    }
}

Note : Here in the catch block, we are interrupting the thread once
       again so the interrupt flag which was false will again become
       true so the Thread will throw InterruptedException (if the thread will go into sleeping OR Waiting state)
----------------------------------------------------------------------   class Interrupt extends Thread
{
   @Override
   public void run()
    {
       try
       {
        Thread.currentThread().interrupt(); //self interruption

       for(int i=1; i<=10; i++)
        {
           System.out.println("i value is :"+i);
           Thread.sleep(1000);
        }

       }
        catch (InterruptedException e)
        {
            System.err.println("Thread is Interrupted :"+e);
        }
        System.out.println("Child thread completed...");
    }
}
public class  InterruptThread1
{
    public static void main(String[] args)
    {    
        Interrupt it = new Interrupt();
        it.start();    
    }
}

Note : catch block is not inside the for loop hence for loop will be
       executed only one time.
 ----------------------------------------------------------------------
 public class InterruptThread2
{
    public static void main(String[] args)
    {
        Thread thread = new Thread(new MyRunnable());
        thread.start();
     
        try
        {
          Thread.sleep(5000); //Main thread is waiting for 3 Sec
        }
        catch (InterruptedException e)
        {
            e.printStackTrace();
        }
       
       thread.interrupt();  
    }
}

class MyRunnable implements Runnable
{
    @Override
    public void run()
    {
        try
        {
            while (!Thread.currentThread().isInterrupted())
            {
                System.out.println("Thread is running by locking the resource");
                Thread.sleep(500);
            }
        }
        catch (Exception e)
        {
            System.out.println("Thread interrupted gracefully.");
        }
        finally
        {
            System.out.println("Thread resource can be release here.");
        }
    }
}
----------------------------------------------------------------------
Deadlock :
------------
It is a situation where two or more than two threads are in blocked state forever, here threads are waiting to acquire another thread resource without releasing it's own resource.

This situation happens when multiple threads demands same resource without releasing its own attached resource so as a result we get Deadlock situation and our execution of the program will go to an infinite state as shown in the diagram. (03-May-25)

public class DeadlockExample
    {
  public static void main(String[] args)
     {
     String resource1 = new String("Ameerpet");  //(L1)
     String resource2 = new String("S R Nagar"); //(L2)

    // t1 tries to lock resource1(L1) then resource2(L2)

    Thread t1 = new Thread()
     {
      @Override
      public void run()
          {
              synchronized (resource1)
                  {
               System.out.println("Thread 1: locked resource 1");
                  try
                   {
                   Thread.sleep(1000);
                   }
                   catch (Exception e)
                   {}                  
               
                //t1 thread is waiting here for Lock2
               synchronized (resource2) //Nested synchronized block
               {
                System.out.println("Thread 1: locked resource 2");
               }
             }
      }
    };


    // t2 tries to lock resource2 then resource1
    Thread t2 = new Thread()
    {
      @Override
      public void run()
      {
        synchronized (resource2)
            {
          System.out.println("Thread 2: locked resource 2");
              try
              {
              Thread.sleep(1000);
              }
              catch (Exception e)
              {}
           
            //t2 thread will wait for L1  (Resourse1)
          synchronized (resource1) //Nested synchronized block
          {
            System.out.println("Thread 2: locked resource 1");
          }
        }
      }
    };    
    t1.start();
    t2.start();
  }
}
---------------------------------------------------------------------
***In btween extends Thread and implements Runnable which one is better and Why ?

Ans : Both are the ways in java.lang package to create a Thread.
      In between extends Thread and implements Runnable, implements
      Runnable is more better approach due to the following reasons :
     
      a) In extends Thread we cannot extend any other class (MI is not supported by using class) but in implements Runnable we can extend only one class.
     
      b) extends Thread provides tight coupling approach so to create multiple threads we need multiple sub class object on the other
      hand implements Runnable provides loose coupling approach so for creating multiple threads we need only sub class object.

       Example :
       Case 1 :
       --------
       class Test extends Thread
       {
       }
       
       Test t1 = new Test();
       t1.start();
       Test t2 = new Test();
       t2.start();
       
       Case 2:
       -------
       class UserThread implements Runnable
       {
       }
       
       UserThread ut = new UserThread();
       
       Thread t1 = new Thread(ut);
       Thread t2 = new Thread(ut);
       
       3) By using extends Thread we can't use Lambda Expression but using implements we can use Lambda Expression
       
       4) A class extends a Thread then all the Thread class methdos are available in the sub class but if we use implements  Runnable then Thread class methos are not available.
     
----------------------------------------------------------------------
volatile Keyword in java : [Reading the value of the variable from
                            the main memory not from the cache memory]
-----------------------------------------------------------------------
While working in a multithreaded environment multiple threads can perform read and write operation with common variable/data (chances of Data inconsistency so use synchronized OR AtomicInteger) concurrently.

In order to store the value temporarly, Every thread is having local cache memory (PC Register) but if we declare a variable with volatile modifier then variable's value is not stored in a thread's local cache; it is always read from the main memory.

So the conclusion is, a volatile variable value is always read from and written directly to the main memory, which ensures that changes made by one thread are visible to all other threads immediately.

package com.ravi.testing_demo;

class SharedData
{
    private volatile boolean flag = false; //FLAG VALUE WILL COME
                                             FROM MAIN MEMORY
    public void startThread()
    {
        Thread writer = new Thread(() ->
        {
            try
            {
                System.out.println("Writer thread started");
                Thread.sleep(5000);  //Writer thread will go for 1 sec waiting state
                flag = true;
                System.out.println("Writer thread make the flag value as true");
            }
            catch (InterruptedException e)
            {
                e.printStackTrace();
            }
        });

       
        Thread reader = new Thread(() ->
        {
            while (!flag) //Cache memory still the value of flag is false
            {
               
            }
            System.out.println("Reader thread got the updated value");
        });

        writer.start();
        reader.start();
    }

}

public class VolatileExample
{    
    public static void main(String[] args)
    {
        new SharedData().startThread();
    }
}
-----------------------------------------------------------------------
