---
title: "Bulkactiviteitsextractie"
feature: REST API
description: "Gegevens over de verwerkingsactiviteit van de batch uit Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1381'
ht-degree: 0%

---


# Uitpakken van bulkactiviteiten

[Referentie eindpunt voor bulkactiviteit extraheren](https://developer.adobe.com/marketo-apis/api/mapi/)

De reeks van het Uittreksel van de Activiteit van het Bulk van REST APIs verstrekt een programmatic interface voor het terugwinnen van grote hoeveelheden activiteitengegevens uit Marketo.  Voor gevallen die geen lage latentie vereisen, en significante volumes van activiteitsgegevens uit Marketo, zoals CRM-integratie, ETL, gegevensopslag, en gegevensarchivering moeten overbrengen.

## Machtigingen

De Bulk Activity Extraheren-API&#39;s vereisen dat de API-gebruiker beschikt over de machtiging &quot;Alleen-lezen activiteit&quot; of &quot;activiteit lezen-schrijven&quot;.

## Filters

<table>
  <tbody>
    <tr>
      <td>Filtertype</td>
      <td>Gegevenstype</td>
      <td>Vereist</td>
      <td>Notities</td>
    </tr>
    <tr>
      <td>createdAt</td>
      <td>Datumbereik</td>
      <td>Ja</td>
      <td>Accepteert een JSON-object met de leden startAt en endAt. startAt keurt datetime goed die het laag-watermerk vertegenwoordigen, en endAt keurt datetime goed die het high-watermerk vertegenwoordigt. Het bereik moet 31 dagen of minder zijn. Taken met dit filtertype retourneren alle toegankelijke records die binnen het datumbereik zijn gemaakt. Datetimes moeten de indeling ISO-8601 hebben, zonder milliseconden.</td>
    </tr>
    <tr>
      <td>activityTypeIds</td>
      <td>Array[geheel getal]</td>
      <td>Nee</td>
      <td>Accepteert een JSON-object met één lid, activityTypeIds. De waarde moet een array van gehele getallen zijn die overeenkomen met de gewenste activiteitstypen. De activiteit "Lead verwijderen" wordt niet ondersteund (gebruik <a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET">Verwijderde leads ophalen</a>in plaats daarvan het eindpunt).Hiermee worden id's van het type activiteit opgehaald met de<a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getActivitiesPagingTokenUsingGET">Activiteitstypen ophalen</a>eindpunt.</td>
    </tr>
    <tr>
      <td>primaryAttributeValueIds</td>
      <td>Array[geheel getal]</td>
      <td>Nee</td>
      <td>Accepteert een JSON-object met één lid, primaryAttributeValueIds. De waarde is een array van id's die de primaire kenmerken aangeven waarop moet worden gefilterd. Er kunnen maximaal 50 id's worden opgegeven.De id's zijn de unieke id voor een lead-veld of een element en kunnen worden opgehaald door het juiste REST API-eindpunt aan te roepen. Als u bijvoorbeeld op een specifiek formulier wilt filteren voor de activiteit Formulier invullen, geeft u de naam van het formulier door aan <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET">Formulier ophalen op naam</a> eindpunt om de Vorm Id terug te winnen.Het volgende is een lijst van activiteitstypes waar het primaire attributenfiltreren wordt gesteund.
        <table>
          <tbody>
            <tr>
              <td>Type activiteit</td>
              <td>Primaire kenmerkwaarde-id</td>
              <td>Retrieval Endpoint</td>
              <td>Middelengroep</td>
            </tr>
            <tr>
              <td>Gegevenswaarde wijzigen</td>
              <td>Id van regelveld</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Lead beschrijven</a></td>
              <td>Kenmerknaam</td>
            </tr>
            <tr>
              <td>Score wijzigen</td>
              <td>Id van regelveld</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Lead beschrijven</a></td>
              <td>Kenmerknaam</td>
            </tr>
            <tr>
              <td>Status wijzigen in Progressie</td>
              <td>Programma-id</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByNameUsingGET">Programma ophalen op naam</a></td>
              <td>Marketingprogramma</td>
            </tr>
            <tr>
              <td>Toevoegen aan lijst</td>
              <td>Statische lijst-id</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET">Statische lijst ophalen op naam</a></td>
              <td>Statische lijst</td>
            </tr>
            <tr>
              <td>Verwijderen uit lijst</td>
              <td>Statische lijst-id</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByNameUsingGET">Statische lijst ophalen op naam</a></td>
              <td>Statische lijst</td>
            </tr>
            <tr>
              <td>Formulier invullen</td>
              <td>Formulier-id</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Forms/operation/getLpFormByNameUsingGET">Formulier ophalen op naam</a></td>
              <td>Webformulier</td>
            </tr>
          </tbody>
        </table>
        Bij gebruik van primaryAttributeValueIds moet het activityTypeIds-filter aanwezig zijn en alleen activiteit-id's bevatten die overeenkomen met de corresponderende elementgroep.Voorbeeld:Als u bijvoorbeeld filtert op Web Form-elementen, is alleen het activity type-id Formulier invullen toegestaan in activityTypeIds.Voorbeeld-hoofdtekst:{"filter":{"createdAt":{"startAt": "2202221-1 07-01T23:59:59-00:00","endAt": "2021-07-02T23:59:59-00:00"},"activityTypeIds":[2],"primaryAttributeValueIds": [16,102,95,8]}}}primaryAttributeValueIds en primaryAttributeValues kunnen niet samen worden gebruikt.</td>
    </tr>
    <tr>
      <td>primaryAttributeValues</td>
      <td>Array[String]</td>
      <td>Nee</td>
      <td>Accepteert een JSON-object met één lid, primaryAttributeValues. De waarde is een array van namen die de primaire kenmerken opgeven waarop moet worden gefilterd. Er kunnen maximaal 50 namen worden opgegeven. De namen zijn de unieke id voor een lead-veld of een element en kunnen worden opgehaald door het juiste REST API-eindpunt aan te roepen. Als u bijvoorbeeld wilt filteren op een specifiek formulier voor de activiteit Formulier invullen, geeft u de formulier-id door aan <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5">Formulier ophalen op id</a> eindpunt om de naam van de Vorm terug te winnen.Het volgende is een lijst van activiteitstypes waar het primaire attributenfiltreren wordt gesteund.
        <table>
          <tbody>
            <tr>
              <td>Type activiteit</td>
              <td>Primaire kenmerkwaarde</td>
              <td>Retrieval Endpoint</td>
              <td>Middelengroep</td>
            </tr>
            <tr>
              <td>Gegevenswaarde wijzigen</td>
              <td>Weergavenaam hoofdveld</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Lead beschrijven</a></td>
              <td>Kenmerknaam</td>
            </tr>
            <tr>
              <td>Score wijzigen</td>
              <td>Weergavenaam hoofdveld</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/describeUsingGET_2">Lead beschrijven</a></td>
              <td>Kenmerknaam</td>
            </tr>
            <tr>
              <td>Status wijzigen in Progressie</td>
              <td>Programmanaam</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs/operation/getProgramByIdUsingGET">Programma ophalen op id</a></td>
              <td>Marketingprogramma</td>
            </tr>
            <tr>
              <td>Toevoegen aan lijst</td>
              <td>Statische lijstnaam</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET">Statische lijst ophalen op id</a></td>
              <td>Statische lijst</td>
            </tr>
            <tr>
              <td>Verwijderen uit lijst</td>
              <td>Statische lijstnaam</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Static-Lists/operation/getStaticListByIdUsingGET">Statische lijst ophalen op id</a></td>
              <td>Statische lijst</td>
            </tr>
            <tr>
              <td>Formulier invullen</td>
              <td>Formuliernaam</td>
              <td><a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Sales-Persons/operation/describeUsingGET_5">Formulier ophalen op id</a></td>
              <td>Webformulier</td>
            </tr>
          </tbody>
        </table>
        Let op: u moet "&lt;<em>programma</em>&gt;.&lt;<em>element</em>&gt;"aantekening om de naam voor de volgende activagroepen uniek te specificeren: Het Marketingprogramma, de Statische Lijst, Web Form.Example:Bijvoorbeeld, een vorm met naam "MPS Uitgaand"die zich onder programma met naam "GL_OP_ALL_2021" zou als "GL_OP_ALL_2021.MPS Uitgaand"filter"wordt gespecificeerd.VoorbeeldBody:{"filter":{"createdAt":{"startAt": "2021-07-01T23":59:59-00:00","endAt": "2021-07-02T23:59:59-00:00"},"activityTypeIds":[2],"primaryAttributeValues":["GL_OP_ALL_2021.MPS Uitgaand"]}}} wanneer het gebruiken van primaryAttributeValues, moet het activityTypeIds filter aanwezig zijn en slechts activiteitenids bevatten die de overeenkomstige activagroep aanpassen. Bijvoorbeeld, als u op de activa van de Vorm van het Web filtreert, dan wordt slechts de "Vul Vorm"activiteitentype id toegestaan in activityTypeIds.primaryAttributeValues en primaryAttributeValueIds kunnen niet samen worden gebruikt.</td>
    </tr>
  </tbody>
</table>

## Opties

| Parameter | Gegevenstype | Vereist | Notities |
|---|---|---|---|
| filter | Array[Object] | Ja | Accepteert een array van filters. Precies één createdAt-filter moet in de array worden opgenomen. Een optioneel activityTypeIds-filter kan worden opgenomen. De filters worden toegepast op de toegankelijke activiteitenset en de resulterende set activiteiten wordt geretourneerd door de exporttaak. |
| format | String | Nee | Accepteert een van de volgende opties: CSV, TSV, SSV Het geëxporteerde bestand wordt gerenderd als een bestand met door komma&#39;s gescheiden waarden, door tabs gescheiden waarden of een bestand met door spaties gescheiden waarden, respectievelijk als dit is ingesteld. Standaard naar CSV als dit niet is ingesteld. |
| columnHeaderNames | Object | Nee | Een JSON-object met sleutelwaardeparen van veld- en kolomkopnamen. De sleutel moet de naam zijn van een veld dat is opgenomen in de exporttaak. De waarde is de naam van de geëxporteerde kolomkop voor dat veld. |
| velden | Array[String] | Nee | Optionele array van tekenreeksen die veldwaarden bevatten. De weergegeven velden worden opgenomen in het geëxporteerde bestand. Standaard worden de volgende velden geretourneerd: `marketoGUIDleadId` `activityDate` `activityTypeId` `campaignId` `primaryAttributeValueId` `primaryAttributeValueattributes`,Deze parameter kan worden gebruikt om het aantal velden te verminderen dat wordt geretourneerd door een subset van de bovenstaande lijst op te geven.Voorbeeld:&quot;velden&quot;: [&quot;leadId&quot;, &quot;activityDate&quot;, &quot;activityTypeId&quot;]Een extra veld &quot;actionResult&quot; kan worden opgegeven om de handeling activity op te nemen (&quot;success&quot;, &quot;overgeslagen&quot; of &quot;failed&quot;). |


## Een taak maken

Als u records wilt exporteren, moet u eerst de taak en de set records definiëren die u wilt ophalen.  De taak maken met de opdracht [Taak voor exportactiviteit maken](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/createExportActivitiesUsingPOST) eindpunt.  Bij het exporteren van activiteiten kunnen twee primaire filters worden toegepast: `createdAt`, die altijd vereist is, en `activityTypeIds`, die optioneel is.  Het filter createdAt wordt gebruikt om een datumbereik te definiëren waarin activiteiten zijn gemaakt, met behulp van de `startAt` en `endAt` parameters, die allebei datetime gebieden zijn, en de vroegste toegelaten aanmaakdatum vertegenwoordigen, respectievelijk de recentste toegelaten aanmaakdatum.  U kunt desgewenst ook filteren op alleen bepaalde typen activiteiten, waarbij u de opdracht `activityTypeIds` filter.  Dit is handig als u resultaten wilt verwijderen die niet relevant zijn voor uw gebruiksgeval.

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

De taak heeft nu de status &#39;Gemaakt&#39;, maar bevindt zich nog niet in de verwerkingswachtrij.  Om het in de rij te zetten zodat het met verwerking kan beginnen, moeten wij roepen [Taak voor exportactiviteit opgeven](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/enqueueExportActivitiesUsingPOST) eindpunt gebruikend exportId van de reactie van de aanmaakstatus.

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

De status rapporteert nu dat de taak in de wachtrij is geplaatst.  Wanneer een worker beschikbaar komt voor deze taak, wordt de status overgeschakeld op &quot;Verwerken&quot; en wordt begonnen met het samenvoegen van records uit Marketo.

## Status opiniepeilingtaak

De taakstatus kan alleen worden opgehaald voor taken die door dezelfde API-gebruiker zijn gemaakt.

Het Extraheren van de Activiteit van de Bulk van Marketo is een asynchroon eindpunt, zodat moet de baanstatus worden gepolled om te bepalen wanneer de baan volledig is.  Opiniepeiling met de opdracht [Taakstatus exportactiviteit ophalen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesStatusUsingGET) eindpunt als volgt:

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

Als de taak is voltooid, haalt u uw gegevens op met de [Exportactiviteitenbestand ophalen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/getExportActivitiesFileUsingGET) eindpunt.

```
GET /bulk/v1/activities/export/{exportId}/file.json
```

De reactie bevat een bestand dat is opgemaakt op de manier waarop de taak is geconfigureerd. Het eindpunt antwoordt met de inhoud van het dossier.

Als een gevraagd lead-veld leeg is (geen gegevens bevat), `then null` wordt in het desbetreffende veld in het exportbestand geplaatst.  In het onderstaande voorbeeld is het veld campagneId voor de geretourneerde activiteit leeg.

```json
marketoGUID,leadId,activityDate,activityTypeId,campaignId,primaryAttributeValueId,primaryAttributeValue,attributes
783957693,5414087,2022-02-13T14:06:20Z,104,8497,1670,MembershipTest1,"{""Reason"":""Changed by Smart Campaign MembershipTestCampaignStepChoice.MembershipTestCampaignStepChoiceSetUp action Change Data Value"",""Program Member ID"":3240303,""Acquired By"":true,""Old Status"":""Not in Program"",""New Status ID"":21,""Success"":false,""New Status"":""On List"",""Old Status ID"":20}"
783958220,5414094,2022-02-13T14:08:50Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":6,""Success"":true,""New Status"":""Attended"",""Old Status ID"":1}"
783958306,5414094,2022-02-13T14:09:16Z,104,17240,3569,SuccessWebCPS,"{""Program Member ID"":3240305,""Acquired By"":false,""Old Status"":""Attended"",""New Status ID"":6,""Success"":false,""New Status"":""Attended"",""Old Status ID"":6}"
783961924,5316669,2022-02-13T14:27:21Z,104,11614,2333,Nurture Automation,"{""Program Member ID"":3240306,""Acquired By"":false,""Old Status"":""Not in Program"",""New Status ID"":27,""Success"":false,""New Status"":""Member"",""Old Status ID"":26}"
```

Om gedeeltelijke en hervattingsvriendelijke herwinning van gehaalde gegevens te steunen, steunt het dossiereindpunt naar keuze de kopbal van HTTP `Range` van het type `bytes`.  Als de header niet is ingesteld, wordt de gehele inhoud geretourneerd.  U kunt meer lezen over het gebruik van de Range-header met Marketo [Bulk extraheren](bulk-extract.md).

## Een taak annuleren

Als een baan verkeerd werd gevormd, of onnodig wordt, kan het gemakkelijk worden geannuleerd gebruikend [Taak exportactiviteit annuleren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Activities/operation/cancelExportActivitiesUsingPOST) eindpunt:

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
