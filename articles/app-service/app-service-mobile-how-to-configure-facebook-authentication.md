---
title: "Uygulama Hizmetleri uygulamanız için Facebook kimlik doğrulamasını yapılandırma"
description: "Uygulama Hizmetleri uygulamanız için Facebook kimlik doğrulaması yapılandırma konusunda bilgi edinin."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: c1b4c91d384c56c4f55bf8d31ced250f51c0d837
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>App Service uygulamanızı Facebook oturum açma bilgilerini kullanacak şekilde yapılandırma
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu konu Azure App Service Facebook kimlik doğrulama sağlayıcısı olarak kullanmak üzere yapılandırmak nasıl gösterir.

Bu konudaki yordamı tamamlamak için doğrulanmış e-posta adresi ve cep telefonu numarası olan bir Facebook hesabı olması gerekir. Yeni bir Facebook hesabı oluşturmak için şu adrese gidin [facebook.com].

## <a name="register"></a>Facebook ile uygulamanızı kaydetme
1. Oturum [Azure portal]ve uygulamanıza gidin. Kopyalama, **URL**. Facebook Uygulamanızı yapılandırmak için bunu kullanır.
2. Başka bir tarayıcı penceresinde gidin [Facebook geliştiriciler] Web sitesi ve, Facebook ile oturum açma hesabı kimlik bilgileri.
3. (İsteğe bağlı) Zaten kaydolmadıysanız tıklatın **uygulamaları** > **kaydetmek geliştirici olarak**, ardından ilkeyi kabul etmek ve kayıt adımları izleyin.
4. Tıklatın **uygulamalarım** > **yeni bir uygulama eklemek** > **Web sitesi** > **atlayın ve uygulama kimliği oluşturma**. 
5. İçinde **görünen adı**, türü, uygulamanız için benzersiz bir ad yazın, **ilgili kişi e-posta**, seçin bir **kategori** uygulamanız için ardından **uygulama kimliği oluşturma**ve güvenlik denetimi tamamlandı. Bu, yeni Facebook uygulamanız için geliştirici panoya götürür.
6. "Facebook Login" altında tıklatın **Get Started**. Uygulamanızın eklemek **yeniden yönlendirme URI'si** için **geçerli OAuth yeniden yönlendirme URI'ler**, ardından **Değişiklikleri Kaydet**. 
   
   > [!NOTE]
   > URI'dir URL yolu ile eklenen, uygulamanızın, yeniden yönlendirme */.auth/login/facebook/callback*. Örneğin, `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. HTTPS şeması kullandığınızdan emin olun.
   > 
   > 
7. Sol gezinti bölmesinde tıklatın **ayarları**. Üzerinde **uygulama gizli anahtarı** alan, tıklatın **Göster**, istenen yeniden değerlerini not parolanızı sağlayın **uygulama kimliği** ve **uygulama gizli anahtarı** . Bunları daha sonra Azure'da Uygulamanızı yapılandırmak için kullanın.
   
   > [!IMPORTANT]
   > Uygulama gizli anahtarı bir önemli güvenlik kimlik bilgisidir. Bu gizli kimseyle paylaşmayın değil veya bir istemci uygulama kapsamındaki dağıtabilirsiniz.
   > 
   > 
8. Uygulamayı kaydetmek için kullanılan Facebook uygulaması bir yönetici hesabıdır. Bu noktada, yalnızca Yöneticiler bu uygulamasına oturum açabilir. Diğer Facebook hesaplarının kimliğini doğrulamak için tıklatın **uygulama İnceleme** ve etkinleştirme **olun < uygulamanızın-adı > ortak** Facebook kimlik doğrulaması kullanarak genel genel erişimi etkinleştirmek için.

## <a name="secrets"></a>Uygulamanıza eklemek Facebook bilgileri
1. Geri [Azure portal], uygulamanıza gidin. Tıklatın **ayarları** > **kimlik doğrulama / yetkilendirme**, emin olun **App Service kimlik doğrulaması** olan **üzerinde**.
2. Tıklatın **Facebook**, hangi daha önce aldığınız uygulama kimliği ve uygulama gizli anahtarı değerler yapıştırın, isteğe bağlı olarak, uygulamanız tarafından gerekli herhangi bir kapsam etkinleştirin ve ardından **Tamam**.
   
    ![][0]
   
    Varsayılan olarak, App Service kimlik doğrulaması sağlar ancak yetkili erişimi üzere site içeriğini ve API'lerini kısıtlamaz. Kullanıcılar, uygulama kodunuzda yetkilendirmeniz gerekir.
3. (İsteğe bağlı) Sitenize yalnızca Facebook tarafından kimliği doğrulanmış kullanıcılar için erişimi kısıtlamak üzere koyulan **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem** için **Facebook**. Bu, tüm istekleri doğrulanmasını gerektirir ve tüm kimliği doğrulanmamış istekler için Facebook kimlik doğrulaması için yönlendirilir.
4. Yapılandırma kimlik doğrulaması yapıldığında tıklatın **kaydetmek**.

Şimdi uygulamanıza kimlik doğrulaması için Facebook kullanmak hazırsınız.

## <a name="related-content"></a>İlgili içerik
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook geliştiriciler]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure portal]: https://portal.azure.com/
