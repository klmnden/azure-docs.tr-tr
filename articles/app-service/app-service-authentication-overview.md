---
title: "Kimlik doğrulama ve yetkilendirme Azure App Service'te | Microsoft Docs"
description: "Kavramsal başvurusu ve kimlik doğrulamasına genel bakış / yetkilendirme özelliğini Azure uygulama hizmeti"
services: app-service
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: b7151b57-09e5-4c77-a10c-375a262f17e5
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/29/2016
ms.author: mahender
ms.openlocfilehash: f0d2644903181cd2e20166feae4f90ddd4037fa8
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="authentication-and-authorization-in-azure-app-service"></a>Azure Uygulama Hizmeti’nde kimlik doğrulaması ve yetkilendirme
## <a name="what-is-app-service-authentication--authorization"></a>App Service kimlik doğrulaması nedir / yetkilendirme?
App Service kimlik doğrulama / yetkilendirme, uygulama arka uç kodunu değiştirmek zorunda değilsiniz böylece kullanıcılar oturum uygulamanız için bir yol sağlayan bir özelliktir. Uygulamanızı korumak ve kullanıcı başına verilerle çalışmak için kolay bir yol sağlar.

Uygulama hizmeti bir üçüncü taraf kimlik sağlayıcısı hesapları depolar ve kullanıcıların kimliğini doğrulayan Federal Kimlik kullanır. Böylece bu bilgileri depolamak uygulamanın yok uygulama üzerinde sağlayıcıya ait kimlik bilgilerini kullanır. Uygulama hizmeti kutunun dışında beş kimlik sağlayıcılarını destekler: Azure Active Directory, Facebook, Google, Microsoft Account ve Twitter. Uygulamanız bu kimlik sağlayıcıları herhangi bir sayıda kullanıcılarınız oturum nasıl seçeneklerini sağlamak için kullanabilirsiniz. Yerleşik destek genişletmek için başka bir kimlik sağlayıcısı tümleştirebilir veya [kendi özel kimlik çözümü][custom-auth].

Hemen kullanmaya başlamak istiyorsanız, şu eğitimlerden birine bakın [iOS uygulamanıza kimlik doğrulaması ekleme] [ iOS] (veya [Android], [Windows], [Xamarin.iOS], [Xamarin.Android], [Xamarin.Forms], veya [Cordova]).

## <a name="how-authentication-works-in-app-service"></a>Kimlik doğrulaması App Service içinde nasıl çalışır
Kimlik sağlayıcılar birini kullanarak kimlik doğrulamak için önce uygulamanız hakkında bilmeniz kimlik sağlayıcısı yapılandırmanız gerekir. Kimlik sağlayıcısı sonra kimlikleri ve uygulama hizmeti sağlamak gizli sağlar. App Service kimlik sağlayıcısından gelen kimlik doğrulama belirteçleri gibi kullanıcı onaylar doğrulayabilmesi bu güven ilişkisi tamamlar.

Bu sağlayıcılar birini kullanarak bir kullanıcıyla oturum açmak için kullanıcının kullanıcılar oturum açtığında bu sağlayıcı için bir uç nokta için yeniden yönlendirilmesi gerekiyor. Müşterilerin bir web tarayıcısı kullanıyorsanız, uygulama hizmeti otomatik olarak kullanıcılar oturum açtığında uç nokta için tüm kimliği doğrulanmamış kullanıcıların doğrudan olabilir. Aksi takdirde, müşterilerinize doğrudan gerekecek `{your App Service base URL}/.auth/login/<provider>`, burada `<provider>` aşağıdaki değerlerden biri: aad, facebook, google, microsoft ya da twitter. Mobil ve API senaryoları bölümlerde bu makalenin sonraki bölümlerinde açıklanmıştır.

Uygulamanızı bir web tarayıcısı aracılığıyla etkileşim kullanıcılar ayarlayabilir, böylece uygulamanız gezinirken bunlar kimliği doğrulanmış kalabileceği bir tanımlama bilgisi gerekir. Mobile, JSON web token (JWT) gibi diğer istemci türlerini için hangi sunulması gerektiğini de `X-ZUMO-AUTH` üstbilgisi, istemciye verilir. Mobile Apps istemci SDK'ları bu sizin için işler. Alternatif olarak, bir Azure Active Directory kimlik belirtecinin ya da erişim belirteci doğrudan eklenebilir `Authorization` üstbilgisi olarak bir [taşıyıcı belirteci](https://tools.ietf.org/html/rfc6750).

Uygulama hizmeti herhangi bir tanımlama bilgisi veya kullanıcıların kimliklerini doğrulamak için uygulamanızın sorunları belirteci doğrular. Uygulamanızı erişebilecek kişileri kısıtlamak için bkz: [yetkilendirme](#authorization) bu makalenin sonraki bölümlerinde bölümü.

### <a name="mobile-authentication-with-a-provider-sdk"></a>Mobil kimlik doğrulama sağlayıcısı SDK ile
Her şeyi arka uçta yapılandırıldıktan sonra uygulama hizmeti ile imzalamak için mobil istemcilerin değiştirebilirsiniz. Burada iki yaklaşım vardır:

* Belirtilen kimlik sağlayıcısı kimliğini oluşturmak ve uygulama hizmeti erişim kazanmak için yayımlayan bir SDK'yı kullanın.
* Tek satırlık bir kod kullanabilir, böylece Mobile Apps istemci SDK'sı kullanıcılar oturum açabilir.

> [!TIP]
> Çoğu uygulamayı yenileme desteği kullanmak üzere ve sağlayıcıyı belirtir diğer avantajlarından yararlanabilmek için kullanıcıların oturum açtığınızda daha tutarlı bir deneyim almak için bir sağlayıcı SDK kullanmanız gerekir.
> 
> 

SDK sağlayıcı kullandığınızda, kullanıcılar uygulamayı çalıştıran işletim sistemine sahip daha sıkı bir şekilde tümleşen bir deneyim için oturum açabilir. Bu ayrıca, bir sağlayıcı belirteci ve grafik API'leri kullanabilir ve kullanıcı deneyimini özelleştirmek çok daha kolay hale getirir istemcide bazı kullanıcı bilgileri sunar. Bazen bloglar ve forumlar, bu "istemci akışı" olarak başvurulan veya istemci kodu kullanıcıları ve istemci kodu'nda oturum açtığında olduğundan "istemci yönlendirilmiş akış" Sağlayıcı belirteci erişimi görürsünüz.

Sağlayıcı belirteci alındıktan sonra uygulama hizmeti için doğrulama gönderilmesi gerekiyor. Uygulama hizmeti belirteci doğruladıktan sonra uygulama hizmeti istemciye döndürülen yeni bir uygulama hizmeti belirteci oluşturur. Mobile Apps istemci SDK'sı, bu exchange yönetmek ve otomatik olarak uygulama arka uç yapılan tüm isteklere belirteci eklemek için yardımcı yöntemler vardır. Bu nedenle seçerseniz geliştiriciler ayrıca sağlayıcı belirteci başvuru kullanmaya devam edebilir.

### <a name="mobile-authentication-without-a-provider-sdk"></a>SDK sağlayıcı olmadan Mobil kimlik doğrulaması
Sağlayıcısını kurma SDK istemiyorsanız, oturum açmak için Azure App Service Mobile Apps özelliğini izin verebilirsiniz. Mobile Apps istemci SDK seçtiğiniz sağlayıcı web görünümüne açın ve kullanıcı oturum açabilir. Bazen bloglar ve forumlar, sunucusu kullanıcılar oturum açtığında sürecini yönetir çünkü bu için "sunucu akış" veya "sunucu yönlendirilmiş akış" olarak adlandırılır ve istemci SDK sağlayıcısı belirtecin hiçbir zaman alan görürsünüz.

Bu akış başlatmak için kod her platform için kimlik doğrulama öğreticideki dahil edilir. Akış sonunda SDK istemcisi bir uygulama hizmeti belirtecine sahip ve belirteç uygulama arka ucu için tüm istekleri için otomatik olarak eklenir.

### <a name="service-to-service-authentication"></a>Hizmetten hizmete kimlik doğrulaması
Kullanıcılar uygulamanıza erişim verebilirsiniz rağmen kendi API çağrısı için başka bir uygulama da güvenebilirsiniz. Örneğin, başka bir web uygulamasında bir API çağrısı bir web uygulaması olabilir. Bu senaryoda, kimlik bilgilerini bir hizmet hesabı kullanıcı kimlik bilgilerini yerine bir belirteç almak üzere kullanın. Bir hizmet hesabı olarak da bilinen olan bir *hizmet sorumlusu* Azure Active Directory dilinde ve kullandığı kimlik doğrulama bu tür bir hesabınız olarak da bilinen bir hizmet senaryodur.

> [!IMPORTANT]
> Mobil uygulamaları müşteri cihazlarda çalıştığından, mobil uygulamalar yapmak *değil* sayısı güvenilir uygulamalar ve hizmet asıl akış kullanmamanız gerekir. Bunun yerine, önceki ayrıntılı bir kullanıcı akışı kullanmanız gerekir.
> 
> 

Hizmetten hizmete senaryoları için uygulama hizmeti uygulamanızı Azure Active Directory kullanarak koruyabilirsiniz. Yalnızca çağrı yapan uygulamanın istemci Kimliğini ve istemci Azure Active Directory'den gizli sağlayarak elde bir Azure Active Directory hizmet asıl yetkilendirme belirteci sağlaması gerekir. [API uygulamaları için hizmet asıl kimlik] Öğreticisi tarafından ASP.NET API uygulamaları kullanan bu senaryo örneği açıklandığı [apia-hizmet].

Bir hizmet senaryo işlemek için uygulama hizmeti kimlik doğrulama kullanmak istiyorsanız, istemci sertifikalarını veya temel kimlik doğrulaması kullanabilirsiniz. Azure istemci sertifikaları hakkında daha fazla bilgi için bkz: [nasıl için yapılandırma TLS karşılıklı kimlik doğrulaması için Web Apps](app-service-web-configure-tls-mutual-auth.md). ASP.NET, temel kimlik doğrulaması hakkında daha fazla bilgi için bkz: [ASP.NET Web API 2 kimlik doğrulaması filtrelerini](http://www.asp.net/web-api/overview/security/authentication-filters).

Hizmet hesabı kimlik doğrulaması bir App Service logic uygulamadan bir API uygulamasına içinde ayrıntılı bir özel durum olan [Logic apps ile App Service üzerinde barındırılan özel API'nizi kullanma](../logic-apps/logic-apps-custom-hosted-api.md).

## <a name="authorization"></a>Yetkilendirme App Service içinde nasıl çalışır
Uygulamanızı erişebilirsiniz istekleri üzerinde tam denetime sahiptir. App Service kimlik doğrulama / yetkilendirme herhangi bir aşağıdaki davranışları ile yapılandırılabilir:

* Uygulamanızı ulaşmak yalnızca kimliği doğrulanmış istekler izin verir.
  
    Bir tarayıcı adsız bir istek gönderdiğinde, uygulama hizmeti seçtiğiniz ve böylece kullanıcılar oturum açabilir kimlik sağlayıcısı için bir sayfaya yönlendirilirsiniz. İstek bir mobil CİHAZDAN geliyorsa, döndürülen yanıtın bir HTTP'dir *401 Yetkisiz* yanıt.
  
    Bu seçenek ile tüm kimlik doğrulama kodu, uygulamanızda hiç yazma gerekmez. Hassas yetkilendirme gerekiyorsa, kullanıcı hakkındaki bilgileri kodunuz için kullanılabilir.
* Uygulamasına ulaşması ancak kimliği doğrulanmış istekler doğrulamak ve kimlik doğrulama bilgilerini HTTP üst bilgilerinde geçirirsiniz tüm istekleri izin verir.
  
    Bu seçenek, uygulama kodunuzun yetkilendirme kararları erteler. Anonim istekler işleme daha fazla esneklik sağlar, ancak kod yazmak zorunda.
* Uygulamanızı ulaşmak tüm isteklere izin vermek ve hiçbir eylemde isteklerinde kimlik doğrulama bilgileri.
  
    Bu durumda, kimlik doğrulama / yetkilendirme özelliğini kapalıdır. Tamamen uygulama kodunuz kadar kimlik doğrulama ve yetkilendirme görevleridir.

Önceki davranışları denetlediği **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem** Azure portalında seçeneği. Seçerseniz ** ile oturum *sağlayıcı adı* **, tüm istekleri doğrulanması gerekir. **İsteğin (hiçbir işlem) izin** kodunuzu, yetkilendirme kararını erteler ancak hala bir kimlik doğrulama bilgilerini sağlar. Kodunuzun her şeyi işlemek istiyorsanız, kimlik doğrulaması devre dışı bırakabilirsiniz / yetkilendirme özelliği.

## <a name="working-with-user-identities-in-your-application"></a>Kullanıcı kimlikleri, uygulamanızda ile çalışma
Uygulama hizmeti bazı kullanıcı bilgileri özel üstbilgileri kullanarak uygulamanıza geçirir. Dış istekleri engelle Bu üstbilgileri ve mevcut ise yalnızca ayarlanacak tarafından App Service kimlik doğrulama / yetkilendirme. Bazı örnek üst bilgiler içerir:

* X-MS-İSTEMCİ-ASIL ADI
* X-MS-CLIENT-PRINCIPAL-ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

Tüm dil veya framework ile yazılan kod, gereken bilgileri bu üstbilgileri elde edebilirsiniz. ASP.NET 4.6 uygulamalar için **ClaimsPrincipal** uygun değerlerle otomatik olarak ayarlanır.

Uygulamanızın üzerinde de ek kullanıcı ayrıntıları bir HTTP GET üzerinden edinebilirsiniz `/.auth/me` uygulamanızın uç noktası. İstekle eklemiştir geçerli bir belirteci bir JSON yükü kullanılıyor sağlayıcı ile ilgili ayrıntıları, temel alınan sağlayıcı belirteci ve başka bir kullanıcı bilgileri döndürür. Mobil uygulamalar sunucusu SDK'ları bu verilerle çalışmak için yardımcı yöntemler sağlar. Daha fazla bilgi için bkz: [nasıl Azure Mobile Apps Node.js SDK'ın](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity), ve [.NET arka uç sunucusu SDK ile Azure Mobile Apps için iş](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info).

## <a name="documentation-and-additional-resources"></a>Belgeleri ve ek kaynaklar
### <a name="identity-providers"></a>Kimlik sağlayıcılar
Aşağıdaki öğreticiler App farklı kimlik doğrulama sağlayıcılarını kullanacak şekilde hizmetinin nasıl yapılandırılacağı gösterilmektedir:

* [Azure Active Directory oturum açma kullanmak için uygulamanızı yapılandırma][AAD]
* [Facebook oturum açma kullanmak için uygulamanızı yapılandırma][Facebook]
* [Google oturum açma kullanmak için uygulamanızı yapılandırma][Google]
* [Microsoft Account oturum açma kullanmak için uygulamanızı yapılandırma][MSA]
* [Twitter oturum açma kullanmak için uygulamanızı yapılandırma][Twitter]

Burada da kullanabilirsiniz sağlanan kimlik sistemi dışındaki kullanmak istiyorsanız, [Önizleme mobil uygulamaları .NET sunucusu SDK özel kimlik doğrulama desteği][custom-auth], web uygulamaları, mobil kullanılabileceği uygulamaları veya API uygulamaları.

### <a name="mobile-applications"></a>Mobil uygulamalar
Aşağıdaki öğreticiler sunucu yönlendirilmiş akışını kullanarak kimlik doğrulaması mobil istemcilerinize eklemek nasıl gösterir:

* [İOS uygulamanıza kimlik doğrulaması ekleme][iOS]
* [Android uygulamanıza kimlik doğrulaması ekleme][Android]
* [Windows kimlik doğrulaması ekleme][Windows]
* [Xamarin.iOS uygulamanıza kimlik doğrulaması ekleme][Xamarin.iOS]
* [Xamarin.Android uygulamanıza kimlik doğrulaması ekleme][Xamarin.Android]
* [Xamarin.Forms uygulamanıza kimlik doğrulaması ekleme][Xamarin.Forms]
* [Cordova uygulamanıza kimlik doğrulaması ekleme][Cordova]

Azure Active Directory istemci yönlendirilmiş akış kullanmak istiyorsanız, aşağıdaki kaynakları kullanın:

* [İOS için Active Directory kimlik doğrulama kitaplığı kullanma][ADAL-iOS]
* [Android için Active Directory kimlik doğrulama kitaplığı kullanma][ADAL-Android]
* [Windows ve Xamarin için Active Directory Authentication Library kullanın][ADAL-dotnet]

İstemci yönlendirilmiş akış Facebook için kullanmak istiyorsanız, aşağıdaki kaynakları kullanın:

* [İOS için Facebook SDK'yı kullanma](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

İstemci yönlendirilmiş akış Twitter için kullanmak istiyorsanız, aşağıdaki kaynakları kullanın:

* [İOS için twitter doku kullanma](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

İstemci yönlendirilmiş akış için Google kullanmak istiyorsanız, aşağıdaki kaynakları kullanın:

* [İOS için oturum açma Google SDK'yı kullanma](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

<!-- ### API applications
The following tutorials show how to protect your API apps:

* [User authentication for API Apps in Azure App Service][apia-user]
* [Service principal authentication for API Apps in Azure App Service][apia-service] -->

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[Windows]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: app-service-mobile-how-to-configure-google-authentication.md
[MSA]: app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
