---
title: V1.0 uygulama türleri | Azure
description: Tür uygulamaları ve Azure Active Directory v2.0 uç noktası tarafından desteklenen senaryoları açıklar.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: saeeda, jmprieur, andret
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef180fb444e32e8b055837fd418e21162ff58339
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60411198"
---
# <a name="application-types-in-v10"></a>V1.0 uygulama türleri

Azure Active Directory (Azure AD) desteklediği kimlik doğrulaması için OAuth 2.0 veya Openıd Connect endüstri standardı protokollerine göre tüm modern uygulama mimarilerine çeşitli.

Aşağıdaki diyagram senaryoları ve uygulama türleri gösterir ve ne kadar farklı bileşenleri eklenebilir:

![Uygulama Türleri ve senaryolar](./media/authentication-scenarios/application_types_and_scenarios.png)

Azure AD tarafından desteklenen beş birincil uygulama senaryoları şunlardır:

- **[Tek sayfalı uygulama (SPA)](single-page-application.md)**: Bir kullanıcının Azure AD tarafından güvenliği sağlanan bir tek sayfalı uygulama için oturum açmanız gerekir.
- **[Web uygulamasına Web tarayıcısı](web-app.md)**: Bir kullanıcının Azure AD tarafından güvenliği sağlanan bir web uygulaması için oturum açmanız gerekir.
- **[Web API'si yerel uygulamaya](native-app.md)**: Telefon, tablet veya PC çalıştıran yerel bir uygulama bir Web API'si Azure AD tarafından güvenli hale getirilen kaynakları almak için bir kullanıcının kimliğini doğrulaması gerektiğinde.
- **[Web uygulaması web API'si için](web-api.md)**: Bir Web API'si Azure AD tarafından güvenliği sağlanan kaynakları almak bir web uygulaması gerekir.
- **[Web API arka plan programı ya da sunucu uygulamasında](service-to-service.md)**: Azure AD tarafından güvenliği sağlanan bir web API'den kaynakları almak bir arka plan programı uygulaması veya web kullanıcı arabirimi ile bir sunucu uygulaması gerekir.

Kod ile çalışmaya başlamadan önce üst düzey senaryoları anlamanıza ve her uygulama türü hakkında daha fazla bilgi edinmek için bağlantıları izleyin. V1.0 uç nokta veya v2.0 uç noktası ile çalışan belirli bir uygulama yazıyorsanız, bilmeniz gereken farklar hakkında bilgi edinebilirsiniz.

> [!NOTE]
> V2.0 uç noktası, tüm Azure AD senaryoları ve özellikleri desteklemiyor. V2.0 uç noktası kullanması gerekip gerekmediğini belirlemek için aşağıdaki hakkında bilgi edinin: [v2.0 sınırlamaları](active-directory-v2-limitations.md).

Burada, çeşitli dilleri ve platformları kullanarak açıklanan senaryoları ve uygulamaları geliştirebilirsiniz. Bunlar tüm kod örnekleri Kılavuzu'nda kullanılabilir olan tam kod örnekleri tarafından desteklenir: [senaryoya göre kod örnekleri v1.0](sample-v1-code.md) ve [senaryoya göre v2.0 kod örnekleri](sample-v2-code.md). Doğrudan ilgili kod örneklerini de indirebilirsiniz [GitHub örnek depoları](https://github.com/Azure-Samples?q=active-directory).

Uygulamanızın belirli bir parça veya bir uçtan uca senaryo segmentini gerekiyorsa, ek olarak, çoğu durumda bu işlevselliği bağımsız olarak eklenebilir. Örneğin, bir web API'si çağıran bir yerel bir uygulamanız varsa, web API'si çağıran bir web uygulamasını kolayca ekleyebilirsiniz.

## <a name="app-registration"></a>Uygulama kaydı

### <a name="registering-an-app-that-uses-the-azure-ad-v10-endpoint"></a>Azure AD v1.0 uç noktası kullanan bir uygulamayı kaydetme

Azure AD kimlik outsources herhangi bir uygulama bir dizinde kayıtlı olması gerekir. Bu adım, Azure AD'ye nerede, kimlik doğrulamasından sonra uygulamanızı tanımlamak için URI yanıtların gönderileceği URL URL'si dahil olmak üzere, uygulamanızla ilgili belirten içerir. Bu bilgiler, birkaç önemli nedenden dolayı gereklidir:

* Azure AD oturum açma veya alışverişi belirteçleri işlerken uygulama ile iletişim kurması gerekir. Bilgiler, Azure AD arasında geçirilen ve uygulama şunları içerir:
  
  * **Uygulama Kimliği URI'si** -bir uygulama için tanımlayıcı. Bu değer, hangi uygulamanın arayan için bir belirteç istediği göstermek için Azure ad kimlik doğrulaması sırasında gönderilir. Ayrıca, böylece uygulamayı, istenen hedef olduğunu bilir bu değer belirtece dahildir.
  * **Yanıt URL'si** ve **yeniden yönlendirme URI'si** -bir web API'si veya web uygulaması için yanıt URL'si Azure AD kimlik doğrulaması başarılı olursa, bir belirteç de kimlik doğrulaması yanıtını gönderir burada konumdur. Yerel bir uygulama için yeniden yönlendirme URI'si, Azure AD bir OAuth 2.0 isteğindeki kullanıcı aracısını yönlendireceği bir benzersiz bir tanımlayıcıdır.
  * **Uygulama Kimliği** -uygulamanın kayıtlı ne zaman Azure AD tarafından oluşturulan bir uygulama için kimliği. Bir yetkilendirme kodunu veya belirteci isteğinde bulunurken uygulama kimliği ve anahtarı Azure AD'ye kimlik doğrulaması sırasında gönderilir.
  * **Anahtar** -bir uygulama kimliği ile birlikte kimlik doğrulaması yapılırken Azure AD ile bir web API'sini çağırmak için gönderilen anahtarı.
* Azure AD uygulama dizin verilerinize, kuruluşunuzdaki diğer uygulamaların erişmek üzere gerekli izinlere sahip olduğundan emin olmak gerekir.

Daha fazla bilgi için edinmek için nasıl [bir uygulamayı Azure AD'ye v1.0 uç noktası ile kaydetme](quickstart-v1-add-azure-ad-app.md).

## <a name="single-tenant-and-multi-tenant-apps"></a>Tek kiracılı ve çok kiracılı uygulamalar

Geliştirilen ve Azure AD ile tümleştirilmiş uygulamalar iki kategorisi vardır anladığınızda sağlama daha anlaşılır hale gelir:

* **Tek kiracılı uygulama** -tek kiracılı bir uygulama bir kuruluş içinde kullanıma yöneliktir. Bir kurumsal geliştirici tarafından yazılan genellikle satır iş kolu (LoB) uygulamaları şunlardır. Tek kiracılı bir uygulama yalnızca bir dizindeki kullanıcılar tarafından erişilmesi gereken ve sonuç olarak, yalnızca bir dizinde sağlanması gerekir. Bu uygulamalar, genellikle kuruluştaki bir geliştirici tarafından kaydedilir.
* **Çok kiracılı uygulama** -çok kiracılı bir uygulama pek çok kuruluşta, kullanım için tasarlanmamış yalnızca bir kuruluş. Bir bağımsız yazılım satıcısı (ISV) tarafından yazılan genellikle yazılım olarak-hizmet (SaaS) uygulamalarıyla şunlardır. Çok kiracılı uygulamaları, bunları kaydetmek için kullanıcı veya yönetici onayı gerektiren nerede bunlar kullanılır, her dizinde sağlanması gerekir. Bir uygulama dizinde kayıtlı ve Graph API'sini ya da belki başka bir web API'si için erişim verilir, bu onay işlemini başlatır. Bir kullanıcı veya farklı bir kuruluşun Yöneticisi uygulamasını kullanmak oturum açtığında, onlara uygulama izinleri görüntüleyen bir iletişim kutusu ile gösterilir. Kullanıcı veya yönetici belirtilen uygulama erişmesini sağlayan ve son olarak, dizinde uygulama kaydeder uygulamaya sonra onay verebilir. Daha fazla bilgi için [onay Framework'ün genel bakış](consent-framework.md).

### <a name="additional-considerations-when-developing-single-tenant-or-multi-tenant-apps"></a>Tek bir kiracı veya çok kiracılı uygulamalar geliştirirken dikkat edilecek diğer noktalar

Bazı ek hususlar, çok kiracılı bir uygulama yerine tek kiracılı bir uygulama geliştirirken ortaya çıkar. Örneğin, uygulamanızın kullanılabilir birden çok dizini kullanıcılara yapıyorsanız, hangi Kiracı belirlemek için bir mekanizma gerekir bulundukları. Tek kiracılı bir uygulama, yalnızca kendi dizinde bir kullanıcının Azure AD'de belirli bir kullanıcı tarafından tüm dizinleri tanımlamak çok kiracılı bir uygulama gereksinimlerini aramak gerekir. Bu görevi gerçekleştirmek için Azure AD oturum açma istekleri bir kiracıya özgü uç nokta yerine herhangi bir çok kiracılı Uygulama yeri yönlendirebilir ortak bir kimlik doğrulama uç noktası sağlar. Bu uç nokta https://login.microsoftonline.com/common Azure AD'de, tüm dizinler için bir kiracıya özgü uç nokta olabilir ancak https://login.microsoftonline.com/contoso.onmicrosoft.com. Ortak uç nokta, oturum açma, oturum kapatma ve belirteç doğrulama sırasında birden fazla Kiracı işlemek için gerekli mantığı gerekeceği için Uygulamanızı geliştirirken dikkat edilmesi özellikle önemlidir.

Şu anda tek kiracılı bir uygulama geliştiriyorsanız, ancak çoğu kuruluş için kullanılabilir hale getirmek istediğiniz kolayca uygulama ve çok kiracılı sağlamak üzere Azure AD yapılandırmasında için özellikli değişiklik yapabilirsiniz. Ayrıca, tek bir kiracı veya çok kiracılı uygulamada kimlik doğrulaması sağlanmaktadır olup olmadığını Azure AD tüm dizinlerdeki tüm belirteçler aynı imzalama anahtarı kullanır.

Bu belgede listelenen her bir senaryo sağlama gereksinimleri tanımlayan bir alt içerir. Azure AD'de bir uygulama yanı sıra tek ve çok kiracılı uygulamaları arasındaki farklar sağlama hakkında daha ayrıntılı bilgi için bkz. [uygulamaları Azure Active Directory ile tümleştirme](quickstart-v1-integrate-apps-with-azure-ad.md) daha fazla bilgi için. Azure AD'de genel uygulama senaryolarının anlamak için okumaya devam edin.

## <a name="next-steps"></a>Sonraki adımlar

- Diğer Azure AD hakkında daha fazla bilgi [kimlik doğrulaması temelleri](authentication-scenarios.md)
