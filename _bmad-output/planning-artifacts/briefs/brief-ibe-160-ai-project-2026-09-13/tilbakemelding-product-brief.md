# Tilbakemelding på product brief

| | |
|---|---|
| **Gruppe** | G03 – G03-gjertsen |
| **Product brief** | `_bmad-output/planning-artifacts/briefs/brief-ibe-160-ai-project-2026-09-13/brief.md` (commit `2605bf7`) |
| **Tilbakemelding fra** | Faglærer i IBE160 (utarbeidet med KI-støtte) |
| **Dato** | 2026-10-06 |

Vurdert fil: `brief.md` i mappen `brief-ibe-160-ai-project-2026-09-13`, som er den eneste briefen i repoet. Det finnes ennå ikke PRD, arkitektur eller epics.

## Samlet vurdering

- **Godt utgangspunkt med justeringer.** Gruppen kan gå videre og innarbeide punktene under.

**Det som er bra:**

1. Problemet er ekte og godt forklart: faktureringsgraden blir sjekket for sent, og et årlig mål på 80 % gjør at små etterslep hoper seg opp. At ideen kommer fra egen konsulenterfaring, gir dere domenekunnskap som gjør det mulig å kontrollere at appen regner riktig.
2. Avgrensningen er moden. Dere bygger mot simulerte data formet som Tripletex sin modell og holder den ekte API-koblingen, fakturering og flere selskaper utenfor v1. Det gjør appen kjørbar for sensor uten nøkler eller kontoer.

**De viktigste endringene:**

1. Avklar hva «AI-agenten» faktisk er. Briefen kaller løsningen en «autonomous AI agent», men alt som er beskrevet (beregne tempo, varsle om underskudd, foreslå hvor det er rom for mer fakturerbar tid) kan gjøres med vanlige regler og beregninger. Bestem om en språkmodell skal brukes, og i så fall til hva, for eksempel til å formulere hint i naturlig språk. Det påvirker både vanskelighetsgrad, kostnad og kjørbarhet.
2. Skriv ned regnestykket. Suksesskriteriet «correctly computes a rolling/yearly utilization pace (not a naive weekly average)» er bra, men det må stå hvordan tempoet beregnes: hva er tilgjengelige timer (7,5 t × virkedager, ferie, helligdager?), hva teller som fakturerbart, og når regnes en måned som «under mål». Uten dette kan dere ikke skrive tester eller vurdere om Claude Codes kode er riktig.
3. Avgrens leder-rollen. Leder-dashbordet betyr innlogging og to roller med ulik tilgang. Det er greit, men bestem om dere trenger ekte innlogging i v1, eller om et enkelt brukervalg mot testdata holder.

## Vanskelighetsgrad og gjennomførbarhet

### Vurdert vanskelighetsgrad

- **Middels**

**Sammenlignbart med:** 2) AI CV- og søknadsassistent (middels) i omfang, med et lite innslag av prognoselogikk som i 4.1) Prognoser og Demand Management (vanskelig). Siden prognosen her er én enkel framskriving per konsulent, havner prosjektet på middels.

**Begrunnelse:**

| Faktor | Nivå (lav / middels / høy) | Kommentar |
|---|---|---|
| Domenelogikk – hvor mange og hvor kompliserte regler og beregninger må stemme? | Middels | Faktureringsgrad, rullerende årstempo, varsel før en måned faller under mål og «hint» om ledig kapasitet. Reglene er overkommelige, men må defineres presist. |
| Datamodell – antall entiteter og relasjoner mellom dem | Middels | Konsulent, leder/team, prosjekt/kunde, timeføring, planlagt arbeid og mål per konsulent. Oversiktlig, men flere relasjoner. |
| Brukere, roller og innlogging | Middels | To roller (konsulent og leder), der lederen ser hele teamet. Krever tilgangsstyring hvis det bygges med ekte innlogging. |
| KI-funksjonalitet i appen, f.eks. kall til språkmodell, prompts i koden og håndtering av usikre svar | Lav til middels | Uklart i briefen. Hvis «agenten» er regelbasert, er KI-delen lav. Hvis en språkmodell skal lage hintene, blir det middels og krever plan for nøkler og testmodus. |
| Integrasjoner og eksterne tjenester, f.eks. API-er, betaling og e-post | Lav | Tripletex er bevisst utsatt til fase 2. E-post/Slack-varsler er også utenfor. |
| Sanntid, samtidighet eller flere brukere som påvirker hverandre | Lav | «Tracks continuously» kan tolkes som bakgrunnsjobb. I v1 holder det at tallene beregnes når siden åpnes. |
| Filhåndtering, f.eks. opplasting, PDF-lesing og eksport | Lav | Ingen filopplasting er beskrevet. Simulerte data kan lastes fra en seed-fil. |
| Sikkerhet og personvern | Middels | Timeføring og faktureringsgrad per ansatt er personopplysninger om arbeidsprestasjon. Bruk bare fiktive data i repoet. |

**Hva vanskelighetsgraden betyr for dere:**

- _Middels:_ Et godt balansert valg. Pass på at kjerneflyten blir ferdig og stabil før dere legger til mer. Her er kjerneflyten: konsulenten ser tempoet sitt, oppgir planlagt arbeid med valgknapper, og får et varsel og et konkret hint. Leder-dashbordet bør komme etter at dette virker.

### Gjennomførbarhet med BMAD og Claude Code

| Spørsmål | Vurdering (OK / risiko / stor risiko) | Kommentar |
|---|---|---|
| **Tid og omfang** – kan v1 realistisk bli ferdig og stabil i løpet av semesteret, med tid til flere iterasjoner? | OK | Fem funksjoner i scope er overkommelig når dataene er simulerte. Risikoen ligger i «agent»-delen hvis den vokser. |
| **BMAD-flyten** – er briefen konkret nok til at PRD, arkitektur og stories kan lages uten store hull, og blir det overkommelig mange stories? | Risiko | Brukere og scope er tydelige, men beregningsreglene og hva KI-en gjør mangler. Det må på plass før PRD, ellers fyller KI-en hullene for dere. |
| **Egnet for Claude Code** – bruker løsningen en vanlig, godt dokumentert teknologistakk som Claude Code håndterer godt, eller krever den nisjeteknologi, spesialmaskinvare eller mye manuell konfigurasjon? | OK | Dashbord, skjemaer og beregninger på en vanlig webstakk er noe Claude Code håndterer godt. |
| **Kontroll på KI-ens arbeid** – kan gruppen selv avgjøre om koden gjør det riktige? Krever domenet kunnskap gruppen ikke har, f.eks. avanserte beregninger eller fagregler, så er det vanskelig å kvalitetssikre. | OK | Dere kjenner domenet. Lag noen håndregnede eksempler (for eksempel en konsulent med 1 400 fakturerte av 1 650 tilgjengelige timer per oktober) som fasit. |
| **Testbarhet** – finnes det tydelige regler og forventede resultater som tester kan skrives mot? | Risiko | God mulighet for enhetstester av tempo og varsel, men bare når reglene er skrevet ned. Hint fra en språkmodell er vanskeligere å teste. |
| **Kjørbar for sensor** – kan appen kjøres lokalt etter README, uten gruppens nøkler, betalte kontoer eller egen infrastruktur? | OK | Simulerte data gjør dette enkelt, så lenge en eventuell språkmodell har en fallback. |
| **Avhengigheter og kostnader** – krever løsningen betalte API-er, f.eks. språkmodeller, og finnes det en plan for kostnad, testmodus eller mock-data? | Risiko | Ikke beskrevet. Hvis dere bruker en språkmodell, trenger dere en plan for nøkkel, kostnad og regelbasert reserveløsning. |

**Konklusjon om gjennomførbarhet:**

- **Gjennomførbart med justert omfang.** Se forslagene under.

**Forslag til justering av omfang eller vanskelighetsgrad:**

1. La beregning, varsel og hint være regelbasert i v1, og legg eventuelt en språkmodell på toppen som formulerer hintet. Da har dere en kjerne som kan testes, og en KI-del som kan slås av.
2. Bygg konsulentens flyt ferdig først (dashbord, planlegging med valgknapper, varsel og hint), og ta leder-dashbordet som neste trinn. Bruk gjerne en enkel innlogging med to faste testbrukere fremfor full brukerhåndtering.

## Hvorfor product brief er viktig for mappen

Product brief er utgangspunktet for PRD, arkitektur, stories og til slutt koden. Del 1 av mappen vurderes blant annet på om sensor kan følge en sporbar vei fra plan til ferdig app. Den vurderes også på om appen gjør det dere har beskrevet, om den er testet, om den er godt designet, og om den kan kjøres etter README. Et uklart, for stort eller for lite brief gjør alt dette vanskeligere senere. Det er mye enklere å rette nå enn sent i semesteret.

## 1. Gjennomgang av briefens deler

| Del av brief | Status | Kommentar |
|---|---|---|
| Executive Summary – er det klart hva appen er, og hvilket problem den løser? | OK | Klart: én assistent per konsulent som følger faktureringsgraden mot årsmålet og varsler før det er for sent. |
| The Problem – er problemet konkret, med reelle situasjoner og brukere? | OK | Godt beskrevet, med konkrete mekanismer (månedsslutt, årsgjennomsnitt, planlegging uten kobling til målet). |
| The Solution – beskriver løsningen brukeropplevelsen, ikke bare teknologi? | Juster | De fem punktene beskriver opplevelsen godt. Det er likevel uklart hva som er «agent» og KI, og hva som er vanlig logikk. Beskriv også et konkret eksempel på et hint. |
| What Makes This Different – er vurderingen ærlig og realistisk? | OK | Ærlig og nøktern: et framoverskuende lag oppå et system som bare registrerer historikk. |
| Who This Serves – er primærbrukerne tydelige, og vet vi hva de trenger? | OK | Konsulent som primærbruker og leder som sekundærbruker er tydelig. |
| Success Criteria – kan kriteriene faktisk sjekkes eller testes? | Juster | Flere er gode, men «at a glance» og «correctly computes» må gjøres målbare. Angi regelen for tempo og varsel, og et eksempel med forventet resultat. |
| Scope – er det klart hva som er med i første versjon, og hva som ikke er det? | OK | Tydelig inn/ut-liste. Avklar varslingskanalen (in-app) som en beslutning, ikke en antakelse. |
| Vision – henger visjonen sammen med resten uten å blåse opp omfanget? | OK | Tripletex-kobling og bredere kapasitetsplanlegging er tydelig lagt etter v1. |

## 2. Utgangspunkt for del 1 av mappen

Punktene følger kriteriene i sensorveiledningen for del 1. Vektene i parentes viser hvor mye hvert kriterium teller i del 1.

| Kriterium i del 1 | Hva briefen bør legge til rette for | Status | Kommentar |
|---|---|---|---|
| **1. Prosess og KI-styring** (30 %) | Brief som er presis nok til at PRD og stories kan bygges direkte på den, slik at krav kan spores fra brief til kode. | Juster | Godt grunnlag, men skriv inn beregningsreglene og KI-rollen før PRD. Repoet har to commits. Commit jevnlig videre, og lagre promptene. |
| **2. Funksjonalitet og omfang** (20 %) | Realistisk omfang for gruppen og semesteret: en tydelig kjerneflyt som kan bli ferdig og stabil, og nok innhold til å vise reell funksjonalitet. | OK | Kjerneflyten er tydelig, og leder-dashbordet gir naturlig utvidelse. |
| **3. Kvalitetssikring og testing** (15 %) | Suksesskriterier og funksjoner som er konkrete nok til å bli testtilfeller. | Juster | Tempo- og varselberegningen egner seg svært godt for tester når reglene og fasiteksemplene er skrevet ned. |
| **4. Design og brukeropplevelse** (10 %) | Tydelige brukere og brukssituasjoner som designet kan bygges rundt, gjerne med de viktigste skjermbildene eller flytene skissert. | OK | Konsulentdashbord, planlegging med valgknapper og leder-dashbord er lette å se for seg. Skisser gjerne hvordan «på/under mål» vises. |
| **5. Kodekvalitet og arkitektur** (10 %) | Teknologivalg som er begrunnet og ikke mer komplekse enn appen trenger. | OK | Ideen om et datalag som kan byttes fra simulerte data til Tripletex senere, er god arkitektur. Unngå å bygge en tung «agent»-ramme hvis regler holder. |
| **6. README og kjørbarhet** (10 %) | Løsning som andre kan kjøre lokalt uten betalte kontoer, og uten tilgang til gruppens egne tjenester og nøkler. | OK | Simulerte data gjør det mulig. Sørg for at testdata og eventuelle testbrukere følger med. |
| **7. Ryddighet i repoet** (5 %) | En plan for hvor hemmeligheter, testdata og dokumentasjon skal ligge. | Juster | Ikke beskrevet. Bestem hvor simulerte data og en eventuell API-nøkkel (`.env`, ikke i Git) skal ligge. |

## 3. Neste steg for gruppen

1. Skriv inn i briefen hvordan faktureringsgrad og tempo beregnes, når et varsel utløses, og to–tre eksempler med forventet resultat.
2. Bestem hva «AI-agenten» er i v1: regelbasert logikk, språkmodell eller en kombinasjon. Hvis dere bruker språkmodell, beskriv testmodus og reserveløsning.
3. Lag PRD med konsulentens kjerneflyt først og leder-dashbordet som eget, senere epic.

Oppdater product brief i repoet når dere har gjort endringene, slik at historikken viser hvordan planen utviklet seg. Det er en del av prosessen sensor ser etter.
