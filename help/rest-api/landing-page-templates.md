---
title: Landingspagina-sjablonen
feature: REST API, Landing Pages
description: Marketo Landing Page Templates beheren via REST API-eindpunten voor gratis formulieren en soorten met instructies, zoeken op id of naam, HTML maken, bijwerken, klonen, Munchkin.
exl-id: f9d1255e-ec13-4b75-96d5-b4cc9457a51b
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '552'
ht-degree: 0%

---

# Landingspagina-sjablonen

[&#x200B; Landing de Verwijzing van het Eindpunt van het Malplaatje van de Pagina &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates)

Landing Page Templates is een bovenliggende bron en afhankelijkheid voor afzonderlijke Marketo-bestemmingspagina&#39;s. De het landen pagina&#39;s leiden het skelet van hun inhoud van het oudermalplaatje af.

## Sjabloontypen

Marketo heeft twee typen Landing Page Templates, gratis en met instructies. Sjablonen voor openstaande landingspagina&#39;s bieden een los gestructureerde bewerkingservaring voor pagina&#39;s die van deze sjablonen zijn afgeleid. Geleide sjablonen bieden een ervaring met een hoge structuur, waarbij elementtypen en -locaties op sjabloonniveau kunnen worden beperkt. Voor meer informatie over de verschillen, zie [&#x200B; dit document &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/demand-generation/landing-pages/understanding-landing-pages/understanding-free-form-vs-guided-landing-pages).

## Query

Het landen van de Malplaatjes van de Pagina steunt de standaardvraagtypes voor activa van [&#x200B; door identiteitskaart &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplateByIdUsingGET), [&#x200B; door naam &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplateByNameUsingGET), en [&#x200B; doorbladerend &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/getLandingPageTemplatesUsingGET). Deze eindpunten retourneren metagegevens voor de sjablonen. Het ophalen van de HTML-inhoud van sjablonen moet per template gebeuren via de bijbehorende id.

## Maken en bijwerken

Sjablonen worden gemaakt als lege elementen met bijbehorende metagegevens. Bij het maken van een sjabloon moeten een naam en map worden opgenomen, samen met een optionele beschrijving, templateType en enableMunchkin-parameter. templateType kan vrij of geleid zijn en is standaard aan freeForm. Zie de sectie Met instructies vs. vrije vorm voor informatie over de verschillende typen. enableMunchkin heeft als standaardwaarde false, en als deze optie is ingeschakeld, wordt het bijhouden van Munchkin niet uitgevoerd op alle onderliggende bestemmingspagina&#39;s van de sjabloon.

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

De inhoud voor het malplaatje moet afzonderlijk via het [&#x200B; Update Landing de Inhoud van het Malplaatje van de Pagina &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/updateLandingPageTemplateContentUsingPOST) eindpunt worden bevolkt.

### Metagegevens bijwerken

Metagegevens voor het landen van paginasjablonen kunnen via het [&#x200B; Update Landing Metadata van het Malplaatje van de Pagina &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Templates/operation/updateLpTemplateUsingPOST) eindpunt worden bijgewerkt. De naam, beschrijving en de instelling enableMunchkin kunnen op deze manier worden bijgewerkt.

### Inhoud bijwerken

Inhoud in Landing Page Templates wordt gemaakt als een destructieve update van alle HTML-inhoud. De inhoud moet worden doorgegeven als multipart/form-data, waarbij de enige parameter de naam content krijgt.

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

Marketo biedt een eenvoudige methode voor het klonen van Landing Page Templates. Dit is een POST-aanvraag via application/x-www-url-formencoded.

De padparameter `id` geeft de id op van de bronlandingspagina die moet worden gekloond.

De parameter `name` wordt gebruikt om de naam van het nieuwe landende Malplaatje van de Pagina te specificeren.

De parameter `folder` wordt gebruikt om de bovenliggende map op te geven waarin het nieuwe landingspaginasjabloon wordt geplaatst. Dit is in de vorm van een ingesloten JSON-object dat  `id` en `type` .

De optionele parameter `description` wordt gebruikt om de nieuwe landingspaginasjabloon te beschrijven.

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

- [&#x200B; Vrije Vorm het Landen de Malplaatjes van de Pagina &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-free-form-landing-page-template)
- [&#x200B; Geleide het Landen de Malplaatjes van de Pagina &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template)
- [&#x200B; Geleide Voorbeelden van het Malplaatje &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/guided-landing-page-template-list)

## Verwijderen

Als u een sjabloon wilt verwijderen, moet deze buiten gebruik zijn en niet zijn goedgekeurd. Dit betekent dat er geen enkele onderliggende landingspagina naar mag verwijzen.  Landing Page Templates with embedded social buttons may not be deleted with this API.
