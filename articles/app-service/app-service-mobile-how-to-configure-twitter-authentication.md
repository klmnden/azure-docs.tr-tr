---
title: Uygulama Hizmetleri uygulamanız için twitter kimlik doğrulamasını yapılandırma
description: Uygulama Hizmetleri uygulamanız için twitter kimlik doğrulaması yapılandırma konusunda bilgi edinin.
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
ms.openlocfilehash: f6449f99fda9c1a612ed9f9134751ff76b25904c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a>Uygulama hizmeti uygulamanızı twitter oturum açma kullanacak şekilde yapılandırma
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu konu Azure App Service Twitter kimlik doğrulama sağlayıcısı olarak kullanmak üzere yapılandırmak nasıl gösterir.

Bu konudaki yordamı tamamlamak için doğrulanmış e-posta adresi ve telefon numarası olan bir Twitter hesabı olması gerekir. Yeni bir Twitter hesabı oluşturmak için şu adrese gidin <a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"> </a>Twitter ile uygulamanızı kaydetme
1. Oturum [Azure portal]ve uygulamanıza gidin. Kopyalama, **URL**. Twitter Uygulamanızı yapılandırmak için bunu kullanır.
2. Gidin [Twitter geliştiriciler] Web sitesi, Twitter hesabı kimlik bilgilerinizle oturum açın ve tıklatın **yeni uygulama oluşturma**.
3. Yazın **adı** ve **açıklama** yeni uygulamanız için. Uygulamanızın Yapıştır **URL** için **Web sitesi** değeri. Sonra **geri çağırma URL'si**, yapıştırma **geri çağırma URL'si** daha önce kopyaladığınız. Bu, mobil uygulama ağ geçidi yoluyla eklenmiş olan */.auth/login/twitter/callback*. Örneğin, `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. HTTPS şeması kullandığınızdan emin olun.
4. Alttaki sayfa koşullarını okuyup kabul edin. Ardından **Twitter uygulamanızı oluşturma**. Bu uygulama görüntüler uygulama ayrıntıları kaydeder.
5. ' I tıklatın **ayarları** sekmesi, onay **Twitter ile oturum aç imzalamak için kullanılan bu uygulamanın izin**, ardından **güncelleştirme ayarlarını**.
6. Seçin **anahtarları ve erişim belirteçleri** sekmesi. Değerlerini Not **tüketici anahtarı (API anahtarı)** ve **tüketici gizli anahtarı (API gizli)**.
   
   > [!NOTE]
   > Tüketici gizli bir önemli güvenlik kimlik bilgisidir. Bu gizli kimseyle paylaşmayın değil veya uygulamanızla dağıtabilirsiniz.
   > 
   > 

## <a name="secrets"> </a>Twitter bilgi uygulamanıza ekleyin
1. Geri [Azure portal], uygulamanıza gidin. Tıklatın **ayarları**ve ardından **kimlik doğrulama / yetkilendirme**.
2. Kimlik doğrulama / yetkilendirme özelliği etkin değil, anahtar etkinleştirmek **üzerinde**.
3. Tıklatın **Twitter**. Uygulama kimliği ve uygulama gizli anahtarı daha önce edindiğiniz değerleri yapıştırın. Daha sonra, **Tamam**'a tıklayın.
   
   ![][1]
   
   Varsayılan olarak, App Service kimlik doğrulaması sağlar ancak yetkili erişimi üzere site içeriğini ve API'lerini kısıtlamaz. Kullanıcılar, uygulama kodunuzda yetkilendirmeniz gerekir.
4. (İsteğe bağlı) Sitenize yalnızca Twitter tarafından kimliği doğrulanmış kullanıcılar için erişimi kısıtlamak üzere koyulan **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem** için **Twitter**. Bu, tüm istekleri doğrulanmasını gerektirir ve tüm kimliği doğrulanmamış istekler için Twitter kimlik doğrulaması için yönlendirilir.
5. **Kaydet**’e tıklayın.

Şimdi uygulamanıza kimlik doğrulaması için Twitter kullanmak hazırsınız.

## <a name="related-content"> </a>İlgili içerik
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter geliştiriciler]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure portal]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
