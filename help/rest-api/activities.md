---
title: Activiteiten
feature: REST API
description: Gebruik de Marketo Engage Activity REST API om activiteitstypen weer te geven, leadactiviteiten met paginerende tokens op te halen en aangepaste wijzigingen en gegevenswaarden af te handelen.
exl-id: 1e69af23-2b0c-467a-897c-1dcf81343e73
source-git-commit: 5260338681c4ea670f6f1b1a1603e30f6acc0865
workflow-type: tm+mt
source-wordcount: '2218'
ht-degree: 0%

---

# Activiteiten

Marketo staat een groot aantal verschillende soorten activiteiten toe die verband houden met loodrecords.  Bijna elke verandering, actie of stroomstap wordt geregistreerd tegen het activiteitenlogboek van een lood en kan via API worden teruggewonnen of leveraged in Slimme Lijst en Slimme filters en trekkers van de Campagne.  Activiteiten zijn altijd gerelateerd aan de hoofdrecord via de leadId, overeenkomend met het Id-veld van de record, en hebben ook een eigen unieke id.

Er zijn een zeer groot aantal potentiële activiteitstypen, die van abonnement aan abonnement kunnen variëren, en unieke definities voor elk hebben. Elke activiteit heeft een eigen unieke eigenschap `id` , `leadId` en `activityDate` , maar de waarden `primaryAttributeValueId` en `primaryAttributeValue` hebben verschillende betekenissen.

Marketo staat ook het maken van aangepaste activiteitstypen toe via de metagegevens voor aangepaste activiteiten. Aangepaste activiteiten toevoegen vindt plaats via de API Aangepaste activiteiten toevoegen.

De meeste activiteiten zullen na enige tijd worden afgezuiverd.

## Beschrijven

Om een lijst van beschikbare types en hun definities voor een instantie terug te winnen, kunt u [&#x200B; gebruiken krijgt de Types van Activiteit &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET) eindpunt.

```http
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

Om activiteiten van Marketo terug te winnen, roep [&#x200B; krijgen de Activiteiten van het Lood &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadActivitiesUsingGET) eindpunt. U moet eerst een het pagineren teken voor datetime terugwinnen die u wilt beginnen activiteiten van terug te winnen. Vervolgens geeft u het paginatietoken door in de queryparameter `nextPageToken` . Bovendien geeft u maximaal tien ID&#39;s van het type activiteit in de `activityTypeIds` -queryparameter door als een lijst met komma&#39;s als scheidingsteken.

U kunt optioneel een query-parameter `listId` opnemen om uw zoekopdracht te beperken tot records die zijn opgenomen in een specifieke statische lijst, of een query-parameter `leadIds` en alleen vanuit een opgegeven reeks leads naar activiteiten zoeken. U kunt maximaal 30 `leadIds` doorgeven als een door komma&#39;s gescheiden lijst.

>[!CAUTION]
>
>Vanaf 2026-12-30 mislukken aanroepen van de `Get Lead Activities` - en `Get Lead Changes` -eindpunten die de parameter `listId` bevatten (foutcode 1003) als de doellijsten 10.000 of meer leads bevatten. Om de dienstverstoringen te vermijden, zorg ervoor dat de vraag behoorlijk scoped is om deze grens te vermijden.

```http
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

Voor de activiteiten van de Verandering van de Waarde van Gegevens, wordt een gespecialiseerde versie van de activiteiten API verstrekt. Het [&#x200B; krijgt de Veranderingen van het Lood &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadChangesUsingGET) eindpunt keert slechts activiteiten van de verslagen van de Verandering van de Waarde van Gegevens aan loodgebieden terug. De interface is hetzelfde als de API voor lead-activiteiten ophalen, met twee verschillen:

* Er is geen `activityTypeIds` parameter, aangezien het eindpunt slechts de Verandering van de Waarde van Gegevens en Nieuwe Loodactiviteiten terugkeert.
* De query-parameter `fields` is vereist, waarbij u een lijst met velden met komma&#39;s kunt doorgeven om aan te geven voor welke velden u wijzigingen wilt ophalen.

>[!CAUTION]
>
>Vanaf 2026-12-30 mislukken aanroepen van de `Get Lead Activities` - en `Get Lead Changes` -eindpunten die de parameter `listId` bevatten (foutcode 1003) als de doellijsten 10.000 of meer leads bevatten. Om de dienstverstoringen te vermijden, zorg ervoor dat de vraag behoorlijk scoped is om deze grens te vermijden.

```http
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

Er is ook een speciaal eindpunt [&#x200B; krijgen Geschrapte Leads &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getDeletedLeadsUsingGET) voor het terugwinnen van geschrapte activiteiten van Marketo.

```http
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

Het terugwinnen van de gegevens van de douaneactiviteit wordt gedaan op de zelfde manier als standaardactiviteiten, door [&#x200B; krijgen de Activiteiten van de Lood &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getLeadActivitiesUsingGET) API.

## Zoektypen

Naast standaard krijgt het Types van Activiteit eindpunt, [&#x200B; krijgt de Types van Activiteit van de Douane &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getCustomActivityTypeUsingGET) en [&#x200B; beschrijft de eindpunten van het Type van Activiteit van de Douane &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/describeCustomActivityTypeUsingGET) details over de activiteitstypes die in de instantie van Marketo worden voorzien, en meta-gegevens betreffende de attributen voor een bepaald type. De normale [&#x200B; krijgt de Types van Activiteit &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/getAllActivityTypesUsingGET) keert nog meta-gegevens betreffende douaneactiviteiten terug, maar wijst niet erop of een bepaald type douane is.

### Get types

```http
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

### Describe types

For type descriptions you must pass `apiName` as a path parameter. By default you get the approved version of the activity. You can optionally pass the `draft=true` parameter to retrieve the draft version of the activity.

```http
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

## Create type

Each custom activity type requires a display name, API name, trigger name, filter name, and primary attribute.

To ensure consistency of your types with Marketo conventions, and to avoid collisions, it is important to follow a few guidelines when creating your types:

**Display Name:** The display name of the activity type should briefly describe what an activity record represents, such as &quot;Send Email&quot;, or &quot;Change Data Value&quot;. These names should typically be in the infinitive form, that is &quot;Attend Event&quot;.  Display names accept alphanumeric characters, spaces and underscores. Display names must contain at least one letter.

**API Name:** The API name is comprised of alphanumeric characters (maximum length of 255). If you are a LaunchPoint partner, you should prepend a representative namespace to your activity type API names. This is to avoid collisions with customer-provisioned types.  The convention is to use all lowercase or camelCase to help distinguish between other text strings.

**Description:** For activities that may have non-obvious behavior should include a description of what the activity type represents with relation to the lead.

**Trigger Name:** Each activity type must have a unique, human-readable trigger name. Trigger names should be in the third-person present tense, such as &quot;Attends an Event&quot;. LaunchPoint partners should include their company name in the activity, such as &quot;Attends Webinar – Acme Company.&quot;

**Filter Name:**  Each activity type must have a unique, human-readable filter name. Filter names should be in the third-person past tense, such as &quot;Attended an Event&quot;. LaunchPoint partners should include their company name in the activity, that is &quot;Attended Webinar – Acme Company.&quot;

**Primary Attribute:** The primary attribute of a custom activity should be the most significant field for the activity type. For example, for an &quot;Attended Event&quot; activity this would be the name of the event. De primaire attributen zijn inbegrepen als parameters door gebrek in elke instantie van een trekker of filter voor dat type van activiteit, en de waarde wordt getoond in het activiteitenlogboek van een persoonverslag zonder boor-down in de activiteit te vereisen.

Wanneer een douaneactiviteit wordt gecreeerd, wordt het gecreeerd als ontwerp, en moet worden goedgekeurd alvorens het kan worden gebruikt om activiteitenverslagen van dat type toe te voegen. Alle updates worden impliciet toegepast op de ontwerpversie van het type. Als u de wijzigingen in de versie van het type wilt doorvoeren, moet het worden goedgekeurd. Wanneer een aangepast type activiteit wordt goedgekeurd en in gebruik is, mogen de bovenstaande velden niet worden gewijzigd.

Wanneer u een type maakt, is de parameter description optioneel, terwijl alle volgende parameters vereist zijn: `apiName`, `name`, `triggerName`, `filterName`, `primaryAttribute` .

```http
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

```http
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

Voor het maken van een kenmerk wordt een vereiste padparameter `apiName` gebruikt. De parameters `name` en `dataType` zijn ook vereist. `The description and` `isPrimary` -parameters zijn optioneel.

```http
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

### Update attributes

When performing updates to attributes, the `apiName` of the attribute is the primary key. The `apiName` parameter must exist for the update to succeed (that is, you cannot change the `apiName` parameter using update).

```http
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

### Delete Attributes

Deleting an attribute takes a required `apiName` path parameter that is the custom activity API name.  Also required is an attribute parameter that is an array of attribute objects.  Each object must contain an `apiName` parameter that is the custom activity type API name.

```http
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

## Add Custom Activities

Custom activities are write-once records of historical activities related to individual person records in Marketo. These activities have a schema that is managed by Marketo Admins or remotely via an API integration. Custom activities are added to lead records via the [Add Custom Activities](https://developer.adobe.com/marketo-apis/api/mapi#tag/Activities/operation/addCustomActivityUsingPOST) endpoint and related to each lead record via its `leadId` field. Custom activities can be viewed in the user interface via the lead&#39;s activity log, or retrieved via Get Lead Activities endpoint by specifying the custom activity&#39;s type ID.

Custom activities are appropriate for recording data that is related to a single person record and which does not need to be updated or overwritten. An example would be recording a person attending an event as an &quot;Attended Event&quot; activity. For records related to a person that may change, such as student enrollment, custom objects should be used instead, as they can be updated, where custom activities may not.

The input member is an array of activity objects. A maximum of 300 activity records can be submitted at a time.

The `leadId`, `activityDate`, `activityTypeId`, `primaryAttributeValue`, and attributes members are required. The attributes array must contain the non-primary attribute. This can be specified using either name (field name), or apiName (API name), and value that corresponds to the value that you are setting.

```http
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

## Timeouts

Activities endpoints have a timeout of 30s unless noted below.

* Get Paging Token: 300s
* Add Custom Activity: 90s
