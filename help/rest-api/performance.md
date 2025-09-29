---
title: Prestaties
feature: REST API
description: Verhoog de Marketo REST API prestaties met HTTP compressie. Schakel gzip in om de bandbreedte te verminderen. Vak-API's worden niet ondersteund en minder dan 1024 bytes niet gecomprimeerd.
exl-id: 173a398a-9d36-4e8d-9dd3-7d0d375b085a
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '136'
ht-degree: 0%

---

# Prestaties

Deze pagina bevat een lijst met prestatiegerelateerde onderwerpen die u kunt gebruiken om de prestaties van uw integratie te verbeteren.

## HTTP-compressie

De Marketo REST API ondersteunt HTTP-compressie van responsinstanties met behulp van standaarden die zijn gedefinieerd in de HTTP 1.1-specificatie. Compressie inschakelen wordt aanbevolen, omdat hierdoor het bandbreedtegebruik en de tijd die nodig is om gegevens op te halen, afnemen.

>[!NOTE]
>
>Payloads van minder dan 1024 bytes worden niet gecomprimeerd en bulk APIs steunen geen compressie.

Als u compressie wilt inschakelen, neemt u de volgende HTTP-header op in de aanvraag:

```html
Accept-Encoding: gzip
```

De Marketo REST API comprimeert de reactiehoofdtekst en neemt deze header op:

```html
Content-Encoding: gzip
```

Hier is een voorbeeld gebruikend Kromme om [ te roepen krijg Leidingen door het eindpunt van het Type van Filter ](https://developer.adobe.com/marketo-apis/api/mapi/#tag/Leads/operation/getLeadsByFilterUsingGET) om 5 lood terug te winnen:

```bash
curl -H 'Accept-Encoding: gzip' 'https://123-ABC-456.mktorest.com/rest/v1/leads.json?filterType=id&filterValues=4,5,7,12,13'
```
