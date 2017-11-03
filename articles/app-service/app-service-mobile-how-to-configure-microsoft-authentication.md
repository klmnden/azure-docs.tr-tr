---
title: "Uygulama Hizmetleri uygulamanız için Microsoft Account kimlik doğrulamasını yapılandırma"
description: "Uygulama Hizmetleri uygulamanız için Microsoft Account kimlik doğrulaması yapılandırma konusunda bilgi edinin."
author: mattchenderson
services: app-service
documentationcenter: 
manager: syntaxc4
editor: 
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 67386b03ae4cc683fe00e11e8dad19d1442eff09
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>Uygulama hizmeti uygulamanızı Microsoft Account oturum açma kullanacak şekilde yapılandırma
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu konu Azure App Service'nın Microsoft Account bir kimlik doğrulama sağlayıcısı olarak kullanmak üzere yapılandırmak nasıl gösterir. 

## <a name="register-microsoft-account"></a>Microsoft hesabı ile uygulamanızı kaydetme
1. Oturum [Azure portal]ve uygulamanıza gidin. Kopyalama, **URL**, daha sonra Microsoft Account Uygulamanızı yapılandırmak için kullanın.
2. Gidin [uygulamalarım] sayfasında Microsoft Account Developer Center'da ve oturum Microsoft hesabınızla gerekiyorsa.
3. Tıklatın **bir uygulama ekleyin**, bir uygulama adı yazın ve tıklayın **uygulaması oluşturma**.
4. Not **uygulama kimliği**, daha sonra ihtiyacınız olacak şekilde. 
5. "Platformları altında" **eklemek Platform** ve "Web"'i seçin.
6. "Yeniden yönlendirme URI'ler" altında uygulamanız için uç noktaya sağlayın, sonra **kaydetmek**. 
   
   > [!NOTE]
   > URI'dir URL yolu ile eklenen, uygulamanızın, yeniden yönlendirme */.auth/login/microsoftaccount/callback*. Örneğin, `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
   > HTTPS şeması kullandığınızdan emin olun.
   
7. "Uygulama parolaları" altında tıklatın **yeni bir parola oluşturmak**. Görüntülenen değeri not edin. Sayfayı çıktıktan sonra bir yeniden görüntülenmeyecek.

    > [!IMPORTANT]
    > Parola önemli güvenlik kimlik bilgisi değil. Parolanızı kimseyle paylaşmayın değil veya bir istemci uygulama kapsamındaki dağıtabilirsiniz.

## <a name="secrets"></a>Uygulama hizmeti uygulamanızı Microsoft hesabı Ekle bilgileri
1. Geri [Azure portal], uygulamanıza gidin, tıklatın **ayarları** > **kimlik doğrulama / yetkilendirme**.
2. Kimlik doğrulama / yetkilendirme özelliği etkin değil, bu geçiş **üzerinde**.
3. Tıklatın **Microsoft hesabı**. Daha önce aldığınız uygulama kimliği ve parola değerleri yapıştırın ve isteğe bağlı olarak uygulamanızın gerektirdiği herhangi bir kapsam etkinleştirin. Daha sonra, **Tamam**'a tıklayın.
   
    ![][1]
   
    Varsayılan olarak, App Service kimlik doğrulaması sağlar ancak yetkili erişimi üzere site içeriğini ve API'lerini kısıtlamaz. Kullanıcılar, uygulama kodunuzda yetkilendirmeniz gerekir.
4. (İsteğe bağlı) Sitenize yalnızca Microsoft hesabı tarafından kimliği doğrulanmış kullanıcılar için erişimi kısıtlamak üzere koyulan **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem** için **Microsoft Account**. Bu, tüm istekleri doğrulanmasını gerektirir ve tüm kimliği doğrulanmamış istekler Microsoft hesabı kimlik doğrulaması için yönlendirilirsiniz.
5. **Kaydet** düğmesine tıklayın.

Şimdi uygulamanıza kimlik doğrulaması için Microsoft Account kullanmak hazırsınız.

## <a name="related-content"></a>İlgili içerik
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[uygulamalarım]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure portal]: https://portal.azure.com/
