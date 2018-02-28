# Besturingssystemen II: Les 2

```bash
$x = aap
echo ...`...;...$...\...       
echo ...\`...\;...\$...\\...   
echo '...`...;...$...\...'
echo "...\`...;...$x...\\..."
echo '...`...;...$x...\...' 

echo $'een\ttwee'
echo $'een\ttwee\ndrie'
echo $'een\ttwee\ndrie\x0bvier'
echo $'een\ttwee\ndrie\cKvier'
echo $'een\ttwee\ndrie\cKvier\cMvijf'

echo $'een\t\c[[7mtwee\c[[27m\ndrie\cKvier\cMvijf'
echo $'een\t\c[[7mtwee\c[[27m\ndrie\cKvier\cMvijf' | hexdump -C
```

lijn 5: Dit wordt niet letterlijk uitgevoerd  
lijn 6:dit kunnen we oplossen door er een backslash (\) voor te zetten.  
lijn 7: letterlijke string  
lijn 8: variabele wordt geinterpreteerd  
lijn 9: variabele wordt niet geinterpreteerd  

lijn 13: \x0b  
lijn 14: \cK  
lijn 15: \cM  

lijn 17: \c[[7mtwee\c[[27m --> zet twee met witte achtergrond en zwarte letters (markering)  
lijn 18: geeft hexadecimale shizzle (zie man hexdump)  

## Standaard I/O

STDIN: zoals perl
STDOUT: zoals perl, 1>| of >| 

```bash
echo aap > t
cat t

echo aap > t ; echo tweede >> t
cat t

echo { echo eerste ; echo tweede } > t
cat t

echo ( echo eerste ; echo tweede ) > t
echo ( echo eerste ; echo tweede ; ) > t
cat t

f () { echo "             "${!1}$'\cM'$1 ;} 
f USER

f () { echo "             "${!1}$'\cM'$1 ;} >> t
f XMODIFIERS
f USER 
cat t

sort < file | cat #onnutig
sort < file | cat -E #beetje nut
sort < file #wel nut

tr 'aeiou' '.' < file
cat file | tr 'aeiou' '.' < file #onnodig cat gebruiken

echo $PATH | wc -c #telt aantal lijnen
wc -c <<< $PATH #dit is beter dan lijn 68

$x="0123             4567"
wc -c <<< $x
wc -c <<< "$x"
```

lijn 41: geeft aap terug (bestand wordt aangemaakt als dit niet bestaat)   
lijn 43: voegt tweede toe aan bestand (zoals perl)   
lijn 46: schrijft eerste en tweede weg naar het bestand (andere manier)  
lijn 49-50: nog een andere manier {} haakjes is performanter (buiten als op achtergrond wordt uigevoerd)  
lijn 53: f () {inhoud functie} -> creërt een functie   
lijn 54: gaat de functie toepassen op USER (param)    
lijn 56-58: meerder input params voor de functie, en append aan bestand t   
lijn 61-62: pipen naar cat heeft geen nut (zonder opties)   
lijn 65: tr (translate) --> veranderd de klinkers (1ste param) door een '.'(2de param)   
lijn 69: dit is een snellere manier dan lijn 68, doet wel hetzelfde   
lijn 71-73: lijn 72 gaat alle spaties als 1 spatie zien dit kan opgelost worden met lijn 73;


```bash
shuf -n 10 -ri 0-9
shuf -n 10 -ri 0-9 | sort | uniq -dc
shuf -n 10 -ri 0-9 | tee t1 t2 | sort | uniq -dc
shuf -n 10 -ri 0-9 | tee t1 t2 | sort | uniq -dc #deze lijn niet kunne overschrijven
```

lijn 90: creërt een aantal random getallen   
lijn 91: kijk het aantal unieke random gegenereerde getallen, sorteer ze en lijst ze met aantal keer voorgekomen.  
lijn 92: gaat de random getallen opslaan in t1 en t2 + zie vorige lijnen    
lijn 93: zie comment   


```bash
type printf

help printf | less
help -s case

man -k search
man -k search | less
man -f kill
man 2 kill
man 1 kill

info kill
info find

kill --help
sort --help
```

lijn 105: vi editor (pijltjes, page up, page down, q=quit) zoals man pages

lijn 108: geeft een overzicht van alle man pages waar search in voorkomt
lijn 110: geeft alle versies van de man pages van kill
lijn 111: geeft sectie 2 van man pages 
lijn 112: default: man kill geeft eigenlijk man 1 kill   

lijn 114-115: info pages --> soms nuttigere informatie te vinden

lijn 117-118: geeft help

## onderwerp?

Proc: 
- geheugenstructuren van linux
- veel interne variabele die op 0 of 1 staan. (vb: net.ipv4.ip_forward=1)
- bestanden waarvan de inhoud 0 of 1 is.

```bash
tree /etc #boomstructuur
tree -d /etc #enkel directories
tree -idf /etc #volledige pathnamen
tree -idfL 2 /etc #2 niveau's diep (L)
tree --du -idfL 2 /etc #overzicht heel de boomstructuur
```

```bash
rm -f t #verwijderd link naar bestand t?

ln -s source t #kopie maken naar source van bestand t
ls -l t #list context van t

tree -dL 1 /usr #1 nivau diep (links worden weergegeven)
cd -P /usr/tmp

pushd /etc/.../...
popd #keert terug naar vorige locatie
cd ~ #cd to home dir

rm -rf a #verwijderd alle mappen onderliggend (zonder bevestiging)
```

lijn 150: wordt een heel klein filetje aangemaakt met een pointer naar het bestand (logische link?)   
lijn 151: geeft ook de ware locatie van het bestand   
lijn 154: cd naar waar de logische link verwijst   
lijn 156-157: gebruikt men samen?? kan ik cd en dan popd doen? pushd en popd werken met een stack structuur 

```bash
ls -ad * #d geeft dirs
ls /etc/*.conf #geeft alle bestanden die voldoen aan patroon
ls -d *.d #geeft dir zonder inhoud eindigend op .d
ls -d *@(.d|.conf) #geeft .d of .conf
ls -d !(*@(.d|.conf)) # ! --> negatie
ls -d !(*@(.d|.c?(on)f?(g))) #geeft .d of .c(on)f(g)
ls -d !(*@(.d|.c?onf?g)) #c?onf?g 
ls -ad .* #alle bestanden beginnend met . (optie -a geeft ook lijnen beginnend met .)
ls -Ad .* #laat . en .. weg
sl #treintje
ls #list
```

### wildcards
(?): exact 1 char  
(*): eender welk en hoeveelheid aan chars
