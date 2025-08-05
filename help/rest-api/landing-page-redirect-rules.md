---
title: Regels voor omleiding van bestemmingspagina
feature: REST API, Landing Pages
description: Configureer omleidingsregels voor de bestemmingspagina via de API.
exl-id: f63aa5ef-5872-4401-be75-6fb9b2977734
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '641'
ht-degree: 0%

---

# Regels voor omleiding van bestemmingspagina

[ het Bestaan van de Pagina richt Verwijzing van Regels Eindpunt van Regels ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules)

Marketo biedt een set REST API&#39;s voor het uitvoeren van CRUD-bewerkingen op URL&#39;s voor omleiding van bestemmingspagina&#39;s. Deze API&#39;s volgen het standaard interfacepatroon voor de bron-API&#39;s die de opties Query, Maken, Bijwerken en Verwijderen bieden.

Regels voor omleiding van bestemmingspagina bieden de mogelijkheid om een bestemmingspagina-URL om te leiden naar een andere pagina-URL. U kunt Marketo-bestemmingspagina&#39;s, andere bestemmingspagina&#39;s dan Marketo of combinaties daarvan omleiden. De extra informatie bij het Omleiden van de Regels van de Pagina kan [ hier ](https://experienceleague.adobe.com/docs/marketo/using/home.html) worden gevonden.

## Query

Het vragen van het landen van de pagina leidt regels na de standaardvraagtypes voor activa van [ door identiteitskaart ](#by_id), en [ doorbladerend ](#browse).

### Op id

Het [ krijgt Landing de Regels van de Omleiding van de Pagina door Identiteitseindpunt ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRuleByIdUsingGET) neemt één enkele het landen van paginaregel opnieuw richt `id` wegparameter en keert één enkele het landen pagina redirect regelverslag terug.

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

Het [ krijgt het Bestaan van de Pagina richt het eindpunt van Regels ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageRedirectRulesUsingGET) terugkeert een lijst van het landen van pagina omleiden regelverslagen.

Er zijn verscheidene facultatieve vraagparameters die tot filterresultaten kunnen worden overgegaan.

De parameter `offset` is een geheel getal dat het maximale aantal items aangeeft dat moet worden geretourneerd (de standaardwaarde is 20). Maximaal is 200. De parameter `maxReturn` is een geheel getal dat opgeeft waar moet worden begonnen met het ophalen van vermeldingen. Kan samen met verschuiving worden gebruikt (standaardwaarde is 0).

De parameter `hostname` kan worden gebruikt om te filteren op hostnaam van de bestemmingspagina&#39;s.

De `redirectToLandingPageId` is een geheel getal dat kan worden gebruikt om te filteren op de landings-id van de pagina waarnaar u omleidt. Met `redirectToPath` kunt u filteren op het pad van de bestemmingspagina&#39;s waarnaar u de omleiding wijzigt.

Met de parameters `earliestUpdatedAt` en `latestUpdatedAt` kunt u lage en hoge datumtijdwatermerken instellen voor het retourneren van omleidingsregels voor landingspagina&#39;s die zijn bijgewerkt of oorspronkelijk binnen het opgegeven bereik zijn gemaakt.

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

[ creeer het Aanvoeren van de Regel van de Omleiding van de Pagina ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/createLandingPageRedirectRuleUsingPOST) eindpunt wordt uitgevoerd met een toepassing/x-www-vorm-urlencoded POST die de volgende drie vereiste parameters heeft.

De parameter `hostname` geeft de hostnaam voor de landingspagina op. Dit moet bij een brandingdomein of alias horen. De maximale lengte is 255 tekens.

De parameter `redirectFrom` geeft de bestemmingspagina van de bron aan. Dit is een JSON-object dat een type/waardepaar bevat dat bepaalt of de bron een Marketo-landingspagina of een niet-Marketo-landingspagina is. Het kenmerk `type` kan &#39;landingPageId&#39; of &#39;path&#39; zijn.

| Parameter | Optioneel/vereist | Type | Beschrijving |
|---|---|---|---|
| &#39;get&#39; | Vereist | String | Methode, actie. |
| &quot;bezoeker&quot; | Vereist | String | Naam van methode. |
| callback | Vereist | Functie | Callback functie die voor elke teruggekeerde campagne moet worden teweeggebracht. |

De parameter `redirectTo` geeft de bestemmingspagina op. Dit is een JSON-object dat een type/waardepaar bevat dat bepaalt of de bron een Marketo-landingspagina of een niet-Marketo-landingspagina is. Het kenmerk `type` kan &#39;landingPageId&#39; of &#39;url&#39; zijn.

| Type bestemmingspagina | type redirectTo | Voorbeeld |
|---|---|---|
| Marketo | landingPageId | {&quot;type&quot;:&quot;landingPageId&quot;,&quot;value&quot;:&quot;1774&quot;} |
| Niet-Marketo | url | {&quot;type&quot;:&quot;url&quot;,&quot;value&quot;:&quot;www.contactLogs.com&quot;} |

Meer informatie bij het creëren van het doorleiden van de landingspagina kan [ hier ](https://experienceleague.adobe.com/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-actions/redirect-a-marketo-landing-page-to-another-page.html) worden gevonden.

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

Het [ eindpunt van de Regels van de Omleiding van de Pagina van 0} Update Landing {neemt één enkele het landen pagina omleiden van regel ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/updateLandingPageRedirectRuleUsingPOST) wegparameter. `id` Dit eindpunt wordt uitgevoerd met een application/x-www-form-urlencoded POST.

Net als met de aanroep create die hierboven wordt beschreven, worden een of meer van de volgende queryparameters doorgegeven om aan te geven welk kenmerk van de regel moet worden bijgewerkt: `hostname`, `redirectFrom`, `redirectTo` .

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

De [ Schrapping die Regel van de Omleiding van de Pagina door Identiteitseindpunt ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/deleteLandingPageRedirectRuleUsingPOST) richt één enkele het landen paginaregel opnieuw richt `id` wegparameter.

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

Het [ krijgt het Bestaan van de Domeinen van de Pagina ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Redirect-Rules/operation/getLandingPageDomainsUsingGET) eindpunt keert een lijst van het landen van paginadomeinverslagen terug.

Er zijn twee facultatieve vraagparameters die tot filterresultaten kunnen worden overgegaan.

De parameter `offset` is een geheel getal dat het maximale aantal items aangeeft dat moet worden geretourneerd (de standaardwaarde is 20, de maximale waarde is 200).

De parameter `maxReturn` is een geheel getal dat opgeeft waar moet worden begonnen met het ophalen van vermeldingen. Kan samen met `offset` worden gebruikt (standaardwaarde is 0).

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
