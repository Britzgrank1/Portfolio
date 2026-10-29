---
title: "Datamodellen: entiteter og JPA-relationer"
date: 2026-10-07
draft: false
description: ""
---

## Hvad tilføjede jeg?
Her startede jeg med at tilføje entiteter som Movie, Genre, Person og Credit, hvilket
skulle være kernen i mit project, senere hen fik jeg tilføjet entiter, som User, UserMovieRating,
Watchlist og Role.



## Hvilken ny teknologi brugte jeg?

- **JPA / Hibernate 7** som ORM, så Java-klasser bliver til tabeller i databasen
- **PostgreSQL** som database
- **Lombok** til getters, setters og constructors
- Annotationerne `@Entity`, `@Table`, `@Id`, `@GeneratedValue` og `@Column`
- Relationerne `@ManyToMany`, `@OneToMany` og `@ManyToOne` med `@JoinTable` og `@JoinColumn`
- `hibernate.hbm2ddl.auto = update`, så Hibernate selv opretter og opdaterer tabellerne
- `HibernateConfig` med en `EntityManagerFactory` som singleton

## Hvorfor strukturerede jeg det sådan?

Her gik jeg med at lave entitet som Credit, fordi jeg har brug for et bindeled mellem Movie og Person, hvis jeg fx tager et eksempel
som Peter Parker, her kan jeg jo ikke gemme ham som person fx Tom Holland da han har roller i andre film, hvilket gøre at han ikke helt passer ind i characterName.
Grunden til at jeg hellere ikke vil kunne bruge Movie er fordi der er mange roller i filmen som spiller hver sin rolle
derfor er Peter Parker en kombination af både Tom Holland og Spider-man og er derfor jeg har lavet kredit


## Hvad virkede godt?

Her har Hibernate virket virkelig godt da det har hjulpet mig med at lave tabeller ud fra de annotationer jeg har i min kode
og nu kan det godt være at, hvis jeg lavede en helt ny ER diagram, så ville det se helt anderledes ud, men det har virkelig 
været en god måde for mig at se, hvad jeg endelig har brug for, at kunne putte et billede på det har hjulpet en del.





## Hvad var svært eller forvirrende?

Der var tidspunkter hvor jeg glemte at putte @ på mine annotationer, hvilket gav mig fejl på min kode.
Når jeg begyndte at tilføje flere og flere entiteter, så begyndt det også at blive svære at holde overblikket da
jeg havde brug for flere entiter end hvad jeg først regnede med til at kunne gøre de ting jeg ville.

## Hvad ville jeg gøre bedre næste gang?

Jeg lavede en Watchlist, som også er med i mit ER-diagram, men fik det aldrig brugt, jeg havde nemlig alle mulige ideer
med hvad man skulle som, at man skal kunne lave en kommentar på en film, rate dem og få anbefalinger, samt at man skulle kunne gemme
dem man har set og se dem, hvilket skulle være watchlist og at man skulle kunne se på en skuespiller og se hvilke film de optræder i og
at man skulle kunne se hvilke skuespiller der er i en specifik film. Her har jeg desværre ikke noget det jeg satte mig ud for, og her kan jeg helt 
klart havde brugt mere tid på projektet og fordelt min tid bedre.

UserMovieRating valgte jeg at lave til at have bindled mellem en User og Movie
her mener jeg heler ikke at en rating tilhøre en user, da en user kan rate flere film, og det burde
heller ikke tilhører Movie fordi en film kan have flere ratings fra adskillige personer,
Hvilket er derfor jeg har lavet entiteten UserMovieRating, da jeg vil kunne kombinere både User og Movie
