---
title: Hızlı Başlangıç - Azure Active Directory B2C kullanarak bir masaüstü uygulaması için oturum açma bilgileri ayarlama | Microsoft Docs
description: Hesap oturum açma bilgileri sağlamak için Azure Active Directory B2C’yi kullanan örnek bir ASP.NET masaüstü uygulaması çalıştırın.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: quickstart
ms.custom: mvc
ms.date: 2/13/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 4e82699b29a0333fae66fefdac8f5326c7e34e11
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34712479"
---
# <a name="quickstart-set-up-sign-in-for-a-desktop-app-using-azure-active-directory-b2c"></a>Hızlı Başlangıç - Azure Active Directory B2C kullanarak bir masaüstü uygulaması için oturum açma bilgileri ayarlama 

Azure Active Directory (Azure AD) B2C, uygulamanız, işletmeniz ve müşterileriniz için koruma sağlamak üzere bulut kimliği yönetimi sunar. Azure AD B2C; uygulamalarınızın, açık standart protokolleri kullanarak sosyal hesaplarda ve kurumsal hesaplarda kimlik doğrulaması gerçekleştirmesine olanak tanır.

Bu hızlı başlangıçta bir sosyal kimlik sağlayıcısı kullanarak oturum açmak ve Azure AD B2C korumalı bir web API’sini çağırmak için Azure AD B2C etkin örnek bir Windows Presentation Foundation (WPF) masaüstü uygulaması kullanıyorsunuz.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* **ASP.NET ve web geliştirme** iş yüküyle [Visual Studio 2017](https://www.visualstudio.com/downloads/). 
* Facebook’tan, Google’dan, Microsoft’tan veya Twitter’dan bir sosyal hesap.

## <a name="download-the-sample"></a>Örneği indirme

GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/master.zip) veya örnek web uygulamasını kopyalayın.

```
git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
```

## <a name="run-the-app-in-visual-studio"></a>Uygulamayı Visual Studio’da çalıştırma

Örnek uygulama proje klasöründen, Visual Studio’da `active-directory-b2c-wpf.sln` çözümünü açın.

Uygulamada hata ayıklamak için **F5**’e basın.

## <a name="create-an-account"></a>Hesap oluşturma

Bir Azure AD B2C ilkesi temelinde **Kaydolma veya Oturum Açma** iş akışını başlatmak için **Oturum aç**’a tıklayın.

![Örnek uygulama](media/active-directory-b2c-quickstarts-desktop-app/wpf-sample-application.png)

Örnek, sosyal kimlik sağlayıcısı kullanma veya bir e-posta adresiyle yerel bir hesap oluşturma da dahil olmak üzere çeşitli kaydolma seçeneklerini destekler. Bu hızlı başlangıç için Facebook’tan, Google’dan, Microsoft’tan veya Twitter’dan bir sosyal kimlik sağlayıcısı hesabı kullanın. 

### <a name="sign-up-using-a-social-identity-provider"></a>Sosyal kimlik sağlayıcısı kullanarak kaydolma

Azure AD B2C, örnek web uygulaması için Wingtip Toys adlı bir kurgusal markaya yönelik özel bir oturum açma sayfası sunar. 

1. Sosyal kimlik sağlayıcısı kullanarak kaydolmak için, kullanmak istediğiniz kimlik sağlayıcısının düğmesine tıklayın. 

    ![Oturum Açma veya Kaydolma sağlayıcısı](media/active-directory-b2c-quickstarts-desktop-app/sign-in-or-sign-up-wpf.png)

    Sosyal hesap kimlik bilgilerinizi kullanarak kimlik doğrulaması (oturum açma) gerçekleştirir ve uygulamaya sosyal hesabınızdaki bilgileri okuma yetkisi verirsiniz. Erişim izni verdiğinizde uygulama sosyal hesabınızdan adınız ve şehriniz gibi profil bilgilerini alabilir. 

2. Kimlik sağlayıcısına ilişkin oturum açma işlemini tamamlayın. Örneğin, Twitter’i seçtiyseniz Twitter kimlik bilgilerinizi girin **Oturum aç**’a tıklayın.

    ![Sosyal hesap kullanarak kimlik doğrulaması ve yetkilendirme gerçekleştirme](media/active-directory-b2c-quickstarts-desktop-app/twitter-authenticate-authorize-wpf.png)

    Yeni hesap profili ayrıntılarınız, sosyal hesabınızdaki bilgilerle önceden doldurulur. 

3. Dilerseniz ayrıntıları değiştirip **Devam**’a tıklayabilir. Girdiğiniz değerler Azure AD B2C kullanıcı hesabı profiliniz için kullanılır.

    ![Yeni hesaba kaydolma profil ayrıntıları](media/active-directory-b2c-quickstarts-desktop-app/new-account-sign-up-profile-details-wpf.png)

    Kimlik sağlayıcısı kullanan yeni bir Azure AD B2C kullanıcı hesabını başarıyla oluşturdunuz. Oturum açma işleminden sonra erişim belirteci *Belirteç bilgileri* metin kutusunda gösterilir. Erişim belirteci, API kaynağına erişilirken kullanılır.

## <a name="edit-your-profile"></a>Profilinizi düzenleme

Azure Active Directory B2C, kullanıcılara profillerini güncelleme olanağı tanıyan bir işlev sunar.  Örnek web uygulaması, iş akışı için bir Azure AD B2C düzenleme profil ilkesi kullanır. 

1. Oluşturduğunuz profili düzenlemek için **Profili düzenle**’ye tıklayın.

    ![Profili düzenleme](media/active-directory-b2c-quickstarts-desktop-app/edit-profile-wpf.png)

2. Oluşturduğunuz hesapla ilişkili kimlik sağlayıcısını seçin. Örneğin, hesabınızı oluşturduğunuzda kimlik sağlayıcısı olarak Twitter’ı kullandıysanız ilişkili profil ayrıntılarını değiştirmek için Twitter’ı seçin.

3. **Görünen adınızı** veya **Şehrinizi** değiştirip **Devam**’a tıklayın.

    *Belirteç bilgileri* metin kutusunda yeni bir erişim belirteci görüntülenir. Profilinizdeki değişiklikleri doğrulamak istiyorsanız erişim belirtecini kopyalayıp https://jwt.ms belirteç kod çözücüsüne yapıştırın.

## <a name="access-a-protected-web-api-resource"></a>Korumalı bir API kaynağına erişme

Azure AD B2C korumalı kaynağından istekte bulunmak için **API’yi çağır**’a tıklayın. 

![API’yi çağırma](media/active-directory-b2c-quickstarts-desktop-app/call-api-wpf.png)

Uygulama, korumalı web API’si kaynağına yönelik isteğe Azure AD erişim belirtecini ekler. Web API’si, erişim belirtecinde bulunan görünen adı tekrar gönderir.

Azure AD B2C korumalı bir web API’si için yetkili bir çağrıda bulunmak üzere Azure AD B2C kullanıcı hesabınızı başarıyla kullandınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure AD B2C hızlı başlangıçlarını veya öğreticilerini denemeyi planlıyorsanız Azure AD B2C kiracınızı kullanabilirsiniz. Artık ihtiyaç duymuyorsanız [Azure AD B2C kiracınızı silebilirsiniz](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Sonraki adımlar

Sonraki adımda kendi Azure AD B2C kiracınızı oluşturup kiracınızı kullanarak çalıştırmak için örneği yapılandıracaksınız. 

> [!div class="nextstepaction"]
> [Azure portalında Azure Active Directory B2C kiracısı oluşturma](active-directory-b2c-get-started.md)
