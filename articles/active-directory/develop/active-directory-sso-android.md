---
title: Android ADAL kullanarak uygulamalar arası SSO'yu nasıl | Microsoft Docs
description: "Uygulamalarınızda çoklu oturum açmayı etkinleştirmek için ADAL SDK'ın özelliklerini kullanma "
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 40710225-05ab-40a3-9aec-8b4e96b6b5e7
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 06/13/2018
ms.author: celested
ms.reviewer: dadobali
ms.custom: aaddev
ms.openlocfilehash: 4abf6bd2d82753e22d4fde92e219109274ce36be
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436149"
---
# <a name="how-to-enable-cross-app-sso-on-android-using-adal"></a>Android ADAL kullanarak uygulamalar arası SSO'yu etkinleştirme
Kullanıcıların yalnızca bir kez kimlik bilgilerini girin ve otomatik olarak uygulamalar arasında iş bu kimlik bilgilerine sahip olması gerekir böylece çoklu oturum açma (SSO) sağlama, artık bir endüstri standardıdır. Bir telefon çağrısı veya kısa mesajla bir kod gibi ek bir faktörü (2FA) kez birlikte küçük ekranlarda, genellikle kullanıcı adı ve parola girme zorluk memnuniyetsizliğine kullanıcı sonuçlarda birden fazla kez oturum açma var.

Ayrıca, diğer uygulamalar gibi Microsoft Accounts veya Microsoft365 üzerinden bir iş hesabı kullanabilir bir kimlik platformu uygularsanız, müşteriler bu kimlik bilgileri, yayımcı ne olursa olsun tüm uygulamalar arasında kullanılabilir olmasını bekleyebilirsiniz.

Microsoft kimlik SDK ile birlikte Microsoft Identity platform müşterilerinizi SSO ile ya da uygulamaların kendi suite'te memnun edecek olanağı sağlar veya Authenticator uygulamalarda tüm ve aracı özelliği ile cihaz.

Müşterileriniz için SSO sağlamak için uygulamanızdaki SDK'yı yapılandırma Bu izlenecek yol size bildirir.

Önceki belge uygulamanızla tümleştirmek nasıl bildiğiniz varsayılır [Microsoft kimlik Android SDK'sı](https://github.com/AzureAD/azure-activedirectory-library-for-android).

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>Microsoft kimlik platformu SSO kavramları
### <a name="microsoft-identity-brokers"></a>Microsoft kimlik aracıları
Microsoft tüm mobil platformlar için kimlik bilgilerini farklı satıcılardan uygulamalar arasında köprüleme sağlayan uygulamalar sağlar ve kimlik bilgilerini doğrulamak nereden tek bir güvenli yer gerektiren özel gelişmiş özellikler sağlar. Bu SDK çağrıları **aracıları**. İOS ve Android üzerinde aracıları müşteriler bağımsız olarak yüklemek veya bazı veya tüm cihaz için çalışanlara yöneten bir şirket tarafından cihaza gönderilen indirilebilir uygulamalar aracılığıyla sağlanır. Bunlar aracı yalnızca bazı uygulamalar veya BT yöneticileri bağlamasına üzerinde temel cihazın tamamını için yönetme güvenliğini destekler. Windows bu işlev, işletim sistemine Web kimlik doğrulama aracısı olarak bilinen, yerleşik bir hesap Seçici tarafından sağlanır.

#### <a name="broker-assisted-logins"></a>Yardımlı oturumları aracı
Aracısı destekli Aracısı uygulama içinde ortaya ve Microsoft Identity platformundan bağımsız olarak tüm uygulamalar arasında cihaz kimlik bilgilerini paylaşmak için depolama ve güvenlik Aracı'nı kullanan oturum açma deneyimlerini oturumlardır. Uygulamalarınızı olan olduğu çıkarımında, kullanıcıların oturum açmak için aracı üzerinde güvenirsiniz. İOS ve Android'de bu aracılar müşteriler bağımsız olarak yüklemek veya kendi kullanıcı için cihaz yöneten bir şirket tarafından cihaza gönderilen indirilebilir uygulamalar aracılığıyla sağlanır. Bu tür bir uygulama örneği, ios'ta Microsoft Authenticator uygulamasıdır. Windows bu işlev, işletim sistemine Web kimlik doğrulama aracısı olarak bilinen, yerleşik bir hesap Seçici tarafından sağlanır.
Deneyimi platforma göre değişiklik gösterir ve bazen kullanıcılar için karışıklığa neden olabilir Aksi takdirde doğru yönetilen. Facebook uygulaması yüklü olan ve başka bir uygulamadan Facebook bağlanma'yı kullanırsanız bu deseni en biliyor. Microsoft Identity platformu aynı deseni kullanır.

Android, kullanıcı için daha az kesintiye uğratan bir durumdur, uygulama üzerinde hesap seçici görüntülenir.

#### <a name="how-the-broker-gets-invoked"></a>Aracının nasıl çağrılır
Uyumlu bir aracı Microsoft Authenticator uygulaması gibi bir cihazda yüklü değilse Microsoft kimlik SDK'ları otomatik olarak bir kullanıcı Microsoft gelen herhangi bir hesabı kullanarak oturum açması istedikleri belirtiyorsa, aracı çağırma iş yapın Kimlik platformu. 
 
 #### <a name="how-microsoft-ensures-the-application-is-valid"></a>Geçerli Microsoft uygulama nasıl sağlar?
 
 Bir uygulama çağrı kimliğini Aracısı sağlama gereksinimi Yardımlı Aracısı oturum açma bilgilerini sağlanan güvenlik için önemlidir. iOS ve Android kötü amaçlı uygulamalar "yasal bir uygulamanın tanımlayıcısı aldatıcı" ve yasal uygulama için gereken belirteçlerini almak için yalnızca belirli bir uygulama için geçerli olan benzersiz tanımlayıcıları uygulamaz. Microsoft her zaman zamanında doğru uygulama ile iletişim sağlamak için geliştirici özel redirectURI geliştiricilerin Microsoft ile kaydolurken sağlamanız istenir. **Geliştiriciler bu yeniden yönlendirme URI'si nasıl çalışıyorlardı aşağıda ayrıntılı olarak ele alınmıştır.** Bu özel redirectURI uygulamanın sertifika parmak izini içerir ve tarafından Google Play Store uygulaması için benzersiz olmasını oldunuz. Bir uygulama Aracısı çağırdığında, Aracı aracısı çağrılır sertifika parmak iziyle sağlamak için Android işletim sistemi ister. Aracısı, bu sertifika parmak izi kimlik sistemi çağrısında Microsoft'a sağlar. Uygulamanın sertifika parmak izini bize kayıt sırasında geliştirici tarafından sağlanan sertifika parmak izi ile eşleşmiyorsa, belirteçleri uygulamanın istediği kaynak için erişim reddedildi. Bu denetim, yalnızca geliştirici tarafından kayıtlı uygulama belirteçlerini almasını sağlar.

Aracılı SSO oturum açma bilgileri, aşağıdaki avantajlara sahiptir:

* Kullanıcı, satıcı ne olursa olsun tüm uygulamalar arasında SSO karşılaşır.
* Uygulamanızı koşullu erişim gibi daha gelişmiş iş özellikleri kullanabilir ve Intune senaryolarını destekler.
* Uygulamanızın iş kullanıcıları için sertifika tabanlı kimlik doğrulamasını destekler.
* Uygulama ve kullanıcı kimliği olarak daha güvenli oturum açma deneyimi ek güvenlik algoritmalarını ve şifreleme ile Aracısı uygulama tarafından doğrulandı.

Microsoft kimlik SDK'ları SSO'yu etkinleştirmek üzere aracı uygulamalarla nasıl çalıştığını, bir gösterim şu şekildedir:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
|  ADAL SDK  | |  ADAL SDK  |   |  ADAL SDK   |
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

### <a name="turning-on-sso-for-broker-assisted-sso"></a>SSO Yardımlı aracısı için SSO açma kapatma
Cihazda yüklü herhangi bir aracı kullanmak bir uygulama için olanağıdır **varsayılan olarak kapalı**. Aracı ile uygulamanızı kullanmak için bazı ek yapılandırma adımları ve uygulamanıza kod ekleyin.

İzlenmesi gereken adımlar şunlardır:

1. Uygulama kodunuzun arama MS SDK Aracısı modunda etkinleştirme
2. Yeni bir yeniden yönlendirme URI'Sİ'kurmak ve hem uygulama hem de uygulama kaydınızı sağlayın
3. Android bildirimindeki doğru izinleri ayarlama

#### <a name="step-1-enable-broker-mode-in-your-application"></a>1. adım: uygulamanızda Aracısı modu etkinleştir
"Ayarlar" veya ilk kurulum kimlik doğrulaması örneğinizin oluşturduğunuzda aracı kullanmak için uygulamanızı özelliği etkinleştirilir. Uygulamanızda Bunu yapmak için:

```
AuthenticationSettings.Instance.setUseBroker(true);
```

#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a>2. adım: yeni bir yeniden yönlendirme URI'si ile URL düzeni oluştur
Doğru uygulama recevies emin olmak için döndürülen kimlik bilgisi, orada belirteçler Android işletim sistemi doğrulayabilirsiniz bir biçimde çağrı uygulamanızı emin olmak için bir gerekli değildir. Android işletim sistemi, Google Play Mağazası'nda sertifika karmasını kullanır. Bu sertifikanın karması, standart dışı bir uygulama tarafından sahte olamaz. Aracı uygulama URI'sini yanı sıra, Microsoft belirteçleri doğru uygulamaya döndürülür sağlar. Benzersiz bir yeniden yönlendirme URI'si uygulamayı kayıtlı olması gerekiyor.

Uygulamanızın yeniden yönlendirme URI'si düzgün biçimde olmalıdır:

`msauth://packagename/Base64UrlencodedSignature`

örn: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

Bu yeniden yönlendirme URI'sini kullanarak, uygulama kayıt kaydedebilirsiniz [Azure portalında](https://portal.azure.com/). Azure AD uygulama kaydı hakkında daha fazla bilgi için bkz. [Azure Active Directory ile tümleştirme](active-directory-how-to-integrate.md).

#### <a name="step-3-set-up-the-correct-permissions-in-your-application"></a>3. adım: uygulamanızda doğru izinleri ayarlayın
Android Aracısı uygulama, uygulamalar arasında kimlik bilgilerini yönetmek için Android işletim sistemi Hesap Yöneticisi özelliğini kullanır. Android'de aracı kullanmak için uygulama bildiriminizi Accountmanager'a hesaplarını kullanmak için izinleri olmalıdır. Bu izinleri ayrıntılı olarak ele alınmıştır [burada Google belgeler için Hesap Yöneticisi](http://developer.android.com/reference/android/accounts/AccountManager.html)

Özellikle, bu izinler şunlardır:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>SSO yapılandırdınız!
Artık Microsoft kimlik SDK'sı otomatik olarak hem uygulamalarınızda paylaşımı kimlik bilgileri ve kendi cihazında varsa aracı çağırma.

