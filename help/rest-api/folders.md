---
title: "Mappen"
feature: REST API
description: "Mappen bewerken met de Marketo API."
source-git-commit: 8c1ffb6db05da49e7377b8345eeb30472ad9b78b
workflow-type: tm+mt
source-wordcount: '1008'
ht-degree: 0%

---


# Mappen

[Mappen eindpuntverwijzing](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders)

Mappen vormen het kernelement van de organisatie in Marketo en elk ander type element heeft minstens één map als bovenliggend element. Deze bovenliggende map kan een map zijn die louter organisatorisch is, of een programma dat een functionele relatie heeft met andere elementtypen en ook de bovenliggende map van andere elementen kan zijn. Mappen kunnen worden gemaakt, opgehaald, bijgewerkt en verwijderd via de API en er kan ook een lijst met de inhoud van mappen worden opgehaald. Hoewel programma&#39;s kunnen worden geretourneerd via een query op de API voor mappen, moeten het maken, bijwerken en verwijderen van programma&#39;s worden uitgevoerd via de API voor programma&#39;s.

## Query

Mappen vragen volgt de standaardquerytypen voor elementen van [door id](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByIdUsingGET), [op naam](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET), en [bladeren](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderUsingGET).

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

De parameter type is vereist en moet een map of programma zijn.  Het type dicteert of de raadpleging aan de omslag tegen een identiteitskaart van de Omslag of een identiteitskaart van het Programma wordt gedaan. Voor dit eindpunt, slechts is één enkel verslag teruggekeerd in de resultaatSerie. Noteer de parameter folderType in het antwoord. Dit kan op vele verschillende soorten omslagen wijzen. De omslagen van de Activiteiten van Marketo hebben of een type van de Omslag van de Marketing of Programma, dat vele verschillende types van activa kan bevatten, terwijl de omslagen van Design Studio een type hebben dat aan het activatype beantwoordt dat zij kunnen houden. Een map met het maptype &quot;E-mail&quot; kan bijvoorbeeld alleen e-mails of andere submappen bevatten, die een mapType e-mail- of e-mailsjabloon kunnen hebben. Typen kunnen zijn:

- E-mail
- E-mailsjabloon
- Openingspagina
- Landingspagina-sjabloon
- Fragment
- Bestand

### Op naam

[Vragen op naam](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET) is ook toegestaan. De vraag door naameindpunt heeft naam als enige vereiste parameter. De naam voert een nauwkeurige koordgelijke tegen het naamgebied van omslagen in de instantie uit, en keert resultaten voor elke omslag terug die die naam aanpassen. Het heeft ook de facultatieve vraagparameters van &quot;type&quot;die Omslag of Programma kunnen zijn, &quot;wortel&quot;identiteitskaart van de omslag om te zoeken door, of &quot;werkruimte&quot;de naam van de werkruimte om binnen te zoeken. Als de hoofdparameter is ingesteld, moet de parameter type ook worden ingesteld.

```
GET /rest/asset/v1/folder/byName.json?name=Test%2010%20-%20deverly
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "19#14e1f2f3688",
    "result": [
        {
            "name": "Test 10 - deverly",
            "description": "This is a test",
            "createdAt": "2015-06-23T06:27:04Z+0000",
            "updatedAt": "2015-06-23T06:27:04Z+0000",
            "url": "https://app-abm.marketo.com/#MF1070A1",
            "folderId": {
                "id": 454,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 416,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Marketing Programs - deverly/Test 10 - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 454
        }
    ]
}
```

Wanneer het zoeken door naam, is het belangrijk om op te merken dat zowel de Activiteiten van de Marketing als de Studio van het Ontwerp hun eigen wortelomslagen zijn, zodat kunnen zij door naam worden teruggewonnen, en worden gebruikt om de rest van de omslaghiërarchie in een bestemmingsinstantie over te steken.

### Bladeren

Mappen kunnen ook [bulksgewijs opgehaald](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderUsingGET). De &quot;wortel&quot;parameter kan worden gebruikt om de ouderomslag te specificeren waaronder de vraag zal worden uitgevoerd en geformatteerd als voorwerp JSON ingebed als waarde voor de vraagparameter. Hoofdmap heeft twee leden:

1. id - De id van de map of het programma.
1. type - Map of Programma, afhankelijk van het type hoofdmap voor de browser.

Als de wortelomslag niet gekend is, of de bedoeling is alle omslagen in een bepaald gebied terug te winnen, kan de wortel als &quot;de Activiteiten van de Marketing&quot;, &quot;de Studio van het Ontwerp&quot;, of de gebieden van het Gegevensbestand van de Lood worden gespecificeerd. De id&#39;s voor elk van deze kunnen worden opgehaald via de [Map ophalen op naam](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/getFolderByNameUsingGET) API, en het specificeren van de naam van het gewenste gebied.

Net als andere eindpunten voor het ophalen van bulkmiddelen, zijn offset en maxReturn optionele parameters voor pagineren.   Andere optionele parameters zijn:

- workSpace - De naam van de werkruimte waarnaar moet worden gefilterd.
- maxDepth - Het maximumaantal niveaus dat in de maphiërarchie kan worden doorlopen. Indien ingesteld op 0, wordt alleen de in de hoofdmap opgegeven map geretourneerd. Indien niet opgegeven, is de standaardwaarde 2.

```
GET /rest/asset/v1/folders.json?root={"id":14,"type":"Folder"}
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "9bd8#14e1f49047c",
    "result": [
        {
            "name": "Marketing Activities",
            "description": "Root node for the Marketing Activities app area",
            "createdAt": "2010-03-27T18:27:45Z+0000",
            "updatedAt": "2010-03-27T18:27:45Z+0000",
            "url": null,
            "folderId": {
                "id": 14,
                "type": "Folder"
            },
            "folderType": "Zone",
            "parent": null,
            "path": "/Marketing Activities",
            "isArchive": false,
            "isSystem": true,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 14
        },
        {
            "name": "Default",
            "description": "Root node of the Marketing activities Default",
            "createdAt": "2010-03-27T18:27:45Z+0000",
            "updatedAt": "2010-03-27T18:27:45Z+0000",
            "url": null,
            "folderId": {
                "id": 15,
                "type": "Folder"
            },
            "folderType": "Zone",
            "parent": {
                "id": 14,
                "type": "Folder"
            },
            "path": "/Marketing Activities/Default",
            "isArchive": false,
            "isSystem": true,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 15
        },
        {
            "name": "Archive",
            "description": "",
            "createdAt": "2010-03-27T18:28:17Z+0000",
            "updatedAt": "2010-03-27T18:28:17Z+0000",
            "url": "https://app-abm.marketo.com/#MF157A1",
            "folderId": {
                "id": 310,
                "type": "Folder"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 15,
                "type": "Folder"
            },
            "path": "/Marketing Activities/Default/Archive",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 310
        }
    ]
}
```

## Responsstructuur

Veel van de structuur van de omslagreactie spreekt voor zich, maar een paar gebieden zijn het waard om individueel te noteren. De `folderId` en bovenliggende velden zijn JSON-objecten die de expliciete id en het type map zelf bevatten. Dit type is het type dat door de API wordt gebruikt in query&#39;s, hoofdparameters en bovenliggende parameters voor een juiste afbakening tussen de mappen Map en Programmatypen. `folderType` geeft het gebruik weer van de map, die kan bestaan uit &quot;Marketing Folder&quot;, &quot;Program&quot;, &quot;Email&quot;, &quot;Email Template&quot;, &quot;Landing Page&quot;, &quot;Landing Page Template&quot;, &quot;Snippet&quot;, &quot;Image&quot;, &quot;Zone&quot; of &quot;File&quot;.  De types de Omslag van de Marketing en Programma wijzen erop dat zij in de Activiteiten van de Marketing bestaan en kunnen veelvoudige types van activa bevatten. De andere typen geven aan dat zij alleen dat type element, submappen en, indien van toepassing, de sjabloonversie van dat type mogen bevatten. Het typeZone vertegenwoordigt de mappen op hoofdniveau die worden aangetroffen in marketingactiviteiten.

Het pad van een map toont de hiërarchie in de mapstructuur, net als bij een Unix-pad. De eerste ingang in de weg zal altijd de Activiteiten van de Marketing of Studio van het Ontwerp zijn. Als de doelinstantie werkruimten heeft, is de tweede vermelding in het pad de naam van de werkruimte die in eigendom is. De `url` in het veld wordt de expliciete URL van het element in de opgegeven instantie weergegeven. Dit is geen universele koppeling en moet als een gebruiker worden geverifieerd om correct te werken. `isSystem` Hiermee wordt aangegeven of de map een systeemmap is. Als deze waarde is ingesteld op true, is de map zelf alleen-lezen, hoewel mappen kunnen worden gemaakt als onderliggende mappen.

## Maken en bijwerken

[Mappen maken](https://developer.adobe.com/marketo-apis/api/asset/#tag/Folders/operation/createFolderUsingPOST) is eenvoudig en wordt uitgevoerd met een POST application/x-www-form-urlencoded die twee vereiste parameters heeft, &quot;name,&quot;een koord, en &quot;ouder,&quot;de ouder om de omslag in tot stand te brengen, die een ingebed voorwerp JSON met twee leden, identiteitskaart, en type, of Omslag of Programma, afhankelijk van het type van de doelomslag is. Optioneel kan &quot;description&quot; (een tekenreeks) ook worden opgenomen en maximaal 2000 tekens bevatten.

```
POST /rest/asset/v1/folders.json
```

```
Content-Type: application/x-www-form-urlencoded
```

```
parent={"id":416,"type":"Folder"}&name=Test 10 - deverly&description=This is a test
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "111be#14e1f193e31",
    "result": [
        {
            "name": "Test 10 - deverly",
            "description": "This is a test",
            "createdAt": "2015-06-23T06:27:04Z+0000",
            "updatedAt": "2015-06-23T06:27:04Z+0000",
            "url": "https://app-abm.marketo.com/#MF1070A1",
            "folderId": {
                "id": 454,
                "type": "FOLDER"
            },
            "folderType": "Marketing Folder",
            "parent": {
                "id": 416,
                "type": "FOLDER"
            },
            "path": "/Marketing Activities/Default/Test 10 - deverly",
            "isArchive": false,
            "isSystem": false,
            "accessZoneId": 1,
            "workspace": "Default",
            "id": 454
        }
    ]
}
```

De updates aan omslagen worden gemaakt door een afzonderlijk eindpunt, en beschrijving, naam, en `isArchive` zijn optionele parameters voor bijwerken. Indien `isArchive` wordt gewijzigd door een update. Dit resulteert in de map die wordt gearchiveerd, als deze wordt gewijzigd in true of niet-gearchiveerd, als deze wordt gewijzigd in false, in de gebruikersinterface van Marketo. Programma&#39;s kunnen niet worden bijgewerkt met deze API.

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

### Verwijderen

U kunt verwijderen uit afzonderlijke mappen als deze leeg zijn. Dit betekent dat deze mappen geen elementen of submappen bevatten. Als een map van het type Program is of als het isSystem-veld op true is ingesteld, kan deze niet met deze API worden verwijderd.

```
POST /rest/asset/v1/folder/{id}/delete.json
```

```json
{
    "success": true,
    "warnings": [],
    "errors": [],
    "requestId": "4180#14e1f3fc017",
    "result": [
        {
            "id": 453
        }
    ]
}
```
