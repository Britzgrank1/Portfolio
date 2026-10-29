---
title: "Security med bcrypt og JWT"
date: 2026-10-26
draft: false
description: ""
---

## Hvad tilføjede jeg?

Jeg fik tilføjet register, login, hashing af passwords, JWT-tokens, roller som user og admin, og beskyttelse af routes,
så fx kun admin kan oprette, rette og slette film. 


## Hvilken ny teknologi brugte jeg?

- **jBCrypt** til at hashe passwords (`BCrypt.hashpw` med `gensalt(12)` og `BCrypt.checkpw` ved login)
- **JWT** med biblioteket `com.auth0:java-jwt`, signeret med HMAC256 og med udløbstid
- Lærerens `TokenSecurity`-klasse til at lave og verificere tokens
- Headeren `Authorization: Bearer <token>`
- `AccessRole`-enum (`ANYONE`, `USER`, `ADMIN`), som implementerer Javalins `RouteRole`
- `app.beforeMatched(...)` til `authenticate` og `authorize`, som kører før controlleren
- `@ManyToMany` mellem User og Role med join-tabellen `user_role`
- `@JsonProperty(access = WRITE_ONLY)` i `UserDTO`, så passwordet aldrig bliver sendt ud
- Statuskoderne **401** (ikke logget ind / ugyldigt token) og **403** (forkert rolle)

## Hvorfor strukturerede jeg det sådan?

grunden til at jeg hasher passwords er fordi hvis databsen bliver lækket, så kan man ikke læse adgangskoderne direkte der fra,
det er også derfor mange bruger bcrypt det som jeg også har gjort, fordi det er med vilje langsomt, så det tager markant længere tid at brute force.
Jeg bruger også JWT fordi serveren ikke behøver at huske hvem der er logget ind, det hele er igennem deres token, som deres brugernavn, rolle og udløbstid.
Så fik jeg så også lavet en LoginDTO og UserDTO grunden til det er fordi jeg gerne ville dele det op i 2 dele, hvor den ene håndere inputs som username og password og den anden
skulle håndtere outputs. Grunden til at passwordet aldrig bliver sendt ud er fordi jeg bruger WRITE_ONLY.



## Hvad virkede godt?

Noget der virket godt var at roller kunne sættes direkte på routes, hvor jeg fx har skrevet i slutning af en route AccessRole.Admin, det gør det også mere
tydeligt for mig, med hvilke roller kan se hvilke routes.

Noget der også virker virkelig godt er Bcrypt når adgangskoder saltes, det beskytter mod Rainbow Tables, især når samme adgangskoder, ender med at se helt forskellige
når det er blive hashet og saltet. 

## Hvad var svært eller forvirrende?

Prøvede at bruge de eksempler der var på kurset, men fordi jeg brugte en ældre Javalin så var syntax lidt anderledes ud med fx app.beforeMatched
som jeg skrev, men i min lærens eksempel stod der config.routes.beforeMatched, istedet for at skifte version til 7.0 så fandt jeg bare ud hvordan det
skulle stå på min version.

Synes også flowet fra login - token - header - authenticate - authorize, var en meget svær process at sætte sig ind.


## Hvad ville jeg gøre bedre næste gang?

Jeg har brugt min SECRET_KEY lokalt og på serveren, rigtigt så burde jeg nok havde lavet 2 forskellige i tilfælde af hvis den ene blev lækket.

til flowet med login - token - header - authenticate - authorize, så kunne jeg måske havde tegnet det hvilket kunne havde hjulpet mig med at visualisere det.

På nuværende tidspunkt så for at blive Admin så skal det bliver gjort manuelt i databasen, jeg har ikke lavet noget sted hvor en admin kan give en bruger admin-rettigheder, hvilket
måske kunne havde været rart af have.

Jeg har heller ikke stillet krav til hvordan password til brugeren skal se ud, her kunne jeg muligvis havde sagt at der skulle være små/store bogstaver og at koden skulle være minimum 8 tegn lang.

