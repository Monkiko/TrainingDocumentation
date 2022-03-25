# Introduction to Sysstat and Recap

### Resources

- https://www.howtoforge.com/sysstat_monitoring_centos
- https://tecadmin.net/how-to-install-sysstat-on-ubuntu-20-04/
- https://www.thegeekstuff.com/2011/03/sar-examples/
- https://github.com/sysstat/sysstat
- https://github.com/rackerlabs/recap
<p><br>
<br>
</p>

## Introduction

Determining what is causing a system to have performance issues is a very complex and convoluted process with many points of failure or opportunities for bottlenecks. One method is to observe resource usage in real-time as issues/events occur and to monitor system connections through tools such as netstat/ss or tcpdump. While effective, the likelihood of identifying that an issue is occurring with a system and then having access quickly enough to gather information is very low. Also, once information is gathered, it can be difficult to determine if the resource usage and system activity is an anomaly or just business as usual.

Another approach is to gather readings throughout the day and then compare them one day to the next to look for patterns/trends and see if the readings correlate to the timeframe an issue/event occurred. This is where having a tool that automatically logs resource usage and other system activity to logs becomes useful in narrowing down possible issues.
<p><br>
<br>
</p>

## What is Sysstat and Recap?

[Recap](https://github.com/rackerlabs/recap) is a system status reporting tool. It generates various reports with information about resource usage, running processes, block device states, MySQL and InnoDB related information, and more in a more human readable format utilizing the information provided by the [sysstat](https://github.com/sysstat/sysstat) tool. It is useful for analyzing a system from a historical perspective as it generates reports every 10 minutes (by default) via cron jobs. Once the system clock hits midnight, a cron job is ran to gather all the reports throughout the day and pipe it all into a single file for each report.
<p><br>
</p>

### Example Recap Report

```
2022-03-24_12:00:01
UPTIME report
12:00:01 up 2 days, 11:33,  0 users,  load average: 0.00, 0.00, 0.00

FREE report
total        used        free      shared  buff/cache   available
Mem:        8148296      271624     6223508        6068     1653164     7563396
Swap:             0           0           0

VMSTAT report
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
0  0      0   6077     86   1528    0    0     1     2   11   20  0  0 100  0  0
0  0      0   6077     86   1528    0    0     0     0  112  100  0  0 99  0  1
0  0      0   6077     86   1528    0    0     0     0   35   73  0  0 100  0  0

IOSTAT report
Linux 5.4.0-104-generic (Monkiko-Tinkerlab)     03/24/22        _x86_64_        (4 CPU)

03/24/22 12:00:03
avg-cpu:  %user   %nice %system %iowait  %steal   %idle
0.04    0.00    0.04    0.01    0.01   99.90

Device            r/s     rkB/s   rrqm/s  %rrqm r_await rareq-sz     w/s     wkB/s   wrqm/s  %wrqm w_await wareq-sz     d/s     dkB/s   drqm/s  %drqm d_await dareq-sz  aqu-sz  %util
loop0            0.00      0.00     0.00   0.00    0.07     3.91    0.00      0.00     0.00   0.00    0.00     0.00    0.00      0.00     0.00   0.00    0.00     0.00    0.00   0.00
loop1            0.00      0.00     0.00   0.00    0.09     3.90    0.00      0.00     0.00   0.00    0.00     0.00    0.00      0.00     0.00   0.00    0.00     0.00    0.00   0.00
loop2            0.00      0.00     0.00   0.00    0.09     3.72    0.00      0.00     0.00   0.00    0.00     0.00    0.00      0.00     0.00   0.00    0.00     0.00    0.00   0.00
loop3            0.01      0.01     0.00   0.00    0.87     1.40    0.00      0.00     0.00   0.00    0.00     0.00    0.00      0.00     0.00   0.00    0.00     0.00    0.00   0.00
loop4            0.00      0.01     0.00   0.00    0.44    18.60    0.00      0.00     0.00   0.00    0.00     0.00    0.00      0.00     0.00   0.00    0.00     0.00    0.00   0.00
loop5            0.00      0.01     0.00   0.00    0.29    19.84    0.00      0.00     0.00   0.00    0.00     0.00    0.00      0.00     0.00   0.00    0.00     0.00    0.00   0.00
loop6            0.09      0.09     0.00   0.00    0.45     1.04    0.00      0.00     0.00   0.00    0.00     0.00    0.00      0.00     0.00   0.00    0.00     0.00    0.00   0.00
loop7            0.00      0.01     0.00   0.00    0.15    11.72    0.00      0.00     0.00   0.00    0.00     0.00    0.00      0.00     0.00   0.00    0.00     0.00    0.00   0.00
loop8            0.00      0.00     0.00   0.00    0.00     1.60    0.00      0.00     0.00   0.00    0.00     0.00    0.00      0.00     0.00   0.00    0.00     0.00    0.00   0.00
sda              0.00      0.03     0.00   0.00    0.34    21.64    0.00      0.00     0.00   0.00    0.38     0.50    0.00      0.00     0.00   0.00    0.00     0.00    0.00   0.00
sdb              0.00      0.03     0.00   0.00    0.11    22.52    0.00      0.00     0.00   0.00    0.00     0.00    0.00      0.00     0.00   0.00    0.00     0.00    0.00   0.00
vda              0.31      4.91     0.14  31.10    0.36    15.74    0.58      9.34     0.42  42.07    1.23    16.09    0.00      0.00     0.00   0.00    0.00     0.00    0.00   0.05

IOTOP report
12:00:05 Total DISK READ:         0.00 B/s | Total DISK WRITE:         0.00 B/s
12:00:05 Current DISK READ:       0.00 B/s | Current DISK WRITE:       0.00 B/s
TIME    TID  PRIO  USER     DISK READ  DISK WRITE  SWAPIN      IO    COMMAND
12:00:06 Total DISK READ:         0.00 B/s | Total DISK WRITE:        27.64 K/s
12:00:06 Current DISK READ:       0.00 B/s | Current DISK WRITE:      47.39 K/s
TIME    TID  PRIO  USER     DISK READ  DISK WRITE  SWAPIN      IO    COMMAND
12:00:06     311 be/3 root        0.00 B/s   27.64 K/s  0.00 %  0.08 % [jbd2/vda1-8]
12:00:07 Total DISK READ:         0.00 B/s | Total DISK WRITE:         0.00 B/s
12:00:07 Current DISK READ:       0.00 B/s | Current DISK WRITE:       0.00 B/s
TIME    TID  PRIO  USER     DISK READ  DISK WRITE  SWAPIN      IO    COMMAND


SAR report
Linux 5.4.0-104-generic (Monkiko-Tinkerlab)     03/24/22        _x86_64_        (4 CPU)

12:00:07        CPU     %user     %nice   %system   %iowait    %steal     %idle
12:00:07        all      0.04      0.00      0.04      0.01      0.01     99.90

Disk Utilization
Filesystem     1K-blocks    Used Available Use% Mounted on
udev             4056816       0   4056816   0% /dev
tmpfs             814832     988    813844   1% /run
/dev/vda1      162420480 4370380 158033716   3% /
tmpfs            4074148       0   4074148   0% /dev/shm
tmpfs               5120       4      5116   1% /run/lock
tmpfs            4074148       0   4074148   0% /sys/fs/cgroup
/dev/sda       103900088   61464  98579360   1% /mnt/volume_sfo3_05
/dev/loop1         56960   56960         0 100% /snap/core18/2344
/dev/loop0         56960   56960         0 100% /snap/core18/2284
/dev/loop2         63488   63488         0 100% /snap/core20/1361
/dev/vda15        106858    5321    101537   5% /boot/efi
/dev/loop3         63488   63488         0 100% /snap/core20/1376
/dev/loop4         69632   69632         0 100% /snap/lxd/22526
/dev/loop5         68864   68864         0 100% /snap/lxd/21835
/dev/loop6         44800   44800         0 100% /snap/snapd/15177
/dev/loop7         44672   44672         0 100% /snap/snapd/14978

Top 10 cpu using processes
12:00:07      UID       PID    %usr %system  %guest   %wait    %CPU   CPU  Command
Average:        0     79957    0.25    0.25    0.00    0.00    0.50     -  pidstat -l 2 2
Average:        0         1    0.25    0.00    0.00    0.00    0.25     -  /sbin/init

Top 10 memory using processes
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
mysql       3073  0.0  0.9 1776024 80708 ?       Ssl  Mar22   3:03 /usr/sbin/mysqld
root         835  0.0  0.4 1095024 39780 ?       Ssl  Mar22   0:12 /usr/lib/snapd/snapd
root         993  0.0  0.2 108104 20624 ?        Ssl  Mar22   0:00 /usr/bin/python3 /usr/share/unattended-upgrades/unattended-upgrade-shutdown --wait-for-signal
root        1058  0.0  0.2 195028 19732 ?        Ss   Mar22   0:11 /usr/sbin/apache2 -k start
root         825  0.0  0.2 196096 19304 ?        Ss   Mar22   0:14 php-fpm: master process (/etc/php/7.4/fpm/php-fpm.conf)
do-agent     811  0.0  0.2 1380872 18100 ?       Ssl  Mar22   0:42 /opt/digitalocean/bin/do-agent --syslog
root         592  0.0  0.2 280196 17996 ?        SLsl Mar22   0:23 /sbin/multipathd -d -s
root         823  0.0  0.2  29272 17996 ?        Ss   Mar22   0:00 /usr/bin/python3 /usr/bin/networkd-dispatcher --run-startup-triggers
root         390  0.0  0.1  35060 14736 ?        S<s  Mar22   0:03 /lib/systemd/systemd-journald
www-data   66249  0.0  0.1 195884 13340 ?        S    00:00   0:00 /usr/sbin/apache2 -k start
```
