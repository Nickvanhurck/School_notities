# Computernetwerken II: Les 3

## routing

### routingtabellen bijwerken
(configuring iptable for dns: https://opensource.com/article/17/4/build-your-own-name-server)  

### routingprotocol: RIP
#### Linux (volgend labo op 01/03)
`ip r` (moderne versie, aangeraden): 
`route -n`: geeft routing tabel   
`route del -net ip_addr`: verwijderd route, ip_addr (172.16.0.0/20)  
`route add -net ip_addr dev eth2`: voegt route toe, ip_addr (172.16.0.0/20)    
`route add -net 4.0.0.0/8 gw 172.16.0.4`: route toevoegen naar router  -(gw=gateway)   
`ping ip_addr`: kijken of je deze kan bereiken.

`ip r`: routing tabel (ip r=ip route)   
`ip r d ip_addr`: verwijderd route  
`ip r a ip_addr`: voegt route toe  
`ip r a ip_addr via ip_addr`: zelfde als gateway(gw)  
`ip r r ip_addr via ip_addr`: past route aan (kan ook voor toevoegen gebruiken, aangeraden)   
`ip r f root ip_addr`: alle subnetten die deel uitmaken van het ip_addr worden verwijderd. Rechtstreeks aangesloten routes worden hier ook verwijderd.

`0.0.0.0/0`: (representeert alle netwerken)

Als je berichten stuur en je krijg geen antwoord, controleren of jij niet de fout bent! --> teller van uitgaande berichten gaat niet omhoog.

`traceroute`: om te zien waar je vast zit (diagnose in linux).

#### Windows
`tracert`: om te zien waar je vast zit (diagnose in windows).

# subnetting herhaling:

Decimaal: 4 groepjes van 3 `XXX.XXX.XXX.XXX`, tussen 0 en 255   
Binair: 4 groepjes van 8 bits `XXXX XXXX.XXXX XXXX.XXXX XXXX.XXXX XXXX`  

0:&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`00000000`    
255:&nbsp;&nbsp;`11111111`

#### Vb.:     
 - lijn 33: `192.168.2.10`     
 - lijn 34: `192.168.2.10`

Klasse A:     
Zo vallen de ip-adressen die beginnen met een getal van `0 tot en met 127` in het zogenaamde klasse A netwerk. Deze maken standaard gebruik van het `subnet 255.0.0.0`.

Klasse B:   
Voor een klasse B netwerk is afgesproken dat deze standaard worden voorzien van het `subnet 255.255.0.0.` Deze klasse geldt voor adressen die beginnen met het getal `128 tot en met 191`.

Klasse C:   
Een klasse C netwerk vervolgens begint zijn ip-adres met een getal tussen de `192 en als hoogste waarde 223`. Hierbij is het `subnetmasker 255.255.255.0`.

![ipv4 Klassen](https://www.master-it.nl/media/IPv4-subnetten-01.png)

#### Subnetting Voorbeeld:

192.168.2.64/26 (ip_addr/subnetmask)

1100 0000 . 1010 1000 . 0000 0010 . 01|00 0000 (ip in binair)    
1111 1111 . 1111 1111 . 1111 1111 . 11|00 0000 (subnet)

De | lijn zorgt voor een scheiding zodat je makkelijk kan zien welke blijven en welke bits je kan veranderen.   
`Links van |`: blijft onveranderd     
`Rechts van |`: kan veranderen

`#hosts?` van 0100 0001 (65) t.e.m. 0111 1110 (126)  
`Network address?` host gedeelte bevat allemaal 0000 0000 (64)   
`Broadcast address?` host gedeelte bevat allemaal 1111 1111 (127)   

