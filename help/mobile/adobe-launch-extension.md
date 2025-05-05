---
title: "Marketo Mobile Extension for [!DNL Adobe Launch]"
feature: Mobile Marketing
description: "Marketo Mobile Extension for [!DNL Adobe Launch] overzicht"
source-git-commit: 2185972a272b64908d6aac8818641af07c807ac2
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---


# Marketo Mobile-extensie voor [!DNL Adobe Launch]

Installatie-instructies voor de extensie Marketo Mobile SDK in [!DNL Adobe Launch]. De onderstaande stappen zijn vereist voor het verzenden van pushberichten en/of In-App-berichten.

## Vereisten

- [Een toepassing toevoegen in Marketo Admin](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (verkrijg uw toepassing Geheime Sleutel en identiteitskaart Munchkin)
- Volg de instructies in het dialoogvenster [!DNL Adobe Launch] portal voor installatie
- [Pushmeldingen instellen](push-notifications.md) (optioneel)

## iOS

### Snelle overbruggingsheader instellen

1. Ga naar Bestand > Nieuw > Bestand en selecteer Koptekstbestand.
1. Geef het bestand de naam &quot;&lt;_ProjectName_>-Bridging-header&quot;.
1. Ga naar Project > Doel > de Fasen van de Bouwstijl > de Verschuivende Compiler > de Generatie van de Code. Voeg het volgende pad toe aan Objectoverbruggingskoptekst:

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

Voor gebruikers met SWIFT: verwijder de volgende importinstructie terwijl de overbruggingsheader in de bovenstaande stappen wordt toegevoegd.

`import Marketo/ALMarketo`

### iOS-testapparaten

Volg de instructies op [IOS-testapparaten toevoegen](installation.md#ios_test_devices)

### Type aangepaste URL afhandelen in AppDelegate

Instructies volgen [hier](installation.md#ios_test_devices)

### Pushmeldingen instellen op iOS

Instructies volgen [hier](push-notifications.md) en gebruik de klassenaam ALMarketo in plaats van &quot;Marketo&quot;

## Android

### Machtigingen configureren

Openen `AndroidManifest.xml` en voeg de volgende machtigingen toe. Uw app moet om de toestemmingen &quot;INTERNET&quot;en &quot;ACCESS_NETWORK_STATE&quot;verzoeken. Als uw toepassing al om deze machtigingen vraagt, slaat u deze stap over.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### ProGuard-configuratie (optioneel)

Als u ProGuard gebruikt voor uw app, voegt u de volgende regels toe in uw `proguard.cfg` bestand. Het bestand bevindt zich in uw projectmap. Als u deze code toevoegt, wordt de Marketo SDK uitgesloten van het verduisteringsproces.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

### Android-testapparaten

Instructies volgen [hier](installation.md#android_test_devices)

## Pushmeldingen instellen op Android

Instructies volgen [hier](installation.md#android_firebase_cloud_messaging_support) en gebruik de klassenaam ALMarketo in plaats van &quot;Marketo&quot;

Volg de instructies voor het instellen van gebruikersprofielen [hier](user-profiles.md) en voor aangepaste handelingen, volg instructies [hier](custom-actions.md#android_custom_action). Gebruik in de volgende instructies de klassenaam &quot;ALMarketo&quot; in plaats van &quot;Marketo&quot;
