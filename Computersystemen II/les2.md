# Computernetwerken II: Les 1
Stappenplan:

  - voeg volgende lijn toe aan /etc/resolv.conf als lab host ip is 192.168.0.203    
    name server            192.168.0.203   
    ofwel verwijder /etc/resolv.conf (ook een opl)   
    --> als het gelukt is zou een ping niet succesvol uitgevoerd kunnen worden.   
  - aanpassen van /etc/named.conf: 
            
```
zone "." IN {
        type hint;
        file "named.ca";
};
```

https://opensource.com/article/17/4/build-your-own-name-server   
http://www.zytrax.com/books/dns/ch8/soa.html  
https://www.interserver.net/tips/kb/bind-reverse-dns-example-setup/

# Computernetwerken II: Les 2

reverse DNS  (wat?)

zone bestand: 

```
@           IN SOA naam.iii.hogent.be. root (
                ...
            )

```

## Opdracht:  DNS (windows)
  +  dnsmgmnt.msc uitvoeren (Windows + R)
  +  rechtermuisknop pp DNS --> All Tasks --> Restart (om wijzigen door te voeren)
     +  Reverse Lookup zones: hieronder bestanden (zonebestanden)

## labo uit te voeren:
1. 

