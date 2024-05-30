---
title: "Gebruikersbeheer"
feature: REST API
description: "Voer CRUD-bewerkingen uit op gebruikersrecords."
source-git-commit: d335bdd9f939c3e557a557b43fb3f33934e13fef
workflow-type: tm+mt
source-wordcount: '1155'
ht-degree: 0%

---


# Gebruikersbeheer

[Referentie voor eindpunten van gebruikersbeheer](https://developer.adobe.com/marketo-apis/api/user/)

Marketo biedt een reeks eindpunten voor gebruikersbeheer waarmee u CRUD-bewerkingen kunt uitvoeren op gebruikersrecords in Marketo. Gebruikers worden gemaakt door een uitnodiging naar een gebruiker te verzenden, die vervolgens een wachtwoord instelt en voor het eerst toegang krijgt tot Marketo.

In tegenstelling tot andere Marketo REST API&#39;s geldt het volgende voor het gebruik van de gebruikersbeheer-API&#39;s:

- U moet de HTTP-headermethode gebruiken om het toegangstoken te verzenden voor verificatie. U kunt geen toegangstoken als parameter van het vraagkoord overgaan. Meer informatie over verificatie is [hier](authentication.md).
- U moet een roltoestemming van twee verschillende groepen selecteren wanneer het creëren van de gebruikersrol voor [Aangepaste service](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api) voor REST API:
   1. &quot;Gebruikers benaderen&quot;-machtiging van de [Toegangsbeheerder](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions) groep
   1. &quot;Access User Management API&quot; van de [API voor toegang](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/descriptions-of-role-permissions) groep
- De instanties van de reactie bevatten niet de &quot;succes&quot;booleaanse attributen die op succes of mislukking van een vraag wijzen. In plaats daarvan moet u de statuscode van de HTTP-reactie evalueren. Als een vraag slaagt, is een 200 statuscode teruggekeerd. Wanneer een aanroep mislukt, wordt een statuscode van een ander niveau dan 200 geretourneerd en bevat de responsstructuur de standaardarray &#39;errors&#39; met foutcode en een beschrijvend foutbericht.
- De notatie van datetime-tekenreeksen is &quot;yyyyMMdd&#39;T&#39;HH:mm:ss.SSS&#39;t&#39;+|-hhm&quot;. Dit is van toepassing op de volgende kenmerken: createdAt, updatedAt, expiredAt.
- De eindpunten van de API voor gebruikersbeheer worden, net als andere eindpunten, niet voorafgegaan door &quot;/rest&quot;.

## Query

De steun van de vraag voor gebruikersbeheer omvat capaciteit om alle gebruikers, rollen, en werkruimten terug te winnen. U kunt ook één gebruikersrecord ophalen met gebruikers-id of met rol-/woordruimterecord met gebruikers-id.

### Gebruiker op id

De [Gebruiker ophalen op gebruikersnaam](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserUsingGET) eindpunt neemt één enkele `userid` padparameter en retourneert één gebruikersrecord voor een gebruiker die zijn uitnodiging heeft geaccepteerd.

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

De [Uitgenodigde gebruiker ophalen op gebruikersnaam](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getInvitedUserUsingGET) eindpunt neemt één enkele `userid` padparameter en retourneert één gebruikersrecord voor een &quot;hangende&quot; gebruiker (zijn uitnodiging nog niet heeft geaccepteerd).

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

De [Rollen en werkruimten ophalen op id](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUserRolesAndWorkspacesUsingGET) eindpunt neemt één enkele `userid` parameter path en retourneert een lijst met gebruikersrollen en werkruimterecords. De reactie bevat een array met één object dat rol- en werkruimte-id en naam voor de opgegeven gebruiker bevat.

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

De [Gebruikers ophalen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getUsersUsingGET) het eindpunt keert een lijst van alle gebruikersverslagen terug. De optionele `pageSize` parameter is een geheel getal dat het maximale aantal items aangeeft dat moet worden geretourneerd. De standaardwaarde is 20. Maximaal is 200. De optionele `pageOffset` parameter is een geheel getal dat opgeeft waar moet worden begonnen met het ophalen van vermeldingen. Kan worden gebruikt met `pageSize`. De standaardwaarde is 0.

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

### Bladeren door rollen

De [Rollen ophalen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getRolesUsingGET) het eindpunt keert een lijst van alle rolverslagen terug.

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

De [Werkruimten ophalen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/getWorkspacesUsingGET) het eindpunt keert een lijst van alle werkruimteverslagen terug.

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

Aan [Geïntegreerde Adobe IMS-abonnementen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), steunt dit eindpunt uitnodiging [Gebruikers met alleen API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user) alleen. Uitnodigen [standaardgebruikers](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), gebruikt u de [Adobe-gebruikersbeheer-API](https://developer.adobe.com/umapi/) in plaats daarvan.

De [Gebruiker uitnodigen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/inviteUserUsingPOST) eindpunt om een e-mailuitnodiging &quot;Welkom naar Marketo&quot; naar een nieuwe gebruiker te verzenden. De e-mailtekst bevat een koppeling &quot;Aanmelden bij Marketo&quot; waarmee de gebruiker voor het eerst toegang krijgt tot Marketo. Om de uitnodiging te accepteren, klikt de ontvanger van de e-mail op de koppeling &quot;Aanmelden bij Marketo&quot;, maakt hij een wachtwoord en krijgt hij toegang tot Marketo. Totdat het acceptatieproces is voltooid, is de uitnodiging in behandeling en kan het gebruikersrecord niet worden bewerkt. Een uitnodiging in behandeling verloopt zeven dagen na verzending. Meer informatie over het beheren van gebruikers kunt u vinden [hier](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users).

Parameters worden in de aanvraag-instantie doorgegeven in de toepassings-/json-indeling.

De volgende parameters zijn vereist:  `emailAddress`, `firstName`, `lastName, userRoleWorkspaces`. De `userRoleWorkspaces` parameter is een array van objecten die `accessRoleId` en `workspaceId` kenmerken.

De `userid` parameter is een unieke tekenreeks-id die voor gebruikersaanmelding wordt gebruikt en die als e-mailadres moet worden opgemaakt. Indien niet in het verzoek vermeld, de waarde van `userid` Wordt standaard ingesteld op de waarde die is opgegeven in `emailAddress` parameter.

Boolean `apiOnly` parameter specificeert of de gebruiker een [Gebruiker met alleen API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user). De `expiresAt` parameter geeft aan wanneer gebruikersaanmelding verloopt en wordt opgemaakt in de W3C ISO-8601-indeling (zonder milliseconden). Als deze niet in de aanvraag wordt opgegeven, verloopt de gebruiker nooit. De `reason` parameter is een tekenreeks die de reden voor de gebruikersuitnodiging beschrijft.

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

Hieronder ziet u een voorbeeld van de e-mailuitnodiging &quot;Welkom bij Marketo&quot; die naar de nieuwe gebruiker wordt verzonden. De onderwerpregel voor de e-mail is &quot;Marketo-aanmeldingsgegevens&quot;. De afzender is het e-mailadres van de gebruiker met alleen de API die is gekoppeld aan de [REST API Custom Service](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/additional-integrations/create-a-custom-service-for-use-with-rest-api)en de ontvanger is zoals opgegeven via de parameters firstName, lastName en emailAddress.

![E-mailadres van gebruiker uitnodigen](assets/invite-user-email.png)

De gebruiker accepteert de e-mailuitnodiging door haar wachtwoord tweemaal in te voeren en op de knop &quot;WACHTWOORD MAKEN&quot; te klikken. Daarna krijgt zij voor het eerst toegang tot Marketo.

## Gebruiker bijwerken

De updateondersteuning voor gebruikers biedt de mogelijkheid om gebruikerskenmerken bij te werken of een gebruiker te verwijderen. Alleen gebruikers die hun uitnodiging hebben geaccepteerd, kunnen worden bijgewerkt. Attributen worden doorgegeven als parameters de aanvraaginstantie in de toepassings-/json-indeling.

### Gebruikerskenmerken bijwerken

Aan [Geïntegreerde Adobe IMS-abonnementen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), ondersteunt dit eindpunt het bijwerken van kenmerken van [Gebruikers met alleen API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user) alleen. Om attributen bij te werken voor [standaardgebruikers](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), gebruikt u de [Adobe-gebruikersbeheer-API](https://developer.adobe.com/umapi/) in plaats daarvan.

De [Gebruikerskenmerken bijwerken](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/updateUserAttributeUsingPOST) eindpunt neemt één enkele `userid` padparameter en retourneert één gebruikersrecord. De aanvraaginstantie bevat een of meer gebruikerskenmerken die moeten worden bijgewerkt: `emailAddress`, `firstName`, `lastName`, `expiresAt`.

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

Aan [Geïntegreerde Adobe IMS-abonnementen](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/marketo-with-adobe-identity/adobe-identity-management-overview), steunt dit eindpunt schrapping van [Gebruikers met alleen API](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/create-an-api-only-user) alleen. Verwijderen [standaardgebruikers](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/administration/users-and-roles/managing-marketo-users), gebruikt u de [Adobe-gebruikersbeheer-API](https://developer.adobe.com/umapi/) in plaats daarvan.

De [Gebruiker verwijderen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteUserUsingPOST) eindpunt neemt één enkele `userid` padparameter en verwijdert de bijbehorende gebruiker uit de instantie. Dit is een destructieve verwijdering en kan niet worden teruggedraaid. Als dit gelukt is, wordt een 200-statuscode geretourneerd, anders wordt een foutbericht geretourneerd.

```
POST /userservice/management/v1/users/{userid}/delete.json
```

#### Uitgenodigde gebruiker verwijderen

De [Uitgenodigde gebruiker verwijderen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteInvitedUserUsingPOST) eindpunt neemt één enkele `userid` padparameter en verwijdert de bijbehorende gebruiker in behandeling uit de instantie (de gebruiker heeft de uitnodiging nog niet geaccepteerd). Dit is een destructieve verwijdering en kan niet worden teruggedraaid. Als dit gelukt is, wordt een 200-statuscode geretourneerd, anders wordt een foutbericht geretourneerd.

```
POST /userservice/management/v1/users/{userid}/invite/delete.json
```

## Rollen bijwerken

De steun van de update voor rollen omvat capaciteit om rollen toe te voegen en te schrappen. Attributen worden doorgegeven als parameters de aanvraagtekst in de toepassings-/json-indeling.

## Rollen toevoegen

De [Rollen toevoegen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/addRolesUsingPOST) eindpunt neemt één enkele `userid` padparameter en voegt een of meer gebruikersrollen toe aan de bijbehorende gebruiker. De aanvraaginstantie bevat een lijst met een of meer objecten die elk een  `accessRoleId` en `workspaceId` kenmerk. Indien succesvol, de volledige lijst van `accessRoleId/workspaceId` paren voor de opgegeven gebruiker worden geretourneerd.

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

De [Rollen verwijderen](https://developer.adobe.com/marketo-apis/api/user/#tag/User-Management/operation/deleteRolesUsingPOST) eindpunt neemt één enkele `userid` padparameter en verwijdert een of meer gebruikersrollen van de bijbehorende gebruiker. De aanvraaginstantie bevat een lijst met een of meer objecten die elk een  `accessRoleId` en `workspaceId` kenmerk. Indien succesvol, is de resterende lijst van accessRoleId/workspaceId paren voor de gespecificeerde gebruiker teruggekeerd.

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
