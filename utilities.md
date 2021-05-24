```cpp
struct processBlock{
    int* value;
    processBlock* next; // address of next block
}

// FIFO queue
struct queueFirstInFirstOut{
    processBlock* front, back;

    /* To remove a new element from the queue*/
    int* pop(){
        if(front == NULL) return -1; //underflow
        
        int* valueToRemove = front->value;
        front = front->next;
        if(front == NULL)
        {
            back = NULL;
        }
        
        return valueToRemove;
    }

    /* To add a new element in queue*/
    void push(int* val){
        processBlock* newBlock = new processBlock();
        newBlock.value = val;
        if(back == NULL){ //initializing the queue when there is no element.
            front = newBlock;
            rear = newBlock;
        }
        else{
            back.next = newBlock;
            back = newBlock;
        }
    }
    
}

struct semaphore{
    int semaphoreValue = 1; //initialized with 1
    queueFirstInFirstOut* queue = new queueFirstInFirstOut();
}

/*up function for the semaphore*/
void up(semaphore *mySemaphore){

    mySemaphore.semaphoreValue += 1; // increasing the value by 1
    if(mySemaphore.semaphoreValue <= 0){
        int* pid = mySemaphore.queue.pop();
        wake(pid); // using system call it will wake the process up
    }

}

/*down function for the semaphore*/
void down(semaphore mySemaphore, int pid){
    mySemaphore.semaphoreValue-=1; // decreasing the value by 1
    if(mySemaphore.semaphoreValue < 0){
        mySemaphore.queue.push(pid);
        sleepTheProcess(); // using system call block or sleep the process
    }
}
```
>**Note:** These are all the utilities function I will be using in *[implementation.md](./implementation.md)*