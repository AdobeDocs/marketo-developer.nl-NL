---
title: Lijsten met benoemde accounts
feature: REST API
description: Leer hoe u lijsten met benoemde accounts van Marketo beheert met de REST API, inclusief machtigingen, velden, filters en eindpunten voor het zoeken, maken, bijwerken en verwijderen van accounts.
exl-id: 98f42780-8329-42fb-9cd8-58e5dbea3809
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 0%

---

# Lijsten met benoemde accounts

[&#x200B; Genoemde Verwijzing van het Eindpunt van de Rekening maakt &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Account-Lists)

[&#x200B; Genoemde Lijsten van de Rekening &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/target-account-management/target/account-lists) in Marketo vertegenwoordigen inzamelingen van genoemde rekeningen. Ze kunnen worden gebruikt voor een groot aantal verschillende gevallen, zoals categorisering, gegevensverrijking en filters voor slimme campagnes. Met de API&#39;s van de lijst met benoemde accounts kunt u deze lijstitems op afstand beheren en het lidmaatschap van deze systemen.
`Content`

## Machtigingen

Als u een query wilt uitvoeren op lijsten met benoemde accounts, is de machtiging Alleen-lezen lijst met benoemde accounts of de machtiging Lijst met benoemde accounts lezen/schrijven vereist. Voor het maken, bijwerken of verwijderen van lijsten is de machtiging Lijst met benoemde accounts lezen/schrijven vereist. Voor het aanvragen van een lidmaatschap van een lijst is de machtiging Benoemd account (alleen-lezen) of Benoemde account voor lezen/schrijven vereist, terwijl voor het beheren van het lidmaatschap de machtiging Benoemde account voor lezen/schrijven is vereist.

## Model

Lijsten met benoemde accounts hebben een beperkt aantal standaardvelden en kunnen niet worden uitgebreid met aangepaste velden.
`Named Account List Field`

| Naam | Gegevenstype | Bijwerkbaar | Notities |
|---|---|---|---|
| marketoGUID | String | Onwaar | Unieke tekenreeks-id van de benoemde accountlijst. Dit veld wordt beheerd door het systeem en is niet toegestaan als veld bij het maken van een record. Veld dat wordt gebruikt door &quot;dedupeBy&quot;:&quot;idField&quot; bij het maken of bijwerken van een bestand. |
| name | String | Waar | Naam van de lijst. Veld dat wordt gebruikt door &quot;dedupeBy&quot;:&quot;dedupeFields&quot; bij het maken of bijwerken van een bestand. |
| createdAt | Datumtijd | Onwaar | De datum waarop de lijst wordt gemaakt. Dit veld wordt beheerd door het systeem en is niet toegestaan als veld bij het maken of bijwerken van een record. |
| updatedAt | Datumtijd | Onwaar | Datumtijd van de meest recente update van de lijst. Dit veld wordt beheerd door het systeem en is niet toegestaan als veld bij het maken of bijwerken van een record. |
| type | String | Onwaar | Type lijst. Kan de waarde &quot;default&quot; of &quot;external&quot; hebben. De externe lijsten zijn die gecreeerd door de Mening van de Rekening van CRM. |

## Query

Het opvragen van accountlijsten is eenvoudig en eenvoudig. Er zijn momenteel slechts twee geldige filterTypes voor het opvragen van benoemde accountlijsten: &quot;dedupeFields&quot; en &quot;idField&quot;. Het veld waarop moet worden gefilterd, wordt ingesteld in de parameter `filterType` van de query en de waarden worden ingesteld in `filterValues as` een lijst met komma&#39;s als scheidingsteken. De filters `nextPageToken` en `batchSize` zijn ook optionele parameters.

```
GET /rest/v1/namedAccountLists.json?filterType=idField&filterValues=dff23271-f996-47d7-984f-f2676861b5fb,dff23271-f996-47d7-984f-f2676861b5fc
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "seq": 0,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
         "name": "Saas List",
         "createdAt": "xxxxxxxx",
         "updatedAt": "xxxxxxxx",
         "type": "default",
         "updateable": true
      },
      {
         "seq": 1,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fc",
         "name": "My Account List",
         "createdAt": "xxxxxxxx",
         "updatedAt": "xxxxxxxx",
         "type": "default",
         "updateable": true
      }
   ]
}
```

## Maken en bijwerken

Het creëren en het bijwerken van genoemde verslagen van de rekeningslijst volgt de gevestigde patronen voor andere het creëren en bijwerken van het Gegevensbestand van de Leiding verrichtingen. Houd er rekening mee dat lijsten met benoemde accounts maar één veld hebben dat kan worden bijgewerkt, `name` .

Het eindpunt laat de twee standaardactietypen toe: &quot;createOnly,&quot;en &quot;updateOnly.&quot;  De lus `action defaults` to &quot;createOnly&quot;.

De optionele `dedupeBy parameter` kan worden opgegeven als de handeling `updateOnly` is.  Toegestane waarden zijn &quot;dedupeFields&quot; (overeenkomend met &quot;name&quot;) of &quot;idField&quot; (overeenkomend met &quot;marketoGUID&quot;).  In de modi `createOnly` is alleen &quot;name&quot; toegestaan als het veld `dedupeBy` . U kunt maximaal 300 records tegelijk verzenden.

```
POST /rest/v1/namedAccountLists.json
```

```json
{
   "action": "createOnly",
   "dedupeBy": "dedupeFields",
   "input": [
      {
         "name": "SAAS List"
      },
      {
         "name": "Manufacturing (Domestic)"
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
         "seq": 0,
         "status": "created",
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
      },
      {
         "seq": 1,
         "status": "created",
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fc"
      }
   ]
}
```

## Verwijderen

Verwijderen van lijsten met benoemde accounts is eenvoudig en kan worden uitgevoerd op basis van de `name` of de `marketoGUID` in de lijst. Om de sleutel te selecteren u wenst om te gebruiken, ga of &quot;dedupeFields&quot;voor naam, of &quot;idField&quot;voor marketoGUID in het `deleteB` lid van uw verzoek over. Als deze waarde niet is ingesteld, wordt dedupeFields standaard ingesteld. U kunt maximaal 300 records tegelijk verwijderen.

```
POST /rest/v1/namedAccountLists/delete.json
```

```json
{
   "deleteBy": "dedupeFields",
   "input": [
      {
         "name": "Saas List"
      },
      {
         "name": "B2C List"
      },
      {
         "name": "Launchpoint Partner List"
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
         "seq": 0,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
         "status": "deleted"
      },
      {
         "seq": 1,
         "id": "dff23271-f996-47d7-984f-f2676861b5fc",
         "status": "deleted"
      },
      {
         "seq": 2,
         "status": "skipped",
         "reasons": [
            {
               "code": "1013",
               "message": "Record not found"
            }
         ]
      }
   ]
}
```

In het geval dat een verslag niet voor een bepaalde sleutel kan worden gevonden, zal het overeenkomstige resultaatpunt a `status` van &quot;overgeslagen&quot;en een reden met een code en een bericht hebben beschrijvend de mislukking, zoals aangetoond in het bovengenoemde voorbeeld.

## Lidmaatschap beheren

### Vraag-lidmaatschap

Het vragen van het lidmaatschap van een genoemde rekeningslijst is eenvoudig, die slechts `i` van de rekeningslijst vereist. Optionele parameters zijn:

-`field` - een door komma&#39;s gescheiden lijst met velden die moeten worden opgenomen in de reactierecords
- `nextPageToke` - voor het pagineren door de resultaatreeks
- `batchSiz` - voor het specificeren van het aantal records dat moet worden geretourneerd

Als `field` unset is, dan `marketoGUI`, `nam`, `createdA`, en `updatedA` zal zijn teruggekeerd. `batchSiz` heeft een maximum- en standaardwaarde van 300.

```
GET /rest/v1/namedAccountList/{id}/namedAccounts.json
```

```json
{
   "requestId": "e42b#14272d07d78",
   "success": true,
   "result": [
      {
         "seq": 0,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
         "name": "Saas List",
         "createdAt": "2017-02-01T00:00:00Z",
         "updatedAt": "2017-03-05T17:21:15Z"
      },
      {
         "seq": 1,
         "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fc",
         "name": "My Account List",
         "createdAt": "2017-02-01T00:00:00Z",
         "updatedAt": "2017-03-05T17:21:15Z"
      }
   ]
}
```

### Leden toevoegen

Benoemde accounts kunnen eenvoudig worden toegevoegd aan een lijst met benoemde accounts. De rekeningen kunnen slechts worden toegevoegd gebruikend hun marketoGUID. U kunt maximaal 300 records tegelijk toevoegen.

```
POST /rest/v1/namedAccountList/{id}/namedAccounts.json
```

```json
{
    "input": [
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        },
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        }
    ]
}
```

```json
{
    "requestId": "string",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        },
        {
            "seq": 1,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        }
    ],
    "success": true,
}
```

### Leden verwijderen

Het verwijderen van verslagen uit een rekeningslijst heeft een verschillend weg, maar de zelfde interface, die a `marketoGUI` voor elk verslag vereist dat u wilt schrappen. U kunt maximaal 300 records tegelijk verwijderen.

```
POST /rest/v1/namedAccountList/{id}/namedAccounts/remove.json
```

```json
{
    "input": [
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        },
        {
             "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb"
        }
    ]
}
```

```json
{
    "requestId": "string",
    "result": [
        {
            "seq": 0,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        },
        {
            "seq": 1,
            "marketoGUID": "dff23271-f996-47d7-984f-f2676861b5fb",
            "status": "added"
        }
    ],
    "success": true
}
```

## Tijdstippen

- De eindpunten van de lijst met benoemde accounts hebben een time-out van 30 seconden, tenzij hieronder vermeld
   - Lijsten met benoemde accounts synchroniseren: 60 s
   - Lijsten met benoemde accounts verwijderen: 60 s
   - Lijsten met benoemde accounts ophalen: 60 s
   - Benoemde accountlijstleden toevoegen: 60 s
   - Benoemde accountlijstleden verwijderen: 60 s
   - Benoemde accountlijstleden ophalen: 60 s
