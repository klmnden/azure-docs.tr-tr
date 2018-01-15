---
title: "Uygulamalar arası SSO'nun ADAL kullanarak iOS etkinleştirme | Microsoft Docs"
description: "Çoklu oturum açma, uygulamalar arasında etkinleştirmek için ADAL SDK özelliklerini kullanma "
services: active-directory
documentationcenter: 
author: brandwe
manager: mtillman
editor: 
ms.assetid: d042d6da-7503-4e20-bb55-06917de01fcd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 04/07/2017
ms.author: brandwe
ms.custom: aaddev
ms.openlocfilehash: a7d93fe6289ade7fbdf3050d49184feb8b370bb5
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="how-to-enable-cross-app-sso-on-ios-using-adal"></a>Uygulamalar arası SSO'nun ADAL kullanarak iOS etkinleştirme
Kullanıcıların yalnızca bir kez kimlik bilgilerini girin ve bu kimlik bilgilerini otomatik olarak gerekir böylece çoklu oturum açma (SSO) iş arasında sağlayan uygulamalar artık müşteriler tarafından bekleniyordu. Ekranda bir telefon araması veya ilerideki kodu gibi ek bir etmen (2FA) kez birlikte küçük, genellikle kullanıcı adı ve parola girme zorluk hızlı memnuniyetsizliği kullanıcı sonuçlarında ürününüz için birden fazla kez bunun var.

Ayrıca, Microsoft Accounts veya Office365 iş hesabından gibi diğer uygulamaları kullanabilir bir kimlik platformu uygularsanız, müşteriler bu kimlik bilgileri, tüm uygulamalarını satıcı olsun genelinde kullanılabilir olmasını bekler.

Bizim Microsoft Identity SDK ile birlikte Microsoft Identity platform bu sabit iş sizin için yapar ve SSO müşterilerinizle kendi uygulama paketinin içinde ya da delight olanağı sağlar veya bizim Aracısı özellik ve tüm aygıt arasında kimlik doğrulayıcı uygulamalar ile.

Bu kılavuz, müşterilerinize Bu kolaylık sağlamak için uygulamanızda bizim SDK'yı yapılandırmak nasıl söyler.

Bu kılavuz için geçerlidir:

* Azure Active Directory
* Azure Active Directory B2C
* Azure Active Directory B2B
* Azure Active Directory Koşullu Erişim

Önceki belge bildiğiniz varsayar nasıl [sağlamak eski portalı uygulamalarında Azure Active Directory için](active-directory-how-to-integrate.md) ve uygulamanız ile tümleşik [Microsoft Identity iOS SDK'sı](https://github.com/AzureAD/azure-activedirectory-library-for-objc).

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>Microsoft Identity platformuna SSO kavramlar
### <a name="microsoft-identity-brokers"></a>Microsoft Identity aracıları
Microsoft kimlik farklı satıcılardan uygulamalar arasında köprü oluşturma için izin uygulamaları mobil her platform için sağlar ve kimlik doğrulamaya nerede tek güvenli bir yerde gerektiren özel Gelişmiş özellikleri sağlar. Bunlar diyoruz **aracıların**. İOS ve Android cihazlarda bu aracıların müşteriler bağımsız olarak yüklemek veya bazı veya tüm cihaz için kendi çalışan yöneten bir şirket tarafından cihaza gönderilir indirilebilir uygulamaları aracılığıyla sağlanır. Bu aracıları yönetme güvenlik yalnızca bazı uygulamalar veya ne BT yöneticileri işlemleriniz bağlı tüm cihaz desteği. Windows, bu işlev, Web kimlik doğrulama aracısı olarak teknik olarak bilinen işletim sistemi içinde yerleşik bir hesabı seçicide tarafından sağlanır.

Hakkında daha fazla bilgi için bu aracıların ve nasıl müşterilerinizin bunları okuyun Microsoft Identity platform için kullanıcıların oturum açma akışını görebileceğiniz kullanırız.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Mobil cihazlarda günlüğe kaydetme için desenleri
Kimlik cihazlarda erişime Microsoft Identity platformuna yönelik iki temel düzenlerden izleyin:

* Olmayan Aracısı Yardımlı oturum açmalar
* Yardımlı Oturum Aracısı

#### <a name="non-broker-assisted-logins"></a>Olmayan Aracısı Yardımlı oturum açmalar
Olmayan Aracısı Yardımlı satır içi uygulamayla gerçekleşir ve bu uygulama için cihazda yerel depolama kullanan oturum açma deneyimlerini oturumlardır. Bu depolama uygulamalar arasında paylaşılıyor olabilir, ancak kimlik bilgileri uygulama veya bu kimlik bilgilerini kullanarak uygulama paketi sıkı bir şekilde bağlıdır. Bir kullanıcı adı ve parola uygulama içinde girdiğinizde, bu büyük olasılıkla birçok mobil uygulamalarda tecrübe ettiklerinize.

Bu oturumlar aşağıdaki avantajlara sahiptir:

* Kullanıcı deneyimi tamamen uygulaması içinde zaten var.
* Kimlik bilgileri, çoklu oturum açma deneyimini paketiniz uygulamaları için sağlama aynı sertifika tarafından imzalanan uygulamalar arasında paylaşılabilir.
* Oturum açma deneyimi geçici denetim önce ve sonra oturum açma uygulamaya sağlanır.

Bu oturumlar aşağıdaki dezavantajları vardır:

* Kullanıcı yalnızca bu Microsoft uygulamanızı yapılandırılmış Identities arasında Microsoft Identity kullanan tüm uygulamalar boyunca çoklu oturum açma deneyimi olamaz.
* Uygulamanızı koşullu erişim gibi daha gelişmiş iş özellikleriyle kullanılamaz veya ürün Intune paketi kullanın.
* Uygulamanızı iş kullanıcıları için sertifika tabanlı kimlik doğrulamasını destekleyemez.

Microsoft Identity SDK'ları uygulamalarınızı SSO'yu etkinleştirmek için paylaşılan depolama ile nasıl bir temsilini şöyledir:

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| ADAL SDK  |  |  ADAL SDK  |  |  ADAK SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a>Yardımlı Oturum Aracısı
Aracısı destekli Aracısı uygulama içinde oluşur ve kimlik bilgilerini Microsoft Identity platform uygulanan tüm uygulamalar için cihazda paylaşmak için depolama ve aracının güvenlik kullanan oturum açma deneyimlerini oturumlardır. Bu, uygulamalarınız kullanıcılar Oturum Aracısı kullandığını anlamına gelir. İOS ve Android cihazlarda bu aracıların müşteriler bağımsız olarak yüklemeniz veya kendi kullanıcı aygıt yöneten bir şirket tarafından cihaza gönderilir indirilebilir uygulamaları aracılığıyla sağlanır. İos'ta Microsoft Authenticator uygulama bu tür bir uygulama örneğidir. Windows bu işlevsellik, Web kimlik doğrulama aracısı olarak teknik olarak bilinen işletim sistemi içinde yerleşik bir hesabı seçicide tarafından sağlanır.
Deneyimi platforma göre değişir ve bazen kullanıcılara işlemleri karışıklığa neden olabilir değilse doğru şekilde yönetilir. Facebook uygulamanın yüklü olması ve başka bir uygulamadan Facebook bağlanmak kullanırsanız bu deseni ile en biliyor. Microsoft Identity platformu aynı düzeni kullanır.

Bu bir "geçiş" Müşteri adayları iOS için kullanıcının oturum açmak istediğiniz hangi hesabı seçmek ön Burada, uygulamanızın Microsoft Authenticator uygulamaları arka gönderilir animasyon gelir.  

Android ve Windows hesabı seçicide kullanıcıya daha az kesintiye uğratan olan, uygulamanızın üzerinde görüntülenir.

#### <a name="how-the-broker-gets-invoked"></a>Aracısı'nı nasıl çağrılır
Microsoft Authenticator uygulaması gibi cihaz uyumlu bir aracı yüklenmişse Microsoft Identity SDK'ları otomatik olarak bir kullanıcı Microsoft Identity platformdan herhangi bir hesabı kullanarak oturum açması istedikleri belirttiğinde, Aracısı çağırma çalışma yapın. Bu hesap, kişisel Microsoft Account, bir iş veya Okul hesabı veya bir sağladığınız hesap ve konak B2C ve B2B Ürünlerimiz kullanarak azure'da olabilir. 

 #### <a name="how-we-ensure-the-application-is-valid"></a>Biz uygulamayı nasıl emin geçerlidir
 
 Aracısı Aracısı sağladığımız güvenlik önemli bir uygulama çağrısı kimliğinden emin olmak için gereken oturumları destekli. İOS ne Android kötü amaçlı uygulamalar "yasal uygulamanın tanımlayıcı aldatıcı" ve yasal uygulamanın amacı belirteçleri almak için yalnızca belirli bir uygulama için geçerli olan benzersiz tanımlayıcıları zorlar. Biz her zaman doğru uygulama çalışma zamanında ile iletişim kuran emin olmak için size özel redirectURI kendi uygulama Microsoft ile kaydedilirken sağlamak için geliştirici isteyin. **Geliştiricilerin bu yeniden yönlendirme URI'si nasıl oluşturabilir aşağıda ayrıntılı olarak ele alınmıştır.** Bu özel redirectURI ve uygulama için benzersiz olması için Apple App Store tarafından sağlamış uygulamanın paket Kimliğini içerir. Bir uygulama Aracısı çağırdığında Aracısı Aracısı adlı paket kimliği ile sağlamak için iOS işletim sistemi sorar. Aracısı'nı bu paket kimliği kimlik sistemimizde çağrısında Microsoft'a sağlar. Uygulamanın paket kimliği için bize kayıt sırasında geliştirici tarafından sağlanan paket kimliği eşleşmiyorsa, biz belirteçleri uygulama kaynak için erişim isteyen reddeder. Bu denetim yalnızca geliştirici tarafından kayıtlı uygulama belirteçlerini almasını sağlar.

**Microsoft Identity SDK Aracısı çağıran veya aracı olmayan Yardımlı akış kullanıyorsa, geliştirici seçimi vardır.** Bununla birlikte Geliştirici Aracısı destekli akışı kullanmayacak şekilde seçerse kullanıcı cihazda eklemiş olabilir ve kendi uygulama Microsoft, müşterilerinin koşullu erişim, Intune yönetim özellikleri ve sertifika tabanlı kimlik doğrulaması gibi sağlar iş özellikleriyle kullanılmasını engeller SSO kimlik bilgilerini kullanma avantajına kaybedersiniz.

Bu oturumlar aşağıdaki avantajlara sahiptir:

* Kullanıcı, satıcı olsun tüm kullanıcıların uygulamaları üzerinden SSO karşılaşır.
* Uygulamanız, koşullu erişim gibi daha gelişmiş iş özellikleri kullanın veya ürün Intune paketi kullanın.
* Uygulamanızı iş kullanıcıları için sertifika tabanlı kimlik doğrulamasını destekler.
* Uygulama ve kullanıcı kimliği olarak çok daha güvenli oturum açma deneyimi ek güvenlik algoritmalarını ve şifreleme ile Aracısı uygulama tarafından doğrulandı.

Bu oturumlar aşağıdaki dezavantajları vardır:

* Kimlik bilgileri seçilen sırada kullanıcının iOS uygulamanızın deneyimi dışında geçti.
* Uygulamanızda müşterileriniz için oturum açma deneyimi yönetme olanağı kaybı.

Microsoft Identity SDK'ları SSO'yu etkinleştirmek için Aracısı uygulamalarla nasıl çalıştığını bir temsilini şöyledir:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+
```

Daha iyi anlamanıza ve SSO Microsoft Identity platform ve SDK'ları kullanarak uygulamanızdaki uygulamanıza açabilmelisiniz bu arka plan bilgileri ile çağrılmak.

## <a name="enabling-cross-app-sso-using-adal"></a>ADAL kullanarak uygulamalar arası SSO'nun etkinleştirme
ADAL iOS SDK'sı burada kullandığımız için:

* Aracı olmayan Yardımlı SSO Uygulama paketiniz için Aç
* Aracısı destekli SSO desteği Aç

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Aracı olmayan için SSO üzerinde kapatma SSO Destekli
Uygulamalar arasında aracı olmayan Yardımlı SSO için Microsoft Identity SDK'ları SSO karmaşıklığını çoğunu sizin için yönetin. Bu doğru kullanıcı önbellekte bulma ve bakımı, sorgu oturum açan kullanıcıların listesini içerir.

Aşağıdakileri yapmak gereken kendi uygulamalarında SSO'yu etkinleştirmek için:

1. Tüm uygulamalar kullanıcı aynı istemci Kimliğini veya uygulama kimliği emin olun
2. Anahtarlıklar paylaştırabilirsiniz tüm uygulamalar aynı imzalama sertifikası Apple paylaşmadığından emin olun
3. Uygulamaların her biri için aynı Anahtarlık yetkilerini isteyin.
4. Microsoft Identity SDK'ları, paylaşılan Anahtarlıkta hakkında bize kullanmak istediğiniz söyleyin.

#### <a name="using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Aynı istemci Kimliğini kullanarak / uygulama kimliği paketiniz uygulamalarının tüm uygulamalar için
Belirteçleri, uygulamalar arasında paylaşmak için izin bilmeniz Microsoft Identity Platform sırayla uygulamalarınızın her birinde aynı istemci Kimliğini veya uygulama kimliği paylaşmak gerekir Bu portalda ilk uygulamanızı kaydolurken, size sağlanan benzersiz bir tanımlayıcıdır.

Aynı uygulama kimliği kullanıyorsa, farklı uygulamalar Microsoft Identity hizmetine nasıl kullanacağını merak ediyor olabilirsiniz Yanıt olan **yeniden yönlendirme URI'ler**. Her uygulamanın birden çok yeniden yönlendirme hazırlama Portalı'nda kayıtlı URI'ler olabilir. Her uygulama paketiniz içinde farklı bir yeniden yönlendirme URI'sine sahip olur. Bu sistem, bir örnek aşağıda verilmiştir:

App1 yeniden yönlendirme URİ'si:`x-msauth-mytestiosapp://com.myapp.mytestapp`

App2 yeniden yönlendirme URİ'si:`x-msauth-mytestiosapp://com.myapp.mytestapp2`

App3 yeniden yönlendirme URİ'si:`x-msauth-mytestiosapp://com.myapp.mytestapp3`

....

Bu durum, aynı istemci kimliği altında iç içe geçmiş / uygulama kimliği ve yeniden yönlendirme URI'si dönmek için bize SDK yapılandırmanızda dayanan Aranan.

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


*Bu yeniden yönlendirme URI biçimi açıklanmıştır aşağıda olduğunu unutmayın. Aracısı desteklemek istediğiniz sürece, bu durumda yukarıdaki şöyle benzemelidir herhangi yeniden yönlendirme URI'si kullanabilir*

#### <a name="create-keychain-sharing-between-applications"></a>Uygulamalar arasında paylaşma Anahtarlık oluşturma
Anahtarlık paylaşımının etkinleştirilmesi, bu belgenin kapsamı dışındadır ve Apple'nın kendi belgede ele alınan [yetenekleri ekleme](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html). Önemli olan çağrılacak, Anahtarlık istediğinize karar olduğundan ve tüm uygulamalar için bu özelliği ekleyin.

Varsa doğru ayarladığınıza yetkilendirmeler yetkili proje dizininde bir dosyasına görmelisiniz `entitlements.plist` şuna benzeyen bir şey içeren:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>keychain-access-groups</key>
    <array>
        <string>$(AppIdentifierPrefix)com.myapp.mytestapp</string>
        <string>$(AppIdentifierPrefix)com.myapp.mycache</string>
    </array>
</dict>
</plist>
```

Her uygulamalarınız etkinleştirdiği Anahtarlık yetkilendirme varsa ve SSO kullanıma hazır sonra Microsoft Identity SDK'sı, Anahtarlık hakkında aşağıdaki ayarı kullanılarak söyleyin, `ADAuthenticationSettings` aşağıdaki ayar:

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [!WARNING]
> Uygulamalar arasında bir Anahtarlık paylaştığınızda herhangi bir uygulama kullanıcıların silin veya tüm belirteçleri, uygulamanızda da kötüsü silin. Bu iş arka plan için belirteçleri kullanan uygulamalar varsa, özellikle felaket niteliğinde değildir. Bir Anahtarlık paylaşımı kaldırma işlemleri Microsoft Identity SDK'ları aracılığıyla içindeki tüm çok dikkatli olmalıdır anlamına gelir.
> 
> 

İşte bu kadar! Microsoft Identity SDK'sı, artık tüm uygulamalar için kimlik bilgileri paylaşacaktır. Kullanıcı listesini, ayrıca uygulama örnekleri arasında paylaşılır.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>SSO destekli aracısı için SSO üzerinde kapatma
Bir uygulama cihazda yüklü herhangi bir aracı kullanmak için özelliği **varsayılan olarak devre dışı**. Uygulamanızı Aracısı ile kullanmak için bazı ek yapılandırma adımları ve uygulamanız için bazı kod eklemeniz gerekir.

İzlemeniz gereken adımlar şunlardır:

1. MS SDK, uygulama kodun çağrıda Aracısı modunu etkinleştirin.
2. Yeni yeniden yönlendirme URI'si oluşturmak ve uygulama ve uygulama kaydınızı için sağlayın.
3. URL şeması kaydediliyor.
4. iOS9 desteği: izin info.plist dosyanıza ekleyin.

#### <a name="step-1-enable-broker-mode-in-your-application"></a>1. adım: uygulamanızı Aracısı modunda etkinleştirme
"Context" ya da kimlik doğrulama nesnenizin ilk kurulum oluşturduğunuzda özelliği Aracısı'nı kullanmak, uygulamanız için etkinleştirilir. Kimlik bilgileri türü kodunuzda ayarlayarak bunu yapabilirsiniz:

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
`AD_CREDENTIALS_AUTO` Ayarı aracısı için duyurmak denemek Microsoft Identity SDK izin `AD_CREDENTIALS_EMBEDDED` arama aracısı için Microsoft Identity SDK önler.

#### <a name="step-2-registering-a-url-scheme"></a>2. adım: bir URL şemasını kaydetme
Microsoft Identity platformu URL'leri Aracısı çağırma ve sonra denetim uygulamanıza geri dönmek için kullanır. Bu gidiş dönüş tamamlamak için Microsoft Identity platform hakkında bilirsiniz uygulamanız için kaydedilen bir URL şeması gerekir. Bu, daha önce uygulamanızla kaydetmiş olabileceği diğer uygulama düzenleri yanı sıra olabilir.

> [!WARNING]
> URL şeması aynı URL şeması kullanarak başka bir uygulama olasılığını en aza indirmek için oldukça benzersiz yaparak öneririz. Apple uygulama Mağazası'nda kayıtlı URL şemalarını benzersizlik değil.
> 
> 

Aşağıda, bu proje yapılandırmanızda görünme bir örnektir. Ayrıca bu Xcode'da de yapabilirsiniz:

```
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>com.myapp.mytestapp</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>x-msauth-mytestiosapp</string>
        </array>
    </dict>
</array>
```

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>Adım 3: yeni yeniden yönlendirme URI'si URL şeması ile oluşturma
Her zaman doğru uygulamaya kimlik bilgisi belirteçleri biz dönmek emin olmak için size geri iOS işletim sistemi doğrulayabilirsiniz şekilde uygulamanıza diyoruz emin olmanız gerekir. İOS işletim sistemi için Microsoft Aracısı uygulamaları, çağıran uygulama paket Kimliğini bildirir. Standart dışı bir uygulama tarafından sahte olamaz. Bu nedenle, biz bu belirteçleri doğru uygulama döndürüldüğünden emin olmak için Aracısı uygulamamız URI'sini birlikte yararlanın. Bizim Geliştirici Portalı'nda bir yeniden yönlendirme URI'si olarak ayarlayın ve bu benzersiz yeniden yönlendirme URI'si hem uygulamanızda kurmak isteriz.

Yeniden yönlendirme URI'si uygun biçiminde olmalıdır:

`<app-scheme>://<your.bundle.id>`

örn: *x msauth mytestiosapp://com.myapp.mytestapp*

Bu yeniden yönlendirme URI'sini kullanarak, uygulama kaydı belirtilmesi gerekiyor [Azure portal](https://portal.azure.com/). Azure AD uygulama kaydı hakkında daha fazla bilgi için bkz: [Azure Active Directory ile tümleştirme](active-directory-how-to-integrate.md).

##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-to-support-certificate-based-authentication"></a>Adım 3a: sertifika tabanlı kimlik doğrulamasını desteklemek için uygulama ve Geliştirici Portalı'nda yeniden yönlendirme URI'si ekleyin
Destek sertifikası tabanlı kimlik doğrulaması ikinci "msauth" uygulamanızda kayıtlı olması gerekir ve [Azure portal](https://portal.azure.com/) destekleyen uygulamanızda eklemek isterseniz, sertifika kimlik doğrulamasını işleyecek şekilde.

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

örn: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*

#### <a name="step-4-ios9-add-a-configuration-parameter-to-your-app"></a>4. adım: iOS9: uygulamanıza bir yapılandırma parametresi ekleme
ADAL kullanan – canOpenURL: Aracısı cihazda yüklü olup olmadığını denetlemek için. İOS 9 Apple ne uygulama düzenleri sorgulayabilir aşağı kilitli. LSApplicationQueriesSchemes kısmına "msauth" eklemeniz gerekir, `info.plist file`.

<key>LSApplicationQueriesSchemes</key> <array> <string>msauth</string></array>

### <a name="youve-configured-sso"></a>SSO yapılandırdıktan!
Şimdi Microsoft Identity SDK otomatik olarak hem kimlik bilgileri, uygulamalar arasında paylaşmak ve cihazlarında varsa, aracı çağırma.

