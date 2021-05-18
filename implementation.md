# Third readers–writers problem

### **Pseudocode**
```cpp
int reader_count = 0; // stores the number of readers who are reading
semaphore reader_mutex; // initially 1 and ensures that reader_count is changed with proper synchronization
semaphore sequence; // initially 1 and maintaines the sequence in which reader and writer arrives
semaphore access_mutex; // initially 1 and gives the access to reader/writer
// Reader's part
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

// Writer's part
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