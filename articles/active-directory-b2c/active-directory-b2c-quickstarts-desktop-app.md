---
title: Hızlı Başlangıç - Azure Active Directory B2C kullanarak bir masaüstü uygulaması için oturum açma bilgileri ayarlama | Microsoft Docs
description: Hesap oturum açma bilgileri sağlamak için Azure Active Directory B2C’yi kullanan örnek bir ASP.NET masaüstü uygulaması çalıştırın.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: quickstart
ms.custom: mvc
ms.date: 11/30/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 94e7972530afee15937b13ae35239a64d9bc986e
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65190538"
---
# <a name="quickstart-set-up-sign-in-for-a-desktop-app-using-azure-active-directory-b2c"></a>Hızlı Başlangıç: Azure Active Directory B2C kullanarak bir masaüstü uygulaması için oturum açma ayarlama 

Azure Active Directory (Azure AD) B2C, uygulamanız, işletmeniz ve müşterileriniz için koruma sağlamak üzere bulut kimliği yönetimi sunar. Azure AD B2C; uygulamalarınızın, açık standart protokolleri kullanarak sosyal hesaplarda ve kurumsal hesaplarda kimlik doğrulaması gerçekleştirmesine olanak tanır. Bu hızlı başlangıçta bir sosyal kimlik sağlayıcısı kullanarak oturum açmak ve Azure AD B2C korumalı bir web API’sini çağırmak için bir Windows Presentation Foundation (WPF) masaüstü uygulaması kullanacaksınız.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2019](https://www.visualstudio.com/downloads/) ile **ASP.NET ve web geliştirme** iş yükü. 
- Facebook’tan, Google’dan, Microsoft’tan veya Twitter’dan bir sosyal hesap.
- GitHub’dan [zip dosyasını indirin](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop/archive/master.zip) veya örnek web uygulamasını kopyalayın.

    ```
    git clone https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop.git
    ```

## <a name="run-the-application-in-visual-studio"></a>Uygulamayı Visual Studio'da çalıştırma

1. Örnek uygulama proje klasöründeki **active-directory-b2c-wpf.sln** çözümünü Visual Studio'da açın.
2. Uygulamada hata ayıklamak için **F5**’e basın.

## <a name="sign-in-using-your-account"></a>Hesabınızı kullanarak oturum açın

1. **Kaydolma veya Oturum Açma** iş akışını başlatmak için **Oturum aç**’a tıklayın.

    ![Örnek uygulama](media/active-directory-b2c-quickstarts-desktop-app/wpf-sample-application.png)

    Örnek, çeşitli kaydolma seçeneklerini destekler. Bu seçenekler, sosyal kimlik sağlayıcısı kullanarak veya bir e-posta adresi kullanarak yerel bir hesap oluşturma içerir. Bu hızlı başlangıç için Facebook’tan, Google’dan, Microsoft’tan veya Twitter’dan bir sosyal kimlik sağlayıcısı hesabı kullanın. 


2. Azure AD B2C, örnek web uygulaması için Wingtip Toys adlı bir kurgusal markaya yönelik özel bir oturum açma sayfası sunar. Sosyal kimlik sağlayıcısı kullanarak kaydolmak için, kullanmak istediğiniz kimlik sağlayıcısının düğmesine tıklayın. 

    ![Oturum Açma veya Kaydolma sağlayıcısı](media/active-directory-b2c-quickstarts-desktop-app/sign-in-or-sign-up-wpf.png)

    Sosyal hesabınızla kimlik bilgileri ve sosyal hesabınızdaki bilgileri okumak için uygulamayı yetkilendirme (oturum açma) kullanarak kimlik doğrulaması. Erişim izni verdiğinizde uygulama sosyal hesabınızdan adınız ve şehriniz gibi profil bilgilerini alabilir. 

2. Kimlik sağlayıcısına ilişkin oturum açma işlemini tamamlayın.

    Yeni hesap profili ayrıntılarınız, sosyal hesabınızdaki bilgilerle önceden doldurulur.

## <a name="edit-your-profile"></a>Profilinizi düzenleme

Azure AD B2C, kullanıcılara profillerini güncelleme olanağı tanıyan bir işlev sunar. Örnek web uygulaması iş akışı için bir Azure AD B2C düzenleme profil kullanıcı akışını kullanır. 

1. Oluşturduğunuz profili düzenlemek için uygulamanın menü çubuğunda **Profili düzenle**’ye tıklayın.

    ![Profili düzenleme](media/active-directory-b2c-quickstarts-desktop-app/edit-profile-wpf.png)

2. Oluşturduğunuz hesapla ilişkili kimlik sağlayıcısını seçin. Örneğin, hesabınızı oluşturduğunuzda kimlik sağlayıcısı olarak Twitter’ı kullandıysanız ilişkili profil ayrıntılarını değiştirmek için Twitter’ı seçin.

3. **Görünen adınızı** veya **Şehrinizi** değiştirip **Devam**’a tıklayın.

    *Belirteç bilgileri* metin kutusunda yeni bir erişim belirteci görüntülenir. Profilinizdeki değişiklikleri doğrulamak istiyorsanız erişim belirtecini kopyalayıp https://jwt.ms belirteç kod çözücüsüne yapıştırın.

## <a name="access-a-protected-api-resource"></a>Korumalı bir API kaynağına erişme

Korumalı kaynaktan istekte bulunmak için **API’yi çağır**’a tıklayın. 

    ![Call API](media/active-directory-b2c-quickstarts-desktop-app/call-api-wpf.png)

    The application includes the Azure AD access token in the request to the protected web API resource. The web API sends back the display name contained in the access token.

Azure AD B2C korumalı bir web API’si için yetkili bir çağrıda bulunmak üzere Azure AD B2C kullanıcı hesabınızı başarıyla kullandınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Azure AD B2C hızlı başlangıçlarını veya öğreticilerini denemeyi planlıyorsanız Azure AD B2C kiracınızı kullanabilirsiniz. Artık ihtiyaç duymuyorsanız [Azure AD B2C kiracınızı silebilirsiniz](active-directory-b2c-faqs.md#how-do-i-delete-my-azure-ad-b2c-tenant).

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir örnek masaüstü uygulaması için kullanılır: 

* Özel oturum açma sayfasıyla oturum açmak
* Sosyal kimlik sağlayıcısı ile oturum açın
* Bir Azure AD B2C hesabı oluşturma
* Azure AD B2C tarafından korunan bir web API'sini çağırma

Kendi Azure AD B2C kiracınızı oluşturarak kullanmaya başlayın. 

> [!div class="nextstepaction"]
> [Azure portalında Azure Active Directory B2C kiracısı oluşturma](tutorial-create-tenant.md)
