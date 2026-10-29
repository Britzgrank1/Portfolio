---
title: "REST API med Javalin"
date: 2026-10-17
draft: false
description: ""
---

## Hvad tilføjede jeg?
Her lavet jeg blandt andet MovieController, GenreController, MovieService, GenreService, Dto'erne og requests.http, hvilket
så gjorde at jeg kunne lave CRUD, i dette tilfælde fik jeg lavet noget post, get, put, delete som er det her CRUD hvor jeg får
testede diverse endpoints jeg har.


## Hvilken ny teknologi brugte jeg?

- **Javalin 6** som web-framework til routes og HTTP
- **Jackson** til at lave Java-objekter om til JSON og tilbage (`JavalinJackson`, så datoer bliver vist som `"2026-10-17"`)
- **DTO'er** (`MovieRequestDTO`, `MovieResponseDTO`, `GenreResponseDTO`), så entiteterne ikke sendes direkte ud
- Lagdelingen **Controller → Service → DAO**
- `ctx.pathParam`, `ctx.bodyAsClass`, `ctx.json` og `ctx.status` i controllerne
- `ApiException` og `app.exception(...)` til fejl i formatet `{ "status", "msg" }`
- **IntelliJ HTTP Client** (`requests.http`) til at teste endpoints

## Hvorfor strukturerede jeg det sådan?

Altså her valgte jeg at gå med 3 lag, hvilket er min Controller, Service og DAO.
Grunden til jeg har gjort det er fordi jeg gerne ville fordele nogle ansvarsområder,
her ved Controlleren ikke noget om min database og min DAO ved ikke noget om HTTP.
Her har jeg så også lavet DT'OER på grund af at min Genre og Movie peger på hinanden og 
det ville så gøre at der ville være et forevigt loop i Jackson, nu vil den så kun sen genre navnene i stedet for hele mit Genre objekt.




## Hvad virkede godt?

Jeg synes det var simpelt, med hvordan man kunne lave routes i Javalin og så derefter teste det i min requests.http
Synes også det var smart med hvordan man laver sine egne exceptions i det her tilfælde havde jeg jo en API exception,
hvilket ville sikre mig at mit program ikke bare ville crash, men faktisk give mig en besked hvis noget faktisk gik galt.


## Hvad var svært eller forvirrende?

Ville ikke sige svært men måske at vælge de rigtige statuskoder som 201, 204, 400, 404.
Dog havde jeg svært ved at få datoer til at stoppe med at komme ud som et array, indtil jeg fandt frem til WRITE_DATES_AS_TIMESTAMPS, hvor hvis man slår det fra
så vil jeg faktisk få det i et rigtigt format.
Jeg ville også kopier de tidligere routes jeg havde i main og lidt glemme at skifte controlleren og metoden, men det var noget jeg fik rettet
hurtigt op på.



## Hvad ville jeg gøre bedre næste gang?

Der er steder hvor jeg fx har lavet den samme metode flere steder, hvis vi fx kigger i MovieService og RatingService.
Her har jeg toMovieDTO i RatingService og TODTO i MovieService. Her kunne jeg nok bare havde lavet en hjælpemetode og brugte den begge steder,
istedet for at jeg har lavet den 2 gange. 
Det er også det samme i min Controllers, hvor jeg har lavet getId til hver af dem, når jeg nok bare kunne havde lavet 1 og kaldt på metoden i alle mine controller.

