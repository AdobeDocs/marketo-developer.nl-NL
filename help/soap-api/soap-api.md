---
title: SOAP API
feature: SOAP
description: Overzicht van Marketo SOAP
exl-id: 6618cc82-15ae-4030-aa00-438e635d8369
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# SOAP API

De SOAP API wordt niet meer actief ontwikkeld. De aanroepen functioneren nog, maar onze ontwikkeling is gericht op [REST](https://developer.adobe.com/marketo-apis/) verder.

Met de Marketo SOAP API kunnen entiteiten en gegevens die in Marketo zijn opgeslagen, worden gemaakt, opgehaald en verwijderd. U kunt de [Marketo-SOAP-SDK](https://github.com/Marketo/SOAP-API-Java-Client) op GitHub. Er zijn ook [clientbibliotheken](https://github.com/Marketo/Community-Supported-Client-Libraries) om u wat tijd te besparen.

Laatste API-versie: 3_1

## SOAP WSDL

Als u het SOAP WSDL-document wilt ophalen, vraagt u uw SOAP API-eindpunt op bij uw **[!UICONTROL Admin]** > **[!UICONTROL Integration]** > **[!UICONTROL Web Services]** -menu.

![SOAP Eindpunt](assets/endpoint-soap.png)

Uw WSDL-URL is:

`<SOAP API Endpoint> + ?WSDL`

Gebruik niet het eindpunt dat in WSDL wordt gedefinieerd. Elke Marketo-instantie heeft een uniek eindpunt waarop aanroepen moeten worden uitgevoerd.

## Limieten

- **Dagelijks quotum:** De meeste abonnementen worden toegewezen 10.000 API vraag per dag (die dagelijks bij 12:00AM CST) terugstelt. Je kunt je dagelijkse quota verhogen via je accountmanager.
- **Snelheidslimiet:** API toegang per instantie beperkt tot 100 vraag per 20 seconden.
- **Gelijktijdige limiet:**  Maximaal tien gelijktijdige API-aanroepen.

Onze aanbeveling is dat de batchgrootten niet groter zijn dan 300. Grotere grootten worden niet ondersteund en kunnen leiden tot time-outs en in extreme gevallen tot een trage afhandeling.

## API-instellingen SOAP in Marketo

1. Ga naar de **[!UICONTROL Admin]** sectie en klik op **[!UICONTROL Web Services]**.

![admin-web-services2](assets/admin-web-services2.png)

1. Stel de juiste [!UICONTROL Encryption Key], klikt u op **[!UICONTROL Save Changes]** en gebruik de SOAP-API [!UICONTROL Endpoint], [!UICONTROL User ID], en [!UICONTROL Encryption Key] waarden om de juiste waarden te genereren [handtekening voor verificatie](authentication-signature.md) voor elke SOAP API-aanroep.

![admin-web-services3](assets/admin-web-services3.png)
