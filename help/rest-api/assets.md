---
title: Assets
feature: REST API
description: Overzicht van Marketo Asset REST-API's voor het opvragen van gegevens op id of naam, bladeren met paginering en het maken of bijwerken van mappen, e-mails, formulieren, sjablonen, bestanden, tokens.
exl-id: 4273a5b1-1904-46e8-b583-fc6f46b388d2
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '894'
ht-degree: 0%

---

# Assets

Marketo biedt API&#39;s voor interactie met de meeste marketing- en organisatiemiddelen binnen Marketo.

## Assets

Tot de Marketo-elementen behoren:

- Mappen
- Programma&#39;s
- E-mails
- E-mailsjablonen
- Landingspagina&#39;s
- Landingspagina-sjablonen
- Fragmenten
- Forms
- Tokens
- Bestanden

## API

Voor een volledige lijst van activa API eindpunten, met inbegrip van parameters en modelleringsinformatie, zie de [&#x200B; Verwijzing van het Eindpunt van Activa API &#x200B;](endpoint-reference.md).

## Query

Assets heeft doorgaans drie patronen waarmee ze kunnen worden opgehaald: op id, op naam en door te bladeren.  Door identiteitskaart en door naam zullen allebei één enkel middel voor een bepaalde parameter terugwinnen, terwijl het doorbladeren zal terugkeren en het pagineren door de volledige lijst van activa van dat type toestaan.  Afzonderlijke typen elementen hebben verschillende parameters waarmee ze kunnen worden gefilterd. Zorg er dus voor dat u hun afzonderlijke documenten voor details bekijkt.

In bepaalde gevallen bladert het doorbladereindpunt voor sommige activa types zullen geen kindactiva, zoals de toelaatbare waarden voor een markering terugkeren, en zij moeten individueel worden teruggewonnen gebruikend of door Naam of door het eindpunt van Id om de volledige reeks meta-gegevens terug te keren.  Anderen hebben mogelijk afzonderlijke eindpunten die volledig zijn bedoeld voor het ophalen van afhankelijke objecten, zoals formuliervelden.

### Op id

```
GET /rest/asset/v1/folder/{id}.json?type=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "1241b#14e21ca814a",
    "result": [
        {
            "name": "Social Media",
            "description": null,
            "createdAt": "2011-03-04T17:01:32Z+0000",
            "updatedAt": "2011-03-04T17:01:32Z+0000",
            "url": null,
            "folderId": {
                "id": 341,
                "type": "Folder"
            },
            "folderType": "Email",
            "parent": {
                "id": 11,
                "type": "Folder"
            },
            "path": "/Design Studio/Default/Emails/Social Media",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 341
        }
    ]
}
```

### Op naam

Om technische redenen kunnen de API&#39;s voor bedrijfsmiddelen niet zoeken naar namen met komma&#39;s (,).  U wordt aangeraden om in uw naamgevingsconventie komma&#39;s uit te sluiten voor alle typen elementen.

```
GET /rest/asset/v1/file/byName.json?name=My File
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":null,
   "result":[
      {
         "id":148,
         "size":270313,
         "mimeType":"image/jpeg",
         "url":"http://mlm.devlocal.marketo.com/rs/test/assets/piKLbhVFvW",
         "folder":{
            "type":"Email",
            "id":10614
         },
         "name":"My File",
         "description":null,
         "createdAt":"2014-12-09T22:33:57Z+0000",
         "updatedAt":"2014-12-09T22:33:57Z+0000"
      }
   ]
}
```

### Bladeren

Door elementen te bladeren, zijn altijd twee queryparameters toegestaan:

- offset - Een geheel getal-verschuiving die het resultaat is van de bewerking.
- maxReturn - Beperkt het aantal geretourneerde records.  De standaardwaarde is 20 als de waarde niet is ingesteld en heeft een maximum van 200.

```
GET /rest/asset/v1/emailTemplates.json?offset=10&maxReturn=50
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"33c4#14a1832b4a8",
   "result":[
      {
         "id":18,
         "name":"AAA0unit3CreateTestEmailTemplateName.2314673e-7bc2-47da-a1e8-66dfdd8a1f1d",
         "description":"AssetAPI: getTemplates test",
         "createdAt":"2014-11-03T19:52:58Z+0000",
         "updatedAt":"2014-11-03T19:52:58Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      },
      {
         "id":177,
         "name":"ABfRGutnwN",
         "description":"HMmHkdTRrGaRpPakdgGKICxfMunCEWDUWiThgAbInfaBXxGxSFfjKQIwerngCHRlGTnAJhKPmwlXLcsjGPtWEiILGyeIJTNVHoHg",
         "createdAt":"2014-11-20T19:31:06Z+0000",
         "updatedAt":"2014-11-20T19:31:06Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      },
      {
         "id":148,
         "name":"ADVHJBQLyw",
         "description":null,
         "createdAt":"2014-11-20T06:42:57Z+0000",
         "updatedAt":"2014-11-20T06:42:57Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      }
   ]
}
```

## Maken en bijwerken

Voor eenvoudige activa types zoals Omslagen, Tokens en Dossiers is er typisch slechts één enkel eindpunt voor verwezenlijking, en dan een extra eindpunt voor het bijwerken van verslagen door identiteitskaart.  Assets wordt gemaakt met een naam die altijd verplicht is. Eventuele metagegevens en id&#39;s worden geretourneerd door het antwoord voor het maken of bijwerken.

Hier ziet u bijvoorbeeld hoe u een token kunt maken:

```
POST /rest/asset/v1/folder/{id}/tokens.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=April Fools&value=2015-04-01&type=date&folderType=Folder
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "e3c2#14e280db5dc",
    "result": [
        {
            "folder": {
                "type": "Folder",
                "value": 416
            },
            "tokens": [
                {
                    "name": "April Fools",
                    "type": "date",
                    "value": "2015-04-01",
                    "computedUrl": "https://app-abm.marketo.com/#MF1047C3"
                }
            ]
        }
    ]
}
```

Als u een map wilt bijwerken, doet u het volgende:

```
POST /rest/asset/v1/folder/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
type=Folder&description=This is a test (update 01)
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "c5b2#14e1f3954bf",
    "result": [
        {
            "name": "Learning - deverly",
            "description": "This is a test (update 01)",
            "createdAt": "2015-03-17T00:17:02Z+0000",
            "updatedAt": "2015-06-23T07:02:07Z+0000",
            "url": "https://app-abm.marketo.com/#MF1044A1",
            "folderId": {
                "id": 407,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 15,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Learning - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 407
        }
    ]
}
```

Andere activa hebben complexere structuren en vereisen updates aan extra subsecties of kindvoorwerpen, en dan uiteindelijk moeten goedkeuring worden onderworpen alvorens zij in gebruik kunnen worden genomen.  Dit zijn onder andere Forms, e-mails, e-mailsjablonen, bestemmingspagina&#39;s en landingssjablonen.  Deze zullen elk één enkel eindpunt voor het creëren van een verslag hebben, dan extra eindpunten voor het bijwerken van meta-gegevens, inhoud, en inhoudssecties.

Bijvoorbeeld, om een het Bestaan Pagina tot stand te brengen, zult u zijn creeer eindpunt met een malplaatjeidentiteitskaart moeten roepen en dan zijn inhoudssecties terugwinnen, en elk individueel bijwerken om inhoud toe te voegen, alvorens het goed te keuren zodat het levend kan worden opgesteld.

### Complex maken

Voor het starten van pagina&#39;s moet eerst een element voor de bestemmingspagina worden gemaakt met behulp van een bovenliggende sjabloon.  Hiermee maakt u een nieuwe bestemmingspagina met de standaardinhoud van de sjabloon voor elke inhoudsectie.

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

#### Secties ophalen

Als u de inhoud voor een bestemmingspagina wilt vullen, moet u de lijst met inhoudssecties ophalen en vervolgens afzonderlijke updates uitvoeren voor elke sectie die afwijkt van de sjabloon.

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

#### Sectie bijwerken

```
POST /rest/asset/v1/landingPage/{id}/content/{contentId}.json?type=Form&value=1
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "5c37#154ea32cf11",
    "result": [
        {
            "id": 174
        }
    ]
}
```

## Goedkeuring

Veel elementtypen hebben een gekoppeld concept- en goedkeuringssysteem, zoals e-mails, bestemmingspagina&#39;s, fragmenten, Forms en de bijbehorende sjablonen.  Wanneer u een element probeert goed te keuren, wordt dit beoordeeld aan de hand van een specifieke set validatieregels. Vervolgens stelt u het element in op een goedgekeurde status of retourneert u een mislukkingsreden.  Voor deze typen elementen worden, telkens wanneer de inhoud van een bepaald element wordt bijgewerkt, wijzigingen aangebracht in een concept van het element, dat geen invloed heeft op de goedgekeurde versie.  Hierdoor kunnen wijzigingen in de inhoud veilig worden aangebracht zonder dat dit van invloed is op live versies van het element.  De veranderingen kunnen dan op de levende versie worden toegepast door het goedkeuringseindpunt te gebruiken.  Hierdoor wordt ook de conceptstatus van het element gewist totdat aanvullende updates worden toegepast.

```
POST /rest/asset/v1/emailTemplate/{id}/approveDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"abe2#14a1832a97d",
   "result":[
      {
         "id":338,
         "name":"lvAVYMZqPS",
         "description":"fZLJQSJRvnYbjGTUpIHHqDOuQgQzXQcWIXoOUPwrVLdMHKcbRqwLoSLkWZTUmaMiCIJSfQiufnnrgITUIqjuAPBLpmliiKuIUFYG",
         "createdAt":"2014-12-05T02:06:21Z+0000",
         "updatedAt":"2014-12-05T02:06:21Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Approved",
         "workspace":"Default"
      }
   ]
}
```

De geslaagde goedkeuring vervangt de vorige live versie door de bijgewerkte versie.

Concepten negeren is ook beschikbaar via een eindpunt voor elk geldig type element.  Als u deze optie gebruikt voor een element dat zich in een goedgekeurde conceptstatus bevindt, verwijdert u het huidige concept en eventuele wijzigingen die in behandeling zijn.  Als u deze optie gebruikt op een element dat momenteel geen goedgekeurde versie heeft, gebeurt er niets en wordt een fout geretourneerd.  Elementen met alleen een concept kunnen worden verwijderd, maar ze mogen niet worden verwijderd.

```
POST /rest/asset/v1/emailTemplate/{id}/discardDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"17bfa#14a1832b3c4",
   "result":[
      {
         "id":344,
         "name":"LkilkvKrkp",
         "description":"yAyUEXuWMtdhpODUmnCkGjpBcyEKnYucxaSoTyYeQzyNbYanxCXWPOzwiIWmeXPUwjfGAUmgnxlhgOPluVqwNittuvxJmNTaHxYM",
         "createdAt":"2014-12-05T02:06:23Z+0000",
         "updatedAt":"2014-12-05T02:06:23Z+0000",
         "folder":{
            "type":"Folder",
            "value":15
         },
         "status":"Draft",
         "workspace":"Default"
      }
   ]
}
```

Assets kan ook niet worden goedgekeurd als de status alleen is goedgekeurd.  Hierdoor worden eventuele live versies van het element verwijderd en wordt het element geretourneerd naar een status met alleen concept, terwijl alle gekoppelde concepten worden verwijderd.  Deze handeling kan alleen worden uitgevoerd op de meeste middelen als deze nergens in Marketo wordt gebruikt, zoals een e-mail waarnaar wordt verwezen in een stap E-mail verzenden of een fragment dat wordt ingesloten in een e-mail.

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

## Verwijderen

Assets met goedkeuring en ontwerpstaten, met uitzondering van formulieren, mag niet worden verwijderd tijdens de goedkeuring en moet vóór verwijdering niet worden goedgekeurd.  Verwijderingen kunnen doorgaans alleen worden uitgevoerd wanneer een element niet is goedgekeurd en buiten gebruik is en in het geval van mappen leeg is van elementen.  Een opmerkelijke uitzondering vormen programma&#39;s, die samen met alle onderliggende inhoud kunnen worden verwijderd, zolang het programma en de inhoud ervan nergens buiten de grenzen van het programma worden gebruikt.

```
POST /rest/asset/v1/program/{id}/delete.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "16501#14db042c6b7",
    "result": [
        {
            "id": 1109
        }
    ]
}
```

## Tijdstippen

API&#39;s voor middelen hebben een time-out van 300 seconden
