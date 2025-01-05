# HyperText Transfer Protocol (HTTP)

HyperText Transfer Protocol, förkortat 'HTTP', är ett protokoll som används på webben för kommunikation. Alla webbläsare använder HTTP för att kommunicera med webbplatser.

Ett protokoll är ett datorspråk som datorer använder för att kunna förstå varandra.

HTTPS är en version av HTTP som lägger till säkerhet genom kryptering.

HTTP har en request-response modell, vilket betyder att klienter (webbläsare, användare) skickar meddelanden, som kallas requests, till server som svarar tillbaka med ett meddelande som kallas response. En server kan inte skicka ett meddelande till en klient utan att den har fått in en request. Den som skickar en request kallas klient och den som skickar response kallas server.

För att kunna skapa API:er måste man förstå strukturen av HTTP meddelanden.

## Request

Varje request innehåller fyra delar: url, metod, headers och body. Såhär kan en hel HTTP request se ut i textform:

```sh
# Start linje: metod uri version
POST /images/save HTTP/1.1 

# Headers
Host: image-gallery.com
Accept: application/json
Content-Type: application/json
Accept-Language: en-US

# Body
{ "name": "my-image.png", "content": "..." }
```

### URL och URI

Denna del bestämmer vilken server meddelandet skall skickas till. Varje server har olika resurser som identifieras med URI:er som också bestäms i URLen.

```
<----------- URL ---------->
<------ HOST -----><- URI ->
https://youtube.com/videos
```

### Metod

Metoder är till för att ge en överblick över vad requesten är till för. Det finns nio stycken, men endast fyra av dem är viktiga. Det finns inga hårda regler för vilka metoder som skall göra vad, men generellt så betyder de följande:

- **GET** - Indikerar att man vill hämta information från servern
- **POST** - Indikerar att man vill uppdatera information som finns på servern
- **PUT** - Indikerar att man vill ladda upp ny information på servern
- **DELETE** - Indikerar att man vill radera information som finns på servern

### Headers

Headers är till för att ge information om själva meddelandet (detta gäller även för responses). Storlek, språk, format och addresser är några exempel på saker som finns i headers. Det finns många inbyggda headers i HTTP men man kan också lägga till egna.

Läs mer: <https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers>

### Body

Kroppen (body) är till för att skicka över information. Man kan säga att kroppen är själva innehållet för meddelandet. Responses har också en kropp. Det är vanligt att skicka data i JSON format här, men det går med andra format också, såsom HTML, XML och plain text.

API:er behöver veta vilket format datan skickas i och detta förmedlas genom `Content-Type`-headern. Den kan se ut såhär för JSON: `Content-Type: application/json`.

## Response

Varje response innehåller tre delar: status, headers och body. Headers och body fungerar på samma sätt som för requests.

```sh
# Start linje: version statuskod statusmeddelande
HTTP/1.1 200 Ok 

# Headers
Date: Wed, 02 Feb 2019
Content-Type: application/json
Content-Length: 39

# Body
{ "imageId": 231, "status": "success" }
```

### Status

Status delen består av en status kod och ett status meddelande. De är till för att informera klienten (den som har gjort anropet/requesten) om vad som har hänt. Om allt har gått bra så får man en "200 Ok" status. Om något har gått snett på servern så får man en "500 Internal Server Error" status. Och det finns många fler, se länken nedanför.

Läs mer: <https://developer.mozilla.org/en-US/docs/Web/HTTP/Status>
