---
title: "Azure Mobile Engagement iOS SDK tümleştirmesi | Microsoft Docs"
description: "En son güncelleştirmeler ve iOS için Azure Mobile Engagement SDK'sı için yordamlar"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 947ea44b-00c1-450f-9a3b-74437954dc56
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 01fdbb43c21ac6932e8462f4a6507fc63e50542d
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="how-to-integrate-engagement-on-ios"></a>Katılım tümleştirmek için iOS hakkında
> [!div class="op_single_selector"]
> * [Windows Evrensel](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-integrate-engagement.md)
>
>

Bu yordam, Engagement analizi ve izleme işlevlerine iOS uygulamanızda etkinleştirmek için en basit yolu açıklar.

Engagement SDK'sı iOS7 + ve Xcode 8 + gerektirir: uygulamanızı dağıtım hedefi en az olmalıdır iOS 7.

> [!NOTE]
> XCode 7'de gerçekten bağımlı sonra kullanabilirsiniz [iOS Engagement SDK'sı v3.2.4](https://aka.ms/r6oouh). Bir bilinen hata varsa bu önceki sürüm Reach modülünü 10 iOS aygıtları bakın çalıştırılırken [reach modülü tümleştirme](mobile-engagement-ios-integrate-engagement-reach.md) daha fazla ayrıntı için. SDK v3.2.4 kullanın sonra yalnızca atlamak seçerseniz `UserNotifications.framework` sonraki adımda içeri aktarın.
>
>

Aşağıdaki adım kullanıcıları, oturumlar, etkinlikleri, kilitlenme ve Technicals ilgili tüm istatistikleri işlem için gereken günlükleri rapor etkinleştirmek için yeterli değildir. Olaylar, hatalar ve işleri gibi diğer istatistikleri işlem için gereken günlükleri rapor katılım API kullanarak el ile yapılması gerekir (bkz [iOS uygulamanızı API etiketleme Gelişmiş Mobile Engagement kullanmayı](mobile-engagement-ios-use-engagement-api.md) Bu istatistikler olduğundan uygulamaya bağlıdır.

## <a name="embed-the-engagement-sdk-into-your-ios-project"></a>Engagement SDK'sı iOS projenize ekleme
* İOS SDK'sı yükle gelen [burada](http://aka.ms/qk2rnj).
* Engagement SDK'sı iOS projenize ekleyin: Xcode'da, sağ, proje seçin tıklayın ve **"Ekle dosyaları..."** ve `EngagementSDK` klasör.
* Katılım çalışmak için ek çerçeveleri gerektirir: Proje Gezgini'nde proje bölmesini açın ve doğru hedef seçin. Ardından, açın **"Derleme aşamaları"** sekmesi ve **"Bağlantı ikiliyi kitaplıklara"** menüsünde bu çerçeveleri ekleyin:

  * `UserNotifications.framework`-bağlantı olarak ayarla`Optional`
  * `AdSupport.framework`-bağlantı olarak ayarla`Optional`
  * `SystemConfiguration.framework`
  * `CoreTelephony.framework`
  * `CFNetwork.framework`
  * `CoreLocation.framework`
  * `libxml2.dylib`

> [!NOTE]
> AdSupport framework kaldırılabilir. Katılım IDFA toplamak için bu framework gerekir. Ancak, IDFA toplamayı devre \<ios-sdk-engagement-ıdfa\> bu kimliğiyle ilgili yeni Apple ilkesiyle uyum sağlamak için
>
>

## <a name="initialize-the-engagement-sdk"></a>Engagement SDK'yı başlatma
Uygulama temsilcinizi değiştirme gerekir:

* Uygulama dosyanızı üst kısmında, katılım Aracısı'nı içeri aktarın:

      [...]
      #import "EngagementAgent.h"
* Katılım Initialize yöntemi içinde '**applicationDidFinishLaunching:**'veya'**uygulama: didFinishLaunchingWithOptions:**':

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
      {
        [...]
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
        [...]
      }

## <a name="basic-reporting"></a>Temel raporlama
### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a>Önerilen yöntem: aşırı yükleme, `UIViewController` sınıfları
Rapor kullanıcıları, oturumlar, etkinlikleri, kilitlenme ve teknik istatistikleri işlem katılım tarafından gerekli tüm günlüklerin etkinleştirmek için yalnızca tüm yapabilirsiniz, `UIViewController` alt sınıfları `EngagementViewController` sınıfları (aynı kural için`UITableViewController`  -\> `EngagementTableViewController`).

**Katılım:**

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

**Katılım ile:**

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a>Alternatif yöntem: çağrı `startActivity()` el ile
Olamaz ya da tekrar etmek istiyor musunuz, `UIViewController` sınıfları, bunun yerine, çağırarak etkinliklerinizi başlatabilirsiniz `EngagementAgent`'s doğrudan yöntemleri.

> [!IMPORTANT]
> SDK otomatik olarak çağıran iOS `endActivity()` uygulama kapatıldığında yöntemi. Bu nedenle, olan *yüksek oranda* çağırmak için önerilen `startActivity` kullanıcı etkinliği değiştirdiğinizde, yöntemi ve *hiçbir zaman* çağrısı `endActivity` bu yöntemi çağırmadan geçerli zorlar beri yöntemi tamamlandı olarak oturumu.
>
>

## <a name="location-reporting"></a>Konum raporlama
Hizmet Koşulları Apple uygulamaları konum yalnızca istatistikleri amaçla izleme kullanmaya izin vermez. Bu nedenle, yalnızca uygulamanız konumu başka bir nedenle izleme de kullanıyorsanız konumu raporları etkinleştirmek için önerilir.

İOS 8 ile başlayarak, anahtar için bir dize ayarlayarak uygulamanızı konum hizmetleri nasıl kullandığı için bir açıklama sağlamalısınız [NSLocationWhenInUseUsageDescription] veya [NSLocationAlwaysUsageDescription]uygulamanızın Info.plist dosyasında. Rapor konumu engagement arka planda istiyorsanız NSLocationAlwaysUsageDescription anahtarı ekleyin. Diğer durumlarda, NSLocationWhenInUseUsageDescription anahtarı ekleyin. Rapor arka plan konumu iOS 11 NSLocationAlwaysAndWhenInUseUsageDescription ve NSLocationWhenInUseUsageDescription gerekir unutmayın.

### <a name="lazy-area-location-reporting"></a>Yavaş alan konumu raporlama
Yavaş alan konumu raporlama ülke, bölgeye ve yere göre cihazlara ilişkili rapor oluşturmaya olanak tanır. Bu tür konumu raporlama yalnızca ağ konumlarını (hücre kimliği veya WIFI göre) kullanır. Aygıt alanına oturum başına en fazla bir kez bildirilir. GPS hiçbir zaman kullanılmaz ve bu nedenle bu konumu rapor çok az (no değil söylemeniz) türü pil üzerindeki etkisi.

Bildirilen alanları kullanıcıları, oturumlar, olayları ve hataları ile ilgili coğrafi istatistikleri hesaplamak için kullanılır. Reach kampanyaları ölçütü olarak de kullanılabilir. Bir aygıt teşekkürler alınabilir için son bilinen alanı bildirilen [aygıt API].

Yavaş alan konumu raporlama etkinleştirmek için katılım Aracısı'nı başlatma sonra aşağıdaki satırı ekleyin:

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a>Gerçek zamanlı konum raporlama
Gerçek zamanlı konum raporlama enlem ve boylam cihazlara ilişkili rapor oluşturmaya olanak tanır. Varsayılan olarak, bu tür konumu raporlama yalnızca ağ konumlarını (hücre kimliği veya WIFI göre) kullanır ve uygulama ön planda (yani oturumu sırasında) çalıştığında raporlama yalnızca etkindir.

Gerçek zamanlı konumlarının *değil* istatistikleri hesaplamak için kullanılır. Gerçek zamanlı coğrafi yalıtma kullanımına izin vermek için kendi tek amacı olan \<Reach-İzleyici-bölge sınırlaması\> Reach kampanyaları ölçütü.

Gerçek zamanlı konum raporlama etkinleştirmek için katılım Aracısı'nı başlatma sonra aşağıdaki satırı ekleyin:

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a>Raporlama GPS dayalı
Varsayılan olarak, gerçek zamanlı konum raporlama ağ tabanlı konum yalnızca kullanır. (Olan daha kesin) tabanlı GPS konumları kullanımını etkinleştirmek için ekleyin:

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a>Arka plan raporlama
Uygulama ön planda (yani oturumu sırasında) çalıştırdığında, varsayılan olarak, gerçek zamanlı konum raporlama yalnızca etkindir. Aynı zamanda arka planda raporlamayı etkinleştirmek için ekleyin:

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [!NOTE]
> Tabanlı ağ konumları yalnızca uygulama arka planda çalıştığında raporlanır, GPS etkin olsa bile.
>
>

Bu işlev uygulaması çağıracaktır [startMonitoringSignificantLocationChanges] uygulamanızın arka planda gittiğinde. Otomatik olarak yeni bir konum olay gelirse arka uygulamanıza yeniden başlatmak olduğunu unutmayın.

## <a name="advanced-reporting"></a>Gelişmiş raporlama
Uygulama belirli olaylar, hatalar ve işleri rapor istiyorsanız, isteğe bağlı olarak, katılım API yöntemlerini kullanmanız gerekebilir `EngagementAgent` sınıfı. Bu sınıfın bir nesnesi çağırarak alınabilir `[EngagementAgent shared]` statik yöntemi.

Katılım API tüm Engagement'ın gelişmiş özelliklerinden kullanacak şekilde sağlar ve nasıl ayrıntılı İos'ta katılım API kullanmak için (teknik belgeleri olarak yanı `EngagementAgent` sınıfı).

## <a name="disable-idfa-collection"></a>IDFA toplamayı devre dışı bırak
Varsayılan olarak, katılım kullanacağı [IDFA] bir kullanıcıyı benzersiz şekilde tanımlamak için. Ancak başka bir uygulamada reklam kullanmıyorsanız, App Store gözden geçirme işlemi tarafından reddedilmiş. IDFA toplamayı önişlemci makrosu ekleyerek devre dışı bırakılacak `ENGAGEMENT_DISABLE_IDFA` pch dosyanızdaki (veya `Build Settings` uygulamanızın). Bu başvuru olduğundan emin `ASIdentifierManager`, `advertisingIdentifier` veya `isAdvertisingTrackingEnabled` uygulama oluşturma.

Tümleştirme sırasında **prefix.pch** dosyası:

    #define ENGAGEMENT_DISABLE_IDFA
    ...

IDFA toplamayı düzgün uygulamanızda katılım test günlüklerinin denetleyerek devre dışı olduğunu doğrulayabilirsiniz. Bkz: tümleştirme sınama\<ios-sdk-engagement-test-ıdfa\> daha fazla bilgi için.

## <a name="disable-log-reporting"></a>Günlük bildirimini devre dışı bırak
### <a name="using-a-method-call"></a>Yöntem çağrısı kullanma
Engagement'ın günlükleri göndermek durdurmak istiyorsanız, çağırabilirsiniz:

    [[EngagementAgent shared] setEnabled:NO];

Bu çağrı kalıcıdır: kullandığı `NSUserDefaults` bilgileri depolamak için.

Yeniden ile aynı işlevini çağırarak raporlama günlüğü etkinleştirebilirsiniz `YES`.

### <a name="integration-in-your-settings-bundle"></a>Ayarlar paketi tümleştirme
Bu işlevi çağırmak yerine, ayrıca bu ayarı doğrudan var olan tümleştirebilirsiniz `Settings.bundle` dosya. Dize `engagement_agent_enabled` olarak kullanılması gereken bir tercih tanımlayıcısı ve bir geçiş anahtara ilişkilendirilmiş olması gerekir (`PSToggleSwitchSpecifier`).

Aşağıdaki örnekte `Settings.bundle` uyguladıktan gösterilmektedir:

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
[aygıt API]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier
