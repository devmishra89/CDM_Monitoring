# CDM_Monitoring
Script to monitor the CDM 
#!/bin/bash
hostname=`hostname`
trigger=20.00
load=`top -b -n3 | awk '/^Cpu/ {print $2}' | sed '3!d' `
response=`echo | awk -v T=$trigger -v L=$load 'BEGIN{if ( L > T){ print "greater"}}'`
if [[ $response = "greater" ]]
then
top -b -n3 | mail -s "High CPU load on $hostname - [ $load ]" e-mail
fi
#Memory
hostname=`hostname`
trigger=80.00
load=`free -m | awk 'NR==2{printf "%.2f%%\t\t", $3*100/$2 }' `
response=`echo | awk -v T=$trigger -v L=$load 'BEGIN{if ( L > T){ print "greater"}}'`
if [[ $response = "greater" ]]
then
free -m | mail -s "High Memory load on $hostname - [ $load ]" e-mail
fi
#Disk
hostname=`hostname`
trigger=80.00
load=`df -h | awk '$NF=="/"{printf "%s\t\t", $5}' `
response=`echo | awk -v T=$trigger -v L=$load 'BEGIN{if ( L > T){ print "greater"}}'`
if [[ $response = "greater" ]]
then
du -sh /* | mail -s "High Disk Usage on $hostname - [ $load ]" e-mail
fi

top -b -n3|awk 'NR>'$(top -b -n3 |grep -n "top -"| cut -d: -f1|awk  END{print}) 



curl -I http://localhost 2>/dev/null | head -n 1 | cut -d$' ' -f2


