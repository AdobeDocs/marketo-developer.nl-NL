---
title: "Landing Pages"
feature: REST API, Landing Pages
description: "Query-openingspagina's uitvoeren in Marketo."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1000'
ht-degree: 0%

---


# Landingspagina&#39;s

[Referentie eindpunt bestemmingspagina](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages)

Openingspagina&#39;s zijn webpagina&#39;s die worden gehost door Marketo.

## Query

Net als bij de meeste andere elementen kunnen landingspagina&#39;s worden gecontroleerd [op naam](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageByNameUsingGET), [door id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageByIdUsingGET)en door [bladeren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/browseLandingPagesUsingGET). Deze query&#39;s retourneren alleen metagegevens en de lijst met inhoudssecties voor een bestemmingspagina moet afzonderlijk worden opgevraagd door de id van de bestemmingspagina.

Door de inhoud van de bestemmingspagina op te vragen, wordt een lijst geretourneerd met inhoudssecties die beschikbaar zijn op de bestemmingspagina. Er moet een sectie aanwezig zijn in de inhoudslijst van een pagina om de inhoud bij te werken:

```
GET /rest/asset/v1/landingPage/{id}/content.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "6307#154ea1689d7",
    "result": [
        {
            "id": "67",
            "type": "Form",
            "index": 1,
            "content": {
                "content": "189",
                "contentType": "Form",
                "contentUrl": "https://app-devlocal1.marketo.com/#FO189A1ZN13LA1"
            },
            "formattingOptions": {
                "zIndex": 15,
                "left": "359px",
                "top": "122px"
            }
        }
    ]
}
```

De resultaten verschillen per geleide en vrije vormmalplaatjes, aangezien de geleide het landen pagina&#39;s met een reeks secties komen die door het malplaatje worden bepaald waarvan zij worden afgeleid, terwijl de vrije vormpagina&#39;s niet met vooraf bepaalde secties komen, en hun inhoud moet vóór het uitgeven worden toegevoegd.  De indeling van het kenmerk &quot;content&quot; kan variëren afhankelijk van het kenmerk &quot;type&quot; en of het veld statisch of dynamisch is.

## Maken en bijwerken

[Openingspagina&#39;s worden gemaakt](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/createLandingPageUsingPOST) door terug te verwijzen naar een sjabloon. De enige velden die u hoeft te maken, zijn naam, sjabloon (de id van de sjabloon) en de map waarin u de pagina wilt plaatsen. Voor extra meta-gegevens die kunnen worden bevolkt, zie de eindpuntverwijzing.

Geldige inhoudssoorten voor [pagina-inhoud plaatsen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content) eindpunten zijn: richText, HTML, Form, Image, Rectangle, Snippet.

```
POST rest/asset/v1/landingPages.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=createLandingPage&folder={"type": "Folder", "id": 11}&template=1&description=this is a test&workspace=default&title=test create&keywords=awesome&formPrefill=false
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "7a39#154cf7922c6",
    "result": [
        {
            "id": 27,
            "name": "createLandingPage",
            "description": "this is a test",
            "createdAt": "2016-05-20T18:41:43Z+0000",
            "updatedAt": "2016-05-20T18:41:43Z+0000",
            "folder": {
                "type": "Folder",
                "value": 11,
                "folderName": "Landing Pages"
            },
            "workspace": "Default",
            "status": "draft",
            "template": 1,
            "title": "test create",
            "keywords": "awesome",
            "robots": "index, nofollow",
            "formPrefill": false,
            "mobileEnabled": false,
            "URL": "https://app-devlocal1.marketo.com/lp/622-LME-718/createLandingPage.html",
            "computedUrl": "https://app-devlocal1.marketo.com/#LP27B2"
        }
    ]
}
```

Metagegevens van de bestemmingspagina kunnen worden bijgewerkt met de [Metagegevenseindpunt bestemmingspagina bijwerken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/updateLandingPageUsingPOST).

## Goedkeuring

Aanlandingspagina&#39;s voldoen aan het standaard conceptgoedgekeurde model, indien er een ontwerpversie en/of een goedgekeurde versie kan zijn. Wanneer updates op een pagina worden toegepast, worden ze altijd eerst toegepast op de conceptversie en worden ze alleen live weergegeven wanneer de pagina is goedgekeurd.

## Verwijderen

Als u een bestemmingspagina wilt verwijderen, moet deze eerst buiten gebruik zijn en niet door andere Marketo-elementen worden genoemd. De bestemmingspagina moet bovendien niet zijn goedgekeurd. Pagina&#39;s worden afzonderlijk verwijderd met de opdracht [Openingspagina verwijderen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/deleteLandingPageByIdUsingPOST) eindpunt. Pagina&#39;s met ingesloten sociale knoppen kunnen niet worden verwijderd via deze API. 

## Klonen

Marketo biedt een eenvoudige methode voor het klonen van een bestemmingspagina. Dit is een aanvraag voor een POST application/x-www-url-formencoded.

De `id` Padparameter geeft de id op van de bestemmingspagina van de bron die moet worden gekloond.

De `name` wordt gebruikt om de naam van de nieuwe bestemmingspagina te specificeren.

De `folder` parameter wordt gebruikt om de bovenliggende map op te geven waarin de nieuwe bestemmingspagina wordt gemaakt. Dit is in de vorm van een ingesloten JSON-object dat `id` en `type`.

De `template` parameter wordt gebruikt om bron te specificeren het Malplaatje van de Pagina van de Landing.

De optionele `description` wordt gebruikt om de nieuwe Openingspagina te beschrijven.

```
POST /rest/asset/v1/landingPage/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=MyNewLandingPage&folder={"type":"Program","id":1119}&template=57
```

```json
{
    "success": true,
    "errors": [],
    "requestId": "1078d#1683e4881c6",
    "warnings": [],
    "result": [
        {
            "id": 3291,
            "name": "MyNewLandingPage",
            "createdAt": "2019-01-11T18:59:25Z+0000",
            "updatedAt": "2019-01-11T18:59:25Z+0000",
            "folder": {
                "type": "Program",
                "value": 1119,
                "folderName": "DefaultProgramWithGuidedLP"
            },
            "workspace": "Default",
            "status": "draft",
            "template": 57,
            "robots": "index, nofollow",
            "formPrefill": false,
            "mobileEnabled": false,
            "URL": "http://na-abm.marketo.com/lp/284-RPR-133/DefaultProgramWithGuidedLPPerkutoTestLP-Clone-1.html",
            "computedUrl": "https://app-abm.marketo.com/#LP3291A1LA1"
        }
    ]
}
```

## Sectie Inhoud beheren

Inhoudssecties worden gerangschikt op basis van hun indexeigenschap en worden uiteindelijk ingedeeld volgens de CSS-regels die worden toegepast wanneer ze door de client worden weergegeven. Inhoudssecties worden opgenomen en beheerd met de bijbehorende [Toevoegen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/addLandingPageContentUsingPOST), [Bijwerken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) en [Verwijderen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/removeLandingPageContentUsingPOST) Sectieeindpunten van de sectie met bestemmingspagina-inhoud worden geplaatst en er kan een query op worden uitgevoerd [Inhoud landingspagina ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/getLandingPageContentUsingGET). Elke sectie heeft een type- en een waardeparameter. Het type bepaalt wat in de waarde moet worden gezet.  Voor deze eindpunten worden gegevens doorgegeven als POST x-www-form-urlencoded, niet als JSON.

**Sectietypen**

| Type | Waarde |
|--- |--- |
| DynamicContent | De id van de segmentatie. |
| Formulier | De id van het formulier. |
| HTML | Inhoud HTML van tekst. |
| Afbeelding | De id van het afbeeldingselement. |
| Rechthoek | Leeg. |
| RichText | Inhoud HTML van tekst.  Mag alleen tekstelementen bevatten. |
| Fragment | De id van het fragment. |
| SocialButton | De id van de sociale knop. |
| Video | De id van de video. |

Voor vrije-formulierpagina&#39;s moeten alle gewenste inhoudssecties worden toegevoegd en worden deze met de id ingesloten in het div-element `mktoContent`. Voor geleide pagina&#39;s kan een lijst van vooraf bepaalde elementen in de lijst van aanwezig zijn [Inhoud landingspagina ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/getLandingPageContentUsingGET) eindpunt. Meer kan worden toegevoegd of hun [inhoud bijgewerkt](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) via hun respectieve eindpunten.

### Dynamische inhoud

Als u een sectie Dynamische inhoud wilt maken, moet deze al voorkomen in de inhoudslijst van de bestemmingspagina. De [Sectie Landingspagina-inhoud bijwerken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageContentUsingPOST) eindpunt dan moet worden gebruikt om het type aan &quot;DynamicContent&quot;te plaatsen. Wanneer een sectie op dynamische inhoud wordt geplaatst, leidt het tot onderliggende dynamische secties binnen de inhoudssectie die allen het basistype van het omgezette element erven. Elke dynamische sectie neemt ook de inhoud van de omgezette sectie over.

```
GET /rest/asset/v1/landingPage/{id}/dynamicContent/RVMtNDg=.json
```

```json
{
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "46e#1560fa169d9",
  "result": [
    {
      "createdAt": "2016-07-21",
      "updatedAt": "2016-07-21",
      "segmentation": 1007,
      "segments": [
        {
          "segmentId": 1018,
          "segmentName": "Default",
          "type": "RichText",
          "content": "\n\t\t\t\t\t\t\tAlice was beginning to get very tired of sitting by her sister on the bank, and having nothing to do: once or twice she had peeped into the book her sister was reading, but it had no pictures or conversations in it.\n\t\t\t\t\t\t"
        },
        {
          "segmentId": 1017,
          "segmentName": "New Segment",
          "type": "RichText",
          "content": "\n\t\t\t\t\t\t\tAlice was beginning to get very tired of sitting by her sister on the bank, and having nothing to do: once or twice she had peeped into the book her sister was reading, but it had no pictures or conversations in it.\n\t\t\t\t\t\t"
        }
      ]
    }
  ]
}
```

[De inhoud bijwerken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Page-Content/operation/updateLandingPageDynamicContentUsingPOST) voor elk afzonderlijk segment gebeurt op basis van de segment-id.

```
POST /rest/asset/v1/landingPage/{id}/dynamicContent/{dynamicContentId}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
segment=New Segment&value=New Content
```

```json
 {
  "success": true,
  "warnings": [],
  "errors": [],
  "requestId": "7516#14e08fe7cbbc",
  "result": [
    {
      "id": 1012
    }
  ]
}
```

## Variabelen

Een van de nieuwe functies in instructiepagina&#39;s voor landen is bewerkbare variabelen.  Variabelen bevatten waarden voor elementen op een landingspagina.  Variabelen kunnen eenvoudig worden gewijzigd met de editor voor de bestemmingspagina, zoals hieronder wordt weergegeven:

![Landingspagina-variabelen](assets/landing-page-variables.png)

Variabelen worden gedefinieerd als metatags binnen `<head>` -element van een landingspagina-sjabloon met instructies. Er zijn drie typen variabelen beschikbaar: String, Color en Boolean.  Hier volgt een voorbeeld van drie definities van variabelen:

```html
<head>
  <meta charset="utf-8">
  <meta class="mktoString" mktoName="My String Variable" id="stringVar" default="Hello World!">
  <meta class="mktoColor" mktoName="My Color Variable" id="colorVar" default="#ffffff">
  <meta class="mktoBoolean" mktoName="My Boolean Variable" id="boolVar" default="true">
</head>
```

Zie voor meer informatie de sectie &quot;Bewerkbare variabele&quot; in [Een sjabloon voor een bestemmingspagina met instructies maken](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/demand-generation/landing-pages/landing-page-templates/create-a-guided-landing-page-template) documentatie.

### Query

Haal variabelen voor een geleide het landen pagina terug door landende pagina ID over te gaan om het eindpunt van de Variabelen van de Pagina van de Landing te krijgen.

```
GET /rest/asset/v1/landingPage/{id}/variables.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "10843#15a6d7e5fa1",
    "result": [
        {
            "id": "stringVar",
            "value": "Hello World!",
            "type": "string"
        },
        {
            "id": "colorVar",
            "value": "#FFFFFF",
            "type": "color"
        },
        {
            "id": "boolVar",
            "value": "true",
            "type": "boolean"
        }
    ]
}
```

In dit voorbeeld bevat de bestemmingspagina met instructies drie variabelen: stringVar, colorVar, boolVar.

### Bijwerken

Werk een variabele voor een geleide het landen pagina bij door landende paginaaid, veranderlijke identiteitskaart, en de veranderlijke waarde door te gaan om het Landing van het eindpunt van de Variabelen van de Pagina bij te werken.

```
POST /rest/asset/v1/landingPage/{id}/variable/{variableId}.json?value={newValue}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "2b07#15a6db77da3",
    "result": [
        {
            "id": "stringVar",
            "value": "Hello Brave New World!",
            "type": "String"
        }
    ]
}
```

## Voorvertoning openingspagina

Marketo biedt de [Volledige inhoud van bestemmingspagina ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Landing-Pages/operation/getLandingPageFullContentUsingGET) eindpunt om een live voorvertoning van een landingspagina op te halen zoals deze in een browser zou worden weergegeven. Er is één vereiste parameter, de `id` padparameter die de id is van de bestemmingspagina die u wilt voorvertonen. Er zijn twee extra optionele queryparameters:

- segmentatie: accepteert een array van JSON-objecten die segmentationId- en segmentId-kenmerken bevatten. Als deze optie is ingesteld, wordt de landingspagina voorvertoond alsof u een lead bent die overeenkomt met die segmenten.
- leadId: Accepteert de integer-id van een lead. Als deze optie is ingesteld, wordt de landingspagina weergegeven alsof deze door de aangewezen lead is bekeken.

```
GET /rest/asset/v1/landingPage/{id}/fullContent.json?leadId=1001&segmentation=[{"segmentationId":1030,"segmentId":1103}]
```

```json
{
  "success": true,
  "errors": [],
  "requestId": "119ab#17692849f1e",
  "warnings": [],
  "result": [
    {
      "id": 1023,
      "content": "<!DOCTYPE html>\n<html>\n <head>\n <meta charset=\"utf-8\">\n \n \n <meta name=\"robots\" content=\"index, nofollow\">\n <title></title>\n <style>\n body {background:#FFFFFF} \n #myConditionalDisplayArea {\n display: true;\n }\n </style>\n <link rel=\"shortcut icon\" href=\"/favicon.ico\" type=\"image/x-icon\" >\n<link rel=\"icon\" href=\"/favicon.ico\" type=\"image/x-icon\" >\n\n\n<style>.mktoGen.mktoImg {display:inline-block; line-height:0;}</style>\n </head>\n <body id=\"bodyId\">\n \n Hello Brave New World!\n <div class=\"mktoText\" id=\"exampleText\"><div>This is an example editable text area.</div>\n<div>Lead Full Name = Hanna Crawford</div>\n<div><br /></div>\n <script type=\"text/javascript\" src=\"//munchkin.marketo.net//munchkin.js\"></script><script>Munchkin.init('123-ABC-456', {customName: 'Test-Landing-Page-APIs_Guided-Landing-Page---deverly', PURL_VISIT_TOKEN, wsInfo: 'j1RR'});</script>\n<div id=\"mktoClickBlockingDiv\"></div>\n </body>\n</html>\n"
    }
  ]
}
```
