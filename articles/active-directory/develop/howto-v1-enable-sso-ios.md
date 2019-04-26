---
title: İOS ADAL kullanarak uygulamalar arası SSO'yu nasıl | Microsoft Docs
description: Uygulamalarınızda çoklu oturum açmayı etkinleştirmek için ADAL SDK'ın özelliklerini kullanma
services: active-directory
author: CelesteDG
manager: mtillman
ms.assetid: d042d6da-7503-4e20-bb55-06917de01fcd
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e79b73123b33a012c062a89fb9748fa101fabcea
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60299626"
---
# <a name="how-to-enable-cross-app-sso-on-ios-using-adal"></a>Nasıl yapılır: İOS ADAL kullanarak uygulamalar arası SSO'yu etkinleştirin

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Çoklu oturum açma (SSO) yalnızca bir kez kimlik bilgilerini girin ve diğer uygulamalar (örneğin, Microsoft Accounts veya Microsoft 365'ten bir iş hesabı) kullanabilir platformları ve uygulamalar arasında otomatik iş bu kimlik bilgilerine sahip kullanıcılar sağlayan yok Yayımcı önemli.

Microsoft kimlik platformu, SDK'ları, tüm cihazlarda uygulamalar veya Aracısı yetenek ve Authenticator uygulamaları, kendi suite'te SSO'yu etkinleştirmek üzere kolaylaştırır.

Bu nasıl yapılır makalesinde müşterilerinize SSO sağlamak için uygulamanızdaki SDK'sını yapılandırmak öğreneceksiniz.

Bu nasıl yapılır şunlar için geçerlidir:

* Azure Active Directory (Azure Active Directory)
* Azure Active Directory B2C
* Azure Active Directory B2B
* Azure Active Directory Koşullu Erişim

## <a name="prerequisites"></a>Önkoşullar

Bu yöntem, bildiğinizi varsayar nasıl yapılır:

* Uygulamanızı Azure AD için eski portalı kullanarak sağlayın. Daha fazla bilgi için bkz. [bir uygulamayı Azure AD'ye v1.0 uç noktası ile kaydetme](quickstart-v1-add-azure-ad-app.md)
* Uygulamanızla tümleştirin [Azure AD'ye iOS SDK'sı](https://github.com/AzureAD/azure-activedirectory-library-for-objc).

## <a name="single-sign-on-concepts"></a>Çoklu oturum açma kavramları

### <a name="identity-brokers"></a>Kimlik aracıları

Microsoft tüm mobil platformlar için kimlik bilgilerini farklı satıcılardan uygulamalar arasında köprü oluşturma ve kimlik bilgilerini doğrulamak nereden tek bir güvenli yer gerektiren gelişmiş özellikleri için izin uygulamalar sağlar. Bunlar adlandırılır **aracıları**.

İOS ve Android üzerinde aracıları müşteriler bağımsız olarak yüklemek veya çalışanlar için bazı yöneten bir şirket tarafından ya da tüm aygıtların cihaza gönderilen indirilebilir uygulamalar aracılığıyla sağlanır. Yalnızca bazı uygulamalar veya BT yöneticisi yapılandırmasına bağlı olarak tüm cihaz için güvenlik yönetimi aracıları desteği. Windows bu işlev, işletim sistemine Web kimlik doğrulama aracısı olarak bilinen, yerleşik bir hesap Seçici tarafından sağlanır.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Mobil cihazlarda oturum açmak için desenler

Cihazlarda kimlik bilgilerine erişim, iki temel düzenlerden izleyin:

* Yardımlı Aracısı olmayan oturumlar
* Yardımlı oturumları aracı

#### <a name="non-broker-assisted-logins"></a>Yardımlı Aracısı olmayan oturumlar

Aracı olmayan Yardımlı satır içi uygulamayla gerçekleşir ve bu uygulama için cihazda yerel depolamayı kullanan oturum açma deneyimlerini oturumlardır. Bu depolama alanı, uygulamalar arasında paylaşılan, ancak kimlik bilgileri uygulama veya bu kimlik bilgilerini kullanarak uygulama paketi sıkı bir şekilde bağlıdır. Bir kullanıcı adı ve parola uygulamanın kendisinden girdiğinizde bu büyük olasılıkla mobil uygulamalarda karşılaştı.

Bu oturum açma bilgileri, aşağıdaki avantajlara sahiptir:

* Kullanıcı deneyimini tamamen uygulama içinde var.
* Kimlik bilgileri, paketiniz uygulamaları için çoklu oturum açma deneyimini sağlamak aynı sertifika tarafından imzalanan uygulamalar arasında paylaşılabilir.
* Denetimin etrafında oturum açma deneyimi, uygulama öncesinde ve sonrasında oturum açma için sağlanır.

Bu oturum açma bilgileri, aşağıdaki dezavantajları vardır:

* Kullanıcılar çoklu oturum üzerinde uygulamanızı yapılandırılmış yalnızca Microsoft kimliklerle arasında bir Microsoft kimliği kullanan tüm uygulamalar arasında deneyimi olamaz.
* Uygulamanızı Intune ürünleri kullanın veya koşullu erişim gibi daha gelişmiş iş özelliklerle kullanılamaz.
* Uygulamanızın iş kullanıcıları için sertifika tabanlı kimlik doğrulamasını destekleyemez.

SDK'ları SSO'yu etkinleştirmek için uygulamalarınızı paylaşılan depolama ile nasıl çalıştığı bir gösterimi aşağıda verilmiştir:

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| ADAL SDK  |  |  ADAL SDK  |  |  ADAL SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### <a name="broker-assisted-logins"></a>Yardımlı oturumları aracı

Aracısı destekli Aracısı uygulama içinde ortaya ve kimlik bilgilerini geçerli kimlik platformu olan tüm uygulamalar arasında cihazdaki paylaşmak için depolama ve güvenlik aracı kullanın oturum açma deneyimlerini oturumlardır. Bu, kullanıcıların oturum açmak için aracı uygulamalarınızın kullanabileceği olduğunu anlamına gelir. İOS ve Android üzerinde bu aracıları indirilebilir uygulamalar aracılığıyla müşteriler bağımsız olarak yüklemek veya cihaza gönderilen kullanıcı için cihaz yöneten bir şirket tarafından sağlanır. Bu tür bir uygulama örneği, ios'ta Microsoft Authenticator uygulamasıdır. Windows bu işlev, Web kimlik doğrulama aracısı olarak bilinen işletim sisteminde yerleşik bir hesap Seçici tarafından sağlanır.

Deneyimi platforma göre değişiklik gösterir ve bazen kullanıcılar için karışıklığa neden olabilir Aksi takdirde doğru yönetilen. Facebook uygulaması yüklü olan ve başka bir uygulamadan Facebook bağlanma'yı kullanırsanız bu deseni en biliyor. Kimlik platformu aynı deseni kullanır.

Bu "geçiş" Müşteri adayları iOS için ön kullanıcının oturum açmak istediğiniz hesabı seçin, uygulamanızın Microsoft Authenticator uygulamaları arka burada gönderilen animasyon gelir.

Android ve Windows hesap Seçici, uygulamanızın üzerinde görüntülenen için hangi kullanıcıya daha az kesintiye uğratan bir durumdur.

#### <a name="how-the-broker-gets-invoked"></a>Aracının nasıl çağrılır

Uyumlu bir aracı Microsoft Authenticator uygulaması gibi bir cihazda yüklü değilse SDK'ları otomatik olarak bir kullanıcı kimlik platformu herhangi bir hesabı kullanarak oturum açması istedikleri belirtiyorsa, aracı çağırma işini yapar. Bu hesap, kişisel Microsoft Account, bir iş veya Okul hesabı veya bir sağladığınız hesap ve B2C ve B2B ürünlerimizi kullanarak azure'da konak olabilir.

#### <a name="how-we-ensure-the-application-is-valid"></a>Biz uygulamanın nasıl emin geçerlidir

Oturum açma bilgileri Aracısı Aracısı sağladığımız güvenlik önemli bir uygulama çağrısı kimliğini sağlama gereksinimi Yardımlı. İOS ya da Android kötü amaçlı uygulamalar "yasal bir uygulamanın tanımlayıcısı aldatıcı" ve yasal uygulama için gereken belirteçlerini almak için yalnızca belirli bir uygulama için geçerli olan benzersiz tanımlayıcıları zorlar. Biz her zaman zamanında doğru uygulama ile iletişim kuran emin olmak için Microsoft ile kendi uygulama kaydı sırasında özel bir redirectURI sağlamak için geliştirici isteriz. Geliştiriciler bu yeniden yönlendirme URI'si nasıl çalışıyorlardı aşağıda ayrıntılı olarak ele alınmıştır. Bu özel redirectURI içeren uygulamanın paket Kimliğini ve tarafından Apple App Store uygulaması için benzersiz olmasını oldunuz. Bir uygulama Aracısı çağırdığında, Aracı aracısı adlı paket kimliği ile sağlamak için iOS işletim sistemi ister. Bu paket kimliği, aracı kimlik sistemimiz çağrısında Microsoft'a sağlar. Uygulamanın paket Kimliğini bize kayıt sırasında geliştirici tarafından sağlanan paket kimliği ile eşleşmiyorsa, biz belirteçleri kaynak uygulama için erişim isteme reddeder. Bu denetim, yalnızca geliştirici tarafından kayıtlı uygulama belirteçlerini almasını sağlar.

**SDK'sı Aracısı çağırır veya aracı olmayan Yardımlı akışın kullandığı Geliştirici seçeneği vardır.** Kullanıcı cihazda zaten eklemiş olabileceğiniz, kimlik bilgileri ve kendi uygulama Microsoft'un sağladığı iş özellikleri ile kullanılmasını engeller ancak bunlar kaybetmek SSO kullanmanın avantajı Geliştirici Aracısı destekli akışı kullanmayacak şekilde seçerse, Koşullu erişim, Intune yönetim özellikleri ve sertifika tabanlı kimlik doğrulaması gibi müşteriler.

Bu oturum açma bilgileri, aşağıdaki avantajlara sahiptir:

* Kullanıcı, satıcı ne olursa olsun tüm uygulamalar arasında SSO karşılaşır.
* Uygulamanızı, koşullu erişim gibi daha gelişmiş iş özellikleri kullanın veya Intune ürünleri kullanın.
* Uygulamanızın iş kullanıcıları için sertifika tabanlı kimlik doğrulamasını destekler.
* Uygulama ve kullanıcı kimliği olarak daha güvenli oturum açma deneyimi ek güvenlik algoritmalarını ve şifreleme ile Aracısı uygulama tarafından doğrulandı.

Bu oturum açma bilgileri, aşağıdaki dezavantajları vardır:

* Kimlik bilgileri seçili durumdayken, iOS uygulamanızın deneyimi kullanıcı geçirilir.
* Oturum açma deneyimi sayesinde müşterileriniz uygulamanız içinde yönetme olanağı kaybı.

SDK'ları SSO'yu etkinleştirmek üzere aracı uygulamalarla nasıl çalıştığını, bir gösterim şu şekildedir:

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

## <a name="enabling-cross-app-sso-using-adal"></a>ADAL kullanarak uygulamalar arası SSO'yu etkinleştirme

ADAL iOS SDK'sı burada kullandığımız için:

* Aracı olmayan Yardımlı SSO paketiniz uygulama açma
* SSO Aracısı destekli desteğini açın

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Aracı olmayan için SSO açma kapatma Yardımlı SSO

Aracı olmayan Yardımlı SSO için uygulamalar arasında SDK'ları SSO karmaşıklığını çoğunu sizin için yönetin. Bu, önbellekte doğru kullanıcı bulma ve bakımını yapma, sorgulama oturum açan kullanıcıların listesini içerir.

Aşağıdakileri yapmanız sahip uygulamalar arasında SSO'yu etkinleştirmek için:

1. Tüm uygulamalar kullanıcı aynı istemci Kimliğini veya uygulama kimliği emin olun.
2. Anahtarlıklar paylaşabileceği tüm uygulamalar aynı Apple imzalama sertifikası paylaşmadığından emin olun.
3. Uygulamaların her biri için aynı anahtar zinciri yetkilendirmesini isteyin.
4. SDK'ları, paylaşılan bir Anahtarlığa hakkında kullanmak istediğinizi söyleyin.

#### <a name="using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Aynı istemci Kimliğini kullanarak / paketiniz uygulamaların tüm uygulamalar için uygulama kimliği

Sırada kimlik platformu belirteçleri uygulamalarınız arasında paylaşmak için izin verip öğrenmek için uygulamalarınızın her birinde aynı istemci Kimliğini veya uygulama kimliği paylaşımında bulunmaları gerekmektedir Bu portalda ilk uygulamanızı kaydettiğinizde, size sağlanan benzersiz bir tanımlayıcıdır.

Yeniden yönlendirme URI'leri aynı uygulama kimliği kullanıyorsa, farklı uygulamalar için Microsoft Identity service tanımlamanıza izin ver Her uygulamanın birden çok yeniden yönlendirme URI'leri ekleme Portalı'nda kayıtlı olabilir. Paketiniz her uygulamayı farklı bir yeniden yönlendirme URI'sine sahip olur. Bu nasıl görüneceğini gösteren bir örnek aşağıda verilmiştir:

App1'i yeniden yönlendirme URI'si: `x-msauth-mytestiosapp://com.myapp.mytestapp`

App2 yeniden yönlendirme URI'si: `x-msauth-mytestiosapp://com.myapp.mytestapp2`

App3 yeniden yönlendirme URI'si: `x-msauth-mytestiosapp://com.myapp.mytestapp3`

....

Bunlar aynı istemci kimliği altında iç içe / uygulama kimliği ve yeniden yönlendirme URI'si geri dönmek için SDK'sı yapılandırmanızda temel alınarak aranabilir.

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

Biçimi bu yeniden yönlendirme URI'leri, aşağıda açıklanmıştır. Aracısı desteklemek istediğiniz sürece bu durumda yukarıdaki gibi benzemelidir herhangi bir yeniden yönlendirme URI'si kullanabilir *

#### <a name="create-keychain-sharing-between-applications"></a>Anahtarlık paylaşımı arasında uygulamalar oluşturma

Anahtarlık paylaşımının etkinleştirilmesi, bu belgenin kapsamı dışındadır ve bunların belgede Apple tarafından kapsanan [yetenekleri ekleme](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html). Bilgilerinizi anahtarlığınızda çağrılması için istediğinize karar önemli olduğunu ve tüm uygulamalarınız genelinde söz konusu özellik ekleyin.

Varsa doğru kurduktan yetkilendirmeler başlıklı proje dizininizde bir dosya görmeniz gerekir `entitlements.plist` şuna benzeyen bir şey içeren:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "https://www.apple.com/DTDs/PropertyList-1.0.dtd">
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

Uygulamalarınızın her etkin Anahtarlık yetkilendirmesini sahip ve SSO kullanıma hazır sonra SDK kimlik bilgilerinizi anahtarlığınızda hakkında aşağıdakileri kullanarak bilgi ayarı, `ADAuthenticationSettings` aşağıdaki ayar:

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [!WARNING]
> Uygulamalarınızda bir Anahtarlığa paylaştığınızda herhangi bir uygulama kullanıcıların silebilir veya daha da kötüsü uygulamanızdaki tüm belirteçlerin silin. Belirteçleri iş arka plan üzerinde dayanan uygulamalar varsa, özellikle çok kötü sonuçlar budur. Bir anahtar zinciri paylaşımı kaldırma işlemleri kimlik SDK'lar aracılığıyla anlamına gelir, tüm çok dikkatli olmalıdır.

İşte bu kadar! SDK'sı, artık tüm uygulamalarınızda kimlik bilgilerini paylaşır. Kullanıcı listesini de uygulama örnekleri arasında paylaşılır.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>SSO Yardımlı aracısı için SSO açma kapatma

Cihazda yüklü herhangi bir aracı kullanmak bir uygulama için olanağıdır **varsayılan olarak kapalı**. Aracı ile uygulamanızı kullanmak için bazı ek yapılandırma adımları ve uygulamanıza kod ekleyin.

İzlenmesi gereken adımlar şunlardır:

1. Uygulama kodunuzun çağrıda MS SDK Aracısı modunu etkinleştirin.
2. Yeni bir yeniden yönlendirme URI'Sİ'kurmak ve hem uygulama hem de uygulama kaydınızı sağlayın.
3. Bir URL şeması kaydediliyor.
4. Bir izni Info.plist dosyanıza ekleyin.

#### <a name="step-1-enable-broker-mode-in-your-application"></a>1. Adım: Uygulamanızdaki Aracısı modunu etkinleştir

"Bağlam" veya ilk kurulum kimlik doğrulaması nesne oluşturduğunuzda aracı kullanmak için uygulamanızı özelliği etkinleştirilir. Kodunuzda, kimlik bilgileri türü ayarlayarak bunu yapabilirsiniz:

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
`AD_CREDENTIALS_AUTO` Ayarı SDK'sı, aracıya duyurmak denemek izin `AD_CREDENTIALS_EMBEDDED` arama aracısı için SDK'sı önler.

#### <a name="step-2-registering-a-url-scheme"></a>2. Adım: Bir URL şeması kaydediliyor

Kimlik platformu URL'leri Aracısı çağırın, sonra uygulamanıza geri denetimi döndürmek için kullanır. Bu gidiş dönüş tamamlamak için kimlik platformu hakkında öğrenmiş olacaksınız uygulamanız için kayıtlı bir URL şeması gerekir. Bu, daha önce uygulamanızla kaydetmiş olabileceği diğer uygulama düzenleri yanı sıra olabilir.

> [!WARNING]
> URL düzeni aynı URL şeması kullanarak başka bir uygulama olasılığını en aza indirmek için oldukça benzersiz yaparak öneririz. Apple app Store'da kaydedilen URL şemalarını benzersizlik değil.

Bu, proje yapılandırmasında görüntülenme şeklini bir örnek aşağıdadır. Ayrıca bu Xcode'da de yapabilirsiniz:

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

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>3. Adım: Yeni bir yeniden yönlendirme URI'si ile URL düzeni oluştur

Her zaman doğru uygulama için kimlik bilgisi belirteçleri biz döndürdüğünden emin olmak için biz iOS işletim sistemi doğrulayabilirsiniz şekilde uygulamanıza geri arama emin olmak gerekiyor. İOS işletim sistemi bunu çağırma uygulamanın paket Kimliğini Microsoft Aracısı uygulamalarına bildirir. Bu, standart dışı bir uygulama tarafından sahte olamaz. Bu nedenle, ki bu belirteçleri doğru uygulama döndürüldüğünden emin olmak için aracı uygulamamız URI'si ile birlikte yararlanın. Bu benzersiz yeniden yönlendirme URI'si her iki uygulamanızda kurmak ve bizim Geliştirici Portalı'nda bir yeniden yönlendirme URI'si olarak ayarlamak gerekli.

Uygulamanızın yeniden yönlendirme URI'si düzgün biçimde olmalıdır:

`<app-scheme>://<your.bundle.id>`

örn: *x msauth mytestiosapp://com.myapp.mytestapp*

Bu yeniden yönlendirme URI'sini kullanarak, uygulama kayıt belirtilmesine gerek [Azure portalında](https://portal.azure.com/). Azure AD uygulama kaydı hakkında daha fazla bilgi için bkz. [Azure Active Directory ile tümleştirme](active-directory-how-to-integrate.md).

##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-to-support-certificate-based-authentication"></a>Adım 3a: Sertifika tabanlı kimlik doğrulamasını desteklemek için uygulama ve geliştirme Portalı'nda bir yeniden yönlendirme URI'si ekleyin

Destek sertifika tabanlı kimlik doğrulaması ikinci "msauth" uygulamanızda kaydedilmesi gerekir ve [Azure portalında](https://portal.azure.com/) uygulamanızda bu desteği eklemek istiyorsanız, sertifika kimlik doğrulamasını işlemek için.

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

örn: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*

#### <a name="step-4-add-a-configuration-parameter-to-your-app"></a>4. Adım: Bir yapılandırma parametresi uygulamanıza ekleme

ADAL kullanan – canOpenURL: aracı cihaz üzerinde yüklü olup olmadığını denetlemek için. İOS 9, hangi uygulama düzenleri sorgulayabilir aşağı Apple kilitli. LSApplicationQueriesSchemes kısmına "msauth" eklemeniz gerekecektir, `info.plist file`.

```
    <key>LSApplicationQueriesSchemes</key>
    <array>
        <string>msauth</string>
    </array>

```

### <a name="youve-configured-sso"></a>SSO yapılandırdınız!

Artık SDK kimlik otomatik olarak hem paylaşım uygulamalarınızda kimlik bilgileri ve kendi cihazında varsa aracı çağırma.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [çoklu oturum açma SAML Protokolü](single-sign-on-saml-protocol.md)
