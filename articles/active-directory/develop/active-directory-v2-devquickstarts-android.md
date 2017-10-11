---
title: "Azure Active Directory v2.0 Android uygulaması | Microsoft Docs"
description: "Kullanıcıların kişisel bir Microsoft hesabı ve iş veya Okul hesapları hem çağrıları grafik API'si ile üçüncü taraf kitaplıklarını kullanarak oturum açtığında bir Android uygulaması oluşturma."
services: active-directory
documentationcenter: 
author: danieldobalian
manager: mbaldwin
editor: 
ms.assetid: 16294c07-f27d-45c9-833f-7dbb12083794
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.openlocfilehash: c0a5a818c61f7af7ff04bf890b54e8364f3b21b1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="add-sign-in-to-an-android-app-using-a-third-party-library-with-graph-api-using-the-v20-endpoint"></a>Grafik API'si v2.0 uç noktası kullanarak bir üçüncü taraf kitaplık kullanılarak bir Android uygulaması için oturum açma ekleme
Microsoft kimlik platformu OAuth2 ve OpenID Connect gibi açık standartlar kullanır. Geliştiricilerin hizmetlerimizle tümleştirmek istediği herhangi bir kitaplığı kullanabilirsiniz. Geliştiricilerin platformumuzu diğer kitaplıklarla birlikte kullanmak için Microsoft identity platformuna bağlanmak için üçüncü taraf kitaplıklarını yapılandırmak nasıl göstermek için bunun gibi birkaç izlenecek yazdıktan. Uygulayan çoğu kitaplık [RFC6749 OAuth2 belirtimi](https://tools.ietf.org/html/rfc6749) Microsoft identity platformuna bağlanabilir.

Bu izlenecek yol oluşturur uygulamasıyla kullanıcılar kendi kuruluşa oturum açabilir ve ardından kendileri için kuruluşlarında grafik API'sini kullanarak arama.

OAuth2 veya Openıd Connect kullanmaya yeni başladıysanız Bu örnek yapılandırmanın çoğunu sizin için anlamlı değil. Okumanızı öneririz [2.0 protokolleri - OAuth 2.0 yetkilendirme kodu akışı](active-directory-v2-protocols-oauth-code.md) arka planı için.

> [!NOTE]
> Bir ifade koşullu erişim ve Intune ilke yönetimi gibi OAuth2 veya Openıd Connect standartları olan bazı özellikleri, bizim açık kaynak Microsoft Azure kimlik kitaplıkları kullanmanızı gerektirir.
> 
> 

V2.0 uç noktası, tüm Azure Active Directory senaryolarını ve özelliklerini desteklemez.

> [!NOTE]
> V2.0 uç kullanmanızın gerekli olup olmadığını belirlemek için okuyun [v2.0 sınırlamaları](active-directory-v2-limitations.md).
> 
> 

## <a name="download-the-code-from-github"></a>Github'dan kodu indirme
Bu öğretici için kod [GitHub'da](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2) korunur.  İzlemek için [uygulamanın çatısını bir .zip karşıdan](https://github.com/Azure-Samples/active-directory-android-native-oidcandroidlib-v2/archive/skeleton.zip) veya çatıyı kopyalayın:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

Örnek ayrıca indirebilirsiniz ve hemen başlayın:

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## <a name="register-an-app"></a>Bir uygulamayı kaydetme
En yeni bir uygulama oluşturma [uygulama kayıt portalı](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList), veya ayrıntılı adımları izleyin [v2.0 uç noktası ile bir uygulama nasıl](active-directory-v2-app-registration.md).  Emin olun:

* Kopya **uygulama kimliği** atanan uygulamanıza yakında gerekir çünkü.
* Ekleme **mobil** uygulamanız için platform.

> Not: Uygulama kayıt Portalı'nı sağlayan bir **yeniden yönlendirme URI'si** değeri. Ancak, bu örnekte, varsayılan değeri kullanmalıdır `https://login.microsoftonline.com/common/oauth2/nativeclient`.
> 
> 

## <a name="download-the-nxoauth2-third-party-library-and-create-a-workspace"></a>NXOAuth2 üçüncü taraf kitaplığını indirin ve çalışma alanı oluşturma
Bu kılavuz için github'dan Google Openıd Connect kod ile temel bir OAuth2 kitaplığı olan OIDCAndroidLib kullanır. Yerel uygulama profilini uygular ve kullanıcı yetkilendirme uç noktası destekler. Microsoft kimlik platformu ile tümleştirme için gereken her şeyi bunlar.

Bilgisayarınıza OIDCAndroidLib depoyu kopyalama.

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](../media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## <a name="set-up-your-android-studio-environment"></a>Android Studio ortamınızı ayarlama
1. Yeni bir Android Studio projesi oluşturun ve Sihirbazı'nda varsayılan değerleri kabul edin.
   
    ![Android Studio'da yeni proje oluşturma](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)
   
    ![Hedef Android cihazları](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)
   
    ![Mobil bir etkinlik ekleme](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)
2. Proje modüllerinizi ayarlamak için proje konumuna kopyalanan depoyu taşıyın. Ayrıca projesi oluşturun ve doğrudan proje konumuna kopyalayın.
   
    ![Proje modülleri](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)
3. Proje modülleri ayarları bağlam menüsünü kullanarak veya Ctrl + Alt + Maj + S kısayol kullanarak açın.
   
    ![Proje modülleri ayarları](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)
4. Proje kapsayıcı ayarları yalnızca istediğinden varsayılan uygulama modülünü Kaldır.
   
    ![Varsayılan uygulama modülü](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)
5. Modülleri geçerli projeye kopyalanan deposuna alın.
   
    ![İçeri aktarma gradle proje](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG) ![yeni modül sayfa oluştur](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)
6. İçin bu adımları yineleyin `oidlib-sample` modülü.
7. Oidclib bağımlılıkları denetleme `oidlib-sample` modülü.
   
    ![oidlib örnek modül oidclib bağımlılıkları](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)
8. Tıklatın **Tamam** ve gradle eşitleme için bekleyin.
   
    Settings.gradle gibi görünmelidir:
   
    ![Settings.gradle ekran görüntüsü](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)
9. Emin olmak için örnek uygulaması derleme düzgün çalışmasını örnek.
   
    Bu Azure Active Directory ile henüz kullanmanız mümkün olmayacaktır. Bazı uç noktaları öncelikle yapılandırmanız gerekecektir. Bu örnek uygulama özelleştirme başlamadan önce bir Android Studio sorunları yok sağlamaktır.
10. Derleme ve çalıştırma `oidlib-sample` Android Studio'da hedefi olarak.
    
    ![Oidlib örnek yapı ilerlemesi](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)
11. Silme `app ` Android Studio güvenliği silmez çünkü projeden modülü kaldırıldığında bırakıldı dizini.
    
    ![Uygulama dizinindeki içeren dosya yapısı](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)
12. Açık **yapılandırmalarını Düzenle** projeden modülü kaldırıldığında, aynı zamanda sol çalıştırma yapılandırmasını kaldırmak üzere menüsü.
    
    ![Düzen yapılandırmaları menüsü](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG)
    ![uygulama yapılandırmasını çalıştırın](../media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)

## <a name="configure-the-endpoints-of-the-sample"></a>Örnek uç noktalarını yapılandırma
Sahip olduğunuza `oidlib-sample` başarıyla çalışırken, Azure Active Directory ile bu çalışma almak için bazı uç noktaları düzenleyelim.

### <a name="configure-your-client-by-editing-the-oidcclientconfxml-file"></a>Oidc_clientconf.xml dosyasını düzenleyerek istemcinizi yapılandırmak
1. Yalnızca bir belirteç almak ve grafik API'sini çağırmak için OAuth2 akışları kullandığınızdan, OAuth2 yalnızca yapmak için istemcinin ayarlayın. OIDC bir sonraki örnekte gelecektir.
   
    ```xml
        <bool name="oidc_oauth2only">true</bool>
    ```
2. Kayıt Portalı'ndan alınan istemci Kimliğiniz yapılandırın.
   
    ```xml
        <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
        <string name="oidc_clientSecret"></string>
    ```
3. Yeniden yönlendirme URI'sine sahip bir aşağıdaki yapılandırın.
   
    ```xml
        <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
    ```
4. Grafik API'sine erişmek için gereken, kapsamı yapılandırın.
   
    ```xml
        <string-array name="oidc_scopes">
            <item>openid</item>
            <item>https://graph.microsoft.com/User.Read</item>
            <item>offline_access</item>
        </string-array>
    ```

`User.Read` Değeri `oidc_scopes` , kullanıcının temel profil imzalı okumanızı sağlar.
Kullanılabilir tüm kapsamların hakkında daha fazla bilgiyi [Microsoft Graph izin kapsamları](https://graph.microsoft.io/docs/authorization/permission_scopes).

İlgili açıklamalar istiyorsanız `openid` veya `offline_access` kapsamlarda Openıd Connect, bkz: [2.0 protokolleri - OAuth 2.0 yetkilendirme kodu akışı](active-directory-v2-protocols-oauth-code.md).

### <a name="configure-your-client-endpoints-by-editing-the-oidcendpointsxml-file"></a>Oidc_endpoints.xml dosyasını düzenleyerek, istemci uç noktalarını yapılandırma
* Açık `oidc_endpoints.xml` dosya ve aşağıdaki değişiklikleri yapın:
  
    ```xml
    <!-- Stores OpenID Connect provider endpoints. -->
    <resources>
        <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
        <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
        <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
        <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
    </resources>
    ```

OAuth2, protokol olarak kullanıyorsanız, bu uç noktaları asla değiştirmeniz gerekir.

> [!NOTE]
> Uç noktaları için `userInfoEndpoint` ve `revocationEndpoint` şu anda Azure Active Directory tarafından desteklenmemektedir. Bu varsayılan example.com değerle bırakırsanız, örnekte kullanılabilir olmadıklarını :-) uyarılmak
> 
> 

## <a name="configure-a-graph-api-call"></a>Bir grafik API çağrısı yapılandırın
* Açık `HomeActivity.java` dosya ve aşağıdaki değişiklikleri yapın:
  
    ```Java
       //TODO: set your protected resource url
        private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
    ```

Burada basit bir grafik API çağrısı bizim bilgileri döndürür.

Yapmanız gereken tüm değişiklikleri izinlerdir. Çalıştırma `oidlib-sample` uygulama ve tıklatın **oturum**.

Başarıyla kimlik doğruladınız sonra seçin **korumalı kaynak isteği** aramanız grafik API'si için test etmek için düğmesi.

## <a name="get-security-updates-for-our-product"></a>Ürünümüz için güvenlik güncelleştirmelerini alma
Ziyaret ederek güvenlik olayları hakkında bildirimleri almanızı öneririz [güvenlik TechCenter](https://technet.microsoft.com/security/dd252948) ve güvenlik önerisi uyarılarına abone.

