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

[Recap](https://github.com/rackerlabs/recap) is a system status reporting tool. It generates various reports with information about resource usage, running processes, block device states, MySQL and InnoDB related information, and more in a more human readable format utilizing the information provided by the [sysstat](https://github.com/sysstat/sysstat) tool. It is useful for analyzing a system from a historical perspective as it generates reports every 10 minutes (by default) via cron jobs.
