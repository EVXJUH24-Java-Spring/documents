# Dependency injection i Spring

Dependency Injection, förtkortning 'DI', är ett design mönster för att skapa och hantera beroenden (objekt) i kod. Ett beroende kan vara vad som helst, men typiskt sätt är det ett objekt som innehåller vissa funktioner eller viss data som ska användas. Konceptet består av två delar: dependency och injection.

Dependency refererar till de objekt, eller beroenden, som ska hanteras och skapas i ett projekt. I Spring kallas dependencyes för "beans". Vanligt förekommande "dependencies" i Spring är services, controllers, repositories och säkerhetsinställningar.

Injection refererar till processen av att hantera dessa beroenden så att system kan använda dem. Detta görs automatiskt i Spring med hjälp av annotationer som `@Autowired`.

Det finns flera sätt att skapa beans (dependencies) i Spring, men det vanligaste är genom `@Component`. Det kan se ut såhär:

```java
// Skapa en klass och annotera den med @Component
// Detta gör så att Spring automatiskt registrerar den som en bean och
//  kan skicka runt den med injection.
// Inget annat än att skapa klassen (och annotera) behövs.
@Component
public class UserDatabase {}
```

När en `@Component` ligger på en klass så kommer Spring automatiskt att upptäcka det och skapa en instans av den. Den kan sedan användas inom andra klasser med hjälp av `@Autowired`. Detta kallas 'injection' och det kan se ut såhär:

```java
// Eftersom klassen har @Component så kommer en instans skapas automatiskt.
@Component
public class UserManager {

    private UserDatabase userDatabase;

    // När instansen (UserManager) skapas av Spring så kommer Spring också att märka denna constructor med @Autowired.
    // Spring letar då upp 'UserDatabase' och skickar in det objektet (som också tidigare har skapats av Spring), eftersom den ligger som parameter för constructorn.
    @Autowired
    public UserManager(UserDatabase userDatabase) {
        this.userDatabase = userDatabase;
    }
}
```

När Spring skapar en instans av `UserManager` (vilket Spring gör för att den också är en bean) så hämtas `UserDatabase` instansen och skickas in i `UserManager` automatiskt.

För att skapa controllers, services och repositories används `@RestController`, `@Service` och `@Repository` i respektive ordning. Alla dessa innehåller `@Component` under huven så den behöver inte skrivas manuellt.

## Motivation och syfte

Varför finns och används dependency injection? Det finns två huvudanledningar: flexibilitet och organisation. I större projekt har man ofta väldigt många objekt — vi pratar tiotals services, controllers och repositories upp till 30-50 totala objekt. Tänk att hantera alla dem manuellt — eller inte, om du vill undvika huvudvärk. Med dependency injection kan Spring hantera alla dessa objekt åt dig, och trycka in dem där du behöver dem.

En annan anledning till att dependency injection är användbart: flexibilitet, speciellt inom testning.

När det kommer till flexibilitet och testning är dependency injection särskilt värdefullt eftersom det möjliggör enkel utbytbarhet av implementationer. Låt oss säga att du har en OrderService som behöver kommunicera med en databas via ett OrderRepository. Med dependency injection kan du enkelt byta ut den riktiga databasimplementationen mot en testklass när du kör enhetstester.

Utan dependency injection skulle din OrderService kanske se ut så här:

```java
public class OrderService {
    private OrderRepository repository = new PostgreSQLOrderRepository();
    // ... resten av koden
}
```

Med denna implementation är `OrderService` hårt kopplad till PostgreSQL-implementationen. Om du vill testa OrderService måste du antingen ha en riktig PostgreSQL-databas tillgänglig, eller göra omfattande ändringar i koden.

Med dependency injection ser det istället ut så här:

```java
public class OrderService {
    private final OrderRepository repository;
    
    public OrderService(OrderRepository repository) {
        this.repository = repository;
    }
    // ... resten av koden
}
```

Det kan verka som en alldeles för enkel, och kanske till och med onödig, ändring, men med den kan du nu kan du enkelt injecta olika implementationer:

- I produktionskod (vanlig kod): `new OrderService(new PostgreSQLOrderRepository())`
- I tester: `new OrderService(new FakeTestOrderRepository())`

Detta mönster, där klasser är löst kopplade till sina beroenden, ger flera fördelar:

- Enklare testning genom möjligheten att injecta mock-objekt (fake objekt)
- Flexibilitet att byta implementation utan att ändra användande kod
- Möjlighet att konfigurera olika beteenden för olika miljöer (utveckling, test, produktion)

Med Spring blir detta ännu smidigare eftersom ramverket hanterar injection åt dig. Du behöver bara deklarera dina beroenden, och Spring ser till att rätt implementation används baserat på din konfiguration.

Detta adderar mer substans till förklaringen av hur dependency injection stödjer testning och ger flexibilitet, med konkreta exempel som visar skillnaden mellan hårt kodade beroenden och dependency injection.

Dependency injection antar att OOP används genom interfaces och abstrakta klasser, för annars fungerar det inte alls lika bra.
