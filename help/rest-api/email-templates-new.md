---
title: E-mailsjablonen
feature: REST API
description: Met de Marketo Asset REST API kunt u afhankelijkheden voor e-mailsjablonen zoeken, maken, bijwerken, klonen, verwijderen, goedkeuren en inspecteren.
exl-id: 50bb0047-d6ea-4c94-a900-18c37b17a147
source-git-commit: 59684e1c5a8082ad12f1e4bfc854c0d2dde35d2a
workflow-type: tm+mt
source-wordcount: '292'
ht-degree: 0%

---

# E-mailsjablonen

[Referentie voor eindpunt e-mailsjabloon](https://developer.adobe.com/marketo-apis/api/asset#tag/Email-Templates)

Met e-mailsjablonen definieert u de structuur en de herbruikbare indeling die worden gebruikt bij het maken van e-mails.

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

U kunt de metagegevens van een e-mailsjabloon ophalen met behulp van de id van het element of met behulp van het filtereindpunt.

### Op ID

#### Verzoek

```http
GET /rest/asset/v2/emailtemplate/{id}
```

#### Antwoord

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "14f9e#1900ab14001",
  "result": [
    {
      "id": "19",
      "name": "Base Newsletter Template",
      "description": "Core responsive template",
      "status": "draft"
    }
  ]
}
```

### Filter

Het filtereindpunt steunt het zoeken binnen een werkruimte en het versmallen van resultaten met extra vraagparameters. `workspaceId` is vereist.

Tot de ondersteunde filters behoren `folderId`, repeat `folderIds` , repeat `status` , `pageIndex`, `pageSize` , `createdBy`, `createdAtStart` , `createdAtEnd`, `modifiedBy`, `modifiedAtStart`, `modifiedAtEnd`, `name`, `sortKey`, `sortOrder`, `isCreatedByMe`, `isModifiedByMe`, `scriptEngine`, `isValueNonNullable` en `includeArchived`.

#### Verzoek

```http
GET /rest/asset/v2/emailtemplate/filter?workspaceId=1001&name=Newsletter&pageIndex=0&pageSize=20
```

#### Antwoord

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "33c4#1900ab1402f",
  "result": [
    {
      "id": "19",
      "name": "Base Newsletter Template",
      "status": "draft"
    }
  ]
}
```

Gebruik de parameter `name` wanneer u een sjabloon op naam moet zoeken.

## Maken

Maak een e-mailsjabloon door een JSON-payload te verzenden. `name` en `appData` zijn vereist. `appData` moet minstens `folderId` of `workspaceId` bevatten.

### Verzoek

```http
POST /rest/asset/v2/emailtemplate
Content-Type: application/json
```

```json
{
  "name": "Base Newsletter Template",
  "description": "Core responsive template",
  "appData": {
    "workspaceId": "1001",
    "folderId": "15",
    "editorType": "emailTemplate"
  },
  "themeId": "42"
}
```

### Antwoord

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "a99f#1900ab1407e",
  "result": [
    {
      "id": "1022",
      "name": "Base Newsletter Template",
      "status": "draft"
    }
  ]
}
```

De aanvraaghoofdtekst kan ook `data`, `editorContext`, `appType` en `status` bevatten.

### Velden maken

`appData` geeft aan waar de sjabloon is opgeslagen en hoe deze wordt bewerkt.

`themeId` kan worden gebruikt om een thema aan het malplaatje te associëren wanneer toepasselijk.

## Bijwerken

Een sjabloon bijwerken op basis van de id van het element.

### Verzoek

```http
POST /rest/asset/v2/emailtemplate/{id}/update
Content-Type: application/json
```

```json
{
  "name": "Base Newsletter Template v2",
  "description": "Updated responsive template",
  "appData": {
    "folderId": "15"
  }
}
```

### Antwoord

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "cf10#1900ab140b3",
  "result": [
    {
      "id": "1022"
    }
  ]
}
```

## Status beheren

E-mailsjablonen gebruiken een concept en een goedgekeurde levenscyclus. Gebruik het eindpunt van de staatsovergang om een malplaatje goed te keuren, het niet goed te keuren, een ontwerp te verwerpen, of een nieuw ontwerp te creëren.

Geldige `action` waarden zijn:

- `approve`
- `unapprove`
- `discard`
- `create_draft`

### Verzoek

```http
POST /rest/asset/v2/emailtemplate/state/transition
Content-Type: application/json
```

### Antwoord

```json
{
  "contentId": "1022",
  "action": "approve"
}
```

## Klonen

Gebruik het klooneindpunt om een kopie van een bestaande sjabloon te maken.

### Verzoek

```http
POST /rest/asset/v2/emailtemplate/clone
Content-Type: application/json
```

### Antwoord

```json
{
  "assetId": "1022",
  "newAsset": {
    "name": "Base Newsletter Template Copy",
    "description": "Cloned template"
  }
}
```

## Verwijderen

Een sjabloon verwijderen op basis van de id van het element.

### Verzoek

```http
POST /rest/asset/v2/emailtemplate/{id}/delete
Content-Type: application/json
```

Dit eindpunt neemt malplaatjeidentiteitskaart in de weg en bepaalt geen verzoeklichaam.

## Gebruikt door

Gebruik het eindpunt `usedby` om elementen op te halen die naar een bepaalde sjabloon verwijzen.

### Verzoek

```http
POST /rest/asset/v2/emailtemplate/usedby
Content-Type: application/json
```

### Antwoord

```json
{
  "assetId": "1022",
  "pageIndex": 0,
  "pageSize": 20,
  "type": "all"
}
```

## Notities

De eindpunten van de vraag keren meta-gegevens voor de activa terug. Gebruik de eindpuntverwijzing voor het volledige reactieschema en om het even welke milieu-specifieke eigenschappen.
