---
title: REST API
feature: REST API
description: REST API-overzicht
exl-id: 4b9beaf0-fc04-41d7-b93a-a1ae3147ce67
source-git-commit: 8ad3e3f0958ea705375651b1c8a75967d807ca80
workflow-type: tm+mt
source-wordcount: '770'
ht-degree: 0%

---

# REST API

Marketo maakt een REST API beschikbaar waarmee een groot aantal mogelijkheden van het systeem op afstand kan worden uitgevoerd. Er zijn veel opties, variërend van het maken van programma&#39;s tot het bulksgewijs importeren van leads, waarmee u een Marketo-instantie met fijnkorrelige besturing kunt besturen.

Deze APIs valt over het algemeen in twee brede categorieën: [ Lood Gegevensbestand ](https://developer.adobe.com/marketo-apis/api/mapi/), en [ Activa ](https://developer.adobe.com/marketo-apis/api/asset/). Looddatabase-API&#39;s maken het mogelijk om persoonlijke records en bijbehorende objecttypen van Marketo op te halen en met elkaar te communiceren, zoals Opportunity en Companies. API&#39;s voor bedrijfsmiddelen maken interactie met marketingmateriaal en workflowgerelateerde gegevens mogelijk.

>[!NOTE]
>De SOAP API wordt afgekeurd en zal na 31 oktober 2025 niet meer beschikbaar zijn. Al nieuwe ontwikkeling zou met Marketo [ REST API ](./rest-api.md) moeten worden gedaan, en de bestaande diensten zouden tegen die datum moeten worden gemigreerd om onderbrekingen in de dienst te vermijden. Als u de dienst hebt die SOAP API gebruikt, te raadplegen gelieve de SOAP API [ Gids van de Migratie ](../soap-api/migration.md) voor informatie over hoe te migreren.
>

- **Dagelijkse Quota:** De Abonnementen worden toegewezen 50.000 API vraag per dag (die dagelijks bij 12:00AM CST terugstelt). Je kunt je dagelijkse quota verhogen via je accountmanager.
- **Grens van het Tarief:** API toegang per geval is beperkt tot 100 vraag per 20 seconden.
- **Gelijktijdige Grens:**  Maximaal tien gelijktijdige API-aanroepen.

De grootte van standaardvraag is beperkt tot een lengte van URI van 8KB, en een lichaamsomvang van 1MB, hoewel het lichaam 10MB voor onze bulk APIs kan zijn. Als er een fout in uw vraag is, zal API typisch nog een statuscode van 200 terugkeren, maar de reactie JSON zal een &quot;succes&quot;lid met een waarde van `false`, en een serie van fouten in het &quot;fouten&quot;lid bevatten. Meer op fouten [ hier ](error-codes.md).

## Aan de slag

De volgende stappen vereisen beheerdersrechten in uw Marketo-instantie.

Voor uw eerste oproep aan Marketo haalt u een lead record op. Als u met Marketo wilt gaan werken, moet u API-referenties opvragen om geverifieerde aanroepen naar uw instantie te kunnen uitvoeren. Meld u aan bij de instantie en ga naar **[!UICONTROL Admin]** -> **[!UICONTROL Users and Roles]** .

![ Admin Gebruikers en Rollen ](assets/admin-users-and-roles.png)

Klik op het tabblad **[!UICONTROL Roles]** en vervolgens op Nieuwe rol en wijs ten minste de machtiging Alleen-lezen regel (of Alleen-lezen persoon) toe aan de rol in de API-groep Toegang. Geef deze een beschrijvende naam en klik op **[!UICONTROL Create]** .

![ Nieuwe Rol ](assets/new-role.png)

Ga nu terug naar de tab [!UICONTROL Users] en klik op **[!UICONTROL Invite New User]** . Geef uw gebruiker een beschrijvende naam die aangeeft dat het een API-gebruiker is, en een e-mailadres en klik op **[!UICONTROL Next]** .

![ Nieuwe Informatie van de Gebruiker ](assets/new-user-info.png)

Controleer vervolgens de optie [!UICONTROL API Only] en wijs de gebruiker de API-rol toe die u hebt gemaakt en klik op **[!UICONTROL Next]** .

![ Nieuwe Toestemmingen van de Gebruiker ](assets/new-user-permissions.png)

Klik op **[!UICONTROL Send]** om het maken van de gebruiker te voltooien.

![ Nieuw Bericht van de Gebruiker ](assets/new-user-message.png)

Ga vervolgens naar het menu [!UICONTROL Admin] en klik op **[!UICONTROL LaunchPoint]** .

![ Lanceerpunt ](assets/admin-launchpoint.png)

Klik op het menu **[!UICONTROL New]** en selecteer **[!UICONTROL New Service]** . Geef uw service een beschrijvende naam en selecteer **[!UICONTROL Custom]** in het vervolgkeuzemenu [!UICONTROL Service] . Geef deze een beschrijving, selecteer vervolgens de nieuwe gebruiker in het vervolgkeuzemenu [!UICONTROL API Only User] en klik op **[!UICONTROL Create]** .

![ Nieuwe Dienst van het Lanceerpunt ](assets/admin-launchpoint-new-service.png)

Klik op **[!UICONTROL View Details]** voor uw nieuwe service om de client-id en het clientgeheim te openen. U kunt nu op de knop **[!UICONTROL Get Token]** klikken om een toegangstoken te genereren dat een uur geldig is. Sla het token voorlopig op in een notitie.

![ krijg Symbolisch ](assets/get-token.png)

Ga vervolgens naar het menu **[!UICONTROL Admin]** en vervolgens naar **[!UICONTROL Web Services]** .

![ de Diensten van het Web ](assets/admin-web-services.png)

Zoek de [!UICONTROL Endpoint] in het vak REST API en sla deze op in een notitie voor nu.

![ REST Eindpunt ](assets/admin-web-services-rest-endpoint-1.png)

Wanneer het maken van vraag aan REST API methodes, moet een toegangstoken in elke vraag worden omvat om de vraag succesvol te zijn. Het toegangstoken moet als kopbal van HTTP worden verzonden.

```
Authorization: Bearer cdf01657-110d-4155-99a7-f986b2ff13a0:int
```

>[!IMPORTANT]
>
>De steun voor authentificatie die **gebruikt access_token** vraagparameter wordt verwijderd op 30 Juni, 2025. Als uw project een vraagparameter gebruikt om het toegangstoken over te gaan, zou het moeten worden bijgewerkt om de **1&rbrace; kopbal van de Vergunning &lbrace;zo spoedig mogelijk te gebruiken.** De nieuwe ontwikkeling zou de **kopbal van de Vergunning** exclusief moeten gebruiken.

Open een nieuw browser lusje en ga het volgende in, gebruikend de aangewezen informatie om [ te roepen krijgt Leads door het Type van Filter ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET)

```
<Your Endpoint URL>/rest/v1/leads.json?&filterType=email&filterValues=<Your Email Address>
```

Als u geen hoofdrecord hebt met uw e-mailadres in uw database, vervangt u dit door een adres waarvan u weet dat het aanwezig is. Druk op Enter in de URL-balk en u krijgt een JSON-reactie die hierop lijkt:

```json
{
    "requestId":"c493#1511ca2b184",
    "result":[
       {
           "id":1,
           "updatedAt":"2015-08-24T20:17:23Z",
           "lastName":"Elkington",
           "email":"developerfeedback@marketo.com",
           "createdAt":"2013-02-19T23:17:04Z",
           "firstName":"Kenneth"
        }
    ],
    "success":true
}
```

## API-gebruik

Elk van uw API-gebruikers wordt afzonderlijk vermeld in het API-gebruiksrapport, dus als u uw webservices opgesplitst naar gebruiker, kunt u gemakkelijk rekenschap geven van het gebruik van elk van uw integratie. Als het aantal API vraag aan uw instantie de grens overschrijdt en verdere vraag veroorzaakt om te ontbreken, staat het gebruiken van deze praktijk u toe om voor het volume van elk van uw diensten rekenschap te geven en laat u evalueren hoe te om de kwestie op te lossen. Ga naar **[!UICONTROL Admin]** -> **[!UICONTROL Integration]** > **[!UICONTROL Web Services]** en klik op het aantal aanroepen in de afgelopen zeven dagen.
