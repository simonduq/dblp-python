## Counting PC-Authored Papers ##
---

This script relies on DBLP to pull the papers related to a given
conference and year. There are three things the user needs to set for
each conference. Let me use USENIX ATC as an example.

1. The conference name as seen by DBLP. For example, for ATC this is
`usenix`.

2. The `venue` as seen by DBLP. Each paper belonging to
this conference will have this venue set. For example, for ATC this is
`USENIX Annual Technical Conference`.

3. The year of the conference.

You might need to poke around DBLP a bit to get these values. For
example, if you go to
[http://dblp.uni-trier.de/search/publ/api?q=/conf/eurosys/2017](http://dblp.uni-trier.de/search/publ/api?q=/conf/eurosys/2017),
you can see what the `venue` and conference name parameters are for Eurosys.

### PC Information ###

Unfortunately, there is no automated way to get the program committee
information.  You need to manually get the list of PC members and put
them in a text file. You then process the list of PC members and add
them to the `pc[year][conf]` list in `checkpc.py`.

An effort must be made to make the names of the PC members identical
to how they appear on DBLP. For example, if an author is called "A
B. C" on DBLP, their PC entry must be changed to reflect that (often
it will be just "A C" in the PC list).

For authors with common names, their DBLP name might have a
number. For example, "A B 001". The PC entry will have to changed to
reflect the correct author.
        
### Checking PC Authorship ###

After parsing the PC list and populating `pc[year][conf]` you must
call `check_pc(conf, year, DBLP-conf-venue)`. `conf` and
`DBLP-conf-venue` must match what DBLP sees for the conference as
mentioned above. The `conf` used in `pc` and `check_pc` must match.

`check_pc` will then print out the total number of papers, the number
of papers with at least one PC member as author, and the percentage of
total papers authored by PC members.

### Same Output ###

```
usenix 2017
Total Papers: 63
PC-Paper Count: 10
Percentage of PC-authored papers:  15.87
```      
    
### Caveats ###

This scripts works to the extent the names of the program committee
match their DBLP name for their papers. As such, it is likely it does
not work well for authors with unicode characters in their names, and
authors with common names. Thus, the results of this script are a
lower bound on the number of PC-authored papers in the conference.
    