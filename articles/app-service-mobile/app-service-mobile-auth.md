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
ms.openlocfilehash: 57deec4acffc1387fcb5c42c17085bb363dfdbc1
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>Azure Mobile Apps’te Kimlik Doğrulama ve Yetkilendirme
## <a name="what-is-app-service-authentication--authorization"></a>App Service kimlik doğrulaması nedir / yetkilendirme?
> [!NOTE]
> Bu makalede geçirilecek bir birleştirilmiş için [App Service kimlik doğrulama / yetkilendirme](../app-service/app-service-authentication-overview.md) Web, mobil ve API Apps kapsayan makalesi.
> 
> 

App Service kimlik doğrulama / yetkilendirme, uygulamanızın uygulama arka uçta gerekli kod değişikliğine kullanıcılar oturum sağlayan bir özelliktir. Uygulamanızı korumak ve kullanıcı başına verilerle çalışmak için kolay bir yol sağlar.

Uygulama hizmeti, federe kimlik kullanan bir 3. taraf **kimlik sağlayıcısı** ("IDP") hesaplarını depolayan ve kullanıcıların kimliğini doğrular. Uygulama bu kimliği kendi yerine kullanır. Uygulama hizmeti kutunun dışında beş kimlik sağlayıcılarını destekler: *Azure Active Directory*, *Facebook*, *Google*, *Microsoft Account*, ve *Twitter*. Bu destek, uygulamalarınız için başka bir kimlik sağlayıcısı veya kendi özel kimlik çözümü ile tümleştirerek genişletebilirsiniz.

Son kullanıcılarınızın nasıl oturum seçeneklerini sağlayabilmesi için uygulamanız bu kimlik sağlayıcıları herhangi bir sayıda kullanabilirsiniz.

Hemen kullanmaya başlamak istiyorsanız, şu eğitimlerden birine bakın:

* [İOS uygulamanıza kimlik doğrulaması ekleme]
* [Xamarin.iOS uygulamanıza kimlik doğrulaması ekleme]
* [Xamarin.Android uygulamanıza kimlik doğrulaması ekleme]
* [Windows kimlik doğrulaması ekleme]

## <a name="how-authentication-works"></a>Kimlik doğrulaması nasıl çalışır
Kimlik sağlayıcılar birini kullanarak kimlik doğrulamak için önce uygulamanız hakkında bilmeniz kimlik sağlayıcısı yapılandırmanız gerekir. Ardından kimlik sağlayıcısı kimlikleri ve uygulama geri sağladığınız gizli ile sağlayacaktır. Bu güven ilişkisi tamamlandıktan ve sağlanan kimliklerini doğrulamak uygulama hizmeti sağlar.

Bu adımlar aşağıdaki konularda ayrıntılı olarak açıklanmaktadır:

* [Uygulamanızı Azure Active Directory oturum açma bilgilerini kullanacak şekilde yapılandırma]
* [Uygulamanızı Facebook oturum açma bilgilerini kullanacak şekilde yapılandırma]
* [Uygulamanızı Google oturum açma bilgilerini kullanacak şekilde yapılandırma]
* [Uygulamanızı Microsoft Hesabı oturum açma bilgilerini kullanacak şekilde yapılandırma]
* [Uygulamanızı Twitter oturum açma bilgilerini kullanacak şekilde yapılandırma]

Her şeyi arka uçta yapılandırıldıktan sonra oturum açmak için istemci değiştirebilirsiniz. Burada iki yaklaşım vardır:

* Tek satırlık bir kod kullanarak, Mobile Apps istemci SDK kullanıcılar oturum sağlar.
* Belirtilen kimlik sağlayıcısı tarafından yayımlanan SDK kimliğini oluşturmak ve uygulama hizmeti erişim kazanmak için kullanın.

> [!TIP]
> Çoğu uygulama yerel-duyguyu'açığa fazla oturum açma deneyimi almak ve yenileme desteği ve diğer sağlayıcıya özgü avantajlarından yararlanmak için bir sağlayıcı SDK kullanmanız gerekir.
> 
> 

### <a name="how-authentication-without-a-provider-sdk-works"></a>SDK sağlayıcı olmadan kimlik doğrulaması nasıl çalışır
Sağlayıcısını kurma SDK istemiyorsanız, sizin için oturum açma gerçekleştirmek Mobile Apps izin verebilirsiniz. Mobile Apps istemci SDK Sağlayıcısı'nda oturum açma işlemini tamamlayabilmek seçtiğiniz bir web görünümü açılır. Bazen, "sunucu akış" veya "sunucu yönlendirilmiş akış" olarak adlandırılan bu iş akışı görebilirsiniz sunucunun oturum açma yönetme ve istemci SDK sağlayıcısı belirtecin hiçbir zaman alır.

Bu akış başlatmak için gereken kodu her platform için kimlik doğrulama öğreticideki ele alınmıştır. Akış sonunda SDK istemcisi bir uygulama hizmeti belirtecine sahip ve belirteç arka uç için tüm istekleri için otomatik olarak eklenir.

### <a name="how-authentication-with-a-provider-sdk-works"></a>Kimlik doğrulama sağlayıcısı SDK ile nasıl çalışır
Sağlayıcı ile çalışma SDK'sı daha sıkı bir şekilde uygulamayı çalıştıran işletim sistemi platformu ile etkileşim kurmak oturum açma deneyimi sağlar. SDK sağlayıcısı ayrıca sağlayıcı belirteci ve grafik API'leri kullanabilir ve kullanıcı deneyimini özelleştirmek çok daha kolay istemcisinde, bazı kullanıcı bilgileri sağlar. Bazen, için "istemci akışı" olarak adlandırılan bu iş akışı görebilirsiniz ya da "istemci yönlendirilmiş akış" kod itibaren istemcide oturum açma işleme ve istemci kodu sağlayıcı belirteci erişimi.

Bir sağlayıcı belirteç alındığında, doğrulama için App Service'e gönderilmesi gerekir. Akış sonunda SDK istemcisi bir uygulama hizmeti belirtecine sahip ve belirteç arka uç için tüm istekleri için otomatik olarak eklenir. Bu nedenle seçerseniz Geliştirici da sağlayıcı belirteci başvuru tutabilirsiniz.

## <a name="how-authorization-works"></a>Yetkilendirme nasıl çalışır?
App Service kimlik doğrulama / yetkilendirme için birkaç seçenek sunar **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem**. Belirtilen bir isteğin kodunuzu almadan önce isteğin kimliği olmadığını görmek için uygulama hizmeti onay ve değil, reddetmek ve yeniden denemeden önce oturum kullanıcı girişimi olabilir.

Kimlik sağlayıcılardan biri istekleri yeniden yönlendirme, kimliği doğrulanmamış bir seçenektir. Bir web tarayıcısında bu yeniden yönlendirme gerçekte kullanıcı yeni bir sayfaya alır. Ancak, mobil istemciniz bu şekilde yönlendirilemez ve kimliği doğrulanmamış yanıtları, bir HTTP alacağı *401 Yetkisiz* yanıt. Bu verildiğinde, istemci yapar ilk istek her zaman için oturum açma uç noktası olmalıdır ve daha sonra başka bir API çağrıları hale getirebilirsiniz. Oturum açmayı önce başka bir API çağrısı çalışırsanız, istemci bir hata alırsınız.

Daha ayrıntılı isterseniz, hangi uç noktaları kimlik doğrulaması gerektiren denetlemek, aynı zamanda çekme "hiçbir eylem (isteğine izin ver)" kimliği doğrulanmamış istekler için. Bu durumda, tüm kimlik doğrulama kararları uygulama kodunuz ertelenmiş olan. Bu ayrıca, özel yetkilendirme kurallarına göre belirli kullanıcıların erişmesine izin vermek sağlar.

## <a name="documentation"></a>Belgeler
Aşağıdaki öğreticiler App hizmetini kullanarak mobil istemcilerinizin kimlik doğrulaması ekleme gösterir:

* [İOS uygulamanıza kimlik doğrulaması ekleme]
* [Xamarin.iOS uygulamanıza kimlik doğrulaması ekleme]
* [Xamarin.Android uygulamanıza kimlik doğrulaması ekleme]
* [Windows kimlik doğrulaması ekleme]

Aşağıdaki öğreticiler App farklı kimlik doğrulama sağlayıcılarını kullanacak şekilde hizmetinin nasıl yapılandırılacağı gösterilmektedir:

* [Uygulamanızı Azure Active Directory oturum açma bilgilerini kullanacak şekilde yapılandırma]
* [Uygulamanızı Facebook oturum açma bilgilerini kullanacak şekilde yapılandırma]
* [Uygulamanızı Google oturum açma bilgilerini kullanacak şekilde yapılandırma]
* [Uygulamanızı Microsoft Hesabı oturum açma bilgilerini kullanacak şekilde yapılandırma]
* [Uygulamanızı Twitter oturum açma bilgilerini kullanacak şekilde yapılandırma]

Burada da kullanabilirsiniz sağlanan kimlik sistemi dışındaki kullanmak istiyorsanız, [Önizleme .NET sunucusu SDK özel kimlik doğrulama desteği](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth).

[İOS uygulamanıza kimlik doğrulaması ekleme]: app-service-mobile-ios-get-started-users.md
[Xamarin.iOS uygulamanıza kimlik doğrulaması ekleme]: app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android uygulamanıza kimlik doğrulaması ekleme]: app-service-mobile-xamarin-android-get-started-users.md
[Windows kimlik doğrulaması ekleme]: app-service-mobile-windows-store-dotnet-get-started-users.md

[Uygulamanızı Azure Active Directory oturum açma bilgilerini kullanacak şekilde yapılandırma]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md
[Uygulamanızı Facebook oturum açma bilgilerini kullanacak şekilde yapılandırma]: ../app-service/app-service-mobile-how-to-configure-facebook-authentication.md
[Uygulamanızı Google oturum açma bilgilerini kullanacak şekilde yapılandırma]: ../app-service/app-service-mobile-how-to-configure-google-authentication.md
[Uygulamanızı Microsoft Hesabı oturum açma bilgilerini kullanacak şekilde yapılandırma]: ../app-service/app-service-mobile-how-to-configure-microsoft-authentication.md
[Uygulamanızı Twitter oturum açma bilgilerini kullanacak şekilde yapılandırma]: ../app-service/app-service-mobile-how-to-configure-twitter-authentication.md
