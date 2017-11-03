---
title: "Test sürücü bir Azure AD B2C masaüstü uygulaması | Microsoft Docs"
description: "Test sürücü oturum açın, kaydolma, profili düzenlemek ve bir Azure AD B2C test ortamı'nı kullanarak kullanıcı Yolculuklar parola sıfırlama"
services: active-directory-b2c
documentationcenter: .net
author: saraford
manager: krassk
editor: PatAltimore
ms.assetid: 86293627-26fb-4e96-a76b-f263f9a945bd
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 10/31/2017
ms.author: saraford
ms.openlocfilehash: 9653d86dd5635016512bf6e6fafbf7195145ed07
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="test-drive-a-desktop-application-configured-with-azure-ad-b2c"></a>Test sürücü Azure AD B2C ile yapılandırılmış bir masaüstü uygulaması

Azure Active Directory B2C uygulaması, iş ve korumalı müşteriler tutmak için bulut kimlik yönetimi sağlar.  Bu hızlı başlangıç örnek Windows Presentation Foundation (WPF) masaüstü uygulaması göstermek için kullanır:

* Kullanarak **kaydolun veya oturum** ilkesi oluşturun veya bir sosyal kimlik sağlayıcısı veya bir e-posta adresi kullanarak yerel bir hesapla oturum açın. 
* **Bir API çağırma** görünen adınızı bir Azure AD B2C almak için kaynak güvenli.

## <a name="prerequisites"></a>Ön koşullar

* [Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi aşağıdaki iş yükleri ile yükleyin:
    - **.NET masaüstü geliştirme**

* Facebook, Google, Microsoft veya Twitter sosyal hesabından. Sosyal bir hesabınız yoksa, geçerli bir eposta adresi gereklidir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Örneği indirme

[İndirme veya örnek uygulama kopyalama](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop) github'dan.

## <a name="run-the-app-in-visual-studio"></a>Visual Studio'da Uygulama Çalıştırma

Örnek uygulama proje klasöründe açın `active-directory-b2c-wpf.sln` Visual Studio'da çözüm. 

Seçin **hata ayıklama > hata ayıklamayı Başlat** oluşturun ve uygulamayı çalıştırın. 

## <a name="create-an-account"></a>Bir hesap oluşturun

Tıklatın **oturum** başlatmak için **kaydolun veya oturum** iş akışı. Bir hesap oluşturulurken, var olan sosyal kimlik sağlayıcısı hesabını veya bir e-posta hesabı kullanabilirsiniz.

![Örnek uygulama](media/active-directory-b2c-quickstarts-desktop-app/wpf-sample-application.png)

### <a name="sign-up-using-a-social-identity-provider"></a>Sosyal kimlik sağlayıcısı kullanarak kaydolun

Sosyal kimlik sağlayıcısı kullanarak oturum açmak için kullanmak istediğiniz kimlik sağlayıcısı düğmesine tıklayın. Bir e-posta adresi kullanmayı tercih ederseniz, atlamak [bir e-posta adresi kullanarak kaydolma](#sign-up-using-an-email-address) bölümü.

![Oturum açın veya kaydolun sağlayıcısı](media/active-directory-b2c-quickstarts-desktop-app/sign-in-or-sign-up-wpf.png)

Sosyal hesabınızın kimlik bilgileri ve sosyal hesabınızdan bilgileri okumak için uygulamayı yetkilendirmeniz (oturum açma) kullanarak kimliğini doğrulaması gerekir. Erişim vererek, uygulama adı ve şehir gibi sosyal hesabından profil bilgilerini alabilir. 

![Kimlik doğrulama ve sosyal bir hesabı kullanarak Yetkilendirme](media/active-directory-b2c-quickstarts-desktop-app/twitter-authenticate-authorize-wpf.png)

Yeni hesap profili bilgilerinizi sosyal hesabınızdan bilgilerle önceden doldurulmuş haldedir. İster ve'ı tıklatırsanız Ayrıntılar Değiştir **devam**.

![Yeni hesap kayıt profili ayrıntıları](media/active-directory-b2c-quickstarts-desktop-app/new-account-sign-up-profile-details-wpf.png)

Bir kimlik sağlayıcısı kullanan yeni bir Azure AD B2C kullanıcı hesabını başarıyla oluşturdunuz. Oturum açma işleminden sonra erişim belirteci gösterilen *belirteci bilgisi* metin kutusu. Erişim belirteci API kaynağa erişilirken kullanılır.

![Yetkilendirme belirteci](media/active-directory-b2c-quickstarts-desktop-app/twitter-auth-token.png)

Sonraki adım: [profilinizi düzenleme atlama](#edit-your-profile) bölümü.

### <a name="sign-up-using-an-email-address"></a>Bir e-posta adresi kullanarak kaydolun

Sosyal hesabı kimlik doğrulaması kullanmayı seçerseniz, geçerli bir eposta adresi kullanarak bir Azure AD B2C kullanıcı hesabı oluşturabilirsiniz. Bir Azure AD B2C yerel kullanıcı hesabı kimlik sağlayıcısı olarak Azure Active Directory kullanır. E-posta adresinizi kullanmak için tıklatın **bir hesabınız yok mu? Şimdi kaydolun** bağlantı.

![Oturum açın veya e-posta kullanarak kaydolun](media/active-directory-b2c-quickstarts-desktop-app/sign-in-or-sign-up-email-wpf.png)

Geçerli bir e-posta adresi girin ve tıklayın **Gönder doğrulama kodu**. Geçerli bir e-posta adresi, Azure AD B2C ' doğrulama kodu almak için gereklidir.

E-posta almak ve tıklatın doğrulama kodunu girin **kodunu doğrulayın**.

Profil bilgilerinizi ekleyin ve tıklatın **oluşturma**.

![E-posta kullanarak yeni hesabıyla kaydolun](media/active-directory-b2c-quickstarts-desktop-app/sign-up-new-account-profile-email-wpf.png)

Yeni bir Azure AD B2C yerel kullanıcı hesabı başarıyla oluşturdunuz. Oturum açma işleminden sonra erişim belirteci gösterilen *belirteci bilgisi* metin kutusu. Erişim belirteci API kaynağa erişilirken kullanılır.

![Yetkilendirme belirteci](media/active-directory-b2c-quickstarts-desktop-app/twitter-auth-token.png)

## <a name="edit-your-profile"></a>Profilinizi düzenleme

Azure Active Directory B2C profillerini güncelleştirme yapmalarına izin vermek için işlevsellik sağlar. Tıklatın **Düzenle profili** oluşturduğunuz profili düzenlemek için.

![Profili düzenleme](media/active-directory-b2c-quickstarts-desktop-app/edit-profile-wpf.png)

Oluşturduğunuz hesapla ilişkili kimlik sağlayıcısı seçin. Örneğin, hesabınızı oluştururken kimlik sağlayıcısı olarak Twitter kullandıysanız, ilişkili profil ayrıntıları değiştirmek için Twitter seçin.

![Düzenlemek için bir sağlayıcı profili ile ilişkilendirilmiş seçin](media/active-directory-b2c-quickstarts-desktop-app/edit-account-choose-provider-wpf.png)

Değişiklik, **görünen adı** veya **Şehir**. 

![Profil güncelleştirme](media/active-directory-b2c-quickstarts-desktop-app/update-profile-wpf.png)

Yeni bir erişim belirteci görüntülenen *belirteci bilgisi* metin kutusu. Profilinizi değişiklikleri doğrulamak istiyorsanız, kopyalayın ve erişim belirteci belirteci decoder https://jwt.ms yapıştırın.

![Yetkilendirme belirteci](media/active-directory-b2c-quickstarts-desktop-app/twitter-auth-token.png)

## <a name="access-a-resource"></a>Bir kaynağa erişim

Tıklatın **API çağrısı** Azure AD B2C'ye bir isteği yapmak için kaynak https://fabrikamb2chello.azurewebsites.net/hello güvenli. 

![API çağrısı](media/active-directory-b2c-quickstarts-desktop-app/call-api-wpf.png)

Uygulama görüntülenen erişim belirteci içerir *belirteci bilgisi* metin kutusuna istek. API erişim belirtecinde yer alan görünen adı geri gönderir.

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adım, Kiracı kullanarak çalıştırmak için örnek kendi Azure AD B2C kiracısı oluşturma ve yapılandırmaktır. 

> [!div class="nextstepaction"]
> [Azure portalında bir Azure Active Directory B2C kiracısı oluşturma](active-directory-b2c-get-started.md)
