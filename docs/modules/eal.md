# EAL

DPDK have base framework that call as EAL (Environment Abstract Layer).
This layer help developer to abstract application dependency from underline OS.
It provide basic functionality like executable thread, memory management, etc.

For detail, please refer to [DPDK EAL](https://doc.dpdk.org/guides/prog_guide/env_abstraction_layer.html) document.

## Initialization

This layer get initialized by using below API call.

```
rte_eal_init(argc, argv)
```

## Example

Let take example to configure lcore in DPDK.
lcore will be cover in thread model section. As intro, please consider lcore as a thread that pin to a perticular CPU.
And also send --no-huge to disable huge pages for now, It will be cover in "hugepage" section.

* Code.

```
#include <stdio.h>
#include <rte_eal.h>
#include <rte_debug.h>

int main(int argc, char **argv) {
  int ret = -1;

  ret = rte_eal_init(argc, argv);
  if (ret < 0)
    rte_panic("Fail to init EAL\n");

  printf("Initialization Completed ! \n");

  return 0;
}

```

* Compile command.

```
gcc -o test test.c -lrte_eal
```

* Configure EAL

```
./build/test -l 0-1 --no-huge
```
