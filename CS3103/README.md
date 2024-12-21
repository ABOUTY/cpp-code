
# Parallel Matrix Multiplication

In this project, you will implement a parallel version of matrix multiplication, 
called `pmm`.

There are three specific objectives to this assignment:

* To familiarize yourself with the Linux pthreads package for threading.
* To learn how to parallelize a program.
* To learn how to program for performance.

## Background

To understand how to make progress on this project, you should first
understand the basics of thread creation, and perhaps locking and signaling
via mutex locks and condition variables. These are described in the following
book chapters:

- [Intro to Threads](http://pages.cs.wisc.edu/~remzi/OSTEP/threads-intro.pdf)
- [Threads API](http://pages.cs.wisc.edu/~remzi/OSTEP/threads-api.pdf)
- [Locks](http://pages.cs.wisc.edu/~remzi/OSTEP/threads-locks.pdf)
- [Using Locks](http://pages.cs.wisc.edu/~remzi/OSTEP/threads-locks-usage.pdf)
- [Condition Variables](http://pages.cs.wisc.edu/~remzi/OSTEP/threads-cv.pdf)

Read these chapters carefully in order to prepare yourself for this project.

## Overview

To learn how to perform a matrix multiplication or how to implement a parallel 
version of matrix multiplication, please refer to the project pdf file.

Your parallel matrix multiplication (`pmm`) will use POSIX threads (pthreads) to 
accelerate the matrix multiplication. It will take two files as inputs, and 
output a matrix product; the general usage from the command line will be as 
follows:

```Shell
[you@machine] ./pmm < matrix.txt > product.txt
```

## Considerations

Doing so effectively and with high performance will require you to address (at
least) the following issues:

- **How to parallelize the matrix multiplication.** Parallelizing the matrix 
    multiplication requires you to think about how to partition the matrix 
    multiplication, so that the computing resources can be fully used. Following 
    the mentioned two directions, you can design your own partition schemes. The 
    input matrix varies from different size, different dense/sparse degree. Your 
    schemes should take all those factors into consideration. 

- **How to determine the number of threads to create.** On Linux, the 
    determination of the number of threads may refer to some interfaces like 
    `get_nprocs()` and `get_nprocs_conf()`; You are suggested to read the man 
    pages for more details. Then, you are required to create an appropriate 
    number of threads to match the number of CPUs available on whichever system 
    your program is running.

- **How to balance the efficiency of different threads.** During your 
    implementations, you need to think about what works can be done in parallel, 
    and what works must be done serially by a single thread. For example, in the 
    second direction, each sub-matrix product can be computed in parallel, but 
    the sum of them needs to be done in serial. Another interesting issue is 
    that what happens if one thread runs much slower than another? Does the 
    partition scheme give less work to faster threads? This issue is often 
    referred to as the _straggler problem_.
    
    
    

## Grading

Your code should compile (and should be compiled) with the following flags:
`-Wall -Werror -pthread -O`. The last one is important: it turns on the
optimizer! In fact, for fun, try timing your code with and without `-O` and
marvel at the difference.

Your code will first be measured for correctness, ensuring that it zips input
files correctly.

If you pass the correctness tests, your code will be tested for performance;
higher performance will lead to better scores.

## Acknowledgements 

This project is taken (and slightly modified) from the [OSTEP projects](https://github.com/remzi-arpacidusseau/ostep-projects/blob/master/initial-utilities/README.md)
repository. 



```c

  
struct args {
	int start;
	int end;
};

pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;

void* multi(void* arg) {
	struct args* range = (struct args*)arg;
	for (int i = 0; i < MAX; i++) {
		for (int j = 0; j < MAX; j++) {
			int thread_private_tmp = 0;
			for (int k = range->start; k < range->end; k++) {
				thread_private_tmp += (int)matrix_a[i][k] * (int)matrix_b[k][j];
			}
			pthread_mutex_lock(&lock);
			matrix_c[i][j] += thread_private_tmp;
			pthread_mutex_unlock(&lock);
		}
	}
	pthread_exit(NULL);
}

int main(){
	int n, p, m, i, j, k;

	scanf("%d%d%d", &n, &p, &m);
	for (i = 0; i < n; i++) {
		for (j = 0; j < p; j++) {
			scanf("%hhd", &matrix_a[i][j]);
		}
	}
	for (i = 0; i < p; i++) {
		for (j = 0; j < m; j++) {
			scanf("%hhd", &matrix_b[i][j]);
		}
	}
	pthread_t child_threads[NUM_THREADS];
	struct args work_ranges[NUM_THREADS];
	int current_start, range;
	current_start = 0;
	range = p/NUM_THREADS;
	for (int i = 0; i < NUM_THREADS; i++) {
		work_ranges[i].start = current_start;
		work_ranges[i].end = current_start + range;
		current_start += range;
	}
	work_ranges[NUM_THREADS - 1].end = p;
	for (int i = 0; i < NUM_THREADS; i++) {
		pthread_create(&child_threads[i], NULL, multi, &work_ranges[i]);
	}
	for (int i = 0; i < NUM_THREADS; i++) {
		pthread_join(child_threads[i], NULL);
	}
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < m; j++){
      		printf("%d\t",matrix_c[i][j]);
		}
    	printf("\n");
	}	
	return 0;

}
```

