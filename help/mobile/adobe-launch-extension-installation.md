---
title: '''[!DNL Adobe Launch] Installatie van extensie'
feature: Mobile Marketing
description: '''[!DNL Adobe Launch] overzicht van extensieinstallatie'
exl-id: d71b7cd7-309b-4882-9bba-7daaaa5ef32d
source-git-commit: 6fc45ff98998217923e2a5b02d00d1522fe3272c
workflow-type: tm+mt
source-wordcount: '665'
ht-degree: 0%

---

# [!DNL Adobe Launch] Installatie van extensie

Installatie-instructies voor [!DNL Adobe Launch] Marketo-extensie De onderstaande stappen zijn vereist voor het verzenden van pushberichten en/of In-App-berichten.

## Vereisten

1. [Een toepassing toevoegen in Marketo Admin](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) (verkrijg uw toepassing Geheime Sleutel en identiteitskaart Munchkin)
1. [Configureer de eigenschap in [!DNL Adobe Launch] portaal](https://experience.adobe.com/#/@amc/data-collection/home)
1. Configureer de geheime sleutel van de toepassing en Munchkin-id voor de eigenschap in het dialoogvenster [!DNL Adobe Launch] portaal
1. [Pushmeldingen instellen](push-notifications.md) (optioneel)

## Marketo Extension installeren op iOS

### Snelle overbruggingsheader instellen

1. Ga naar [!UICONTROL File] > [!UICONTROL New] > [!UICONTROL File] en selecteer **[!UICONTROL Header File]**.

1. Geef het bestand de naam &quot;&lt;_ProjectName_>-Bridging-header&quot;.

1. Ga naar [!UICONTROL Project] > [!UICONTROL Target] > [!UICONTROL Build Settings] > [!UICONTROL Swift Compiler] > [!UICONTROL Code Generation]. Voeg het volgende pad toe aan de koptekst voor Objectief overbruggen:

`$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

## Extensie initialiseren

>[!BEGINTABS]

>[!TAB Doelstelling C]

Werk de `applicationDidBecomeActive` methode zoals hieronder

```
(void)applicationDidBecomeActive:(UIApplication*) application
{
 [[ALMarketo sharedInstance] initializeMarketo:nil];
}
```

>[!TAB Swift]

Werk de `applicationDidBecomeActive` methode zoals hieronder

```
func applicationDidBecomeActive(_ application: UIApplication)
{
 ALMarketo.sharedInstance().initializeMarketo(nil)
}
```

>[!ENDTABS]

## iOS-testapparaten

1. Selecteren **[!UICONTROL Project]** > **[!UICONTROL Target]** > **[!UICONTROL Info]** > **[!UICONTROL URL Types]**.
1. Id toevoegen: ${PRODUCT_NAME}
1. URL-schema&#39;s instellen: mkto-&lt;s_ecret key_=&quot;&quot;>
1. Inclusief `application:openURL:sourceApplication:annotation:` tot `AppDelegate.m file` (doelstelling C)

### Type aangepaste URL afhandelen in AppDelegate

>[!BEGINTABS]

>[!TAB Doelstelling C]

```
#ifdef __IPHONE_10_0
-(BOOL)application:(UIApplication *)application 
           openURL:(NSURL *)url 
           options:(NSDictionary *)options{
    return [[ALMarketo sharedInstance] application:application
                                         openURL:url
                               sourceApplication:nil
                                      annotation:nil];
}
#endif

- (BOOL)application:(UIApplication *)application
            openURL:(NSURL *)url
  sourceApplication:(NSString *)sourceApplication
         annotation:(id)annotation {
    return [[ALMarketo sharedInstance] application:application
                                         openURL:url
                               sourceApplication:nil
                                      annotation:nil];
}
```

>[!TAB Swift]

```
func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    return ALMarketo.sharedInstance().application(application, open: url, sourceApplication: nil, annotation: nil)
}
```

>[!ENDTABS]

## Marketo SDK installeren op Android

### Android Extension Setup

Instructies volgen in [!DNL Adobe Launch] portaal

### Machtigingen configureren

Openen `AndroidManifest.xml` en voeg de volgende machtigingen toe. Uw app moet om de toestemmingen &quot;INTERNET&quot;en &quot;ACCESS_NETWORK_STATE&quot;verzoeken. Als uw toepassing al om deze machtigingen vraagt, slaat u deze stap over.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

## Extensie initialiseren

ProGuard-configuratie (optioneel)

Als u ProGuard gebruikt voor uw app, voegt u de volgende regels toe in uw `proguard.cfg` bestand. Het bestand bevindt zich in uw `project` map. Als u deze code toevoegt, wordt de Marketo SDK uitgesloten van het verduisteringsproces.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

## Android-testapparaten

&quot;MarketoActivity&quot; toevoegen aan `AndroidManifest.xml` in de toepassingstag.

```xml
<activity android:name="com.marketo.MarketoActivity"  android:configChanges="orientation|screenSize" >
    <intent-filter android:label="MarketoActivity" >
        <action  android:name="android.intent.action.VIEW"/>
        <category  android:name="android.intent.category.DEFAULT"/>
        <category  android:name="android.intent.category.BROWSABLE"/>
        <data android:host="add_test_device" android:scheme="mkto" />
    </intent-filter>
</activity>
```

## Ondersteuning voor Firebase Cloud Messaging

De MME Software Development Kit (SDK) voor Android is bijgewerkt naar een modern, stabiel en schaalbaar framework dat meer flexibiliteit en nieuwe technische functies voor uw Android-app-ontwikkelaar bevat.

Ontwikkelaars van Android-apps kunnen nu rechtstreeks Google gebruiken [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) (FCM) met deze SDK.

### FCM toevoegen aan uw toepassing

1. De nieuwste Marketo Android SDK integreren in Android App.  Stappen zijn beschikbaar op [GitHub](https://github.com/Marketo/android-sdk).
1. Firebase-app configureren op Firebase-console.
   1. Een project maken/toevoegen op [](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/)Firebase Console.
      1. In de [Firebase-console](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/), selecteert u **[!UICONTROL Add Project]**.
      1. Selecteer uw GCM-project in de lijst met bestaande Google Cloud-projecten en selecteer **[!UICONTROL Add Firebase]**.
      1. Selecteer in het welkomstscherm van Firebase de optie **[!UICONTROL Add Firebase to your Android App]**.
      1. Geef uw pakketnaam en SHA-1 op en selecteer **[!UICONTROL Add App]**. Een nieuwe `google-services.json` bestand voor uw Firebase-app wordt gedownload.
      1. Selecteren **[!UICONTROL Continue]** en volgt u de gedetailleerde instructies voor het toevoegen van de Google Services-plug-in in Android Studio.

   1. Navigeren naar **[!UICONTROL Project Settings]** in [!UICONTROL Project Overview]
      1. Klikken **[!UICONTROL General]** tab. Download de `google-services.json` bestand.
      1. Klikken op **[!UICONTROL Cloud Messaging]** tab. Kopiëren [!UICONTROL Server Key] &amp; [!UICONTROL Sender ID]. Geef deze [!UICONTROL Server Key] &amp; [!UICONTROL Sender ID] naar Marketo.
   1. FCM-wijzigingen configureren in Android App
      1. Schakel naar de projectweergave in Android Studio om de hoofdmap van uw project te bekijken
         1. De gedownloade `google-services.json` bestand in de hoofdmap van de Android-toepassingsmodule
         1. Op projectniveau `build.gradle` het volgende toevoegen:

            ```
            buildscript {
              dependencies {
                classpath 'com.google.gms:google-services:4.0.0'
              }
            }
            ```

         1. Voeg het volgende toe in build.gradle op toepassingsniveau:

            ```
            dependencies {
              compile 'com.google.firebase:firebase-core:17.4.0'
            } 
            // Add to the bottom of the file 
            apply plugin: 'com.google.gms.google-services'
            ```

         1. Tot slot klikt u op **[!UICONTROL Sync now]** in de balk die wordt weergegeven in de id
   1. Bewerk manifest van uw app De FCM SDK voegt automatisch alle vereiste machtigingen en de vereiste ontvangerfunctionaliteit toe. Zorg ervoor dat u de volgende verouderde (en mogelijk schadelijke) elementen uit het manifest van uw app verwijdert:

      ```xml
      <uses-permission android:name="android.permission.WAKE_LOCK" />
      <permission android:name="<your-package-name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
      <uses-permission android:name="<your-package-name>.permission.C2D_MESSAGE" />
      
      ...
      
      <receiver>
        android:name="com.google.android.gms.gcm.GcmReceiver"
        android:exported="true"
        android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
          <action android:name="com.google.android.c2dm.intent.RECEIVE" />
          <category android:name="<your-package-name> />
        </intent-filter> 
      </receiver>
      ```


### Veelgestelde vragen over FCM

Veelgestelde vragen over ondersteuning voor Firebase Cloud Messaging.

**V: Waar kan ik instructies vinden om bij te werken naar de nieuwste versie van de MME SDK?** Instructies vindt u op de Marketo Developer Site [HIER](installation.md).

**V: Zal ik voor het bijwerken van de nieuwste versie van de SDK een bijgewerkte versie van mijn Android-toepassing publiceren naar mijn bestaande gebruikers?** Nee.

**V: Hoe beïnvloedt dit de bestaande MME-klanten die Android Apps hebben gepubliceerd die zijn geïntegreerd met Marketo Android SDK?** Ze kunnen een bestaande GCM-client-app op Android als volgt migreren naar Firebase Cloud Messaging (FCM):

1. In de [Firebase-console](https://accounts.google.com/ServiceLogin?passive=1209600&amp;osid=1&amp;continue=https://console.firebase.google.com/&amp;followup=https://console.firebase.google.com/), selecteert u **[!UICONTROL Add Project]**.
1. Selecteer uw GCM-project in de lijst met bestaande Google Cloud-projecten en selecteer **[!UICONTROL Add Firebase]**.
1. Selecteer in het welkomstscherm van Firebase de optie **[!UICONTROL Add Firebase to your Android App]**.
1. Geef uw pakketnaam en SHA-1 op en selecteer **[!UICONTROL Add App]**. Een nieuw Google-services.json-bestand voor uw
1. Firebase-app wordt gedownload.
1. Selecteren **[!UICONTROL Continue]** en volgt u de gedetailleerde instructies voor het toevoegen van de Google Services-plug-in in Android Studio.

**V: Kunnen we ons richten op de leads die gemaakt zijn met de oude Marketo SDK die de GCM App gebruikte?** Ja. Alle leads die met de Marketo SDK zijn gemaakt, kunnen worden verzonden voor de pushberichten.
