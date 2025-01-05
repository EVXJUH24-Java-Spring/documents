# Repositories

Ett repository är ett objekt som är till för datahantering och datalagring, typiskt sätt genom att kommunicera med en databas.

Ett exempel utan databaser kan se ut såhär:

```java
@Repository
public class UserRepository {

    private Map<String, User> users = new HashMap<>();

    public void save(User user) {
        this.users.put(user.getName(), user);
    }

    public void remove(User user) {
        this.users.remove(user.getName(), user);
    }

    public Collection<User> getAll() {
        return this.users.values();
    }
}
```

Kommunikation med databaser är lite speciellt i Spring, men väldigt enkelt att implementera. Ett repository med SQL implementeras med `implements JpaRepository<?, ?>`. I dessa fall, med databaser, skall modeller skapas genom klasser och några annotationer måste användas. Mer information om modeller i separat dokument.

Ett exempel med `JpaRepository` kan se ut såhär:

```java
@Entity
public class User {

    @Id
    public String username;
}

@Repository
public interface UserRepository extends JpaRepository<User, String> {}
```

Spring implementerar automatiskt en massa metoder som kan användas för att spara, uppdatera och radera information, så man behöver inte lägga till någon kod. `JpaRepository` tar in två generiska typer: den första är modellen/klassen som skall sparas i databasen, och den andra konfigurerar datatypen på id:t för modellen. Det är vanligt att använda `UUID` som id för modeller.

Ett annat exempel:

```java
@Entity
public class Image {

    @Id
    public UUID id;

    public String name;
    public byte[] data;
}

@Repository
public interface ImageRepository extends JpaRepository<Image, UUID> {}
```

Notera att man måste ansluta till en databas med konfigurationsfiler för att repositories skall fungera. Se databas sektionen nedan för mer information.

Med `JpaRepository` är det även möjligt att skapa egna queries, utöver de som kommer från Spring. Det finns två bra sätt att hantera det på:

A)

```java
@Repository
public interface UserRepository extends JpaRepository<User, String> {
    @Query("SELECT user FROM User user WHERE user.status = 1")
    Collection<User> findAllActiveUsers();
}
```

Använd `@Query` med ett SQL statement för att skapa en egen query, eller B)

```java
@Repository
public interface UserRepository extends JpaRepository<User, String> {
    Collection<User> findByEmailAddress(String emailAddress);
}
```

Det andra sättet gör så att Spring skapar queries baserat på metodens namn. Se [baeldung](https://www.baeldung.com/spring-data-derived-queries) för mer information om detta.

## Databaser

Spring har inbyggt stöd för databaser. För att ansluta till en databas, vilket Spring gör automatiskt, så lägger man in information om inlogg, address och annat i `application.properties`. Informationen som behövs skiljer beroende på vilken databas som ska användas. För att ansluta till en PostgreSQL databas:

```
spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=jdbc:postgresql://localhost:5432/database-name
spring.datasource.username=postgres
spring.datasource.password=password
spring.datasource.driver-class-name=org.postgresql.Driver
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
```

Man behöver också ha med dependencies så att Spring vet vilken databas som ska användas och hur den hanteras (konfigurationsfilen använder dessa dependencies). Om man vill använda PostgreSQL så måste följande dependencies inkluderas:

1. Spring Data Jpa
2. PostgreSQL Driver

... vilket kan se ut såhär med Maven:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <scope>runtime</scope>
</dependency>
```

... och såhär med Gradle:

```kts
dependencies {
  implementation("org.springframework.boot:spring-boot-starter-data-jpa")
  runtimeOnly("org.postgresql:postgresql")
}
```

Databasen kan nu användas genom repositories.
