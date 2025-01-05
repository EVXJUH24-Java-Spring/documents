# Controller-service-repository arkitektur

Spring projekt brukar delas upp i tre delar: controllers, services och repositories. Delarna är till för olika saker och har olika ansvarsområden. Detta kallas för "arkitekturen" i ett Spring projekt.

Det är helt möjligt att använda en annan arkitektur, men "Controller, Service och Repository" arkitekturen rekommeras av bra anledningar. De viktigaste är:

1. Återanvändning
   - Tanken bakom arkitekturen är att dela upp ansvarsområden så att delarna hanterar olika saker. Resultatet av detta blir att delarna är "decoupled", vilket betyder att de kan stå för sig själva. På grund av detta kan de återanvändas; vissa delar kan till och med återanvändas utanför Spring.
2. Testbarhet
   - Eftersom delarna blir "decoupled" så är de också enkla att testa. Detta är på grund av att delarna ofta inte har några beroenden vilket man vill undvika i unit tester. Utan beroenden så kan de testas individuellt i enheter.
3. Anpassning
   - Eftersom delarna blir "decoupled" så är de också enkla att modifiera och utöka. Principen här är samma som för testbarheten: delarna har inga beroenden och då kan de modifieras på fler sätt utan att förstöra andra delar.
4. Förståelse
   - Eftersom delarna har specifika ansvar så blir det enkelt att förstå vilken del som gör vad. Om du exempelvis vet att du behöver lösa en bugg i logiken så kan du snabbt resonera fram till att det är service lagret som är fel eftersom det ansvarar för "business logic".

Typiskt sätt så följer delarna ett visst flöde när det kommer till kommunikation. Målet med ett webb API är generellt att lagra och uppdatera information.

- Controllers - Ansvarar för kommunikation mellan användare (webbläsare) och server. De tar emot requests och skickar responses i form an HTTP.
- Services - Ansvarar för "business logic" vilket är all logik specifik för applikationen (men har inget med protokoll, kommunikation (HTTP) eller datalagring (databaser) direkt att göra)
- Repositories - Ansvarar för datalagring och pratar ofta med en eller flera databaser

Dessa delar kombineras för att få in allt, eftersom en komplett applikation kräver alla: kommunikation, logik och datalagring. Flödet ser ut som följande:

```
User --> Controller --> Service --> Repository --> Service --> Controller --> User
```

När en förfrågan kommer in till API:et (till en endpoint i en controller), vilket användaren skickar iväg, så hanteras den av en controller:

```
User --> Controller
```

Controllern tar hand om informationen och skickar vidare det viktiga till en service som ska applicera logik:

```
User --> Controller --> Service
```

Ett bra exempel är:

En person vill skicka pengar till sin kompis och trycker på "Överför" knappen i bankappen. Knappen skickar en förfrågan till bankens API som tar emot den. Controllern som har tagit emot förfrågan skickar över viktig information till en service (vem som vill skicka pengar, hur mycket, till vem, bankkonton och så vidare) som gör själva överföringen av pengar. Logiken här är själva överföringen.

När servicen är klar med sin logik som behöver information ibland lagras eller uppdateras, och då anroppar servicen ett repository som tar hand om det (typiskt sätt genom att prata med en databas):

```
User --> Controller --> Service --> Repository
```

När det är klart så måste API:et skicka tillbaka information till användaren så att de vet vad som har hänt (exempelvis om överföringen lyckades eller inte):

```
User --> Controller --> Service --> Repository (här skickas information tillbaka i flödet) --> Service --> Controller --> User
```

Det är viktigt att allting relaterat till kommunikation mellan användare och API **endast** finns i controllers. Lika med services och logik. Lika med repositories och datalagring. Ett exempel på något som skall undvikas är:

Det är vanligt att använda en klass som heter `ResponseEntity` för att hantera API svar i controllers, då de ofta kräver JSON. Eftersom `ResponseEntity` endast har med kommunikation mellan API och användare att göra ska den aldrig användas innanför en service. Samma gäller principen bakom DTOs. DTOs är objekt som skickas mellan server och client, alltså kommunikationrelaterat, vilket innebär att de inte skall användas inom services och repositories.

## Uppdelning

I de flesta projekt finns flera modeller, som exempelvis användare, produkter, inlägg, kommentarer och bilder. Det är vanligt att dela upp controllers, services och repositories i enlighet med dessa modeller. Som exempel, med `users, posts & comments`, hade följande klasser funnits:

- Controllers
  - UserController
  - PostController
  - CommentController
- Services
  - UserService
  - PostService
  - CommentService
- Repositories
  - UserRepository
  - PostRepository
  - CommentRepository
