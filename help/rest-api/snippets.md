---
title: Fragmenten
feature: REST API, Snippets
description: Marketo Asset REST API voor fragmenten, met inbegrip van vraag door Identiteitskaart en doorblader met status, het krijgen van inhoud, het creëren en het bijwerken van HTML, Tekst, en dynamische inhoud.
exl-id: 87901c29-ee59-4224-848d-3bd6a6c52718
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '456'
ht-degree: 0%

---

# Fragmenten

[ Verwijzing van het Eindpunt van het Fragment ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Snippets)

Fragmenten zijn herbruikbare HTML-componenten die kunnen worden ingesloten in e-mails en bestemmingspagina&#39;s en die kunnen worden gesegmenteerd voor dynamische inhoud. Fragmenten hebben geen bijbehorende sjablonen en kunnen binnen andere elementen in Marketo worden gemaakt en geïmplementeerd.

## Query

Querying-fragmenten volgen het standaardpatroon voor elementen, maar hebben geen Op naam-methode. Zowel staan 0} door Identiteitskaart [ en ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Snippets/operation/getSnippetByIdUsingGET) doorbladert [ methodes het gebruik van het statusgebied toe om of goedgekeurde of ontwerp versies van het fragment terug te winnen.](https://developer.adobe.com/marketo-apis/api/asset/#tag/Snippets/operation/getSnippetUsingGET)

### Op id

```
GET /rest/asset/v1/snippet/{id}.json?status=approved
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"fa0f#14b04375f0a",
   "result":[
      {
         "id":83,
         "name":"BYkHVJEedl",
         "description":"yzSLvNFyrmeVmyLzqryUfGlDOJTnvyyfsQTXPDCGdCwcWUlfoCNApUqYgwZGElrUFoxBHJcMdXdqTKvtjtfsmPgokyRgVLeHyJCw",
         "createdAt":"2015-01-19T22:01:52Z+0000",
         "updatedAt":"2015-01-19T22:01:52Z+0000",
         "folder":{
            "type":"Folder",
            "value":662
         },
         "status":"approved",
         "workspace":"Default"
      }
   ]
}
```

### Bladeren

```
GET /rest/asset/v1/snippets.json?maxReturn=3
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"f9cc#14b04376181",
   "result":[
      {
         "id":23,
         "name":"ADJvMLBMpS",
         "description":"XkzFUVLXVHrojLGLJVLPpwguOXuDvhAqaaSkBUVzgHrgDhqqRzyXlULIXSHJvfBHjCSaMwjyEdrdxcjFCRoNFVvdBBTDfSrUJzaR",
         "createdAt":"2015-01-15T20:10:39Z+0000",
         "updatedAt":"2015-01-15T20:10:39Z+0000",
         "url": null,
         "folder":{
            "type":"Folder",
            "value":620,
            "folderName": "Snippets"
         },
         "status":"draft",
         "workspace":"Default"
      },
      {
         "id":46,
         "name":"Biswa Snippet",
         "description":"",
         "createdAt":"2015-01-16T05:18:55Z+0000",
         "updatedAt":"2015-01-16T05:19:27Z+0000",
         "url": null,
         "folder":{
            "type":"Folder",
            "value":630,
            "folderName": "Snippets"
         },
         "status":"draft",
         "workspace":"Default"
      },
      {
         "id":12,
         "name":"dJJQkKbUYq",
         "description":"VXuHkYMREHrhxUSgYbKfaNeLisdFxOromCXQNrgmModvkuoyZdQjtAbXxDUbBvoDVCZmAVbasiHyWoWfTwgrGxnzpKepGrAUvfen",
         "createdAt":"2015-01-15T05:12:33Z+0000",
         "updatedAt":"2015-01-15T05:12:33Z+0000",
         "url": null,
         "folder":{
            "type":"Folder",
            "value":615,
            "folderName": "Snippets"
         },
         "status":"draft",
         "workspace":"Default"
      }
   ]
}
```

## Query-inhoud

De inhoud van een bepaald fragment kan worden opgehaald op basis van de fragment-id.

```
GET /rest/asset/v1/snippet/{id}/content.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"5c50#14b04376159",
   "result":[
      {
         "type":"HTML",
         "content":"draft testUpdateSnippetContent1 HTML Content"
      },
      {
         "type":"Text",
         "content":"draft testUpdateSnippetContent1 Text Content"
      }
   ]
}
```

De vraag keert een lijst van inhoudssecties terug,  die bestaan uit secties van het type HTML of DynamicContent, en optioneel een sectie met een type tekst.

## Maken en bijwerken

De fragmenten volgen het complexe patroon van de activaverwezenlijking, waar de vraag [ fragment ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Snippets/operation/createSnippetUsingPOST) creeert, en zijn inhoud afzonderlijk wordt gemaakt, zodat moet de eerste vraag aan creëren eindpunt, met een facultatieve beschrijving zijn.   Gegevens worden doorgegeven als x-www-form-urlencoded, niet als JSON.

```
POST /rest/asset/v1/snippets.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=Test Snippet 09 - deverly&folder={"id":395,"type":"Folder"}&description=This is a test snippet
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "bd57#14e231ee3a1",
    "result": [
        {
            "id": 13,
            "name": "Test Snippet 09 - deverly",
            "description": "This is a test snippet",
            "createdAt": "2015-06-24T01:11:43Z+0000",
            "updatedAt": "2015-06-24T01:11:43Z+0000",
            "url": "https://app-abm.marketo.com/#SN13B2ZN395",
            "folder": {
                "type": "Folder",
                "value": 395,
                "folderName": "Snippets"
            },
            "status": "draft",
            "workspace": "Default"
        }
    ]
}
```

Het toevoegen of vervangen van inhoud in een fragment wordt uitgevoerd door id. De inhoud kan van het type Text, HTML of DynamicContent zijn. Als het type Tekst is, dan is de inhoudsparameter gewone teksteindpunt, terwijl als het HTML is, dan is het de gewenste prijsverhogingstekst. Als het type is ingesteld op DynamicContent, moet de inhoudsparameter worden ingesteld op de id van de segmentatie die aan het fragment moet worden gekoppeld.

```
POST /rest/asset/v1/snippet/{id}/content.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
type=HTML&content=draft testUpdateSnippetContent1 HTML Content
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"73d9#14b04376139",
   "result":[
      {
         "id":82
      }
   ]
}
```

[ het Bijwerken van meta-gegevens ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Snippets/operation/updateSnippetUsingPOST) wordt ook gedaan door identiteitskaart. Alleen naam en beschrijving kunnen worden bijgewerkt:

```
POST /rest/asset/v1/snippet/{id}.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=Test Snippet&description=New Description
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"9ad0#14b043762b1",
   "result":[
      {
         "id":82,
         "name":"Test Snippet",
         "description":"New Description",
         "createdAt":"2015-01-19T22:01:52Z+0000",
         "updatedAt":"2015-01-19T22:01:53Z+0000",
         "url": "https://app-abm.marketo.com/#SN3B2ZN395",
         "folder":{
            "type":"Folder",
            "value":662,
            "folderName": "Snippets"
         },
         "status":"draft",
         "workspace":"Default"
      }
   ]
}
```

## Dynamische inhoud

Fragmenten volgen het standaardpatroon voor dynamische inhoud, maar vertegenwoordigen slechts één volledige inhoudssectie op zich, zodat elk fragment slechts één dynamische sectie kan bevatten, met een lijst van naar keuze interne secties voor elk segment in de gebruikte segmentatie. Dynamische inhoud kan alleen door snippet-id worden opgevraagd, aangezien er mogelijk slechts één dynamische inhoudssectie in een fragment is.

```
GET /rest/asset/v1/snippet/{id}/dynamicContent.json
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "ae3#14c2b499111",
    "result": [
        {
            "createdAt": "2015-03-13T06:24:35Z+0000",
            "updatedAt": "2015-03-17T20:29:42Z+0000",
            "id": 70,
            "segmentation": 1001,
            "content": [
                {
                    "id": "Nzk*",
                    "segmentId": 1001,
                    "segmentName": "Area",
                    "content": "Sample HTML for Area",
                    "type": "HTML"
                },
                {
                    "id": "Nzk*",
                    "segmentId": 1001,
                    "segmentName": "Area",
                    "content": "Sample Text for Area",
                    "type": "Text"
                },
                {
                    "id": "Nzk*",
                    "segmentId": 1002,
                    "segmentName": "Default",
                    "content": "Sample HTML for Default",
                    "type": "HTML"
                },
                {
                    "id": "Nzk*",
                    "segmentId": 1002,
                    "segmentName": "Default",
                    "content": "Sample Text for Default",
                    "type": "Text"
                }
            ]
        }
    ]
}
```

## Goedkeuring

Fragmenten beschikken over eindpunten voor het goedkeuren, niet goedkeuren en verwijderen van concepten, die het standaardmiddelenpatroon volgen. Een fragment moet de status concept hebben voordat het kan worden goedgekeurd.

### Goedkeuren

```
POST /rest/asset/v1/snippet/{id}/approveDraft.json
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "11903#14db1af2f6c",
    "result": [
        {
            "id": 3,
            "name": "Test Snippet 02 - deverly",
            "description": "hey this is a test snippet!",
            "createdAt": "2015-06-02T00:32:37Z+0000",
            "updatedAt": "2015-06-02T00:32:37Z+0000",
            "url": "https://app-abm.marketo.com/#SN3B2ZN395",
            "folder": {
                "type": "Folder",
                "value": 395,
                "folderName": "Snippets"
            },
            "status": "approved",
            "workspace": "Default"
        }
    ]
}
```

### Niet goedkeuren

Het eindpunt `unapprove` kan alleen worden gebruikt voor goedgekeurde fragmenten.

```
POST /rest/asset/v1/snippet/{id}/unapprove.json
```

```json
{
    "success": true,
    "warnings": [ ],
    "errors": [ ],
    "requestId": "7d20#14db1c7a2a9",
    "result": [
        {
            "id": 89,
            "name": "Test Snippet 01 - deverly",
            "description": "",
            "createdAt": "2015-05-15T19:01:22Z+0000",
            "updatedAt": "2015-05-15T19:07:07Z+0000",
            "url": "https://app-abm.marketo.com/#SN1B2ZN395",
            "folder": {
                "type": "Folder",
                "value": 395,
                "folderName": "Snippets"
            },
            "status": "draft",
            "workspace": "Default"
        }
    ]
}
```

### Concept verwijderen

Het fragment moet zich in de conceptstatus bevinden om te worden verwijderd.  Een goedgekeurd fragment kan niet worden verwijderd.

```
POST /rest/asset/v1/snippet/{id}/discardDraft.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":"674c#14b043760de",
   "result":[
      {
         "id":88
      }
   ]
}
```

## Klonen

[ het Klonen van een fragment ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Snippets/operation/cloneSnippetUsingPOST) met API is eenvoudig en volgt het standaardpatroon, met een vereiste naam, identiteitskaart van het originele fragment en de omslag, evenals een facultatieve beschrijving.  Als er geen goedgekeurde versie bestaat, wordt de conceptversie gekloond.

```
POST /rest/asset/v1/snippet/{id}/clone.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
name=Test Snippet Clone 3 - deverly&folder={"id":395,"type":"Folder"}&description=This is a test snippet
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "21c9#14e2327e33d",
    "result": [
        {
            "id": 14,
            "name": "Test Snippet Clone 3 - deverly",
            "description": "This is a test snippet",
            "createdAt": "2015-06-24T01:21:33Z+0000",
            "updatedAt": "2015-06-24T01:21:33Z+0000",
            "url": "https://app-abm.marketo.com/#SN14B2ZN395",
            "folder": {
                "type": "Folder",
                "value": 395,
                "folderName": "Snippets"
            },
            "status": "draft",
            "workspace": "Default"
        }
    ]
}
```
