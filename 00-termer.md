# Termer, begrepp och förkortningar

## Webb

En del av internet som innehåller webbplatser och webbsidor. En webbplats är en applikation som finns på webben. En webbsida är en specifik "sida" som finns på en webbplats. En hemsida är huvudsidan för en webbplats. Dessa termer brukar blandas även om det finns tekniska skillnader mellan dem.

## HyperText Transfer Protocol (HTTP)

Ett protokoll, eller dator språk, som datorer på webben använder för att prata med varandra och förstå varandra.

## Application Programming Interface (API)

_i webb sammanhang_

Den delen av kod, i en webbapplikation, som användare (med webbläsare exempelvis) kan kommunicera med genom HTTP protokollet. Det är typiskt sett en server med endpoints som används för att kommunicera med servern. All kod har tekniskt sätt ett API och alla webbservrar har tekniskt sätt ett API även om de är olika.

## Endpoint

_i webb sammanhang_

En funktion inom en webbapplikation som har ett ansvar, ofta CRUD. Det kan som exempel vara att spara bilder, söka information som personer och gilla blogginlägg.

## Backend

_i webb sammanhang_

Koden för en webbapplikation som får allt att fungera i bakgrunden. Den har ofta en koppling till en databas för att lagra data och kommunicerar med en frontend.

## Frontend

Koden för en webbapplikation som kan synas visuellt. Denna del är ofta byggd med HTML, CSS och JavaScript. En frontend kan bestå av knappar, boxar, tabeller, navbars och mer.

## Spring

Ett Java ramverk för webbutveckling med mycket stöd.

## Spring Boot

Ett fördefinierat sätt att konfigurera ett Spring projekt för att komma igång snabbt. Kommer med inställningar och användbara bibliotek.

## Dependency Injection (DI)

Ett design mönster som används mycket inom Spring och Java.

## Bean

Spring kallar dependencies (enligt Dependency Injection) för "beans".

## @Autowired

Spring använder `@Autowired` för att hantera injection enligt Dependency Injection.

## Model

En klass som skall representera en struktur i en databas, typiskt sätt en tabell. Instanser av modeller representerar rader i tabellen.

## Controller-Service-Repository arkitektur

En uppdelning av komponenter som är populär i Spring, Java och andra ramverk.

## Controller

Hanterar kommunikation med klient och användare genom att ta emot requests och svara med responses. Pratar ofta med en eller flera services.

## Service

Hanterar businesslogik för applikationen. Det kan vara felhantering, validering, koppling mellan objekt och mer. Pratar ofta med en eller flera repositories.

## Repository

Hanterar koppling till databas och datahantering. Svarar ofta på en förfrågan från service.

## Lombok

Ett Java bibliotek som kan implementera mycket boilerplate kod, som exempelvis getters, setters och constructors.

## Data Transfer Object (DTO)

Ett objekt som skickas mellan server och client. Inom backendutveckling kallas alla objekt som skickas i HTTP responses för "DTOs". Med andra ord, om en server skickar information till en client så kallas informationen för en "DTO".

## Bruno (applikation)

En HTTP client som kan användas för att testa API:er och lära sig HTTP.

## Object-relational mapper (ORM)

Ett ramverk med automatisk databashantering. Klasser bildar modeller som bildar tabeller. Relationer och joins hanteras automatiskt.

## Hibernate

En populär ORM för Java.

## Token

En hemlig sträng som kan användas för inloggning och autentisering, men även annat.

## Secret

En hemlig sträng som fungerar som ett lösenord för en säker algoritm, exempelvis kryptering.
