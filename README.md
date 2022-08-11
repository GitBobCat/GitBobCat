The Plan

I run a personal mailserver, postfix/dovecot, on my Raspberry Pi. Some idiots got hacked and gifted a couple of aliased email addresses to 'The Community'. This was about five years ago but 'Masters of The L33T' have decided to sell this information to 'Idiots of The L33T' who spend all day wasting IP addresses filling up my logfiles with PassWord123 because logging in on an alias that no longer exists is really going to work and your chances of success will be improved if you use a for shit dictionary.

The only way, as far as I can see, to mitigate this is to parse the logfiles and dump the idiot into IPTables. Then the idiot goes off and buys more IP addresses from their masters and starts over. Do not even try to suggest I dick about with some .cf .conf or other file in postfix/dovecot. It just blows up the size of the logfile. Also do not suggest I should download or subscribe to someone else's unfocussed IP shitlist.

I want a community gathered shitlist whereby members run a script that parses mail.log and dumps the L33T idiots IP addresses into a file that gets uploaded to a place on github where it is parsed for a consensus and reduced to an IPTables shitlist for everyone else to use and I will throw my stupid toys out of my stupid pram if I do not get one!

Naturally, despite your man pages and recommendations to add text I can copy to a file that is unidentified whilst not mentioning I have to uncomment some lines in auth-10 under conf.d, I do not have the skills required to achieve this.

Now I am going to suggest that if this Plan succeeds THE LIST will be read by those who rent IP addresses and they will take action like their reputation depends on it. Hah Hah Hah. I'm totally fruitloop

My, for shit, prototype script is,

Oh crap github hates me. Try prototype.txt

Copies text to file just in case naughty words are not allowed.
