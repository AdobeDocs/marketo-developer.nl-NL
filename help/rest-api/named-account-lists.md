---
title: "Lijsten met benoemde accounts"
feature: REST API
description: "Configureer benoemde accountlijsten."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '696'
ht-degree: 0%

---


# Lijsten met benoemde accounts

[Verwijzing naar eindpunt voor benoemde accountlijsten](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Named-Account-Lists)

[Lijsten met benoemde accounts](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/target-account-management/target/account-lists) in Marketo staan voor verzamelingen benoemde accounts. Ze kunnen worden gebruikt voor een groot aantal verschillende gevallen, zoals categorisering, gegevensverrijking en filters voor slimme campagnes. Met de API&#39;s van de lijst met benoemde accounts kunt u deze lijstitems op afstand beheren en het lidmaatschap van deze systemen.
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

Het opvragen van accountlijsten is eenvoudig en eenvoudig. Er zijn momenteel slechts twee geldige filterTypes voor het opvragen van benoemde accountlijsten: &quot;dedupeFields&quot; en &quot;idField&quot;. Het veld waarop moet worden gefilterd, wordt ingesteld in het dialoogvenster `filterType` parameter van de query en de waarden worden ingesteld in `filterValues as` een door komma&#39;s gescheiden lijst. De `nextPageToken` en `batchSize` filters zijn ook optionele parameters.

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

Het creëren en het bijwerken van genoemde verslagen van de rekeningslijst volgt de gevestigde patronen voor andere het creëren en bijwerken van het Gegevensbestand van de Leiding verrichtingen. Houd er rekening mee dat lijsten met benoemde accounts maar één veld hebben dat kan worden bijgewerkt. `name`.

Het eindpunt laat de twee standaardactietypen toe: &quot;createOnly,&quot;en &quot;updateOnly.&quot;  De `action defaults` naar &quot;createOnly&quot;.

De optionele `dedupeBy parameter` kan worden opgegeven als de handeling is `updateOnly`.  Toegestane waarden zijn &quot;dedupeFields&quot; (overeenkomend met &quot;name&quot;) of &quot;idField&quot; (overeenkomend met &quot;marketoGUID&quot;).  In `createOnly` modi, alleen &quot;name&quot; is toegestaan als de `dedupeBy` veld. U kunt maximaal 300 records tegelijk verzenden.

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

Verwijderen van lijsten met benoemde accounts is eenvoudig en kan worden uitgevoerd op basis van de `name`of de `marketoGUID` van de lijst. Om de sleutel te selecteren u wenst te gebruiken, ga of &quot;dedupeFields&quot;voor naam, of &quot;idField&quot;voor marketoGUID in`deleteB` lid van uw verzoek. Als deze waarde niet is ingesteld, wordt dedupeFields standaard ingesteld. U kunt maximaal 300 records tegelijk verwijderen.

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

Als een record niet kan worden gevonden voor een bepaalde sleutel, heeft het bijbehorende resultaatitem een`status` van &quot;overgeslagen&quot;en een reden met een code en een bericht beschrijvend de mislukking, zoals aangetoond in het bovengenoemde voorbeeld.

## Lidmaatschap beheren

### Vraag-lidmaatschap

Het opvragen van het lidmaatschap van een lijst met benoemde accounts is eenvoudig. Alleen de`i` van de rekeninglijst. Optionele parameters zijn:

-`field` - een door komma&#39;s gescheiden lijst met velden die moeten worden opgenomen in de reactierecords -`nextPageToke` - voor het doorbladeren van de resultatenset -`batchSiz` - voor het opgeven van het aantal records dat moet worden geretourneerd

Indien`field` is niet ingesteld, dan`marketoGUI`,`nam`, `createdA`, en`updatedA` wordt geretourneerd. `batchSiz` heeft een maximum- en standaardwaarde van 300.

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

Het verwijderen van records uit een accountlijst heeft een ander pad, maar heeft dezelfde interface, waarvoor een`marketoGUI` voor elke record die u wilt verwijderen. U kunt maximaal 300 records tegelijk verwijderen.

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
