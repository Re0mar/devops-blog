# Monitoring met New Relic

*Daniël Groenendijk*, oktober 2023

Een van de voornaamste voordelen van het gebruik van Kubernetes binnen een applicatie is de mogelijkheden die het bied rondom het schalen van beschikbare services om er zo voor te zorgen dat er nooit te weinig processie kracht is voor een service en dat dit ook niet overbodig gebruikt wordt. Andere voordelen zijn aanwezig rondom het automatisch herstarten van gecrashte services, het versimpelen van onderling netwerken en het toevoegen van eenvoudige load balancers. Hoewel deze features veel toevoegen en ondersteunen rondom het beheren van micro-services applicaties kunnen ze niet optimaal gebruikt worden op het moment dat de developer geen overzicht heeft op de performance van de individuele services of de connecties hiertussen. Dit is het gat dat monitoring services zoals New Relic invullen door het aanbieden van overzichten rondom de performance van applicaties, services, endpoints of zelfs individuele requests zodat het voor een ontwikkelaar snel duidelijk is op welke punten de services aanpassing vereisen in de configuratie, schaling of misschien zelfs in de code doordat het afhandelen van requests te lang duurt. Met deze data kan er snel worden aangetoond dat de afspraken rondom performance worden nageleefd, dat er een bottleneck aanwezig is binnen de applicatie of dat er onnodige kosten gemaakt worden op een service.

Een applicatie waarbinnen dit overzicht kan worden gebruikt is [Pitstop](https://github.com/EdwinVW/pitstop). Binnen deze applicatie wordt de administratie, planning en het contact met klanten beheert voor het garage bedrijf Pitstop. Om deze verschillende functionaliteiten te kunnen ondersteunen bestaat de applicatie uit meerdere micro-services die gezamenlijk functioneren binnen een Kubernetes cluster. Omdat het binnen deze usecase gaat om een kleiner bedrijf met wissende vraag naar de functionaliteit die de applicatie brengt is er hier een grotere kans op de risico's uit de vorige alinea terwijl de potentiële uitkomsten rampzalig kunnen zijn voor het bedrijf. Om deze reden worden de toegevoegde waardes van New Relic voor Pitstop met de eventuele nadelen in dit artikel in kaart gebracht, ook volgt een korte instructie voor het toevoegen van New Relic aan een project.

![New Relic Dashboard](images/dashboard-ui.webp)

## Functionaliteiten

De features die New Relic aanbied worden bereikt door het integreren van de New Relic APM binnen de services en vervolgens de gewenste agents toe te voegen aan de services waarvoor de observaties gewenst zijn. Er is een grote variatie op het gebied van mogelijke agents waardoor het mogelijk is om de web transacties binnen een service te monitoren met een agent terwijl een andere soort meetrieken verzamelt van een Java Virtual Machine(JVM). Doordat er agents beschikbaar zijn voor Java, .NET, C, Ruby, PHP, Javascript en nog een aantal andere platformen is het altijd mogelijk een agent te vinden die past binnen de codebase van het project. De functionaliteiten die deze agents leveren met hun observaties en het bijbehorende web platform zijn als volgt.

### Meetrieken

New Relic is in staat gegevens te verzamelen rondom de web transacties die plaats vinden binnen de applicatie, hieronder vallen de gemiddelde response snelheid en errors over een selecteerbare tijdsperiode samen met het aantal afgehandelde requests per minuut. Hiernaast worden ook de interacties met database engines en de runtime statistieken van JVM's verzamelt waardoor het precies te zien is wat voor invloed een request op de applicatie heeft en het automatisch weergeven van request of queries die opmerkelijk traag zijn vergeleken met de rest van de applicatie om deze te laten optimaliseren. Doordat zowel inkomend als uitgaand web verkeer ondersteund wordt voor monitoring is het niet alleen mogelijk eigen services te observeren maar ook om te kunnen valideren dat extern gebruikte resources niet voor onnodige vertraging zorgen.

### Foutafhandeling

Naast het observeren van de globale gebruiksstatistieken bied New Relic ook een dashboard met de errors die plaatsvinden binnen de applicatie. Ieder van deze errors kan in detail worden weergeven waarbij de service, request en stack trace weergeven worden. Doordat er op deze manier niet alleen een beeld is van de foutmelding maar ook van de handelingen die gedaan zijn door de gebruiker die hiertoe leden is het voor een administrator makkelijk om deze te documenteren en na te spelen waardoor er een betere opdracht kan worden gemaakt voor de ontwikkelaars en er zo meer tijd besteed kan worden aan het voorkomen en oplossen van bugs in plaats van het uitzoeken wat een gebruiker heeft gedaan om het systeem te laten falen.

### Logboeken

Het bijhouden van een logboek van de gehele applicatie is een ander feature dat het gebruik van New Relic aantrekkelijk kan maken. Doordat de logs gefilterd worden op niveau en vervolgens worden samengevoegd en weergeven op een pagina is het mogelijk in een blik een beeld te krijgen van de recente activiteiten zonder dat dit vereist dat de logs van meerdere services individueel moeten worden bekeken. Hiernaast is dit logboek te bereiken via het web dashboard waardoor het niet vereist is om overweg te kunnen met een command line interface om de historie te kunnen uitlezen, dit staat toe dat ook medewerkers met minder technische kennis snel kunnen zien waar en wanneer een eventuele handeling is gebeurt.

### Rapporten

Een feature wat mogelijk als minder belangrijk overkomt voor een ontwikkelaar is de mogelijkheid om wekelijks rapporten te versturen naar belanghebbenden, deze rapporten bevatten een samenvatting van de beschikbaarheid, performance en fouten die er gedurende de week zijn opgetreden. Hoewel er hierin geen details worden weergeven waaraan een ontwikkelaar wat heeft levert het wel een goede tool om bij managers, aandeelhouders en klanten te kunnen aantonen dat de applicatie werkt volgens de eisen uit bijvoorbeeld een SLA. Het automatisch aanmaken van deze rapporten zorgt er zo voor niet alleen voor dat het aantonen van het voldoen aan deze eisen geen extra tijd kost maar ook dat een ontwikkelaar zich kan richten op het ontwikkelen van het product in plaats van het geruststellen van het management.

## Tekortkomingen en zorgpunten

Hoewel New Relic overkomt als de ultieme tool voor het monitoren van een applicatie zijn er een aantal punten die het gebruik minder aantrekkelijk kunnen maken, hierbij gaat het natuurlijk over de kosten voor het gebruik maar ook andere zaken zoals privacy beleid en data bezit. Omdat New Relic het monitoren van applicaties aanbied als een service en hierbij gebruik maakt van hun eigen servers en hosting zal een gebruiker afhankelijk zijn van New Relic in plaats van dat ze dit zelf beheren. Dit zorgt ervoor dat er veel vrijheid verloren gaat rondom het beheer maar er hiervoor dus ook geen extra tijd hoeft te worden besteed.

### Kosten

![New Relic Pricing](images/relic-pricing.webp)

Zoals te verwachten valt voor een tool die zoveel functionaliteit bied is het gebruik van New Relic niet gratis, hoewel er bij het registreren en installeren eerst wordt gewerkt met een gratis proef account is het gebruik hiervan gelimiteerd tot 100gb aan data per maand zonder de mogelijkheid om extra gebruikers toe te voegen. Op het moment dat er meer gebruikers vereist zijn op het platform komen de kosten op $10 voor de eerste gebruiker en $99 voor iedere die hierna volgt. Hiernaast kost iedere gb aan data die door New Relic wordt afgehandeld $0.30 op het moment dat de grens van 100gb wordt overschreden. Deze betaalde standaard versie is vereist voor het gebruik van de geavanceerde features en voor technische ondersteuning.

### SaaS

New Relic is een Software as a Service product wat inhoud dat alle hosting en dataopslag plaatsvind op servers van New Relic. Dit is prettig op het gebied dat een klant zich geen zorgen hoeft te maken over het hosten van New Relic of de onvermijdelijke problemen die komen met het opslaan van vertrouwelijke informatie. Helaas betekent dit ook dat New Relic in het bezit is van de technische details van de applicatie en deze kan doorverkopen aan derde partijen of gebruiken op andere manieren dan dat gewenst is. Dit risico rondom data integriteit zal voor ieder project opnieuw een aandachtspunt worden. Het ontbreken van de optie om New Relic zelf te hosten zorgt er dan ook voor dat het beleid rondom gegevens opslag het gebruik van New Relic binnen een project kan vereisen of verbieden.

### Overhead

Omdat New Relic voor het verzamelen van gegevens moet worden toegevoegd aan de services die geobserveerd worden en het verzamelen en versturen van deze gegevens niet vanzelf gaat zorgt het toevoegen van New Relic voor een kleine toename in de overhead van een applicatie en kan daarmee mogelijk voor een afname in presentaties leiden. Hoewel deze afname in de meeste gevallen niet merkbaar is zal er rekening mee moeten worden gehouden dat deze processen tijd kosten en ook voor een toename in netwerk verkeer zullen leiden wat voor hogere kosten kan zorgen wanneer er gebruik gemaakt wordt van hosting waarbinnen dit geld kost.

### Aanbod

Een nadeel van het grote aanbod aan services dat wordt aangeboden door New Relic is dat moeilijk kan zijn een precies beeld te krijgen bij welke functionaliteiten vereist worden binnen een applicatie en er daarom onnodige agents worden toegevoegd, ook is het een mogelijkheid dat te features die New Relic aanbied te geavanceerd zijn voor de usecase waarbij het beter kan zijn te kiezen voor een simpelere oplossing.

## Installatie

- Set up van cloud dashboard && Integratie met python dare2date applicatie

Het opzetten van New Relic bestaat uit twee gedeeltes, het eerste en belangrijkste is het registreren bij New Relic wat gedaan moet worden op het [online platform](https://newrelic.com/signup). Zodra dit aanmeldproces is doorlopen is er een centraal dashboard aangemaakt zonder data sources. Om deze bronnen toe te voegen kan er gebruik worden gemaakt van het ```Add data``` menu waarbij er gekozen moet worden voor de specifieke bron van de gegevens.

![Toevoegen Data source](images/adddata.png)

Voor het bijvoorbeeld toevoegen van een Python applicatie als databron wordt er eerst een applicatie naam vereist om deze bron te kunnen identificeren. Hierna bestaat het installatie proces simpelweg uit het gebruiken van ```pip install newrelic``` en het toevoegen van een ```newrelic.ini``` bestand. Hierna kan het programma gestart worden met ```NEW_RELIC_CONFIG_FILE=newrelic.ini newrelic-admin run-program python app.py```. Op het moment dat dit is uitgevoerd zal de connectie binnen enkele minuten zichtbaar worden op het dashboard en kunnen er meetrieken verzameld worden.

## Conclusie

Zoals te concluderen valt is New Relic een geweldige tool voor het monitoren van applicaties of deze nu een monolith of micro-services structuur hebben en er zo voor te kunnen zorgen dat eventuele problemen die optreden snel geïdentificeerd en gerepareerd kunnen worden. Hiernaast brengt de optie tot het automatisch versturen van statusrapporten een eenvoudige oplossing voor het inlichten van belanghebbende met minder technische kennis. Door de grote hoeveelheid aangeboden features en de bijkomende prijs is het gebruiken van New Relic misschien niet het meest aantrekkelijk voor kleinere bedrijven of individuele projecten maar zal het zich al snel onmisbaar tonen bij grote en complexe projecten waarbij het verzamelen en benutten van runtime gegevens vereist is voor de levensduur en performance van de applicatie.

## Bronnen

- <https://learn.newrelic.com>
- <https://medium.com/tensult/what-is-new-relic-8106dfde903d>
- <https://www.techtarget.com/searchitoperations/tip/Learn-how-New-Relic-works-and-when-to-use-it-for-IT-monitoring>
- <https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-pricing-billing/new-relic-one-pricing-billing/#how-pricing-works>

## Repo

<https://github.com/Re0mar/devops-blog>
