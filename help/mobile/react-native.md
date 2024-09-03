---
title: React Native
feature: Mobile Marketing
description: React Native voor Marketo installeren
exl-id: 462fd32e-91f1-4582-93f2-9efe4d4761ff
source-git-commit: e7cb23c4d578d949553b2b7a6e127d6be54cdf23
workflow-type: tm+mt
source-wordcount: '811'
ht-degree: 0%

---

# React Native

In dit artikel vindt u informatie over het installeren en instellen van een eigen SDK voor Marketo om uw mobiele app te integreren met ons platform.

## Vereisten

[ voeg een toepassing in Marketo Admin ](https://experienceleague.adobe.com/en/docs/marketo/using/product-docs/mobile-marketing/admin/add-a-mobile-app) toe (verkrijg uw toepassing Geheime Sleutel en identiteitskaart Munchkin).

## SDK-integratie

### Android SDK-integratie

**Opstelling die Gradle** gebruikt

Voeg de Marketo SDK-afhankelijkheid toe met de nieuwste versie: voeg onder de sectie voor afhankelijkheden in het bestand op toepassingsniveau `build.gradle` toe (inclusief de juiste versie van Marketo SDK)

```
implementation 'com.marketo:MarketoSDK:0.x.x'
```

**voeg mavencentcentrale bewaarplaats** toe

Marketo SDK is beschikbaar op de [ Gemaakt centrale bewaarplaats ](https://mvnrepository.com/). Als u deze bestanden wilt synchroniseren, voegt u `mavencentral` repository toe aan root `build.gradle`

```
build script {
  repositories {
    google()
    mavencentral()
  }
}
```

Synchroniseer vervolgens uw project met de bestanden in de grijze ruimte.

#### iOS SDK-integratie

Voordat u een bridge voor uw React Native-project maakt, is het belangrijk dat u onze SDK instelt in uw Xcode-project.

**integratie SDK - Gebruikend CocoaPods**

Het is eenvoudig om de SDK van iOS in uw app te gebruiken. Voer de volgende stappen uit om dit in het Xcode-project van uw app in te stellen met CocoaPods, zodat u ons platform kunt integreren met uw app.

De download [ CocoaPods ](https://cocoapods.org/) - als Ruby gem wordt gedistribueerd, is het een gebiedsbeheer voor doelstelling-C en Swift die het proces om derdebibliotheken in uw code, zoals iOS SDK te gebruiken vereenvoudigt.

Als u het wilt downloaden en installeren, start u een opdrachtregelterminal op uw Mac en voert u de volgende opdracht uit:

1. CocoaPods installeren.

`$ sudo gem install cocoapods`

1. Open het podbestand. (In iOS-map van het ReactNative-project)

`$ open -a Xcode Podfile`

1. Voeg de volgende regel toe aan het podbestand.

`$ pod 'Marketo-iOS-SDK'`

1. Sla het bestand op en sluit het.

1. Download en installeer Marketo iOS SDK.

`$ pod install`

1. De werkruimte openen in Xcode.

`$ open App.xcworkspace`

## Installatie-instructies voor de native module

Soms moet een React Native-app toegang krijgen tot een native platform-API die niet standaard beschikbaar is in JavaScript, bijvoorbeeld de native API&#39;s voor toegang tot Apple of Google Pay. Misschien wilt u sommige bestaande objectc-, SWIFT-, Java- of C++-bibliotheken opnieuw gebruiken zonder dat u deze opnieuw moet implementeren in JavaScript, of u wilt krachtige, multi-threaded code schrijven voor bijvoorbeeld beeldverwerking.

Het NativeModule-systeem stelt instanties van Java/Objectieve C/C++ (native) klassen als JS-objecten beschikbaar voor JavaScript (JS), zodat u willekeurige native code vanuit JS kunt uitvoeren. Hoewel we niet verwachten dat deze functie deel uitmaakt van het gebruikelijke ontwikkelingsproces, is het van essentieel belang dat deze bestaat. Als React Native geen native API exporteert die uw JS-app nodig heeft, kunt u deze zelf exporteren!

React Native-bridge wordt gebruikt voor communicatie tussen de JSX- en native toepassingslagen. In ons geval kan de host-app de JSX-code schrijven die de methoden van de Marketo SDK kan aanroepen.

### Android

Dit bestand bevat de omvattende methoden die de methoden van de Marketo SDK intern kunnen aanroepen met parameters die u opgeeft.

```
public class RNMarketoModule extends ReactContextBaseJavaModule {

   final Marketo marketoSdk;
   RNMarketoModule(ReactApplicationContext context) {
       super(context);
       marketoSdk = Marketo.getInstance(context);
   }

   @NonNull
   @Override
   public String getName() {
       return "RNMarketoModule";
   }

   @ReactMethod
   public void associateLead(ReadableMap leadData) {

       MarketoLead mLead = new MarketoLead();
       try {
           mLead.setCity(leadData.getString(MarketoLead.KEY_CITY));
           mLead.setFirstName(leadData.getString(MarketoLead.KEY_FIRST_NAME));
           mLead.setLastName(leadData.getString(MarketoLead.KEY_LAST_NAME));
           mLead.setAddress(leadData.getString(MarketoLead.KEY_ADDRESS));
           mLead.setEmail(leadData.getString(MarketoLead.KEY_EMAIL));
           mLead.setBirthDay(leadData.getString(MarketoLead.KEY_BIRTHDAY));
           mLead.setCountry(leadData.getString(MarketoLead.KEY_COUNTRY));
           mLead.setFacebookId(leadData.getString(MarketoLead.KEY_FACEBOOK));
           mLead.setGender(leadData.getString(MarketoLead.KEY_GENDER));
           mLead.setState(leadData.getString(MarketoLead.KEY_STATE));
           mLead.setPostalCode(leadData.getString(MarketoLead.KEY_POSTAL_CODE));
           mLead.setTwitterId(leadData.getString(MarketoLead.KEY_TWITTER));
           marketoSdk.associateLead(mLead);
       }
       catch (MktoException e){
       }
   }
   @ReactMethod
   public void setSecureSignature(ReadableMap readableMap) {
       MarketoConfig.SecureMode secureMode = new MarketoConfig.SecureMode();
       secureMode.setAccessKey(readableMap.getString("accessKey"));
       secureMode.setEmail(readableMap.getString("email"));
       secureMode.setSignature(readableMap.getString("signature"));
       secureMode.setTimestamp(readableMap.getInt("timeStamp"));
       marketoSdk.setSecureSignature(secureMode);
   }
   @ReactMethod
      public void initializeSDK(String frameworkType, String munchkinId, String appSecreteKey){
          marketoSdk.initializeSDK(munchkinId,appSecreteKey,frameworkType);
    }
   

   @ReactMethod
   public void initializeMarketoPush(String projectId){
       marketoSdk.initializeMarketoPush( projectId);
   }

   @ReactMethod
   public void initializeMarketoPush(String projectId, String channelName){
       marketoSdk.initializeMarketoPush( projectId, channelName);
   }

   @ReactMethod
   public void uninitializeMarketoPush(){
       marketoSdk.uninitializeMarketoPush();
   }

   @ReactMethod
   public void reportAction(String action){
       Marketo.reportAction(action, null);
   }

   @ReactMethod
   public void reportAction(String action, ReadableMap readableMap){
       MarketoActionMetaData marketoActionMetaData = new MarketoActionMetaData();
       marketoActionMetaData.setActionDetails(readableMap.getString("setMetric"));
       marketoActionMetaData.setActionMetric(readableMap.getString("setLength"));
       marketoActionMetaData.setActionLength(readableMap.getString("actionDetails"));
       marketoActionMetaData.setActionType(readableMap.getString("actionType"));
       Marketo.reportAction(action, marketoActionMetaData);
   }
}
```

**registreer het Pakket**

Laat het Marketo-pakket reageren.

```
public class MarketoPluginPackage implements ReactPackage {

   @NonNull
   @Override
   public List createNativeModules(@NonNull ReactApplicationContext reactContext) {
           List modules = new ArrayList<>();

           modules.add(new RNMarketoModule(reactContext));

           return modules;    
   }

   @NonNull
   @Override
   public List createViewManagers(@NonNull ReactApplicationContext reactContext) {
       return Collections.emptyList();
   }
}
```

Als u de pakketregistratie wilt voltooien, voegt u het MarketoPluginPackage toe aan de pakketlijst React in de toepassingsklasse:

```
public class MainApplication extends Application implements ReactApplication {
 
  private final ReactNativeHost mReactNativeHost =
      new ReactNativeHost(this) {
        @Override
        public boolean getUseDeveloperSupport() {
          return BuildConfig.DEBUG;
        }
 
        @Override
        protected List getPackages() {
          @SuppressWarnings("UnnecessaryLocalVariable")
          List packages = new PackageList(this).getPackages();
          packages.add(new MarketoPluginPackage());     //Add the Marketo Package here.
          // Packages that cannot be autolinked yet can be added manually here, for example:
          return packages;
        }
}
```

### iOS

In de volgende gids zult u een inheemse module, _RNMarketoModule_ creëren, die u zal toestaan om tot Marketo APIs van JavaScript toegang te hebben.

Open om aan de slag te gaan het iOS-project in uw React Native-toepassing in Xcode. U vindt uw iOS-project hier in een React Native-app. We raden u aan Xcode te gebruiken om uw eigen code te schrijven. Xcode is gemaakt voor iOS-ontwikkeling en als u deze code gebruikt, kunt u snel kleinere fouten oplossen, zoals de syntaxis van code.

Maak onze hoofd- en implementatiebestanden van de aangepaste native module. Maak een nieuw bestand met de naam `MktoBridge.h` en voeg er het volgende aan toe:

```
//
//  MktoBridge.h
//
//  Created by Marketo, An Adobe company.
//

#import <Foundation/Foundation.h>
#import <React/RCTBridgeModule.h>

NS_ASSUME_NONNULL_BEGIN

@interface MktoBridge : NSObject 

@end

NS_ASSUME_NONNULL_END
```

Maak het corresponderende implementatiebestand `MktoBridge.m` in dezelfde map en voeg de volgende inhoud toe:

```
//
//  MktoBridge.h
//  Created by Marketo, An Adobe company.
//

#import <Foundation/Foundation.h>
#import <React/RCTBridgeModule.h>

NS_ASSUME_NONNULL_BEGIN

@interface MktoBridge : NSObject <RCTBridgeModule>

@end

NS_ASSUME_NONNULL_END


//
//  MktoBridge.m
//  Created by Marketo, An Adobe company.
//

#import "MktoBridge.h"
#import <MarketoFramework/Marketo.h>
#import <React/RCTBridge.h>
#import "ConstantStringsHeader.h"

@implementation MktoBridge 

RCT_EXPORT_MODULE(RNMarketoModule);
 
+(BOOL)requiresMainQueueSetup{
  return NO;
}
 
RCT_EXPORT_METHOD(initializeSDK:(NSString *) munchkinId SecretKey: (NSString *) secretKey andFrameworkType: (NSString *) frameworkType){
  [[Marketo sharedInstance] initializeWithMunchkinID:munchkinId appSecret:secretKey mobileFrameworkType:frameworkType launchOptions:nil];
}
 
RCT_EXPORT_METHOD(reportAction:(NSString *)actionName withMetaData:(NSDictionary *)metaData){
  MarketoActionMetaData *meta = [[MarketoActionMetaData alloc] init];
  [meta setType:[metaData objectForKey:KEY_ACTION_TYPE]];
  [meta setDetails:[metaData objectForKey:KEY_ACTION_DETAILS]];
  [meta setLength:[metaData valueForKey:KEY_ACTION_LENGTH]];
  [meta setMetric:[metaData valueForKey:KEY_ACTION_METRIC]];
  [[Marketo sharedInstance] reportAction:actionName withMetaData:meta];
}
 
RCT_EXPORT_METHOD(associateLead:(NSDictionary *)leadDetails){
  MarketoLead *lead = [[MarketoLead alloc] init];
  if ([leadDetails objectForKey:KEY_EMAIL] != nil) {
    [lead setEmail:[leadDetails objectForKey:KEY_EMAIL]];
  }
  if ([leadDetails objectForKey:KEY_FIRST_NAME] != nil) {
    [lead setFirstName:[leadDetails objectForKey:KEY_FIRST_NAME]];
  }
  
  if ([leadDetails objectForKey:KEY_LAST_NAME] != nil) {
    [lead setLastName:[leadDetails objectForKey:KEY_LAST_NAME]];
  }
  
  if ([leadDetails objectForKey:KEY_CITY] != nil) {
    [lead setCity:[leadDetails objectForKey:KEY_CITY]];
  }
    [[Marketo sharedInstance] associateLead:lead];
}
 
RCT_EXPORT_METHOD(uninitializeMarketoPush){
  [[Marketo sharedInstance] unregisterPushDeviceToken];
}
 
RCT_EXPORT_METHOD(reportAll){
  [[Marketo sharedInstance] reportAll];
}
 
RCT_EXPORT_METHOD(setSecureSignature:(NSDictionary *)secureSignature){
  MKTSecuritySignature *secSignature = [[MKTSecuritySignature alloc]
                                        initWithAccessKey:[secureSignature objectForKey:KEY_ACCESSKEY]
                                        signature:[secureSignature objectForKey:KEY_SIGNATURE]
                                        timestamp: [secureSignature objectForKey:KEY_EMAIL]
                                        email:[secureSignature objectForKey:KEY_EMAIL]];
  
    [[Marketo sharedInstance] setSecureSignature:secSignature];
}

RCT_EXPORT_METHOD(requestPermission:(RCTPromiseResolveBlock)resolve rejecter:(RCTPromiseRejectBlock)reject) {
  UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];
  [center requestAuthorizationWithOptions:(UNAuthorizationOptionAlert + UNAuthorizationOptionSound + UNAuthorizationOptionBadge)
                        completionHandler:^(BOOL granted, NSError * _Nullable error) {
    if (error) {
      reject(@"PERMISSION_ERROR", @"Permission request failed", error);
    } else {
      resolve(@(granted));
    }
  }];
}

RCT_EXPORT_METHOD(registerForRemoteNotifications) {
  dispatch_async(dispatch_get_main_queue(), ^{
    [[UIApplication sharedApplication] registerForRemoteNotifications];
  });
}


@end
```

#### Marketo SDK initialiseren

Zoek een plaats in uw toepassing waar u een vraag aan de methode createCalendarEvent () van de inheemse module zou willen toevoegen. Hieronder ziet u een voorbeeld van een component, NewModuleButton die u in uw app kunt toevoegen. U kunt de native module aanroepen in de onPress()-functie van NewModuleButton.

```
import React from 'react';
import { NativeModules, Button } from 'react-native';

const NewModuleButton = () => {
  const onPress = () => {
    console.log('We will invoke the native module here!');
  };

  return (
    
  );
  };

export default NewModuleButton;
```

Dit JavaScript-bestand laadt de native module naar de JavaScript-laag.

```javascript
import React from 'react';
import {Node} from 'react';
import { NativeModules } from 'react-native';

const { RNMarketoModule } = NativeModules;
```

Als de bovenstaande bestanden correct zijn geplaatst, kunnen we de JS-module importeren in elke JS-klasse en de methoden ervan rechtstreeks aanroepen. Bijvoorbeeld:

Merk op dat wij &quot;responseNative&quot;als kadertype voor React inheemse apps moeten overgaan. 

```
// Initialize marketo SDK with Munchkin & Seretkey you have from step 1.
RNMarketoModule.initializeSDK("MunchkinID","SecreteKEY","FrameworkType")

//You can create a Marketo Lead by calling the associateLead function.
RNMarketoModule.associateLead({ email: "", firstName: "", lastName:"", city:""})

//You can report any user performed action by calling the reportaction function.
RNMarketoModule.reportAction("Bought Shirt", {actionType:"Shopping", actionDetails: "Red Shirt", setLength : 20, setMetric : 30 })

//You can set Secure Signature by calling this method.
RNMarketoModule.setSecureSignature({accessKey: "Key102", email: "testleadrk@001.com", signature : "asdfghjkloiuytds", timeStamp: "12345678987654"})

//This function will Enable user notifications (Only Android)
RNMarketoModule.initializeMarketoPush("350312872033", "MKTO")

//The token can also be unregistered on logout.
RNMarketoModule.uninitializeMarketoPush()
```

#### Pushmeldingen configureren

Initialiseer Duwen met project-id en kanaalnaam

```
RNMarketoModule.initializeMarketoPush("ProjectId", "Channel_name")
```

Voeg de volgende service toe aan `AndroidManifest.xml`

```xml
<service android:exported="true" android:name=".MyFirebaseMessagingService" android:stopWithTask="true">
    <intent-filter>
        <action  android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
    </intent-filter/>
    <intent-filter> 
        <action android:name="com.google.firebase.MESSAGING_EVENT"/> 
    </intent-filter/>
</activity/>
```

Een klasse met de naam `FirebaseMessagingService.java` maken en de volgende code toevoegen

```java
import com.google.firebase.messaging.FirebaseMessagingService;
import com.google.firebase.messaging.RemoteMessage;
import com.marketo.Marketo;

public class MyFirebaseMessagingService extends FirebaseMessagingService {

   @Override
   public void onNewToken(String token) {
       super.onNewToken(token);
       Marketo marketoSdk = Marketo.getInstance(this.getApplicationContext());
       marketoSdk.setPushNotificationToken(token);
   }

   @Override
   public void onMessageReceived(RemoteMessage remoteMessage) {
       Marketo marketoSdk = Marketo.getInstance(this.getApplicationContext());
       marketoSdk.showPushNotification(remoteMessage);
   }
}
```

De toestemmingen moeten in uw project van Xcode worden toegelaten om pushberichten naar het apparaat van de gebruiker te verzenden.

Om pushberichten te verzenden, [ voeg Push Meldingen ](push-notifications.md) toe.

Stel iOS Push-berichten in.
Maak het bestand PushNotifications.tsx en voeg het volgende toe:

```
import { NativeModules } from 'react-native';
const { RNMarketoModule } = NativeModules;

const requestPermission = (): Promise<boolean> => {
return RNMarketoModule.requestPermission()
.then((granted: boolean) => {
console.log('Permission granted:', granted);
return granted;
})
.catch((error: any) => {
console.error('Permission error:', error);
throw error;
});
};

const registerForRemoteNotifications = (): void => {
RNMarketoModule.registerForRemoteNotifications();
};

export { requestPermission, registerForRemoteNotifications };
```


`App.tsx` toevoegen om pushberichten toe te staan

```
import React, { useEffect } from 'react';

useEffect(() => {
requestPermission().then(granted => {
if (granted) {
registerForRemoteNotifications();
}
});
}, []);
```

`AppDelegate.mm` bijwerken met APNS-gedelegeerde methoden:

```
#import "AppDelegate.h"
#import "MktoBridge.h"
#import <MarketoFramework/Marketo.h>
#import <React/RCTBundleURLProvider.h>

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  self.moduleName = @"MyNewApp";
  // You can add your custom initial props in the dictionary below.
  // They will be passed down to the ViewController used by React Native.
  self.initialProps = @{};

  return [super application:application didFinishLaunchingWithOptions:launchOptions];
}

- (NSURL *)sourceURLForBridge:(RCTBridge *)bridge
{
  return [self bundleURL];
}

-(void)userNotificationCenter:(UNUserNotificationCenter *)center 
      willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler{
    completionHandler(UNAuthorizationOptionSound | UNAuthorizationOptionAlert | UNAuthorizationOptionBadge);
}

- (void)userNotificationCenter:(UNUserNotificationCenter *)center
didReceiveNotificationResponse:(UNNotificationResponse *)response
         withCompletionHandler:(void(^)(void))completionHandler {
    [[Marketo sharedInstance] userNotificationCenter:center 
                      didReceiveNotificationResponse:response
                               withCompletionHandler:completionHandler];
}

- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
    // Register the push token with Marketo
    [[Marketo sharedInstance] registerPushDeviceToken:deviceToken];
}

- (void)applicationWillTerminate:(UIApplication *)application {
    [[Marketo sharedInstance] unregisterPushDeviceToken];
}

-(void)application:(UIApplication *)application didFailToRegisterForRemoteNotificationsWithError:(NSError *)error{
  NSLog(@"Failed to register for remote notification - %@", [error userInfo]);
}

- (NSURL *)bundleURL
{
#if DEBUG
  return [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index"];
#else
  return [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"];
#endif
}

@end
```

### Testapparaten toevoegen

**Android**

Voeg &quot;MarketoActivity&quot; toe aan het `AndroidManifest.xml` -bestand in de toepassingstag.

```xml
<activity android:name="com.marketo.MarketoActivity" android:configChanges="orientation|screenSize" android:exported="true">
    <intent-filter android:label="MarketoActivity">
        <action  android:name="android.intent.action.VIEW"/>
        <category  android:name="android.intent.category.DEFAULT"/>
        <category  android:name="android.intent.category.BROWSABLE"/>
        <data android:host="add_test_device" android:scheme="mkto"/>
    </intent-filter/>
</activity/>
```

**iOS**

1. Selecteer Project > Doel > Info > URL-typen.

1. Id toevoegen: ${PRODUCT_NAME}

1. URL-schema&#39;s instellen: `mkto-<S_ecret Key_>`

1. `application:openURL:sourceApplication:annotation:` opnemen in `AppDelegate.m` -bestand (doelstelling-C)

**iOS - het Type/Deplinks van de Douane van de Handle in AppDelegate** 

```
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary *)options{
   
    return [[Marketo sharedInstance] application:app
                                         openURL:url
                                         options:options];    
}
```

Deze constanten worden gebruikt wanneer API wordt aangeroepen vanuit javascript. U moet constante bestanden maken en het volgende toevoegen.

```
// Lead attributes.
static NSString *const KEY_FIRST_NAME = @"firstName";
static NSString *const KEY_LAST_NAME = @"lastName";
static NSString *const KEY_ADDRESS = @"address";
static NSString *const KEY_CITY = @"city";
static NSString *const KEY_STATE = @"state";
static NSString *const KEY_COUNTRY = @"country";
static NSString *const KEY_GENDER = @"gender";
static NSString *const KEY_EMAIL = @"email";
static NSString *const KEY_TWITTER = @"twitterId";
static NSString *const KEY_FACEBOOK = @"facebookId";
static NSString *const KEY_LINKEDIN = @"linkedinId";
static NSString *const KEY_LEAD_SOURCE = @"leadSource";
static NSString *const KEY_BIRTHDAY = @"dateOfBirth";
static NSString *const KEY_FACEBOOK_PROFILE_URL = @"facebookProfileURL";
static NSString *const KEY_FACEBOOK_PROFILE_PIC = @"facebookPhotoURL";

// Custom actions
static NSString *const KEY_ACTION_TYPE = @"actionType";
static NSString *const KEY_ACTION_DETAILS = @"actionDetails";
static NSString *const KEY_ACTION_LENGTH = @"setLength";
static NSString *const KEY_ACTION_METRIC = @"setMetric";

//Secure Signature
static NSString *const KEY_ACCESSKEY = @"accessKey";
static NSString *const KEY_SIGNATURE = @"signature";
static NSString *const KEY_TIMESTAMP = @"timeStamp";
```

Voorbeeldengebruik

```
//You can create a Marketo Lead by calling the associateLead function.
RNMarketoModule.associateLead({ email: "", firstName: "", lastName:"", city:""})
```
