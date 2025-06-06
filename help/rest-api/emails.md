---
title: "E-mails"
feature: REST API
description: "API's voor het bewerken van e-mailmiddelen."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1946'
ht-degree: 0%

---


# E-mails

[E-maileindpuntverwijzing](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails) Er is een volledige set REST-eindpunten beschikbaar voor het manipuleren van e-mailelementen.

Opmerking: als u [Marketo Predictive Content](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/predictive-content/working-with-predictive-content/understanding-predictive-content), zullen de volgende eindpunten ontbreken als zij naar een e-mail verwijzen die vooruitlopende inhoud bevat: [E-mailinhoud ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET), [Sectie E-mailinhoud bijwerken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailComponentContentUsingPOST), [Concept voor e-mail goedkeuren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/approveDraftUsingPOST). De vraag keert een 709 foutencode, en het overeenkomstige foutenbericht terug.

## Query

Het zoekpatroon voor e-mailberichten is gelijk aan dat van sjablonen, zodat query&#39;s zijn toegestaan [door id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByIdUsingGET), [op naam](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByNameUsingGET), en [bladeren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailUsingGET)en voor het filteren op basis van de map met de browser en op naam-API&#39;s.

Opmerking: als een e-mailbericht onderdeel is van een e-mailprogramma dat wordt gebruikt [A/B-tests](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/email-marketing/email-programs/email-program-actions/email-test-a-b-test/add-an-a-b-test), dan is die e-mail niet beschikbaar voor vraag gebruikend de volgende eindpunten: [E-mail ophalen per id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByIdUsingGET), [E-mail ophalen op naam](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailByNameUsingGET), [E-mails ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailUsingGET). De aanroep geeft aan dat de zoekopdracht is geslaagd, maar bevat de volgende waarschuwing: &quot;Geen elementen gevonden voor de opgegeven zoekcriteria.&quot;

### Op ID

```
GET /rest/asset/v1/email/1351.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"9ad0#14a1832af8c",
   "result":[
      {
         "id":1356,
         "name":"sakZxhxkwV",
         "description":"sample description",
         "createdAt":"2014-12-05T02:06:21Z+0000",
         "updatedAt":"2014-12-05T02:06:21Z+0000",
         "subject":{
            "type":"Text",
            "value":"sample subject"
         },
         "fromName":{
            "type":"Text",
            "value":"RBxEtmdQZz"
         },
         "fromEmail":null,
         "replyEmail":{
            "type":"Text",
            "value":"Qlikf@testmail.com"
         },
         "folder":{
            "type":"folder",
            "value":10421
         },
         "operational":false,
         "textOnly":false,
         "publishToMSI":false,
         "webView":false,
         "status":false,
         "template":338,
         "workspace":"Default",
         "isOpenTrackingDisabled": false,
         "version": 2,
         "autoCopyToText": true,
         "ccFields": [
            {
              "attributeId": "157",
              "objectName": "lead",
              "displayName": "Lead Owner Email Address",
              "apiName": null
            }
          ],
         "preHeader": "My awesome preheader!"
      }
   ]
}
```

### Op naam

Voor door naam, kunt u naar keuze een omslag tot onderzoek slechts in die omslag overgaan.

```
GET /rest/asset/v1/email/byName.json?name=My Email&folder={"id":1056,"type"="Folder"}
```

```json
{
   "success":true,
   "warnings":[
   ],
   "errors":[
   ],
   "requestId":"3a7f#14c484de875",
   "result":[
      {
         "id":1032,
         "name":"My Email",
         "description":"eCjxjIHmYPLtecoSphkvIXlrygOBDLhgyQKnsKMpiKWgSCKhkPMUFvFPUvEylmFiLjQGnffXGaiNLxAwiFOmIDvxEINoaSYascJw",
         "createdAt":"2015-03-23T20:23:25Z+0000",
         "updatedAt":"2015-03-23T20:23:25Z+0000",
         "subject":{
            "type":"Text",
            "value":"ezyKBmDcyCcUIrXASrLSvRuWQgWpRZxQstJoStgMSLEBASGKMpAnVeWrgJsaVFoFJUEXhEIPpDAWpzajzingUruFpiMcRRwtoBzU"
         },
         "fromName":{
            "type":"Text",
            "value":"dAiqRNJOdY"
         },
         "fromEmail":{
            "type":"Text",
            "value":"ilZxG@testmail.com"
         },
         "replyEmail":{
            "type":"Text",
            "value":"VYsCS@testmail.com"
         },
         "folder":{
            "type":"folder",
            "value":1056
         },
         "operational":false,
         "textOnly":false,
         "publishToMSI":false,
         "webView":false,
         "status":"draft",
         "template":32,
         "workspace":"Default",
         "isOpenTrackingDisabled": false,
        "version": 2,
         "autoCopyToText": true,
         "ccFields": [
            {
              "attributeId": "157",
              "objectName": "lead",
              "displayName": "Lead Owner Email Address",
              "apiName": null
            }
          ],
         "preHeader": "My awesome preheader!"
      }
   ]
}
```

### Bladeren

Bladeren in mappen werkt net als andere eindpunten in de Asset API en biedt optionele filtering op `status`, `folder`, `earliestUpdatedAt`/`latestUpdatedAt`, `maxReturn`, en `offset`. `status` is goedgekeurd of is een concept. `folder` is een JSON-object dat `id` en `type`. `maxReturn` is een geheel getal dat het aantal resultaten beperkt (de standaardwaarde is 20, de maximale waarde is 200), en `offset` is een geheel getal dat kan worden gebruikt met `maxReturn` om door grote resultaatreeksen te lezen (gebrek is 0).

```
GET /rest/asset/v1/emails.json?maxReturn=3&folder={"id":341,"type":"Folder"}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "17576#14e22eb29cb",
    "result": [
        {
            "id": 2137,
            "name": "Social Sharing in Email",
            "description": "",
            "createdAt": "2011-03-04T17:12:42Z+0000",
            "updatedAt": "2011-03-04T19:04:36Z+0000",
            "url": null,
            "subject": {
                "type": "Text",
                "value": "Republish this content to your favorite social site!"
            },
            "fromName": {
                "type": "Text",
                "value": "Demo Master Marketo"
            },
            "fromEmail": {
                "type": "Text",
                "value": "demomaster@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "demomaster@marketo.com"
            },
            "folder": {
                "type": "Folder",
                "value": 341,
                "folderName": "Social Media"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": true,
            "status": "approved",
            "template": null,
            "workspace": "Default",
            "isOpenTrackingDisabled": false,
            "version": 2,
            "autoCopyToText": true,
            "ccFields": [
               {
                 "attributeId": "157",
                 "objectName": "lead",
                 "displayName": "Lead Owner Email Address",
                 "apiName": null
               }
             ],
            "preHeader": "My awesome preheader!"
        }
    ]
}
```

## Query-inhoud

U kunt [de beschikbare bewerkbare secties ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) voor een e-mailbericht door de inhoud ervan op te vragen en desgewenst op de status te filteren om de secties voor de goedgekeurde versie of de conceptversie op te halen.

```
GET /rest/asset/v1/email/1356/content.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"8a44#14c484de8c8",
   "result":[
      {
         "htmlId":"edit_text_3",
         "value":[
            {
               "type":"HTML",
               "value":"Content from testCreateEmailTemplate2"
            },
            {
               "type":"Text",
               "value":"Content from testCreateEmailTemplate2"
            }
         ],
         "contentType":"Text"
      }
   ]
}
```

Secties kunnen worden geretourneerd als secties met een type dynamicContent. Zie de [Dynamische inhoud](dynamic-content.md) voor meer informatie.

## Query CC-velden

U kunt de set velden ophalen die is ingeschakeld voor e-mail CC in de doelinstantie door de [E-mailCC-velden ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailCCFieldsUsingGET) eindpunt.

```
GET /rest/asset/v1/email/ccFields.json
```

```json
{  
   "success":true,
   "errors":[ ],
   "requestId":"e54b#16796fdbd4e",
   "warnings":[ ],
   "result":[  
      {  
         "attributeId":"157",
         "objectName":"lead",
         "displayName":"Lead Owner Email Address",
         "apiName":"leadOwnerEmailAddress"
      },
      {  
         "attributeId":"396",
         "objectName":"company",
         "displayName":"Account Owner Email Address",
         "apiName":"accountOwnderEmailAddress"
      }
   ]
}
```

## Maken en bijwerken

[E-mails worden gemaakt](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/createEmailUsingPOST) op basis van een bronsjabloon en beschikken over een lijst met bewerkbare secties die zijn afgeleid van elk afzonderlijk HTML-element in die sjabloon met een klasse van &quot;marketEditable&quot; en een unieke id-eigenschap. Als u een e-mailbericht maakt met de API, wordt er een record gemaakt op basis van de sjabloon en worden er aanvullende metagegevens doorgegeven. De volgende parameters zijn vereist voor een geslaagde e-mailaanroep maken: naam, sjabloon, map.

De volgende parameters zijn optioneel voor het maken: `subject`, `fromName`, `fromEmail`, `replyEmail`, `operational`, `isOpenTrackingDisabled`. Indien niet ingesteld, `subject` is leeg, `fromName`, `fromEmail` en `replyEmail` wordt ingesteld op standaardinstellingen voor instanties, en `operational` en `isOpenTrackingDisabled` false. `isOpenTrackingDisabled` Hiermee bepaalt u of de pixel voor het openen van een e-mail wordt opgenomen wanneer deze wordt verzonden.

```
POST /rest/asset/v1/emails.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=My New Email 02 - deverly&folder={"id":1017,"type":"Program"}&template=24&description=This is a test email&subject=Hey There&fromName=SomeBody&fromEmail=somebody@marketo.com&replyEmail=somebody@marketo.com
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "f557#14e22db88d9",
    "result": [
        {
            "id": 2212,
            "name": "My New Email 02 - deverly",
            "description": "This is a test email",
            "createdAt": "2015-06-23T23:58:09Z+0000",
            "updatedAt": "2015-06-23T23:58:09Z+0000",
            "url": "https://app-abm.marketo.com/#EM2212A1LA1",
            "subject": {
                "type": "Text",
                "value": "Hey There"
            },
            "fromName": {
                "type": "Text",
                "value": "SomeBody"
            },
            "fromEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "folder": {
                "type": "Program",
                "value": 1017,
                "folderName": "Landing Page - promotion"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "draft",
            "template": 24,
            "workspace": "Default",
            "isOpenTrackingDisabled": false,
            "version": 2,
            "autoCopyToText": false,
            "ccFields": null,  
            "preHeader": null
        }
    ]
}
```

[Een e-mail bijwerken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailContentUsingPOST) record kan worden uitgevoerd door id. Zo kunt u de beschrijving of naam van het e-mailbericht bijwerken.

```
POST /rest/asset/v1/email/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
description=This is an Email&name=Updated Email
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "f557#14e22db88d9",
    "result": [
        {
            "id": 2212,
            "name": "Updated Email",
            "description": "This is an Email",
            "createdAt": "2015-06-23T23:58:09Z+0000",
            "updatedAt": "2015-06-23T23:58:09Z+0000",
            "url": "https://app-abm.marketo.com/#EM2212A1LA1",
            "subject": {
                "type": "Text",
                "value": "Hey There"
            },
            "fromName": {
                "type": "Text",
                "value": "SomeBody"
            },
            "fromEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "folder": {
                "type": "Program",
                "value": 1017,
                "folderName": "Landing Page - promotion"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "draft",
            "template": 24,
            "workspace": "Default",
            "isOpenTrackingDisabled": false,
            "version": 2,
            "autoCopyToText": false,
            "ccFields": null,  
            "preHeader": null
        }
    ]
}
```

### Sectie, type en update van inhoud

De inhoud voor elke sectie van een e-mail moet afzonderlijk worden bijgewerkt, behalve het onderwerp, fromName, fromEmail, and responseEmail, die worden bijgewerkt met de [E-mailinhoud bijwerken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateEmailContentUsingPOST) eindpunt. Wanneer u dit eindpunt gebruikt, kunnen deze waarden ook worden ingesteld op het gebruik van dynamische inhoud in plaats van statische inhoud. Elke parameter is een JSON-object type/waarde, waarbij het type &#39;Text&#39; of &#39;DynamicContent&#39; is en de waarde ofwel de juiste tekstwaarde is, ofwel de id van de segmentatie die moet worden gebruikt voor de dynamische inhoud. Gegevens worden doorgegeven als POST x-www-form-urlencoded, niet als JSON.  isOpenTrackingDisabled kan worden ingesteld met E-mailinhoud bijwerken

```
POST /rest/asset/v1/email/{id}/content.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
subject={"type":"Text","value":"Gettysburg Address"}&fromEmail={"type":"Text","value":"abe@testmail.com"}&fromName={"type":"Text","value":"Abe Lincoln"}&replyTO={"type":"Text","value":"replies@testmail.com"}
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"c865#14a1832afac",
   "result":[
      {
         "id":1356
      }
   ]
}
```

Als u een sectie instelt voor het gebruik van dynamische inhoud, moet de sectie-id worden opgehaald via de aanroep E-mailinhoud ophalen.

### Bewerkbare sectie bijwerken

Bewerkbare secties worden bijgewerkt door hun individuele htmlIds. Alleen de id van de e-mail en htmlId van de sectie zijn vereist als padparameters, terwijl type, waarde en textValue optioneel zijn. Het type kan een van de volgende waarden hebben: &quot;Tekst,&quot; &quot;Dynamische inhoud&quot; of &quot;Fragment&quot;. Dit heeft invloed op de doorgegeven waarde. Als het type Tekst is, is de waarde een tekenreeks die de HTML-inhoud van de sectie bevat. Als het DynamicContent is, dan is het een blok JSON, met drie leden, type, dat &quot;DynamicContent&quot; zal zijn, segmentatie die identiteitskaart van de segmentatie is voor de inhoud te gebruiken, en gebrek, dat een koord is dat de standaard HTML inhoud van de sectie bevat. De optionele textValue parameter is een tekenreeks die de tekstversie van de sectie bevat. Gegevens worden doorgegeven als POST x-www-form-urlencoded, niet als JSON.

```
POST /rest/asset/v1/email/{id}/content/{htmlId}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
type=Text&value=<h1>Hello World!</h1>&textValue=Hello World!
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "155ac#14d58dfa9ad",
    "result": [
        {
            "id": 2179
        }
    ]
}
```

Opmerking: als automatische kopie naar tekst is uitgeschakeld voor een fragment dat is ingesloten in een e-mail, wordt de HTML-waarde van het fragment bijgewerkt en wordt de tekstversie van een andere sectie in de e-mail bijgewerkt. De tekst in de e-mailtekstversie bevat dan de bijgewerkte waarde van het fragment HTML, niet de vorige versie zoals zou worden verwacht als automatische kopie uitgeschakeld.

## Modules

In E-maileditor 1.0 is een module een sectie van uw e-mailbericht die in de sjabloon is gedefinieerd. Modules kunnen elke combinatie bevatten van elementen, variabelen en andere HTML-inhoud zoals beschreven [hier](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Modules). Marketo biedt een set API&#39;s voor het beheer van modules in een e-mailbericht. Voor aan modules gerelateerde eindpunten waarvoor de methode HTTP-POST vereist is, wordt de hoofdtekst opgemaakt als &quot;application/x-www-form-urlencoded&quot; (niet als JSON).

De meeste op module betrekking hebbende eindpunten vereisen &quot;moduleId&quot;als wegparameter. Dit is een tekenreeks die de module beschrijft. moduleIds wordt geretourneerd door [E-mailinhoud ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) eindpunt als &quot;htmlId&quot;attribuut (zie [Query](#modules_query) hieronder).

### Query

Om met modules te werken, moet u een moduleId parameter specificeren, die uniek de module identificeert. U kunt ook de parameter van de moduleindex specificeren, die een geheel is dat de orde van de module in e-mail beschrijft.

[ModuleIds en hun indexen ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailContentByIdUsingGET) door de e-mailid op te geven als padparameter.

In het volgende voorbeeld wordt een 1.0-e-mailbericht opgevraagd dat is gebaseerd op de sjabloon &quot;Skelet&quot; die is gevonden in de sectie &quot;Starter Templates&quot; van de Sjabloonkiezer-gebruikersinterface.

```
GET /rest/asset/v1/email/{moduleId}/content.json
```

```json
{
  "success": true,
  "warnings": [ ],
  "errors": [ ],
  "requestId": "3d79#158da6492bd",
  "result": [
    {
      "htmlId": "free-image",
      "contentType": "Module",
      "index": 1,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "single",
      "value": {
        "width": "600",
        "altText": "",
        "style": "-ms-interpolation-mode: bicubic; outline: none; border-right-width: 0; border-bottom-width: 0; border-left-width: 0; text-decoration: none; border-top-width: 0; display: block; max-width: 100%; line-height: 100%; height: auto; width: 600px"
      },
      "contentType": "Image",
      "parentHtmlId": "free-image",
      "isLocked": false
    },
    {
      "htmlId": "video",
      "contentType": "Module",
      "index": 2,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "video2",
      "value": {
        
      },
      "contentType": "Video",
      "parentHtmlId": "video",
      "isLocked": false
    },
    {
      "htmlId": "free-text",
      "contentType": "Module",
      "index": 3,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "text",
      "value": [
        {
          "type": "HTML",
          "value": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Harum officiis dolorum, nulla, mollitia ducimus iure modi perferendis tenetur ea illum veniam aut sapiente deserunt repellendus. Excepturi illo numquam sint harum."
        },
        {
          "type": "Text",
          "value": "Lorem ipsum dolor sit amet, consectetur adipisicing elit. Harum officiis dolorum, nulla, mollitia ducimus iure modi perferendis tenetur ea illum veniam aut sapiente deserunt repellendus. Excepturi illo numquam sint harum."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "free-text",
      "isLocked": false
    },
    {
      "htmlId": "two-articles",
      "contentType": "Module",
      "index": 6,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "article3",
      "value": {
        "height": "auto",
        "width": "270",
        "style": "-ms-interpolation-mode: bicubic; outline: none; border-right-width: 0; border-bottom-width: 0; border-left-width: 0; text-decoration: none; border-top-width: 0; display: block; max-width: 100%; line-height: 100%; height: auto; width: 270px"
      },
      "contentType": "Image",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "articleTitle",
      "value": [
        {
          "type": "HTML",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        },
        {
          "type": "Text",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "text2",
      "value": [
        {
          "type": "HTML",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        },
        {
          "type": "Text",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "article4",
      "value": {
        "height": "auto",
        "width": "270",
        "style": "-ms-interpolation-mode: bicubic; outline: none; border-right-width: 0; border-bottom-width: 0; border-left-width: 0; text-decoration: none; border-top-width: 0; display: block; max-width: 100%; line-height: 100%; height: auto; width: 270px"
      },
      "contentType": "Image",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "articleTitle2",
      "value": [
        {
          "type": "HTML",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        },
        {
          "type": "Text",
          "value": "LOREM IPSUM DOLOR SIT AMET"
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "text3",
      "value": [
        {
          "type": "HTML",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        },
        {
          "type": "Text",
          "value": "Gumbo beet greens corn soko endive gumbo gourd. shallot courgette tatsoi pea sprouts fava bean collard greens dandelion okra wakame tomato. Dandelion cucumber earthnut pea peanut soko zucchini."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "two-articles",
      "isLocked": false
    },
    {
      "htmlId": "footer",
      "contentType": "Module",
      "index": 7,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "footerText",
      "value": [
        {
          "type": "HTML",
          "value": "<p style=\"text-align: center;\"><span style=\"color: #333333;\"><strong>Acme, Inc<\/strong><\/span><\/p> \n<div style=\"text-align: center;\">\n  You received this because you've subscribed to our newsletter. Click \n <a href=\"{{system.unsubscribeLink}}\" target=\"_blank\" class=\"mktNoTrack\">here<\/a> to unsubscribe. \n <br> \n<\/div>"
        },
        {
          "type": "Text",
          "value": "Acme, Inc \n You received this because you've subscribed to our newsletter. Click here <{{system.unsubscribeLink}}> to unsubscribe."
        }
      ],
      "contentType": "Text",
      "parentHtmlId": "footer",
      "isLocked": false
    },
    {
      "htmlId": "spacer",
      "contentType": "Module",
      "index": 0,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "CTA",
      "contentType": "Module",
      "index": 4,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    },
    {
      "htmlId": "hr",
      "contentType": "Module",
      "index": 5,
      "parentHtmlId": "template-wrapper",
      "isLocked": false
    }
  ]
}
```

De resultaatarray bevat elementen die zowel modules als HTML-elementen die onderling zijn gecombineerd, beschrijven. Moduleelementen bevatten het kenmerk &quot;contentType&quot;: &quot;Module&quot; en het kenmerk &quot;index&quot;. ModuleId wordt opgeslagen in het &quot;htmlId&quot;attribuut.

In vervolg op het bovenstaande voorbeeld &quot;Skelet&quot; bevat de volgende tabel een overzicht van moduleIds en de bijbehorende indexen in het e-mailbericht.

| moduleId (ook bekend als htmlId) | Index |
|---|---|
| spacer | 0 |
| afbeelding zonder afbeelding | 1 |
| video | 2 |
| vrije tekst | 3 |
| CTA | 4 |
| hr | 5 |
| twee artikelen | 6 |
| voettekst | 7 |

#### Toevoegen

[Een module toevoegen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/addModuleUsingPOST) naar een e-mailbericht door een van de bestaande modules te selecteren in de e-mailsjabloon die in gebruik is. Doe dit door e-mailidentiteitskaart, en moduleId als wegparameters te specificeren. De parameter van de indexvraag wordt vereist en bepaalt de orde van de module in e-mail. Als de indexwaarde de grootste bestaande indexwaarde overschrijdt, dan wordt de module toegevoegd aan e-mail.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/add.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
index=10
```

```
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "1063e#158d6ad2c3f",
    "result": [
        {
            "id": 1028
        }
    ]
}
```

#### Verwijderen

[Een module verwijderen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/deleteModuleUsingPOST) door e-mailidentiteitskaart, en moduleId als wegparameters te specificeren.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/delete.json
```

```
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "2356#158d6f6104a",
    "result": [
        {
            "id":1028
        }
    ]
}
```

#### Dupliceren

[Een module dupliceren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/duplicateModuleUsingPOST) door e-mailidentiteitskaart, en moduleId als wegparameters te specificeren. Deze vraag dupliceert de module, die het onder de originele module plaatst en de andere modules neer duwt.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/duplicate.json
```

```
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "e740#158d705d967",
    "result": [
        {
            "id":1028
        }
    ]
}
```

#### Opnieuw rangschikken

[Modules opnieuw rangschikken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/rearrangeModulesUsingPOST)array die alle modules en de gewenste positie binnen de e-mail voor elke module bevat. Elk arrayelement bevat een JSON-object met de volgende notatie: { &quot;index&quot;: &lt;_index_>, &quot;moduleId&quot;: &quot;&lt;_moduleId_>&quot; }, waarbij &lt;_index_> is het op nul gebaseerde modulevolgordenummer en &lt;_moduleId_> is moduleId.

```
POST /rest/asset/v1/email/{id}/content/rearrange.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
positions=[ {"index": 0, "moduleId": "free-image"}, {"index": 1, "moduleId": "title"}, {"index": 2, "moduleId": "mkvideo"}, {"index": 3, moduleId": "free-text"}, {"index": 4, "moduleId": "blankSpace"}, {"index": 5, "moduleId": "Separator"}, {"index": 6, "moduleId": "callToAction"}, {"index": 7, "moduleId": "blankSpace2"}, {"index": 8, "moduleId": "blankSpace3"} ]
```

```
{
    "success": true,
    "warnings":[ ],
    "errors":[ ],
    "requestId": "e67a#158d72d1cde",
    "result":[
        {
            "id": 1030
        }
    ]
}
```

#### Naam wijzigen

[De naam van een module wijzigen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/renameUsingPOST) op een e-mail door de nieuwe naam via de parameter name door te geven. Geef de e-mailid en moduleId (bestaande naam) op als padparameters.

```
POST /rest/asset/v1/email/{id}/content/{moduleId}/rename.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=MarketoVideo
```

```
{
    "success": true,
    "warnings":[ ],
    "errors": [ ],
    "requestId":"11521#158d740abc0",
    "result": [
        {
            "id": 1030
        }
    ]
}
```

## Variabelen

In E-maileditor 1.0 worden variabelen gebruikt om waarden voor elementen in uw e-mail op te slaan. Elke variabele wordt bepaald door Marketo-specifieke syntaxis aan uw HTML toe te voegen zoals beschreven [hier](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/email-marketing/general/email-editor-2/email-template-syntax#EmailTemplateSyntax-Variables). Marketo biedt een set API&#39;s voor het beheer van variabelen in een e-mailbericht.

### Query

[Variabelen ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailVariablesUsingGET) voor een e-mailbericht door de e-mailid op te geven als parameter path.

In het volgende voorbeeld wordt een 1.0-e-mailbericht opgevraagd dat is gebaseerd op de sjabloon &quot;Skelet&quot; die is gevonden in de sectie &quot;Starter Templates&quot; van de Sjabloonkiezer-gebruikersinterface.

```
GET /rest/asset/v1/email/{id}/variables.json
```

```
{
  "success": true,
  "warnings": [ ],
  "errors": [  ],
  "requestId": "756#158dade55e8",
  "result": [
    {
      "name": "twoArticlesSpacer5",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer6",
      "value": "15",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "footerSpacer2",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer7",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLinkText2",
      "value": "CALL TO ACTION",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer8",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLinkText",
      "value": "CALL TO ACTION",
      "moduleScope": false
    },
    {
      "name": "freeTextSpacer",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "freeTextSpacer2",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "ctaSpacer2",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "hrBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "freeTextBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "spacerBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLink2",
      "value": "http:\/\/mylink",
      "moduleScope": false
    },
    {
      "name": "hrBorderColor",
      "value": "#e6e6e6",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderSize",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "ctaLink",
      "value": "http:\/\/mylink",
      "moduleScope": false
    },
    {
      "name": "freeImageBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "spacerSpacer",
      "value": "40",
      "moduleScope": false
    },
    {
      "name": "footerSpacer",
      "value": "10",
      "moduleScope": false
    },
    {
      "name": "ctaLinkText",
      "value": "CALL TO ACTION",
      "moduleScope": false
    },
    {
      "name": "twoArticlesButtonBackgroundColor2",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "ctaBorderSize",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "ctaBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "footerBackgroundColor",
      "value": "#ffffff",
      "moduleScope": false
    },
    {
      "name": "twoArticlesLink",
      "value": "http:\/\/mylink",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "ctaBorderColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderColor2",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "hrBorderSize",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "twoArticlesButtonBackgroundColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "twoArticlesBorderSize2",
      "value": "1",
      "moduleScope": false
    },
    {
      "name": "ctaButtonBackgroundColor",
      "value": "#333333",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer4",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer3",
      "value": "15",
      "moduleScope": false
    },
    {
      "name": "twoArticlesSpacer2",
      "value": "20",
      "moduleScope": false
    },
    {
      "name": "ctaSpacer",
      "value": "20",
      "moduleScope": false
    }
  ]
}
```

De resultaatarray bevat elementen die variabelen beschrijven, één variabele per element.

Variabelen kunnen globaal worden toegepast op de volledige e-mail, of lokaal op een specifieke module. Variabelen van een bereik bevatten de kenmerken &quot;name&quot;, &quot;value&quot; en &quot;moduleScope&quot;. Het kenmerk &quot;moduleScope&quot; is booleaans, waarbij false algemeen en true lokaal aangeeft. Lokale variabelen bevatten een extra kenmerk &quot;moduleId&quot; dat de module opgeeft waaraan de variabele is gekoppeld.

#### Bijwerken

[Een variabele bijwerken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/updateVariableUsingPOST) in een e-mailbericht door de nieuwe gewenste waarde door te geven via de parameter value. Geef de e-mailid en de variabelenaam op als padparameters. Als u een modulevariabele bijwerkt, dan moet u de parameter moduleId ook overgaan om de module te specificeren waaraan de variabele wordt geassocieerd.

In het volgende voorbeeld werken we een algemene variabele met de naam &quot;hrBorderSize&quot; bij naar de waarde 1.

```
POST /rest/asset/v1/email/{id}/variable/{name}.json
```

```
Content-Type: application/x-www-form-urlencoded; charset=utf-8
```

```
value=2
```

```
{
    "success":true,
    "warnings":[ ],
    "errors":[ ],
    "requestId":"feb5#158db4be57e",
    "result": [
        {
            "name":"hrBorderSize",
            "value":"2",
            "moduleScope":false
        }
    ]
}
```

In het volgende voorbeeld werken we een lokale variabele met de naam &quot;ctaLinkText&quot; bij naar de waarde &quot;Click this button!&quot; in moduleId &quot;CTA&quot;.

```
POST /rest/asset/v1/email/1032/variable/ctaLinkText.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
value=Click this button!&moduleId=CTA
```

```json
{
    "success": true,
    "warnings":[ ],
    "errors":[ ],
    "requestId": "7f34#158dc28d2f7",
    "result": [
        {
            "name":"ctaLinkText",
            "value":"Click this button!",
            "moduleScope":true,
            "moduleId":"CTA"
        }
    ]
}
```

## Goedkeuring

E-mails volgen het standaardpatroon voor goedkeuring van elementrecords. U kunt een concept goedkeuren, een goedgekeurde versie goedkeuren en een bestaand concept van een e-mailbericht door elk van hun eigen eindpunten verwijderen.

### Goedkeuren

Wanneer het roepen van het goedkeuringseindpunt, zal e-mail tegen de regels voor e-mails van Marketo worden bevestigd. De `from name`, `from email`, `reply to email`, en `subject` moet worden ingevuld voordat het e-mailbericht kan worden goedgekeurd.

```
POST /rest/asset/v1/email/{id}/approveDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"15dbf#14a1832ae86",
   "result":[
      {
         "id":1362
      }
   ]
}
```

#### Niet goedkeuren

De `unapprove` deze bewerking kan alleen worden uitgevoerd op goedgekeurde e-mails.

```
POST /rest/asset/v1/email/{id}/unapprove.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"3514#14a1832b0fa",
   "result":[
      {
         "id":1364
      }
   ]
}
```

#### Negeren

Het e-mailbericht moet de status concept hebben om te worden verwijderd. Een goedgekeurd e-mailbericht kan niet worden verwijderd.

```
POST /rest/asset/v1/email/{id}/discardDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"182c0#14a1832af4f",
   "result":[
      {
         "id":1362
      }
   ]
}
```

#### Verwijderen

```
POST /rest/asset/v1/email/{id}/delete.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"169cd#14a1832adba",
   "result":[
      {
         "id":1361
      }
   ]
}
```

## Klonen

Marketo biedt een eenvoudige methode voor het klonen van een e-mail. Dit type verzoek wordt ingediend met een application/x-www-url-urlencoded POST, en neemt twee vereiste parameters, naam, en omslag, een ingebed voorwerp JSON met identiteitskaart en type. beschrijving is ook een optionele parameter . Als er geen goedgekeurde versie bestaat, wordt de conceptversie gekloond.

```
POST /rest/asset/v1/email/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=Clone of Social Sharing in Email&folder={"id":239,"type":"Folder"}&description=This is a test of clone email
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "bd49#15706f43d96",
    "result": [
        {
            "id": 2250,
            "name": "Clone of Social Sharing in Email",
            "description": "This is a test of clone email",
            "createdAt": "2016-09-07T23:20:52Z+0000",
            "updatedAt": "2016-09-07T23:20:52Z+0000",
            "url": "https://app-abm.marketo.com/#EM2250B2",
            "subject": {
                "type": "Text",
                "value": "Hey There"
            },
            "fromName": {
                "type": "Text",
                "value": "SomeBody"
            },
            "fromEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "replyEmail": {
                "type": "Text",
                "value": "somebody@marketo.com"
            },
            "folder": {
                "type": "Folder",
                "value": 239,
                "folderName": "Tradeshows and Events"
            },
            "operational": false,
            "textOnly": false,
            "publishToMSI": false,
            "webView": false,
            "status": "draft",
            "template": 24,
            "workspace": "Default",
            "isOpenTrackingDisabled": false
        }
    ]
}
```

## Voorbeeld verzenden

U kunt een voorbeeld-e-mailbericht activeren via de api, zodat deze naar de queryparameter emailAddress wordt verzonden. U kunt ook optioneel een parameter leadId toevoegen om een bepaalde lead uit uw database na te bootsen en een parameter textOnly om alleen de tekstversie van de e-mail te verzenden.

```
POST /rest/asset/v1/email/{id}/sendSample.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
emailAddress=abe@testmail.com&textOnly=true
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "360b#14cce7d2708",
    "result": [
        {
            "id": 2179
        }
    ]
}
```

## E-mail voorvertonen

Marketo biedt de [Volledige e-mail ophalen](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/getEmailFullContentUsingGET) eindpunt om een levende voorproef van een e-mail terug te winnen aangezien het naar een ontvanger zou worden verzonden. Dit eindpunt kan slechts op Versie 1.0 Emails worden gebruikt. Er is één vereiste parameter, de parameter van de identiteitskaart van de wegparameter die identiteitskaart van de e-mailactiva is u aan voorproef wenst. Er zijn drie extra optionele queryparameters:

- status: accepteert de waarden &quot;concept&quot; of &quot;goedgekeurd&quot; die standaard worden ingesteld op de goedgekeurde versie, indien goedgekeurd, concept indien niet goedgekeurd
- type: accepteert &quot;Text&quot; of &quot;HTML&quot; en heeft standaard de waarde HTML
- leadId:. Accepteert de integer-id van een lead. Wanneer ingesteld, wordt het e-mailbericht voorvertoond alsof het door de opgegeven lead is ontvangen

```
GET /rest/asset/v1/email/{id}/fullContent.json
```

```json
{
   "success": true,
   "warnings": [ ],
   "errors": [ ],
   "requestId": null,
   "result": [
      {
         "id": 339,
         "status": "draft",
         "content": "<!DOCTYPE HTML PUBLIC \"-//W3C//DTD HTML 1.01 Transitional//EN\" \"http://www.w1.org/TR/html4/loose.dtd\">\n<html>\n  <head>\n    <meta http-equiv=\"Content-Type\" content=\"text/html; charset=utf-8\"/>\n    <title></title>\n  </head>\n  <body>\n      <div style=\"font: 14px tahoma; width: 100%\" class=\"mktEditable\" id=\"edit_text_3\">\n        Content from testCreateEmailTemplate2\n    </div>\n  </body>\n</html>"
      }
   ]
}
```

## HTML vervangen

Marketo biedt de [Volledige inhoud e-mailen bijwerken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Emails/operation/createEmailFullContentUsingPOST) eindpunt om de volledige inhoud van een e-mailmiddel te vervangen. Dit eindpunt kan slechts op Versie 1.0 Emails worden gebruikt die de functie UI &quot;uitgeeft Code&quot;hebben gehad die op hen wordt gebruikt, en de verhouding aan hun gebroken oudermalplaatje hebben gehad. Deze API is voornamelijk bedoeld voor gebruik op elementen die zijn gekloond als onderdeel van een programma en kan niet worden gewijzigd met de eindpunten voor de standaardinhoud. E-mails met dynamische inhoud worden niet ondersteund. Als u HTML probeert te vervangen in een e-mailbericht waarin de relatie intact is, wordt een fout geretourneerd.

Dit eindpunt verwacht een Content-Type: multipart/form-data met de id parameter in de weg, identiteitskaart van e-mail, en één parameter in het lichaam, inhoud als volledig HTML e-maildocument met het Content-Type &quot;text/html.&quot;. Een verkeerd geformuleerd HTML-document geeft een waarschuwing weer, maar staat geen goedkeuring toe tijdens het opnemen van JavaScript en/of `<script>`-tags in het document wordt de aanroep mislukt en een fout gegenereerd.

```
POST /rest/asset/v1/email/{id}/fullContent.json
```

```
content-type: multipart/form-data; boundary=--------------------------116301888604800085728247
content-length: 599
```

```html
----------------------------116301888604800085728247
Content-Disposition: form-data; name="content"; filename="email_content.html"
Content-Type: text/html

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 1.01 Transitional//EN" "http://www.w1.org/TR/html4/loose.dtd">
 <html>
 <head>
 <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
 <title></title>
 </head>
 <body>
 <div style="font: 14px tahoma; width: 100%" class="mktEditable" id="edit_text_3">
 EMAIL TEST CONTENT
 </div>
 </body>
 </html>
----------------------------116301888604800085728247--
```

```json
{
   "success": true,
   "warnings": [ ],
   "errors": [ ],
   "requestId": "15dbf#14a1832ae86",
   "result": [
      {
         "id": 1001
      }
   ]
}
```
