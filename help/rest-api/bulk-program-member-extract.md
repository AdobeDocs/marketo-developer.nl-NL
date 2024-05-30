---
title: "Bulk Program Member Extract"
feature: REST API
description: "Batchverwerking van gegevensextractie door leden."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '1142'
ht-degree: 0%

---


# Bulkprogrammalid extraheren

[Referentie voor eindpunt voor uittrekpunt van lid van het bulkprogramma](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members)

De reeks van het Uittreksel van het Lid van het Bulk van het Programma van REST APIs verstrekt een programmatic interface voor het terugwinnen van grote reeksen verslagen van het programmalid uit Marketo. Dit is de aanbevolen interface voor gebruiksgevallen die een continue uitwisseling van gegevens tussen Marketo en een of meer externe systemen vereisen, voor ETL-, data warehousing- en archiefdoeleinden.

## Machtigingen

De Bulk APIs van het Lid van het Programma Extraheren vereist dat de het bezitten API gebruiker een rol met één of allebei van de read-Only Lead, of lees-schrijf Lead toestemmingen heeft.

## Beschrijven

[Lid van programma beschrijven](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Program-Members/operation/describeProgramMemberUsingGET2) is de primaire bron van de waarheid voor of de gebieden voor gebruik, en meta-gegevens over die gebieden beschikbaar zijn. De `name` bevat de naam van de REST API.

```
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

Programmaleden ondersteunen verschillende filteropties. Er kunnen meerdere filtertypen worden opgegeven voor een taak. In dat geval worden ze ANDed bij elkaar geplaatst. U moet de `programId` of de `programIds` filter. Alle andere filters zijn optioneel. De `updatedAt` filter vereist extra infrastructuurcomponenten die nog niet op alle abonnementen zijn uitgevoerd.

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
      <td>Accepteert de id van een programma. Taken retourneren alle toegankelijke records die lid zijn van het programma op het moment dat de taak wordt verwerkt.Programma-id's ophalen met de opdracht <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs">Programma's ophalen</a> parameter.Kan niet worden gebruikt met filter programIds.</td>
    </tr>
    <tr>
      <td>programIds</td>
      <td>Array[geheel getal]</td>
      <td>Accepteert een array van maximaal 10 programma-id's. Taken retourneren alle toegankelijke records die lid zijn van de programma's op het moment dat de taak wordt verwerkt. Een extra veld "programId" wordt als eerste veld toegevoegd aan het exportbestand. In dit veld wordt het programma aangegeven waaruit een register voor het lidmaatschap van het programma is opgehaald.De id van het programma ophalen met de <a href="https://developer.adobe.com/marketo-apis/api/asset/#tag/Programs">Programma's ophalen</a> eindpunt.Kan niet met programId filter worden gebruikt.</td>
    </tr>
    <tr>
      <td>isExhausted</td>
      <td>Boolean</td>
      <td>Accepteert een Booleaanse waarde die wordt gebruikt voor het filteren van de lidmaatschapsrecords voor programma's <a href="https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/email-marketing/drip-nurturing/using-engagement-programs/people-who-have-exhausted-content">personen met een verouderde inhoud</a>.</td>
    </tr>
    <tr>
      <td>nurtureCadence</td>
      <td>String</td>
      <td>Accepteert een tekenreeks die wordt gebruikt voor het filteren van de lidmaatschapsrecords van een programma voor een bepaalde boomkwekerij. De toegestane waarden zijn:
        <ul>
          <li>pause - cadence is paused</li>
          <li>norm - normaal geluid</li>
        </ul></td>
    </tr>
    <tr>
      <td>statusNames</td>
      <td>Array[String]</td>
      <td>Accepteert een array van statusnamen van programmaleden. De veelvoudige statusnamen worden ORed samen.Banen met dit filtertype keren alle toegankelijke verslagen terug de waarvan status van het programmalid om het even welke gespecificeerde statusnamen aanpast. Zowel de standaardnamen als de door de gebruiker gedefinieerde statusnamen kunnen worden gebruikt. Als het filter statusNames wordt gebruikt met het filter 'programIds', wordt elk programma gecontroleerd op lidmaatschapsrecords waarvan de status overeenkomt met een van de statusnamen. Als een statusnaam niet wordt gevonden in een van de programma's, wordt de fout "1003, Ongeldige gegevens" geretourneerd.
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
      <td>Accepteert een JSON-object met de leden startAt en endAt. startAt keurt datetime goed die het laag-watermerk vertegenwoordigen, en endAt keurt datetime goed die het high-watermerk vertegenwoordigt. Het bereik moet 31 dagen of minder zijn. Datumtijden moeten een ISO-8601-indeling hebben, zonder milliseconden.Jobs met dit filtertype retourneert alle toegankelijke records die het laatst binnen het datumbereik zijn bijgewerkt.</td>
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
|---|---|---|---|
| velden | Array[String] | Ja | De parameter fields accepteert een JSON-array met tekenreeksen. De weergegeven velden worden opgenomen in het geëxporteerde bestand. De volgende veldtypen kunnen worden geëxporteerd:`LeadCustom` `LeadProgram` MemberCustom `ProgramMember`. Geef een veld op met de REST API-naam die kan worden opgehaald met de endpunten Lead2 beschrijven en/of Programmalid beschrijven. |
| columnHeaderNames | Object | Nee | Een JSON-object met sleutelwaardeparen van veld- en kolomkopnamen. De sleutel moet de naam zijn van een veld dat is opgenomen in de exporttaak. De waarde is de naam van de geëxporteerde kolomkop voor dat veld. |
| format | String | Nee | Accepteert één van: CSV, TSV, SSV. Het geëxporteerde bestand wordt gerenderd als een bestand met door komma&#39;s gescheiden waarden, door tabs gescheiden waarden of door spaties gescheiden waarden, indien ingesteld. De standaardwaarde is CSV als de waarde is uitgeschakeld. |


## Een taak maken

De parameters voor de taak worden gedefinieerd voordat de exportbewerking wordt gestart met de opdracht [Taak voor lid van exportprogramma maken](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/createExportProgramMembersUsingPOST) eindpunt. We moeten de `filter` met de programma-id en de `fields` die nodig zijn voor de uitvoer. Optioneel kunnen we de `format` van het bestand en de `columnHeaderNames`.

```
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

Dit retourneert een statusreactie die aangeeft dat de taak is gemaakt. De taak is gedefinieerd en gemaakt, maar is nog niet uitgeschakeld. Daartoe [Taak van het Lid van het Programma van de Uitvoer](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/enqueueExportProgramMembersUsingPOST) het eindpunt moet worden geroepen gebruikend `exportId` vanuit de reactie op de aanmaakstatus:

```
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

Dit zal met een eerste `status` van &quot;In de wachtrij geplaatst&quot; waarna deze wordt ingesteld op &quot;Verwerking&quot; wanneer er een beschikbare exportsleuf is.

## Status opiniepeilingtaak

Opmerking: de status kan alleen worden opgehaald voor taken die door dezelfde API-gebruiker zijn gemaakt.

Aangezien dit een asynchroon eindpunt is, moeten wij na het creëren van de baan zijn status onderzoeken om zijn vooruitgang te bepalen. Opiniepeiling met de opdracht [Status van lid van exportprogramma ophalen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Leads/operation/getExportLeadsStatusUsingGET) eindpunt. De status wordt slechts eenmaal om de 60 seconden bijgewerkt, dus een lagere stemfrequentie dan dit wordt aanbevolen, en in bijna alle gevallen is dit nog steeds buitensporig. Het statusveld kan reageren met elk van de volgende opties: Gemaakt, In wachtrij geplaatst, Verwerken, Geannuleerd, Voltooid, Mislukt.

```
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

Het statuseindpunt antwoordt erop wijzend dat de baan nog verwerkt, zodat is het dossier nog niet beschikbaar voor terugwinning. Eenmaal de taak `status` Als de waarde &quot;Voltooid&quot; is, kan deze worden gedownload.

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

Als u het bestand van een voltooide programmalidexport wilt ophalen, roept u gewoon de [Lid-bestand van exportprogramma ophalen](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/getExportProgramMembersFileUsingGET) eindpunt met uw `exportId`.

De reactie bevat een bestand dat is opgemaakt op de manier waarop de taak is geconfigureerd. Het eindpunt antwoordt met de inhoud van het dossier. Als een gevraagd veld voor een programmalid leeg is (geen gegevens bevat), `null` wordt in het desbetreffende veld in het exportbestand geplaatst.

```
GET /bulk/v1/program/members/export/{exportId}/file.json
```

```
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

Om gedeeltelijke en hervattingsvriendelijke herwinning van gehaalde gegevens te steunen, steunt het dossiereindpunt naar keuze de de kopbalWaaier van HTTP van de typebytes. Als de header niet is ingesteld, wordt de gehele inhoud geretourneerd. U kunt meer lezen over het gebruik van de Range-header met Marketo [Bulk extraheren](bulk-extract.md).

## Een taak annuleren

Als een baan verkeerd werd gevormd, of onnodig wordt, kan het gemakkelijk worden geannuleerd gebruikend [Taak voor programmalid annuleren](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Bulk-Export-Program-Members/operation/cancelExportProgramMembersUsingPOST) eindpunt:

```
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
