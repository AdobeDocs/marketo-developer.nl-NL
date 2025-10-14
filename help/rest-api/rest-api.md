---
title: REST API
feature: REST API
description: Leer hoe u Marketo REST API gebruikt, API-gebruikers en LaunchPoint instelt, quota's en limieten weergeeft, authenticeert met de machtigingsheader en haalt leads op.
exl-id: 4b9beaf0-fc04-41d7-b93a-a1ae3147ce67
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '790'
ht-degree: 0%

---

# REST API

Marketo maakt een REST API beschikbaar waarmee een groot aantal mogelijkheden van het systeem op afstand kan worden uitgevoerd. Er zijn veel opties, variërend van het maken van programma&#39;s tot het bulksgewijs importeren van leads, waarmee u een Marketo-instantie met fijnkorrelige besturing kunt besturen.

Deze APIs valt over het algemeen in twee brede categorieën: [&#x200B; Lood Gegevensbestand &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/), en [&#x200B; Activa &#x200B;](https://developer.adobe.com/marketo-apis/api/asset/). Looddatabase-API&#39;s maken het mogelijk om persoonlijke records en bijbehorende objecttypen van Marketo op te halen en met elkaar te communiceren, zoals Opportunity en Companies. API&#39;s voor bedrijfsmiddelen maken interactie met marketingmateriaal en workflowgerelateerde gegevens mogelijk.

>[!NOTE]
>De SOAP API wordt afgekeurd en zal na 31 oktober 2025 niet meer beschikbaar zijn. Al nieuwe ontwikkeling zou met Marketo [&#x200B; REST API &#x200B;](./rest-api.md) moeten worden gedaan, en de bestaande diensten zouden tegen die datum moeten worden gemigreerd om onderbrekingen in de dienst te vermijden. Als u de dienst hebt die SOAP API gebruikt, gelieve de Gids van de Migratie van SOAP API [&#128279;](../soap-api/migration.md) voor informatie over te raadplegen hoe te migreren.
>

- **Dagelijkse Quota:** De Abonnementen worden toegewezen 50.000 API vraag per dag (die dagelijks bij 12 :00AM CST terugstelt). Je kunt je dagelijkse quota verhogen via je accountmanager.
- **Grens van het Tarief:** API toegang per geval is beperkt tot 100 vraag per 20 seconden.
- **Gelijktijdige Grens:**  Maximaal tien gelijktijdige API-aanroepen.

De grootte van standaardvraag is beperkt tot een lengte van URI van 8KB, en een lichaamsomvang van 1MB, hoewel het lichaam 10MB voor onze bulk APIs kan zijn. Als er een fout in uw vraag is, zal API typisch nog een statuscode van 200 terugkeren, maar de reactie JSON zal een &quot;succes&quot;lid met een waarde van `false`, en een serie van fouten in het &quot;fouten&quot;lid bevatten. Meer op fouten [&#x200B; hier &#x200B;](error-codes.md).

## Aan de slag

De volgende stappen vereisen beheerdersrechten in uw Marketo-instantie.

Voor uw eerste oproep aan Marketo haalt u een lead record op. Als u met Marketo wilt gaan werken, moet u API-referenties opvragen om geverifieerde aanroepen naar uw instantie te kunnen uitvoeren. Meld u aan bij de instantie en ga naar **[!UICONTROL Admin]** -> **[!UICONTROL Users and Roles]** .

![&#x200B; Admin Gebruikers en Rollen &#x200B;](assets/admin-users-and-roles.png)

Klik op het tabblad **[!UICONTROL Roles]** en vervolgens op Nieuwe rol en wijs ten minste de machtiging Alleen-lezen regel (of Alleen-lezen persoon) toe aan de rol in de API-groep Toegang. Geef deze een beschrijvende naam en klik op **[!UICONTROL Create]** .

![&#x200B; Nieuwe Rol &#x200B;](assets/new-role.png)

Ga nu terug naar de tab [!UICONTROL Users] en klik op **[!UICONTROL Invite New User]** . Geef uw gebruiker een beschrijvende naam die aangeeft dat het een API-gebruiker is, en een e-mailadres en klik op **[!UICONTROL Next]** .

![&#x200B; Nieuwe Informatie van de Gebruiker &#x200B;](assets/new-user-info.png)

Controleer vervolgens de optie [!UICONTROL API Only] en wijs de gebruiker de API-rol toe die u hebt gemaakt en klik op **[!UICONTROL Next]** .

![&#x200B; Nieuwe Toestemmingen van de Gebruiker &#x200B;](assets/new-user-permissions.png)

Klik op **[!UICONTROL Send]** om het maken van de gebruiker te voltooien.

![&#x200B; Nieuw Bericht van de Gebruiker &#x200B;](assets/new-user-message.png)

Ga vervolgens naar het menu [!UICONTROL Admin] en klik op **[!UICONTROL LaunchPoint]** .

![&#x200B; Lanceerpunt &#x200B;](assets/admin-launchpoint.png)

Klik op het menu **[!UICONTROL New]** en selecteer **[!UICONTROL New Service]** . Geef uw service een beschrijvende naam en selecteer **[!UICONTROL Custom]** in het vervolgkeuzemenu [!UICONTROL Service] . Geef deze een beschrijving, selecteer vervolgens de nieuwe gebruiker in het vervolgkeuzemenu [!UICONTROL API Only User] en klik op **[!UICONTROL Create]** .

![&#x200B; Nieuwe Dienst van het Lanceerpunt &#x200B;](assets/admin-launchpoint-new-service.png)

Klik op **[!UICONTROL View Details]** voor uw nieuwe service om de client-id en het clientgeheim te openen. U kunt nu op de knop **[!UICONTROL Get Token]** klikken om een toegangstoken te genereren dat een uur geldig is. Sla het token voorlopig op in een notitie.

![&#x200B; krijg Symbolisch &#x200B;](assets/get-token.png)

Ga vervolgens naar het menu **[!UICONTROL Admin]** en vervolgens naar **[!UICONTROL Web Services]** .

![&#x200B; de Diensten van het Web &#x200B;](assets/admin-web-services.png)

Zoek de [!UICONTROL Endpoint] in het vak REST API en sla deze op in een notitie voor nu.

![&#x200B; REST Eindpunt &#x200B;](assets/admin-web-services-rest-endpoint-1.png)

Wanneer het maken van vraag aan REST API methodes, moet een toegangstoken in elke vraag worden omvat om de vraag succesvol te zijn. Het toegangstoken moet als kopbal van HTTP worden verzonden.

```
Authorization: Bearer cdf01657-110d-4155-99a7-f986b2ff13a0:int
```

>[!IMPORTANT]
>
>De steun voor authentificatie die **gebruikt access_token** vraagparameter wordt verwijderd op 30 Juni, 2025. Als uw project een vraagparameter gebruikt om het toegangstoken over te gaan, zou het moeten worden bijgewerkt om de **1&rbrace; kopbal van de Vergunning &lbrace;zo spoedig mogelijk te gebruiken.** De nieuwe ontwikkeling zou de **kopbal van de Vergunning** exclusief moeten gebruiken.

Open een nieuw browser lusje en ga het volgende in, gebruikend de aangewezen informatie om [&#x200B; te roepen krijgt Leads door het Type van Filter &#x200B;](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET)

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
