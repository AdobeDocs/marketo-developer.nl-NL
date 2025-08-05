---
title: Activiteiten
feature: REST API
description: Een API voor het beheer van Marketo Engage-activiteiten.
exl-id: 1e69af23-2b0c-467a-897c-1dcf81343e73
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '2029'
ht-degree: 0%

---

# Activiteiten

Marketo staat een groot aantal verschillende soorten activiteiten toe die verband houden met loodrecords.  Bijna elke verandering, actie of stroomstap wordt geregistreerd tegen het activiteitenlogboek van een lood en kan via API worden teruggewonnen of leveraged in Slimme Lijst en Slimme filters en trekkers van de Campagne.  Activiteiten zijn altijd gerelateerd aan de hoofdrecord via de leadId, overeenkomend met het Id-veld van de record, en hebben ook een eigen unieke id.

Er zijn een zeer groot aantal potentiële activiteitstypen, die van abonnement aan abonnement kunnen variëren, en unieke definities voor elk hebben. Elke activiteit heeft een eigen unieke eigenschap `id` , `leadId` en `activityDate` , maar de waarden `primaryAttributeValueId` en `primaryAttributeValue` hebben verschillende betekenissen.

Marketo staat ook het maken van aangepaste activiteitstypen toe via de metagegevens voor aangepaste activiteiten. Aangepaste activiteiten toevoegen vindt plaats via de API Aangepaste activiteiten toevoegen.

De meeste activiteiten zullen na enige tijd worden afgezuiverd.

## Beschrijven

Om een lijst van beschikbare types en hun definities voor een instantie terug te winnen, kunt u [ gebruiken krijgt de Types van Activiteit ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET) eindpunt.

```
GET /rest/v1/activities/types.json
```

```json
  "requestId": "6e78#148ad3b76f1",
  "success": true,
  "result": [
    {
      "id": 2,
      "name": "Fill Out Form",
      "description": "User fills out and submits form on web page",
      "primaryAttribute": {
        "name": "Webform ID",
        "dataType": "integer"
      },
      "attributes": [
        {
          "name": "Client IP Address",
          "dataType": "string"
        },
        {
          "name": "Form Fields",
          "dataType": "text"
        },
        {
          "name": "Query Parameters",
          "dataType": "string"
        },
        {
          "name": "Referrer URL",
          "dataType": "string"
        },
        {
          "name": "User Agent",
          "dataType": "string"
        },
        {
          "name": "Webpage ID",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

Reële reacties omvatten veel meer definities. In dit voorbeeld is het weergegeven type een &quot;Formulier invullen&quot;, dat een primair kenmerk van &quot;Webform ID&quot; heeft. Dit kenmerk verwijst naar de Marketo-id van het formulier dat is ingevuld en kan worden gebruikt om terug te verwijzen naar dat specifieke element in Marketo. Bovendien zijn er definities voor elk van de mogelijke attributen van een bepaalde activiteitenverslag van dit type en hun gegevenstypes. Als het veld leeg is, wordt dat specifieke kenmerk weggelaten uit een afzonderlijke activiteitenrecord.

## Query

Om activiteiten van Marketo terug te winnen, roep [ krijgen de Activiteiten van het Lood ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET) eindpunt. U moet eerst een het pagineren teken voor datetime terugwinnen die u wilt beginnen activiteiten van terug te winnen. Vervolgens geeft u het paginatietoken door in de queryparameter `nextPageToken` . Bovendien geeft u maximaal tien ID&#39;s van het type activiteit in de `activityTypeIds` -queryparameter door als een lijst met komma&#39;s als scheidingsteken.

U kunt of een listId vraagparameter naar keuze omvatten om uw onderzoek tot slechts die verslagen te beperken inbegrepen in een specifieke statische lijst, of een leadIds vraagparameter en onderzoek naar activiteiten van slechts een gespecificeerde reeks lood. U kunt maximaal 30 leadIds doorgeven als een door komma&#39;s gescheiden lijst.

```
GET /rest/v1/activities.json?activityTypeIds=1&nextPageToken=WQV2VQVPPCKHC6AQYVK7JDSA3I3LCWXH3Y6IIZ7YSGQLXHCPVE5Q====
```

```json
{
  "requestId": "24fd#15188a88d7f",
  "result": [
    {
      "id": 102988,
      "marketoGUID": "102988",
      "leadId": 1,
      "activityDate": "2023-01-16T23:32:19Z",
      "activityTypeId": 1,
      "primaryAttributeValueId": 71,
      "primaryAttributeValue": "localhost/munchkintest2.html",
      "attributes": [
        {
          "name": "Client IP Address",
          "value": "10.0.19.252"
        },
        {
          "name": "Query Parameters",
          "value": ""
        },
        {
          "name": "Referrer URL",
          "value": ""
        },
        {
          "name": "User Agent",
          "value": "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36"
        },
        {
          "name": "Webpage URL",
          "value": "/munchkintest2.html"
        }
      ]
    }
  ],
  "success": true,
  "nextPageToken": "WQV2VQVPPCKHC6AQYVK7JDSA3J62DUSJ3EXJGDPTKPEBFW3SAVUA====",
  "moreResult": false
}
```

Gebruik voor de eerste aanroep de API Pagingtoken ophalen om `nextPageToken` op te halen. Voor verdere vraag aan dit eindpunt, gebruik `nextPageToken returned` van de reactie. Dit eindpunt retourneert altijd `the nextPageToken` .

Als het kenmerk `moreResult` true is, zijn er meer resultaten beschikbaar. Ga door met het aanroepen van dit eindpunt totdat het kenmerk `moreResult` false retourneert, wat betekent dat er geen resultaten beschikbaar zijn. De `nextPageToken` die door deze API wordt geretourneerd, moet altijd opnieuw worden gebruikt voor de volgende herhaling van deze aanroep.

In sommige gevallen reageert deze API mogelijk met minder dan 300 activity-items, maar het kenmerk `moreResult` is ook ingesteld op true.  Dit wijst erop dat er meer activiteiten zijn die kunnen worden teruggekeerd en dat het eindpunt voor recentere activiteiten kan worden gevraagd door teruggekeerde `nextPageToken` in een verdere vraag te omvatten.

Let op: binnen elk resultaat array-item wordt het kenmerk integer van `id` vervangen door het `marketoGUID` string-kenmerk als unieke id.

### Wijzigingen in gegevenswaarde

Voor de activiteiten van de Verandering van de Waarde van Gegevens, wordt een gespecialiseerde versie van de activiteiten API verstrekt. Het [ krijgt de Veranderingen van het Lood ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadChangesUsingGET) eindpunt keert slechts activiteiten van de verslagen van de Verandering van de Waarde van Gegevens aan loodgebieden terug. De interface is hetzelfde als de API voor lead-activiteiten ophalen, met twee verschillen:

* Er is geen `activityTypeIds` parameter, aangezien het eindpunt slechts de Verandering van de Waarde van Gegevens en Nieuwe Loodactiviteiten terugkeert.
* De query-parameter `fields` is vereist, waarbij u een lijst met velden met komma&#39;s kunt doorgeven om aan te geven voor welke velden u wijzigingen wilt ophalen.

```
GET /rest/v1/activities/leadchanges.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ&fields=firstName,lastName,department
```

```json
{
  "requestId": "a9ae#148add1e53d",
  "success": true,
  "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBRGA3TQ===",
  "moreResult": true,
  "result": [
    {
      "id": 1078,
      "marketoGUID": "1078",
      "leadId": 775,
      "activityDate": "2014-09-17T22:31:49+0000",
      "activityTypeId": 13,
      "fields": [
        {
          "id": 48,
          "name": "firstName",
          "newValue": "FirstName_6176",
          "oldValue": "FirstName_4914"
        }
      ],
      "attributes": [
        {
          "name": "Reason",
          "value": "Web service API"
        },
        {
          "name": "Source",
          "value": "Web service API"
        },
        {
          "name": "Lead ID",
          "value": 775
        }
      ]
    }
  ]
}
```

Elke activiteit in de reactie heeft een veldarray, met een lijst met wijzigingen in de activiteit, die de `id` en `name` van het gewijzigde veld en de nieuwe en oude waarden ten opzichte van de wijziging opgeeft.

Let op: binnen elk resultaat array-item wordt het kenmerk integer van `id` vervangen door het `marketoGUID` string-kenmerk als unieke id.

### Verwijderde leads

Er is ook een speciaal eindpunt [ krijgen Geschrapte Leads ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getDeletedLeadsUsingGET) voor het terugwinnen van geschrapte activiteiten van Marketo.

```
GET /rest/v1/activities/deletedleads.json?nextPageToken=GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBQ
```

```json
{
  "requestId": "a9ae#148add1e53d",
  "success": true,
  "nextPageToken": "GIYDAOBNGEYS2MBWKQYDAORQGA5DAMBOGAYDAKZQGAYDALBRGA3TQ===",
  "moreResult": true,
  "result": [
    {
      "id": 2,
      "marketoGUID": "2",
      "leadId": 6,
      "activityDate": "2013-09-26T06:56:35+0000",
      "activityTypeId": 37,
      "primaryAttributeValueId": 6,
      "primaryAttributeValue": "Owyliphys Iledil",
      "attributes": []
    },
    {
      "id": 3,
      "marketoGUID": "3",
      "leadId": 9,
      "activityDate": "2013-12-28T00:39:45+0000",
      "activityTypeId": 37,
      "primaryAttributeValueId": 4,
      "primaryAttributeValue": "First Last",
      "attributes": []
    }
  ]
}
```

Let op: binnen elk resultaat array-item wordt het kenmerk integer van `id` vervangen door het `marketoGUID` string-kenmerk als unieke id.

### Resultaten doorbladeren

Door gebrek, de eindpunten die in deze sectie worden vermeld terugkeren 300 activiteitenpunten tegelijkertijd.  Als het kenmerk `moreResult` true is, zijn meer resultaten beschikbaar. Roep het eindpunt aan tot het `moreResult` attribuut vals terugkeert, wat betekent dat er geen meer resultaten beschikbaar zijn. De `nextPageToken` die van dit eindpunt is teruggekeerd zou altijd voor de volgende herhaling van deze vraag moeten worden opnieuw gebruikt.

In sommige gevallen, kan dit eindpunt met minder dan 300 activiteitenpunten antwoorden, maar ook heeft de `moreResult` attributen die aan waar worden geplaatst.  Dit wijst erop dat er extra activiteiten zijn die kunnen worden teruggekeerd en dat het eindpunt voor recentere activiteiten kan worden gevraagd door teruggekeerde `nextPageToken` in een verdere vraag te omvatten. De `nextPageToken` moet een URL zijn die in de aanvraag is gecodeerd.

## Aangepaste activiteitstypen

De Activiteiten van de douane functioneren enkel zoals standaardactiviteiten, behalve het schema wordt beheerd door derden, en niet door Marketo. Instanties van aangepaste activiteiten zijn via de `leadId` gekoppeld aan hoofdrecords, net als standaardactiviteiten, maar zowel primaire als secundaire kenmerken zijn willekeurig gedefinieerd. Wanneer een type van douaneactiviteit wordt goedgekeurd, wordt een overeenkomstige Slimme trekker en een filter van de Lijst gecreeerd, zodat de lood op huidige of historische gegevens van de douaneactiviteit kunnen worden verwerkt.

* Maximumaantal aangepaste activiteiten: 10
* Maximum aantal kenmerken per aangepaste activiteit: 20

Het terugwinnen van de gegevens van de douaneactiviteit wordt gedaan op de zelfde manier als standaardactiviteiten, door [ krijgen de Activiteiten van de Lood ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getLeadActivitiesUsingGET) API.

## Zoektypen

Naast standaard krijgt het Types van Activiteit eindpunt, [ krijgt de Types van Activiteit van de Douane ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getCustomActivityTypeUsingGET) en [ beschrijft de eindpunten van het Type van Activiteit van de Douane ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/describeCustomActivityTypeUsingGET) details over de activiteitstypes die in de instantie van Marketo worden voorzien, en meta-gegevens betreffende de attributen voor een bepaald type. De normale [ krijgt de Types van Activiteit ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/getAllActivityTypesUsingGET) keert nog meta-gegevens betreffende douaneactiviteiten terug, maar wijst niet erop of een bepaald type douane is.

### Typen ophalen

```
GET /rest/v1/activities/external/types.json
```

```json
{
  "requestId": "185d6#14b51985ff0",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attends Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved"
    }
  ]
}
```

### Typen beschrijven

Voor tekstbeschrijvingen moet u `apiName` doorgeven als padparameter. Standaard krijgt u de goedgekeurde versie van de activiteit. U kunt optioneel de parameter `draft=true` doorgeven om de conceptversie van de activiteit op te halen.

```
GET /rest/v1/activities/external/type/{apiName}/describe.json
```

```json
{
  "requestId": "185d6#14b51985ff0",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attends Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      },
      "attributes": [
        {
          "apiName": "conferenceDate",
          "name": "Conference Date",
          "description": "Date of the conference",
          "dataType": "datetime"
        },
        {
          "apiName": "numberOfAttendees",
          "name": "Number of Attendees",
          "description": "Number of people attending conference",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

## Tekst maken

Voor elk type aangepaste activiteit zijn een weergavenaam, API-naam, triggernaam, filternaam en primair kenmerk vereist.

Om ervoor te zorgen dat uw typen consistent zijn met de Marketo-conventies en om botsingen te voorkomen, is het belangrijk om een aantal richtlijnen te volgen bij het maken van uw typen:

**Naam van de Vertoning:** de vertoningsnaam van het activiteitstype zou kort moeten beschrijven wat een activiteitenverslag, zoals &quot;verzendt E-mail&quot;, of &quot;de Waarde van Gegevens van de Verandering&quot;vertegenwoordigt. Deze namen moeten doorgaans de oneindige vorm hebben, namelijk &quot;Gebeurtenis bijwonen&quot;.  Bij weergavenamen worden alfanumerieke tekens, spaties en onderstrepingstekens geaccepteerd. Weergavenamen moeten ten minste één letter bevatten.

**API Naam:** de API naam wordt samengesteld uit alfanumerieke karakters (maximumlengte van 255). Als u een partner van LaunchPoint bent, zou u een representatieve namespace aan uw naam van het type van activiteit API moeten voorafgaan. Dit moet botsingen met klant-provisioned types vermijden.  De conventie is om alle kleine letters of camelCase te gebruiken om onderscheid te maken tussen andere tekstreeksen.

**Beschrijving:** voor activiteiten die niet duidelijk gedrag kunnen hebben zou een beschrijving van moeten omvatten wat het activiteitstype met betrekking tot de lood vertegenwoordigt.

**Naam van de Trekker:** Elk activiteitstype moet een unieke, mens-leesbare trekkernaam hebben. De namen van de trekkers zouden in de derde aanwezige spanningen moeten zijn, zoals &quot;bijwoont een Gebeurtenis&quot;. De partners van LaunchPoint zouden hun bedrijfsnaam in de activiteit, zoals &quot;bijwonen Webinar - Acme Bedrijf moeten omvatten.&quot;

**Naam van de Filter:**  Elk type activiteit moet een unieke, leesbare filternaam hebben. De namen van de filter zouden in de derde gespannen moeten zijn, zoals &quot;bijgewoond een Gebeurtenis&quot;. De partners van LaunchPoint zouden hun bedrijfsnaam in de activiteit moeten omvatten, die &quot;Bijgewoonde Webinar - Acme Bedrijf.&quot;is

**Primair Attribuut:** de primaire attributen van een douaneactiviteit zouden het meest significante gebied voor het activiteitstype moeten zijn. Voor een activiteit &quot;Bijgewoonde gebeurtenis&quot; zou dit bijvoorbeeld de naam van de gebeurtenis zijn. De primaire attributen zijn inbegrepen als parameters door gebrek in elke instantie van een trekker of filter voor dat type van activiteit, en de waarde wordt getoond in het activiteitenlogboek van een persoonverslag zonder boor-down in de activiteit te vereisen.

Wanneer een douaneactiviteit wordt gecreeerd, wordt het gecreeerd als ontwerp, en moet worden goedgekeurd alvorens het kan worden gebruikt om activiteitenverslagen van dat type toe te voegen. Alle updates worden impliciet toegepast op de ontwerpversie van het type. Als u de wijzigingen in de versie van het type wilt doorvoeren, moet het worden goedgekeurd. Wanneer een aangepast type activiteit wordt goedgekeurd en in gebruik is, mogen de bovenstaande velden niet worden gewijzigd.

Wanneer u een type maakt, is de parameter description optioneel, terwijl alle volgende parameters vereist zijn: `apiName`, `name`, `triggerName`, `filterName`, `primaryAttribute` .

```
POST /rest/v1/activities/external/type.json
```

```json
{
  "apiName": "attendConference",
  "name": "Attend Conference",
  "description": "Attend the conference",
  "triggerName": "Attends Conference",
  "filterName": "Attended Conference",
  "primaryAttribute": {
    "apiName": "conferenceName",
    "name": "Conference Name",
    "description": "Name of the conference"
  }
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attends Conference",
      "filterName": "Attended Conference",
      "status": "draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      }
    }
  ]
}
```

## Type update

Het bijwerken van een type lijkt erg op het bijwerken, behalve dat apiName de enige vereiste parameter is als padparameter.

```
POST /rest/v1/activities/external/type/{apiName}.json
```

```json
{
  "name": "Attend Conference",
  "description": "Attend the conference",
  "triggerName": "Attend Conference",
  "filterName": "Attended Conference",
  "primaryAttribute": {
    "apiName": "conferenceName",
    "name": "Conference Name",
    "description": "Name of the conference"
  }
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "status": "draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      }
    }
  ]
}
```

## Type goedkeuren

De types kunnen met het Goedkeuren Type van Activiteit van de Douane, het Ontwerp van het Type van Activiteit van de Douane, en het Type van Activiteit van de Douane van de Schrapping worden beheerd, enkel zoals standaardMarketo activa.

## Kenmerken van type aangepaste activiteit

Elk type van douaneactiviteit kan van 0-20 secundaire attributen hebben. Secundaire kenmerken kunnen elk geldig veldtype voor een Marketo-veld hebben. Ze worden toegevoegd, bijgewerkt en verwijderd apart van het bovenliggende type, maar kunnen worden bewerkt tijdens het gebruik van een type activiteit en vervolgens worden goedgekeurd. Wanneer de gebieden op een levend type worden uitgegeven, dan hebben alle activiteiten van dat type die na goedkeuring worden gecreeerd de nieuwe secundaire reeks attributen. Wijzigingen worden niet met terugwerkende kracht toegepast op bestaande activiteiten die dat type delen.

Wees voorzichtig met het verwijderen van kenmerken, omdat dit van invloed is op de beschikbaarheid ervan voor gebruik in de overeenkomende filters.

Bij updates in de lijst met secundaire kenmerken wordt de API-naam van elk kenmerk gebruikt als primaire sleutel. De API-naam voor een kenmerk kan niet worden gewijzigd, maar moet worden verwijderd en opnieuw worden toegevoegd met de gewenste API-naam.

Geldige gegevenstypen voor kenmerken zijn: string, boolean, integer, float, link, email, currency, date, date, date, phone, text.

Wanneer u het primaire kenmerk van een type activiteit wijzigt, moet een bestaand primair kenmerk eerst op false worden ingesteld. `isPrimary`

### Kenmerken maken

Voor het maken van een kenmerk wordt een vereiste padparameter `apiName` gebruikt. Ook de parameters `name` en `dataType` zijn vereist.`The description and` `isPrimary` -parameters zijn optioneel.

```
POST /rest/v1/activities/external/type/{apiName}/attributes/create.json
```

```json
{
  "attributes": [
    {
      "apiName": "conferenceDate",
      "name": "Conference Date",
      "description": "Date of the conference",
      "dataType": "datetime"
    },
    {
      "apiName": "numberOfAttendees",
      "name": "Number of Attendees",
      "description": "Number of people attending conference",
      "dataType": "integer"
    }
  ]
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved with draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      },
      "attributes": [
        {
          "apiName": "conferenceDate",
          "name": "Conference Date",
          "description": "Date of the conference",
          "dataType": "datetime"
        },
        {
          "apiName": "numberOfAttendees",
          "name": "Number of Attendees",
          "description": "Number of people attending conference",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

### Kenmerken bijwerken

Wanneer updates van kenmerken worden uitgevoerd, is de `apiName` van het kenmerk de primaire sleutel. De update werkt alleen als de parameter `apiName` bestaat (u kunt de parameter `apiName` dus niet wijzigen met update).

```
POST /rest/v1/activities/external/type/{apiName}/attributes/update.json
```

```json
{
  "attributes": [
    {
      "apiName": "conferenceDate",
      "name": "Conference Date",
      "description": "Date of the conference",
      "dataType": "datetime"
    },
    {
      "apiName": "numberOfAttendee",
      "name": "Number of Attendee",
      "description": "Number of people attending conference",
      "dataType": "integer"
    }
  ]
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved with draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      },
      "attributes": [
        {
          "apiName": "conferenceDate",
          "name": "Conference Date",
          "description": "Date of the conference",
          "dataType": "datetime"
        },
        {
          "apiName": "numberOfAttendee",
          "name": "Number of Attendee",
          "description": "Number of people attending conference",
          "dataType": "integer"
        }
      ]
    }
  ]
}
```

### Kenmerken verwijderen

Als u een kenmerk verwijdert, wordt een vereiste padparameter `apiName` gebruikt. Dit is de API-naam voor aangepaste activiteit.  Ook vereist is een kenmerkparameter die een array van kenmerkobjecten is.  Elk object moet een parameter `apiName` bevatten die de API-naam van het type aangepaste activiteit is.

```
POST /rest/v1/activities/external/type/{apiName}/attributes/delete.json
```

```json
{ "attributes":[ { "apiName":"conferenceDate" }, { "apiName":"numberOfAttendees" } ] }
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 100001,
      "apiName": "attendConference",
      "name": "Attend Conference",
      "description": "Attend the conference",
      "triggerName": "Attend Conference",
      "filterName": "Attended Conference",
      "createdAt": "2016-02-03T22:36:23Z",
      "updatedAt": "2016-02-03T22:36:23Z",
      "status": "approved with draft",
      "primaryAttribute": {
        "apiName": "conferenceName",
        "name": "Conference Name",
        "description": "Name of the conference",
        "dataType": "string"
      }
    }
  ]
}
```

## Aangepaste activiteiten toevoegen

Aangepaste activiteiten zijn een schriftelijke registratie van historische activiteiten met betrekking tot individuele persoonrecords in Marketo. Deze activiteiten hebben een schema dat door Marketo Admins of ver via een API integratie wordt beheerd. De activiteiten van de douane worden toegevoegd aan loodverslagen via [ voeg het eindpunt van de Activiteiten van de Douane ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Activities/operation/addCustomActivityUsingPOST) toe en verwant met elk loodverslag via zijn `leadId` gebied. De activiteiten van de douane kunnen in het gebruikersinterface via het de activiteitenlogboek van het lood worden bekeken, of via krijgen van het Punt van Activiteiten van de Leiding door het type identiteitskaart van de douaneactiviteit te specificeren.

Aangepaste activiteiten zijn geschikt voor het vastleggen van gegevens die betrekking hebben op één record en die niet hoeven te worden bijgewerkt of overschreven. Een voorbeeld zou een persoon die een gebeurtenis bijwoont als &quot;Bijgewoonde gebeurtenis&quot;activiteit registreren. Voor verslagen met betrekking tot een persoon die, zoals studenteninschrijving kan veranderen, zouden de douanevoorwerpen in plaats daarvan moeten worden gebruikt, aangezien zij kunnen worden bijgewerkt, waar de douaneactiviteiten niet kunnen.

Het invoerlid is een array van activiteitsobjecten. Er kunnen maximaal 300 activiteitenrecords tegelijk worden ingediend.

De leden `leadId`, `activityDate`, `activityTypeId`, `primaryAttributeValue` en attributes zijn vereist. De array attributes moet het niet-primaire kenmerk bevatten. Dit kan worden opgegeven met de naam (veldnaam) of apiName (API-naam) en de waarde die overeenkomt met de waarde die u instelt.

```
POST /rest/v1/activities/external.json
```

```json
{
  "input": [
    {
      "leadId": 1001,
      "activityDate": "2016-09-26T06:56:35+07:00",
      "activityTypeId": 1001,
      "primaryAttributeValue": "Game Giveaway",
      "attributes": [
        {
          "apiName": "uRL",
          "value": "http://www.nvidia.com/game-giveaway"
        }
      ]
    },
    {
      "leadId": 1200,
      "activityDate": "2016-09-26T06:56:35+07:00",
      "activityTypeId": 1001,
      "primaryAttributeValue": "Game Giveaway",
      "attributes": [
        {
          "apiName": "uRL",
          "value": "http://www.nvidia.com/game-giveaway"
        }
      ]
    },
    {
      "leadId": 3000,
      "activityDate": "2016-09-26T06:56:35+07:00",
      "activityTypeId": 1001,
      "primaryAttributeValue": "Contest Form",
      "attributes": [
        {
          "apiName": "uRL",
          "value": "http://www.nvidia.com/game-giveaway"
        }
      ]
    }
  ]
}
```

```json
{
  "requestId": "e42b#14272d07d78",
  "success": true,
  "result": [
    {
      "id": 50,
      "marketoGUID": "50",
      "status": "added"
    },
    {
      "id": 51,
      "marketoGUID": "51",
      "status": "added"
    },
    {
      "status": "skipped",
      "errors": [
        {
          "code": "1004",
          "message": "Lead not found"
        }
      ]
    }
  ]
}
```

## Tijdstippen

De eindpunten van activiteiten hebben een onderbreking van 30 s tenzij hieronder vermeld.

* Pagingtoken ophalen: 300 seconden
* Aangepaste activiteit toevoegen: 90 seconden
