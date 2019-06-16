---
title: Microsoft Account kimlik doğrulama - Azure App Service'ı yapılandırma
description: Uygulama Hizmetleri uygulamanıza Microsoft Account kimlik doğrulaması yapılandırmayı öğrenin.
author: mattchenderson
services: app-service
documentationcenter: ''
manager: syntaxc4
editor: ''
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/19/2018
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: e3da856efd7d44f15f9de27c9e38375d40dc211d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60850977"
---
# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>App Service uygulamanızı Microsoft Account login kullanacak şekilde yapılandırma
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu konuda, Azure App Service'ı Microsoft Account bir kimlik doğrulama sağlayıcısı olarak kullanmak üzere yapılandırma gösterilmektedir. 

## <a name="register-microsoft-account"> </a>Microsoft hesabı ile uygulamanızı kaydetme
1. Oturum [Azure portal]ve uygulamanıza gidin. Kopyalama, **URL**, daha sonra Microsoft Account Uygulamanızı yapılandırmak için kullanın.
2. Gidin [uygulamalarım] sayfasında Microsoft Account Developer Center'da ve oturum açın, Microsoft hesabınızla gerekirse.
3. Tıklayın **uygulama ekleme**, bir uygulama adı girin ve tıklayın **Oluştur**.
4. Not **uygulama kimliği**, daha sonra ihtiyaç duyacaksınız. 
5. "Platforms altında" tıklayın **Platform Ekle** ve "Web"'i seçin.
6. "Yeniden yönlendirme URI'leri" altında uygulamanız için uç nokta sağlayın ve ardından **Kaydet**. 
   
   > [!NOTE]
   > URL yoluyla eklenmiş uygulamanızın URI'dir, yeniden yönlendirme */.auth/login/microsoftaccount/callback*. Örneğin, `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
   > HTTPS şeması kullandığınızdan emin olun.
   
7. "Uygulama gizli öğeleri altında" tıklayın **yeni parola oluştur**. Görüntülenen değeri not edin. Sayfadan çıkmak sonra bir yeniden görüntülenmez.

    > [!IMPORTANT]
    > Parola, bir önemli güvenlik kimlik bilgisidir. Parola kimseyle paylaşmayın değil veya bir istemci uygulaması içinde dağıtın.
    
8. **Kaydet**'e tıklayın.

## <a name="secrets"> </a>App Service uygulamanızı Microsoft Account bilgilerini ekleyin
1. Geri [Azure portal], uygulamanıza gidin, tıklayın **ayarları** > **kimlik doğrulama / yetkilendirme**.
2. Kimlik doğrulama / yetkilendirme özelliği etkin değil, bu geçiş **üzerinde**.
3. Tıklayın **Microsoft hesabı**. Daha önce aldığınız uygulama kimliği ve parola değerleri yapıştırın ve isteğe bağlı olarak uygulamanızın gerektirdiği herhangi bir kapsam etkinleştirin. Daha sonra, **Tamam**'a tıklayın.
   
    ![][1]
   
    Varsayılan olarak, App Service kimlik doğrulaması sağlar, ancak site içerik ve API'ler için yetkili erişimi kısıtlamaz. Kullanıcılar, uygulama kodunuzda yetkilendirmeniz gerekir.
4. (İsteğe bağlı) Sitenizi yalnızca Microsoft hesabı ile kimliği doğrulanmış kullanıcılar için erişimi kısıtlamak için ayarlanmış **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem** için **Microsoft Account**. Bu, tüm istekleri kimliğinin doğrulanmasını gerektirir ve kimliği doğrulanmamış tüm istekleri kimlik doğrulaması için Microsoft hesabı yönlendirilirsiniz.
5. **Kaydet**’e tıklayın.

Şimdi uygulamanıza kimlik doğrulaması için Microsoft Account kullanmaya hazırsınız.

## <a name="related-content"> </a>İlgili içerik
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[Uygulamalarım]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure portal]: https://portal.azure.com/
