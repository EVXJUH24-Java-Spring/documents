# Services

En service är ett objekt som är till för att hantera det som kallas "business logic". Business logik innebär att logik som är specifik för en applikation. I en webbserver för en bank så skulle pengar överföring kunna vara ett exempel på business logik. Business logik är helt beroende på applikationen.

Services kan skrivas hur som helst och behöver inte innehålla något speciellt. Skapa en service genom att annotera en klass med `@Service`:

```java
package se.deved.spring;

import org.springframework.stereotype.Service;

@Service
public class MyService {}
```

Det är vanligt att services pratar med en eller flera repositories för att lagra data. Det hanteras med dependency injection och kan se ut såhär:

```java
package se.deved.spring;

import se.deved.spring.MyBlogPostRepository;
import se.deved.spring.MyBlogPost;

import org.springframework.stereotype.Service;
import org.springframework.beans.factory.annotation.Autowired;

@Service
public class MyBlogPostService {

    private MyBlogPostRepository repository;

    @Autowired
    public MyBlogPostService(MyBlogPostRepository repository) {
        this.repository = repository;
    }

    public void saveBlogPost(MyBlogPost blogPost) {
        this.repository.save(blogPost);
    }

}
```

Mer om dependency injection finns i ett separat dokument.

Försök att separera kod för controller, repository och service. Kod relaterad till HTTP kommunikation bör hållas till controllers och skall inte finnas i services. Samma gäller för datalagring när det kommer till repositories och services.
