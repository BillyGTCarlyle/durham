# iPerf3 tests

In an attempt to better understand the performance of the network links between our sites, we ran various tests using [iPerf3](https://iperf.fr/). These tests involved starting up an iperf3 server on the data transfer node at Durham and then connecting to it from the other HPC systems to run 30 second transfer tests. Results are shown in the table below.

| Source | Destination | Bitrate (Gbits/s) |
| --- | --- | --- |
| CSD3 | COSMA | 8.66 |
| ARCHER2 | COSMA | 2.23 |
| JASMIN | COSMA | 1.41 |

Durham has 4x10Gb/s network links, so the transfer from Cambridge appears to be close to saturating one of these links. We also experimented with running multiple iperf3 tests into Durham simultaneously to see whether we could exploit more bandwidth in this way. Initially we performed two simultaneous tests between JASMIN and COSMA:

| Source | Destination | Bitrate (Mbits/s) |
| --- | --- | --- |
| JASMIN | COSMA | 1330 |
| JASMIN | COSMA | 680 |

In this case the first test reached almost the same bandwidth as the individual test described above, while the second test was significantly slower.

We then ran three simultaneous tests from different sites into Durham:

| Source | Destination | Bitrate (Mbits/s) |
| --- | --- | --- |
| CSD3 | COSMA | 1520 |
| ARCHER2 | COSMA | 595 |
| JASMIN | COSMA | 272 |

This time all three tests were dramatically slower than their equivalent individual test, and in fact even the total bandwidth measured by all three tests fell a long way short of just the individual Cambridge test. It is unknown whether this was simply due to busier conditions on the network at the time when the simultaneous tests were run.
