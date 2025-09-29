---
title: Bestanden
feature: REST API
description: Gids aan Marketo REST API dossiervraag door identiteitskaart of naam, doorblader met omslag en compensatie, creeer of update via multipart uploaden, insertOnly, MIME types, geen stromen
exl-id: 17361cdc-2309-442c-803c-34ce187aee1a
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '289'
ht-degree: 0%

---

# Bestanden

[ Verwijzing van het Eindpunt van dossiers 0&rbrace;](https://developer.adobe.com/marketo-apis/api/asset/#tag/Files)

Met Marketo-abonnementen kunt u willekeurige bestanden opslaan, zoals afbeeldingen, scripts, documenten en stijlpagina&#39;s. U kunt deze allemaal op afstand bewerken via de REST API. De opslag die beschikbaar is in Marketo-abonnementen is niet geoptimaliseerd voor toepassingen met veel bandbreedte. Er moeten dus alternatieven worden gebruikt voor correcte audio- en videostreaming toepassingen.

## Query

Het vragen van dossiers is eenvoudig en volgt de standaardvraagtypes voor activa van [ door identiteitskaart ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Files/operation/getFileByIdUsingGET), [ door naam ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Files/operation/getFileByNameUsingGET), en [ doorbladerend ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Files/operation/getFilesUsingGET).

### Op id

```
GET /rest/asset/v1/file/{id}.json
```

```json
{
   "success":true,
   "warnings":[ ],
   "errors":[ ],
   "requestId":null,
   "result":[
      {
         "id":147,
         "size":61346,
         "mimeType":"image/jpeg",
         "url":"http://mlm.devlocal.marketo.com/rs/test/assets/rYXNeQFFVu",
         "folder":{
            "type":"Email",
            "id":10613
         },
         "name":"rYXNeQFFVu",
         "description":null,
         "createdAt":"2014-12-09T22:33:57Z+0000",
         "updatedAt":"2014-12-09T22:33:57Z+0000"
      }
   ]
}
```

### Op naam

Geef de naam van het bestand op met behulp van de parameter `name` .

```
GET /rest/asset/v1/file/byName.json?name=foo.png
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "9049#15918a76619",
    "result": [
        {
            "id": 46488,
            "size": 13987,
            "mimeType": "image/png",
            "url": "http://na-abm.marketo.com/rs/mktodemoaccount88/assets/foo.png",
            "folder": {
                "type": "Image",
                "id": 436,
                "name": "My Images"
            },
            "name": "foo.png",
            "description": "This is a test file.",
            "createdAt": "2015-05-06T22:16:58Z+0000",
            "updatedAt": "2015-05-06T22:19:29Z+0000"
        }
    ]
}
```

### Bladeren

Er zijn drie optionele parameters:

- map - bovenliggende map die is opgegeven als JSON-blok met kenmerken &quot;id&quot; en &quot;type&quot;
- offset - geheel getal dat opgeeft waar moet worden begonnen met ophalen van items (standaardwaarde is 0); kan worden gebruikt met parameter maxReturn
- maxReturn - geheel getal dat het maximale aantal te retourneren items aangeeft (standaardwaarde is 20, maximum 200)

```
GET /rest/asset/v1/files.json?folder={"id":436, "type": "Folder"}&maxReturn=3
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "17e4e#14e23372d80",
    "result": [
        {
            "id": 46484,
            "size": 1454,
            "mimeType": "text/plain",
            "url": "http://na-abm.marketo.com/rs/mktodemoaccount88/assets/websites.png",
            "folder": {
                "type": "Image",
                "id": 436,
                "name": "My Images - deverly"
            },
            "name": "websites.png",
            "description": "This is a test file.",
            "createdAt": "2015-05-06T20:15:58Z+0000",
            "updatedAt": "2015-06-22T02:12:36Z+0000"
        },
        {
            "id": 46486,
            "size": 4169,
            "mimeType": "image/png",
            "url": "http://na-abm.marketo.com/rs/mktodemoaccount88/assets/mobile.png",
            "folder": {
                "type": "Image",
                "id": 436,
                "name": "My Images - deverly"
            },
            "name": "mobile.png",
            "description": null,
            "createdAt": "2015-05-06T22:13:33Z+0000",
            "updatedAt": "2015-05-06T22:13:33Z+0000"
        },
        {
            "id": 46488,
            "size": 13987,
            "mimeType": "image/png",
            "url": "http://na-abm.marketo.com/rs/mktodemoaccount88/assets/foo.png",
            "folder": {
                "type": "Image",
                "id": 436,
                "name": "My Images - deverly"
            },
            "name": "foo.png",
            "description": "This is a test file.",
            "createdAt": "2015-05-06T22:16:58Z+0000",
            "updatedAt": "2015-05-06T22:19:29Z+0000"
        }
    ]
}
```

## Maken en bijwerken

[ Creërend een dossier ](https://developer.adobe.com/marketo-apis/api/asset/#tag/Files/operation/createFileUsingPOST) wordt gedaan met een multipart/form-gegeven type van verzoek. Minimaal, worden de naam, de omslag, en het dossier vereist in het verzoek, met een facultatieve beschrijving, en een insertOnly vlag, die een creeer vraag verhindert een bestaand dossier met de zelfde naam bij te werken. Voor de bestandsparameter is naast de parameter name een bestandsnaam vereist in de header Content-Disposition. U moet ook een Content-Type-header voor het bestand doorgeven. Dit is het MIME-type dat Marketo gebruikt om het bestand mee te dienen.

```
POST /rest/asset/v1/files.json
```

```
------WebKitFormBoundary2VyWOacQSupl4gUL
Content-Disposition: form-data; name="file"; filename="marketo.html"
Content-Type: text/html
<html>
<body>
<h1>Test Page - marketo.html</h1>
</body>
</html>
------WebKitFormBoundary2VyWOacQSupl4gUL
Content-Disposition: form-data; name="name"
marketo.html
------WebKitFormBoundary2VyWOacQSupl4gUL
Content-Disposition: form-data; name="folder"
{"id":436,"type":"Folder"}
------WebKitFormBoundary2VyWOacQSupl4gUL
Content-Disposition: form-data; name="description"
This is a test file
------WebKitFormBoundary2VyWOacQSupl4gUL—
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "278d#14e23316f63",
    "result": [
        {
            "id": 46960,
            "size": 69,
            "mimeType": "text/html",
            "url": "http://na-abm.marketo.com/rs/mktodemoaccount88/assets/marketo.html",
            "folder": {
                "type": "Image",
                "id": 436,
                "name": "My Images - deverly"
            },
            "name": "marketo.html",
            "description": "This is a test file",
            "createdAt": "2015-06-24T01:31:59Z+0000",
            "updatedAt": "2015-06-24T01:31:59Z+0000"
        }
    ]
}
```

[ die een dossier ](https://developer.adobe.com/marketo-apis/api/asset/#tag/File-Contents/operation/updateContentUsingPOST) bijwerken kan op zijn identiteitskaart worden gedaan gebaseerd. De enige parameter is een bestandsparameter die dezelfde vereisten heeft als het maken.

```
POST /rest/asset/v1/file/{id}/content.json
```

```
------WebKitFormBoundary2VyWOacQSupl4gUL
Content-Disposition: form-data; name="file"; filename="marketo.html"
Content-Type: text/html
<html>
<body>
<h1>Test Page - marketo.html</h1>
</body>
</html>
------WebKitFormBoundary2VyWOacQSupl4gUL--
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": null,
    "result": [
        {
            "id": 67,
            "size": 512000,
            "mimeType": "image/png",
            "url": "http://pages.devlocal.marketo.com/rs/test/assets/aLZiwCkXor",
            "folder": {
                "type": "Email",
                "id": 10391
            },
            "name": "aLZiwCkXor",
            "description": null,
            "createdAt": "2014-12-18T09:03:43Z+0000",
            "updatedAt": "2015-01-07T04:40:20Z+0000"
        }
    ]
}
```
