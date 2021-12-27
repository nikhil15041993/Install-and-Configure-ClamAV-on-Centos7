# Install-and-Configure-ClamAV-on-Centos7

ClamAV is an open source antivirus tool. Its basic usage is for detecting viruses, malware, and malicious software on Linux-based machines. The threat from viruses, Trojans, and other forms of malware is real. 

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


