---
title: Facebook kimlik doğrulama - Azure App Service'ı yapılandırma
description: Facebook kimlik doğrulamasını App Service uygulamanız için yapılandırmayı öğrenin.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: syntaxc4
editor: ''
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/19/2018
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: f37a0c9e4c664ac9631a0a07fa6f114e62939845
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59522901"
---
# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>App Service uygulamanızı Facebook oturum açma bilgilerini kullanacak şekilde yapılandırma
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu konuda, Azure App Service'ı, Facebook kimlik doğrulama sağlayıcısı kullanmak için yapılandırma gösterilmektedir.

Bu konudaki yordamı tamamlamak için doğrulanmış e-posta adresi ve cep telefonu numarası olan bir Facebook hesabı olmalıdır. Yeni Facebook hesabı oluşturmak için Git [facebook.com].

## <a name="register"> </a>Facebook ile uygulamanızı kaydetme
1. Oturum [Azure portal]ve uygulamanıza gidin. Kopyalama, **URL**. Facebook Uygulamanızı yapılandırmak için kullanır.
2. Başka bir tarayıcı penceresinde gidin [Facebook geliştiriciler] hesap kimlik bilgileri Web sitesi ve, Facebook ile oturum açın.
3. (İsteğe bağlı) Önceden kaydolmadıysanız tıklayın **uygulamaları** > **geliştiricisi olarak kaydolun**, ardından ilkeyi kabul edin ve kayıt adımları izleyin.
4. Tıklayın **uygulamalarım** > **yeni uygulama Ekle**.
5. İçinde **görünen ad**, uygulamanız için benzersiz bir ad yazın. Ayrıca sağlar, **ilgili kişi e-posta**ve ardından **uygulama kimliği oluşturma** ve güvenlik denetimi tamamlandı. Bu yeni Facebook Uygulama geliştirici panosu açılır.
6. Altında **Facebook oturum açma**, tıklayın **ayarlanan**ve ardından **ayarları** sol taraftaki gezinti **Facebook oturum açma**.
7. Uygulamanızın ekleme **yeniden yönlendirme URI'si** için **geçerli OAuth yeniden yönlendirme URI'leri**, ardından **Değişiklikleri Kaydet**.
   
   > [!NOTE]
   > URL yoluyla eklenmiş uygulamanızın URI'dir, yeniden yönlendirme */.auth/login/facebook/callback*. Örneğin, `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. HTTPS şeması kullandığınızdan emin olun.
   > 
   > 
8. Sol gezinti bölmesinde tıklayın **ayarları** > **temel**. Üzerinde **uygulama gizli anahtarı** alan, tıklayın **Göster**, istenen ve değerlerini not edin, parolanızı girebilirsiniz **uygulama kimliği** ve **uygulama gizli anahtarı** . Bunlar daha sonra Azure'da Uygulamanızı yapılandırmak için kullanırsınız.
   
   > [!IMPORTANT]
   > Uygulama gizli anahtarı bir önemli güvenlik kimlik bilgisidir. Bu gizli dizi kimseyle paylaşmayın değil veya bir istemci uygulaması içinde dağıtın.
   > 
   > 
9. Uygulamayı kaydetmek için kullanılan Facebook uygulama yönetici hesabıdır. Bu noktada, yalnızca Yöneticiler bu uygulamaya oturum açabilirsiniz. Diğer Facebook hesaplarının kimliğini doğrulamak için tıklayın **uygulama incelemesi** ve etkinleştirme **olun \<uygulamanızın-adı > ortak** Facebook kimlik doğrulaması kullanarak genel genel erişimi etkinleştirmek için.

## <a name="secrets"> </a>Uygulamanıza Facebook bilgilerini ekleyin
1. Geri [Azure portal], uygulamanıza gidin. Tıklayın **ayarları** > **kimlik doğrulama / yetkilendirme**, emin olun **App Service kimlik doğrulaması** olduğu **üzerinde**.
2. Tıklayın **Facebook**, daha önce aldığınız uygulama kimliği ve uygulama gizli anahtarı değerleri yapıştırın, isteğe bağlı olarak uygulamanızın ihtiyaç duyduğu herhangi bir kapsam etkinleştirin ve ardından tıklayın **Tamam**.
   
    ![][0]
   
    Varsayılan olarak, App Service kimlik doğrulaması sağlar, ancak site içerik ve API'ler için yetkili erişimi kısıtlamaz. Kullanıcılar, uygulama kodunuzda yetkilendirmeniz gerekir.
3. (İsteğe bağlı) Sitenizi yalnızca Facebook tarafından kimliği doğrulanmış kullanıcılar için erişimi kısıtlamak için ayarlanmış **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem** için **Facebook**. Bu, tüm istekleri kimliğinin doğrulanmasını gerektirir ve kimliği doğrulanmamış tüm istekleri için kimlik doğrulaması için Facebook yönlendirilirsiniz.
4. Kimlik doğrulama işlemini yapılandırmayı bitirdiğinizde, tıklayın **Kaydet**.

Şimdi uygulamanıza kimlik doğrulaması için Facebook kullanmaya hazırsınız.

## <a name="related-content"> </a>İlgili içerik
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook geliştiriciler]: https://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: https://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure portal]: https://portal.azure.com/
