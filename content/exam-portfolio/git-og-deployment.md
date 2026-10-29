---
title: "deployment"
date: 2026-10-23
draft: false
description: ""
---

## Hvad tilføjede jeg?

Her fik jeg tilføjet workflow til deployment, samt også Dockerfile og et healthcheck til at se
om min exampAPI faktisk kører og svarer. Jeg brugte Caddy til at håndterer api requests og jeg brugte Watchtower til at sikre, når
jeg fik pushed noget nyt fra mit github, at den ville lave en ny container med det nye image og fjerne det gamle.
Jeg fik også givet github min Docker brugernavn og token som jeg har gemt i Github Secrets, hvilket går at min Github og Docker kunne sammenarbejde.

## Hvilken ny teknologi brugte jeg?

- **Docker** og en `Dockerfile` (`amazoncorretto:17-alpine`), der pakker min `app.jar` ind i et image
- **Maven Shade-plugin**, så alle biblioteker kommer med i én `app.jar`
- **GitHub Actions** (`workflow.yml`), der bygger med Maven og kører testene ved hver push
- **Docker Hub** til at gemme imaget (`exam_backend:latest`)
- **GitHub Secrets** til mit Docker Hub-login, så det ikke står i koden
- En **DigitalOcean droplet** med `docker-compose`, hvor min API, PostgreSQL, Caddy og Watchtower kører
- **Caddy** som reverse proxy med automatisk HTTPS på `exam.datudvikling.dk`
- **Watchtower**, der opdager et nyt image og genstarter min container automatisk
- Et **healthcheck**-endpoint (`/api/healthcheck`), som Docker bruger til at se om appen kører
- **Miljøvariabler** på serveren til database og `SECRET_KEY` i stedet for `config.properties`

## Hvorfor strukturerede jeg det sådan?

Altså fik puttede mine miljøvariabler på serveren og ikke i imaget fordi mit Docker Hun er offentlig, hvilket så gør at
hvis mine miljøvariabler var der så ville alle have adgang til dem og grunden til at jeg har watchtower er fordi så jeg kunne 
automatisere processen, hvor nu er der ikke behov for at jeg selv skal logge ind på serveren og genstarte den efter hver push.
Jeg har så også prøvede at følge pipeline, hvor github actions tester min tests når det er pushed, men inden at docker image er bygget og sendt til mit Docker hub, på grund af hvis nogle tests fejler,
så vil det ikke blive pushed til et nyt image, hvilket så sikre mig at de metoder jeg tester faktisk fungere når de er deployet


## Hvad virkede godt?

Altså noget der virker virkeligt godt, er at nu når jeg pusher, så kommer den nye version online efter nogle få minutter


## Hvad var svært eller forvirrende?

Jeg havde et lille Git uheld, hvor jeg stod påd en gamle lokale main branch, og den havde faktisk kun en README, så når jeg 
prøvede at push så dukkede der ikke noget op på min github, men det fik jeg fixed når jeg fik skiftet til min master branch og pushede med
git push github master:main.

Jeg kom også tidligere i starten af projektet at have min config.properties fremvist i min github, hvilket ikke var særlig godt når den har oplysninger
som helt ikke skal være offentligt.

Havde også endnu et problem med når jeg prøvede at få Watchtower til at fungere, så var min client version åbenbart 1.25, hvilket var forældet,
men det kunne jeg så fixe ved at sætte DOCKER_API_VERSION til 1.44 istedet.

Fik også på et tidspunkt **502 Bad Gateway**, har ikke fået det ligesiden, men tror det er pågrund af at jeg har flere Java apps kørende på mit droplet,
hvilket har taget det meste af den 1GB jeg har til rådighed.


## Hvad ville jeg gøre bedre næste gang?

Sikre mig at der er mere ram til rådighed på min droplet, fx ved at fjerne gamle projekter, også nok til næste gang at sikre mig at
min .gitignore er klar som det første inden første commit, så jeg ikke kommer ud i at folk kan kigge på mine gamle commits og finde mine secret variables.
og nu siden det ikke er ud i et rigtigt firma, så har jeg valgt at genbruge de værdier der var offentlige fx database password osv, men rigtigt burde jeg nok havde ændet det.



