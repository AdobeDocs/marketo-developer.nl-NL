---
title: SOAP API
feature: SOAP
description: Marketo SOAP API is afgekeurd na 31 oktober 2025. Leer om aan REST te migreren, uw WSDL terug te winnen, quota's, tariefgrenzen, en auth opstelling te zien.
exl-id: 6618cc82-15ae-4030-aa00-438e635d8369
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '249'
ht-degree: 0%

---

# SOAP API

De SOAP API wordt afgekeurd en zal na 31 oktober 2025 niet meer beschikbaar zijn. Al nieuwe ontwikkeling zou met Marketo [&#x200B; REST API &#x200B;](../rest-api/rest-api.md) moeten worden gedaan, en de bestaande diensten zouden tegen die datum moeten worden gemigreerd om onderbrekingen in de dienst te vermijden. Als u de dienst hebt die SOAP API gebruikt, gelieve de Gids van de Migratie van SOAP API [&#128279;](./migration.md) voor informatie over te raadplegen hoe te migreren.

## SOAP WSDL

Als u het SOAP WSDL-document wilt ophalen, vraagt u het Eindpunt van de SOAP API aan via uw menu **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web Services]** .

![&#x200B; Eindpunt van SOAP &#x200B;](assets/endpoint-soap.png)

Uw WSDL-URL is:

`<SOAP API Endpoint> + ?WSDL`

Gebruik niet het eindpunt dat in WSDL wordt gedefinieerd. Elke Marketo-instantie heeft een uniek eindpunt waarop aanroepen moeten worden uitgevoerd.

## Limieten

- **Dagelijkse Quota:** De meeste abonnementen worden toegewezen 10.000 API vraag per dag (die dagelijks bij 12 :00AM CST terugstelt). Je kunt je dagelijkse quota verhogen via je accountmanager.
- **Grens van het Tarief:** API toegang per instantie beperkt tot 100 vraag per 20 seconden.
- **Gelijktijdige Grens:**  Maximaal tien gelijktijdige API-aanroepen.

Onze aanbeveling is dat de batchgrootten niet groter zijn dan 300. Grotere grootten worden niet ondersteund en kunnen leiden tot time-outs en in extreme gevallen tot een trage afhandeling.

## SOAP API-instellingen in Marketo

1. Ga naar de sectie **[!UICONTROL Admin]** en klik op **[!UICONTROL Web Services]** .

![&#x200B; admin-web-services2 &#x200B;](assets/admin-web-services2.png)

1. Plaats aangewezen [!UICONTROL Encryption Key], klik **[!UICONTROL Save Changes]** en gebruik SOAP API [!UICONTROL Endpoint], [!UICONTROL User ID], en [!UICONTROL Encryption Key] waarden om de correcte [&#x200B; authentificatiehandtekening &#x200B;](authentication-signature.md) voor elke vraag van SOAP API te produceren.

![&#x200B; admin-web-services3 &#x200B;](assets/admin-web-services3.png)
