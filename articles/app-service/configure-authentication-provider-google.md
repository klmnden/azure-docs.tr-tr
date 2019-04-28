---
title: Google kimlik doğrulama - Azure App Service'ı yapılandırma
description: Google kimlik doğrulaması App Service uygulamanız için yapılandırmayı öğrenin.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: syntaxc4
editor: ''
ms.assetid: 2b2f9abf-9120-4aac-ac5b-4a268d9b6e2b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/19/2018
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: 50905b86924e0f564eaf4867c2906ad8740ddbaf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60851189"
---
# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>App Service uygulamanızı Google oturum açma bilgilerini kullanacak şekilde yapılandırma
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu konuda Google kimlik doğrulama sağlayıcısı kullanmak için Azure App Service yapılandırma gösterilmektedir.

Bu konudaki yordamı tamamlamak için doğrulanmış e-posta adresi olan bir Google hesabı olmalıdır. Yeni bir Google hesabı oluşturmak için [accounts.google.com](https://go.microsoft.com/fwlink/p/?LinkId=268302) adresine gidin.

## <a name="register"> </a>Google ile uygulamanızı kaydetme
1. Oturum [Azure portal]ve uygulamanıza gidin. Kopyalama, **URL**, daha sonra Google Uygulamanızı yapılandırmak için kullanın.
2. Gidin [Google API'leri](https://go.microsoft.com/fwlink/p/?LinkId=268303) Web sitesine gidin, Google hesabı kimlik bilgilerinizle oturum tıklayın **proje oluştur**, sağlayan bir **proje adı**, ardından  **Oluşturma**.
3. Proje oluşturulduktan sonra onu seçin. Proje panosunda tıklayın **API önizlemesine Git**.
4. Seçin **etkinleştirme API'leri ve Hizmetleri**. Arama **Google + API**ve bu seçeneği belirleyin. Ardından **etkinleştirme**.
5. Sol gezinti bölmesindeki **kimlik bilgilerini** > **OAuth onay ekranı**seçin, **e-posta adresi**, girin bir **Ürünadı**, tıklatıp **Kaydet**.
6. İçinde **kimlik bilgilerini** sekmesinde **kimlik bilgilerini oluştur** > **OAuth istemcisi kimliği**.
7. "İstemci kimliği oluşturma" ekranında seçin **Web uygulaması**.
8. App Service yapıştırın **URL** , daha önce kopyalanmasını **yetkili JavaScript kaynakları**, uygulamanızın yeniden yönlendirme yapıştırın URI **yeniden yönlendirme URI'SİNİN yetkili**. Yeniden yönlendirme URI'si uygulamanızı yoluyla eklenmiş URL'sidir */.auth/login/google/callback*. Örneğin, `https://contoso.azurewebsites.net/.auth/login/google/callback`. HTTPS şeması kullandığınızdan emin olun. Sonra **Oluştur**’a tıklayın.
9. Sonraki ekranda, istemci Kimliğini ve istemci gizli dizisi değerlerini not edin.

    > [!IMPORTANT]
    > Gizli bir önemli güvenlik kimlik bilgisidir. Bu gizli dizi kimseyle paylaşmayın değil veya bir istemci uygulaması içinde dağıtın.


## <a name="secrets"> </a>Google bilgilerini uygulamanıza ekleme
1. Geri [Azure portal], uygulamanıza gidin. Tıklayın **ayarları**, ardından **kimlik doğrulama / yetkilendirme**.
2. Kimlik doğrulama / yetkilendirme özelliği etkin değilse, geçiş açtığınızdan **üzerinde**.
3. Tıklayın **Google**. Daha önce aldığınız uygulama kimliği ve uygulama gizli anahtarı değerleri yapıştırın ve isteğe bağlı olarak uygulamanızın gerektirdiği herhangi bir kapsam etkinleştirin. Daha sonra, **Tamam**'a tıklayın.
   
   ![][1]
   
   Varsayılan olarak, App Service kimlik doğrulaması sağlar, ancak site içerik ve API'ler için yetkili erişimi kısıtlamaz. Kullanıcılar, uygulama kodunuzda yetkilendirmeniz gerekir.
4. (İsteğe bağlı) Sitenizi yalnızca Google tarafından kimliği doğrulanmış kullanıcılar için erişimi kısıtlamak için ayarlanmış **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem** için **Google**. Bu, tüm istekleri kimliğinin doğrulanmasını gerektirir ve kimliği doğrulanmamış tüm istekleri için Google kimlik doğrulaması için yönlendirilirsiniz.
5. **Kaydet**’e tıklayın.

Şimdi uygulamanıza kimlik doğrulaması için Google'ı kullanmaya hazırsınız.

## <a name="related-content"> </a>İlgili içerik
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: https://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure portal]: https://portal.azure.com/

