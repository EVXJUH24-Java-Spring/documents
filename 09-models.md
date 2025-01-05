# Models

En modell är en klass som representerar ett objekt, som skall sparas i databas, i en applikation. Exempel på modeller kan inkludera: produkter, citat, todos, användare, inlägg/posts, kommentarer, väderdata och sport data.

Det är egentligen inget speciellt med modeller, då de endast är vanliga klasser, men det finns några saker som är bra att veta när det kommer till modeller i just Spring, specifikt vid användning av SQL.

För att en modell (klass) skall kunna sparas i databas så måste `@Entity` användas:

```java
import jakarta.persistence.Entity;

// Detta markerar klassen som en "modell".
// Entity är ett annat ord för modell.
@Entity
public class User {}
```

`@Id` annotationen användas för att informera Spring om vilken variabel som ska agera som SQL id (primary key):

```java
import java.util.UUID;

import jakarta.persistence.Entity;
import jakarta.persistence.Id;

@Entity
public class User {

    @Id
    private UUID id;
}
```

Om du vill att Spring/databasen genererar id:n åt dig så kan du använda `@GeneratedValue`:

```java
import java.util.UUID;

import jakarta.persistence.Entity;
import jakarta.persistence.Id;

@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private UUID id;
}
```

Spring hanterar allt annat automatiskt, såsom att skapa tabeller med rätt kolumner och datatyper. Du kan dock ändra på vissa saker, exempelvis kolumn namn, med `@Column`:

```java
import java.util.UUID;

import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.Id;

@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private UUID id;

    @Column(name = "username", length = 30, nullable = false)
    private String name;

    @Column(nullable = true, unique = true)
    private String email;
}
```

Modeller används typiskt sett tillsammans med repositories (JpaRepository).
