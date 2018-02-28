## Hashes

```shell
shuf -rn20 -i 1-20
```

```perl
@X=qw(19 19 20 12 7 14 13 17 12 14 18 16 11 11 3 8 1 5 3 19);
@Y=qw(16 2 3 6 14 4 3 14 1 10 15 8 12 8 13 3 12 13 5 10);

# Dubbels verwijderen
# Hash creëren (unieke keys)
# De bijhorende waren doet er niet doe, undef mag ook dus iets helemaal anders zijn.

# 1. 19 toevoegen
# 2. Er gebeurt niets
# 3. 20: een tweede element toevoegen

# Resultaat: een hash met evenveel verschillende elementen als in @X

$A{$_}=undef for @X;

print join "
", keys %A;

# Gesorteerd
print join "
", sort {$a <=> $b} keys %A;

# Of:
# Dit doet net hetzelfde als daarnet
# Je creeërt evenveel elementen als de verschillende waarden in W
@B{@X}=();


## Geef mij de elementen van X die niet voorkomen in Y
@C{@X}=();

delete @C{@Y};

print join "
", keys %C;

# Laaste bit op 1 zetten (binaire or)
@D{$_}|=1 for @X;
@D{$_}|=2 for @Y;   #

print join "
", sort {$a <=> $b} grep { $A{$_} == 3} keys %A;

## Examenvraag
# Ge hebt een verzaeming, 1 tot 25 
# Je krijgt in die verzameling een deelverzameling (die tot die verzameling behoren)
# van alle getallen, tussen 1 en 25 die niet tot de deelverzameling behoren, het getal die dichtst bij hen ligt.
# 2 4 8 = deelverzameling
# Ik zit met het getal 5, 5 ligt het dichtst bij 4

## Normaal zijn dat maar 2 stapjes achtereen.


# zie foto in bestanden (labo 3)
```