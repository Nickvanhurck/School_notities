# Computernetwerken II: Les 4
## OSPF: link state routing protocol
zie slides

Ofwel ben je een router volledig volgens OSPF ofwel niks (dus niet maar voor een deel).   

rip: volledige informatie enkel meld aan rechtstreekse buren.
ospf: enkel lokaal informatie vermeld aan alle ospf routers op het netwerk

### Hoe wordt de LSDB gesynchroniseerd?
- Zie slides

### werking OSPF (wiki)
OSPF is een opvolger van het RIP-protocol; de schaalbaarheid van RIP bleek onvoldoende in grotere netwerken. Een OSPF-router geeft regelmatig aan wat de status van haar interfaces is en, wat een verbinding "kost". Dit wordt berekend op basis van bandbreedte. Op basis van een kortstepadalgoritme, bijvoorbeeld dat van Edsger Dijkstra, wordt bepaald hoe een IP-pakket verstuurd moet worden.

OSPF is net als IS-IS een link-state protocol.

OSPF-routers communiceren met elkaar door middel van OSPF-packets. Deze packets hebben een eigen IP-protocol nummer (net zoals bijvoorbeeld TCP) en zijn dus niet afhankelijk van een hogere netwerklaag. Het initiële contact wordt gelegd door middel van multicast packets. Nadat de OSPF-routers elkaar ontdekt hebben zullen ze 1-op-1 verder communiceren en elkaars link-state database met elkaar uitwisselen. Hierbij is één van de routers de "master", de andere de "slave".

Op een broadcast-netwerk (zoals ethernet) zullen meerdere OSPF-routers een DR (designated router) en een BDR (backup designated router) kiezen. Alle non-DR routers zullen dan onderling geen link-state database uitwisselen, maar alleen met de DR en BDR. Op een point-to-point netwerk (zoals STM) zijn geen DRs.

### werking RIP (wiki)
RIP of Routing Information Protocol is een open netwerkprotocol waarmee routers aan elkaar doorgeven welke routes voorhanden zijn om IP-informatie naar zijn bestemming te sluizen. Het is een distance-vector routing protocol en gebruikt UDP voor transport van gegevens en het aantal tussenliggende routers (hops) als metriek.

RIP is gebaseerd op een aantal RFC's: RFC 1058[1] voor RIPv1 en RFC 2453[2] voor RIPv2. Deze laatste vervangt de oude RFC 1388[3] en RFC 1723.[4] RIP 2 biedt onder andere de mogelijkheid om gebruik te maken van subnetmasks.

RIP is een van de eerste routerprotocollen. Omdat het in grote netwerken steeds meer bandbreedte gebruikt, een hoplimiet van 15 heeft en de routingtable-updates langzaam convergeren wordt het meer en meer door protocollen als OSPF, IS-IS, Shortest Path Bridging en Border Gateway Protocol vervangen.

RIP is een interior gateway protocol.

## TODO
selectie designated router (DR) en backup DR
- in plaats van adjacencies met alle routers die je rechtstreeks kan zien
--> een DR definiëren 
- in plaats van elk subnetwerk 1 adjacency -> op x routers -> x-1 adjacencies
- enkel adjacency vormen met DR
- Bij verandering enkel aan DR laten weten
- Deze zal dit doorsturen naar de rest van de routers

elke kabel heeft 1 DR en 1 keer backup (router id bepaald dit)

RIP: max 50 routers 
OSPF: max +- 500 routers
- anders bottleneck (zie slides)
- gebruik van area's (om meer dan 500 routers te gebruiken)
- 1 backbone area die alle area's verbind

--> LABO VOLGENDE WEEK OSPF


# Labo 4
## RIP
### Config linux
```bash
ripquery -n <ip_addr> #genereren van general RIP, ip_addr van rip router

#linux instellen als rip router (verschillende alternatieven)
routed #deamon dat wordt geconfigureerd met 1 enkel bestand ("/etc/gateways")
gated #deamon dat kan functioneren als RIPv1, RIPv2, OSPF en nog andere routers

```

#### routed
/etc/gateways bevat voor elke route die niet via RIP kan aangeleerd worden, een lijn met als syntax:   
 `net|host <netwerk> gateway <router> metric <n> passive|active`   
 
 <b>passive:</b> onbeperkte lifetime.  
 Als routed actief is, heeft het geen zin om de routingtabel manueel aan te passen.  
 routed ondersteund enkel RIPv1 

#### gated
gated wordt ingesteld met behulp van het /etc/gated.conf bestand.    
Om een toestel met 2 interfaces als RIPv2 in te stellen zie voorbeeld pagina 38 onderaan.   
gated laat toe om voor elke interface van de router precies in te stelllen met welke routes rekening moet gehouden worden, welke geadverteerd worden en hoe geaggregeerd kan worden. gated kan niet alleen via een config bestand, maar ook dynamisch geconfigureerd worden, via de gdc opdracht.

Gated Interactive Interface (GII) biedt mogelijkheid tot interactie met gated deamon.

#### quagga software (alternatief voor gated)
individuele bestanden in de /etc/quagga map configureert elke deamon. Voor elk routing protocol een andere deamon.   
Fedora linux: een script met dezelfde naam als de daemon in de /etc/rc.d/init.d directory. Dit script aanvaardt doorgaans één enkel argument, met als mogelijke waarden stop, start of restart

bv:  
`# /etc/rc.d/init.d/zebra start`

