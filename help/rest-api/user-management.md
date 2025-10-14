---
title: Gebruikersbeheer
feature: REST API
description: Handleiding voor Marketo-gebruikersbeheer-API's voor CRUD voor gebruikers, op header gebaseerde auth, rollen en werkruimten, statuscode-afhandeling, gegevenstijdindeling en queryeindpunten.
exl-id: 2a58f496-0fe6-4f7e-98ef-e9e5a017c2de
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '1197'
ht-degree: 0%

---

# Gebruikersbeheer

[&#x200B; Verwijzing van het Eindpunt van het Beheer van de Gebruiker &#x200B;](https://developer.adobe.com/marketo-apis/api/user/)

Marketo biedt een reeks eindpunten voor gebruikersbeheer waarmee u CRUD-bewerkingen kunt uitvoeren op gebruikersrecords in Marketo. Gebruikers worden gemaakt door een uitnodiging naar een gebruiker te verzenden, die vervolgens een wachtwoord instelt en voor het eerst toegang krijgt tot Marketo.

In tegenstelling tot andere Marketo REST API&#39;s geldt het volgende voor het gebruik van de gebruikersbeheer-API&#39;s:

- U moet de HTTP-headermethode gebruiken om het toegangstoken te verzenden voor verificatie. U kunt geen toegangstoken als parameter van het vraagkoord overgaan. Meer informatie over authentificatie is [&#x200B; hier &#x200B;](authentication.md).
- U moet een roltoestemming van twee verschillende groepen selecteren wanneer het creëren van de gebruikersrol voor [&#x200B; de Dienst van de Douane &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api) voor REST API:
   1. De toestemming van de &quot;Gebruikers van de Toegang&quot;van de [&#x200B; Admin van de Toegang &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions) groep
   1. De &quot;API van het Beheer van de Gebruiker van de Toegang&quot;van de [&#x200B; Toegang API &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions) groep
- De instanties van de reactie bevatten niet de &quot;succes&quot;booleaanse attributen die op succes of mislukking van een vraag wijzen. In plaats daarvan moet u de statuscode van de HTTP-reactie evalueren. Als een vraag slaagt, is een 200 statuscode teruggekeerd. Wanneer een aanroep mislukt, wordt een statuscode van een ander niveau dan 200 geretourneerd en bevat de responsstructuur de standaardarray &#39;errors&#39; met foutcode en een beschrijvend foutbericht.
- De indeling van datetime-tekenreeksen is `yyyyMMdd'T'HH:mm:ss.SSS't'+|-hhmm` . Dit is van toepassing op de volgende kenmerken: `createdAt`, `updatedAt`, `expiresAt` .
- De eindpunten van de API voor gebruikersbeheer worden, net als andere eindpunten, niet voorafgegaan door &quot;/rest&quot;.

## Query

De steun van de vraag voor gebruikersbeheer omvat capaciteit om alle gebruikers, rollen, en werkruimten terug te winnen. U kunt ook één gebruikersrecord ophalen op basis van gebruikers-id of een rol-/werkruimterecord op basis van gebruikers-id.

### Gebruiker op id

Het [&#x200B; krijgt Gebruiker door Identiteitseur &#x200B;](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserUsingGET) eindpunt neemt één enkele `userid` wegparameter en keert één enkel gebruikersverslag voor een gebruiker terug die hun uitnodiging heeft goedgekeurd.

```
GET /userservice/management/v1/users/{userid}/user.json
```

```json
{
  "userid": "jamie@houselannister.com",
  "firstName": "Jamie",
  "lastName": "Lannister",
  "emailAddress": "jamie@lannister.com",
  "optedIn": false,
  "failedLogins": 0,
  "failedDeviceCode": 0,
  "isLocked": false,
  "lockedReason": null,
  "id": 0,
  "apiOnly": false,
  "userRoleWorkspaces": [
    {
      "accessRoleId": 1,
      "accessRoleName": "Admin",
      "workspaceId": 0,
      "workspaceName": "AllZones"
    },
    {
      "accessRoleId": 2,
      "accessRoleName":
      "Standard User",
      "workspaceId": 1008,
      "workspaceName": "World"
    }
  ],
  "expiresAt": "2020-12-31T08:00:00.000t+0000",
  "lastLoginAt": "2020-02-05T01:02:23.000t+0000"
}
```

### Uitgenodigde gebruiker op gebruikersnaam

[&#x200B; krijgt Uitgenodigde Gebruiker door Identiteitseindpunt &#x200B;](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getInvitedUserUsingGET) neemt één enkele `userid` wegparameter en keert één enkel gebruikersverslag voor een &quot;hangende&quot;gebruiker terug (heeft nog niet hun uitnodiging goedgekeurd).

```
GET /userservice/management/v1/users/{userid}/invite.json
```

```json
{
    "id": 25112,
    "firstName": "Jamie",
    "lastName": "Lannister",
    "emailAddress": "jamie@lannister.com",
    "userId": "jamie@lannister.com",
    "subscriptionId": 3381,
    "status": "pending",
    "expiresAt": "20200807T20:49:54.0t+0000",
    "createdAt": "20200731T20:49:54.0t+0000",
    "updatedAt": "20200731T20:49:54.0t+0000"
}
```

### Rollen en werkruimten per id

Het [&#x200B; krijgt Rollen en Werkruimten door Identiteitseindpunt &#x200B;](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserRolesAndWorkspacesUsingGET) neemt één enkele `userid` wegparameter en keert een lijst van gebruikersrol en werkruimtekendecords terug. De reactie bevat een array met één object dat rol- en werkruimte-id en naam voor de opgegeven gebruiker bevat.

```
GET /userservice/management/v1/users/{userid}/roles.json
```

```json
[
  {
    "accessRoleId": 1,
    "accessRoleName": "Admin",
    "workspaceId": 0,
    "workspaceName": "AllZones"
  },
  {
    "accessRoleId": 2,
    "accessRoleName": "Standard User",
    "workspaceId": 1008,
    "workspaceName": "World"
  }
]
```

### Gebruikers doorbladeren

Het [&#x200B; krijgt Gebruikers &#x200B;](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUsersUsingGET) eindpunt keert een lijst van alle gebruikersverslagen terug. De optionele parameter `pageSize` is een geheel getal dat het maximale aantal items opgeeft dat moet worden geretourneerd. De standaardwaarde is 20. Maximaal is 200. De optionele parameter `pageOffset` is een geheel getal dat opgeeft waar moet worden begonnen met het ophalen van vermeldingen. Kan worden gebruikt met `pageSize` . De standaardwaarde is 0.

```
GET /userservice/management/v1/users/allusers.json
```

```json
[
  {
    "userid": "jamie@lannister.com",
    "firstName": "Jamie",
    "lastName": "Lannister",
    "emailAddress": "jamie@houselannister.com",
    "id": 6785,
    "apiOnly": false
  },
  {
    "userid": "jeoffery@housebaratheon.com",
    "firstName": "Jeoffery",
    "lastName": "Baratheon",
    "emailAddress": "jeoffery@housebaratheon.com",
    "id": 7718,
    "apiOnly": false
  },
  {
    "userid": "rickon@housestark.com",
    "firstName": "Rickon",
    "lastName": "Stark",
    "emailAddress": "rickon@housestark.com",
    "id": 8612,
    "apiOnly": false
  }
]
```

>[!NOTE]
>
>In het bovenstaande codevoorbeeld is de weergegeven `userid` bestemd voor een klant die is gemigreerd naar Adobe IMS. Voor klanten die nog niet hebben gemigreerd, wordt een normaal e-mailadres weergegeven in het veld `userid` .

### Bladeren door rollen

Het [&#x200B; krijgt het eindpunt van Rollen &#x200B;](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getRolesUsingGET) keert een lijst van alle rolverslagen terug.

```
GET /userservice/management/v1/users/roles.json
```

```json
[
    {
        "id": 1,
        "name": "Admin",
        "description": "All permissions",
        "type": "system",
        "hidden": false,
        "onlyAllZones": true,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20100327T18:27:42.0t+0000"
    },
    {
        "id": 2,
        "name": "Standard User",
        "description": "All permissions except Admin",
        "type": "system",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20180423T02:33:29.0t+0000"
    },
    {
        "id": 24,
        "name": "RTP Launcher",
        "description": "Role required for launcher in RTP",
        "type": "system",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20151024T01:45:40.0t+0000",
        "updatedAt": "20171024T23:41:24.0t+0000"
    },
    {
        "id": 25,
        "name": "RTP Editor",
        "description": "Role required for editor in RTP",
        "type": "system",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20151024T01:45:40.0t+0000",
        "updatedAt": "20171024T23:41:24.0t+0000"
    },
    {
        "id": 101,
        "name": "Analytics User",
        "description": "Has access to Analytics",
        "type": "custom",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20180423T02:33:29.0t+0000"
    },
    {
        "id": 102,
        "name": "Marketing User",
        "description": "All permissions except Admin",
        "type": "custom",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20100327T18:27:42.0t+0000"
    },
    {
        "id": 103,
        "name": "Web Designer",
        "description": "Has access to Design Studio except approval permission",
        "type": "custom",
        "hidden": false,
        "onlyAllZones": false,
        "createdAt": "20100327T18:27:42.0t+0000",
        "updatedAt": "20180423T02:33:29.0t+0000"
    }
]
```

### Bladeren door werkruimten

Het [&#x200B; krijgt het 1&rbrace; eindpunt van de Werkruimten &lbrace;keert een lijst van alle werkruimteverslagen terug.](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getWorkspacesUsingGET)

```
GET /userservice/management/v1/users/workspaces.json
```

```json
[
  {
    "id": 1,
    "name": "Default",
    "description": "Initial workspace for Marketing Activities, Design Studio, and so on.",
    "globalViz": 0,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20160910T23:08:05.0t+0000",
    "updatedAt": "20160910T23:08:05.0t+0000"
  },
  {
    "id": 1008,
    "name": "World",
    "description": "",
    "globalViz": 0,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20181119T21:59:36.0t+0000",
    "updatedAt": "20181119T21:59:36.0t+0000"
  },
  {
    "id": 1009,
    "name": "Reproduction - US English - All Leads",
    "description": "A Workspace for recreating customer-reported problems.",
    "globalViz": 1,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20190129T23:36:37.0t+0000",
    "updatedAt": "20190129T23:36:37.0t+0000"
  },
  {
    "id": 1010,
    "name": "US",
    "description": "United States - Qualified Leads",
    "globalViz": 0,
    "status": "active",
    "currencyInfo": null,
    "createdAt": "20190322T15:55:40.0t+0000",
    "updatedAt": "20190322T15:55:40.0t+0000"
  }
]
```

## Gebruiker uitnodigen

Op [&#x200B; Adobe IMS-Geïntegreerde abonnementen &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), steunt dit eindpunt uitnodiging van [&#x200B; API-Enige Gebruikers &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user) slechts. Om [&#x200B; standaardGebruikers &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users) uit te nodigen, gebruik in plaats daarvan [&#x200B; Adobe gebruikersbeheer API &#x200B;](https://developer.adobe.com/umapi/).

Het [&#x200B; Uitnodigt Gebruiker &#x200B;](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/inviteUserUsingPOST) eindpunt uit om een &quot;Onthaal naar Marketo&quot;e-mailuitnodiging naar nieuwe gebruiker te verzenden. De e-mailtekst bevat een koppeling &quot;Aanmelden bij Marketo&quot; waarmee de gebruiker voor het eerst toegang krijgt tot Marketo. Om de uitnodiging te accepteren, klikt de ontvanger van de e-mail op de koppeling &quot;Aanmelden bij Marketo&quot;, maakt hij een wachtwoord en krijgt hij toegang tot Marketo. Totdat het acceptatieproces is voltooid, is de uitnodiging in behandeling en kan het gebruikersrecord niet worden bewerkt. Een uitnodiging in behandeling verloopt zeven dagen na verzending. Meer informatie over het beheren van gebruikers kan [&#x200B; hier &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users) worden gevonden.

Parameters worden in de aanvraaghoofdtekst doorgegeven in de `application/json` -indeling.

De volgende parameters zijn vereist:  `emailAddress` , `firstName` , `lastName, userRoleWorkspaces` . De parameter `userRoleWorkspaces` is een array van objecten die `accessRoleId` - en `workspaceId` -kenmerken bevatten.

De parameter `userid` is een unieke tekenreeks van de gebruikersidentificatie die wordt gebruikt voor gebruikersaanmelding en moet worden opgemaakt als een e-mailadres. Als deze waarde niet in de aanvraag wordt opgegeven, wordt de waarde van `userid` standaard ingesteld op de waarde die in de parameter `emailAddress` wordt opgegeven.

De booleaanse `apiOnly` parameter specificeert of de gebruiker een [&#x200B; API-Enige gebruiker &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user) is. De parameter `expiresAt` geeft aan wanneer de gebruikersaanmelding verloopt en is opgemaakt in de indeling W3C ISO-8601 (zonder milliseconden). Als deze niet in de aanvraag wordt opgegeven, verloopt de gebruiker nooit. De parameter `reason` is een tekenreeks die de reden voor de gebruikersuitnodiging beschrijft.

Het eindpunt keert een waarde van &quot;waar&quot;terug als succesvol, anders een foutenmelding is teruggekeerd.

```
POST /userservice/management/v1/users/invite.json
```

```
Content-Type: application/json
```

```json
{
  "emailAddress": "daenerys@housetargaryen.com",
  "firstName": "Daenerys",
  "lastName": "Targaryen",
  "expiresAt": "2020-12-31T23:59:59-05:00",
  "reason": "Keeper of dragons",
  "userRoleWorkspaces": [
    {
      "accessRoleId": 1,
      "workspaceId": 0
    }
  ]
}
```

```
true
```

Hieronder ziet u een voorbeeld van de e-mailuitnodiging &quot;Welkom bij Marketo&quot; die naar de nieuwe gebruiker wordt verzonden. De e-mailonderwerpregel is &quot;de Informatie van de Login van Marketo&quot;, is de afzender het e-mailadres van de API-Enige Gebruiker verbonden aan de [&#x200B; REST API de Dienst van de Douane &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api), en de ontvanger is zoals gespecificeerd via de parameters firstName, lastName, en emailAddress.

![&#x200B; nodigde E-mail van de Gebruiker uit &#x200B;](assets/invite-user-email.png)

De gebruiker accepteert de e-mailuitnodiging door haar wachtwoord tweemaal in te voeren en op de knop &quot;WACHTWOORD MAKEN&quot; te klikken. Daarna krijgt zij voor het eerst toegang tot Marketo.

## Gebruiker bijwerken

De updateondersteuning voor gebruikers biedt de mogelijkheid om gebruikerskenmerken bij te werken of een gebruiker te verwijderen. Alleen gebruikers die hun uitnodiging hebben geaccepteerd, kunnen worden bijgewerkt. Attributen worden doorgegeven als parameters de aanvraaginstantie in de toepassings-/json-indeling.

### Gebruikerskenmerken bijwerken

Op [&#x200B; Adobe IMS-Geïntegreerde abonnementen &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), steunt dit eindpunt het bijwerken van attributen van [&#x200B; API-Enige Gebruikers &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user) slechts. Om attributen voor [&#x200B; standaardGebruikers &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users) bij te werken, gebruik in plaats daarvan [&#x200B; het Beheer API van de Gebruiker van Adobe &#x200B;](https://developer.adobe.com/umapi/).

Het [&#x200B; eindpunt van de Attributen van de Gebruiker van de Update &#x200B;](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/updateUserAttributeUsingPOST) neemt één enkele `userid` wegparameter en keert één enkel gebruikersverslag terug. De hoofdtekst van de aanvraag bevat een of meer gebruikerskenmerken die moeten worden bijgewerkt: `emailAddress`, `firstName`, `lastName`, `expiresAt` .

```
POST /userservice/management/v1/users/{userid}/update.json
```

```
Content-Type: application/json
```

```json
{
  "firstName": "JAMIE",
  "lastName": "LANISTER",
  "expiresAt": "20211231T08:00:00.000t+0000"
}
```

```json
{
  "userid": "jamie@houselannister.com",
  "firstName": "JAMIE",
  "lastName": "LANISTER",
  "emailAddress": "jamie@houselannister.com",
  "optedIn": false,
  "failedLogins": 0,
  "failedDeviceCode": 0,
  "isLocked": false,
  "lockedReason": null,
  "id": 0,
  "apiOnly": false,
  "userRoleWorkspaces": [
    {
      "accessRoleId": 1,
      "accessRoleName": "Admin",
      "workspaceId": 0,
      "workspaceName": "AllZones"
    },
    {
      "accessRoleId": 2,
      "accessRoleName":
      "Standard User",
      "workspaceId": 1008,
      "workspaceName": "World"
    }
  ],
  "expiresAt": "2021-12-31T08:00:00.000t+0000"
  "lastLoginAt": "2020-02-05T01:02:23.000t+0000"
}
```

#### Gebruiker verwijderen

Op [&#x200B; Adobe IMS-Geïntegreerde abonnementen &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), steunt dit eindpunt schrapping van [&#x200B; API-Enige Gebruikers &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user) slechts. Om [&#x200B; standaardGebruikers &#x200B;](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users) te schrappen, gebruik in plaats daarvan [&#x200B; het Beheer API van de Gebruiker van Adobe &#x200B;](https://developer.adobe.com/umapi/).

Het [&#x200B; eindpunt van de Gebruiker van de Schrapping &#x200B;](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteUserUsingPOST) neemt één enkele `userid` wegparameter en schrapt de overeenkomstige gebruiker van de instantie. Dit is een destructieve verwijdering en kan niet worden teruggedraaid. Als dit gelukt is, wordt een 200-statuscode geretourneerd, anders wordt een foutbericht geretourneerd.

```
POST /userservice/management/v1/users/{userid}/delete.json
```

#### Uitgenodigde gebruiker verwijderen

Het [&#x200B; Schrapping Uitgenodigde Gebruiker &#x200B;](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteInvitedUserUsingPOST) eindpunt neemt één enkele `userid` wegparameter en schrapt de overeenkomstige &quot;hangende&quot;gebruiker van de instantie (de gebruiker had nog niet hun uitnodiging goedgekeurd). Dit is een destructieve verwijdering en kan niet worden teruggedraaid. Als dit gelukt is, wordt een 200-statuscode geretourneerd, anders wordt een foutbericht geretourneerd.

```
POST /userservice/management/v1/users/{userid}/invite/delete.json
```

## Rollen bijwerken

De steun van de update voor rollen omvat capaciteit om rollen toe te voegen en te schrappen. Attributen worden doorgegeven als parameters de aanvraagtekst in de toepassings-/json-indeling.

## Rollen toevoegen

Het [&#x200B; voegt 1&rbrace; eindpunt van Rollen &lbrace;neemt één enkele &#x200B;](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/addRolesUsingPOST) wegparameter toe en voegt één of meerdere gebruikersrollen aan de overeenkomstige gebruiker toe. `userid` De aanvraaginstantie bevat een lijst met een of meer objecten die elk een  `accessRoleId` en een `workspaceId` -kenmerk. Als dit lukt, wordt de volledige lijst met `accessRoleId/workspaceId` paren voor de opgegeven gebruiker geretourneerd.

```
POST /userservice/management/v1/users/{userid}/roles/create.json
```

```
Content-Type: application/json
```

```json
[
  {
    "accessRoleId": 2,
    "workspaceId": 1008
  }
]
```

```json
[
  {
    "accessRoleId": 1,
    "accessRoleName": "Admin",
    "workspaceId": 0,
    "workspaceName": "AllZones"
  },
  {
    "accessRoleId": 2,
    "accessRoleName": "Standard User",
    "workspaceId": 1008,
    "workspaceName": "World"
  }
]
```

## Rollen verwijderen

Het [&#x200B; punt van de Rollen van de Schrapping &#x200B;](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteRolesUsingPOST) neemt één enkele `userid` wegparameter en schrapt één of meerdere gebruikersrollen van de overeenkomstige gebruiker. De aanvraaginstantie bevat een lijst met een of meer objecten die elk een  `accessRoleId` en een `workspaceId` -kenmerk. Indien succesvol, is de resterende lijst van accessRoleId/workspaceId paren voor de gespecificeerde gebruiker teruggekeerd.

```
POST /userservice/management/v1/users/{userid}/roles/delete.json
```

```
Content-Type: application/json
```

```json
[
  {
    "accessRoleId": 2,
    "workspaceId": 1008
  }
]
```

```json
[
  {
    "accessRoleId": 1,
    "accessRoleName": "Admin",
    "workspaceId": 0,
    "workspaceName": "AllZones"
  }
]
```
