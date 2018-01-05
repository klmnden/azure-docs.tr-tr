---
title: "Kimlik doğrulama ve yetkilendirme Azure mobil uygulamalar | Microsoft Docs"
description: "Kavramsal başvurusu ve kimlik doğrulamasına genel bakış / yetkilendirme özelliğini Azure Mobile Apps için"
services: app-service\mobile
documentationcenter: 
author: mattchenderson
manager: cfowler
editor: 
ms.assetid: a46dbf70-867d-48f6-8885-7f5207ad102e
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 90c11b09351f019c45f5f1b025d67947b69b3b0a
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2018
---
# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>Azure Mobile Apps’te Kimlik Doğrulama ve Yetkilendirme
## <a name="what-is-app-service-authentication--authorization"></a>App Service kimlik doğrulaması nedir / yetkilendirme?
> [!NOTE]
> Bu konu geçirilen bir birleştirilmiş için [App Service kimlik doğrulama / yetkilendirme](../app-service/app-service-authentication-overview.md) Web, mobil ve API Apps kapsayan konu.
> 
> 

App Service kimlik doğrulama / yetkilendirme, uygulamanızın uygulama arka uçta gerekli kod değişikliğine kullanıcılar oturum sağlayan bir özelliktir. Uygulamanızı korumak ve kullanıcı başına verilerle çalışmak için kolay bir yol sağlar.

Uygulama hizmeti, federe kimlik kullanan bir 3. taraf **kimlik sağlayıcısı** ("IDP") hesaplarını depolayan ve kullanıcıların kimliğini doğrular ve uygulama bu kimliği kendi yerine kullanır. Uygulama hizmeti kutunun dışında beş kimlik sağlayıcılarını destekler: *Azure Active Directory*, *Facebook*, *Google*, *Microsoft Account*, ve *Twitter*. Bu destek, uygulamalarınız için başka bir kimlik sağlayıcısı veya kendi özel kimlik çözümü ile tümleştirerek genişletebilirsiniz.

Son kullanıcılarınızın nasıl oturum seçeneklerini sağlayabilmesi için uygulamanız bu kimlik sağlayıcıları herhangi bir sayıda yararlanabilirsiniz.

Hemen kullanmaya başlamak istiyorsanız, lütfen şu eğitimlerden birine bakın:

* [İOS uygulamanıza kimlik doğrulaması ekleme]
* [Xamarin.iOS uygulamanıza kimlik doğrulaması ekleme]
* [Xamarin.Android uygulamanıza kimlik doğrulaması ekleme]
* [Windows kimlik doğrulaması ekleme]

## <a name="how-authentication-works"></a>Kimlik doğrulaması nasıl çalışır
Kimlik sağlayıcılar birini kullanarak kimlik doğrulamak için önce uygulamanız hakkında bilmeniz kimlik sağlayıcısı yapılandırmanız gerekir. Ardından kimlik sağlayıcısı kimlikleri ve uygulama geri sağladığınız gizli ile sağlayacaktır. Bu güven ilişkisi tamamlandıktan ve sağlanan kimliklerini doğrulamak uygulama hizmeti sağlar.

Bu adımlar aşağıdaki konularda ayrıntılı olarak açıklanmaktadır:

* [Azure Active Directory oturum açma kullanmak için uygulamanızı yapılandırma]
* [Facebook oturum açma kullanmak için uygulamanızı yapılandırma]
* [Google oturum açma kullanmak için uygulamanızı yapılandırma]
* [Microsoft Account oturum açma kullanmak için uygulamanızı yapılandırma]
* [Twitter oturum açma kullanmak için uygulamanızı yapılandırma]

Her şeyi arka uçta yapılandırıldıktan sonra oturum açmak için istemci değiştirebilirsiniz. Burada iki yaklaşım vardır:

* Tek satırlık bir kod kullanarak, Mobile Apps istemci SDK kullanıcılar oturum sağlar.
* Kimliğini oluşturmak ve uygulama hizmeti erişim kazanmak için belirtilen kimlik sağlayıcısı tarafından yayımlanan SDK yararlanın.

> [!TIP]
> Çoğu uygulama yerel-duyguyu'açığa fazla oturum açma deneyimi almak ve yenileme desteği ve diğer sağlayıcıya özgü avantajlarından yararlanmak için bir sağlayıcı SDK kullanmanız gerekir.
> 
> 

### <a name="how-authentication-without-a-provider-sdk-works"></a>SDK sağlayıcı olmadan kimlik doğrulaması nasıl çalışır
Sağlayıcısını kurma SDK istemiyorsanız, sizin için oturum açma gerçekleştirmek Mobile Apps izin verebilirsiniz. Mobile Apps istemci SDK Sağlayıcısı'nda seçtiğiniz web görünümü açın ve oturum açma tamamlayın. Bazen üzerinde bloglar ve forumlar bu başvuruda "sunucu akış" veya "sunucu yönlendirilmiş akış" olarak Server'dan bu yana görürsünüz oturum açma yönetme ve istemci SDK sağlayıcı belirteci hiçbir zaman alır.

Bu akış başlatmak için gereken kodu her platform için kimlik doğrulama öğreticideki ele alınmıştır. Akış sonunda SDK istemcisi bir uygulama hizmeti belirtecine sahip ve belirteç arka uç için tüm istekleri için otomatik olarak eklenir.

### <a name="how-authentication-with-a-provider-sdk-works"></a>Kimlik doğrulama sağlayıcısı SDK ile nasıl çalışır
Sağlayıcı ile çalışma SDK'sı daha sıkı bir şekilde uygulamayı çalıştıran işletim sistemi platformu ile etkileşim kurmak oturum açma deneyimi sağlar. Bu ayrıca, bir sağlayıcı belirteci ve grafik API'leri kullanabilir ve kullanıcı deneyimini özelleştirmek çok daha kolay hale getirir istemcide bazı kullanıcı bilgileri sunar. Bazen bloglar ve forumlar bu "istemci akışı" olarak başvurulan veya oturum açma "istemci yönlendirilmiş akış" kod itibaren istemcide işleme ve istemci kodu sağlayıcı belirteci erişimi görürsünüz.

Bir sağlayıcı belirteç alındığında, doğrulama için App Service'e gönderilmesi gerekir. Akış sonunda SDK istemcisi bir uygulama hizmeti belirtecine sahip ve belirteç arka uç için tüm istekleri için otomatik olarak eklenir. Bu nedenle seçerseniz Geliştirici da sağlayıcı belirteci başvuru tutabilirsiniz.

## <a name="how-authorization-works"></a>Yetkilendirme nasıl çalışır?
App Service kimlik doğrulama / yetkilendirme için birkaç seçenek sunar **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem**. Belirtilen bir isteğin kodunuzu almadan önce isteğin kimliği olmadığını görmek için uygulama hizmeti onay ve değil, reddetmek ve yeniden denemeden önce oturum kullanıcı girişimi olabilir.

Kimlik sağlayıcılardan biri istekleri yeniden yönlendirme, kimliği doğrulanmamış bir seçenektir. Bir web tarayıcısında bu yeni bir sayfa kullanıcıya gerçekten kalırsınız. Ancak, mobil istemciniz bu şekilde yönlendirilemez ve kimliği doğrulanmamış yanıtları, bir HTTP alacağı *401 Yetkisiz* yanıt. Bu verildiğinde, istemci yapar ilk istek her zaman için oturum açma uç noktası olmalıdır ve daha sonra başka bir API çağrıları hale getirebilirsiniz. Oturum açmayı önce başka bir API çağrısı çalışırsanız, istemci bir hata alırsınız.

Daha ayrıntılı isterseniz, hangi uç noktaları kimlik doğrulaması gerektiren denetlemek, aynı zamanda çekme "hiçbir eylem (isteğine izin ver)" kimliği doğrulanmamış istekler için. Bu durumda, tüm kimlik doğrulama kararları uygulama kodunuz ertelenmiş olan. Bu ayrıca, özel yetkilendirme kurallarına göre belirli kullanıcıların erişmesine izin vermek sağlar.

## <a name="documentation"></a>Belgeler
Aşağıdaki öğreticiler App hizmetini kullanarak mobil istemcilerinizin kimlik doğrulaması ekleme gösterir:

* [İOS uygulamanıza kimlik doğrulaması ekleme]
* [Xamarin.iOS uygulamanıza kimlik doğrulaması ekleme]
* [Xamarin.Android uygulamanıza kimlik doğrulaması ekleme]
* [Windows kimlik doğrulaması ekleme]

Aşağıdaki öğreticiler App farklı kimlik doğrulama sağlayıcıları yararlanacağınızı hizmetinin nasıl yapılandırılacağı gösterilmektedir:

* [Azure Active Directory oturum açma kullanmak için uygulamanızı yapılandırma]
* [Facebook oturum açma kullanmak için uygulamanızı yapılandırma]
* [Google oturum açma kullanmak için uygulamanızı yapılandırma]
* [Microsoft Account oturum açma kullanmak için uygulamanızı yapılandırma]
* [Twitter oturum açma kullanmak için uygulamanızı yapılandırma]

Burada, ayrıca yararlanabileceğiniz sağlanan kimlik sistemi dışındaki kullanmak istiyorsanız, [Önizleme .NET sunucusu SDK özel kimlik doğrulama desteği](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth).

[İOS uygulamanıza kimlik doğrulaması ekleme]: app-service-mobile-ios-get-started-users.md
[Xamarin.iOS uygulamanıza kimlik doğrulaması ekleme]: app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android uygulamanıza kimlik doğrulaması ekleme]: app-service-mobile-xamarin-android-get-started-users.md
[Windows kimlik doğrulaması ekleme]: app-service-mobile-windows-store-dotnet-get-started-users.md

[Azure Active Directory oturum açma kullanmak için uygulamanızı yapılandırma]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook oturum açma kullanmak için uygulamanızı yapılandırma]: ../app-service/app-service-mobile-how-to-configure-facebook-authentication.md
[Google oturum açma kullanmak için uygulamanızı yapılandırma]: ../app-service/app-service-mobile-how-to-configure-google-authentication.md
[Microsoft Account oturum açma kullanmak için uygulamanızı yapılandırma]: ../app-service/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter oturum açma kullanmak için uygulamanızı yapılandırma]: ../app-service/app-service-mobile-how-to-configure-twitter-authentication.md
