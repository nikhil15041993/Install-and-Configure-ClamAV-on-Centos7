# Install-and-Configure-ClamAV-on-Centos7

ClamAV is an open source antivirus tool. Its basic usage is for detecting viruses, malware, and malicious software on Linux-based machines. The threat from viruses, Trojans, and other forms of malware is real. 

## Recommended System Requirements

Minimum recommended RAM for ClamAV:

FreeBSD and Linux server edition: 2 GiB+
Linux non-server edition: 2 GiB+
Windows 7 & 10 32-bit: 2 GiB+
Windows 7 & 10 64-bit: 3 GiB+
macOS: 3 GiB+
Minimum recommended CPU for ClamAV:

1 CPU at 2.0 Ghz+



## 1. Install ClamAV packages
To install ClamAV on CentOS 7, we need to install and enable EPEL repository.

```
yum install epel-release

yum -y install clamav-server clamav-data clamav-update clamav-filesystem clamav clamav-scanner-systemd clamav-devel clamav-lib clamav-server-systemd

freshclam

crontab -e
@hourly   /usr/local/bin/freshclam --quiet


groupadd clamav
useradd -g clamav -s /bin/false -c "Clam Antivirus" clamav


```
## FreshClam

Before you can start the ClamAV scanning engine (using either clamd or clamscan), you must first have ClamAV Virus Database (.cvd) file(s) installed in the appropriate location on your system.

The tool freshclam is used to download and update ClamAV’s official virus signature databases.

```
freshclam
```

## Add a service user account

If you're planning to run freshclam or clamd as a service on a Linux or Unix system, you should create a service account.

### Create a service user account (and group)
Linux / Unix
As root or with sudo, run:

```
groupadd clamav
useradd -g clamav -s /bin/false -c "Clam Antivirus" clamav
```

## Configure clamd.scan

 Edit the configs you need on /etc/clam.d/scan.conf
 
 ```
 Logfile /var/log/clamd.scan
 
 TCPScoket 3310
 
 TCPAddr 127.0.0.1
 
 ```
 
 ## Configure freshclam.conf
 
 open freshclam.conf on your text editor 
 
 ```
 vi /etc/freshclam.conf
 
 
 #Uncommand
 
 TestDatabase no
 ```
 
 ## One-Time Scanning
 
### ClamScan
clamscan is a command line tool which uses libclamav to scan files and/or directories for viruses.

```
clamscan [options] [file/directory/-]

```

```
--log=FILE - save scan report to FILE
--database=FILE/DIR - load virus database from FILE or load all supported db files from DIR
--official-db-only[=yes/no(*)] - only load official signatures
--max-filesize=#n - files larger than this will be skipped and assumed clean
--max-scansize=#n - the maximum amount of data to scan for each container file
--leave-temps[=yes/no(*)]- do not remove temporary files
--file-list=FILE - scan files from FILE
--quiet - only output error messages
--bell - sound bell on virus detection
--cross-fs[=yes(*)/no] - scan files and directories on other filesystems
--move=DIRECTORY - move infected files into DIRECTORY
--copy=DIRECTORY - copy infected files into DIRECTORY
--bytecode-timeout=N - set bytecode timeout (in milliseconds)
--heuristic-alerts[=yes(*)/no] - toggles heuristic alerts
--alert-encrypted[=yes/no(*)] - alert on encrypted archives and documents
--nocerts - disable authenticode certificate chain verification in PE files
--disable-cache - disable caching and cache checks for hash sums of scanned files

```

## On-Access Scanning

Purpose

This guide is for users interested in leveraging and understanding ClamAV's On-Access Scanning feature. It will walk through how to set up and use the On-Access Scanner and step through some common issues and their solutions

Requirements

On-Access is only available on Linux systems. On Linux, On-Access requires a kernel version >= 3.8. 

Some OS distributors have disabled fanotify, despite kernel support. You can check for fanotify support on your kernel by running the command:

```
cat /boot/config-<kernel_version> | grep FANOTIFY
You should see the following:


CONFIG_FANOTIFY=y
CONFIG_FANOTIFY_ACCESS_PERMISSIONS=y

```


## Configure clamd.scan

 Edit the configs you need on /etc/clam.d/scan.conf
 
 ```
    OnAccessExcludeUname clamav
    OnAccessPrevention  yes

    OnAccessIncludePath /home
    OnAccessExcludePath /home/user2
    OnAccessExcludePath /home/user4
```


