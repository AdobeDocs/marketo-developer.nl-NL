---
title: Bulk extraheren
feature: REST API
description: Leer hoe u de Marketo Bulk Extraheren REST API kunt gebruiken voor het exporteren van leads, activiteiten, programmaleden en aangepaste objecten, met OAuth, taakwachtrijen en dagelijkse limieten van 500 MB.
exl-id: 6a15c8a9-fd85-4c7d-9f65-8b2e2cba22ff
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1702'
ht-degree: 0%

---

# Bulk extraheren

Marketo verstrekt interfaces voor terugwinning van grote reeksen persoon en op persoon betrekking hebbende gegevens, genoemd Bulk Extract. Interfaces worden momenteel aangeboden voor drie objecttypen:

- Leads (personen)
- Activiteiten
- Programmaleden
- Aangepaste objecten

Uitpakken van opsommingstekens wordt uitgevoerd door een taak te maken, de gegevensset te definiëren die moet worden opgehaald, de taak te vragen, te wachten totdat de taak is voltooid en een bestand te schrijven, en het bestand vervolgens via HTTP op te halen. Deze taken worden asynchroon uitgevoerd en kunnen worden gepolled om de status van de export op te halen.

`Note:` Eindpunten van de bulk-API worden niet voorafgegaan door &#39;/rest&#39;, zoals andere eindpunten.

## Verificatie

De bulk extract APIs gebruikt de zelfde OAuth 2.0 authentificatiemethode zoals andere Marketo REST APIs. Hiervoor moet een geldig toegangstoken worden verzonden als HTTP-header `Authorization: Bearer {_AccessToken_}` .

>[!IMPORTANT]
>
>De steun voor authentificatie die **gebruikt access_token** vraagparameter wordt verwijderd op 30 Juni, 2025. Als uw project een vraagparameter gebruikt om het toegangstoken over te gaan, zou het moeten worden bijgewerkt om de **1&rbrace; kopbal van de Vergunning &lbrace;zo spoedig mogelijk te gebruiken.** De nieuwe ontwikkeling zou de **kopbal van de Vergunning** exclusief moeten gebruiken.

## Limieten

- Max. gelijktijdige exporttaken: 2
- Max. aantal exporttaken (inclusief momenteel uitgevoerde taken): 10
- Bewaarperiode bestand: zeven dagen
- Standaard dagelijkse exporttoewijzing: 500MB (die dagelijks opnieuw wordt ingesteld op 12 :00AM CST). Hogere aankopen zijn mogelijk.
- Maximale tijdbereik voor filter Datumbereik (createdAt of updatedAt): 31 dagen

De filters van het Extraheren van de Lood van het Bulk voor UpdatedAt en Slimme Lijst zijn niet beschikbaar voor sommige abonnementstypes. Indien niet beschikbaar, keert een vraag aan het Create eindpunt van de Baan van de Uitvoer een fout &quot;1035, niet gestaafd filtertype voor doelabonnement&quot;terug. Klanten kunnen contact opnemen met Marketo Support om deze functionaliteit in hun abonnement te laten inschakelen.

### Wachtrij

De API&#39;s voor bulksgewijs uitpakken gebruiken een taakwachtrij (die wordt gedeeld door leads, activiteiten, programmaleden en aangepaste objecten). Extraheren-taken moeten eerst worden gemaakt en vervolgens worden opgevraagd. U kunt de functie Exportlead/activiteit/Programmalid maken aanroepen en de eindpunten Exportleiding/activiteit/Programmalid in de wachtrij plaatsen. Nadat de vraag is gesteld, worden de taken uit de wachtrij gezet en gestart wanneer computerbronnen beschikbaar komen.

Het maximumaantal taken in de wachtrij is 10. Als u een baan probeert te vragen wanneer de rij volledig is, keert het eindpunt van de Baan van de Uitvoer van de Enqueue een fout &quot;1029, Te veel banen in rij&quot;terug. Er kunnen maximaal twee taken tegelijk worden uitgevoerd (status is &quot;Verwerking&quot;).

### Bestandsgrootte

De bulkextractie-API&#39;s worden gemeten op basis van de grootte op schijf van de gegevens die door een bulkextractietaak worden opgehaald. De expliciete grootte in bytes voor een taak kan worden bepaald door het kenmerk `fileSize` te lezen op basis van de voltooide statusreactie van een exporttaak.

Het dagelijkse quotum is maximaal 500 MB per dag, dat wordt gedeeld tussen leads, activiteiten, programmaleden en aangepaste objecten. Wanneer het quotum wordt overschreden, kunt u niet een andere baan tot stand brengen of in rij brengen tot de dagelijkse quota bij middernacht [&#x200B; Centrale Tijd &#x200B;](https://en.wikipedia.org/wiki/Central_Time_Zone) terugstelt. Tot die tijd wordt een fout &quot;1029, het dagelijkse quotum van de Uitvoer overschreden&quot; geretourneerd. Naast de dagelijkse quota is er geen maximale bestandsgrootte.

Als een taak in de wachtrij is geplaatst of wordt verwerkt, wordt deze uitgevoerd tot voltooiing (zonder een fout of annulering van een taak). Als een taak om een of andere reden mislukt, moet u deze opnieuw maken. Bestanden worden alleen volledig geschreven wanneer een taak de voltooide status bereikt (gedeeltelijke bestanden worden nooit weggeschreven). U kunt verifiëren dat een dossier volledig werd geschreven door het te berekenen hash SHA-256 en het vergelijken van dat met checksum die door de eindpunten van de baanstatus wordt teruggekeerd.

U kunt de totale hoeveelheid schijf bepalen die voor de huidige dag wordt gebruikt door Get de Leiding van de Uitvoer/Activiteit/de Banen van het Lid van het Programma te roepen. Deze eindpunten geven een lijst weer van alle banen in de afgelopen zeven dagen. U kunt die lijst filteren tot alleen de taken die op de huidige dag zijn voltooid (met de kenmerken `status` en `finishedAt` ). Vervolgens telt u de bestandsgrootten voor deze taken samen om het totale gebruikte bedrag te produceren. U kunt een bestand op geen enkele manier verwijderen om schijfruimte vrij te maken.

## Machtigingen

Bulk Extract gebruikt het zelfde toestemmingenmodel zoals Marketo REST API, en vereist geen extra speciale te gebruiken toestemmingen, hoewel de specifieke toestemmingen voor elke reeks eindpunten worden vereist.

Opsommingtaken zijn alleen toegankelijk voor de API-gebruiker die ze heeft gemaakt, waaronder opiniepeilingen naar status en het ophalen van bestandsinhoud.

De bulkuittrekseleindpunten zijn zich niet van de werkruimten van Marketo bewust. Extractieaanvragen bevatten altijd gegevens in alle werkruimten, ongeacht hoe u de gebruiker van de aangepaste service Alleen de API definieert.

## Een taak maken

Marketo-API&#39;s voor bulkextractie gebruiken het concept van een taak voor het starten en uitvoeren van gegevensextractie. Laten we eens kijken naar het maken van een eenvoudige uitvoertaak voor leads.

```
POST /bulk/v1/leads/export/create.json
```

```json
{
   "fields": [
      "firstName",
      "lastName"
   ],
   "format": "CSV",
   "columnHeaderNames": {
      "firstName": "First Name",
      "lastName": "Last Name"
   },
   "filter": {
      "createdAt": {
         "startAt": "2023-01-01T00:00:00Z",
         "endAt": "2023-01-31T00:00:00Z"
      }
   }
}
```

In dit eenvoudige verzoek wordt een taak samengesteld die de waarden in de velden &quot;firstName&quot; en &quot;lastName&quot; retourneert, met de kolomkoppen &quot;Voornaam&quot; en &quot;Achternaam&quot; als CSV-bestand, die elke lead bevatten die tussen 1 januari 2023 en 31 januari 2023 is gemaakt.

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Created",
         "createdAt": "2023-01-21T11:47:30-08:00",
         "queuedAt": "2023-01-21T11:48:30-08:00",
         "format": "CSV",
      }
   ]
}
```

Wanneer we de taak maken, wordt een taak-id in het kenmerk `exportId` geretourneerd. Vervolgens kunnen we deze taak-id gebruiken om de taak in de wachtrij te plaatsen, de taak te annuleren, de status te controleren of het voltooide bestand op te halen.

### Algemene parameters

Elk eindpunt van de baanverwezenlijking deelt sommige gemeenschappelijke parameters voor het vormen van het dossierformaat, gebiedsnamen, en filter van een bulkextractietaak. Elk subtype van extractietaak kan extra parameters hebben:

| Parameter | Gegevenstype | Notities |
|---|---|---|
| format | String | Hiermee bepaalt u de bestandsindeling van de geëxtraheerde gegevens met opties voor door komma&#39;s gescheiden waarden, door tabs gescheiden waarden en door puntkomma&#39;s gescheiden waarden. Accepteert één van: CSV, SSV, TSV. De notatie wordt standaard ingesteld op CSV. |
| columnHeaderNames | Object | Hiermee kunt u de namen van kolomkoppen in het geretourneerde bestand instellen. Elke lidsleutel is de naam van de kolomkop waarvan de naam moet worden gewijzigd en de waarde is de nieuwe naam van de kolomkop. Bijvoorbeeld &quot;columnHeaderNames&quot;: { &quot;firstName&quot;: &quot;First Name&quot;, &quot;lastName&quot;: &quot;Last Name&quot; }, |
| filter | Object | Filter toegepast op de extractietaak. De typen en opties variëren per taaktype. |

## Taken ophalen

Soms moet u uw recente taken opvragen. Dit wordt gemakkelijk gedaan met Get de Banen van de Uitvoer voor het overeenkomstige objecten type. Elk eindpunt voor Exporttaken ophalen ondersteunt een filterveld van het type `status` ,  `batchSize` om het aantal geretourneerde taken te beperken en `nextPageToken` om door grote resultaatsets te bladeren. Het statusfilter ondersteunt elke geldige status voor een exporttaak: Gemaakt, In wachtrij geplaatst, Verwerking, Geannuleerd, Voltooid en Mislukt. De batchSize heeft een maximum en gebrek van 300. Laten we de lijst met lead-exporttaken ophalen:

```
GET /bulk/v1/leads/export.json?status=Completed,Failed
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Completed",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "startedAt": "2017-01-21T11:51:30-08:00",
         "finishedAt": "2017-01-21T12:59:30-08:00",
         "format": "CSV",
         "numberOfRecords": 122323,
         "fileSize": 123424,
         "fileChecksum": "sha256:c16514c7e80fcac5ea055dacae9617fc3c29aff5365e3743071313ce0ed2a815"
      }
      ...
   ]
}
```

Het eindpunt reageert met `status` reactie van elke baan die in de afgelopen zeven dagen voor dat objecten type in de resultaatserie werd gecreeerd. De reactie zal slechts resultaten voor banen omvatten die door de API gebruiker worden bezeten die de vraag maakt.

## Een taak starten

Met onze baan-id in hand, laten we de baan beginnen:

```
POST /bulk/v1/leads/export/{exportId}/enqueue.json
```

Hierdoor wordt de uitvoering van de taak uitgeschakeld en wordt een statusreactie teruggestuurd. Aangezien het exporteren altijd asynchroon wordt uitgevoerd, moeten we de status van de taak opvragen om te bepalen of deze is voltooid. De status voor een bepaalde baan zal niet vaker dan om de 60 seconden worden bijgewerkt, zodat de status nooit vaker dan dat zou moeten worden opgevraagd. Houd er echter rekening mee dat in de meeste gevallen geen vaker dan eens per 5 minuten opiniepeiling vereist is. Gegevens over elke succesvolle export worden tien dagen bewaard.

## Status opiniepeilingtaak

Het is eenvoudig de status van de taak te bepalen.

De status kan alleen worden opgevraagd voor taken die zijn gemaakt door dezelfde API-gebruiker die deze heeft gemaakt.

```
GET /bulk/v1/leads/export/{exportId}/status.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Completed",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "startedAt": "2017-01-21T11:51:30-08:00",
         "finishedAt": "2017-01-21T12:59:30-08:00",
         "format": "CSV",
         "numberOfRecords": 122323,
         "fileSize": 123424,
         "fileChecksum": "sha256:d9c73f0b6960c71623c8bafe29603b3e8e20fd0e4eeaefd119c0227506ea9be4"
      }
   ]
}
```

Het binnenste `status` lid geeft de voortgang van de taak aan en kan een van de volgende waarden zijn: Gemaakt, In wachtrij geplaatst, Verwerken, Geannuleerd, Voltooid, Mislukt. In dit geval is onze taak voltooid, dus kunnen we stoppen met opiniepeilingen en doorgaan met het ophalen van het bestand. Na voltooiing geeft het `fileSize` -lid de totale lengte van het bestand in bytes aan en het `fileChecksum` -lid bevat de SHA-256-hash van het bestand. Taakstatus is beschikbaar gedurende 30 dagen nadat de status Voltooid of Mislukt is bereikt.

## Uw gegevens ophalen

Wanneer uw taak is voltooid, kunt u het bestand gemakkelijk ophalen.

```
GET /bulk/v1/leads/export/{exportId}/file.json
```

De reactie bevat een bestand dat is opgemaakt op de manier waarop de taak is geconfigureerd. Het eindpunt antwoordt met de inhoud van het dossier. Als een baan niet heeft voltooid, of een slechte baan ID wordt overgegaan, antwoorden de dossiereindpunten met een status van 404 niet Gevonden, en een plaintext foutenmelding als lading, in tegenstelling tot de meeste andere eindpunten van Marketo REST.

Om gedeeltelijke en hervatting-vriendschappelijke terugwinning van gehaalde gegevens te steunen, steunt het dossiereindpunt naar keuze de kopbal van HTTP `Range` van het type `bytes` (per [&#x200B; RFC 7233 &#x200B;](https://datatracker.ietf.org/doc/html/rfc7233)). Als de header niet is ingesteld, wordt de gehele inhoud geretourneerd. Om de eerste 10.000 bytes van een dossier terug te winnen, zou u de volgende kopbal als deel van uw verzoek van GET tot het eindpunt overgaan, die van byte 0 begint:

```
Range: bytes=0-9999
```

Wanneer het terugwinnen van het gedeeltelijke dossier, antwoordt het eindpunt met statuscode 206, en het terugkeren van de Accept-waaiers, Content-Length, en Content-Range kopballen:

```
Accept-Ranges: bytes
Content-Length: 1000
Content-Range: bytes 0-9999/123424
```

### Gedeeltelijke inning en hervatting

Bestanden kunnen gedeeltelijk worden opgehaald of later worden hervat met de header `Range` . Het bereik voor een bestand begint bij byte 0 en eindigt bij de waarde `fileSize` minus 1. De lengte van het bestand wordt ook gerapporteerd als de noemer in de waarde van de antwoordheader van `Content-Range` wanneer een aanroep van het eindpunt Exportbestand ophalen wordt gedaan. Als een herwinning gedeeltelijk ontbreekt, kan het later worden hervat. Bijvoorbeeld, als u probeert om een dossier terug te winnen 1000 lange bytes, maar slechts de eerste 725 bytes werden ontvangen, kan de herwinning van het punt van mislukking opnieuw worden geprobeerd door het eindpunt opnieuw te roepen en een nieuwe waaier over te gaan:

```
Range: bytes 724-999
```

Hiermee worden de resterende 275 bytes van het bestand geretourneerd.

#### Verificatie bestandsintegriteit

De eindpunten van de taakstatus retourneren een controlesom in het kenmerk `fileChecksum` wanneer `status` &quot;Voltooid&quot; is. De controlesom is een SHA-256-hash van het geëxporteerde bestand. U kunt de controlesom vergelijken met SHA-256 knoeiboel van het teruggewonnen dossier om te verifiëren dat het volledig is.

Hier volgt een voorbeeld van een reactie met de controlesom:

```json
{
    "exportId": "45547609-6732-418a-bb7b-17b0160b2317",
    "format": "CSV",
    "status": "Completed",
    "createdAt": "2019-06-04T23:13:12Z",
    "queuedAt": "2019-06-04T23:14:02Z",
    "startedAt": "2019-06-04T23:15:19Z",
    "finishedAt": "2019-06-04T23:36:40Z",
    "numberOfRecords": 1776,
    "fileSize": 400785,
    "fileChecksum": "sha256:83aca1351c9398d2770330e21a9e278880fd2f1eeaf8c8238bf7676d5c21d1c6"
}
```

Hier is een voorbeeld van het creëren van de hash SHA-256 van een teruggewonnen dossier genoemd &quot;bulk_lead_export.csv&quot;gebruikend sha256sum bevel-lijn nut:

```
$ sha256sum bulk_lead_export.csv
83aca1351c9398d2770330e21a9e278880fd2f1eeaf8c8238bf7676d5c21d1c6 *bulk_lead_export.csv
```

## Een taak annuleren

Als een baan verkeerd werd gevormd, of onnodig wordt, kan het gemakkelijk worden geannuleerd:

```
POST /bulk/v1/leads/export/{exportId}/cancel.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Cancelled",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "format": "CSV",
      }
   ]
}
```

Dit reageert met een status die aangeeft dat de taak is geannuleerd.
