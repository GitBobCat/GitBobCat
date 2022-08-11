The Plan

I run a personal mailserver, postfix/dovecot, on my Raspberry Pi. Some idiots got hacked and gifted a couple of aliased email addresses to 'The Community'. This was about five years ago but 'Masters of The L33T' have decided to sell this information to 'Idiots of The L33T' who spend all day wasting IP addresses filling up my logfiles with PassWord123 because logging in on an alias that no longer exists is really going to work and your chances of success will be improved if you use a for shit dictionary.

The only way, as far as I can see, to mitigate this is to parse the logfiles and dump the idiot into IPTables. Then the idiot goes off and buys more IP addresses from their masters and starts over. Do not even try to suggest I dick about with some .cf .conf or other file in postfix/dovecot. It just blows up the size of the logfile. Also do not suggest I should download or subscribe to someone else's unfocussed IP shitlist.

I want a community gathered shitlist whereby members run a script that parses mail.log and dumps the L33T idiots IP addresses into a file that gets uploaded to a place on github where it is parsed for a consensus and reduced to an IPTables shitlist for everyone else to use and I will throw my stupid toys out of my stupid pram if I do not get one!

Naturally, despite your man pages and recommendations to add text I can copy to a file that is unidentified whilst not mentioning I have to uncomment some lines in auth-10 under conf.d, I do not have the skills required to achieve this.

Now I am going to suggest that if this Plan succeeds THE LIST will be read by those who rent IP addresses and they will take action like their reputation depends on it. Hah Hah Hah. I'm totally fruitloop

My, for shit, prototype script is,

Oh crap github hates me. Try prototype.txt

#!/bin/sh

#take a copy of the miscreants
iptables -nvL | grep -v "0     0" | grep "DROP" > iphitrate.txt

#Take a copy of the last 1000 entries.
#tail -1000 mail.log > mail01.log

#Take a copy of mail.log
cat mail.log > mail01.log

#Append a copy of mail.log.1
#cat mail.log.1 >> mail01.log

#Remove leading postfix/dovecot notes.
#This allows easier searching for [***.***.***.***] entries.
#Removes text up to third colon(:).
grep -o -P '(?<=:).*(?=)'  mail01.log | grep -o -P '(?<=:).*(?=)' | grep -o -P '(?<=:).*(?=)' > mail02.log

#delete the old block list
rm block01.txt

#Look for block strings and extract ip addresses between delimiters
#appending addresses to the block list.
grep "connection after AUTH"   mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt
grep "disconnect from unknown" mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt
grep "authentification failed" mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt
grep "security.ipip.net"       mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt
grep "nternet-measurement.com" mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt
grep "censys-scanner.com"      mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt
grep "shadowserver.org"        mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt
grep "mcdlv.net"               mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt
grep "shodan.io"               mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt
grep "onyphe.net"              mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt  
grep "planner.co.in"           mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt
grep "cyberresilience.io"      mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt
grep "stemsresearch.com"       mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt
grep "binaryedge.ninja"        mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt
grep "bmsend.com"              mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt
grep "threatsinkhole.com"      mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt
grep "no auth attempts"        mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt
#grep "spameri"                mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt
#grep "dickhead"               mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt
#grep "knobcheese"             mail02.log | grep -o -P '(?<=\[).*(?=\])'        >> block01.txt
grep "user=<dickhead"          mail02.log | grep -o -P '(?<=rip\=).*(?=\, lip)' >> block01.txt
grep "user=<knobcheese"        mail02.log | grep -o -P '(?<=rip\=).*(?=\, lip)' >> block01.txt
grep "no auth attempts"        mail02.log | grep -o -P '(?<=rip\=).*(?=\, lip)' >> block01.txt

#Catch/Remove spurious unknown[unknown] entries.
#Catch/Remove my ip address.
#Sort the list.
grep -v "unknown"      block01.txt > block02.txt
grep -v "myipaddress"  block02.txt > block03.txt

#Sort the list.
cat block03.txt | sort -u          > block04.txt

#take a copy of iptables.
iptables-save > iptables01.txt

#search for and extract duplicates.
grep -f block04.txt iptables01.txt | grep -o -P '(?<=-d ).*(?=\/)' > duplicates.txt

#remove duplicates
grep -v -x -f duplicates.txt block04.txt > block05.txt

#add results to iptables.
echo "blocking IP addresses..."
BLOCKDB=block05.txt
#BLOCKDB=digital.txt
IPS=$(grep -Ev "^#" $BLOCKDB)
for i in $IPS
do
echo "Adding " $i
iptables -A INPUT  -s $i -j DROP 
iptables -A OUTPUT -d $i -j DROP
done
echo "done."


Copies text to file just in case naughty words are not allowed.
