---
title: "Data fra TMDB"
date: 2026-10-18
draft: false
description: ""
---

## Hvad tilføjede jeg?

Her fik jeg tilføjet TmdbClient som henter populære film og genrer fra deres Tmdbs API og så har jeg også ImportService,
som jeg har til at gemme dem i min egen database

## Hvilken ny teknologi brugte jeg?

- **TMDB API** (The Movie Database) som ekstern datakilde, med endpoints for populære film og genrer
- Javas indbyggede **HttpClient**, `HttpRequest` og `HttpResponse` til at kalde API'et
- **Jackson `ObjectMapper`** til at læse JSON-svaret (`JsonNode` og `readerForListOf`)
- Egne DTO'er til TMDB-data (`TmdbMovieDTO`, `TmdbGenreDTO`)
- `@JsonIgnoreProperties(ignoreUnknown = true)` og `@JsonProperty("release_date")` til kun at tage de felter jeg skal bruge og mappe TMDB's navne til mine
- `ImportService`, der henter genrer og film og gemmer dem i databasen via mine DAO'er
- API-nøglen ligger i en **miljøvariabel** (`System.getenv("API_KEY")`), så den ikke står i koden

## Hvorfor strukturerede jeg det sådan?

Altså fik strukturerede det så TmdbClient og ImportService er adskilt fordi jeg gerne ville have at den ene snakker med TMDB imens den
anden gemmer det i databsen, filmene fra TMDB indeholder kun genre id'er og ikke hele genrer derfor gemte jeg genrene først i databsen så
jeg derefter kunne koble det til filmene.
Jeg har også en metode getByTmdbId som tjekker om filmen allerede findes, så den ikke bliver gemt to gange hvis man importere igen.
Så fik jeg også lavet specifikke TMDB dto'er fordi TMDB har et andet JSON format som jeg ikke var interesseret i, når jeg fx kun ville have release_date og vote_average


## Hvad virkede godt?

Noget som virkede godt var ignoreUnknown = true, hvilket gjorde at jeg ikke skulle mappe alle TMDB felterne,
det har jo så gjort det lettere for mig at indhente de præcise ting jeg gerne ville have


## Hvad var svært eller forvirrende?

Jeg havde svært ved at koble TMDB's genre id med mine egne Genre entiter. Jeg fik også slettede min tidligere TmdbService og MovieDTO
da det var fra min gamle SP1 opgave, hvor jeg troede jeg kunne genbruge det hele, hvilket jeg kom frem til at jeg ikke kunne.


## Hvad ville jeg gøre bedre næste gang?

Jeg blev fortalt af min underviser at tråde kunne havde været en god ide til det her projekt, det fik jeg ikke gjort men kunne havde været et godt valg,
da det nok vil havde fået min import fra TMDB til at blive hurtigere, da istedet for at den køre et kald af gangen så ville den så kunne køre flere på samtid.


