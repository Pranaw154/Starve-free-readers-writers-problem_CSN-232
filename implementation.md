# Starve Free Readers–Writers Problem

### **Pseudocode**
```cpp
int reader_count = 0; // stores the number of readers who are reading
semaphore reader_mutex; // initially 1 and ensures that reader_count is changed with proper synchronization
semaphore sequence; // initially 1 and maintaines the sequence in which reader and writer arrives
semaphore access_mutex; // initially 1 and gives the access to reader/writer

// Readers part
void reader(){
  /* Entry section */
    down(sequence); // maintain the arrival sequence
    down(reader_mutex); // we need to change(increase) the value of reader_count

    if(reader_count==0) down(access_mutex); // if the current reader is the first one get the access of resources

    reader_count += 1; // increase the number of current readers
    up(sequence); // free the arrival sequence
    up(reader_mutex); // free the reader_mutex as we have incremented the reader_count
  /**/

  /* Critical section */
    read(); // reader perform whatever read operation required
  /**/

  /* Exit section*/
    down(reader_mutex); // we need to change(decrease) the value of reader_count
    reader_count -= 1; // decrease the number of current readers

    if(reader_count==0) up(access_mutex); // if current number of reader is 0 free the access of resources

    up(reader_mutex); // free the reader_mutex as we have incremented the reader_count
  /**/

}

// Writers part
void writer(){
  /* Entry section */
    down(sequence); // maintain the arrival sequence
    down(access_mutex); // get the access
    up(sequence); // free the arrival sequence
  /**/

  /* Critical section */
    write() // writer perform whatever write operation required
  /**/

  /* Exit section*/
    up(access_mutex); // free the access
  /**/

}
```
> **Note:** ***down*** is also called as ***P***, ***wait*** or ***sleep***. Meanwhile, ***up*** is also known as ***V***, ***signal*** or ***wake-up***.

## Explanation
As mentioned earlier in [README](./README.md), in this variant gives priority to the processes (reader or writer) on the basis of ***arrival*** of their requests. 

To implement this, I used three *semaphores*- 

- First is **sequence** which maintains the sequence in which readers and writers arrives in a queue. This semaphore will be called with the *down()* function as soon as a new process arrives and is freed when the process can access the resources. 

- Second semaphore that I used is **reader_mutex** which is used to monitor the **reader_count** variable. Whenever a reader tries to join or leave it needs to change the **reader_count** variable, so this **reader_mutex** semaphore ensures that no simultaneous access of **reader_count** variable is there to create synchronization issue. 

- Another semaphore that I used is **access_mutex** which writer will request before updating the data and release after updating it. Now, whenever the first reader arrives it must should take the permission to access the data, which it seeks by calling the **access_mutex** and when the last reader is leaving then it should free up the **access_mutex**. As we can see in the pseudocode (in Readers part) we deal with this **access_mutex** only when the first reader enters or last reader leaves. 

***In readers part***, in *entry section* **sequence** semaphore is checked and then we call the **reader_mutex** so that the variable **reader_count** can be updated (increased). Now we check if it is the first reader, if found so then we call the **access_mutex** semaphore to gain the access of critical section. After this we update the **reader_count** variable and then release the **sequence** semaphore followed by the **reader_mutex** semaphore so that the newer process can access it. After all of this, this reader enters into *critical section* and reads the required data. Then it goes in the *exit section* and calls the **reader_mutex** semaphore as it needs to update (decrease) the value of **reader_count** variable and then the value of **reader_count** is decremented. After this if there are no more readers then the **access_mutex** is freed. Then finally the **reader_mutex** semaphore is released.

***In writers part***, in *entry section* first of all the **sequence** semaphore is checked. After that the **access_mutex** is checked to make sure that there is no conflict of resources in critical section. Then we release the **sequence** semaphore so that other process can access it. After that the writer get into the *critical section* and does its work there. After its task is complete, it comes out in *exit section* and release the **access_mutex** semaphore

In this way, we can overcome the limitations of first and second readers writers problem by eliminating the scope of starvation.

### Verification
- As **access_mutex** semaphore ensures that there is no conflict of resources in critical section (i.e., not more than one writer is simultaneously present in the critical section as well as both writers and readers are not simultaneously present in the critical section), we can say that the above synchronization mechanism follows mutual exclusion.
- Since after each execution the required semaphores are released so that they can be accessed by other processes. And, there is no deadlock situations occuring here, so our synchronization mechanism follows the progress requirement rule.
- Since **sequence** semaphore makes sure that order of arrival of processes is preserved and we preserve it in a queue which ensures that each process will get a chance. This means that the mechanism follows the bounded waiting rule.

>**Note:** All the utilities I have used here are present in *[utilities.md](./utilities.md)*