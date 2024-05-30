---
title: "REST API"
feature: REST API
description: "REST API-overzicht"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '643'
ht-degree: 0%

---


# REST API

Marketo maakt een REST API beschikbaar waarmee een groot aantal mogelijkheden van het systeem op afstand kan worden uitgevoerd. Er zijn veel opties, variërend van het maken van programma&#39;s tot het bulksgewijs importeren van leads, waarmee u een Marketo-instantie met fijnkorrelige besturing kunt besturen.

Deze API&#39;s vallen over het algemeen in twee brede categorieën: [Database lead](https://developer.adobe.com/marketo-apis/api/mapi/), en [Element](https://developer.adobe.com/marketo-apis/api/asset/). Looddatabase-API&#39;s maken het mogelijk om persoonlijke records en bijbehorende objecttypen, zoals Opportunity en Companies, op te halen en te communiceren met Marketo. API&#39;s voor bedrijfsmiddelen maken interactie met marketingmateriaal en workflowgerelateerde gegevens mogelijk.

- **Dagelijks quotum:** Aan abonnementen worden 50.000 API-aanroepen per dag toegewezen (die dagelijks opnieuw worden ingesteld om 12:00AM CST). Je kunt je dagelijkse quota verhogen via je accountmanager.
- **Snelheidslimiet:** API toegang per instantie beperkt tot 100 vraag per 20 seconden.
- **Gelijktijdige limiet:**  Maximaal tien gelijktijdige API-aanroepen.

De grootte van standaardvraag is beperkt tot een lengte van URI van 8KB, en een lichaamsomvang van 1MB, hoewel het lichaam 10MB voor onze bulk APIs kan zijn. Als er een fout in uw vraag is, zal API typisch nog een statuscode van 200 terugkeren, maar de reactie JSON zal een &quot;succes&quot;lid met een waarde van bevatten `false`en een array met fouten in het lid &#39;fouten&#39;. Meer informatie over fouten [hier](error-codes.md).

## Aan de slag

De volgende stappen vereisen beheerdersrechten in uw Marketo-instantie.

Voor uw eerste oproep aan Marketo haalt u een lead record op. Als u met Marketo wilt gaan werken, moet u API-referenties opvragen om geverifieerde aanroepen naar uw instantie te kunnen uitvoeren. Meld u aan bij uw exemplaar en ga naar de **[!UICONTROL Admin]** -> **[!UICONTROL Users and Roles]**.

![Gebruikers en rollen voor beheerders](assets/admin-users-and-roles.png)

Klik op de knop [!UICONTROL Roles] en vervolgens Nieuwe rol. Wijs ten minste de machtiging Alleen-lezen regel (of Alleen-lezen persoon) toe aan de rol in de API-groep Toegang. Geef deze een beschrijvende naam en klik op [!UICONTROL Create].

![Nieuwe rol](assets/new-role.png)

Ga nu terug naar het tabblad Gebruikers en klik op Nieuwe gebruiker uitnodigen. Geef uw gebruiker een beschrijvende naam die aangeeft dat het een API-gebruiker is, en een e-mailadres en klik op **[!UICONTROL Next]**.

![Nieuwe gebruikersgegevens](assets/new-user-info.png)

Schakel vervolgens de optie Alleen API in en geef uw gebruiker de door u gemaakte API-rol en klik op **[!UICONTROL Next]**.

![Nieuwe gebruikersmachtigingen](assets/new-user-permissions.png)

Als u het maken van de gebruiker wilt voltooien, klikt u op **[!UICONTROL Send]**.

![Nieuw gebruikersbericht](assets/new-user-message.png)

Ga vervolgens naar het menu Beheer en klik op **[!UICONTROL LaunchPoint]**.

![Launchpoint](assets/admin-launchpoint.png)

Klik op het menu Nieuw en selecteer [!UICONTROL New Service]. Geef uw service een beschrijvende naam en selecteer &quot;Aangepast&quot; in het vervolgkeuzemenu Service. Geef het een beschrijving, dan selecteer uw nieuwe gebruiker van het API slechts drop-down menu van de Gebruiker en klik [!UICONTROL Create].

![Nieuwe opstartservice](assets/admin-launchpoint-new-service.png)

Klik de Details van de Mening voor uw nieuwe dienst om tot identiteitskaart van de Cliënt en Geheim van de Cliënt toegang te hebben. U kunt nu op de knop [!UICONTROL Get Token] om een toegangstoken te produceren die één uur geldig is. Sla het token voorlopig op in een notitie.

![Token ophalen](assets/get-token.png)

Ga vervolgens naar het menu Beheer en vervolgens naar **[!UICONTROL Web Services]**.

![Webservices](assets/admin-web-services.png)

Zoek het eindpunt in het vak REST API en sla het op in een notitie voor nu.

![REST Endpoint](assets/admin-web-services-rest-endpoint-1.png)

Open een nieuw browser lusje en ga het volgende in, gebruikend de aangewezen informatie om te roepen [Regels ophalen op filtertype](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET):

```
<Your Endpoint URL>/rest/v1/leads.json?access_token=<Your Access Token>&filterType=email&filterValues=<Your Email Address>
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

Elk van uw API-gebruikers wordt afzonderlijk vermeld in het API-gebruiksrapport, dus als u uw webservices opgesplitst naar gebruiker, kunt u gemakkelijk rekenschap geven van het gebruik van elk van uw integratie. Als het aantal API vraag aan uw instantie de grens overschrijdt en verdere vraag veroorzaakt om te ontbreken, staat het gebruiken van deze praktijk u toe om voor het volume van elk van uw diensten rekenschap te geven en laat u evalueren hoe te om de kwestie op te lossen. Ga naar **[!UICONTROL Admin]** -> **[!UICONTROL Integration]** > **[!UICONTROL Web Services]** en het klikken van het aantal vraag in de afgelopen zeven dagen.
