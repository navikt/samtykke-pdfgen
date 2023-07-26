# samtykke-pdfgen
REST API bygget på [navikt/pdfgen](https://github.com/navikt/pdfgen) for PDF generering av samtykker på PDF-format PDF/A i NAV's digital samtykkeløsning for brukertester.

Generering skjer ved å sende en HTTP POST request til route `/api/v1/genpdf/samtykke/samtykke` med en body på formatet i [/data/samtykke/samtykke.json](/data/samtykke/samtykke.json). Et eksempel på hvordan dette er implementert kan man se i [navikt/samtykke-api](https://github.com/navikt/samtykke-api/blob/31fd2352804957ebaef4669508531ce8f2c57438/src/main/kotlin/no/nav/consent/pdf/generateConsentPDF.kt#L15).

## Kom i gang med utvikling

### Forutsetninger
- Docker kjørende
- Autentisert lokalt for [GitHub Container Registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry#authenticating-to-the-container-registry)

### Kjør lokalt
Bruk ferdilaget bash script
````
bash run_dev.sh
````

Eller kjør direkte i Docker
````bash
docker run \
    -v <path_to_templates>:/app/templates \
    -v <path_to_fonts>:/app/fonts \
    -v <path_to_data>:/app/data \
    -v <path_to_resources>:/app/resources \
    -p 8080:8080 \
    -e DISABLE_PDF_GET=false \
    -e JDK_JAVA_OPTIONS \
    -it \
    --rm \
    ghcr.io/navikt/pdfgen:2.0.11
````

Så skal PDF-generatoren være tilgjengelig på `http://0.0.0.0:8080/api/v1/genpdf/samtykke/samtykke`

### Genering av PDF med test data
Med milvjøvariablen `DISABLE_PDF_GET` satt til `false` kan man generere PDF'er med den eksisterende test data'en i [/data/samtykke/samtykke.json](/data/samtykke/samtykke.json). Man kan da sende en HTTP GET request til route `/api/v1/genpdf/samtykke/samtykke` med en tom body for å genere samtykket med test data'en.

## Testmiljø på NAIS
Testmiljøet til PDF-generatoren kjører i `dev-gcp` clusteret på NAIS. For å inspisere generatoren i `dev-gcp` må [naisdevice](https://doc.nais.io/device/?h=nais) være aktivert og du må være medlem av team-researchops. Kontakt i `#researchops` eller `#samtykke-løsning` på Slack for å få tilgang.

## For NAV-ansatte
Interne henvendelser kan sendes på Slack via kanalene `#researchops` eller `#samtykke-løsning`. 