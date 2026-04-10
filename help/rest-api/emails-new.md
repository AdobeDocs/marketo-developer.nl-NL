---
title: E-mails
feature: REST API
description: Met de Marketo Asset REST API kunt u afhankelijkheden voor e-mailmiddelen zoeken, maken, bijwerken, klonen, verwijderen, goedkeuren en inspecteren.
exl-id: b41a3ae5-2b25-4103-84b4-320fc2c44bd6
source-git-commit: e2606d6cb12c572603ff069617de58417e43ca63
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 1%

---

# E-mails

[E-maileindpuntverwijzing](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails_New)

E-mails zijn elementrecords die de metagegevens van berichten, de configuratie van de inhoud, de instellingen en de goedkeuringsstatus definiëren.

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

U kunt e-mailmetagegevens ophalen via element `id` of met het filtereindpunt.

### Op ID

#### Verzoek

```http
GET /rest/asset/v2/email/{id}
```

#### Antwoord

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "7b3c#1900ab12cd3",
  "result": [
    {
      "id": "1017",
      "name": "Spring Launch Email",
      "description": "Main announcement email",
      "status": "draft"
    }
  ]
}
```

### Filter

Het filtereindpunt steunt het zoeken binnen een werkruimte en het versmallen van resultaten met extra vraagparameters.

`workspaceId` is vereist.

Ondersteunde queryparameters:

| Parameter | Beschrijving |
| - | - |
| `workspaceId` | De vereiste werkruimte-id die wordt gebruikt om het bereik van de filteraanvraag te bepalen. |
| `folderId` | Filterresultaten naar één map. |
| `folderIds` | Herhaalde parameter die naar meerdere mappen filtert. |
| `status` | Herhaalde parameter die door een of meer e-mailstatussen filtert. |
| `pageIndex` | Op nul gebaseerde pagina-index voor gepagineerde resultaten. |
| `pageSize` | Maximumaantal resultaten dat per pagina moet worden geretourneerd. |
| `createdBy` | Filterresultaten van de aanmaakgebruiker. |
| `createdAtStart` | Retourneert elementen die op of na deze tijdstempel zijn gemaakt. |
| `createdAtEnd` | Retourneert elementen die op of vóór deze tijdstempel zijn gemaakt. |
| `modifiedBy` | Filterresultaten van de laatste wijzigende gebruiker. |
| `modifiedAtStart` | Retourneert elementen die op of na deze tijdstempel zijn gewijzigd. |
| `modifiedAtEnd` | Retourneert elementen die op of voor deze tijdstempel zijn gewijzigd. |
| `name` | Filterresultaten op e-mailnaam. |
| `sortKey` | Hiermee selecteert u het veld waarmee de resultaten worden gesorteerd. |
| `sortOrder` | Hiermee stelt u de sorteerrichting in. |
| `isCreatedByMe` | Hiermee beperkt u de resultaten tot elementen die door de huidige gebruiker zijn gemaakt. |
| `isModifiedByMe` | Hiermee beperkt u de resultaten tot elementen die het laatst door de huidige gebruiker zijn gewijzigd. |
| `templateId` | Filterresultaten op sjabloon-id. |
| `scriptEngine` | Filterresultaten op basis van de configuratie van de scriptengine. |
| `isValueNonNullable` | Filters die erop zijn gebaseerd of de waarde niet-nullable is. |
| `includeArchived` | Hiermee neemt u gearchiveerde elementen op in de resultaten. |

#### Verzoek

```http
GET /rest/asset/v2/email/filter?workspaceId=1001&name=Spring%20Launch&status=draft&status=approved&pageIndex=0&pageSize=20
```

#### Antwoord

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "5dd1#1900ab13011",
  "result": [
    {
      "id": "1017",
      "name": "Spring Launch Email",
      "status": "draft"
    }
  ]
}
```

Gebruik de parameter `name` wanneer u een e-mailbericht op naam moet zoeken.

## Maken

Maak een e-mail door een JSON-payload te verzenden. `name` , `appData` en `headers` zijn vereist. `headers.subject` is vereist en `appData` moet ten minste een van `folderId` , `workspaceId` of `programId` bevatten.

### Verzoek

```http
POST /rest/asset/v2/email
Content-Type: application/json
```

```json
{
  "name": "Spring Launch Email",
  "description": "Main announcement email",
  "appData": {
    "workspaceId": "1001",
    "folderId": "2002",
    "editorType": "email"
  },
  "headers": {
    "subject": "Introducing the Spring Launch",
    "fromName": "Marketing Team",
    "fromEmail": "marketing@example.com",
    "replyEmail": "reply@example.com",
    "preheader": "See what changed this quarter",
    "ccEmails": [
      "owner@example.com"
    ]
  },
  "settings": {
    "isOperational": false,
    "isTextOnly": false,
    "isWebPageView": true,
    "enableUrlTracking": true
  },
  "templateId": "3003"
}
```

### Antwoord

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "238c#1900ab1313f",
  "result": [
    {
      "id": "1017",
      "name": "Spring Launch Email",
      "status": "draft"
    }
  ]
}
```

De aanvraaghoofdtekst kan ook `data`, `editorContext`, `themeId`, `appType` en `status` bevatten.

### Velden maken

* `appData` geeft aan waar het e-mailbericht wordt gemaakt en hoe het wordt bewerkt.
* `headers` bevat de waarden van de berichtkopbal, met inbegrip van onderwerp, afzender, antwoordadres, preheader, en facultatieve ontvangers CC.
* `settings` bestuurt de werking en het teruggeven gedrag zoals tekst-slechts wijze, Web-pagina mening, en URL het volgen.
* `templateId` identificeert de e-mailsjabloon die wordt gebruikt bij het maken van de e-mail.

## Bijwerken

Werk een e-mail bij op basis van de id van het element. De aanvraaghoofdtekst gebruikt het schema `UpdateEmailRequest` en alle eigenschappen zijn optioneel, zodat u alleen de velden kunt verzenden die u wilt wijzigen.

### Verzoek

```http
POST /rest/asset/v2/email/{id}/update
Content-Type: application/json
```

```json
{
  "description": "Updated announcement email",
  "headers": {
    "subject": "Spring Launch Is Live",
    "preheader": "Read the latest release notes"
  },
  "settings": {
    "enableUrlTracking": true,
    "isWebPageView": true
  }
}
```

### Antwoord

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "9fd3#1900ab13210",
  "result": [
    {
      "id": "1017"
    }
  ]
}
```

## Status beheren

E-mails gebruiken een concept en een goedgekeurde levenscyclus. Gebruik het eindpunt van de staatsovergang om een e-mail goed te keuren, het niet goed te keuren, een ontwerp te verwerpen, of een nieuw ontwerp te creëren.

Geldige `action` waarden zijn:

* `approve`
* `unapprove`
* `discard`
* `create_draft`

### Verzoek

```http
POST /rest/asset/v2/email/state/transition
Content-Type: application/json
```

### Antwoord

```json
{
  "contentId": "1017",
  "action": "approve"
}
```

## Klonen

Gebruik het klooneindpunt om een kopie van een bestaande e-mail te maken.

### Verzoek

```http
POST /rest/asset/v2/email/clone
Content-Type: application/json
```

### Antwoord

```json
{
  "assetId": "1017",
  "newAsset": {
    "name": "Spring Launch Email Copy",
    "description": "Cloned from Spring Launch Email"
  }
}
```

## Verwijderen

Een e-mailbericht verwijderen op basis van de id van het element.

### Verzoek

```http
POST /rest/asset/v2/email/{id}/delete
Content-Type: application/json
```

Dit eindpunt neemt de activa `id` in de weg en bepaalt geen verzoeklichaam.

## Gebruikt door

Gebruik het eindpunt `usedby` om elementen op te halen die naar een bepaalde e-mail verwijzen.

### Verzoek

```http
POST /rest/asset/v2/email/usedby
Content-Type: application/json
```

### Antwoord

```json
{
  "assetId": "1017",
  "pageIndex": 0,
  "pageSize": 20,
  "type": "all"
}
```

## Notities

De eindpunten van de vraag keren meta-gegevens voor de activa terug. Gebruik de eindpuntverwijzing voor het volledige schema van beschikbare gebieden en om het even welke milieu-specifieke eigenschappen.
