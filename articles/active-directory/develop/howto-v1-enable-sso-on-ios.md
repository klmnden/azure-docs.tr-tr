---
title: İOS ADAL kullanarak uygulamalar arası SSO'yu nasıl | Microsoft Docs
description: "Uygulamalarınızda çoklu oturum açmayı etkinleştirmek için ADAL SDK'ın özelliklerini kullanma "
services: active-directory
author: CelesteDG
manager: mtillman
ms.assetid: d042d6da-7503-4e20-bb55-06917de01fcd
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 04/07/2017
ms.author: celested
ms.reviewer: brandwe
ms.custom: aaddev
ms.openlocfilehash: 203cb4f57cfa50a17b66a9b70a44568e57ec4835
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39582010"
---
# <a name="how-to-enable-cross-app-sso-on-ios-using-adal"></a>İOS ADAL kullanarak uygulamalar arası SSO'yu etkinleştirme

Çoklu oturum açma (SSO), böylece kullanıcıların yalnızca bir kez kimlik bilgilerini girin ve bu kimlik bilgilerini otomatik olarak bulunması gerekir çalışma boyunca sağlayan uygulamalar artık müşteriler tarafından bekleniyor. Bir telefon çağrısı veya kısa mesajla bir kod gibi ek bir faktörü (2FA) kez birlikte küçük ekranlarda, genellikle kullanıcı adı ve parola girme zorluk hızlı memnuniyetsizliğine kullanıcı sonuçlarda bu ürün için birden fazla kez yapmak var.

Ayrıca, diğer uygulamalar gibi Microsoft Accounts veya Office 365 üzerinden bir iş hesabı kullanabilir bir kimlik platformu uygularsanız, müşteriler bu kimlik bilgilerini, satıcı ne olursa olsun tüm uygulamalar arasında kullanılabilir olmasını bekleyebilirsiniz.

Microsoft kimlik Larımız yanı sıra Microsoft Identity platform bu zor işi yapar ve SSO ile müşterilerinize ya da uygulamaların kendi suite'te etkileyin olanağı sağlar veya olarak Aracısı yetenek ve Authenticator uygulamalar, cihazın tamamını arasında.

SDK'mız içinde bu avantajı müşterilerinize sağlamak için uygulamanızı yapılandırma Bu izlenecek yol size bildirir.

Bu kılavuzda şunlar için geçerlidir:

* Azure Active Directory
* Azure Active Directory B2C
* Azure Active Directory B2B
* Azure Active Directory Koşullu Erişim

Önceki belge bildiğiniz varsayılır nasıl [sağlamak için Azure Active Directory eski portalda uygulamaları](active-directory-how-to-integrate.md) ve uygulamanızla tümleşik [Microsoft Identity iOS SDK'sı](https://github.com/AzureAD/azure-activedirectory-library-for-objc).

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>Microsoft kimlik platformu SSO kavramları

### <a name="microsoft-identity-brokers"></a>Microsoft kimlik aracıları

Microsoft tüm mobil platformlar için kimlik bilgilerini farklı satıcılardan uygulamalar arasında köprüleme sağlayan uygulamalar sağlar ve kimlik bilgilerini doğrulamak nereden tek bir güvenli yer gerektiren özel gelişmiş özellikler sağlar. Bunlar diyoruz **aracıları**. İOS ve Android'de bu aracılar müşterilerin bağımsız olarak yüklemek veya bazı veya tüm cihaz için çalışanlara yöneten bir şirket tarafından cihaza gönderilen indirilebilir uygulamalar aracılığıyla sağlanır. Bunlar aracı yalnızca bazı uygulamalar veya BT yöneticileri bağlamasına üzerinde temel cihazın tamamını için yönetme güvenliğini destekler. Windows bu işlev, işletim sistemine Web kimlik doğrulama aracısı olarak bilinen, yerleşik bir hesap Seçici tarafından sağlanır.

Daha fazla bilgi için bu aracıları nasıl kullandığımızı ve nasıl müşterilerinizin onları kendi oturum açma akışını Microsoft Identity platformuna yönelik görebileceğiniz okumaya devam edin.

### <a name="patterns-for-logging-in-on-mobile-devices"></a>Mobil cihazlarda oturum açmak için desenler

Cihazlarda kimlik bilgilerine erişim Microsoft Identity platformuna yönelik iki temel düzenlerden izleyin:

* Yardımlı Aracısı olmayan oturumlar
* Yardımlı oturumları aracı

#### <a name="non-broker-assisted-logins"></a>Yardımlı Aracısı olmayan oturumlar

Aracı olmayan Yardımlı satır içi uygulamayla gerçekleşir ve bu uygulama için cihazda yerel depolamayı kullanan oturum açma deneyimlerini oturumlardır. Bu depolama alanı, uygulamalar arasında paylaşılan, ancak kimlik bilgileri uygulama veya bu kimlik bilgilerini kullanarak uygulama paketi sıkı bir şekilde bağlıdır. Bir kullanıcı adı ve parola uygulamanın kendisinden girdiğinizde bu büyük olasılıkla mobil uygulamalarda karşılaştı.

Bu oturum açma bilgileri, aşağıdaki avantajlara sahiptir:

* Kullanıcı deneyimini tamamen uygulama içinde var.
* Kimlik bilgileri, paketiniz uygulamaları için çoklu oturum açma deneyimini sağlamak aynı sertifika tarafından imzalanan uygulamalar arasında paylaşılabilir.
* Denetimin etrafında oturum açma deneyimi, uygulama öncesinde ve sonrasında oturum açma için sağlanır.

Bu oturum açma bilgileri, aşağıdaki dezavantajları vardır:

* Kullanıcı bir Microsoft Identity yalnızca Microsoft, uygulamanızın yapılandırılmış Identities arasında kullanan tüm uygulamalar arasında çoklu oturum açma deneyimi olamaz.
* Uygulamanızı Intune ürünleri kullanın veya koşullu erişim gibi daha gelişmiş iş özelliklerle kullanılamaz.
* Uygulamanızın iş kullanıcıları için sertifika tabanlı kimlik doğrulamasını destekleyemez.

Microsoft kimlik SDK'ları SSO'yu etkinleştirmek için uygulamalarınızı paylaşılan depolama ile nasıl çalıştığı bir gösterimi aşağıda verilmiştir:

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

#### <a name="broker-assisted-logins"></a>Yardımlı oturumları aracı

Aracısı destekli Aracısı uygulama içinde ortaya ve Microsoft Identity platformundan bağımsız olarak tüm uygulamalar arasında cihaz kimlik bilgilerini paylaşmak için depolama ve güvenlik Aracı'nı kullanan oturum açma deneyimlerini oturumlardır. Bu, kullanıcıların oturum açmak için aracı uygulamalarınızın kullanabileceği olduğunu anlamına gelir. İOS ve Android üzerinde bu aracıları indirilebilir uygulamalar aracılığıyla müşteriler bağımsız olarak yüklemek veya cihaza gönderilen kullanıcı için cihaz yöneten bir şirket tarafından sağlanır. Bu tür bir uygulama örneği, ios'ta Microsoft Authenticator uygulamasıdır. Windows bu işlev, Web kimlik doğrulama aracısı olarak bilinen işletim sisteminde yerleşik bir hesap Seçici tarafından sağlanır.
Deneyimi platforma göre değişiklik gösterir ve bazen kullanıcılar için karışıklığa neden olabilir Aksi takdirde doğru yönetilen. Facebook uygulaması yüklü olan ve başka bir uygulamadan Facebook bağlanma'yı kullanırsanız bu deseni en biliyor. Microsoft Identity platformu aynı deseni kullanır.

Bu "geçiş" Müşteri adayları iOS için ön kullanıcının oturum açmak istediğiniz hesabı seçin, uygulamanızın Microsoft Authenticator uygulamaları arka burada gönderilen animasyon gelir. 

Android ve Windows hesap Seçici, uygulamanızın üzerinde görüntülenen için hangi kullanıcıya daha az kesintiye uğratan bir durumdur.

#### <a name="how-the-broker-gets-invoked"></a>Aracının nasıl çağrılır

Uyumlu bir aracı Microsoft Authenticator uygulaması gibi bir cihazda yüklü değilse Microsoft kimlik SDK'ları otomatik olarak bir kullanıcı Microsoft gelen herhangi bir hesabı kullanarak oturum açması istedikleri belirtiyorsa, aracı çağırma iş yapın Kimlik platformu. Bu hesap, kişisel Microsoft Account, bir iş veya Okul hesabı veya bir sağladığınız hesap ve B2C ve B2B ürünlerimizi kullanarak azure'da konak olabilir.

#### <a name="how-we-ensure-the-application-is-valid"></a>Biz uygulamanın nasıl emin geçerlidir
 
 Oturum açma bilgileri Aracısı Aracısı sağladığımız güvenlik önemli bir uygulama çağrısı kimliğini sağlama gereksinimi Yardımlı. İOS ya da Android kötü amaçlı uygulamalar "yasal bir uygulamanın tanımlayıcısı aldatıcı" ve yasal uygulama için gereken belirteçlerini almak için yalnızca belirli bir uygulama için geçerli olan benzersiz tanımlayıcıları zorlar. Biz her zaman zamanında doğru uygulama ile iletişim kuran emin olmak için Microsoft ile kendi uygulama kaydı sırasında özel bir redirectURI sağlamak için geliştirici isteriz. **Geliştiriciler bu yeniden yönlendirme URI'si nasıl çalışıyorlardı aşağıda ayrıntılı olarak ele alınmıştır.** Bu özel redirectURI içeren uygulamanın paket Kimliğini ve tarafından Apple App Store uygulaması için benzersiz olmasını oldunuz. Bir uygulama Aracısı çağırdığında, Aracı aracısı adlı paket kimliği ile sağlamak için iOS işletim sistemi ister. Bu paket kimliği, aracı kimlik sistemimiz çağrısında Microsoft'a sağlar. Uygulamanın paket Kimliğini bize kayıt sırasında geliştirici tarafından sağlanan paket kimliği ile eşleşmiyorsa, biz belirteçleri kaynak uygulama için erişim isteme reddeder. Bu denetim, yalnızca geliştirici tarafından kayıtlı uygulama belirteçlerini almasını sağlar.

**Microsoft kimlik SDK'sı Aracısı çağırır veya aracı olmayan Yardımlı akışın kullandığı Geliştirici seçeneği yoktur.** Kullanıcı cihazda zaten eklemiş olabileceğiniz, kimlik bilgileri ve kendi uygulama Microsoft'un sağladığı iş özellikleri ile kullanılmasını engeller ancak bunlar kaybetmek SSO kullanmanın avantajı Geliştirici Aracısı destekli akışı kullanmayacak şekilde seçerse, Koşullu erişim, Intune yönetim özellikleri ve sertifika tabanlı kimlik doğrulaması gibi müşteriler.

Bu oturum açma bilgileri, aşağıdaki avantajlara sahiptir:

* Kullanıcı, satıcı ne olursa olsun tüm uygulamalar arasında SSO karşılaşır.
* Uygulamanızı, koşullu erişim gibi daha gelişmiş iş özellikleri kullanın veya Intune ürünleri kullanın.
* Uygulamanızın iş kullanıcıları için sertifika tabanlı kimlik doğrulamasını destekler.
* Uygulama ve kullanıcı kimliği olarak daha güvenli oturum açma deneyimi ek güvenlik algoritmalarını ve şifreleme ile Aracısı uygulama tarafından doğrulandı.

Bu oturum açma bilgileri, aşağıdaki dezavantajları vardır:

* Kimlik bilgileri seçili durumdayken, iOS uygulamanızın deneyimi kullanıcı geçirilir.
* Oturum açma deneyimi sayesinde müşterileriniz uygulamanız içinde yönetme olanağı kaybı.

Microsoft kimlik SDK'ları SSO'yu etkinleştirmek üzere aracı uygulamalarla nasıl çalıştığını, bir gösterim şu şekildedir:

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

Daha iyi anlamanıza ve SSO uygulamanızın içinden Microsoft Identity platformu ve SDK'ları kullanarak uygulamak için bu arka plan bilgileri kullanarak.

## <a name="enabling-cross-app-sso-using-adal"></a>ADAL kullanarak uygulamalar arası SSO'yu etkinleştirme

ADAL iOS SDK'sı burada kullandığımız için:

* Aracı olmayan Yardımlı SSO paketiniz uygulama açma
* SSO Aracısı destekli desteğini açın

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Aracı olmayan için SSO açma kapatma Yardımlı SSO

Uygulamalar arasında aracı olmayan Yardımlı SSO için Microsoft kimlik SDK'ları SSO karmaşıklığını çoğunu sizin için yönetin. Bu, önbellekte doğru kullanıcı bulma ve bakımını yapma, sorgulama oturum açan kullanıcıların listesini içerir.

Aşağıdakileri yapmanız sahip uygulamalar arasında SSO'yu etkinleştirmek için:

1. Tüm uygulamalar kullanıcı aynı istemci Kimliğini veya uygulama kimliği emin olun.
2. Anahtarlıklar paylaşabileceği tüm uygulamalar aynı Apple imzalama sertifikası paylaşmadığından emin olun
3. Uygulamaların her biri için aynı anahtar zinciri yetkilendirmesini isteyin.
4. Microsoft kimlik SDK'ları, paylaşılan bir Anahtarlığa hakkında kullanmak istediğinizi söyleyin.

#### <a name="using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>Aynı istemci Kimliğini kullanarak / paketiniz uygulamaların tüm uygulamalar için uygulama kimliği

Microsoft Identity platformu belirteçleri uygulamalarınız arasında paylaşmak için izin verip öğrenmek için sırada uygulamalarınızın her birinde aynı istemci Kimliğini veya uygulama kimliği paylaşımında bulunmaları gerekmektedir Bu portalda ilk uygulamanızı kaydettiğinizde, size sağlanan benzersiz bir tanımlayıcıdır.

Aynı uygulama kimliği kullanıyorsa, farklı uygulamalar için Microsoft Identity hizmeti nasıl kullanacağını merak ediyor olabilirsiniz Yanıt olan **yeniden yönlendirme URI'leri**. Her uygulamanın birden çok yeniden yönlendirme URI'leri ekleme Portalı'nda kayıtlı olabilir. Paketiniz her uygulamayı farklı bir yeniden yönlendirme URI'sine sahip olur. Bu nasıl görüneceğini gösteren bir örnek aşağıda verilmiştir:

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


*Bu yeniden yönlendirme URI'leri biçimi aşağıda açıklanan unutmayın. Aracısı desteklemek istediğiniz sürece bu durumda yukarıdaki gibi benzemelidir herhangi bir yeniden yönlendirme URI'si kullanabilir*

#### <a name="create-keychain-sharing-between-applications"></a>Anahtarlık paylaşımı arasında uygulamalar oluşturma

Anahtarlık paylaşımının etkinleştirilmesi, bu belgenin kapsamı dışındadır ve bunların belgede Apple tarafından kapsanan [yetenekleri ekleme](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html). Bilgilerinizi anahtarlığınızda çağrılması için istediğinize karar önemli olduğunu ve tüm uygulamalarınız genelinde söz konusu özellik ekleyin.

Varsa doğru kurduktan yetkilendirmeler başlıklı proje dizininizde bir dosya görmeniz gerekir `entitlements.plist` şuna benzeyen bir şey içeren:

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

Uygulamalarınızın her etkin Anahtarlık yetkilendirmesini sahip ve SSO kullanıma hazır sonra Microsoft Identity SDK Anahtarlığınıza hakkında aşağıdaki ayarı kullanarak bildirin, `ADAuthenticationSettings` aşağıdaki ayar:

```
defaultKeychainSharingGroup=@"com.myapp.mycache";
```

> [!WARNING]
> Uygulamalarınızda bir Anahtarlığa paylaştığınızda herhangi bir uygulama kullanıcıların silebilir veya daha da kötüsü uygulamanızdaki tüm belirteçlerin silin. Belirteçleri iş arka plan üzerinde dayanan uygulamalar varsa, özellikle çok kötü sonuçlar budur. Bir anahtar zinciri paylaşımı kaldırma işlemleri Microsoft kimlik SDK'ları aracılığıyla anlamına gelir, tüm çok dikkatli olmalıdır.

İşte bu kadar! Microsoft kimlik SDK'sı, artık tüm uygulamalarınızda kimlik bilgilerini paylaşır. Kullanıcı listesini de uygulama örnekleri arasında paylaşılır.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>SSO Yardımlı aracısı için SSO açma kapatma

Cihazda yüklü herhangi bir aracı kullanmak bir uygulama için olanağıdır **varsayılan olarak kapalı**. Aracı ile uygulamanızı kullanmak için bazı ek yapılandırma adımları ve uygulamanıza kod ekleyin.

İzlenmesi gereken adımlar şunlardır:

1. Uygulama kodunuzun çağrıda MS SDK Aracısı modunu etkinleştirin.
2. Yeni bir yeniden yönlendirme URI'Sİ'kurmak ve hem uygulama hem de uygulama kaydınızı sağlayın.
3. Bir URL şeması kaydediliyor.
4. iOS9 desteği: izin Info.plist dosyanıza ekleyin.

#### <a name="step-1-enable-broker-mode-in-your-application"></a>1. adım: uygulamanızda Aracısı modu etkinleştir

"Bağlam" veya ilk kurulum kimlik doğrulaması nesne oluşturduğunuzda aracı kullanmak için uygulamanızı özelliği etkinleştirilir. Kodunuzda, kimlik bilgileri türü ayarlayarak bunu yapabilirsiniz:

```
/*! See the ADCredentialsType enumeration definition for details */
@propertyADCredentialsType credentialsType;
```
`AD_CREDENTIALS_AUTO` Ayarı, aracıya duyurmak denemek Microsoft kimlik SDK'sı izin `AD_CREDENTIALS_EMBEDDED` arama aracısı için Microsoft kimlik SDK'sı önler.

#### <a name="step-2-registering-a-url-scheme"></a>2. adım: bir URL şeması kaydediliyor

Microsoft Identity platformu URL'leri Aracısı çağırın, sonra uygulamanıza geri denetimi döndürmek için kullanır. Bu gidiş dönüş tamamlamak için Microsoft Identity platformu hakkında öğrenmiş olacaksınız uygulamanız için kayıtlı bir URL şeması gerekir. Bu, daha önce uygulamanızla kaydetmiş olabileceği diğer uygulama düzenleri yanı sıra olabilir.

> [!WARNING]
> URL düzeni aynı URL şeması kullanarak başka bir uygulama olasılığını en aza indirmek için oldukça benzersiz yaparak öneririz. Apple app Store'da kaydedilen URL şemalarını benzersizlik değil.
> 
> 

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

#### <a name="step-3-establish-a-new-redirect-uri-with-your-url-scheme"></a>3. adım: yeni bir yeniden yönlendirme URI'si ile URL düzeni oluştur

Her zaman doğru uygulama için kimlik bilgisi belirteçleri biz döndürdüğünden emin olmak için biz iOS işletim sistemi doğrulayabilirsiniz şekilde uygulamanıza geri arama emin olmak gerekiyor. İOS işletim sistemi bunu çağırma uygulamanın paket Kimliğini Microsoft Aracısı uygulamalarına bildirir. Bu, standart dışı bir uygulama tarafından sahte olamaz. Bu nedenle, ki bu belirteçleri doğru uygulama döndürüldüğünden emin olmak için aracı uygulamamız URI'si ile birlikte yararlanın. Bu benzersiz yeniden yönlendirme URI'si her iki uygulamanızda kurmak ve bizim Geliştirici Portalı'nda bir yeniden yönlendirme URI'si olarak ayarlamak gerekli.

Uygulamanızın yeniden yönlendirme URI'si düzgün biçimde olmalıdır:

`<app-scheme>://<your.bundle.id>`

örn: *x msauth mytestiosapp://com.myapp.mytestapp*

Bu yeniden yönlendirme URI'sini kullanarak, uygulama kayıt belirtilmesi gerekiyor [Azure portalında](https://portal.azure.com/). Azure AD uygulama kaydı hakkında daha fazla bilgi için bkz. [Azure Active Directory ile tümleştirme](active-directory-how-to-integrate.md).

##### <a name="step-3a-add-a-redirect-uri-in-your-app-and-dev-portal-to-support-certificate-based-authentication"></a>Adım 3a: sertifika tabanlı kimlik doğrulamasını desteklemek için uygulama ve geliştirme Portalı'nda bir yeniden yönlendirme URI'si ekleyin

Destek sertifika tabanlı kimlik doğrulaması ikinci "msauth" uygulamanızda kaydedilmesi gerekir ve [Azure portalında](https://portal.azure.com/) uygulamanızda bu desteği eklemek istiyorsanız, sertifika kimlik doğrulamasını işlemek için.

`msauth://code/<broker-redirect-uri-in-url-encoded-form>`

örn: *msauth://code/x-msauth-mytestiosapp%3A%2F%2Fcom.myapp.mytestapp*

#### <a name="step-4-ios9-add-a-configuration-parameter-to-your-app"></a>4. adım: iOS9: bir yapılandırma parametresi uygulamanıza ekleme

ADAL kullanan – canOpenURL: aracı cihaz üzerinde yüklü olup olmadığını denetlemek için. İOS 9 Apple ne uygulama düzenleri sorgulayabilir aşağı kilitli. LSApplicationQueriesSchemes kısmına "msauth" eklemeniz gerekecektir, `info.plist file`.

<key>LSApplicationQueriesSchemes</key> <array> <string>msauth</string></array>

### <a name="youve-configured-sso"></a>SSO yapılandırdınız!

Artık Microsoft kimlik SDK'sı otomatik olarak hem uygulamalarınızda paylaşımı kimlik bilgileri ve kendi cihazında varsa aracı çağırma.