
# Besturingssystemen II: Les 3

## 

```bash
#Waar words is een bestand

stat words #meer statistieken voor files
file words #geeft het type van de file
alias #geeft alle aliassen weer

touch t
'rm' t #commando wordt uitgevoerd zonder alias
unalias rm #verwijderd alias voor 1 sessie

touch t
touch µ{0..9} #maakt alle bestanden van µ0 t.e.m. µ9
ls µ* #geeft alle gemaakte bestandjes weer
rm -i µ? #verwijderd alle µ0 t.e.m. µ9
yes | rm -i µ? #zegt ja op alle vragen
yes | rm -i µ? 2> /dev/null #uitvoer wordt weg gesmeten, 2> staat voor standaard uitvoer

shuf -n 10 -re y n # maakt random lijst van y en n voor 10 elementen en shuffelt deze
shuf -n 10 -re n n n n Y # met 80% kans een nee en 20% een ja, als we -n 10 weg doen blijft dit oneindig lang lopen
shuf -n 10 -re n n n n Y | rm -iv * 2> /dev/null #gaat 20% verwijderen uit map met student files

cp µ* #werkt niet
cp µ* studenten/ #kopieert alle bestanden µ* naar studenten/ 
touch ../µ* #weet niet
mv -u µ? .. #u (kopie als recenter zie slides)


cp ../dw . #kopieert dw bestand naar .
> dw #maakt bestand leeg (overschrijft lege lijn naar bestand)

touch t{0..9}
rm -i t4 t8 t9
touch -c t{0..9} #aanpassen van datum indien nieuwe file (opfrissen)

touch -r dw ref #datum van dw overkopieren naar ref (erven op basis van ander bestand)

untill [[ dw -nt ref ]] ; do : ; done ; sl #als het gelijk is doet het niks anders krijg je treintje

touch -t 201501070000 #geeft timestamp aan file 2015(j)01(m)07(d)00(h)00(m)
touch -t 201501080000

# Aangeraden touch -r en -t te beheersen (zegt hij)

find . -maxdpth 1 -type f -newer t1 ! -newer t2
find / -maxdpth 1 -type f -newer t1 ! -newer t2

while read ; do ((RANDOM%10)) && echo $REPLY ; done < lees
while read ; do ((RANDOM%10)) && echo $REPLY ; done < lees > t1
while read ; do ((RANDOM%10)) && echo $REPLY ; done < lees > t2
ls -l t1 t2 #vergelijk files
comm t1 t2 #vergelijkt bestanden --> moeten gesorteerd zijn voor comm
sort t1 > t3
sort t4 > t4
comm t3 t4 #nu gaat het wel
comm -12 t3 t4 #1: items die enkel in 1ste kolom voorkomen niet tonen
               #2: items die enkel in 2ste kolom voorkomen niet tonen
               #3: items die gemeenschappelijk voorkomen niet tonen
comm -23 t3 t4 #zie man page

diff t1 t2 #difference hoeft niet gesorteerd te worden --> wel trager als comm

while read ; do ((RANDOM%10)) && echo $REPLY ; done < lees > t1
while read ; do ((RANDOM%10)) && echo $REPLY ; done < lees > t2

wc t1 t2

#zie slides
diff -y t1 t2 #zet waarden tegenover elkaar
diff -y --suppress-common-lines t1 t2 #waardes die in beide voorkomen eruit halen
diff -yW30 --suppress-common-lines t1 t2 #W30 geeft kolom grote van 30

md5sum t t1 t2 t3 t4 tel *.csv #maakt hash voor bestand
md5sum t t1 t2 t3 t4 tel *.csv > r #maakt hash voor bestand
cat r
mdsum -c r #c (check) gaat per lijn checken of deze hetzelfde is en geeft OK
md5sum -c r | grep -v OK$ #geeft enkel lijnen die niet ok zijn
                          #-v Invert the sense of matching, to select non-matching lines.
sha512sum t t1 t2 t3 t4 tel *.csv #gebruik dit niet om te kijken of een bestand verwijderd omdat dit te intensief is voor processor.

paste tel lees #zo kun je bestanden horzontaal naast elkaar plaatsen
paste -s tel lees #-s (serial) paste one file at a time instead of in parallel
paste -sd\; tel #-d(delimiter) reuse characters from LIST instead of TABs (gaat ; als delim. gebruiken)
join -t$'\t' #-t: use CHAR as input and output field separator
join -t$'\t' -o 1.2,2.2,0 dw cn #op basis van zelfde woorden (inner join)
join -t$'\t' -o 1.2,2.2,0 -a 2 dw cn #outer join
join -t$'\t' -o 1.2,2.2,0 -v 2 dw cn #right outer join enkel tweede kolom

sort -t\; -k3 competitors.csv > c #-t staat hier voor delimiter, -k3: vanaf 3de veld
#-k3,3 van 3de tot 3de veld
sort -t\; -k2,2 winners.csv > w
join -t\; -1 2 -2 3 c w #-1 3:2de veld van bestand 1, -2 2: 3de veld van 2de bestand
# deze joint op gemeenschappelijk veld

join -t\; -1 2 -2 3 -o 1.1 c w | sort | uniq -dc | sort -k1,1nr | head -n 10 #zie man pages
```

## Bestanden in stukken hakken

```bash
split -l5 lees
split -da1 -l5 lees µ #split ze in 5 bestanden, kan ook toegepast worden op binaire file
ls -ltr µ*
paste µ{0..3} #voegt ze weer samen

split -da2 -b20KB /usr/bin/find µ #d numeric suffix, a number of suffix (default 2)
split -da1 -b25KB /usr/bin/find µ
ls -l µ*
cat µ? > t

#gebruik printf i.p.v. echo

xargs < lees printf "%20s%20s%20s\n" #xargs zal de volledige file doorgeven als een lijst van argumenten, printf print deze file in 3 kolommen uit.
```

## gebruik printf i.p.v. echo

```bash
xargs < lees printf "%20s%20s%20s\n" #xargs zal de volledige file doorgeven als een lijst van argumenten, printf print deze file in 3 kolommen uit.

locate "*HTML*" #find files by name (pattern)
locate "*HTML*" #-b (basename) Match only the base name against the specified patterns.
#locate werkt met snapshots, deze worden genomen in idle tijd (geeft dus geen zekerheid over exact bestande) --> gaat wel zeer snel

find #find - search for files in a directory hierarchy, zwaarder als locate
find /usr -type f -name "*HTML*" #vind filenames in /usr met pattern=*HTML*
find /usr -type f -size +100M #vind files groter als 100MB
find /usr -type f -size +100M 2> /dev/null
find /usr -type f -size +100M 2> /dev/null -printf "%s %t %p\n" #%s size, %t timestamp, %p path

locate -b "*HTML*" | xargs md5sum 
locate -b "*HTML*" | xargs -i md5sum "{}" #door -i kan ik placeholder gebruiken, nu gaan bestanden met spaties ook gehashed worden

xargs < lees | wc -l  #1 lijn
xargs < words | wc -l #16 lijnen

locate -b "*HTML*" | xargs ls -lad
#studenten = map
locate -b "*HTML*" | xargs -pi cp "{}" studenten #-p (prompt) de vraag ja/nee
```