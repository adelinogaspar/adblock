# Ad Block Site List
List of sites with advertising that could be imported on [ddwrt] to block on your route.

## Script to paste on ddwrt

Administration > Commands:

```
#!/bin/sh
logger WAN up script executing
if test -s /tmp/hosts0
then
        rm /tmp/hosts0
fi

logger Downloading https://raw.githubusercontent.com/adelinogaspar/adblock/master/list.txt
wget -O - https://raw.githubusercontent.com/adelinogaspar/adblock/master/list.txt | grep 0.0.0.0 |
	sed 's/[[:space:]]*#.*$//g;' |
	grep -v localhost | tr ' ' '\t' |
	tr -s '\t' | tr -d '\015' | sort -u >/tmp/hosts0
grep addn-hosts /tmp/dnsmasq.conf ||
	echo "addn-hosts=/tmp/hosts0" >>/tmp/dnsmasq.conf
logger Restarting dnsmasq
killall dnsmasq
dnsmasq --conf-file=/tmp/dnsmasq.conf
```

[ddwrt]: https://wiki.dd-wrt.com/wiki/index.php/Ad_blocking
