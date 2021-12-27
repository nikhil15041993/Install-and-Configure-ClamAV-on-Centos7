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
 
