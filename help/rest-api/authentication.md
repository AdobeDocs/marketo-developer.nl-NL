---
title: Verificatie
feature: REST API
description: Marketo-gebruikers verifiëren voor API-gebruik.
exl-id: f89a8389-b50c-4e86-a9e4-6f6acfa98e7e
source-git-commit: e0fc654efe4501f734ab5158ce0bfd3ed08896ce
workflow-type: tm+mt
source-wordcount: '579'
ht-degree: 0%

---

# Verificatie

Marketo REST API&#39;s worden geverifieerd met OAuth 2.0 met twee poten. Client ID&#39;s en Client Secrets worden geleverd door aangepaste services die u definieert. Elke douanedienst is eigendom van een API-Enige gebruiker die een reeks rollen en toestemmingen heeft die de dienst machtigen om specifieke acties uit te voeren. Een toegangstoken wordt geassocieerd met één enkele douanedienst. De het symbolische vervaldatum van de toegang is onafhankelijk van tokens verbonden aan andere douanediensten die in een instantie kunnen aanwezig zijn.

## Toegangstoken maken

De knoppen `Client ID` en `Client Secret` vindt u in het menu **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL LaunchPoint]** door de aangepaste service te selecteren en op **[!UICONTROL View Details]** te klikken.

![ krijgt de Details van de Dienst van het HERSTEL ](assets/authentication-service-view-details.png)

![ Geloofsbrieven van het Lanceerpunt ](assets/admin-launchpoint-credentials.png)

De `Identity URL` vindt u in het menu **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web Services]** in de sectie REST API.

Creeer een toegangstoken gebruikend een HTTP GET (of POST) verzoek als zo:

```
GET <Identity URL>/oauth/token?grant_type=client_credentials&client_id=<Client Id>&client_secret=<Client Secret>
```

Als uw aanvraag geldig was, ontvangt u een JSON-antwoord dat vergelijkbaar is met het volgende:

```json
{
    "access_token": "cdf01657-110d-4155-99a7-f986b2ff13a0:int",
    "token_type": "bearer",
    "expires_in": 3599,
    "scope": "apis@acmeinc.com"
}
```

Responsdefinitie

- `access_token` - Het token dat u doorgeeft met volgende aanroepen om te verifiëren met de doelinstantie.
- `token_type` - De OAuth-verificatiemethode.
- `expires_in` - De resterende levensduur van de huidige token in seconden (waarna deze ongeldig is). Wanneer een toegangstoken oorspronkelijk wordt gecreeerd, is zijn levensduur 3600 seconden of één uur.
- `scope` - De eigenaar van de aangepaste service die is gebruikt voor verificatie.

## Een toegangstoken gebruiken

Wanneer het maken van vraag aan REST API methodes, moet een toegangstoken in elke vraag worden omvat om de vraag succesvol te zijn.

Er zijn twee methodes die u kunt gebruiken om een teken in uw vraag, als kopbal van HTTP, of als parameter van het vraagkoord te omvatten:

1. HTTP-header

   `Authorization: Bearer cdf01657-110d-4155-99a7-f986b2ff13a0:int`

1. Query-parameter

   `access_token=cdf01657-110d-4155-99a7-f986b2ff13a0:int`

   >[!IMPORTANT]
   >
   >De steun voor authentificatie die **gebruikt access_token** vraagparameter wordt verwijderd in een verdere versie. Als uw project een vraagparameter gebruikt om het toegangstoken over te gaan, zou het moeten worden bijgewerkt om de **1} kopbal van de Vergunning {zo spoedig mogelijk te gebruiken.** De nieuwe ontwikkeling zou de **kopbal van de Vergunning** exclusief moeten gebruiken.

## Tips en aanbevolen procedures

Het beheren van de vervaldatum van toegangstoken is belangrijk om ervoor te zorgen dat uw integratie regelmatig werkt en onverwachte authentificatiefouten tijdens normale verrichting voorkomt. Wanneer het ontwerpen van authentificatie voor uw integratie, ben zeker om het teken en de vervalperiode op te slaan in de reactie van de Identiteit.

Voordat u een REST-aanroep maakt, moet u de geldigheid van de token controleren op basis van de resterende levensduur. Als het teken is verlopen, dan vernieuwt het door [ Punt van de Identiteit ](https://developer.adobe.com/marketo-apis/api/identity/#tag/Identity/operation/identityUsingGET) te roepen. Dit helpt ervoor zorgen dat uw vraag REST nooit wegens een verlopen teken ontbreekt. Dit helpt u de latentie van uw vraag van REST op een voorspelbare manier beheren, die voor eindgebruiker-onder ogen ziet toepassingen van cruciaal belang is.

Als een verlopen teken wordt gebruikt om een vraag van het SPEL voor authentiek te verklaren, zal de vraag REST ontbreken en een 602 foutencode terugkeren. Als een ongeldig token wordt gebruikt om een REST-aanroep te verifiëren, wordt een 601-foutcode geretourneerd. Als één van beide codes worden ontvangen, zou de cliënt het teken door het aanroepen van het eindpunt van de Identiteit moeten vernieuwen.

Als u het eindpunt van de Identiteit roept alvorens uw teken is verlopen, zal het zelfde teken en de resterende levensduur in de reactie zijn teruggekeerd.

Herinner dat uw toegangstokens op een per-douane-dienst basis en niet op een gebruikersbasis worden bezeten. Hoewel twee reacties van de Identiteit binnen het bereik van de zelfde gebruiker kunnen zijn, zijn de toegangstokens en de vervalperiodes onafhankelijk van elkaar als zij met geloofsbrieven van twee verschillende diensten werden gemaakt. Houd dit in mening wanneer u veelvoudige reeksen geloofsbrieven in de zelfde toepassing hebt; identiteitskaart van de Cliënt kan een nuttige sleutel zijn om hen onafhankelijk te beheren.
