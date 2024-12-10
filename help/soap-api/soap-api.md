---
title: SOAP API
feature: SOAP
description: Overzicht van Marketo SOAP
exl-id: 6618cc82-15ae-4030-aa00-438e635d8369
source-git-commit: 7a3df193e47e7ee363c156bf24f0941879c6bd13
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 0%

---

# SOAP API

De SOAP API wordt afgekeurd en zal na 31 oktober 2025 niet meer beschikbaar zijn.  Al nieuwe ontwikkeling zou met Marketo [ REST ](https://developer.adobe.com/marketo-apis/) API moeten worden gedaan, en de bestaande diensten zouden tegen die datum moeten worden gemigreerd om onderbrekingen in de dienst te vermijden.

## SOAP WSDL

Als u het SOAP WSDL-document wilt ophalen, vraagt u uw SOAP API-eindpunt op via uw menu **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web Services]** .

![ SOAP Eindpunt ](assets/endpoint-soap.png)

Uw WSDL-URL is:

`<SOAP API Endpoint> + ?WSDL`

Gebruik niet het eindpunt dat in WSDL wordt gedefinieerd. Elke Marketo-instantie heeft een uniek eindpunt waarop aanroepen moeten worden uitgevoerd.

## Limieten

- **Dagelijkse Quota:** De meeste abonnementen worden toegewezen 10.000 API vraag per dag (die dagelijks bij 12:00AM CST terugstelt). Je kunt je dagelijkse quota verhogen via je accountmanager.
- **Grens van het Tarief:** API toegang per instantie beperkt tot 100 vraag per 20 seconden.
- **Gelijktijdige Grens:**  Maximaal tien gelijktijdige API-aanroepen.

Onze aanbeveling is dat de batchgrootten niet groter zijn dan 300. Grotere grootten worden niet ondersteund en kunnen leiden tot time-outs en in extreme gevallen tot een trage afhandeling.

## API-instellingen SOAP in Marketo

1. Ga naar de sectie **[!UICONTROL Admin]** en klik op **[!UICONTROL Web Services]** .

![ admin-web-services2 ](assets/admin-web-services2.png)

1. Plaats aangewezen [!UICONTROL Encryption Key], klik **[!UICONTROL Save Changes]** en gebruik SOAP API [!UICONTROL Endpoint], [!UICONTROL User ID], en [!UICONTROL Encryption Key] waarden om de correcte [ authentificatiehandtekening ](authentication-signature.md) voor elke SOAP API vraag te produceren.

![ admin-web-services3 ](assets/admin-web-services3.png)
