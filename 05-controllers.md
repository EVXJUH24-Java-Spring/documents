# Controllers

En controller är ett objekt som är till för att hantera förfrågningar som kommer från användare (HTTP requests) och de svar som API:et vill ge tillbaka. De hanterar kommunikation mellan server och client.

För att en klass ska bli en controller används `@RestController`:

```java
package se.deved.spring;

import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {}
```

Annotationen gör så att Spring automatiskt hittar klassen och registrerar den som en controller, vilket görs i bakgrunden. Alla endpoints som skrivs in i controller klassen registreras också automatiskt av Spring. I Spring är en endpoint en metod som anropas, automatiskt av Spring, när en förfrågan kommer in till API:et. Det kan se ut såhär:

```java
package se.deved.spring;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {

  @GetMapping("/") // Sökvägen / namnet på endpointen
  public void myEndpoint() {
    System.out.println("A request was sent.");
  }

}
```

Lägg gärna till denna kod själv och starta applikationen. Du borde kunna skicka en förfrågan, via Bruno exempelvis, till `http://localhost:8080`, och se att `A request was sent.` skrivs ut i konsolen för Spring applikationen.

## Request hantering

Det finns några olika annotationer och klasser som kan användas på endpoints för att hantera requests. De beskrivs individuellt nedanför men de kan kombineras och användas flera gånger i samma endpoint.

### Methods

Varje HTTP request består av en HTTP metod. Man kan välja vilken metod en viss endpoint ska acceptera genom att sätta en av följande annotationer på metoden:

- `@GetMapping`
- `@PostMapping`
- `@PutMapping`
- `@DeleteMapping`

De tar alla in ett argument: URI:en som ska användas för endpointen. Exempel:

```java
package se.deved.spring;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {

  @GetMapping("/get-data")
  public void myGetEndpoint() {
    System.out.println("A GET request was sent to '/get-data'.");
  }

  @PostMapping("/add-data")
  public void myPostEndpoint() {
    System.out.println("A POST request was sent to '/add-data'.");
  }

}
```

Endast en annotation (av de ovanför) kan användas per endpoint.

### Headers

För att hämta headers från HTTP requesten används `@RequestHeader` på antingen enskilda parametrar i endpointen, eller genom att hämta alla headers samtidigt i en `Map`. Exempel:

```java
package se.deved.spring;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {

  // Hämta specifika headers.
  @GetMapping("/get-data")
  public void myGetEndpoint(
    @RequestHeader String host,
    @RequestHeader String someOtherHeaderName
  ) {
    System.out.println("A GET request was sent to '/get-data'.");
    System.out.println("'host' header = " + host);
  }

  // Hämta alla headers.
  @PostMapping("/add-data")
  public void myPostEndpoint(
    @Requestheader Map<String, String> headers
  ) {
    System.out.println("A POST request was sent to '/add-data'.");
  }

}
```

Det går även att ha ett eget namn på parametern men ändå koppla till en specifik header:

```java
package se.deved.spring;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {

  @GetMapping("/get-data")
  public void myGetEndpoint(
    @RequestHeader("actual-header-name") String myOwnParameterName,
    @RequestHeader("Content-Type") String someOtherName
  ) {
    System.out.println("A GET request was sent to '/get-data'.");
    System.out.println("'Content-Type' header = " + someOtherName);
  }

}
```

Man kan hantera olika datatyper. Spring omvandlar datatyperna automatiskt i bakgrunden. Om ett nummer förväntas (`int`) men en string skickas in så felhanterar Spring det också automatiskt. Med `int` kan det se ut såhär:

```java
package se.deved.spring;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {

  @GetMapping("/get-data")
  public void myGetEndpoint(
    @RequestHeader("some-number") int myNumber
  ) {
    System.out.println("A GET request was sent to '/get-data'.");
  }

}
```

### Path variables

Path variables är ett sätt att få information från användaren (likt kroppen i ett HTTP anropp) på ett enkelt och smidigt sätt. Informationen skrivs in direkt i URI:en och hämtas med `@PathVariable`:

```java
package se.deved.spring;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.PathVariable;

@RestController
public class MyController {

  // Exempel: /get-data/Ironman
  // Exempel: /get-data/JagGillarGlass
  @GetMapping("/get-data/{myVariable}")
  public void myGetEndpoint(
    @PathVariable String myVariable
  ) {
    System.out.println("A GET request was sent with a path variable of value '" + myVariable + "'.");
  }

}
```

Namnet på parametern måste stämma överens med namnet innanför URI:en. Testa gärna att lägga in koden själv och skicka iväg några requests, exempelvis: `http://localhost:8080/get-data/Hello` och `http://localhost:8080/get-data/A variable`.

Likt `@RequestHeader` kan man lägga till ett alias: `@PathVariable("actual-name")`. Det går även att använda andra datatyper, såsom nummer, på parametern.

### Query parameters

Query parameters är ett sätt att få information från användaren, likt path variables. Query parameters är bättre på att få in information om flera saker (medan path variables oftast bara tar en sträng så kan man med query parameters ta in flera strängar exempelvis). Hämta query parameters med `@RequestParam`:

```java
package se.deved.spring;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestParam;

@RestController
public class MyController {

  // Exempel: /get-data?param=hej&value=pådig
  @GetMapping("/get-data")
  public void myGetEndpoint(
    @RequestParam String param,
    @RequestParam String value,
  ) {
    System.out.println("A GET request was sent with a query parameter 'param' of value '" + param + "'.");
  }

}
```

Namnet på parametern bestämmer namnet på query parametern. Testa gärna att lägga in koden själv och skicka iväg några requests, exempelvis: `http://localhost:8080/get-data?param=Hello&value=there` och `http://localhost:8080/get-data?param=Pancakes&value=Icecream`.

### Request body

Det vanligaste sättet att ta in information, när det kommer till större saker, är genom request kroppen. Typiskt sätt används JSON men andra format stöds också av Spring. Hämta kroppen med `@RequestBody`:

```java
package se.deved.spring;

import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestBody;

@RestController
public class MyController {

  @PostMapping("/get-data")
  public void myPostEndpoint(
    @RequestBody String value,
  ) {
    System.out.println("A POST request was sent with the body '" + value + "'.");
  }

}
```

_Notera: body kan inte användas på GET requests._

Spring märker automatiskt av datatypen och parsar korrekt. Om ett objekt skickas in så antar Spring att det är JSON.

## Response hantering

Det finns några olika annotationer och klasser som kan användas för att hantera responses. Det bästa sättet i de flesta fallen är att använda `ResponseEntity` som kan göra allt.

### Response body

Bestäm kroppen för svaret med `.body()` eller direkt med `.ok()` (se status codes för mer information om de andra sakerna som visas här):

```java
package se.deved.spring;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.http.ResponseEntity;

@RestController
public class MyController {

  @GetMapping("/get-data")
  public ResponseEntity<String> myGetEndpoint() {
    return ResponseEntity.badRequest().body("Hello!");
  }

  @GetMapping("/get-data-two")
  public ResponseEntity<MyClass> myOtherGetEndpoint() {
    return ResponseEntity.ok(new MyClass("some value"));
  }

}
```

Klassen `ResponseEntity` bestämmer vilken datatyp som returneras. Innanför `<>` skrivs den. Om du vill returnera en sträng så skriver du `ResponseEntity<String>`. Om du vill returnera ett JSON objekt så skriver du klassens namn `ResponseEntity<MyClass>`.

### Status codes

Bestäm status kod på svaret med `.ok()` eller andra metoder:

```java
package se.deved.spring;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.http.ResponseEntity;

@RestController
public class MyController {

  @GetMapping("/get-data")
  public ResponseEntity<String> myGetEndpoint() {
    return ResponseEntity.ok("Hello!");

    // Eller om resursen inte finns
    // return ResponseEntity.notFound();

    // Eller om något gick snett
    // return ResponseEntity.internalServerError();

    // Det finns fler funktioner för andra status koder också.
  }

}
```

### Headers

Lägg till headers med `.header()`:

```java
package se.deved.spring;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.http.ResponseEntity;

@RestController
public class MyController {

  @GetMapping("/get-data")
  public ResponseEntity<String> myGetEndpoint() {
    return ResponseEntity
      .ok()
      .header("my-header", "my-value")
      .header("other-header", "other-value")
      .body("Hello!");
  }

}
```

Spring lägger till många headers automatiskt också, som `Content-Type` och `Content-Length`.
