---
title: "Test sürücü bir Azure AD B2C web uygulaması | Microsoft Docs"
description: "Test sürücü oturum açın, kaydolma, profili düzenlemek ve bir Azure AD B2C test ortamı'nı kullanarak kullanıcı Yolculuklar parola sıfırlama"
services: active-directory-b2c
documentationcenter: .net
author: saraford
manager: krassk
editor: PatAltimore
ms.assetid: 2ffb780d-2c51-4c2e-b8d6-39c40a81a77e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/31/2017
ms.author: saraford
ms.openlocfilehash: 2a51f15fd38a901548dcf4c56922f9715c328463
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="test-drive-a-web-application-configured-with-azure-ad-b2c"></a>Test sürücü Azure AD B2C ile yapılandırılmış bir web uygulaması

Azure Active Directory B2C uygulaması, iş ve korumalı müşteriler tutmak için bulut kimlik yönetimi sağlar.  Bu hızlı başlangıç örnek bir Yapılacaklar listesi uygulaması göstermek için kullanır:

* Kullanarak **kaydolun veya oturum** ilkesi oluşturun veya bir sosyal kimlik sağlayıcısı veya bir e-posta adresi kullanarak yerel bir hesapla oturum açın. 
* Yapılacak iş öğeleri oluşturma ve düzenleme için Azure AD B2C tarafından güvence altına bir API çağırma.

## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi aşağıdaki iş yükleri ile yükleyin:
    - **ASP.NET ve web geliştirme**

* Facebook, Google, Microsoft veya Twitter sosyal hesabından. Sosyal bir hesabınız yoksa, geçerli bir eposta adresi gereklidir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Örneği indirme

[İndirme veya örnek uygulama kopyalama](https://github.com/Azure-Samples/active-directory-b2c-dotnet-webapp-and-webapi) github'dan.

## <a name="run-the-app-in-visual-studio"></a>Visual Studio'da Uygulama Çalıştırma

Örnek uygulama proje klasöründe açın `B2C-WebAPI-DotNet.sln` Visual Studio'da çözüm. 

Çözüm iki projeden oluşan:

* **TaskWebApp** – burada kullanıcının yönetebileceği kendi Yapılacaklar listesi öğeleri bir ASP.NET MVC web uygulaması.  
* **TaskService** – tüm CRUD işlemleri yöneten bir ASP.NET Web API arka uç kullanıcının yapılacaklar listesi öğelerini üzerinde gerçekleştirilir. Web uygulaması bu API'yi çağıran ve sonuçları görüntüler.

Bu Hızlı Başlangıç için her ikisini de çalıştırmanız gerekir. `TaskWebApp` ve `TaskService` aynı anda projeleri. 

1. Çözüm Gezgini'nde sağ tıklatın ve çözüm **başlangıç projelerini Ayarla...** . 
2. Seçin **birden fazla başlangıç projesi** radyo düğmesi.
3. Değişiklik **eylem** için her iki projeye de **Başlat**. **Tamam** düğmesine tıklayın.

![Visual Studio Başlangıç Sayfası Ayarla](media/active-directory-b2c-quickstarts-web-app/setup-startup-projects.png)

Seçin **hata ayıklama > hata ayıklamayı Başlat** oluşturun ve her iki uygulamayı çalıştırın. Her uygulamanın kendi bir tarayıcı sekmesi açar:

* `https://localhost:44316/`-Bu sayfaya ASP.NET web uygulamasıdır. Bu hızlı başlangıç uygulamada ile doğrudan etkileşim.
* `https://localhost:44332/`-Bu ASP.NET web uygulaması tarafından çağrılan API web sayfasıdır.

## <a name="create-an-account"></a>Bir hesap oluşturun

Tıklatın **kaydolun / oturum** başlatmak için ASP.NET web uygulamasının bağlantı **kaydolun veya oturum** iş akışı. Bir hesap oluşturulurken, var olan sosyal kimlik sağlayıcısı hesabını veya bir e-posta hesabı kullanabilirsiniz.

![Örnek ASP.NET web uygulaması](media/active-directory-b2c-quickstarts-web-app/web-app-sign-in.png)

### <a name="sign-up-using-a-social-identity-provider"></a>Sosyal kimlik sağlayıcısı kullanarak kaydolun

Sosyal kimlik sağlayıcısı kullanarak oturum açmak için kullanmak istediğiniz kimlik sağlayıcısı düğmesine tıklayın. Bir e-posta adresi kullanmayı tercih ederseniz, atlamak [bir e-posta adresi kullanarak kaydolma](#sign-up-using-an-email-address) bölümü.

![Oturum açın veya kaydolun sağlayıcısı](media/active-directory-b2c-quickstarts-web-app/sign-in-or-sign-up-web.png)

Sosyal hesabınızın kimlik bilgileri ve sosyal hesabınızdan bilgileri okumak için uygulamayı yetkilendirmeniz (oturum açma) kullanarak kimliğini doğrulaması gerekir. Erişim vererek, uygulama adı ve şehir gibi sosyal hesabından profil bilgilerini alabilir. 

![Kimlik doğrulama ve sosyal bir hesabı kullanarak Yetkilendirme](media/active-directory-b2c-quickstarts-web-app/twitter-authenticate-authorize-web.png)

Oturum açma işleminin kimlik sağlayıcısı için Son'u tıklatın. Örneğin, **oturum** Twitter düğmesi.

Yeni hesap profili bilgilerinizi sosyal hesabınızdan bilgilerle önceden doldurulmuş haldedir.

![Yeni hesap kayıt profili ayrıntıları](media/active-directory-b2c-quickstarts-web-app/new-account-sign-up-profile-details-web.png)

Görünen ad, iş unvanı ve şehir alanları güncelleştirin ve tıklayın **devam**.  Girdiğiniz değerler Azure AD B2C kullanıcı hesabı profiliniz için kullanılır.

Bir kimlik sağlayıcısı kullanan yeni bir Azure AD B2C kullanıcı hesabını başarıyla oluşturdunuz. 

Sonraki adım: [, talep görüntülemek için atlama](#view-your-claims) bölümü.

### <a name="sign-up-using-an-email-address"></a>Bir e-posta adresi kullanarak kaydolun

Sosyal hesabı kimlik doğrulaması kullanmayı seçerseniz, geçerli bir eposta adresi kullanarak bir Azure AD B2C kullanıcı hesabı oluşturabilirsiniz. Bir Azure AD B2C yerel kullanıcı hesabı kimlik sağlayıcısı olarak Azure Active Directory kullanır. E-posta adresinizi kullanmak için tıklatın **bir hesabınız yok mu? Şimdi kaydolun** bağlantı.

![Oturum açın veya e-posta kullanarak kaydolun](media/active-directory-b2c-quickstarts-web-app/sign-in-or-sign-up-email-web.png)

Geçerli bir e-posta adresi girin ve tıklayın **Gönder doğrulama kodu**. Geçerli bir e-posta adresi, Azure AD B2C ' doğrulama kodu almak için gereklidir. 

E-posta almak ve tıklatın doğrulama kodunu girin **kodunu doğrulayın**.

Profil bilgilerinizi ekleyin ve tıklatın **oluşturma**.

![E-posta kullanarak yeni hesabıyla kaydolun](media/active-directory-b2c-quickstarts-web-app/sign-up-new-account-profile-email-web.png)

Yeni bir Azure AD B2C yerel kullanıcı hesabı başarıyla oluşturdunuz.

## <a name="reset-your-password"></a>Parolanızı sıfırlama

Bir e-posta adresi kullanarak hesabınıza oluşturduysanız, Azure AD B2C parolalarını sıfırlamasına izin vermek için işlevselliğine sahiptir. Oluşturduğunuz profili düzenlemek için menü çubuğunda, profil adına tıklayın ve seçin **parola sıfırlama**.

Bunu girme ve tıklayarak e-posta adresinizi doğrulayın **Gönder doğrulama kodu**. E-posta adresinize bir doğrulama kodu gönderilir.

E-postayla aldığınız doğrulama kodunu girin ve tıklayın **kodunu doğrulayın**.

E-posta adresinizi doğrulandıktan sonra tıklatın **devam**.

Yeni parolanızı girin ve tıklayın **devam**.

## <a name="view-your-claims"></a>Talep görüntüleyin

Tıklatın **talep** son eyleminizi ile ilişkili talep görüntülemek için web uygulaması menü çubuğunda. 

![E-posta kullanarak yeni hesabıyla kaydolun](media/active-directory-b2c-quickstarts-web-app/view-claims-sign-up-web.png)

Bu örnekte, son eylem için idi. *oturum açın veya kaydolun* karşılaşırsınız. Bildirim **talep türü** `http://schemas.microsoft.com/claims/authnclassreference` olan `b2c_1_susi` son eylemini belirten kaydolma veya oturum açma. Son eylemi parola sıfırlama olduysa **talep türü** olacaktır `b2c_1_reset`.

## <a name="edit-your-profile"></a>Profilinizi düzenleme

Azure Active Directory B2C profillerini güncelleştirme yapmalarına izin vermek için işlevsellik sağlar. Web uygulaması menü çubuğunda, profil adına tıklayın ve seçin **Düzenle profili** oluşturduğunuz profili düzenlemek için.

![Profili düzenleme](media/active-directory-b2c-quickstarts-web-app/edit-profile-web.png)

Değişiklik, **görünen adı** ve **Şehir**.  Tıklatın **devam** profilinizi güncelleştirmek için.

![Profil güncelleştirme](media/active-directory-b2c-quickstarts-web-app/update-profile-web.png)

Sayfanın sağ üst kısmında görünen adı güncelleştirmelerinizi adınızı değiştirdikten sonra dikkat edin. 

Tıklatın **talep**. Yapılan değişiklikler **görünen adı** ve **Şehir** Taleplerde yansıtılır.

![Görünüm talepleri](media/active-directory-b2c-quickstarts-web-app/view-claims-update-web.png)

 Bildirim **talep türü** `http://schemas.microsoft.com/claims/authnclassreference` sürümüne güncelleştirilmiş `b2c_1_edit_profile` gerçekleştirilen son eylem profil düzenleme olduğunu belirten. Ayrıca unutmayın, adı ve şehir olduğundan yeni değerleri *Sara S.* ve *Seattle*.

## <a name="access-a-resource"></a>Bir kaynağa erişim

Tıklatın **Yapılacaklar listesi** girin ve yapılacaklar listesi öğelerinizi değiştirmek için. ASP.NET web uygulaması, web API kaynak isteme izni kullanıcının yapılacaklar listesi öğeleri üzerinde işlem gerçekleştirmek için isteğinde bir erişim belirteci içerir. 

Metin girin **yeni öğe** metin kutusu. Tıklatın **Ekle** Azure AD B2C çağırmak için bir Yapılacaklar listesi öğesini ekler web API güvenliği.

![Bir Yapılacaklar listesi öğesi ekleme](media/active-directory-b2c-quickstarts-web-app/add-todo-item-web.png)

## <a name="other-scenarios"></a>Diğer senaryolar

Sürücü test etmek için diğer senaryolar aşağıdaki gibidir:

* Oturum tıklatın ve uygulama dışı **Yapılacaklar listesi**. Oturum açmak için istenir ve liste öğeleri kalıcı nasıl dikkat edin. 
* Farklı türde bir hesabı kullanarak yeni bir hesap oluşturun. Örneğin, bir e-posta adresi kullanarak önceden bir hesabı oluşturduysanız sosyal kimlik sağlayıcısı kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adım, Kiracı kullanarak çalıştırmak için örnek kendi Azure AD B2C kiracısı oluşturma ve yapılandırmaktır. 

> [!div class="nextstepaction"]
> [Azure portalında bir Azure Active Directory B2C kiracısı oluşturma](active-directory-b2c-get-started.md)