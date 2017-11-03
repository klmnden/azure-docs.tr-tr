---
title: "Uygulamalar arası SSO'nun ADAL kullanarak Android üzerinde etkinleştirme | Microsoft Docs"
description: "Çoklu oturum açma, uygulamalar arasında etkinleştirmek için ADAL SDK özelliklerini kullanma "
services: active-directory
documentationcenter: 
author: danieldobalian
manager: mbaldwin
editor: 
ms.assetid: 40710225-05ab-40a3-9aec-8b4e96b6b5e7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 04/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: 9c7e959530a836fe5ddf74708363a636c39b3cc6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-enable-cross-app-sso-on-android-using-adal"></a>Uygulamalar arası SSO'nun ADAL kullanarak Android üzerinde etkinleştirme
Kullanıcıların yalnızca bir kez kimlik bilgilerini girin ve bu kimlik bilgilerini otomatik olarak gerekir böylece çoklu oturum açma (SSO) iş arasında sağlayan uygulamalar artık müşteriler tarafından bekleniyordu. Ekranda bir telefon araması veya ilerideki kodu gibi ek bir etmen (2FA) kez birlikte küçük, genellikle kullanıcı adı ve parola girme zorluk hızlı memnuniyetsizliği kullanıcı sonuçlarında ürününüz için birden fazla kez bunun var.

Ayrıca, Microsoft Accounts veya Office365 iş hesabından gibi diğer uygulamaları kullanabilir bir kimlik platformu uygularsanız, müşteriler bu kimlik bilgileri, tüm uygulamalarını satıcı olsun genelinde kullanılabilir olmasını bekler.

Bizim Microsoft Identity SDK ile birlikte Microsoft Identity platform bu sabit iş sizin için yapar ve SSO müşterilerinizle kendi uygulama paketinin içinde ya da delight olanağı sağlar veya bizim Aracısı özellik ve tüm aygıt arasında kimlik doğrulayıcı uygulamalar ile.

Bu kılavuz, müşterilerinize Bu kolaylık sağlamak için uygulamanızda bizim SDK'yı yapılandırmak nasıl söyler.

Bu kılavuz için geçerlidir:

* Azure Active Directory
* Azure Active Directory B2C
* Azure Active Directory B2B
* Azure Active Directory Koşullu Erişim

Önceki belge bildiğiniz varsayar nasıl [sağlamak eski portalı uygulamalarında Azure Active Directory için](active-directory-how-to-integrate.md) ve uygulamanız ile tümleşik [Microsoft Identity Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android).

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>Microsoft Identity platformuna SSO kavramlar
### <a name="microsoft-identity-brokers"></a>Microsoft Identity aracıları
Microsoft kimlik farklı satıcılardan uygulamalar arasında köprü oluşturma için izin uygulamaları mobil her platform için sağlar ve kimlik doğrulamaya nerede tek güvenli bir yerde gerektiren özel Gelişmiş özellikleri sağlar. Bunlar diyoruz **aracıların**. İOS ve Android bunlar müşteriler bağımsız olarak yüklemek veya bazı veya tüm cihaz için kendi çalışan yöneten bir şirket tarafından cihaza gönderilir indirilebilir uygulamaları aracılığıyla sağlanır. Bu aracıları yönetme güvenlik yalnızca bazı uygulamalar veya ne BT yöneticileri işlemleriniz bağlı tüm cihaz desteği. Windows, bu işlev, Web kimlik doğrulama aracısı olarak teknik olarak bilinen işletim sistemi içinde yerleşik bir hesabı seçicide tarafından sağlanır.

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
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
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
 
 Aracısı Aracısı sağladığımız güvenlik önemli bir uygulama çağrısı kimliğinden emin olmak için gereken oturumları destekli. İOS ne Android kötü amaçlı uygulamalar "yasal uygulamanın tanımlayıcı aldatıcı" ve yasal uygulamanın amacı belirteçleri almak için yalnızca belirli bir uygulama için geçerli olan benzersiz tanımlayıcıları zorlar. Biz her zaman doğru uygulama çalışma zamanında ile iletişim kuran emin olmak için size özel redirectURI kendi uygulama Microsoft ile kaydedilirken sağlamak için geliştirici isteyin. **Geliştiricilerin bu yeniden yönlendirme URI'si nasıl oluşturabilir aşağıda ayrıntılı olarak ele alınmıştır.** Bu özel redirectURI ve Google Play mağazası tarafından uygulama için benzersiz olmasını sağlamış uygulamanın sertifika parmak izi içerir. Bir uygulama Aracısı çağırdığında Aracısı Aracısı adlı sertifika parmak iziyle sağlamak için Android işletim sistemi sorar. Aracısı'nı bu sertifika parmak izi kimliğini sistemimizde çağrısında Microsoft'a sağlar. Sertifika parmak izi uygulamanın bize kayıt sırasında geliştirici tarafından sağlanan sertifika parmak izi eşleşmiyorsa, biz uygulama istenirken kaynak belirteçleri için erişimi reddeder. Bu denetim yalnızca geliştirici tarafından kayıtlı uygulama belirteçlerini almasını sağlar.

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

Daha iyi anlamanıza ve SSO Microsoft Identity platform ve SDK'ları kullanarak uygulamanızdaki uygulamanıza açabilmelisiniz bu arka plan bilgileri ile çağrılmak.

## <a name="enabling-cross-app-sso-using-adal"></a>ADAL kullanarak uygulamalar arası SSO'nun etkinleştirme
Burada ADAL Android SDK'sı için kullanın:

* Aracı olmayan Yardımlı SSO Uygulama paketiniz için Aç
* Aracısı destekli SSO desteği Aç

### <a name="turning-on-sso-for-non-broker-assisted-sso"></a>Aracı olmayan için SSO üzerinde kapatma SSO Destekli
Uygulamalar arasında aracı olmayan Yardımlı SSO için Microsoft Identity SDK'ları SSO karmaşıklığını çoğunu sizin için yönetin. Bu doğru kullanıcı önbellekte bulma ve bakımı, sorgu oturum açan kullanıcıların listesini içerir.

Aşağıdakileri yapmak gereken kendi uygulamalarında SSO'yu etkinleştirmek için:

1. Tüm uygulamalar kullanıcı aynı istemci Kimliğini veya uygulama kimliği emin olun
2. Tüm uygulamalarınızın aynı SharedUserID kümesine sahip olun.
3. Depolama paylaştırabilirsiniz tüm uygulamalar aynı imzalama sertifikası Google Play Mağazası'ndan paylaşmadığından emin olun.

#### <a name="step-1-using-the-same-client-id--application-id-for-all-the-applications-in-your-suite-of-apps"></a>1. adım: aynı istemci Kimliğini kullanarak / uygulama kimliği paketiniz uygulamalarının tüm uygulamalar için
Belirteçleri, uygulamalar arasında paylaşmak için izin bilmeniz Microsoft Identity Platform sırayla uygulamalarınızın her birinde aynı istemci Kimliğini veya uygulama kimliği paylaşmak gerekir Bu portalda ilk uygulamanızı kaydolurken, size sağlanan benzersiz bir tanımlayıcıdır.

Aynı uygulama kimliği kullanıyorsa, farklı uygulamalar Microsoft Identity hizmetine nasıl kullanacağını merak ediyor olabilirsiniz Yanıt olan **yeniden yönlendirme URI'ler**. Her uygulamanın birden çok yeniden yönlendirme hazırlama Portalı'nda kayıtlı URI'ler olabilir. Her uygulama paketiniz içinde farklı bir yeniden yönlendirme URI'sine sahip olur. Bu sistem, bir örnek aşağıda verilmiştir:

App1 yeniden yönlendirme URİ'si:`msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D`

App2 yeniden yönlendirme URİ'si:`msauth://com.example.userapp1/KmB7PxIytyLkbGHuI%2UitkW%2Fejk%4E`

App3 yeniden yönlendirme URİ'si:`msauth://com.example.userapp2/Pt85PxIyvbLkbKUtBI%2SitkW%2Fejk%9F`

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

#### <a name="step-2-configuring-shared-storage-in-android"></a>2. adım: Android paylaşılan depolama ortamı yapılandırma
Ayarı `SharedUserID` bu belgenin kapsamı dışındadır, ancak üzerinde Google Android belgeleri okuyarak öğrenilebilecek [bildirim](http://developer.android.com/guide/topics/manifest/manifest-element.html). Önemli olan, sharedUserID çağrılacağı ve tüm uygulamalar için kullanmak istediğinize karar ' dir.

Bulduktan sonra `SharedUserID` tüm uygulamalarınızda SSO kullanmaya hazır olursunuz.

> [!WARNING]
> Uygulamalar arasında depolama paylaştığınızda herhangi bir uygulama bu bölge kullanıcılar silin veya tüm belirteçleri, uygulamanızda da kötüsü silin. Bu iş arka plan için belirteçleri kullanan uygulamalar varsa, özellikle felaket niteliğinde değildir. Depolama paylaşımı Microsoft Identity SDK'ları aracılığıyla tüm kaldırma işlemleri çok dikkatli olmalıdır anlamına gelir.
> 
> 

İşte bu kadar! Microsoft Identity SDK'sı, artık tüm uygulamalar için kimlik bilgileri paylaşacaktır. Kullanıcı listesini, ayrıca uygulama örnekleri arasında paylaşılır.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>SSO destekli aracısı için SSO üzerinde kapatma
Bir uygulama cihazda yüklü herhangi bir aracı kullanmak için özelliği **varsayılan olarak devre dışı**. Uygulamanızı Aracısı ile kullanmak için bazı ek yapılandırma adımları ve uygulamanız için bazı kod eklemeniz gerekir.

İzlemeniz gereken adımlar şunlardır:

1. MS SDK, uygulama kodun çağrıda Aracısı modunu etkinleştir
2. Yeni bir yeniden yönlendirme URI'sine kurmak ve hem uygulama hem de uygulama kaydınızı sağlayın
3. Android bildirimindeki doğru izinleri ayarlama

#### <a name="step-1-enable-broker-mode-in-your-application"></a>1. adım: uygulamanızı Aracısı modunda etkinleştirme
"Ayarlar" ya da kimlik doğrulama örneğinizi ilk kurulumu oluşturduğunuzda özelliği Aracısı'nı kullanmak, uygulamanız için etkinleştirilir. Kodunuzda ApplicationSettings türünüz ayarlayarak bunu yapabilirsiniz:

```
AuthenticationSettings.Instance.setUseBroker(true);
```


#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a>2. adım: yeni yeniden yönlendirme URI'si URL şeması ile oluşturma
Her zaman doğru uygulamaya kimlik bilgisi belirteçleri biz dönmek emin olmak için size geri Android işletim sistemi doğrulayabilirsiniz şekilde uygulamanıza diyoruz emin olmanız gerekir. Android işletim sistemi Google Play Mağazası'nda sertifika karmasını kullanır. Standart dışı bir uygulama tarafından sahte olamaz. Bu nedenle, biz bu belirteçleri doğru uygulama döndürüldüğünden emin olmak için Aracısı uygulamamız URI'sini birlikte yararlanın. Bizim Geliştirici Portalı'nda bir yeniden yönlendirme URI'si olarak ayarlayın ve bu benzersiz yeniden yönlendirme URI'si hem uygulamanızda kurmak isteriz.

Yeniden yönlendirme URI'si uygun biçiminde olmalıdır:

`msauth://packagename/Base64UrlencodedSignature`

örn: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

Bu yeniden yönlendirme URI'sini kullanarak, uygulama kaydı belirtilmesi gerekiyor [Azure portal](https://portal.azure.com/). Azure AD uygulama kaydı hakkında daha fazla bilgi için bkz: [Azure Active Directory ile tümleştirme](active-directory-how-to-integrate.md).

#### <a name="step-3-set-up-the-correct-permissions-in-your-application"></a>3. adım: uygulamanızın doğru izinleri ayarlayın
Aracısı uygulamamız Android uygulamalar arasında kimlik bilgilerini yönetmek için Android işletim sistemi Hesap Yöneticisi özelliğini kullanır. Android Aracısı'nı kullanmak için uygulama bildiriminizi AccountManager hesaplarını kullanmak için izinleri olmalıdır. Bu ayrıntılı olarak ele alınmıştır [Google hesabı Manager belgeleri burada](http://developer.android.com/reference/android/accounts/AccountManager.html)

Özellikle, bu izinler şunlardır:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>SSO yapılandırdıktan!
Şimdi Microsoft Identity SDK otomatik olarak hem kimlik bilgileri, uygulamalar arasında paylaşmak ve cihazlarında varsa, aracı çağırma.

