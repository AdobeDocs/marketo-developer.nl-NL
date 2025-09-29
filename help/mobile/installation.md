---
title: Installatie
feature: Mobile Marketing
description: Handleiding voor de installatie en initialisatie van de Marketo Mobile SDK op iOS en Android met CocoaPods, Swift Package Manager of Gradle, waardoor push- en in-app berichten kunnen worden ingeschakeld.
exl-id: e0b79d85-3509-46d2-a77d-cee211c5ec7f
source-git-commit: 7557b9957c87f63c2646be13842ea450035792be
workflow-type: tm+mt
source-wordcount: '774'
ht-degree: 0%

---

# Installatie

Installatie-instructies voor Marketo Mobile SDK. De onderstaande stappen zijn vereist voor het verzenden van pushberichten en/of In-App-berichten.

## Marketo SDK installeren op iOS

### Vereisten

1. [ voeg een toepassing in Marketo Admin ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) toe (verkrijg uw toepassing Geheime Sleutel en identiteitskaart van Munchkin)
1. [ Push Berichten van de Opstelling ](push-notifications.md) (facultatief)

### Framework installeren via CocoaPods

1. CocoaPods installeren. `$ sudo gem install cocoapods`
1. Wijzig de map in de projectmap en maak een podbestand met de standaardwaarden. `$ pod init`
1. Open het podbestand. `$ open -a Xcode Podfile`
1. Voeg de volgende regel toe aan het podbestand. `$ pod 'Marketo-iOS-SDK'`
1. Sla het bestand op en sluit het.
1. Download en installeer Marketo iOS SDK. `$ pod install`
1. De werkruimte openen in Xcode. `$ open App.xcworkspace`

### Framework installeren met Swift Package Manager

1. Selecteer uw project van de Navigator van het Project en onder &quot;voeg de Afhankelijkheid van het Pakket&quot;toe, klik &#39;+&#39; zoals hieronder getoond:

   ![ voeg Afhankelijkheid ](assets/dependency-manager-add.png) toe

1. Voeg Marketo-pakket van deze repo toe. Voeg deze URL voor deze gegevensopslagruimte toe: <https://github.com/Marketo/ios-sdk>.

   ![ Repo URL ](assets/dependency-manager-url.png)

1. Voeg nu de bundel van het Middel toe zoals getoond: plaats `MarketoFramework.XCframework` in projectnavigator en open het in Vinder. Sleep `MKTResources.bundle` naar Bundelbronnen kopiëren.

### Snelle overbruggingsheader instellen

1. Ga naar Bestand > Nieuw > Bestand en selecteer Koptekstbestand.

   ![ Uitgezochte &quot;Dossier van de Kopbal&quot;](assets/choose-header-file.png)

1. Noem het dossier &quot;&lt; _ProjectName_>-Bridging-Header&quot;.

1. Ga naar Project > Doel > de Fasen van de Bouwstijl > de Verschuivende Compiler > de Generatie van de Code. Voeg het volgende pad toe aan Objectoverbruggingskoptekst:

   `$(PODS_ROOT)/<_ProjectName_>-Bridging-Header.h`

   ![ bouwt Fasen ](assets/build-phases.png)

## SDK initialiseren

Voordat u de Marketo iOS SDK kunt gebruiken, moet u deze initialiseren met uw Munchkin-account-id en toepassingsbeveiligingssleutel. U vindt deze allemaal in het gebied Marketo Admin onder &quot;Mobiele apps en apparaten&quot;.

1. Open uw bestand AppDelegate.m (doelstelling-C) of Bridging (SWIFT) en importeer het headerbestand Marketo.h.

   ```
   #import <MarketoFramework/MarketoFramework.h>
   ```

1. Plak de volgende code in de functie `application:didFinishLaunchingWithOptions`:.

   Merk op dat wij &quot;inheems&quot;als kadertype voor Inheemse Apps moeten overgaan.

>[!BEGINTABS]

>[!TAB  Doelstelling C ]

```
Marketo *sharedInstance = [Marketo sharedInstance];

[sharedInstance initializeWithMunchkinID:@"munchkinAccountId" appSecret:@"secretKey" mobileFrameworkType:@"native" launchOptions:launchOptions];
```

>[!TAB  Swift ]

```
let sharedInstance: Marketo = Marketo.sharedInstance()

sharedInstance.initialize(withMunchkinID: "munchkinAccountId", appSecret: "secretKey", mobileFrameworkType: "native", launchOptions: launchOptions)
```

>[!ENDTABS]

1. Vervang `munkinAccountId` en `secretKey` hiervoor door gebruik te maken van de opties &quot;Munchkin-account-id&quot; en &quot;Geheime sleutel&quot; in de sectie Marketo **[!UICONTROL Admin]** > **[!UICONTROL Mobile Apps and Devices]** .

## iOS-testapparaten

1. Selecteer Project > Doel > Info > URL-typen.
1. Id toevoegen: ${PRODUCT_NAME}
1. URL-schema&#39;s instellen: `mkto-<Secret Key_>`
1. Omvat toepassing :openURL: sourceApplication :annotation: aan het dossier AppDelegate.m (doelstelling-C)

## Type aangepaste URL afhandelen in AppDelegate

>[!BEGINTABS]

>[!TAB  Doelstelling C ]

```
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options{

    return [[Marketo sharedInstance] application:app
                                         openURL:url
                                         options:options];
}
```

>[!TAB  Swift ]

```
private func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool
    {
        return Marketo.sharedInstance().application(app, open: url, options: options)
    }
```

>[!ENDTABS]

## Marketo SDK installeren op Android

### Vereisten

1. [ voeg een toepassing in Marketo Admin ](https://experienceleague.adobe.com/nl/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) toe (verkrijg uw toepassing Geheime Sleutel en identiteitskaart van Munchkin)
1. [ Push Berichten van de Opstelling ](push-notifications.md#android_setup_push) (facultatief)
1. [ Download Marketo SDK voor Android ](https://codeload.github.com/Marketo/android-sdk/zip/refs/heads/master)

### Android SDK Setup () met Gradle

1. Voeg onder de sectie Afhankelijkheden in het bestand build.gradle op toepassingsniveau toe

`implementation 'com.marketo:MarketoSDK:0.8.9'`

1. Het hoofdbestand `build.gradle` moet

   ```
   buildscript {
       repositories {
           google()
           mavenCentral()
       }
   ```

1. Uw project synchroniseren met verloopbestanden

### Machtigingen configureren

Open `AndroidManifest.xml` en voeg de volgende machtigingen toe. Uw app moet om de toestemmingen &quot;INTERNET&quot;en &quot;ACCESS_NETWORK_STATE&quot;verzoeken. Als uw toepassing al om deze machtigingen vraagt, slaat u deze stap over.

```xml
<uses‐permission android:name="android.permission.INTERNET"></uses‐permission>
<uses‐permission android:name="android.permission.ACCESS_NETWORK_STATE"></uses‐permission>
```

### SDK initialiseren

1. Open de klasse Application of Activity in uw app en importeer de Marketo SDK in uw Activiteit vóór setContentView of in Application Context.

   ```java
   // Initialize Marketo
   Marketo marketoSdk = Marketo.getInstance(getApplicationContext());
   marketoSdk.initializeSDK("native","munchkinAccountId","secretKey");
   ```

1. ProGuard-configuratie (optioneel)

   Als u ProGuard gebruikt voor uw app, voegt u de volgende regels toe in uw `proguard.cfg` -bestand. Het bestand bevindt zich in uw projectmap. Als u deze code toevoegt, wordt de Marketo SDK uitgesloten van het verduisteringsproces.

   ```
   -dontwarn com.marketo.*
   -dontnote com.marketo.*
   -keep class com.marketo.`{ *; }
   ```

## Android-testapparaten

Voeg &quot;MarketoActivity&quot; toe aan het `AndroidManifest.xml` -bestand in de toepassingstag.

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
   1. Creeer/voeg een Project op [&#128279;](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/) de Console van de Vuurbasis toe.
      1. In de [ console van de Vuurbasis ](https://accounts.google.com/ServiceLogin?passive=1209600&osid=1&continue=https://console.firebase.google.com/&followup=https://console.firebase.google.com/), uitgezochte `Add Project`.
      1. Selecteer uw GCM-project in de lijst met bestaande Google Cloud-projecten en selecteer `Add Firebase` .
      1. Selecteer `Add Firebase to your Android App` in het welkomstscherm van Firebase.
      1. Geef de pakketnaam en SHA-1 op en selecteer `Add App` . Er wordt een nieuw `google-services.json` -bestand voor de Firebase-app gedownload.
      1. Selecteer `Continue` en volg de gedetailleerde instructies voor het toevoegen van de Google Services-plug-in in Android Studio.

   1. Navigeer naar &#39;Projectinstellingen&#39; in projectoverzicht
      1. Klik op het tabblad Algemeen. Download het bestand &#39;google-services.json&#39;.
      1. Klik op het tabblad Cloud Messaging. Kopieer &#39;Server-sleutel&#39; en &#39;Afzender-id&#39;. Geef deze &#39;Server Key&#39; en &#39;Sender ID&#39; op aan Marketo.
   1. FCM-wijzigingen configureren in Android App
      1. Schakel naar de projectweergave in Android Studio om de hoofdmap van uw project te bekijken
         1. Verplaats het gedownloade bestand &#39;google-services.json&#39; naar de hoofdmap van de Android-toepassingsmodule
         1. Voeg het volgende toe in build.gradle op projectniveau:

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

         1. Klik ten slotte op &quot;Nu synchroniseren&quot; in de balk die wordt weergegeven in de id
   1. Het manifest van uw app bewerken De FCM SDK voegt automatisch alle vereiste machtigingen en de vereiste ontvangerfunctionaliteit toe. Zorg ervoor dat u de volgende verouderde (en mogelijk schadelijke) elementen uit het manifest van uw app verwijdert:

      ```xml
      <uses-permission android:name="android.permission.WAKE_LOCK" />
      <permission android:name="<your-package-name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />
      <uses-permission android:name="<your-package-name>.permission.C2D_MESSAGE" />
      
      ...
      
      <receiver>
        android:name="com.google.android.gms.gcm.GcmReceiver"
        android:exported="true"
        android:permission="com.google.android.c2dm.permission.SEND"
        <intent-filter>
          <action android:name="com.google.android.c2dm.intent.RECEIVE" />
          <category android:name="<your-package-name> />
        </intent-filter>
      </receiver>
      ```
