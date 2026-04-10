---
title: Fragmenten
feature: REST API
description: Met de Marketo Asset REST API kunt u afhankelijkheden voor fragmenten zoeken, maken, bijwerken, klonen, verwijderen, goedkeuren en inspecteren.
exl-id: 9dd532d1-1dd7-4581-86dd-1943fab66cbb
source-git-commit: e2606d6cb12c572603ff069617de58417e43ca63
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 1%

---

# Fragmenten

Fragmenten zijn herbruikbare inhoudselementen waarnaar andere elementen kunnen verwijzen, zoals e-mails.

## Toegang

Voor de eindpunten die in dit artikel worden beschreven, is een toegangstoken vereist:

```text
?access_token=<access_token>
```

Voor aanvragen is ook de header `x-app-type` vereist:

```text
x-app-type: <app-type>
```

## Query

U kunt fragmentmetagegevens ophalen met de id van het element of met het filtereindpunt.

### Op ID

#### Verzoek

```http
GET /rest/asset/v2/fragment/{id}
```

#### Antwoord

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "fa0f#1900ab15001",
  "result": [
    {
      "id": "83",
      "name": "Hero Banner Fragment",
      "description": "Reusable hero block",
      "status": "approved"
    }
  ]
}
```

### Filter

Het filtereindpunt steunt het zoeken binnen een werkruimte en het versmallen van resultaten met extra vraagparameters. `workspaceId` is vereist.

todo: maak van dit een tabel
Tot de ondersteunde filters behoren `folderId`, repeat `folderIds`, repeat `status`, `pageIndex`, `pageSize`, `createdBy`, `createdAtStart`, `createdAtEnd`, `modifiedBy`, `modifiedAtStart`, `modifiedAtEnd`, `name`, `fragmentType`, `sortKey`, `sortOrder`, `isCreatedByMe`, `isModifiedByMe`, `scriptEngine`, `isValueNonNullable`, en `includeArchived` .

#### Verzoek

```http
GET /rest/asset/v2/fragment/filter?workspaceId=1001&fragmentType=email&pageIndex=0&pageSize=20
```

#### Antwoord

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "f9cc#1900ab1504a",
  "result": [
    {
      "id": "83",
      "name": "Hero Banner Fragment",
      "status": "approved"
    }
  ]
}
```

Gebruik de parameter `name` wanneer u een fragment op naam moet zoeken.

## Maken

Maak een fragment door een JSON-payload te verzenden. `name` , `appData` en `settings` zijn vereist. `settings` moet `fragmentType` en `supportedChannels` bevatten.

### Verzoek

```http
POST /rest/asset/v2/fragment
Content-Type: application/json
```

```json
{
  "name": "Hero Banner Fragment",
  "description": "Reusable hero block",
  "appData": {
    "workspaceId": "1001",
    "folderId": "395",
    "editorType": "fragment"
  },
  "settings": {
    "fragmentType": "email",
    "fragmentSubType": "html",
    "supportedChannels": [
      "email"
    ]
  }
}
```

### Antwoord

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "bd57#1900ab1509d",
  "result": [
    {
      "id": "13",
      "name": "Hero Banner Fragment",
      "status": "draft"
    }
  ]
}
```

De aanvraaghoofdtekst kan ook `data`, `editorContext`, `themeId`, `appType` en `status` bevatten.

### Velden maken

* `appData` geeft aan waar het fragment wordt opgeslagen en hoe het wordt bewerkt.
* `settings.fragmentType` geeft de fragmentcategorie aan, zoals een e-mailfragment.
* `settings.fragmentSubType` kan worden gebruikt om de fragmentindeling verder te definiëren.
* `settings.supportedChannels` geeft een lijst weer van de kanalen waar het fragment kan worden gebruikt.

## Bijwerken

Een fragment bijwerken op basis van element-id.

### Verzoek

```http
POST /rest/asset/v2/fragment/{id}/update
Content-Type: application/json
```

```json
{
  "name": "Hero Banner Fragment v2",
  "description": "Updated reusable hero block",
  "settings": {
    "fragmentSubType": "html",
    "supportedChannels": [
      "email"
    ]
  }
}
```

### Antwoord

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "73d9#1900ab150f0",
  "result": [
    {
      "id": "13"
    }
  ]
}
```

## Status beheren

Fragmenten gebruiken een concept en een goedgekeurde levenscyclus. Gebruik het eindpunt van de statusovergang om een fragment goed te keuren, het niet goed te keuren, een concept te verwijderen of een nieuw concept te maken.

Geldige `action` waarden zijn:

* `approve`
* `unapprove`
* `discard`
* `create_draft`

### Verzoek

```http
POST /rest/asset/v2/fragment/state/transition
Content-Type: application/json
```

### Antwoord

```json
{
  "contentId": "13",
  "action": "approve"
}
```

## Klonen

Gebruik het klooneindpunt om een kopie van een bestaand fragment te maken.

### Verzoek

```http
POST /rest/asset/v2/fragment/clone
Content-Type: application/json
```

### Antwoord

```json
{
  "assetId": "13",
  "newAsset": {
    "name": "Hero Banner Fragment Copy",
    "description": "Cloned fragment"
  }
}
```

## Verwijderen

Verwijder een fragment op middel van element-id.

### Verzoek

```http
POST /rest/asset/v2/fragment/{id}/delete
Content-Type: application/json
```

Dit eindpunt neemt fragmentidentiteitskaart in de weg en bepaalt geen verzoeklichaam.

## Gebruikt door

Gebruik het eindpunt `usedby` om elementen op te halen die naar een bepaald fragment verwijzen.

### Verzoek

```http
POST /rest/asset/v2/fragment/usedby
Content-Type: application/json
```

### Antwoord

```json
{
  "assetId": "13",
  "pageIndex": 0,
  "pageSize": 20,
  "type": "all"
}
```
