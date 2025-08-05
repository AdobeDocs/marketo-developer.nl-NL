---
title: Uitpakken van bulkactiviteiten
feature: REST API
description: Gegevens van Marketo over de verwerking van batches.
exl-id: 6bdfa78e-bc5b-4eea-bcb0-e26e36cf6e19
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '1332'
ht-degree: 0%

---

# Uitpakken van bulkactiviteiten

[ Uittreksel van het EindpuntVerwijzing van de Activiteit van de Bulk ](https://developer.adobe.com/marketo-apis/api/mapi/)

De reeks van het Uittreksel van de Activiteit van het Bulk van REST APIs verstrekt een programmatic interface voor het terugwinnen van grote hoeveelheden activiteitengegevens uit Marketo.  Voor gevallen die geen lage latentie vereisen, en significante volumes van activiteitsgegevens uit Marketo, zoals CRM-integratie, ETL, gegevensopslag, en gegevensarchivering moeten overbrengen.

## Machtigingen

De Bulk Activity Extraheren-API&#39;s vereisen dat de API-gebruiker beschikt over de machtiging &quot;Alleen-lezen activiteit&quot; of &quot;activiteit lezen-schrijven&quot;.

## Filters

| Filtertype | Gegevenstype | Vereist | Notities |
| --- | --- | --- | --- |
| createdAt | Datumbereik | Ja | Accepteert een JSON-object met de leden `startAt` en `endAt` . `startAt` accepteert een datetime die het lage watermerk vertegenwoordigt en `endAt` accepteert een datetime die het hoge watermerk vertegenwoordigt. Het bereik moet 31 dagen of minder zijn. Taken met dit filtertype retourneren alle toegankelijke records die binnen het datumbereik zijn gemaakt. Datumtijden moeten een ISO-8601-indeling hebben, zonder milliseconden. |
| activityTypeIds | Array\[geheel getal\] | Nee | Accepteert een JSON-object met één lid, `activityTypeIds` . De waarde moet een array van gehele getallen zijn die overeenkomen met de gewenste activiteitstypen. De &quot;Lood van de Schrapping&quot;activiteit wordt niet gesteund (gebruik [ krijgen schrapte Leads ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET) eindpunt in plaats daarvan). Haal activiteitentype ids gebruikend terug [ krijgt het eindpunt van de Types van Activiteit ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET). |
| [ primaryAttributeValueIds ](#primaryattributevalueids-options) | Array\[geheel getal\] | Nee | Accepteert een JSON-object met één lid, `primaryAttributeValueIds` . De waarde is een array van id&#39;s die de primaire kenmerken opgeven waarop moet worden gefilterd. Er kunnen maximaal 50 id&#39;s worden opgegeven. De id&#39;s zijn de unieke id voor een lead-veld of een element en kunnen worden opgehaald door het juiste REST API-eindpunt aan te roepen. Bijvoorbeeld, om op een specifieke Vorm voor de &quot;Vul uit de activiteit van de Vorm&quot;te filtreren, ga de naam van de Vorm tot [ over krijgt Vorm door het 1} eindpunt van de Naam om Vorm identiteitskaart terug te winnen. ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) Hieronder volgt een lijst met activiteitstypen waarvoor het filteren van primaire kenmerken wordt ondersteund. |
| [ primaryAttributeValues ](#primaryattributevalues-options) | Array\[String\] | Nee | Accepteert een JSON-object met één lid, `primaryAttributeValues` . De waarde is een array van namen die de primaire kenmerken opgeven waarop moet worden gefilterd. Er kunnen maximaal 50 namen worden opgegeven. De namen zijn het unieke herkenningsteken voor of een lood gebied of activa, en kunnen worden teruggewonnen door het aangewezen REST API eindpunt te roepen. Bijvoorbeeld, om op een specifieke Vorm voor de &quot;Vul uit de activiteit van de Vorm&quot;te filtreren, ga Vorm ID tot [ over krijgen Vorm door Identiteitseindpunt ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) om de naam van de Vorm terug te winnen. Hieronder volgt een lijst met activiteitstypen waarvoor het filteren van primaire kenmerken wordt ondersteund. |

### Opties voor primaryAttributeValueIds {#primaryattributevalueids-options}

| Type activiteit | Primaire kenmerkwaarde-id | Retrieval Endpoint | Middelengroep |
| --- | --- | --- | --- |
| Gegevenswaarde wijzigen | Id van regelveld | [ beschrijf lood ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Kenmerknaam |
| Score wijzigen | Id van regelveld | [ beschrijf lood ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Kenmerknaam |
| Status wijzigen in Progressie | Programma-id | [ krijgt Programma door Naam ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByNameUsingGET) | Marketingprogramma |
| Toevoegen aan lijst | Statische lijst-id | [ krijgt Statische Lijst door Naam ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Statische lijst |
| Verwijderen uit lijst | Statische lijst-id | [ krijgt Statische Lijst door Naam ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET) | Statische lijst |
| Formulier invullen | Formulier-id | [ krijg Vorm door Naam ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET) | Webformulier |

Wanneer u `primaryAttributeValueIds` gebruikt, moet het filter `activityTypeIds` aanwezig zijn en mag het alleen activiteiten-id&#39;s bevatten die overeenkomen met de overeenkomstige elementgroep. Als u bijvoorbeeld filtert op webformulierelementen, is alleen de activiteitstype-id Formulier invullen toegestaan in `activityTypeIds` .

Hoofdtekst voorbeeldaanvraag:

```json
{
  "filter": {
    "createdAt": {
      "startAt": "2021-07-01T23:59:59-00:00",
      "endAt": "2021-07-02T23:59:59-00:00"
    },
    "activityTypeIds": [
      2
    ],
    "primaryAttributeValueIds": [
      16,102,95,8
    ]
  }
}
```

`primaryAttributeValueIds` en `primaryAttributeValues` kunnen niet samen worden gebruikt.

### Opties voor primaryAttributeValues {#primaryattributevalues-options}

| Type activiteit | Primaire kenmerkwaarde | Retrieval Endpoint | Middelengroep |
| --- | --- | --- | --- |
| Gegevenswaarde wijzigen | Weergavenaam hoofdveld | [ beschrijf lood ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Kenmerknaam |
| Score wijzigen | Weergavenaam hoofdveld | [ beschrijf lood ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2) | Kenmerknaam |
| Status wijzigen in Progressie | Programmanaam | [ krijgt Programma door Id ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByIdUsingGET) | Marketingprogramma |
| Toevoegen aan lijst | Statische lijstnaam | [ krijgt Statische Lijst door Id ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Statische lijst |
| Verwijderen uit lijst | Statische lijstnaam | [ krijgt Statische Lijst door Id ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET) | Statische lijst |
| Formulier invullen | Formuliernaam | [ krijg Vorm door identiteitskaart ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5) | Webformulier |

Merk op dat u &quot;&lt; <em> programma </em>> moet gebruiken.&lt;<em> activa </em>>&quot;aantekening om de naam voor de volgende activagroepen te specificeren: Het Programma van de marketing, Statische Lijst, de Vorm van het Web. Een formulier met de naam &quot;MPS Outbound&quot; dat zich bijvoorbeeld onder een programma met de naam &quot;GL_OP_ALL_2021&quot; bevindt, wordt opgegeven als &quot;GL_OP_ALL_2021.MPS Outbound&quot;.

Hoofdtekst voorbeeldaanvraag:

```json
{
  "filter": {
    "createdAt": {
      "startAt": "2021-07-01T23:59:59-00:00",
      "endAt": "2021-07-02T23:59:59-00:00"
    },
    "activityTypeIds": [
      2
    ],
    "primaryAttributeValues": [
      "GL_OP_ALL_2021.MPS Outbound"
    ]
  }
}
```

Wanneer u `primaryAttributeValues` gebruikt, moet het filter `activityTypeIds` aanwezig zijn en mag het alleen activiteiten-id&#39;s bevatten die overeenkomen met de overeenkomstige elementgroep. Als u bijvoorbeeld filtert op webformulierelementen, is alleen de activiteitstype-id Formulier invullen toegestaan in `activityTypeIds` . `primaryAttributeValues` en `primaryAttributeValueIds` kunnen niet samen worden gebruikt.

## Opties

| Parameter | Gegevenstype | Vereist | Notities |
|---|---|---|---|
| filter | Serie [ Voorwerp ] | Ja | Accepteert een array van filters. Precies één `createdAt` -filter moet in de array worden opgenomen. Er kan een optioneel `activityTypeIds` -filter worden opgenomen. De filters worden toegepast op de toegankelijke activiteitenset en de resulterende set activiteiten wordt geretourneerd door de exporttaak. |
| format | String | Nee | Accepteert een van de volgende opties: CSV, TSV, SSV Het geëxporteerde bestand wordt gerenderd als een bestand met door komma&#39;s gescheiden waarden, door tabs gescheiden waarden of een bestand met door spaties gescheiden waarden, indien ingesteld. De standaardwaarde is CSV als de waarde is uitgeschakeld. |
| columnHeaderNames | Object | Nee | Een JSON-object met sleutelwaardeparen van veld- en kolomkopnamen. De sleutel moet de naam zijn van een veld dat is opgenomen in de exporttaak. De waarde is de naam van de geëxporteerde kolomkop voor dat veld. |
| velden | Serie [ Koord ] | Nee | Optionele array van tekenreeksen die veldwaarden bevatten. De weergegeven velden worden opgenomen in het geëxporteerde bestand. Standaard worden de volgende velden geretourneerd: <ul><li>`marketoGUIDleadId`</li><li> `activityDate` </li><li>`activityTypeId` </li><li>`campaignId`</li><li> `primaryAttributeValueId` </li><li>`primaryAttributeValue`</li><li> `attributes`</li></ul>. Deze parameter kan worden gebruikt om het aantal gebieden te verminderen die door een ondergroep van de bovenstaande lijst te specificeren zijn teruggekeerd:`"fields": ["leadId", "activityDate", "activityTypeId"]`. U kunt een extra veld `actionResult` opgeven om de handeling activity op te nemen: `("succeeded", "skipped", or "failed")` . |

## Een taak maken

Als u records wilt exporteren, moet u eerst de taak en de set records definiëren die u wilt ophalen.  Creeer de baan gebruikend [ creeer de Eis van de Activiteit van de Uitvoer ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/createExportActivitiesUsingPOST) eindpunt.  Bij het exporteren van activiteiten kunnen twee primaire filters worden toegepast: `createdAt` (altijd verplicht) en `activityTypeIds` (optioneel).  Het filter `createdAt` wordt gebruikt om een datumbereik te definiëren waarin activiteiten zijn gemaakt. Hierbij wordt gebruikgemaakt van de parameters `startAt` en `endAt` , die beide datetime-velden zijn en die respectievelijk de vroegste toegestane aanmaakdatum en de laatst toegestane aanmaakdatum vertegenwoordigen.  U kunt desgewenst ook filteren op alleen bepaalde typen activiteiten met het filter `activityTypeIds` .  Dit is handig als u resultaten wilt verwijderen die niet relevant zijn voor uw gebruiksscenario.

```
POST /bulk/v1/activities/export/create.json
```

```json
{
   "format": "CSV",
   "filter": {
      "createdAt": {
         "startAt": "2017-07-01T23:59:59-00:00",
         "endAt": "2017-07-31T23:59:59-00:00"
      },
      "activityTypeIds": [
         1,
         12,
         13
      ]
   }
}
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Created",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

De taak heeft nu de status &#39;Gemaakt&#39;, maar bevindt zich nog niet in de verwerkingswachtrij.  Om het in de rij te zetten zodat kan het beginnen verwerkend, het [ in de rij stellende eindpunt van de Baan van de Activiteit van de Uitvoer van de Activiteit ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/enqueueExportActivitiesUsingPOST) gebruikend exportId van de reactie van de aanmaakstatus.

```
POST /bulk/v1/activities/export/{exportId}/enqueue.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "exportId": "ce45a7a1-f19d-4ce2-882c-a3c795940a7d",
         "status": "Queued",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

De status rapporteert dat de taak in de wachtrij is geplaatst.  Wanneer een worker beschikbaar komt voor deze taak, wordt de status overgeschakeld op &quot;Verwerken&quot; en begint de taak met het samenvoegen van records van Marketo.

## Status opiniepeilingtaak

De taakstatus kan alleen worden opgehaald voor taken die door dezelfde API-gebruiker zijn gemaakt.

Het Extraheren van de Activiteit van de Bulk van Marketo is een asynchroon eindpunt, zodat moet de baanstatus worden gepolled om te bepalen wanneer de baan volledig is.  Opiniepeiling die het [ gebruiken krijgt de Status van de Taak van de Activiteit van de Uitvoer ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesStatusUsingGET) als volgt:

```
GET /bulk/v1/activities/export/{exportId}/status.json
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
         "numberOfRecords": 15423,
         "fileSize": 12342,
         "fileChecksum": "sha256:c16514c7e80fcac5ea055dacae9617fc3c29aff5365e3743071313ce0ed2a815"
      }
   ]
}
```

Het statusveld kan reageren met een van de volgende waarden:

- Gemaakt
- In wachtrij
- Verwerking
- Geannuleerd
- Voltooid
- Mislukt

## Uw gegevens ophalen

Zodra de baan volledig is, wint uw gegevens terug gebruikend [ krijgt het 1} eindpunt van het Dossier van de Activiteit van de Uitvoer.](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesFileUsingGET)

```
GET /bulk/v1/activities/export/{exportId}/file.json
```

De reactie bevat een bestand dat is opgemaakt op de manier waarop de taak is geconfigureerd. Het eindpunt antwoordt met de inhoud van het dossier.

Als een gevraagd lead-veld leeg is (geen gegevens bevat), wordt `then null` in het corresponderende veld in het exportbestand geplaatst.  In het onderstaande voorbeeld is het veld `campaignId` voor de geretourneerde activiteit leeg.

```json
marketoGUID,leadId,activityDate,activityTypeId,campaignId,primaryAttributeValueId,primaryAttributeValue,attributes
783957693,5414087,2022-02-13T14:06:20Z,104,8497,1670,MembershipTest1,"{""Reason"":""Changed by Smart Campaign MembershipTestCampaignStepChoice.MembershipTestCampaignStepChoiceSetUp action Change Data Value"",""Program Member ID"":3240303,""Acquired By"":true,""Old Status"":""Not in Program"",""New Status ID"":21,""Success"":false,""New Status"":""On List"",""Old Status ID"":20}"
783958220,5414094,2022-02-13T14:08:50Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":6,""Success"":true,""New Status"":""Attended"",""Old Status ID"":1}"
783958306,5414094,2022-02-13T14:09:16Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Attended"",""New Status ID"":6,""Success"":false,""New Status"":""Attended"",""Old Status ID"":6}"
783961924,5316669,2022-02-13T14:27:21Z,104,11614,2333,Nurture Automation,"{""Program Member ID"":3240306,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":27,""Success"":false,""New Status"":""Member"",""Old Status ID"":26}"
```

Om gedeeltelijke en hervattingsvriendelijke herwinning van gehaalde gegevens te steunen, steunt het dossiereindpunt naar keuze de kopbal van HTTP `Range` van het type `bytes`.  Als de header niet is ingesteld, wordt de gehele inhoud geretourneerd.  U kunt meer lezen over het gebruiken van de kopbal van de Waaier met Marketo [ Bulk Extraheert ](bulk-extract.md).

## Een taak annuleren

Als een baan verkeerd werd gevormd, of onnodig wordt, kan het gemakkelijk worden geannuleerd gebruikend het [ annuleert het eindpunt van de Taak van de Activiteit van de Uitvoer ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/cancelExportActivitiesUsingPOST):

```
POST /bulk/v1/activities/export/{exportId}/cancel.json
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
         "format": "CSV"
      }
   ]
}
```

Deze reactie heeft een status die aangeeft dat de taak is geannuleerd.
