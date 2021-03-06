# NordVPN IP Tool
This tool can be used for gathering all [NordVPN](https://nordvpn.com/) IP addresses for Blacklists or other usage. Our logs reveal that NordVPN is used regularly by scammers to attempt fraud and attacks to our systems. To create a blacklist of all NordVPN's IP addresses, I wrote the following script.

* List of USA Servers: https://nordvpn.com/servers/
* Tool from NordVPN for subdomain format: https://nordvpn.com/servers/tools/

## Basic Usage
In the next few section, we will cover a few examples of how to use the script to generate the IP list(s).
### Albania (AL)
Albania (first on NordVPN list (alphabetical)). According to NordVPN's site, they have 6 servers so I entered 100 to be safe and we found 1 extra that was not listed on their site:

![Albania Screenshot](images/al.png)

Run the script by passing the country code (initials), beginning number and end number.

*Notice that the subdomain name numbers may lie outside of the count number. For instance, USA (at the time of writing this) has 1798 servers - but a server number of 2971 exists (us2971.nordvpn.com). So, the count number should be adjusted until the total results from the script yeilds the count listed (or higher) on the NordVPN site. The count will begin at 0 and will stop at the count number*

```
root@demon:~$ ./nordvpn-blacklist-gen.sh al 0 100
al7.nordvpn.com:Address: 80.246.28.33
al8.nordvpn.com:Address: 80.246.28.35
al9.nordvpn.com:Address: 31.171.152.19
al10.nordvpn.com:Address: 31.171.152.11
al11.nordvpn.com:Address: 31.171.152.115
al12.nordvpn.com:Address: 31.171.152.235
al13.nordvpn.com:Address: 31.171.152.243
root@demon:~$ 
```
### United States (US)
USA (example used because that's where I live). According to NordVPN's site, they have 1798 servers so I entered 3000 to be safe.

![USA Screenshot](images/us.PNG)

```
root@demon:~/nordvpn-ip-tool$ ./nordvpn-blacklist-gen.sh us 0 3000
us324.nordvpn.com:Address: 104.200.65.178
us349.nordvpn.com:Address: 162.210.198.129
us350.nordvpn.com:Address: 162.210.198.130
us351.nordvpn.com:Address: 162.210.198.131
us352.nordvpn.com:Address: 162.210.198.132
us353.nordvpn.com:Address: 209.58.133.167
us354.nordvpn.com:Address: 209.58.133.168
us355.nordvpn.com:Address: 209.58.133.169
us356.nordvpn.com:Address: 209.58.133.170
us381.nordvpn.com:Address: 209.58.144.227
us382.nordvpn.com:Address: 209.58.144.228
us383.nordvpn.com:Address: 209.58.144.229
us384.nordvpn.com:Address: 209.58.144.230
us435.nordvpn.com:Address: 23.227.207.3
us436.nordvpn.com:Address: 23.227.207.4
us437.nordvpn.com:Address: 23.227.207.5
us438.nordvpn.com:Address: 23.227.207.6
us502.nordvpn.com:Address: 181.215.110.136
us503.nordvpn.com:Address: 181.215.110.137
us504.nordvpn.com:Address: 181.215.110.138
us505.nordvpn.com:Address: 181.215.110.139
us510.nordvpn.com:Address: 181.215.110.144
us511.nordvpn.com:Address: 181.215.110.145
us512.nordvpn.com:Address: 181.215.110.146
us513.nordvpn.com:Address: 181.215.110.147
...
```
## Advanced Usage
You can use the [avail-country-list.txt](avail-country-list.txt) as input in a simple Bash `for` or `while` loop to scan for all countries. Below is an example of a `while` loop using `read` and will scan each country code for 0-10 subdomains.
```
root@demon:/infosec/defense/nordvpn-ip-tool# while read country;\
 do ./nordvpn-blacklist-gen.sh $country 0 10; done < avail-country-list.txt 
al7.nordvpn.com,80.246.28.33
al8.nordvpn.com,80.246.28.35
al9.nordvpn.com,31.171.152.19
al10.nordvpn.com,31.171.152.11
ar4.nordvpn.com,131.255.7.84
ar8.nordvpn.com,190.105.235.82
ar9.nordvpn.com,131.255.4.122
ar10.nordvpn.com,131.255.4.237
au7.nordvpn.com,168.1.12.244
ba5.nordvpn.com,185.99.3.18
ba6.nordvpn.com,185.99.3.20
ba7.nordvpn.com,185.99.3.104
ba8.nordvpn.com,185.99.3.106
bg8.nordvpn.com,82.102.23.85
bg9.nordvpn.com,82.102.23.86
cl7.nordvpn.com,190.105.239.117
cl8.nordvpn.com,45.228.209.43
cl9.nordvpn.com,45.228.209.51
cl10.nordvpn.com,190.105.239.168
cr8.nordvpn.com,190.112.223.136
cr9.nordvpn.com,179.48.251.226
cr10.nordvpn.com,179.48.251.229
cy0.nordvpn.com,185.205.184.18
cy4.nordvpn.com,185.106.102.216
cy5.nordvpn.com,185.106.102.240
cy6.nordvpn.com,185.106.102.201
cy7.nordvpn.com,185.191.206.3
cy8.nordvpn.com,185.191.206.8
cy9.nordvpn.com,185.191.206.13
cy10.nordvpn.com,185.106.102.200
```
### Resume Scan
If you decide to `tee` the output to a log file and your results don't match the server count listed on the NordVPN page, you can resume the scan by simply making the begin number the end number used in the previous command liek so,
```
root@demon:~# ./nordvpn-blacklist-gen.sh us 0 3000 | tee us-scan-04.30.2019.csv
...
```
Which scans from `us0.nordvpn.com` to `us3000.nordvpn.com`. Then,
```
root@demon:~# ./nordvpn-blacklist-gen.sh us 3001 5000 | tee -a us-scan-04.30.2019.csv
...
```
Will then scan from `us3001.nordvpn.com` to `us5000.nordvpn.com` and **append** the results using the `-a` argument to `tee` to the CSV file.
## Installation
You can simply copy the script to your $PATH like so,
```
root@demon:~# git clone https://github.com/weaknetlabs/nordvpn-ip-tool/
root@demon:~# cd nordvpn-ip-tool
root@demon:~/nordvpn-ip-tool# chmod +x nordvpn-blacklist-gen.sh
root@demon:~/nordvpn-ip-tool# sudo cp nordvpn-blacklist-gen.sh /usr/local/bin/
```
## Dependencies
Below is a list of the dependencies required for this script,
* [Linux](https://www.demonlinux.com/)
* [Bash](https://www.gnu.org/software/bash/)
* [host](https://linux.die.net/man/1/host)
* [seq](https://linux.die.net/man/1/seq)
* [printf](https://linux.die.net/man/3/printf)

## Example Scans
Check the [CSV Directory](csv/) for a few example scans. These were last ran on 04.30.2019.
