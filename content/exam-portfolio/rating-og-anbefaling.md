---
title: "Rating og anbefaling"
date: 2026-10-27
draft: false
description: ""
---

## Hvad tilføjede jeg?

Jeg fik tilføjet 2 nye endpoints som er min post /api/movies/{id}/rating hvor man kan rate en film 1-10 og med en kommentar
og min anden endpoint get /api/recommendations, som giver max 5 film, som har en genre tilsvarende til hvad du rated 7 eller over fra før


## Hvilken ny teknologi brugte jeg?

- **JPQL** med parameter binding (`:username`, `:genreIds`)
- `JOIN FETCH` og `DISTINCT`, så film og genrer hentes i samme query
- **Subqueries** med `IN` og `NOT IN` til at finde film i de rigtige genrer, som brugeren ikke har ratet
- **Dirty checking** i Hibernate: en eksisterende rating bliver opdateret bare ved at ændre feltet inde i transaktionen
- `@UniqueConstraint` på `(user_id, movie_id)`, så en bruger kun kan have én rating per film
- **Java Streams** (`flatMap`, `map`, `limit(5)`) til at lave anbefalingerne
- Nye DTO'er: `RateMovieDTO`, `RatingResponseDTO` og `RecommendationDTO`
- Routes beskyttet med `AccessRole.USER`

## Hvorfor strukturerede jeg det sådan?
Altså min anbefalingslogik var at de film som skulle anbefales var nogle film som user ikke har rated før,
her valgte jeg så max 5 film skulle fremvises da det godt kan virke overvældne for personen hvis der er flere film der dukker op
og grunden til at jeg valgte at en film skulle først være rated 7 istedet for 9 eller 10 er fordi det ville formindske med en høj grad
hvor meget jeg kunne faktisk anbefale dem

Her gik jeg så også med rate igen = opdatere, hvor hvis en suer rater den samme film igen, så blive den gamle rating opdateret i stedet for at have 2 forskellige ratings fra 
samme user til en enkel film.

Grunden til at jeg ikke har placeret anbefalingsloggiken i DAO er fordi jeg ville have at DAO skulle kun hente data, imens mit service lager skulle være den der bestemmer
hvad der en god film, og hvor mange der skulle vises.


## Hvad virkede godt?

Den måde hvordan jeg ændre en brugers allerede eksisterende rating med at ændre setrating og setcomment, her opdater hibernate selv ændringen,
hvilket var lang bedre end at lave en update query.
jeg havde brugt @UniqueConstraint, hvilket stopper databsen i at gemme 2 forskellige ratings fra samme bruger om samme film,
hvilket har hjulpet en del med at stoppe duplikationer. 


## Hvad var svært eller forvirrende?

At skrive JPQL med subqueries, fx fik jeg brugt NOT IN og JOIN FETCH, det er ikke noget som jeg normalt bruger til quieries,
her var der også et tidspunkt hvor jeg manglede et mellemrum i en af mine queries hvor det så fik den hele til at gå i stykker.

Det var også svært at finde den rigtige User ud fra tokeneet fordi den har kun brugernavnet og derfor var jeg så nød til at lave en query, som
kunne slå brugeren op i databasen for at koble ratingen til den


## Hvad ville jeg gøre bedre næste gang?

Lige nu kan man ikke bare slette en film der er blevet rated af en bruger, det vil give 500, det er fordi at userens rating peger på filmen
og jeg har ikke nogen cascade.

Anbefalingerne er virkelig simple, man kunne nok havde gjort mere ud af den, fx kunne jeg nok havde gjort hvor den kigger på de film der er blevet
rated og hvilke genre der optræder mest i dem, og så anbefale ud fra det.


