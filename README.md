# design.regionhalland.se
Källkodsbibliotek för den kod som är unik för design.regionhalland.se

# Att skapa en egen utvecklings-miljö för design.regionhalland.se
Syftet med denna guide är att förklara hur man skapar en lokal utvecklingsinstallation av design.regionhalland.se för någon som behöver jobba med koden och har god IT-kompetens men är ovan vid de specifika verktygen. Det finns andra sätt att installera koden, men här tar vi med alla de verktyg som vi själva använder. Efter att ha gått igenom alla steg skall man ha en fungerande test-server igång och en grundläggande förståelse för de olika komponenternas funktioner. 

Förutsättningar: 
- En dator där du har behörighet att installera programvara. 
- Internetuppkoppling (utan allt för entusiastisk brandvägg). 

# Installation (Ofärdig beskrivning - EJ FÄRDIG FÖR BRUK)

## Versionshantering - installera "Git" om du inte redan har det. https://git-scm.com/
Git gör det enkelt att hantera ändringar i källkod och installeras på din fysiska maskin. Tröskeln kan kännas lite hög men belöningen är stor när olika personer kan ha flera versioner var av samma kod och samarbeta utan att det blir kaos. *Du behöver Git för flera kommande steg* - inklusive att kopiera koden från design.regionhalland.se och hantera de ändringar och eventuella förslag du gör. Det är vanligt att använda Git från kommandoraden men det finns även Git-klienter med grafiskt användargränssnitt för dig som föredrar att jobba grafiskt.

## Virtuella maskiner - Installera VirtualBox https://www.virtualbox.org/
Med virtualbox simuleras en helt separat maskin i din dator direkt i mjukvaran och det gör det enkelt att ge varje projekt du jobbar med en egen server. Den simulerade (virtuella) maskinen vet inte att den är en simulering utan du kan jobba med den som om du hade en dedikerad fysisk maskin - vilket också gör det *lätt att skapa en ny om du råkar förstöra din virtuella maskin*. En maskin du skapar manuellt i VirtualBox är helt tom och behöver installeras med operativsystem etc. Här hoppar vi dock över det eftersom vi strax får hjälp av andra verktyg att installera allt vi behöver. *Skapa alltså ingen virtuell maskin själv just nu*. 

Vi kommer använda den virtuella maskinen som server vilket kan låta konstigt om man är van att en server är en stor och dyr fysisk maskin. Faktum är att en server är en programvara som sköter en specifik uppgift (att lyssna efter anrop från andra datorer, skaffa fram innehållet de frågar efter och skicka det) och anledningen att man har speciella maskiner är att de skall orka med belastningen. I det här fallet räcker din vanliga dator gott eftersom webbplatsen bara har en enda användare. 

## Installera Vagrant - används senare för att skapa din virtuella maskin https://www.vagrantup.com/
Som nämndes ovan så är en virtuell maskin du skapar direkt i VirtualBox tom och kräver att man installerar operativsystem, webbserver osv för hand. Vagrant gör det möjligt att automatisera de här momenten och kommer i sin tur att användas av Trellis som innehåller allt det vi behöver för Wordpress-utveckling. 

## Installera Trellis - sätter upp hela Wordpress-servern åt oss https://roots.io/trellis/docs/installing-trellis/
Trellis är en mjukvara som använder Vagrant för att sätta upp en maskin åt oss i VirtualBox som innehåller en webbserver med Wordpress installerat utan att att vi behöver installera allting för hand. Följa helst instruktionerna direkt hos Trellis, men de viktiga stegen är följande: 

### Skapa den mapp där du vill ha ditt projekt. 
Från ett terminalfönster: gå till den katalog där du vill ha ditt projekt och skriv "mkdir design.test && cd design.test" för att skapa mappen och sedan gå in i den. 

### Ladda ned ("klona") Trellis med Git till din nya mapp
Från terminalen skriv "git clone --depth=1 git@github.com:roots/trellis.git && rm -rf trellis/.git". 
Nu har du Trellis-koden på din dator. 

### Klona även Bedrock
I terminalen: "git clone --depth=1 git@github.com:roots/bedrock.git site && rm -rf site/.git". 
Bedrock kommer från samma team som Trellis och används för att göra Wordpress lättare att administrera. 

### Konfigurera din kommande Wordpress-server
Innan din nya server kan skapas behöver du ställa in vilken URL du vill nå den på, vilka lösenord som skall gälla osv. Detta gör du i filerna "projektmapp/trellis/group_vars/wordpress_sites.yml" och "projektmapp/trellis/group_vars/vault.yml". Det du behöver ändra är t.ex. example.com till design.test i båda filerna och lösenorden om du så vill. 

## Låt Trellis skapa din nya webbserver med Wordpress
Väl kommen så här långt är du redo att faktiskt skapa din nya maskin som innehåller Wordpress och webbservern Nginx färdiginstallerat. Gå till "projektmapp/trellis" och kör kommandot "vagrant up" i terminalen. Nu bör en process börja där en ny virtuell maskin skapas och fylls med mängder av nödvändig programvara som också konfigureras för att vara färdig för drift. Det kan ta 5-10 minuter och under tiden visas ganska mycket output i terminalfönstret. Om allt går bra så får du i slutet ett meddelande om att maskinen är färdig. 
* För att stoppa maskinen - i "projektmapp/trellis" kör "vagrant halt".
* För att starta maskinen efter ett stopp - i "projektmapp/trellis" kör "vagrant halt"
* För att radera maskinen - i "projektmapp/trellis" kör "vagrant destroy". (Du kan sedan köra "vagrant up" igen för att skapa en ny). 

## Testa din nya virtuella webbserver
Gå till http://design.test (eller den URL du konfigurerade i "vault.yml" och "wordpress_sites-yml"-filerna. Du bör nu se en tom standard Wordpress-webbplats. 

## Yarn

## Composer


(Skaffa källkoden för design.regionhalland.se - "klona" med Git Klona koden för design.regionhalland.se till din lokala dator med Git genom att öppna ett terminalfönster, gå dit du vill ha mappen för projektet och skriva: git clone https://github.com/RegionHalland/design.regionhalland.se.git. (Här förutsätter vi att du använder vanliga Git och inte någon av de grafiska versionerna).Du har nu hämtat källkoden till din lokala dator och kopplat mappen till vad som händer online i kodbasen. Med hjälp av Git kan du i framtiden hämta nya ändringar från nätet eller skicka upp dina egna ändringsförslag. Bara källkoden räcker dock inte så långt - vi behöver någonting som kan omvandla den till en fungerande webb, men först skaffar vi en miljö där vi kan labba ostört utan att förstöra något på din vanliga dator: en virtuell server.)
