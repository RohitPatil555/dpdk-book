
# Threading Model

In DPDK, processing/worker thread call lcore and those are created equal to number of CPU.
There is a max limit for lcore that is configure during compilation time.
```
meson configure -Dmax_lcores=<number of core>
```
let say, system have 32 core and max is set to 16. Then DPDK create only 16 lcore thread.

Now, lets look at lcore threading behaviour with configuration.

#### Code

```
#include <stdio.h>
#include <rte_eal.h>
#include <rte_debug.h>
#include <rte_cycles.h>

int main(int argc, char **argv) {
  int ret = -1;

  ret = rte_eal_init(argc, argv);
  if (ret < 0)
    rte_panic("Fail to init EAL\n");

  printf("Initialization Completed ! \n");

  while(1)
    rte_delay_ms(1000);

  return 0;
}

```

#### Compile

```
gcc -o test test.c -lrte_eal
```

#### Try Out

Let try multiple option and see its effect.

##### Command

Without any configuration on lcore

```
./test --no-huge &
```

##### Output

```
# pstree -p 4860 -t   
test(4860)-+-{eal-intr-thread}(4861)
           |-{lcore-worker-1}(4863)
           |-{lcore-worker-2}(4864)
           |-{lcore-worker-3}(4865)
           |-{rte_mp_handle}(4862)
           `-{test}(4866)
```

I have machine with 4 core, we can see all core get use (using taskset command).
Here it found that core 0 is use for all generic operation e.g. eal-intr-thread, rte_mp_handler, test.
And Core 1,2, and 3 is use for packet processin (lcore threads) lcore-worker-1, lcore-worker-2, and lcore-worker-3.

##### Command

Put limit on lcore, let use core 1 and 2 for lcore.
Please execute "killall test" before running below command.

```
./test -l 1-2 --no-huge &
```

##### Output

```
# pstree -tp 5457
test(5457)-+-{eal-intr-thread}(5458)
           |-{lcore-worker-2}(5460)
           |-{rte_mp_handle}(5459)
           `-{test}(5461)
```

If we look at output we can see only one lcore-worker-2 is running. But we provided two core "-l 1-2".
Why this is happening ? It is very easy, in DPDK main thread also act as lcore. And its name kept same as process name.

If we do taskset command exercise, it show us main and lcore-worker-2 thread are using core 1 and 2.
And other utilities eal-intr-thread, rte_mp_handle, and 5461-test running on non-lcore CPU (core-0 and 3).

#### Conclusion

If we use lcore option then DPDK try to use other core to run utitlity task.

In this way, DPDK make sure to run single processing thread on one dedicated core/cpu to get full computation power.
