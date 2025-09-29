---
title: Extraheren voor bulklood
feature: REST API
description: Leer hoe u REST API's van Marketo Bulk Lead Extract kunt gebruiken om exporten met datums, lijsten en slimme lijstfilters, aangepaste velden en CSV/TSV-indelingen samen te vouwen.
exl-id: 42796e89-5468-463e-9b67-cce7e798677b
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1195'
ht-degree: 0%

---

# Extraheren voor bulklood

[ BulkLood Extraheren Verwijzing Eindpunt van het Eindpunt ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads)

De reeks van het Uittreksel van de Leiding van het Bulk van REST APIs verstrekt een programmatic interface voor het terugwinnen van grote reeksen lood/persoonverslagen uit Marketo. Deze kan ook worden gebruikt om incrementeel leads op te halen op basis van de gemaakte datum van de record, de meest recente update, het lidmaatschap van een statische lijst of het lidmaatschap van een slimme lijst. De aanbevolen interface voor gebruiksgevallen die een continue uitwisseling van gegevens tussen Marketo en een of meer externe systemen vereisen, voor ETL-, data warehousing- en archiefdoeleinden.

## Machtigingen

De Bulk Lood Extraheren APIs vereist dat de het bezitten API gebruiker een rol met één of allebei van de read-Only lood, of lees-schrijf lood toestemmingen heeft.

## Filters

Leads ondersteunen verschillende filteropties. Voor bepaalde filters, zoals `updatedAt` , `smartListName` en `smartListId` , zijn extra infrastructuurcomponenten nodig die nog niet voor alle abonnementen zijn geïmplementeerd. Per exporttaak kan slechts één filtertype worden opgegeven.

| Filtertype | Gegevenstype | Notities |
|---|---|---|
| createdAt | Datumbereik | Accepteert een JSON-object met de leden `startAt` en `endAt` . `startAt` accepteert een datetime die het lage watermerk vertegenwoordigt en `endAt` accepteert een datetime die het hoge watermerk vertegenwoordigt. Het bereik moet 31 dagen of minder zijn. Datumtijden moeten een ISO-8601-indeling hebben, zonder milliseconden. Taken met dit filtertype retourneren alle toegankelijke records die binnen het datumbereik zijn gemaakt. |
| updatedAt* | Datumbereik | Accepteert een JSON-object met de leden `startAt` en `endAt` . `startAt` accepteert een datetime die het lage watermerk vertegenwoordigt en `endAt` accepteert een datetime die het hoge watermerk vertegenwoordigt. Het bereik moet 31 dagen of minder zijn. Datumtijden moeten een ISO-8601-indeling hebben, zonder milliseconden. Opmerking: dit filter filtert niet op het zichtbare veld &quot;updatedAt&quot;, dat alleen de updates van standaardvelden weerspiegelt. Het filter wordt gebaseerd op wanneer de meest recente veldupdate aan een lead recordJobs met dit filtertype werd gemaakt, retourneert alle toegankelijke records die het laatst binnen het datumbereik zijn bijgewerkt. |
| staticListName | String | Accepteert de naam van een statische lijst. Taken met dit filtertype retourneren alle toegankelijke records die lid zijn van de statische lijst op het moment dat de taak wordt verwerkt. Haal statische lijstnamen terug gebruikend het Get eindpunt van Lijsten. |
| staticListId | Geheel | Accepteert de id van een statische lijst. Taken met dit filtertype retourneren alle toegankelijke records die lid zijn van de statische lijst op het moment dat de taak wordt verwerkt. Haal statische lijstitems terug gebruikend het Get eindpunt van Lijsten. |
| smartListName* | String | Accepteert de naam van een slimme lijst. Taken met dit filtertype retourneren alle toegankelijke records die lid zijn van de slimme lijsten op het moment dat de taak wordt verwerkt. Haal slimme lijstnamen terug gebruikend het Get Slimme eindpunt van Lijsten. |
| smartListId* | Geheel | Accepteert de id van een slimme lijst. Taken met dit filtertype retourneren alle toegankelijke records die lid zijn van de slimme lijsten op het moment dat de taak wordt verwerkt. Haal slimme lijstids terug gebruikend het Get Slimme eindpunt van Lijsten. |

Filtertype is niet beschikbaar voor alle abonnementen. Als deze optie niet beschikbaar is voor uw abonnement, wordt een fout weergegeven wanneer u het eindpunt Taak voor lead exporteren aanroept (&quot;1035, Niet-ondersteund filtertype voor doelabonnement&quot;). Klanten kunnen contact opnemen met Marketo Support om deze functionaliteit in hun abonnement te laten inschakelen.

## Opties

Het eindpunt van de Taak van de Lood van de Uitvoer van de Create verstrekt verscheidene formatterende opties, die de gebruiker de capaciteit geven om bepaalde gebieden binnen het uitgevoerde dossier, de capaciteit te omvatten om kolomkopballen van deze gebieden anders te noemen, en het formaat van het uitgevoerde dossier.

| Parameter | Gegevenstype | Vereist | Notities |
|---|---|---|---|
| velden | Serie [ Koord ] | Ja | De parameter fields accepteert een JSON-array met tekenreeksen. Elke tekenreeks moet de REST API-naam van een Marketo lead-veld zijn. De weergegeven velden worden opgenomen in het geëxporteerde bestand. De kolomkop voor elk veld is de REST API-naam van elk veld, tenzij deze wordt overschreven door columnHeader. Opmerking: Wanneer de functie [!DNL Adobe Experience Cloud Audience Sharing] is ingeschakeld, vindt een cookie-synchronisatieproces plaats waarbij [!DNL Adobe Experience Cloud] ID (ECID) aan Marketo-leads wordt gekoppeld. U kunt het veld ecids opgeven om ECID&#39;s op te nemen in het exportbestand. |
| columnHeaderNames | Object | Nee | Een JSON-object met sleutelwaardeparen van veld- en kolomkopnamen. De sleutel moet de naam zijn van een veld dat is opgenomen in de exporttaak. Dit is de API-naam van het veld die kan worden opgehaald door de optie Lead beschrijven aan te roepen. De waarde is de naam van de geëxporteerde kolomkop voor dat veld. |
| format | String | Nee | Accepteert één van: CSV, TSV, SSV. Het geëxporteerde bestand wordt gerenderd als een bestand met door komma&#39;s gescheiden waarden, door tabs gescheiden waarden of door spaties gescheiden waarden, indien ingesteld. De standaardwaarde is CSV als de waarde is uitgeschakeld. |

## Een taak maken

De parameters voor de baan worden bepaald alvorens de uitvoer te schoppen gebruikend [ creeer het eindpunt van de Baan van de Uitvoer ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/createExportLeadsUsingPOST). We moeten definiëren welke `fields` nodig is voor het exporteren, het type parameters van `filter` , de `format` van het bestand en de namen van kolomkoppen, indien van toepassing.

```
POST /bulk/v1/leads/export/create.json
```

```json
{
   "fields": [
      "firstName",
      "lastName",
      "id",
      "email"
   ],
   "format": "CSV",
   "columnHeaderNames": {
      "firstName": "First Name",
      "lastName": "Last Name",
      "id": "Marketo Id",
      "email": "Email Address"
   },
   "filter": {
      "createdAt": {
         "startAt": "2017-01-01T00:00:00Z",
         "`endAt`": "2017-01-31T00:00:00Z"
      }
   }
}
```

Met dit verzoek wordt een reeks leads geëxporteerd die zijn gemaakt tussen 1 januari 2017 en 31 januari 2017, inclusief de waarden van de overeenkomende velden `firstName` , `lastName` , `id` en `email` .

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

Dit retourneert een statusreactie die aangeeft dat de taak is gemaakt. De taak is gedefinieerd en gemaakt, maar is nog niet uitgeschakeld. Om dit te doen, moet het [ eindpunt van de Baan van de Uitvoer van 0&rbrace; Enqueue het Lood van de Lijn worden geroepen gebruikend exportId van de reactie van de aanmaakstatus:](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/enqueueExportLeadsUsingPOST)

```
POST /bulk/v1/leads/export/{exportId}/enqueue.json
```

```json
{
    "requestId": "147e4#16b24d9b913",
    "result": [
        {
            "exportId": "fad2cd1b-e822-4025-be1e-9caa9cf1d4b8",
            "format": "CSV",
            "status": "Queued",
            "createdAt": "2019-06-04T23:35:43Z",
            "queuedAt": "2019-06-04T23:36:17Z"
        }
    ],
    "success": true
}
```

Dit reageert met een `status` van &quot;In wachtrij&quot; waarna deze wordt ingesteld op &quot;Verwerken&quot; wanneer er een beschikbare exportsleuf is.

## Status opiniepeilingtaak

`Note:` De status kan alleen worden opgehaald voor taken die door dezelfde API-gebruiker zijn gemaakt.

Aangezien dit een asynchroon eindpunt is, moeten wij na het creëren van de baan zijn status onderzoeken om zijn vooruitgang te bepalen. Opiniepeiling die [ gebruikt krijgt de Status van de Taak van de Lood van de Uitvoer ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET) eindpunt. De status wordt slechts eenmaal om de 60 seconden bijgewerkt, dus een lagere stemfrequentie dan dit wordt aanbevolen, en in bijna alle gevallen is dit nog steeds buitensporig. Laten we even kijken naar de opiniepeiling.

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
         "status": "Processing",
         "createdAt": "2017-01-21T11:47:30-08:00",
         "queuedAt": "2017-01-21T11:48:30-08:00",
         "format": "CSV"
      }
   ]
}
```

Het statuseindpunt antwoordt erop wijzend dat de baan nog verwerkt, zodat is het dossier nog niet beschikbaar voor terugwinning. Nadat de taakstatus is gewijzigd in &quot;Voltooid&quot;, wordt de taak voorbereid voor downloaden.

Het statusveld kan reageren op:

- Gemaakt
- In wachtrij
- Verwerking
- Geannuleerd
- Voltooid
- Mislukt

## Uw gegevens ophalen

Om het dossier van een voltooide looduitvoer terug te winnen, roep eenvoudig het [ krijgen het 1&rbrace; eindpunt van het Dossier van de Lood van de Uitvoer &lbrace;met uw ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsFileUsingGET).`exportId`

```
GET /bulk/v1/leads/export/{exportId}/file.json
```

De reactie bevat een bestand dat is opgemaakt op de manier waarop de taak is geconfigureerd. Het eindpunt antwoordt met de inhoud van het dossier.

Als een gevraagd lead-veld leeg is (geen gegevens bevat), wordt `null` in het corresponderende veld in het exportbestand geplaatst. In het onderstaande voorbeeld is het e-mailveld voor de geretourneerde lead leeg.

```csv
firstName,lastName,email,cookies
Russell,Wilson,null,_mch-localhost-1536605780000-12105
```

Om gedeeltelijke en hervattingsvriendelijke herwinning van gehaalde gegevens te steunen, steunt het dossiereindpunt naar keuze de de kopbalWaaier van HTTP van de typebytes. Als de header niet is ingesteld, wordt de gehele inhoud geretourneerd. Lees meer over het gebruiken van de kopbal van de Waaier met Marketo [ Bulk Extraheren ](bulk-extract.md).

## Een taak annuleren

Als een baan verkeerd werd gevormd, of onnodig wordt, kan het gemakkelijk worden geannuleerd gebruikend het [ annuleert de Uitvoer ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/cancelExportLeadsUsingPOST) eindpunt van de Lood van de Uitvoer:

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
         "format": "CSV"
      }
   ]
}
```

Dit reageert met een status die aangeeft dat de taak is geannuleerd.
