# Application Programming Interface (API)

Ett API, inom webbutveckling, är en del av en applikation som användare (klienter) kommunicerar med genom HTTP protokollet. Utveckling i Spring ramverket går till stor del ut på att skapa API:er som kan användas av andra. Webb API:er består av "endpoints". En endpoint är en funktion som hanterar kommunikation mellan klient och server. I Spring skapas endpoints i form av metoder som ligger i controllers.

Man kan tänka på endpoints som telefoner. Man kan ringa med dem och svara när andra ringer. När en koppling har öppnats kan kommunikation påbörjas. Samma egenskap finns hos endpoints. Man kan anropa endpoints, genom HTTP, som öppnar en kommunikationskanal.

Varje API har en viss design. Det vill säga att de har ett visst antal endpoints med specifika inställningar, sökvägar och funktionalitet. Typiskt sätt så har en endpoint ett specifikt ansvar. I en bank applikation så skulle några endpoints kunna inkludera:

- En endpoint som används för att överföra pengar
- En endpoint som används för att skapa nya bankkonton
- En endpoint som används för att hämta köphistorik

Vid design av endpoints måste man bestäma vilka HTTP metoder som skall användas, vilka HTTP headers som skall finnas, vilket format HTTP body:n skall vara i och mer.

Med syn till controller-service-repository arkitektur, som är vanligt förekommande i Spring applikationer, är endast controller-delen själva API:et. Services och repositories är inte en del av webb API:et. De används dock fortfarande i bakgrunden, av controllers.

En Spring controller med två endpoints kan se ut såhär (mer information i separat dokument):

```java
@RestController
public class MyController {

  @GetMapping("/get-account")
  public Account getAccountEndpoint(String accountName) {
    // Hantera request...
    // Svara:
    return account;
  }

  @PostMapping("/create-account")
  public String createAccountEndpoint(String accountName) {
    // Hantera request...
    // Svara:
    return "Created account!";
  }
}
```
