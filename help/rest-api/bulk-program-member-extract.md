---
title: Bulkprogrammalid extraheren
feature: REST API
description: Met Marketo Bulk Program Member Extract REST API's kunt u grote lidrecords exporteren voor ETL, data warehousing en archivering, met machtigingen en veldmetagegevens.
exl-id: 6e0a6bab-2807-429d-9c91-245076a34680
source-git-commit: 59684e1c5a8082ad12f1e4bfc854c0d2dde35d2a
workflow-type: tm+mt
source-wordcount: '1294'
ht-degree: 1%

---

# Bulkprogrammalid extraheren

[Referentie voor eindpunt voor uittrekpunt van lid van het bulkprogramma](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members)

De reeks van het Uittreksel van het Lid van het Bulk van het Programma van REST APIs verstrekt een programmatic interface voor het terugwinnen van grote reeksen verslagen van het programmalid uit Marketo. Dit is de aanbevolen interface voor gebruiksgevallen die een continue uitwisseling van gegevens tussen Marketo en een of meer externe systemen vereisen, voor ETL-, data warehousing- en archiefdoeleinden.

## Machtigingen

De Bulk APIs van het Lid van het Programma Extraheren vereist dat de het bezitten API gebruiker een rol met één of allebei van de read-Only Lead, of lees-schrijf Lead toestemmingen heeft.

## Beschrijven

[&#x200B; beschrijf het Lid van het Programma &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi#tag/Program-Members/operation/describeProgramMemberUsingGET2) is de primaire bron van waarheid voor of de gebieden voor gebruik, en meta-gegevens over die gebieden beschikbaar zijn. Het attribuut `name` bevat de REST API-naam.

```http
GET /rest/v1/programs/members/describe.json
```

```json
{
    "requestId": "f813#1791563c7cc",
    "result": [
        {
            "name": "API Program Membership",
            "description": "Map for API program membership fields",
            "createdAt": "2021-03-20T01:30:05Z",
            "updatedAt": "2021-03-20T01:30:05Z",
            "dedupeFields": [
                "leadId",
                "programId"
            ],
            "searchableFields": [
                [
                    "leadId"
                ],
                [
                    "myCustomField"
                ],
                [
                    "reachedSuccess"
                ],
                [
                    "statusName"
                ]
            ],
            "fields": [
                {
                    "name": "acquiredBy",
                    "displayName": "acquiredBy",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "attendanceLikelihood",
                    "displayName": "attendanceLikelihood",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "createdAt",
                    "displayName": "createdAt",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "isExhausted",
                    "displayName": "isExhausted",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "leadId",
                    "displayName": "leadId",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "membershipDate",
                    "displayName": "membershipDate",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "nurtureCadence",
                    "displayName": "nurtureCadence",
                    "dataType": "string",
                    "length": 4,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "program",
                    "displayName": "program",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "programId",
                    "displayName": "programId",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "reachedSuccess",
                    "displayName": "reachedSuccess",
                    "dataType": "boolean",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "reachedSuccessDate",
                    "displayName": "reachedSuccessDate",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "registrationLikelihood",
                    "displayName": "registrationLikelihood",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "statusName",
                    "displayName": "statusName",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "statusReason",
                    "displayName": "statusReason",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "trackName",
                    "displayName": "trackName",
                    "dataType": "string",
                    "length": 255,
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "updatedAt",
                    "displayName": "updatedAt",
                    "dataType": "datetime",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "waitlistPriority",
                    "displayName": "waitlistPriority",
                    "dataType": "integer",
                    "updateable": false,
                    "crmManaged": false
                },
                {
                    "name": "myCustomField",
                    "displayName": "myCustomField",
                    "dataType": "string",
                    "length": 255,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "registrationCode",
                    "displayName": "registrationCode",
                    "dataType": "string",
                    "length": 100,
                    "updateable": true,
                    "crmManaged": false
                },
                {
                    "name": "webinarUrl",
                    "displayName": "webinarUrl",
                    "dataType": "string",
                    "length": 2000,
                    "updateable": true,
                    "crmManaged": false
                }
            ]
        }
    ],
    "success": true
}
```

## Filters

Programmaleden ondersteunen verschillende filteropties. Er kunnen meerdere filtertypen worden opgegeven voor een taak. In dat geval worden ze ANDed bij elkaar geplaatst. U moet het filter `programId` of `programIds` opgeven. Alle andere filters zijn optioneel. Voor het filter `updatedAt` zijn aanvullende infrastructuurcomponenten vereist die nog niet voor alle abonnementen zijn geïmplementeerd.

<table>
  <tbody>
    <tr>
      <td>Filtertype</td>
      <td>Gegevenstype</td>
      <td>Notities</td>
    </tr>
    <tr>
      <td>programId</td>
      <td>Geheel</td>
      <td>Accepteert de id van een programma. Taken retourneren alle toegankelijke records die lid zijn van het programma op het moment dat de taak wordt verwerkt.Haal programma ids terug gebruikend <a href="https://developer.adobe.com/marketo-apis/api/asset#tag/Programs"> krijgt het </a> eindpunt van Programma's.Kan niet worden gebruikt met filter programIds.</td>
    </tr>
    <tr>
      <td>programIds</td>
      <td>Array[geheel getal]</td>
      <td>Accepteert een array van maximaal 10 programma-id's. Taken retourneren alle toegankelijke records die lid zijn van de programma's op het moment dat de taak wordt verwerkt.Als eerste veld wordt een extra veld met de naam programId toegevoegd aan het exportbestand. In dit veld wordt aangegeven uit welk programma een lidmaatschapsrecord is opgebouwd.Haal programma ids terug gebruikend <a href="https://developer.adobe.com/marketo-apis/api/asset#tag/Programs"> krijgt het </a> eindpunt van Programma's.Kan niet worden gebruikt met filter programId.</td>
    </tr>
    <tr>
      <td>isExhausted</td>
      <td>Boolean</td>
      <td>Accepteert boolean die wordt gebruikt om de verslagen van het programmalidmaatschap voor <a href="https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/drip-nurturing/using-engagement-programs/people-who-have-exhausted-content"> mensen te filtreren die inhoud </a> uitgeput hebben.</td>
    </tr>
    <tr>
      <td>nurtureCadence</td>
      <td>String</td>
      <td>Accepteert een tekenreeks die wordt gebruikt voor het filteren van de lidmaatschapsrecords voor een bepaalde cultuur.Toegestane waarden zijn:
        <ul>
          <li>pause - cadence is paused</li>
          <li>norm - normaal geluid</li>
        </ul></td>
    </tr>
    <tr>
      <td>statusNames</td>
      <td>Array[String]</td>
      <td>Accepteert een array van statusnamen van programmaleden. Meerdere statusnamen zijn ORed samen.Taken met dit filtertype retourneren alle toegankelijke records waarvan de status van het programmalid overeenkomt met een van de opgegeven statusnamen. U kunt zowel standaardnamen als door de gebruiker gedefinieerde statusnamen gebruiken.Als het filter statusNames met &grave; programIds' filter wordt gebruikt, dan wordt elk programma gecontroleerd lidmaatschapsverslagen waarvan status om het even welke statusnamen aanpast. Als een statusnaam niet wordt gevonden in een van de programma's, wordt de fout "1003, Ongeldige gegevens" geretourneerd.
        <table>
          <tbody>
            <tr>
              <td>Bijgewoond</td>
              <td>Bijgewoond op bestelling</td>
              <td>Afgekeerd</td>
            </tr>
            <tr>
              <td>Geklikt</td>
              <td>Contactpersoon</td>
              <td>Omgezet</td>
            </tr>
            <tr>
              <td>Betrokken</td>
              <td>Formulier met vulling</td>
              <td>Betrokken</td>
            </tr>
            <tr>
              <td>Uitgenodigd</td>
              <td>Lid</td>
              <td>Geen weergave</td>
            </tr>
            <tr>
              <td>Niet in programma</td>
              <td>Op lijst</td>
              <td>Geopend</td>
            </tr>
            <tr>
              <td>Geregistreerd</td>
              <td>Registreren</td>
              <td>Registratiefout</td>
            </tr>
            <tr>
              <td>Verzonden</td>
              <td>Geabonneerd</td>
              <td>Niet geabonneerd</td>
            </tr>
            <tr>
              <td>Weergegeven</td>
              <td>Bezocht</td>
              <td>Bezochte Booth</td>
            </tr>
            <tr>
              <td>Waitlijst</td>
              <td>Webinhoud</td>
              <td></td>
            </tr>
          </tbody>
        </table></td>
    </tr>
    <tr>
      <td>updatedAt*</td>
      <td>Datumbereik</td>
      <td>Accepteert een JSON-object met de leden startAt en endAt. startAt keurt datetime goed die het laag-watermerk vertegenwoordigen, en endAt keurt datetime goed die het high-watermerk vertegenwoordigt. Het bereik moet 31 dagen of minder zijn. Datumtijden moeten een ISO-8601-indeling hebben, zonder milliseconden.Taken met dit filtertype retourneren alle toegankelijke records die het laatst binnen het datumbereik zijn bijgewerkt.</td>
    </tr>
  </tbody>
</table>

Filtertype is niet beschikbaar voor alle abonnementen. Als deze optie niet beschikbaar is voor uw abonnement, wordt een fout weergegeven wanneer u het eindpunt Taak van het lid van het programma voor export maken aanroept (&quot;1035, Niet-ondersteund filtertype voor doelabonnement&quot;). Klanten kunnen contact opnemen met Marketo Support om deze functionaliteit in hun abonnement te laten inschakelen.

## Opties

Het eindpunt van de Taak van het Lid van het Programma van de Uitvoer leidt verstrekt verscheidene formatterende opties. Met deze opties kan de gebruiker:

- Geef de velden op die u wilt opnemen in het geëxporteerde bestand
- De naam van kolomkoppen in deze velden wijzigen
- De indeling van het geëxporteerde bestand opgeven

| Parameter | Gegevenstype | Vereist | Notities |
| --- | --- | --- | --- |
| velden | Serie [ Koord ] | Ja | De parameter fields accepteert een JSON-array met tekenreeksen. De weergegeven velden worden opgenomen in het geëxporteerde bestand. De volgende veldtypen kunnen worden geëxporteerd: `LeadCustom` `LeadProgram` MemberCustom `ProgramMember` . Geef een veld op met de naam REST API, die kan worden opgehaald met de eindpunten van het programmalid beschrijven Lead2 en/of beschrijven. |
| columnHeaderNames | Object | Nee | Een JSON-object met sleutelwaardeparen van veld- en kolomkopnamen. De sleutel moet de naam zijn van een veld dat is opgenomen in de exporttaak. De waarde is de naam van de geëxporteerde kolomkop voor dat veld. |
| format | String | Nee | Accepteert één van: CSV, TSV, SSV. Het geëxporteerde bestand wordt gerenderd als een bestand met door komma&#39;s gescheiden waarden, door tabs gescheiden waarden of door spaties gescheiden waarden, indien ingesteld. De standaardwaarde is CSV als de waarde is uitgeschakeld. |

## Een taak maken

De parameters voor de baan worden bepaald alvorens de uitvoer te schoppen gebruikend [&#x200B; creeer het eindpunt van de Taak van het Lid van het Programma van de Uitvoer &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members/operation/createExportProgramMembersUsingPOST). We moeten definiëren welke `filter` de programma-id bevat en welke `fields` nodig is voor het exporteren. Desgewenst kunnen de `format` van het bestand en de `columnHeaderNames` worden gedefinieerd.

```http
POST /bulk/v1/program/members/export/create.json
```

```json
{
   "format": "CSV",
   "fields": [
        "firstName",
        "lastName",
        "email",
        "membershipDate",
        "program",
        "statusName",
        "leadId",
        "reachedSuccess",
        "leadCustomField01",
        "leadCustomField02",
        "pMCustomField01",
        "pMCustomField02"
   ],
   "filter": {
      "programId":1044
   }
}
```

```json
{
    "requestId": "4d44#16f92734f6e",
    "result": [
        {
            "exportId": "b5ca52a9-5ecb-4966-b5a9-11659a8b4c2b",
            "format": "CSV",
            "status": "Created",
            "createdAt": "2020-01-11T02:33:48Z"
        }
    ],
    "success": true
}
```

Dit retourneert een statusreactie die aangeeft dat de taak is gemaakt. De taak is gedefinieerd en gemaakt, maar is nog niet uitgeschakeld. Om dit te doen, moet het [&#128279;](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members/operation/enqueueExportProgramMembersUsingPOST) eindpunt van de Taak van het Lid van het Programma van de Enqueue  &lbrace;worden geroepen gebruikend `exportId` van de reactie van de aanmaakstatus:

```http
POST /bulk/v1/program/members/export/{exportId}/enqueue.json
```

```json
{
    "requestId": "d70b#16f9273ae32",
    "result": [
        {
            "exportId": "b5ca52a9-5ecb-4966-b5a9-11659a8b4c2b",
            "format": "CSV",
            "status": "Queued",
            "createdAt": "2020-01-11T02:33:48Z",
            "queuedAt": "2020-01-11T02:34:13Z"
        }
    ],
    "success": true
}
```

Dit zal met een aanvankelijke `status` van &quot;In de rij&quot;antwoorden waarna zal het aan &quot;Verwerking&quot;worden geplaatst wanneer er een beschikbare uitvoergroef is.

## Status opiniepeilingtaak

Opmerking: de status kan alleen worden opgehaald voor taken die door dezelfde API-gebruiker zijn gemaakt.

Aangezien dit een asynchroon eindpunt is, moeten wij na het creëren van de baan zijn status onderzoeken om zijn vooruitgang te bepalen. Opiniepeiling die het [&#x200B; gebruiken krijgt de Status van de Taak van het Lid van het Programma van de Uitvoer &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET) eindpunt. De status wordt slechts eenmaal om de 60 seconden bijgewerkt, dus een lagere stemfrequentie dan dit wordt aanbevolen, en in bijna alle gevallen is dit nog steeds buitensporig. Het statusveld kan reageren met elk van de volgende opties: Gemaakt, In wachtrij geplaatst, Verwerken, Geannuleerd, Voltooid, Mislukt.

```http
GET /bulk/v1/program/members/export/{exportId}/status.json
```

```json
{
    "requestId": "9a40#16f9274d250",
    "result": [
        {
            "exportId": "b5ca52a9-5ecb-4966-b5a9-11659a8b4c2b",
            "format": "CSV",
            "status": "Processing",
            "createdAt": "2020-01-11T02:33:48Z",
            "queuedAt": "2020-01-11T02:34:13Z",
            "startedAt": "2020-01-11T02:35:19Z"
        }
    ],
    "success": true
}
```

Het statuseindpunt antwoordt erop wijzend dat de baan nog verwerkt, zodat is het dossier nog niet beschikbaar voor terugwinning. Zodra de taak `status` is gewijzigd in &quot;Voltooid&quot;, kan deze worden gedownload.

```json
{
    "requestId": "11ad1#16f9ff6da23",
    "result": [
        {
            "exportId": "1118dc83-273b-4d44-becb-4d212fece550",
            "format": "CSV",
            "status": "Completed",
            "createdAt": "2020-01-11T02:33:48Z",
            "queuedAt": "2020-01-11T02:34:13Z",
            "startedAt": "2020-01-11T02:35:19Z"
            "finishedAt": "2020-01-11T02:36:12Z",
            "numberOfRecords": 13,
            "fileSize": 1752,
            "fileChecksum": "sha256:b3c8e70e6e501cf1025e345a66b409d4fd07364c7da773cfa68a2b68ce1a7212"
        }
    ],
    "success": true
}
```

## Uw gegevens ophalen

Om het dossier van een voltooide uitvoer van het programmalid terug te winnen, roep eenvoudig het [&#x200B; krijgen het 1&rbrace; eindpunt van het Dossier van het Lid van het Programma van de Uitvoer met uw `exportId`.](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members/operation/getExportProgramMembersFileUsingGET)

De reactie bevat een bestand dat is opgemaakt op de manier waarop de taak is geconfigureerd. Het eindpunt antwoordt met de inhoud van het dossier. Als een gevraagd veld voor een programmalid leeg is (geen gegevens bevat), wordt `null` in het corresponderende veld in het exportbestand geplaatst.

```http
GET /bulk/v1/program/members/export/{exportId}/file.json
```

```text
firstName,lastName,email,Member Date,Program,Status,Lead Id,Success,leadCustomField01,leadCustomField02,pMCustomField01,pMCustomField02
Meera,Reed,mree@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1789,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Jon,Umber,jumb@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1790,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Lyanna,Mormont,lmor@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1791,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Rickon,Stark,rsta@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1792,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Hodor,null,hodor@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1793,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Osha,null,osha@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1794,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Jojen,Reed,Jree@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1795,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Rickard,Karstark,rkar@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1796,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Maester,Luwin,mluw@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1797,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Rodrik,Cassel,rcas@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1798,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Jory,Cassel,jcas@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1799,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
Septa,Mordane,smor@housestark.com,2020-01-08T18:10:26Z,PMCF Program,On List,1800,false,Lead01_Value,Lead02_Value,PM01_Value,PM02_Value
```

Om gedeeltelijke en hervattingsvriendelijke herwinning van gehaalde gegevens te steunen, steunt het dossiereindpunt naar keuze de de kopbalWaaier van HTTP van de typebytes. Als de header niet is ingesteld, wordt de gehele inhoud geretourneerd. U kunt meer lezen over het gebruiken van de kopbal van de Waaier met Marketo [&#x200B; Bulk Extraheert &#x200B;](bulk-extract.md).

## Een taak annuleren

Als een baan verkeerd werd gevormd, of onnodig wordt, kan het gemakkelijk worden geannuleerd gebruikend het [&#x200B; annuleert het eindpunt van de Taak van het Lid van het Programma van de Uitvoer &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi#tag/Bulk-Export-Program-Members/operation/cancelExportProgramMembersUsingPOST):

```http
POST /bulk/v1/program/members/export/{exportId}/cancel.json
```

```json
{
    "requestId": "bb4f#16f86727f89",
    "result": [
        {
            "exportId": "f0d3520c-3a60-4568-9e71-2e619d3805a4",
            "format": "CSV",
            "status": "Cancelled",
            "createdAt": "2020-01-07T21:47:35Z"
        }
    ],
    "success": true
}
```

Dit reageert met een `status` die aangeeft dat de taak is geannuleerd.
