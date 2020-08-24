# memory-usage
#!/bin/bash
# GNU bash, version 4.4.20

w=60 # in percent for warning threshold
c=90 # in percent for critical threshold
host=`hostname -f`
e=alerte@nospam.com # Email for receiving alerts
logfile="/var/log/memory.log"

# Calculate real occupied memory in percent

realmem=`free | awk 'FNR == 3 {print $4/($3+$4)*100-100}'| sed 's/-//g'|cut -d "." -f1`


if [ "$realmem" -gt "$w" ]

then
date=`date`
echo "The memory usage has reached $realmem% on $host" | mail -s "$host : High Memory Usage Alert" $e
echo "$date : Alert T1 : The memory usage has reached $realmem% on $host." >> $logfile
exit 1

if [ "$realmem" -lt "$w" ]

then
date=`date`
echo "The memory usage has reached $realmem% on $host" | mail -s "$host : Low Memory Usage Alert" $e
echo "$date : Alert T0 : The memory usage has reached $realmem% on $host." >> $logfile
exit 0


if [ "$realmem" -gt "$c" ]

then

echo "The memory usage has reached $realmem% on $host" | mail -s "$host : Very High Memory Usage Alert" $e
echo "$date : Alert T2 : The memory usage has reached $realmem% on $host." >> $logfile
exit 2

fi
fi
fi
