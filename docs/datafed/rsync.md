# Rsync

Rsync is a standard tool for data transfer, and widely used for small volumes of data.



## Transfer rates


| | Durham | Edinburgh | Cambridge | RAL |
| --- | --- | --- | --- | --- |
| Durham | | 1150MB/s | 491MB/s | 71MB/s |
| Edinburgh | 1143MB/s | | 585MB/s | 94MB/s |
| Cambridge | 331MB/s | 566MB/s | | 52MB/s |
| RAL | 99MB/s | 128MB/s | 72.8MB/s | |

(from location in column to location in row).

Note that the tests to and from RAL (JASMIN) were performed using a small dataset and may not be representative of the performance with larger datasets - see below for details.

## Transfer details and parameters

### Durham to TURSA (Edinburgh)

The command used is:

```
echo TOTP_CODE | rsync -av -e "ssh -i /path/to/ssh/key" tmp.tar TURSAUSER@tursa.dirac.ed.ac.uk:
```

#### From login0a (with a 100Mbps connection):

5th November 2025

```
sent 986,506,495 bytes  received 35 bytes  73,074,557.78 bytes/sec
total size is 986,265,600  speedup is 1.00
```

#### From dataweb (40Gbps connectivity with bonded 4x 10G links over JANET):

5th November 2025

```
sent 986,506,495 bytes  received 35 bytes  78,920,522.40 bytes/sec
total size is 986,265,600  speedup is 1.00
```

### Between Durham, Cambridge and Edinburgh

These transfers were conducted using a 5TB data set (500x10GB files). A simple rsync command with no special parameters (other than `-v` to show progress) was issued on the login node of the source machine to initiate each transfer.

| Source | Destination | Transfer rate (MB/s) |
| ----------- | ----------- | ----------- |
| CSD3 | COSMA | 491.2 |
| COSMA | CSD3 | 331.0 |
| CSD3 | COSMA | 337.5 |
| COSMA | CSD3 | 311.1 |
| COSMA | ARCHER2 | 860.6 |
| ARCHER2 | COSMA | 1150 |
| COSMA | ARCHER2 | 1143 |
| ARCHER2 | COSMA | 1004 |
| ARCHER2 | CSD3 | 551.2 |
| CSD3 | ARCHER2 | 585.4 |
| ARCHER2 | CSD3 | 566.7 |
| CSD3 | ARCHER2 | 268.5 |

The transfer rate between Durham and Edinburgh was similar to the speed observed when using Globus. However, the other transfers were noticeably slower than Globus, with the Edinburgh/Cambridge transfers being about half the speed and the Durham/Cambridge ones around one third to one quarter of the speed.

### Between JASMIN and other sites

The transfers involving JASMIN were performed in a similar way, but used a much smaller (9x10GB files) dataset due to restricted disk space on JASMIN.

| Source | Destination | Transfer rate (MB/s) |
| ----------- | ----------- | ----------- |
| JASMIN | COSMA | 55.9 |
| COSMA | JASMIN | 99.5 |
| JASMIN | COSMA | 71.0 |
| COSMA | JASMIN | 93.5 |
| JASMIN | ARCHER2 | 94.3 |
| ARCHER2 | JASMIN | 119.7 |
| JASMIN | ARCHER2 | 57.4 |
| ARCHER2 | JASMIN | 128.8 |
| JASMIN | CSD3 | 47.2 |
| CSD3 | JASMIN | 72.8 |
| JASMIN | CSD3 | 52.6 |
| CSD3 | JASMIN | 46.0 |

These transfers were much slower than Globus, which was typically able to transfer this small dataset in or out of JASMIN at a rate of 200-800MB/s. This suggests that rsync may have a larger start up overhead than Globus and therefore perform more poorly on smaller transfers that do not run for long enough to effectively amortise this cost. To test this hypothesis, a transfer of the small 90GB dataset from COSMA to ARCHER2 was performed. This ran at a rate of 117.2MB/s, much slower than the roughly 1GB/s observed with the larger dataset between those systems.
