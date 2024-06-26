---
title: Fouten
feature: Webhooks
description: "Foutcodes voor webhooks"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---


# Fouten

Deze pagina bevat een lijst met foutresponscodes voor webhaken in Marketo.

1000 en 1001 worden gegenereerd door Marketo en 2xx tot 5xx zijn fouten die worden geretourneerd door het systeem dat de Marketo-webhaak aanroept.

Marketo kan alleen waarden weer in een veld toewijzen als de responscode van de webhaak van het 2xx-ras is. Als de bedoeling van de webhaak is om waarden in het Marketo lead record via de respons te wijzigen, moet de Web-service die wordt aangeroepen 2xx retourneren. Bij alle andere responscodes wordt de webhaak genegeerd voor het bijwerken van hoofdrecordwaarden.

| Antwoordcode | Beschrijving |
| --- | --- |
| 1000 | Dit wijst erop dat de de stroomactie van Webhaak van de Vraag binnen een Campagne van de Partij wordt gehuisvest. Webhaken kunnen alleen worden geactiveerd vanuit triggercampagnes. |
| 1001 | Dit geeft aan dat de webservice een lege responsstructuur heeft uitgegeven. |

## Fout bij het ophalen van een WebHaak

Fouten van Webhooks kunnen door de [!UICONTROL Webhook is Called] trigger:

![Webhaak wordt aangeroepen](assets/webhook-called.png)

* Response - Response is de letterlijke antwoordlading die door het verzoek is ontvangen.
* Fouttype - Dit komt overeen met de reden-zin van het HTTP-statusbericht.

Deze kunnen worden gebruikt om voorspelbare fouten en uitzonderingen te behandelen en erop te reageren. Afhankelijk van de service waarmee u integreert, is het mogelijk om bepaalde foutklassen automatisch te herstellen, terwijl waarschuwingen kunnen worden gemaakt om gebruikers op de hoogte te stellen van onverwachte fouten.
