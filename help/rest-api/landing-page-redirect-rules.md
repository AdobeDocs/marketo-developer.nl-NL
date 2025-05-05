---
title: "Regels voor omleiding van bestemmingspagina"
feature: REST API, Landing Pages
description: "Configureer de omleidingsregels voor de landingspagina via de API."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 0%

---


# Regels voor omleiding van bestemmingspagina

[Referentie voor eindpunt van regels voor omleiding van bestemmingspagina](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules)

Marketo biedt een set REST API&#39;s voor het uitvoeren van CRUD-bewerkingen op URL&#39;s voor omleiding van bestemmingspagina&#39;s. Deze API&#39;s volgen het standaard interfacepatroon voor de bron-API&#39;s die de opties Query, Maken, Bijwerken en Verwijderen bieden.

Regels voor omleiding van bestemmingspagina bieden de mogelijkheid om een bestemmingspagina-URL om te leiden naar een andere pagina-URL. U kunt Marketo-bestemmingspagina&#39;s, andere bestemmingspagina&#39;s dan Marketo of combinaties daarvan omleiden. Aanvullende informatie over Regels voor omleiding van landingspagina&#39;s vindt u [hier](https://experienceleague.adobe.com/docs/marketo/using/home.html?lang=nl-NL).

## Query

Het vragen van het doorleiden van de landingspagina volgt de standaardvraagtypes voor activa van [door id](#by_id), en [bladeren](#browse).

### Op id

De [Regels voor omleiding van bestemmingspagina via id ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRuleByIdUsingGET) het eindpunt neemt één enkele het landen paginaregelomleiding `id` padparameter en retourneert één landingspagina-omleidingsregelrecord.

```
GET /rest/asset/v1/redirectRule/{id}.json
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "3d0#1707b2521e4",
    "warnings": [],
    "result": [
        {
            "id": 20,
            "redirectFromUrl": "https://calqeauto.com/DefDelPro1_LandingPage1.html",
            "hostname": "calqeauto.com",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5483
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5559
            },
            "redirectToUrl": "https://calqeauto.com/DefDelPro1_LandingPage2.html",
            "createdAt": "2020-02-25T06:56:44Z+0000",
            "updatedAt": "2020-02-25T06:56:44Z+0000"
        }
    ]
}
```

### Bladeren

De [Regels voor omleiding van bestemmingspagina ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRulesUsingGET) het eindpunt keert een lijst van het landen van pagina terug omleiden regelverslagen.

Er zijn verscheidene facultatieve vraagparameters die tot filterresultaten kunnen worden overgegaan.

De `offset` parameter is een geheel getal dat het maximale aantal items aangeeft dat moet worden geretourneerd (standaardwaarde is 20). Maximaal is 200. De `maxReturn` parameter is een geheel getal dat opgeeft waar moet worden begonnen met het ophalen van vermeldingen. Kan samen met verschuiving worden gebruikt (standaardwaarde is 0).

De `hostname` parameter kan worden gebruikt om op hostname van de landingspagina&#39;s te filteren.

De `redirectToLandingPageId` is een geheel getal dat kan worden gebruikt om te filteren op de landings-id waarnaar u doorstuurt. De `redirectToPath` U kunt gebruiken om te filteren op het pad van de bestemmingspagina&#39;s waarnaar u omleidt.

De `earliestUpdatedAt` en `latestUpdatedAt` Met parameters kunt u lage en hoge datumtijdwatermerken instellen voor het retourneren van omleidingsregels voor landingspagina&#39;s die zijn bijgewerkt of oorspronkelijk binnen het opgegeven bereik zijn gemaakt.

```
GET /rest/asset/v1/redirectRules.json&maxReturn=3
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "12213#1707b27efb5",
    "warnings": [],
    "result": [
        {
            "id": 5,
            "redirectFromUrl": "https://www.kirtideep.contact/LandingPage2.html",
            "hostname": "www.kirtideep.contact",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5406
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5404
            },
            "redirectToUrl": "https://www.kirtideep.contact/www.showLogs.com.html",
            "createdAt": "2019-11-14T06:26:29Z+0000",
            "updatedAt": "2019-11-14T06:26:29Z+0000"
        },
        {
            "id": 6,
            "redirectFromUrl": "https://www.kirtideep.contact/www.showLogs.com.html",
            "hostname": "www.kirtideep.contact",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5404
            },
            "redirectTo": {
                "type": "url",
                "value": "www.contactLogs.com"
            },
            "redirectToUrl": "www.contactLogs.com",
            "createdAt": "2019-11-14T06:27:10Z+0000",
            "updatedAt": "2019-11-14T06:27:10Z+0000"
        },
        {
            "id": 7,
            "redirectFromUrl": "https://www.kirtideep.contact/contact/log/check",
            "hostname": "www.kirtideep.contact",
            "redirectFrom": {
                "type": "path",
                "value": "/contact/log/check"
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5404
            },
            "redirectToUrl": "https://www.kirtideep.contact/www.showLogs.com.html",
            "createdAt": "2019-11-14T06:27:49Z+0000",
            "updatedAt": "2019-11-14T06:27:49Z+0000"
        }
    ]
}
```

## Maken

De [Regel voor omleiding bestemmingspagina maken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/createLandingPageRedirectRuleUsingPOST) Het eindpunt wordt uitgevoerd met een application/x-www-form-urlencoded POST die de volgende drie vereiste parameters heeft.

De `hostname` parameter geeft de hostnaam voor de landingspagina op. Dit moet bij een brandingdomein of alias horen. De maximale lengte is 255 tekens.

De `redirectFrom` parameter geeft de bestemmingspagina van de bron aan. Dit is een JSON-object dat een type/waardepaar bevat dat bepaalt of de bron een Marketo-landingspagina of een niet-Marketo-landingspagina is. De `type` kan het kenmerk &quot;landingPageId&quot; of &quot;path&quot; zijn.

| Parameter | Optioneel/vereist | Type | Beschrijving |
|---|---|---|---|
| &#39;get&#39; | Vereist | String | Methode, actie. |
| &quot;bezoeker&quot; | Vereist | String | Naam van methode. |
| callback | Vereist | Functie | Callback functie die voor elke teruggekeerde campagne moet worden teweeggebracht. |


De `redirectTo` parameter geeft de bestemmingspagina aan. Dit is een JSON-object dat een type/waardepaar bevat dat bepaalt of de bron een Marketo-landingspagina of een niet-Marketo-landingspagina is. De `type` kan het kenmerk &quot;landingPageId&quot; of &quot;url&quot; zijn.

| Type bestemmingspagina | type redirectTo | Voorbeeld |
|---|---|---|
| Marketo | landingPageId | {&quot;type&quot;:&quot;landingPageId&quot;,&quot;value&quot;:&quot;1774&quot;} |
| Niet-Marketo | url | {&quot;type&quot;:&quot;url&quot;,&quot;value&quot;:&quot;www.contactLogs.com&quot;} |

Meer informatie over het maken van omleidingsregels voor landingspagina&#39;s vindt u [hier](https://experienceleague.adobe.com/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-actions/redirect-a-marketo-landing-page-to-another-page.html?lang=nl-NL).

```
POST /rest/asset/v1/redirectRules.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
hostname=calqeauto.com&redirectFrom={"type":"landingPageId", "value":"5483"}&redirectTo={"type":"landingPageId", "value":"5559"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "d7c6#1707b223522",
    "warnings": [],
    "result": [
        {
            "id": 20,
            "redirectFromUrl": "https://calqeauto.com/DefDelPro1_LandingPage1.html",
            "hostname": "calqeauto.com",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5483
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5559
            },
            "redirectToUrl": "https://calqeauto.com/DefDelPro1_LandingPage2.html",
            "createdAt": "2020-02-25T06:56:44Z+0000",
            "updatedAt": "2020-02-25T06:56:44Z+0000"
        }
    ]
}
```

## Bijwerken

De [Regels voor omleiding van bestemmingspagina bijwerken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/updateLandingPageRedirectRuleUsingPOST) eindpunt neemt één landingspagina omleiden regel `id` padparameter. Dit eindpunt wordt uitgevoerd met een application/x-www-form-urlencoded POST.

Zoals met creeer hierboven beschreven vraag, worden één of meer van de volgende vraagparameters overgegaan om te specificeren welke attributen van de regel aan update: `hostname`, `redirectFrom`, `redirectTo`.

De bijgewerkte omleidingsregelrecord van de bestemmingspagina wordt geretourneerd in de reactie.

```
POST /rest/asset/v1/redirectRule/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
redirectTo={"type":"landingPageId", "value":"5561"}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "57b2#1707b3852d7",
    "warnings": [],
    "result": [
        {
            "id": 20,
            "redirectFromUrl": "https://calqeauto.com/DefDelPro1_LandingPage1.html",
            "hostname": "calqeauto.com",
            "redirectFrom": {
                "type": "landingPageId",
                "value": 5483
            },
            "redirectTo": {
                "type": "landingPageId",
                "value": 5561
            },
            "redirectToUrl": "https://calqeauto.com/DefDelPro1_LandingPage3.html",
            "createdAt": "2020-02-25T06:56:44Z+0000",
            "updatedAt": "2020-02-25T07:20:53Z+0000"
        }
    ]
}
```

## Verwijderen

De [Regel voor omleiding van bestemmingspagina door id verwijderen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/deleteLandingPageRedirectRuleUsingPOST) het eindpunt neemt één enkele het landen paginaregelomleiding `id` padparameter.

```
POST /rest/asset/v1/redirectRule/{id}/delete.json
```

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "d505#154d01c8364",
  "result": [
    {
      "id": 2
    }
  ]
}
```

## Bladeren door domeinen op bestemmingspagina

De [Domeinen landingspagina&#39;s ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageDomainsUsingGET) het eindpunt keert een lijst van het landen van paginadomeinverslagen terug.

Er zijn twee facultatieve vraagparameters die tot filterresultaten kunnen worden overgegaan.

De `offset` parameter is een geheel getal dat het maximale aantal items aangeeft dat moet worden geretourneerd (standaardwaarde is 20, maximum 200).

De `maxReturn` parameter is een geheel getal dat opgeeft waar moet worden begonnen met het ophalen van vermeldingen. Kan samen met `offset` (standaardwaarde is 0).

```
POST /rest/asset/v1/landingPageDomains.json?maxReturn=3
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "6eb8#1707b43d3cb",
    "warnings": [],
    "result": [
        {
            "hostname": "calqeauto.com",
            "type": "domain"
        },
        {
            "hostname": "www.google.com",
            "type": "domain-alias"
        },
        {
            "hostname": "www.kirti.com",
            "type": "domain-alias"
        }
    ]
}
```
