# Linux Common Operations

> **A beginner-friendly guide to common Linux operations.**
>
> This document covers package management, service management, symbolic links, date and time, and time synchronization. All examples are based on **CentOS 7** unless otherwise specified.

---

# 0. Linux Terminal Essentials

Before learning Linux commands, it is helpful to become familiar with some commonly used terminal shortcuts and productivity tips. These shortcuts can greatly improve your efficiency when working in the command line.

---

## 0.1 Command History

### View Command History

```bash
history
```

Display all previously executed commands.

Example:

```text
100  ls
101  pwd
102  cd /home
103  vim test.txt
104  history
```

---

### Execute a Previous Command

Run command number **102** again.

```bash
!102
```

Run the last executed command.

```bash
!!
```

Run the most recent command beginning with **py(python)**.

```bash
!py
```

---

### Search Command History

Press:

```text
Ctrl + R
```

Then type a keyword.

Example:

```text
(reverse-i-search)`ssh':
```

Press **Enter** to execute the matched command.

---

### Clear History

```bash
history -c
```

---

## 0.2 Keyboard Shortcuts

| Shortcut | Description |
|----------|-------------|
| ↑ | Previous command |
| ↓ | Next command |
| Tab | Auto-complete commands or filenames |
| Tab Tab | Show all possible matches |
| Ctrl + R | Search command history |
| Ctrl + A | Move cursor to beginning of line |
| Ctrl + E | Move cursor to end of line |
| Ctrl + U | Delete from cursor to beginning |
| Ctrl + K | Delete from cursor to end |
| Ctrl + W | Delete previous word |
| Ctrl + Y | Paste deleted text |
| Ctrl + C | Terminate current command |
| Ctrl + Z | Suspend current process |
| Ctrl + L | Clear terminal screen |
| Ctrl + D | Exit the current shell |

---

## 0.3 Auto Completion

Press:

```text
Tab
```

Example:

```text
sys<Tab>
```

becomes

```text
systemctl
```

Press **Tab** twice to display all available matches.

---

## 0.4 Useful Tips

Repeat the previous command.

```bash
!!
```

Run the previous command with `sudo`.

```bash
sudo !!
```

Display the current shell.

```bash
echo $SHELL
```

---

## Summary

Mastering these shortcuts can significantly improve your efficiency when using the Linux terminal. They reduce repetitive typing, speed up navigation, and make command-line work much more productive.

---

# 1. View Command Manual (man)

## Purpose

The **man** command displays the official manual page for a Linux command.

It is the most important tool for learning Linux commands because it provides detailed explanations of:

- Command usage
- Available options
- Examples
- Description

---

## Syntax

```bash
man <command>
```

---

## Examples

```bash
man ls

man cp

man mkdir

man systemctl
```

---

## Common Usage

View the manual for the `ls` command.

```bash
man ls
```

Search for the **copy** command.

```bash
man cp
```

---

## Navigation

Inside the manual page:

| Key | Function |
|------|----------|
| ↑ / ↓ | Scroll one line |
| Page Up / Page Down | Scroll one page |
| Space | Next page |
| b | Previous page |
| /keyword | Search keyword |
| n | Next search result |
| N | Previous search result |
| q | Quit |

---

## Notes

- Every important Linux command has a manual page.
- If you don't know how a command works, always check `man` first.
- Some commands also provide a quick help page:

```bash
ls --help
```

---

# 2. Software Installation

Linux software is usually installed through the package manager.

Different Linux distributions use different package managers.

| Distribution | Package Manager |
|--------------|-----------------|
| CentOS | yum |
| Rocky Linux | yum / dnf |
| Fedora | dnf |
| Ubuntu | apt |
| Debian | apt |

---

# 2.1 yum (CentOS)

## Purpose

`yum` is the default package manager for CentOS.

It can:

- Install software
- Remove software
- Search software
- Update software

---

## Syntax

```bash
yum [option] package-name
```

---

## Common Options

| Option | Description |
|---------|-------------|
| install | Install software |
| remove | Uninstall software |
| search | Search packages |
| update | Update packages |
| -y | Automatically answer "Yes" |

---

## Examples

Install Git.

```bash
sudo yum install git
```

Install Nginx.

```bash
sudo yum install -y nginx
```

Search for Docker.

```bash
yum search docker
```

Remove Git.

```bash
sudo yum remove git
```

Update all installed packages.

```bash
sudo yum update
```

---

## Example Output

```text
Installed:
git.x86_64

Complete!
```

---

## Tips

Using `-y` skips the confirmation prompt.

Instead of

```bash
yum install git
```

you can use

```bash
yum install -y git
```

---

## Notes

Most package management commands require **root privileges**.

Use:

```bash
sudo
```

or switch to the root user.

```bash
su -
```

---

# 2.2 apt (Ubuntu)

## Purpose

`apt` is the default package manager for Ubuntu and Debian.

Its usage is very similar to `yum`.

---

## Syntax

```bash
apt [option] package-name
```

---

## Common Commands

Install software.

```bash
sudo apt install git
```

Search software.

```bash
apt search docker
```

Remove software.

```bash
sudo apt remove git
```

Update package list.

```bash
sudo apt update
```

Upgrade installed packages.

```bash
sudo apt upgrade
```

---

## Comparison

| Operation | CentOS | Ubuntu |
|-----------|---------|---------|
| Install | yum install | apt install |
| Remove | yum remove | apt remove |
| Search | yum search | apt search |
| Update packages | yum update | apt upgrade |

---

# 3. Service Management (systemctl)

## Purpose

`systemctl` is used to manage Linux system services.

Typical operations include:

- Start
- Stop
- Restart
- Enable startup
- Disable startup
- Check status

---

## Syntax

```bash
systemctl <operation> service-name
```

---

## Common Operations

| Command | Description |
|----------|-------------|
| start | Start a service |
| stop | Stop a service |
| restart | Restart a service |
| status | Check service status |
| enable | Enable auto-start |
| disable | Disable auto-start |

---

## Examples

Start Nginx.

```bash
sudo systemctl start nginx
```

Stop Nginx.

```bash
sudo systemctl stop nginx
```

Restart Nginx.

```bash
sudo systemctl restart nginx
```

View service status.

```bash
systemctl status nginx
```

Enable auto-start at boot.

```bash
sudo systemctl enable nginx
```

Disable auto-start.

```bash
sudo systemctl disable nginx
```

---

## Example Output

```text
● nginx.service - The nginx HTTP Server
Loaded: loaded
Active: active (running)
```

---

## Tips

To check all running services:

```bash
systemctl list-units --type=service
```

To view failed services:

```bash
systemctl --failed
```

---

## Notes

- Most service management operations require administrator privileges.
- If you modify a service configuration file, restart the service to apply the changes.

---

# 4. Symbolic Links

## Purpose

A **symbolic link (soft link)** works like a Windows shortcut.

It points to another file or directory without copying it.

---

## Syntax

```bash
ln -s <target> <link-name>
```

| Parameter | Description |
|-----------|-------------|
| target | Original file or directory |
| link-name | Name of the shortcut |

---

## Examples

Create a symbolic link to a file.

```bash
ln -s /home/user/test.txt shortcut.txt
```

Create a symbolic link to a folder.

```bash
ln -s /opt/project project
```

---

## Verify

```bash
ls -l
```

Example output:

```text
shortcut.txt -> /home/user/test.txt
```

---

## Remove a Symbolic Link

```bash
rm shortcut.txt
```

Only the link is deleted.

The original file remains unchanged.

---

## Notes

- A symbolic link does **not** duplicate data.
- Deleting the original file makes the symbolic link invalid.
- Symbolic links can point to files or directories.

---

# 5. Date and Time

## Purpose

The `date` command displays or formats the current system date and time.

It can also calculate dates.

---

## Syntax

```bash
date
```

```bash
date [+FORMAT]
```

```bash
date -d "STRING"
```

---

## Common Format Symbols

| Symbol | Meaning |
|---------|----------|
| %Y | Four-digit year |
| %y | Two-digit year |
| %m | Month |
| %d | Day |
| %H | Hour (24-hour) |
| %M | Minute |
| %S | Second |
| %s | Unix timestamp |

---

## Examples

Display current date.

```bash
date
```

Display only the date.

```bash
date +"%Y-%m-%d"
```

Example:

```text
2026-07-13
```

Display date and time.

```bash
date +"%Y-%m-%d %H:%M:%S"
```

Example:

```text
2026-07-13 15:08:32
```

Display Unix timestamp.

```bash
date +%s
```

---

## Date Calculation

Tomorrow.

```bash
date -d "+1 day"
```

Yesterday.

```bash
date -d "-1 day"
```

One week later.

```bash
date -d "+1 week"
```

One month ago.

```bash
date -d "-1 month"
```

---

## Tips

The `-d` option is extremely useful in shell scripting.

Example:

```bash
date -d "next Friday"
```

---

# 6. Time Zone Configuration

Linux stores time zone information separately from the system clock.

---

## View Current Time Zone

```bash
timedatectl
```

---

## Set Time Zone

List all available time zones.

```bash
timedatectl list-timezones
```

Set the time zone to China.

```bash
sudo timedatectl set-timezone Asia/Shanghai
```

Verify the configuration.

```bash
timedatectl
```

---

## Notes

Using the correct time zone is important because:

- Log files rely on accurate timestamps.
- Scheduled tasks (cron jobs) depend on local time.
- Server synchronization requires consistent time settings.

---

# 7. Time Synchronization (NTP)

## Purpose

NTP (Network Time Protocol) keeps the system clock synchronized with an Internet time server.

Accurate system time is essential for:

- Servers
- Databases
- Security certificates
- Distributed systems

---

## Install NTP

CentOS:

```bash
sudo yum install -y ntp
```

---

## Manage the NTP Service

Start the service.

```bash
sudo systemctl start ntpd
```

Stop the service.

```bash
sudo systemctl stop ntpd
```

Restart the service.

```bash
sudo systemctl restart ntpd
```

Check status.

```bash
systemctl status ntpd
```

Enable auto-start.

```bash
sudo systemctl enable ntpd
```

---

## Manually Synchronize Time

```bash
sudo ntpdate -u ntp.aliyun.com
```

Example output:

```text
adjust time server 203.xxx.xxx.xxx offset -0.002 seconds
```

---

## Best Practice

For production servers:

- Enable automatic synchronization.
- Use reliable NTP servers.
- Keep the time zone configured correctly.

# 8. IP Address

## Purpose

An **IP Address (Internet Protocol Address)** is a unique identifier assigned to every device connected to a network.

It allows devices to communicate with each other.

For IPv4, an IP address consists of four decimal numbers separated by dots.

Example:

```text
192.168.88.131
```

---

## IPv4 Format

```text
a.b.c.d
```

Where:

- Each number ranges from **0 to 255**
- Total length is **32 bits**
- Example:

```text
192.168.1.100
```

---

## Special IP Addresses

### 127.0.0.1

```text
127.0.0.1
```

Also called:

- Localhost
- Loopback Address

It always refers to **the current machine**.

Example:

```bash
ping 127.0.0.1
```

---

### 0.0.0.0

Depending on the context, it can mean:

- Current machine
- All available network interfaces
- Any IP address

For example, many servers listen on:

```text
0.0.0.0:80
```

Meaning:

> Accept connections from every network interface.

---

## View Local IP Address

### CentOS 7

```bash
ifconfig
```

Example output:

```text
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>

inet 192.168.88.131
netmask 255.255.255.0
```

---

### Modern Linux (Recommended)

Many newer Linux distributions no longer install `ifconfig` by default.

Instead, use:

```bash
ip addr
```

or

```bash
ip a
```

---

## Tips

Display only the IP address:

```bash
hostname -I
```

---

## Notes

- Public IP is used on the Internet.
- Private IP is used inside local networks.
- VMware virtual machines usually use private IP addresses.

---

# 9. Hostname

## Purpose

The **hostname** is the name assigned to a Linux computer.

It helps identify devices on a network.

---

## View Hostname

```bash
hostname
```

Example output:

```text
centos7
```

---

## Change Hostname

```bash
sudo hostnamectl set-hostname myserver
```

Example:

```bash
sudo hostnamectl set-hostname linux-lab
```

---

## Verify

```bash
hostname
```

Output:

```text
linux-lab
```

---

## View Detailed Information

```bash
hostnamectl
```

Example output:

```text
Static hostname: linux-lab
Operating System: CentOS Linux 7
Kernel: 3.10...
```

---

## Notes

Changing the hostname usually requires logging in again before the shell prompt updates.

---

# 10. Configure a Static IP (VMware)

## Why Use a Static IP?

By default, VMware assigns an IP address automatically (DHCP).

Every reboot may change the IP.

A static IP ensures:

- Stable SSH connections
- Stable web server access
- Convenient remote management

---

## Step 1

Configure the VMware virtual network.

> This step depends on your VMware network settings (NAT or Bridged).

---

## Step 2

Edit the network configuration file.

```bash
sudo vim /etc/sysconfig/network-scripts/ifcfg-ens33
```

---

## Example Configuration

```ini
TYPE="Ethernet"

BOOTPROTO="static"

ONBOOT="yes"

IPADDR="192.168.88.130"

NETMASK="255.255.255.0"

GATEWAY="192.168.88.2"

DNS1="192.168.88.2"
```

---

## Important Parameters

| Parameter | Description |
|-----------|-------------|
| BOOTPROTO | DHCP or static |
| ONBOOT | Start network on boot |
| IPADDR | Static IP address |
| NETMASK | Subnet mask |
| GATEWAY | Default gateway |
| DNS1 | DNS server |

---

## Restart Network

```bash
sudo systemctl restart network
```

---

## Verify

```bash
ip addr
```

or

```bash
ifconfig
```

---

## Tips

Always ensure:

- IPADDR belongs to the same subnet.
- Gateway matches VMware settings.
- DNS is configured correctly.

Otherwise, Internet access may fail.

---

# 11. Process Management

Processes are running programs in Linux.

Useful commands include:

- ps
- kill

---

# 11.1 ps Command

## Purpose

Display running processes.

---

## Syntax

```bash
ps -ef
```

---

## Common Options

| Option | Description |
|---------|-------------|
| -e | Show all processes |
| -f | Full-format output |

---

## Examples

Show all processes.

```bash
ps -ef
```

Find an Nginx process.

```bash
ps -ef | grep nginx
```

Find an SSH process.

```bash
ps -ef | grep ssh
```

---

## Example Output

```text
UID   PID  PPID  CMD

root    1     0  /usr/lib/systemd/systemd

root 1024     1  nginx

user 3256  3001  bash
```

---

## Important Columns

| Column | Meaning |
|---------|----------|
| UID | User |
| PID | Process ID |
| PPID | Parent Process ID |
| CMD | Command |

---

## Tips

Every process has a unique **PID**.

Many Linux commands use the PID.

---

# 11.2 kill Command

## Purpose

Terminate a running process.

---

## Syntax

```bash
kill PID
```

---

## Examples

Terminate process 3256.

```bash
kill 3256
```

Force terminate.

```bash
kill -9 3256
```

---

## Common Signals

| Signal | Description |
|---------|-------------|
| 15 | Gracefully terminate (default) |
| 9 | Force terminate |

---

## Best Practice

Always try:

```bash
kill PID
```

Only use:

```bash
kill -9 PID
```

if the process cannot exit normally.

---

# 12. Network Tools

---

# 12.1 netstat

## Purpose

Display network connections and port usage.

---

## Syntax

```bash
netstat -anp
```

Search a specific port.

```bash
netstat -anp | grep 80
```

---

## Common Options

| Option | Description |
|---------|-------------|
| -a | Show all connections |
| -n | Show numeric addresses |
| -p | Display process information |

---

## Example

```bash
netstat -anp | grep ssh
```

---

## Tips

If port **80** is occupied:

```bash
netstat -anp | grep 80
```

Find the corresponding PID.

---

# 12.2 nmap

## Purpose

Scan hosts and ports.

Useful for:

- Network troubleshooting
- Security testing
- Discovering open ports

---

## Basic Syntax

```bash
nmap target
```

---

## Examples

Scan localhost.

```bash
nmap localhost
```

Scan another host.

```bash
nmap 192.168.88.131
```

---

## Example Output

```text
22/tcp open ssh

80/tcp open http
```

---

## Notes

Install if necessary.

```bash
sudo yum install -y nmap
```

---

# 12.3 ping

## Purpose

Test whether another host is reachable.

---

## Syntax

```bash
ping target
```

---

## Examples

Ping Baidu.

```bash
ping www.baidu.com
```

Ping Google DNS.

```bash
ping 8.8.8.8
```

Send only four packets.

```bash
ping -c 4 8.8.8.8
```

---

## Example Output

```text
64 bytes from 8.8.8.8

icmp_seq=1

ttl=118

time=25.4 ms
```

---

## Common Options

| Option | Description |
|---------|-------------|
| -c | Number of packets |
| -i | Interval |
| -W | Timeout |

---

## Notes

If ping fails:

- Check the network cable.
- Verify the IP address.
- Check the gateway.
- Check DNS configuration.
- Ensure the firewall allows ICMP.

---
# 13. File Download

Linux provides two commonly used tools for downloading resources from the Internet:

- **wget** — Download files directly.
- **curl** — Transfer data between a client and a server.

---

# 13.1 wget

## Purpose

`wget` is a command-line utility used to download files from HTTP, HTTPS, and FTP servers.

It is suitable for downloading:

- Source code
- Software packages
- Images
- Documents
- Backup files

---

## Syntax

```bash
wget [OPTIONS] URL
```

---

## Common Examples

Download a file.

```bash
wget https://example.com/file.zip
```

Download and rename the file.

```bash
wget -O linux.zip https://example.com/file.zip
```

Continue downloading an interrupted file.

```bash
wget -c https://example.com/file.iso
```

Download files in the background.

```bash
wget -b https://example.com/file.zip
```

---

## Common Options

| Option | Description |
|---------|-------------|
| -O | Save with a specified filename |
| -c | Resume interrupted downloads |
| -b | Download in the background |
| -q | Quiet mode |
| --limit-rate | Limit download speed |

---

## Example Output

```text
Connecting to example.com...

Saving to: "file.zip"

100%==============================

Download completed.
```

---

## Tips

The downloaded file is saved in the **current working directory** unless another location is specified.

Check the current directory:

```bash
pwd
```

---

# 13.2 curl

## Purpose

`curl` is a powerful command-line tool used to transfer data over various protocols.

Unlike `wget`, `curl` can also:

- Send HTTP requests
- Test APIs
- Upload files
- Download files
- View webpage source code

---

## Syntax

```bash
curl [OPTIONS] URL
```

---

## Common Examples

View webpage source.

```bash
curl https://example.com
```

Download a file.

```bash
curl -O https://example.com/file.zip
```

Save with another filename.

```bash
curl -o linux.zip https://example.com/file.zip
```

Display HTTP headers.

```bash
curl -I https://example.com
```

---

## Common Options

| Option | Description |
|---------|-------------|
| -O | Save using the original filename |
| -o | Save using a custom filename |
| -I | Display HTTP headers |
| -L | Follow redirects |
| -X | Specify HTTP request method |

---

## Tips

Developers often use `curl` to test REST APIs.

Example:

```bash
curl -X GET https://api.example.com/users
```

---

# wget vs curl

| Feature | wget | curl |
|---------|------|------|
| Download files | ✅ | ✅ |
| Resume downloads | ✅ | Limited |
| API testing | ❌ | ✅ |
| Upload files | ❌ | ✅ |
| View webpage source | Limited | ✅ |

---

# 14. System Monitoring

---

# 14.1 top

## Purpose

`top` displays real-time information about:

- CPU usage
- Memory usage
- Running processes
- System load

It is similar to **Task Manager** on Windows.

---

## Syntax

```bash
top
```

---

## Common Information

```text
Tasks

CPU

Memory

Load Average

PID

USER

COMMAND
```

---

## Interactive Shortcuts

| Key | Function |
|------|----------|
| q | Quit |
| P | Sort by CPU usage |
| M | Sort by memory usage |
| k | Kill a process |
| h | Help |

---

## Example

```bash
top
```

---

## Tips

Modern Linux systems often install **htop**, which provides a more user-friendly interface.

Install:

```bash
sudo yum install -y htop
```

---

# 14.2 df

## Purpose

Display disk space usage.

---

## Syntax

```bash
df
```

---

## Human-readable Output

```bash
df -h
```

---

## Example Output

```text
Filesystem      Size  Used Avail Use%

/dev/sda1        40G   15G   23G   40%
```

---

## Common Options

| Option | Description |
|---------|-------------|
| -h | Human-readable format |
| -T | Display filesystem type |

---

## Tips

Always use:

```bash
df -h
```

instead of

```bash
df
```

because the output is easier to read.

---

# 14.3 iostat

## Purpose

Display CPU and disk I/O statistics.

Useful for diagnosing:

- High disk usage
- Slow disk performance
- CPU utilization

---

## Install

```bash
sudo yum install -y sysstat
```

---

## Syntax

```bash
iostat
```

Display updated statistics every two seconds.

```bash
iostat 2
```

---

## Example Output

```text
CPU

Device

tps

kB_read/s

kB_wrtn/s
```

---

## Notes

`iostat` belongs to the **sysstat** package.

---

# 14.4 sar

## Purpose

Collect and report system performance statistics.

It can monitor:

- CPU
- Memory
- Network
- Disk
- System load

---

## Syntax

View CPU usage.

```bash
sar -u
```

View network statistics.

```bash
sar -n DEV
```

View memory usage.

```bash
sar -r
```

---

## Tips

`sar` is widely used for long-term server performance monitoring.

---

# 15. Environment Variables

## Purpose

Environment variables store system configuration information.

Programs can read these variables during execution.

---

## View All Variables

```bash
env
```

or

```bash
printenv
```

---

## View One Variable

```bash
echo $HOME
```

---

## Create a Temporary Variable

```bash
export NAME=Linux
```

Verify:

```bash
echo $NAME
```

Output:

```text
Linux
```

---

## Permanent Environment Variables

### Current User

Edit:

```bash
~/.bashrc
```

Apply changes.

```bash
source ~/.bashrc
```

---

### System-wide

Edit:

```bash
/etc/profile
```

Apply changes.

```bash
source /etc/profile
```

---

## Notes

Temporary variables disappear after logging out.

Permanent variables remain after reboot.

---

# 16. PATH Variable

## Purpose

`PATH` tells Linux where to search for executable programs.

Without PATH:

```bash
/usr/bin/python
```

With PATH:

```bash
python
```

---

## View PATH

```bash
echo $PATH
```

---

## Example

```text
/usr/local/bin

/usr/bin

/bin
```

---

## Add a New Directory

Temporary.

```bash
export PATH=$PATH:/home/user/bin
```

Permanent.

Add the following line to:

```bash
~/.bashrc
```

```bash
export PATH=$PATH:/home/user/bin
```

Reload:

```bash
source ~/.bashrc
```

---

## Variable Expansion

Output a variable.

```bash
echo $PATH
```

Use braces.

```bash
echo ${PATH}ABC
```

Braces avoid ambiguity when appending text.

---

# 17. File Compression

Linux commonly uses:

- tar
- gzip
- zip
- unzip

---

# 17.1 tar

## Purpose

Archive multiple files into one package.

---

## Compress

```bash
tar -zcvf archive.tar.gz folder
```

---

## Options

| Option | Meaning |
|---------|----------|
| -z | Use gzip |
| -c | Create archive |
| -v | Verbose output |
| -f | Specify filename |

---

## Example

```bash
tar -zcvf project.tar.gz project/
```

---

## Extract

```bash
tar -zxvf project.tar.gz
```

Extract to another directory.

```bash
tar -zxvf project.tar.gz -C /home/user/
```

---

## Extraction Options

| Option | Meaning |
|---------|----------|
| -x | Extract archive |
| -C | Target directory |

---

# 17.2 zip

## Compress

Compress a folder.

```bash
zip -r project.zip project/
```

Compress multiple files.

```bash
zip archive.zip file1 file2 file3
```

---

## Extract

```bash
unzip project.zip
```

Extract to another folder.

```bash
unzip project.zip -d /home/user/
```

---

## Common Options

| Option | Description |
|---------|-------------|
| -r | Compress directories recursively |
| -d | Specify extraction directory |

---

# tar vs zip

| Feature | tar.gz | zip |
|---------|---------|------|
| Linux native | ✅ | ✅ |
| Windows compatible | Limited | ✅ |
| Compression ratio | Higher | Medium |
| Common usage | Linux servers | Cross-platform sharing |

---

# Conclusion

Congratulations! 🎉

You have now learned the most commonly used Linux operations, including:

- Package management
- Service management
- Symbolic links
- Date and time management
- Network configuration
- Process management
- File downloading
- System monitoring
- Environment variables
- File compression

These commands form the foundation of Linux system administration and are essential for daily development, server management, and DevOps workflows.

> **Next Step:** Continue practicing these commands in a Linux terminal to build confidence and improve your command-line skills.

**Learned up to here on 13/7/2026.**