# Linux-IPC-Shared-memory
Ex06-Linux IPC-Shared-memory

# AIM:
To Write a C program that illustrates two processes communicating using shared memory.

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux Process API - Shared Memory

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

# DEVELOPED BY: V.SHRIYHA
# REG.NO: 212224230267

## Write a C program that illustrates two processes communicating using shared memory.
~~~
//shm.c

#include<unistd.h> 
#include<stdlib.h> 
#include<stdio.h> 
#include<string.h>
#include<sys/shm.h>
#define TEXT_SZ 2048 
struct shared_use_st{
    int written_by_you;
    char some_text[TEXT_SZ];
};
int main() {
    int running = 1;
    void *shared_memory = (void *)0; 
    struct shared_use_st *shared_stuff; 
    char buffer[BUFSIZ];
    int shmid;
    shmid = shmget((key_t)1234, sizeof(struct shared_use_st), 0666 | IPC_CREAT);
    printf("Shared memort id = %d \n", shmid);
    if (shmid == -1) {
        fprintf(stderr, "shmget failed\n"); 
        exit(EXIT_FAILURE);
    }
    shared_memory = shmat(shmid, (void *)0, 0);
    if (shared_memory == (void *)-1) {
        fprintf(stderr, "shmat failed\n"); 
        exit(EXIT_FAILURE);
    }
    printf("Memory Attached at %x\n", (int) shared_memory); 
    shared_stuff = (struct shared_use_st *)shared_memory; 
    while(running) {
        while(shared_stuff->written_by_you == 1) {
            sleep(1);
            printf("waiting for client.\n");
        }
        printf("Enter Some Text: "); 
        fgets(buffer, BUFSIZ, stdin);
        strncpy(shared_stuff->some_text, buffer, TEXT_SZ);
        shared_stuff->written_by_you = 1;
        if(strncmp(buffer, "end", 3) == 0) {
            running = 0;
        }
    }
    if (shmdt(shared_memory) == -1) {
        fprintf(stderr, "shmdt failed\n"); 
        exit(EXIT_FAILURE);
    } 
    exit(EXIT_SUCCESS);
}
~~~

## OUTPUT
<img width="917" height="228" alt="Screenshot 2025-10-04 130723" src="https://github.com/user-attachments/assets/4aaca302-11ee-4370-9871-fe7a7bb3e6f7" />
<img width="1033" height="667" alt="Screenshot 2025-10-04 130734" src="https://github.com/user-attachments/assets/035a220e-7f0f-47f8-a291-84fe401a4e6d" />


# RESULT:
The program is executed successfully.
