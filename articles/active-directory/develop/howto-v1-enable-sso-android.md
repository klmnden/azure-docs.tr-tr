---
title: Android ADAL kullanarak uygulamalar arası SSO'yu nasıl | Microsoft Docs
description: Uygulamalarınızda çoklu oturum açmayı etkinleştirmek için ADAL SDK'ın özelliklerini kullanma
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.assetid: 40710225-05ab-40a3-9aec-8b4e96b6b5e7
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: brandwe, jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: eb11a4a926c676d37a0bf6be456e3b831a5d8357
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65962647"
---
# <a name="how-to-enable-cross-app-sso-on-android-using-adal"></a>Nasıl yapılır: Android ADAL kullanarak uygulamalar arası SSO'yu etkinleştirin

[!INCLUDE [active-directory-develop-applies-v1-adal](../../../includes/active-directory-develop-applies-v1-adal.md)]

Çoklu oturum açma (SSO) yalnızca bir kez kimlik bilgilerini girin ve diğer uygulamalar (örneğin, Microsoft Accounts veya Microsoft 365'ten bir iş hesabı) kullanabilir platformları ve uygulamalar arasında otomatik iş bu kimlik bilgilerine sahip kullanıcılar sağlayan yok Yayımcı önemli.

Microsoft kimlik platformu, SDK'ları, tüm cihazlarda uygulamalar veya Aracısı yetenek ve Authenticator uygulamaları, kendi suite'te SSO'yu etkinleştirmek üzere kolaylaştırır.

Bu nasıl yapılır makalesinde müşterilerinize SSO sağlamak için uygulamanızdaki SDK'sını yapılandırmak öğreneceksiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu yöntem, bildiğinizi varsayar nasıl yapılır:

- Uygulamanızı Azure Active Directory (Azure AD) eski portalı kullanarak sağlayın. Daha fazla bilgi için bkz. [bir uygulamayı kaydetme](quickstart-register-app.md)
- Uygulamanızla tümleştirin [Azure AD Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android).

## <a name="single-sign-on-concepts"></a>Çoklu oturum açma kavramları

### <a name="identity-brokers"></a>Kimlik aracıları

Microsoft tüm mobil platformlar için kimlik bilgilerini farklı satıcılardan uygulamalar arasında köprü oluşturma ve kimlik bilgilerini doğrulamak nereden tek bir güvenli yer gerektiren gelişmiş özellikleri için izin uygulamalar sağlar. Bunlar adlandırılır **aracıları**.

İOS ve Android üzerinde aracıları müşteriler bağımsız olarak yüklemek veya çalışanlar için bazı yöneten bir şirket tarafından ya da tüm aygıtların cihaza gönderilen indirilebilir uygulamalar aracılığıyla sağlanır. Yalnızca bazı uygulamalar veya BT yöneticisi yapılandırmasına bağlı olarak tüm cihaz için güvenlik yönetimi aracıları desteği. Windows bu işlev, işletim sistemine Web kimlik doğrulama aracısı olarak bilinen, yerleşik bir hesap Seçici tarafından sağlanır.

#### <a name="broker-assisted-login"></a>Broker Yardımlı oturumu açma

Aracısı destekli Aracısı uygulama içinde ortaya ve kimlik bilgilerini geçerli kimlik platformu olan tüm uygulamalar arasında cihazdaki paylaşmak için depolama ve güvenlik aracı kullanın oturum açma deneyimlerini oturumlardır. Uygulamalarınızı olan olduğu çıkarımında, kullanıcıların oturum açmak için aracı üzerinde güvenirsiniz. İOS ve Android'de bu aracılar müşteriler bağımsız olarak yüklemek veya kendi kullanıcı için cihaz yöneten bir şirket tarafından cihaza gönderilen indirilebilir uygulamalar aracılığıyla sağlanır. Bu tür bir uygulama örneği, ios'ta Microsoft Authenticator uygulamasıdır. Windows bu işlev, işletim sistemine Web kimlik doğrulama aracısı olarak bilinen, yerleşik bir hesap Seçici tarafından sağlanır.
Deneyimi platforma göre değişiklik gösterir ve bazen kullanıcılar için karışıklığa neden olabilir Aksi takdirde doğru yönetilen. Facebook uygulaması yüklü olan ve başka bir uygulamadan Facebook bağlanma'yı kullanırsanız bu deseni en biliyor. Kimlik platformu aynı deseni kullanır.

Android, kullanıcı için daha az kesintiye uğratan bir durumdur, uygulama üzerinde hesap seçici görüntülenir.

#### <a name="how-the-broker-gets-invoked"></a>Aracının nasıl çağrılır

Microsoft Authenticator uygulaması gibi bir cihaz uyumlu bir aracı yüklendiyse kimlik SDK'ları otomatik olarak bir kullanıcı oturum açmak istedikleri gösterdiğinde İş Aracısı, çağırma kimlik platformu herhangi bir hesabı kullanarak yapar.

#### <a name="how-microsoft-ensures-the-application-is-valid"></a>Geçerli Microsoft uygulama nasıl sağlar?

Bir uygulama çağrı kimliğini Aracısı sağlama gereksinimi Yardımlı Aracısı oturum açma bilgilerini sağlanan güvenlik için önemlidir. iOS ve Android kötü amaçlı uygulamalar "yasal bir uygulamanın tanımlayıcısı aldatıcı" ve yasal uygulama için gereken belirteçlerini almak için yalnızca belirli bir uygulama için geçerli olan benzersiz tanımlayıcıları uygulamaz. Microsoft her zaman zamanında doğru uygulama ile iletişim sağlamak için geliştirici özel redirectURI geliştiricilerin Microsoft ile kaydolurken sağlamanız istenir. **Geliştiriciler bu yeniden yönlendirme URI'si nasıl çalışıyorlardı aşağıda ayrıntılı olarak ele alınmıştır.** Bu özel redirectURI uygulamanın sertifika parmak izini içerir ve tarafından Google Play Store uygulaması için benzersiz olmasını oldunuz. Bir uygulama Aracısı çağırdığında, Aracı aracısı çağrılır sertifika parmak iziyle sağlamak için Android işletim sistemi ister. Aracısı, bu sertifika parmak izi kimlik sistemi çağrısında Microsoft'a sağlar. Uygulamanın sertifika parmak izini bize kayıt sırasında geliştirici tarafından sağlanan sertifika parmak izi ile eşleşmiyorsa, belirteçleri uygulamanın istediği kaynak için erişim reddedildi. Bu denetim, yalnızca geliştirici tarafından kayıtlı uygulama belirteçlerini almasını sağlar.

Aracılı SSO oturum açma bilgileri, aşağıdaki avantajlara sahiptir:

* Kullanıcı, satıcı ne olursa olsun tüm uygulamalar arasında SSO karşılaşır.
* Uygulamanızı koşullu erişim gibi daha gelişmiş iş özellikleri kullanabilir ve Intune senaryolarını destekler.
* Uygulamanızın iş kullanıcıları için sertifika tabanlı kimlik doğrulamasını destekler.
* Uygulama ve kullanıcı kimliği olarak daha güvenli oturum açma deneyimi ek güvenlik algoritmalarını ve şifreleme ile Aracısı uygulama tarafından doğrulandı.

SDK'ları SSO'yu etkinleştirmek üzere aracı uygulamalarla nasıl çalıştığını, bir gösterim şu şekildedir:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    else's   |
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

### <a name="turning-on-sso-for-broker-assisted-sso"></a>SSO Yardımlı aracısı için SSO açma kapatma

Bir uygulamayı cihazda yüklü herhangi bir aracı kullanmak için özelliği varsayılan olarak kapalıdır. Aracı ile uygulamanızı kullanmak için bazı ek yapılandırma adımları ve uygulamanıza kod ekleyin.

İzlenmesi gereken adımlar şunlardır:

1. Uygulama kodunuzun arama MS SDK Aracısı modunda etkinleştirme
2. Yeni bir yeniden yönlendirme URI'Sİ'kurmak ve hem uygulama hem de uygulama kaydınızı sağlayın
3. Android bildirimindeki doğru izinleri ayarlama

#### <a name="step-1-enable-broker-mode-in-your-application"></a>1\. adım: Uygulamanızdaki Aracısı modunu etkinleştir

"Ayarlar" veya ilk kurulum kimlik doğrulaması örneğinizin oluşturduğunuzda aracı kullanmak için uygulamanızı özelliği etkinleştirilir. Uygulamanızda Bunu yapmak için:

```
AuthenticationSettings.Instance.setUseBroker(true);
```

#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a>2\. adım: Yeni bir yeniden yönlendirme URI'si ile URL düzeni oluştur

Doğru uygulama döndürülen aldığından emin olmak için kimlik bilgisi belirteç, orada Android işletim sistemi doğrulayabilirsiniz bir biçimde çağrı uygulamanızı emin olmak için bir gerekli değildir. Android işletim sistemi, Google Play Mağazası'nda sertifika karmasını kullanır. Bu sertifikanın karması, standart dışı bir uygulama tarafından sahte olamaz. Aracı uygulama URI'sini yanı sıra, Microsoft belirteçleri doğru uygulamaya döndürülür sağlar. Benzersiz bir yeniden yönlendirme URI'si uygulamayı kayıtlı olması gerekiyor.

Uygulamanızın yeniden yönlendirme URI'si düzgün biçimde olmalıdır:

`msauth://packagename/Base64UrlencodedSignature`

örn: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

Bu yeniden yönlendirme URI'sini kullanarak, uygulama kayıt kaydedebilirsiniz [Azure portalında](https://portal.azure.com/). Azure AD uygulama kaydı hakkında daha fazla bilgi için bkz. [Azure Active Directory ile tümleştirme](active-directory-how-to-integrate.md).

#### <a name="step-3-set-up-the-correct-permissions-in-your-application"></a>3\. adım: Uygulamanızdaki doğru izinleri ayarla

Android Aracısı uygulama, uygulamalar arasında kimlik bilgilerini yönetmek için Android işletim sistemi Hesap Yöneticisi özelliğini kullanır. Android'de aracı kullanmak için uygulama bildiriminizi Accountmanager'a hesaplarını kullanmak için izinleri olmalıdır. Bu izinleri ayrıntılı olarak ele alınmıştır [burada Google belgeler için Hesap Yöneticisi](https://developer.android.com/reference/android/accounts/AccountManager.html)

Özellikle, bu izinler şunlardır:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>SSO yapılandırdınız!

Artık SDK kimlik otomatik olarak hem paylaşım uygulamalarınızda kimlik bilgileri ve kendi cihazında varsa aracı çağırma.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [çoklu oturum açma SAML Protokolü](single-sign-on-saml-protocol.md)
