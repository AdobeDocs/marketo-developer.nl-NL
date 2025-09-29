---
title: Marketo Mobile-extensie voor  [!DNL Adobe Launch]
feature: Mobile Marketing
description: Installeer en configureer de extensie Marketo Mobile SDK in Adobe Launch voor iOS en Android, inclusief installatie voor pushberichten en in-app berichten.
exl-id: 2f8691ff-0442-45a5-aeba-c91c3af5c711
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 0%

---

# Marketo Mobile-extensie voor [!DNL Adobe Launch]

Installatie-instructies voor Marketo Mobile SDK-extensie in [!DNL Adobe Launch] . De onderstaande stappen zijn vereist voor het verzenden van pushberichten en/of In-App-berichten.

## Vereisten

- [ voeg een toepassing in Marketo Admin ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) toe (verkrijg uw toepassing Geheime Sleutel en identiteitskaart van Munchkin)
- Volg de instructies op de [!DNL Adobe Launch] -portal voor installatie
- [ Push Berichten van de Opstelling ](push-notifications.md) (facultatief)

## iOS

### Snelle overbruggingsheader instellen

1. Ga naar Bestand > Nieuw > Bestand en selecteer Koptekstbestand.
1. Noem het dossier &quot;&lt; _ProjectName_>-Bridging-Header&quot;.
1. Ga naar Project > Doel > de Fasen van de Bouwstijl > de Verschuivende Compiler > de Generatie van de Code. Voeg het volgende pad toe aan Objectoverbruggingskoptekst:

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

Voor gebruikers met SWIFT: verwijder de volgende importinstructie terwijl de overbruggingsheader in de bovenstaande stappen wordt toegevoegd.

`import Marketo/ALMarketo`

### iOS-testapparaten

Volg instructies bij [ het Toevoegen van de Apparaten van de Test van iOS ](installation.md#ios_test_devices)

### Type aangepaste URL afhandelen in AppDelegate

Volg instructies [ hier ](installation.md#ios_test_devices)

### Pushmeldingen instellen op iOS

Volg instructies [ hier ](push-notifications.md) en gebruik de klassennaam &quot;ALMarketo&quot;in plaats van &quot;Marketo&quot;

## Android

### Machtigingen configureren

Open `AndroidManifest.xml` en voeg de volgende machtigingen toe. Uw app moet om de toestemmingen &quot;INTERNET&quot;en &quot;ACCESS_NETWORK_STATE&quot;verzoeken. Als uw toepassing al om deze machtigingen vraagt, slaat u deze stap over.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### ProGuard-configuratie (optioneel)

Als u ProGuard gebruikt voor uw app, voegt u de volgende regels toe in uw `proguard.cfg` -bestand. Het bestand bevindt zich in uw projectmap. Als u deze code toevoegt, wordt de Marketo SDK uitgesloten van het verduisteringsproces.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

### Android-testapparaten

Volg instructies [ hier ](installation.md#android_test_devices)

## Pushmeldingen instellen op Android

Volg instructies [ hier ](installation.md#android_firebase_cloud_messaging_support) en gebruik de klassennaam &quot;ALMarketo&quot;in plaats van &quot;Marketo&quot;

Voor vestiging volgen de gebruikersprofielen hier instructies [ ](user-profiles.md) en voor douaneacties hier instructies [ volgen ](custom-actions.md#android_custom_action). Gebruik in de volgende instructies de klassenaam &quot;ALMarketo&quot; in plaats van &quot;Marketo&quot;
