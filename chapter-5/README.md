# Interlude: Process API

## The fork() system call
Is used create a new process

### Before fork
````
Process A (PID = 100)
|
|-- executing code...
|-- int x = 10;
|-- fork();   <-- about to happen
|-- printf("Hello\n");
````

### At the moment fork()
````
           fork()
             |
   ---------------------
   |                   |
Parent (PID=100)   Child (PID=101)
````

### After fork()
````
Parent (PID=100)         Child (PID=101)
------------------       ------------------
fork() returns 101       fork() returns 0

printf("Hello\n");       printf("Hello\n");
````

### That means, output becomes
````
Hello
Hello
````

## The wait() system call
Is used for parent process wait the children to finish first

### Without wait
````
Time →
----------------------------------------
Parent:  fork() ---- printf("Parent done")
Child:        |------ printf("Child done")
````

#### Output is unpredictable
````
Parent done
Child done
````
or
````
Child done
Parent done
````

### With wait()
````
Time →
----------------------------------------
Parent:  fork() -- wait() -------- printf("Parent done")
Child:        |------ work ---- exit()
````

#### Output guaranteed
````
Child done
Parent done
````

## The exec() system call
exec() system call is used to transform current process's memory with a new program.

````
int main() {
    int pid = fork();

    if (pid == 0) {
        // Child process
        printf("Child: about to run ls\n");
        execl("/bin/ls", "ls", NULL);

        // Only runs if exec fails
        perror("exec failed");
    } else {
        // Parent process
        printf("Parent: waiting for child\n");
        wait(NULL);
        printf("Parent: child finished\n");
    }

    return 0;
}
````