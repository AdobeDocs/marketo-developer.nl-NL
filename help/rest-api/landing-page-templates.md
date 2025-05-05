---
title: "Landing Page Templates"
feature: REST API, Landing Pages
description: "Templates voor bestemmingspagina's maken en bewerken."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '533'
ht-degree: 0%

---


# Landingspagina-sjablonen

[Referentie voor eindpunt landingspagina-sjabloon](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates)

Landing Page Templates is een bovenliggende bron en afhankelijkheid voor afzonderlijke Marketo-bestemmingspagina&#39;s. De het landen pagina&#39;s leiden het skelet van hun inhoud van het oudermalplaatje af.

## Sjabloontypen

Marketo heeft twee typen Landing Page Templates, gratis en met instructies. Sjablonen voor openstaande landingspagina&#39;s bieden een los gestructureerde bewerkingservaring voor pagina&#39;s die van deze sjablonen zijn afgeleid. Geleide sjablonen bieden een ervaring met een hoge structuur, waarbij elementtypen en -locaties op sjabloonniveau kunnen worden beperkt. Zie voor meer informatie over de verschillen [dit document](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/demand-generation/landing-pages/understanding-landing-pages/understanding-free-form-vs-guided-landing-pages).

## Query

Landingspaginasjablonen ondersteunen de standaardquerytypen voor elementen van [door id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplateByIdUsingGET), [op naam](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplateByNameUsingGET), en [bladeren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplatesUsingGET). Deze eindpunten retourneren metagegevens voor de sjablonen. Het ophalen van de HTML-inhoud van sjablonen moet per sjabloon gebeuren via de bijbehorende id.

## Maken en bijwerken

Sjablonen worden gemaakt als lege elementen met bijbehorende metagegevens. Bij het maken van een sjabloon moeten een naam en map worden opgenomen, samen met een optionele beschrijving, templateType en enableMunchkin-parameter. templateType kan vrij of geleid zijn en is standaard aan freeForm. Zie de sectie Met instructies vs. vrije vorm voor informatie over de verschillende typen. enableMunchkin heeft als standaardwaarde false, en als deze optie is ingeschakeld, wordt Munchkin-tracking niet uitgevoerd op alle onderliggende landingspagina&#39;s van de sjabloon.

```
POST /rest/asset/v1/landingPageTemplates.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=New LPT - PHP&folder={"id":12,"type":"Folder"}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "11b7#14dfe1e3bcf",
    "result": [
        {
            "id": 286,
            "name": "assetAPITest",
            "description": "test",
            "createdAt": "2015-06-16T20:45:03Z+0000",
            "updatedAt": "2015-06-16T20:45:03Z+0000",
            "url": "https://app-devlocal1.marketo.com/#LT286B2ZN12",
            "folder": {
                "type": "Folder",
                "value": 12,
                "folderName": "Templates"
            },
            "status": "draft",
            "workspace": "Default"
        }
    ]
}
```

Inhoud voor de sjabloon moet afzonderlijk worden ingevuld via de [Inhoud landingspagina bijwerken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/updateLandingPageTemplateContentUsingPOST) eindpunt.

### Metagegevens bijwerken

Metagegevens voor landingspaginasjablonen kunnen worden bijgewerkt via de [Sjabloonmetagegevens bestemmingspagina bijwerken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/updateLpTemplateUsingPOST) eindpunt. De naam, beschrijving en de instelling enableMunchkin kunnen op deze manier worden bijgewerkt.

### Inhoud bijwerken

De inhoud in de Malplaatjes van de Openingspagina wordt gemaakt als destructieve update aan de volledige inhoud van de HTML. De inhoud moet worden doorgegeven als multipart/form-data, waarbij de enige parameter de naam content krijgt.

```
POST /rest/asset/v1/landingPageTemplate/286/content.json
```

```
content-type: multipart/form-data; boundary=--------------------------435851813185237176536801
----------------------------435851813185237176536801
Content-Disposition: form-data; name="content"; filename="content.txt"
Content-Type: text/plain

<html>
<head>
</head>
<body>
<div>Placeholder Content</div>
</body>
</html>
----------------------------435851813185237176536801--
```

```
 {
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "7516#14e0dc60bbc",
  "result": [
    {
      "id": 286
    }
  ]
}
```

## Klonen

Marketo biedt een eenvoudige methode voor het klonen van Landing Page Templates. Dit is een aanvraag voor een POST application/x-www-url-formencoded.

De `id` De wegparameter specificeert identiteitskaart van het bron het Plaatsen Malplaatje van de Pagina aan kloon.

De `name` parameter wordt gebruikt om de naam van het nieuwe Malplaatje van de Openende Pagina te specificeren.

De `folder` parameter wordt gebruikt om de ouderomslag te specificeren waar het nieuwe Plaatsen van het Malplaatje van de Pagina zal verblijven. Dit is in de vorm van een ingesloten JSON-object dat  `id` en `type`.

De optionele `description` parameter wordt gebruikt om het nieuwe Malplaatje van de Openende Pagina te beschrijven.

```
POST /rest/asset/v1/landingPageTemplate/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=Standard Template Clone&folder={"type": "Folder", "id": 732}
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "dee6#1683e9fd410",
    "warnings": [],
    "result": [
        {
            "id": 61,
            "name": "Standard Template Clone",
            "createdAt": "2019-01-11T20:34:48Z+0000",
            "updatedAt": "2019-01-11T20:34:48Z+0000",
            "url": "https://app-abm.marketo.com/#LT61B2ZN732",
            "folder": {
                "type": "Folder",
                "value": 732,
                "folderName": "Test LP Template Clone"
            },
            "status": "draft",
            "workspace": "Default",
            "templateType": "freeForm",
            "enableMunchkin": true
        }
    ]
}
```

## Goedkeuring

De Malplaatjes van de Pagina van de landing volgen het standaard ontwerp-goedgekeurd model, waar er een ontwerp en/of een goedgekeurde versie kan zijn. Wanneer updates op een sjabloon worden toegepast, worden ze altijd eerst toegepast op de conceptversie en worden ze alleen live weergegeven wanneer de sjabloon is goedgekeurd.

Een sjabloon kan alleen worden goedgekeurd als het voldoet aan de regels voor het type ervan, ofwel in de vorm van een vrije vorm. Zie de respectievelijke ontwerpdocumenten voor meer informatie over de vereisten voor het maken en goedkeuren van sjablonen van hun respectievelijke typen:

- [Sjablonen voor openingspagina van vrije vorm](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-free-form-landing-page-template)
- [Sjablonen voor bestemmingspagina met instructies](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template)
- [Voorbeelden van geleide sjablonen](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/guided-landing-page-template-list)

## Verwijderen

Als u een sjabloon wilt verwijderen, moet deze buiten gebruik zijn en niet zijn goedgekeurd. Dit betekent dat er geen enkele onderliggende landingspagina naar mag verwijzen.  Landing Page Templates with embedded social buttons may not be deleted with this API.
