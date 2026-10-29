---
title: "Test af DAO og REST"
date: 2026-10-28
draft: false
description: ""
---

## Hvad tilføjede jeg?

Her tilføjede jeg 7 tests til min MovieDAOTest klasse, som er getAll, getById, getById_withMissingId, create, update,
delete, getByTmdbId.

Jeg fik også lavet 6 tests til min RestApiTest, GetallMovies, Register_withoutpassword, Login_withWrongPassword, rateMovie_withoutToken
og examStory_register_login_rate_recommend og rateMovie_withinvalidRating.

Fik også tilføjet logging på dette tidspunkt hvor hver request blivver logget med metode, path, statuskode og hvor lang tid den tog
hvilket ville se sådan her ud: GET /api/movies 200 (12 ms), hvor jeg både bruger WARN til klientfejl, INFO når det går godt og Error til serverfejl. 

## Hvilken ny teknologi brugte jeg?

- **JUnit** med `@Test`, `@BeforeAll`, `@BeforeEach` og `@TestInstance(PER_CLASS)`
- **Hamcrest** til assertions (`assertThat`, `is`, `hasSize`, `notNullValue`)
- **Testcontainers**, som starter en rigtig PostgreSQL i Docker til testene (`jdbc:tc:postgresql:16.2`)
- `HibernateTestConfig` med en test-`EntityManagerFactory` (`create-drop`), så testene ikke rører min rigtige database
- En **test-populator**, der fylder databasen med kendte film og genrer før hver test
- **Rest Assured** (`given()`, `when()`, `then()`) til at teste mine endpoints over HTTP
- `Main.startServer(7777, emf)`, så testene starter den samme app som i produktion, bare med test-database og en anden port
- Testene kører også i **GitHub Actions** ved hver push
- **SLF4J + Logback** til request logging, som jeg tilføjede samtidig

## Hvorfor strukturerede jeg det sådan?

Til test giver jeg Testcontainerne en frisk, tom database hver gang for at sikre mig at det der sker i testene
ikke kommer til at have indflydelse på min rigtige database, det er også derfor jeg har lavet startServer(port, emf) i min main.
Grunden til at jeg har DAO test er fordi jeg gerne ville teste databasekoden for sig, og så fik jeg så lavet Rest assured test
for at se om hele lagene fungere som den skal altså route-security-controller-service-dao

## Hvad virkede godt?

Jeg har fx lavet en exam-story test som gennemgår hele min feature automatisk, og at Github Actions sikre sig 
at mine tests fungere som de skal.

## Hvad var svært eller forvirrende?

Jeg fik faktisk en fejl i min Rest Assured til at starte med og det var en netværksfejl, hvor den ville sige til mig (Værten kendes ikke)
og det havde noget med at gøre at jeg var igang med at hente en maven dependency, men imens mistede jeg netværket, hvilket ødelagde den,
hverken reload eller sync maven dependencies virkede og jeg var nød til at skrive en kommando i maven terminal som var mvn -U dependency:resolve før den fungerede

Havde også et problem fordi alle mine test deler den samme Database så jeg skulle komme med en eller anden måde at ryde op efter hver test og her kom jeg så frem til noget der hedder TRUNCATE.


## Hvad ville jeg gøre bedre næste gang?

Næste gang kunne jeg nok havde lavet noget unit tests, her kunne jeg fx teste min verifyPassword eller min rating validering

jeg skrev faktisk også alle mine tests til sidst, det bedste ville nok havde været at fået gjort det løbende.

Jeg tester faktisk også kun min MovieDAO, og ikke min RatingDAO og SecurityDAO, det kunne jeg muligvis også godt havde lavet nogle test til.