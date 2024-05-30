---
title: "SOAP API"
feature: SOAP
description: "Overzicht Marketo SOAP"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---


# SOAP API

De SOAP API is niet langer actief ontwikkeld. De aanroepen functioneren nog, maar onze ontwikkeling is gericht op [REST](https://developer.adobe.com/marketo-apis/) verder.

Met de Marketo SOAP API kunnen entiteiten en gegevens die in Marketo zijn opgeslagen, worden gemaakt, opgehaald en verwijderd. U kunt de [Marketo-SOAP-SDK](https://github.com/Marketo/SOAP-API-Java-Client) op GitHub. Er zijn ook [clientbibliotheken](https://github.com/Marketo/Community-Supported-Client-Libraries) om u wat tijd te besparen.

Laatste API-versie: 3_1

## SOAP WSDL

Om het document van SOAP WSDL terug te winnen, verkrijg uw Eindpunt van de ZEEP API van uw **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web Services]** -menu.

![Eindpunt van SOAP](assets/endpoint-soap.png)

Uw WSDL-URL is:

`<SOAP API Endpoint> + ?WSDL`

Gebruik niet het eindpunt dat in WSDL wordt gedefinieerd. Elke Marketo-instantie heeft een uniek eindpunt waarop aanroepen moeten worden uitgevoerd.

## Limieten

- **Dagelijks quotum:** De meeste abonnementen worden toegewezen 10.000 API vraag per dag (die dagelijks bij 12:00AM CST) terugstelt. Je kunt je dagelijkse quota verhogen via je accountmanager.
- **Snelheidslimiet:** API toegang per instantie beperkt tot 100 vraag per 20 seconden.
- **Gelijktijdige limiet:**  Maximaal tien gelijktijdige API-aanroepen.

Onze aanbeveling is dat de batchgrootten niet groter zijn dan 300. Grotere grootten worden niet ondersteund en kunnen leiden tot time-outs en in extreme gevallen tot een trage afhandeling.

## SOAP API-instellingen in Marketo

1. Ga naar de Admin sectie en klik de Diensten van het Web.

![admin-web-services2](assets/admin-web-services2.png)

1. Stel een geschikte coderingssleutel in, klik op &quot;Wijzigingen opslaan&quot; en gebruik de waarden voor het eindpunt van de SOAP API, de gebruikers-id en de coderingssleutel om de juiste waarden te genereren [handtekening voor verificatie](authentication-signature.md) voor elke SOAP API-aanroep.

![admin-web-services3](assets/admin-web-services3.png)
