---
title: Microsoft kimlik platformu hakkında | Azure
description: Azure Active Directory kimlik hizmeti ve geliştirici platformunun geliştirilmesiyle ortaya çıkan Microsoft kimlik platformu hakkında bilgi edinin.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c7b3eee08c036862e6ce9f0c590a596f7b1d3fb0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60301025"
---
# <a name="about-microsoft-identity-platform"></a>Microsoft kimlik platformu hakkında

Microsoft kimlik platformu, Azure Active Directory (Azure AD) kimlik hizmeti ve geliştirici platformunun geliştirilmesiyle ortaya çıkmıştır. Bu platform geliştiricilerin tüm Microsoft kimlikleriyle oturum açan ve Microsoft Graph veya diğer Microsoft API'leri ya da geliştiricilerin derlemiş olduğu API'lere çağrı göndermek için gerekli belirteçleri alan uygulamalar derlemesini sağlar. Bu tam özellikli platform kimlik doğrulaması hizmetine, açık kaynak kitaplıklara, uygulama kaydı ve yapılandırmasına (bir geliştirici portalı ve uygulama API'si aracılığıyla), tam kapsamlı geliştirici belgelerine, kod örneklerine ve geliştiricilere yönelik diğer içeriklere sahiptir. Microsoft Identity Platform OAuth 2.0 ve OpenID Connect gibi sektör standardı protokolleri destekler.

Şimdiye kadar, geliştiricilerin çoğu Azure AD v1.0 uç noktasından belirteç isteyip Azure AD kimliklerini doğrulayarak, uygulama kaydı ve yapılandırması için Azure AD Authentication Library'yi (ADAL), Azure portalını ve programlı uygulama yapılandırması için Azure AD Graph API'sini kullanarak Azure AD v1.0 platformuyla çalışıyordu. Azure AD v1.0 platformu, kurumsal uygulamalarda kullanılmaya devam edecek, olgun bir platform teklifidir.

Microsoft kimlik platformunun özelliklerini genişletmek ve geliştirmek için, artık Azure AD v2.0 uç noktası olarak bilinen özelliği kullanarak daha geniş bir Microsoft kimlikleri kümesi (Azure AD kimlikleri, Microsoft hesapları (outlook.com ve hotmail.com gibi), Azure AD B2C aracılığıyla sosyal ve yerel hesaplar) için kimlik doğrulaması yapabilirsiniz. Burada, Microsoft Authentication Library'yi (MSAL) ya da herhangi bir açık kaynak OAuth2.0 veya OpenID Connect kitaplığını, uygulama kaydı ve yapılandırması için Azure portalını ve programlı uygulama yapılandırması için Microsoft Graph API'sini kullanacaksınız. Güncelleştirilmiş Microsoft kimlik platformu (özellikle de MSAL kitaplıkları ve en son Azure portalı uygulama kaydı deneyimi), geçen yıl önemli ölçüde geliştirildi. Bu sürümü sonlandırmak için, geliştiricilerin uygulamalarını geliştirir ve test ederken en son Microsoft kimlik platformunu kullanmalarını öneriyoruz.

En son ADAL'ı ve en son MSAL'yi kullanan uygulamalar birbiriyle SSO yapabilecektir. ADAL'dan MSAL'ye güncelleştirilen uygulamalar kullanıcı oturum açma durumunu koruyacaktır. Geliştiriciler uygun görürlerse uygulamalarını MSAL'ye güncelleştirmeyi seçebilir, ama ADAL ile derlenmiş uygulamalar da çalışmaya ve desteklenmeye devam edecektir.

## <a name="microsoft-identity-platform-experience"></a>Microsoft kimlik platformu deneyimi

Aşağıdaki diyagramda, uygulama kaydı deneyimi, SDK'lar, uç noktalar ve desteklenen kimlikler de dahil olmak üzere, üst düzeyde Microsoft kimlik deneyimi gösterilir.

![Bugün Microsoft kimlik platformu](./media/about-microsoft-identity-platform/about-microsoft-identity-platform.svg)

Microsoft kimlik platformunun iki uç noktası (v1.0 ve v2.0) ve bu uç noktaları işlemek için iki istemci kitaplığı kümesi vardır. Yeni uygulama geliştirirken, uç noktaların ve kimlik doğrulama kitaplıklarının avantajlarını ve geçerli durumunu göz önünde bulundurun. Ayrıca şunları da dikkate alın:

* Desteklenen platformlar

    * [ADAL](active-directory-authentication-libraries.md) .NET, JavaScript, iOS, Android, Java, ve Python'u destekler
    * [MSAL Önizleme](reference-v2-libraries.md) .NET, JavaScript, iOS ve Android'i destekler
    * Her iki uç nokta da API'leri ve Oturum açma işlemini korumak için .NET ve Node.js sunucu ara yazılımını destekler. 

* Yeniliklerin büyük bölümü, örneği dinamik onay ve artımlı onay v2.0 uç noktasında ve MSAL'de yapılmıştır ama bu arada v1.0 ile ADAL'ı desteklemeye de devam ediyoruz.

    Azure portalında, daha önce uygulamanıza gereken tüm kapsamları statik olarak tanımlamanız gerekiyordu. v2.0 uç noktasıyla ve portalların bu uç noktayla ilişkilendirilmesiyle birlikte, aynı eskiden olduğu gibi kapsamları statik olarak tanımlayabilir veya uygulamanıza izin gerektiğinde bunları dinamik olarak isteyebilirsiniz. Dinamik yöntem isteğe bağlı bir özellik daha (artımlı onay) sağlar. Artımlı onay, kullanıcı ilk kez kimliğini doğruladığında kapsamların ihtiyacınız olan bir alt kümesini istemenize ve gerektikçe ek kapsamlar istemenize olanak tanır. 
    
    Örneğin mobil cihazda kamera uygulaması kullanılırken, kullanıcı uygulamanın kameraya erişmesine izin verilmesini ister ve ancak kullanıcı onay aldıktan sonra uygulamanın kameraya erişmesine ve fotoğraf çekmesine izin verilir.  Uygulama yeni fotoğrafı kaydetmeye hazır olduğunda, fotoğraf okuma/yazma izni isteyebilir. 

* Olası yeni değişiklikler

    MSAL üretim ortamında kullanılmaya uygundur. Geçerli üretim kitaplıklarımıza sağladığımız üretim düzeyi desteğinin aynısını MSAL'ye de sağlıyoruz. Önizleme sırasında, API'de, iç önbellek biçiminde ve bu kitaplığın diğer mekanizmalarında değişiklikler yapabiliriz. Hata düzeltmeleri veya özellik geliştirmelerinin yanı sıra bu değişiklikleri de almanız gerekir. Bu durum uygulamanızı etkileyebilir. Örnek vermek gerekirse, önbellek biçiminde yapılan bir değişiklik kullanıcılarınızı etkileyebilir, örneğin yeniden oturum açmalarını gerektirebilir. Bir API değişikliğinden dolayı kodunuzu güncelleştirmeniz gerekebilir. Genel kullanım (GA) sürümünü sağladığımızda, altı ay içinde GA sürümüne güncelleştirmenizi isteyeceğiz çünkü kitaplığın önizleme sürümünü kullanarak yazılan uygulamalar bundan sonra çalışmayabilir.

## <a name="next-steps"></a>Sonraki adımlar

v1.0 ve v2.0 hakkında daha fazla bilgi edinin.

* [v1.0 hakkında](v1-overview.md)
* [v2.0 hakkında](v2-overview.md)
