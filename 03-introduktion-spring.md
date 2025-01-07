## Introduktion till Spring

Spring är ett ramverk som kan göra många saker och det används främst för att bygga webbapplikationer. Det går att bygga både frontend och backend med Spring men fokuset i denna kurs kommer vara specifikt på backend.

Att skapa en webbserver och därefter ett webb API kräver många steg. Först måste man sätta upp en HTTP server som kan kommunicera med andra datorer. Servern måste sedan kunna hantera alla anrop och därför behöver den kunna parsa HTTP, JSON och annat. När det är klart så måste det finnas såkallade "endpoints" för att webb API:et skall fungera. Allt detta tar mycket tid att skapa och det är därför man använder sig utav Spring. Spring har redan allt detta implementerat redan så att man kan fokusera på sitt eget (själva webb API:et).

Att programmera i Spring, även om det är i Java, är väldigt annorlunda från vanlig Java på grund av hur de har designat ramverket. Dependency Injection gör saker väldigt annorlunda exempelvis och därför kan det ta en stund att vänja sig och förstå vad som händer. Flödet är också annorlunda då utvecklaren har mycket mindre kontroll över programmet än vanligt (därför räknas Spring som ett ramverk och inte ett bibliotek).

## Spring Boot

Spring Boot är ett tillägg till Spring som erbjuder fördefinierade inställningar. Det används för att snabbare komma igång med Spring. Man kan säga att Spring är som att köpa möbler från IKEA: du måste bygga dem själv. Spring Boot är som att beställa förbyggda möbler.

## Controller, service och repository

Det är vanligt att använda controller-service-repository mönstret i Spring projekt. Det innebär att man delar upp webbserver i tre delar (främst, men det finns ofta fler delar):

1. **Controller:** ansvarar för kommunikation mellan server och klient
2. **Service:** ansvarar för validering av data, felhantering och "business" logik
3. **Repository:** ansvarar för datalagring, som att spara och radera information

Anledningen till att denna arkitektur är bra är främst på grund av flexibilitet, men också på grund av organisering av kod. För att tackla organiseringen först: det blir väldigt lätt att leta upp specifika fel om ansvaren är uppdelade. Säg att du jobbar på ett Spring projekt och en bugg dyker upp: när du gör ett anrop till servern så får du tillbaka ett resultat men inget verkar sparas i databasen. I detta fall är det högst sannolikt att felet ligger i eller mellan service och repository delarna. Du behöver inte ens kolla i controller delen.

Nu till att förklara flexibiliteten: fokuset ligger främst på testning, men det finns även andra fördelar. Säg nu att du har skrivit en ny webbserver och du vill testa koden med unit testing, men du har inte delat upp programmet i flera delar (såsom controller-service-repository). När du nu skall testa programmet, hur gör du för att testa individuella delar? Eftersom all kod ligger innanför samma komponent så blir det svårt att skilja ett ansvar från ett annat. Om du exempelvis vill se om datalagring fungerar och ett fel uppstår i testet, hur vet du då vad som är fel? Det kan vara flera saker: kommunikation med klient, validering av data, eller själva datalagringen. Problemet blir att en komponent har för många ansvar och det blir svårt att testa. Däremot, om koden delas upp i flera delar, som controller-service-repository, så kan ansvaren enkelt testas. Om man vill testa datalagring skriver man ett test specifikt för repository delen och kan då vara säker - om del uppstår - att det endast har med datalagring att göra.man ett test specifikt för repository delen och kan då vara säker - om fel uppstår - att det endast har med datalagring att göra.

## Dependency injection

Dependency injection är en stor del av att jobba med Spring. För att kunna skicka runt alla olika objekt som används - controllers, services, repositories och mer - utnyttjas Dependency Injection genom automatisk registrering av dependencies och automatisk injection. Dependencies i Spring kallas "beans".
