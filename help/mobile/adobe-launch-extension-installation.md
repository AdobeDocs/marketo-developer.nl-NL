---
title: '[!DNL Adobe Launch] Installatie van extensies'
feature: Mobile Marketing
description: '[!DNL Adobe Launch] Overzicht van installatie van extensies'
exl-id: d71b7cd7-309b-4882-9bba-7daaaa5ef32d
source-git-commit: 3649db037a95cfd20ff0a2c3d81a3b40d0095c39
workflow-type: tm+mt
source-wordcount: '664'
ht-degree: 0%

---

# [!DNL Adobe Launch] Installatie van extensies

Installatie-instructies voor [!DNL Adobe Launch] Marketo-extensie. De onderstaande stappen zijn vereist voor het verzenden van pushberichten en/of In-App-berichten.

## Vereisten

1. [ voeg een toepassing in Marketo Admin ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) toe (verkrijg uw toepassing Geheime Sleutel en identiteitskaart van Munchkin)
1. [ vorm het bezit in  [!DNL Adobe Launch]  portaal ](https://experience.adobe.com/#/@amc/data-collection/home)
1. De geheime sleutel van de toepassing en de Munchkin-id voor de eigenschap in de portal [!DNL Adobe Launch] configureren
1. [ Push Berichten van de Opstelling ](push-notifications.md) (facultatief)

## Marketo Extension installeren op iOS

### Snelle overbruggingsheader instellen

1. Ga naar [!UICONTROL File] > [!UICONTROL New] > [!UICONTROL File] en selecteer **[!UICONTROL Header File]** .

1. Noem het dossier &quot;&lt; _ProjectName_>-Bridging-Header&quot;.

1. Ga naar [!UICONTROL Project] > [!UICONTROL Target] > [!UICONTROL Build Settings] > [!UICONTROL Swift Compiler] > [!UICONTROL Code Generation] . Voeg het volgende pad toe aan de koptekst voor Objectief overbruggen:

`$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

## Extensie initialiseren

>[!BEGINTABS]

>[!TAB  Doelstelling C ]

Werk de methode `applicationDidBecomeActive` hieronder bij

```
(void)applicationDidBecomeActive:(UIApplication*) application
{
 [[ALMarketo sharedInstance] initializeMarketo:nil];
}
```

>[!TAB  Swift ]

Werk de methode `applicationDidBecomeActive` hieronder bij

```
func applicationDidBecomeActive(_ application: UIApplication)
{
 ALMarketo.sharedInstance().initializeMarketo(nil)
}
```

>[!ENDTABS]

## iOS-testapparaten

1. Selecteer **[!UICONTROL Project]** > **[!UICONTROL Target]** > **[!UICONTROL Info]** > **[!UICONTROL URL Types]** .
1. Id toevoegen: ${PRODUCT_NAME}
1. URL-schema&#39;s instellen: mkto-&lt;S_ecret Key_>
1. `application:openURL:sourceApplication:annotation:` opnemen in `AppDelegate.m file` (doelstelling-C)

### Type aangepaste URL afhandelen in AppDelegate

>[!BEGINTABS]

>[!TAB  Doelstelling C ]

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

>[!TAB  Swift ]

```
func application(_ application: UIApplication, open url: URL, sourceApplication: String?, annotation: Any) -> Bool {
    return ALMarketo.sharedInstance().application(application, open: url, sourceApplication: nil, annotation: nil)
}
```

>[!ENDTABS]

## Marketo SDK installeren op Android

### Android Extension Setup

Instructies volgen in de portal [!DNL Adobe Launch]

### Machtigingen configureren

Open `AndroidManifest.xml` en voeg de volgende machtigingen toe. Uw app moet om de toestemmingen &quot;INTERNET&quot;en &quot;ACCESS_NETWORK_STATE&quot;verzoeken. Als uw toepassing al om deze machtigingen vraagt, slaat u deze stap over.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

## Extensie initialiseren

ProGuard-configuratie (optioneel)

Als u ProGuard gebruikt voor uw app, voegt u de volgende regels toe in uw `proguard.cfg` -bestand. Het bestand bevindt zich in de map `project` . Als u deze code toevoegt, wordt de Marketo SDK uitgesloten van het verduisteringsproces.

```
-dontwarn com.marketo.*
-dontnote com.marketo.*
-keep class com.marketo.**{ *; }
```

## Android  Testen  Apparaten

Voeg &quot;MarketoActivity&quot; toe aan `AndroidManifest.xml` in de toepassingstag.

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

De MME Software Development Kit (SDK) voor Android is bijgewerkt naar een modern, stabieler en schaalbaar framework dat meer flexibiliteit en nieuwe technische functies voor uw Android-app-ontwikkelaar bevat.

Android app-ontwikkelaars kunnen nu direct Google [ Firebase Cloud Messaging ](https://firebase.google.com/docs/cloud-messaging/) (FCM) met deze SDK gebruiken.

### FCM toevoegen aan uw toepassing

1. Integreer de nieuwste Marketo Android SDK in Android App.  De stappen zijn beschikbaar bij [ GitHub ](https://github.com/Marketo/android-sdk).
1. Firebase-app configureren op Firebase-console.
   1. Creeer/voeg een Project op [ ](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/) de Console van de Vuurbasis toe.
      1. In de [ console van de Vuurbasis ](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), uitgezochte **[!UICONTROL Add Project]**.
      1. Selecteer uw GCM-project in de lijst met bestaande Google Cloud-projecten en selecteer **[!UICONTROL Add Firebase]** .
      1. Selecteer **[!UICONTROL Add Firebase to your Android App]** in het welkomstscherm van Firebase.
      1. Geef de pakketnaam en SHA-1 op en selecteer **[!UICONTROL Add App]** . Er wordt een nieuw `google-services.json` -bestand voor de Firebase-app gedownload.
      1. Selecteer **[!UICONTROL Continue]** en volg de gedetailleerde instructies voor het toevoegen van de Google Services-plug-in in Android Studio.

   1. Navigeren naar **[!UICONTROL Project Settings]** in [!UICONTROL Project Overview]
      1. Klik op **[!UICONTROL General]** tab. Download het `google-services.json` -bestand.
      1. Klik op de tab **[!UICONTROL Cloud Messaging]** . Kopieer [!UICONTROL Server Key] &amp; [!UICONTROL Sender ID] . Geef deze [!UICONTROL Server Key] &amp; [!UICONTROL Sender ID] door aan Marketo.
   1. FCM-wijzigingen configureren in Android App
      1. Schakel naar de projectweergave in Android Studio om de hoofdmap van uw project te bekijken
         1. Het gedownloade `google-services.json` bestand verplaatsen naar de hoofdmap van de Android-toepassingsmodule
         1. Voeg het volgende toe op projectniveau `build.gradle` :

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

         1. Klik tot slot op **[!UICONTROL Sync now]** in de bar die in identiteitskaart verschijnt
   1. Het manifest van uw app bewerken De FCM SDK voegt automatisch alle vereiste machtigingen en de vereiste ontvangerfunctionaliteit toe. Zorg ervoor dat u de volgende verouderde (en mogelijk schadelijke) elementen uit het manifest van uw app verwijdert:

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

**Q: Waar kan ik instructies vinden om aan de recentste versie van MME SDK bij te werken?** de Instructies kunnen op de Plaats van de Ontwikkelaar van Marketo [ ](installation.md) worden gevonden.

**Q: Zal het bijwerken aan de recentste versie van SDK me vereisen om een bijgewerkte versie van mijn Toepassing van Android aan mijn bestaande gebruikers te publiceren?** Nee.

**Q: Hoe beïnvloedt het de bestaande klanten MME die Android Apps hebben gepubliceerd die met Marketo Android SDK worden geïntegreerd?** Ze kunnen een bestaande GCM-client-app op Android als volgt migreren naar Firebase Cloud Messaging (FCM):

1. In de [ console van de Vuurbasis ](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), uitgezochte **[!UICONTROL Add Project]**.
1. Selecteer uw GCM-project in de lijst met bestaande Google Cloud-projecten en selecteer **[!UICONTROL Add Firebase]** .
1. Selecteer **[!UICONTROL Add Firebase to your Android App]** in het welkomstscherm van Firebase.
1. Geef de pakketnaam en SHA-1 op en selecteer **[!UICONTROL Add App]** . Een nieuw Google-services.json-bestand voor uw
1. Firebase-app wordt gedownload.
1. Selecteer **[!UICONTROL Continue]** en volg de gedetailleerde instructies voor het toevoegen van de Google Services-plug-in in Android Studio.

**Q: Kunnen wij de lood richten die gebruikend de oude SDK van Marketo worden gecreeerd die app GCM gebruikte?** Ja. Alle leads die met Marketo SDK zijn gemaakt, kunnen worden gebruikt voor het verzenden van de pushberichten.
