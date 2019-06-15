---
title: Twitter kimlik doğrulama - Azure App Service'ı yapılandırma
description: Twitter kimlik doğrulaması App Service uygulamanız için yapılandırmayı öğrenin.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: syntaxc4
editor: ''
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/19/2018
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: 51a2ac93fd2d863855c820ba147418c5397c2a89
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60851552"
---
# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a>App Service uygulamanızı twitter oturum açma bilgilerini kullanacak şekilde yapılandırma
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu konuda, Azure App Service'ı bir kimlik doğrulama sağlayıcısı olarak Twitter'ı kullanmak için yapılandırma gösterilmektedir.

Bu konudaki yordamı tamamlamak için doğrulanmış e-posta adresi ve telefon numarası olan bir Twitter hesabı olmalıdır. Yeni bir Twitter hesabı oluşturmak için Git <a href="https://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"> </a>Twitter ile uygulamanızı kaydetme
1. Oturum [Azure portal]ve uygulamanıza gidin. Kopyalama, **URL**. Twitter Uygulamanızı yapılandırmak için kullanır.
2. Gidin [Twitter geliştiriciler] Web sitesi, Twitter hesabı kimlik bilgilerinizle oturum açın ve tıklayın **yeni uygulama oluştur**.
3. Yazın **adı** ve **açıklama** yeni uygulamanız için. Yapıştırma seçeneğiyle, uygulamanızın **URL** için **Web sitesi** değeri. Sonra **geri çağırma URL'si**, Yapıştır **geri çağırma URL'si** daha önce kopyaladığınız. Bu, mobil uygulama ağ geçidi yoluyla eklenmiş olan */.auth/login/twitter/callback*. Örneğin, `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. HTTPS şeması kullandığınızdan emin olun.
4. En altta sayfanın okuyun ve koşulları kabul edin. Ardından **kendi Twitter uygulamanızı oluşturun**. Bu uygulama görüntüler uygulama ayrıntıları kaydeder.
5. Tıklayın **ayarları** sekmesinde, onay **Twitter ile oturum aç imzalamak için bu uygulamanın izin**, ardından **güncelleştirme ayarları**.
6. Seçin **anahtarlar ve erişim belirteçleri** sekmesi. Değerlerini not edin **tüketici anahtarı (API anahtarı)** ve **tüketici gizli anahtarı (API gizli anahtarı)** .
   
   > [!NOTE]
   > Tüketici gizli bir önemli güvenlik kimlik bilgisidir. Değil Bu gizli dizi kimseyle paylaşmayın veya uygulamanızla birlikte dağıtmayın.
   > 
   > 

## <a name="secrets"> </a>Twitter bilgilerini uygulamanıza ekleme
1. Geri [Azure portal], uygulamanıza gidin. Tıklayın **ayarları**, ardından **kimlik doğrulama / yetkilendirme**.
2. Kimlik doğrulama / yetkilendirme özelliği etkin değilse, geçiş açtığınızdan **üzerinde**.
3. Tıklayın **Twitter**. Daha önce edindiğiniz değerleri yapıştırın, uygulama kimliği ve uygulama gizli anahtarı. Daha sonra, **Tamam**'a tıklayın.
   
   ![][1]
   
   Varsayılan olarak, App Service kimlik doğrulaması sağlar, ancak site içerik ve API'ler için yetkili erişimi kısıtlamaz. Kullanıcılar, uygulama kodunuzda yetkilendirmeniz gerekir.
4. (İsteğe bağlı) Sitenizi yalnızca Twitter tarafından kimliği doğrulanmış kullanıcılar için erişimi kısıtlamak için ayarlanmış **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem** için **Twitter**. Bu, tüm istekleri kimliğinin doğrulanmasını gerektirir ve kimliği doğrulanmamış tüm istekleri için Twitter kimlik doğrulaması için yönlendirilirsiniz.
5. **Kaydet**’e tıklayın.

Şimdi uygulamanıza kimlik doğrulaması için Twitter'ı kullanmaya hazırsınız.

## <a name="related-content"> </a>İlgili içerik
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter geliştiriciler]: https://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
