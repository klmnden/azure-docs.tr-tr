---
title: İçin özel olarak geliştirilmiş bir uygulamada belirli alanları doldurmak nasıl | Microsoft Docs
description: Azure AD ile özel olarak geliştirilen bir uygulama kaydı sırasında belirli alanları doldurun hakkında yönergeler
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: ryanwi
ms.collection: M365-identity-device-management
ms.openlocfilehash: b01ff1e2d0c9bc926d54bd54716e0579ef395ec0
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67655984"
---
# <a name="how-to-fill-out-specific-fields-for-a-custom-developed-application"></a>Nasıl için özel olarak geliştirilmiş bir uygulamada belirli alanları doldurun

Bu makalede, kullanılabilir tüm alanları uygulama kayıt formunda kısa bir açıklamasını sunar [Azure portalında](https://portal.azure.com).

## <a name="register-a-new-application"></a>Yeni uygulama kaydetme

-   Yeni bir uygulamayı kaydetmek için gidin [Azure portalında](https://portal.azure.com).

-   Sol gezinti bölmesinden tıklayın **Azure Active Directory.**

-   Seçin **uygulama kayıtları** tıklatıp **Ekle**.

-   Bu kayıt formunu açın.

## <a name="fields-in-the-application-registration-form"></a>Kayıt formunu alanları


| Alan            | Açıklama                                                                              |
|------------------|------------------------------------------------------------------------------------------|
| Ad             | Uygulamanın adı. Bu, en az dört karakter olmalıdır.                |
| Desteklenen hesap türleri| Hangi hesapların desteklemek üzere uygulamanızı istediğinizi seçin: yalnızca kuruluş bu dizinde hesapları, tüm kuruluş dizinindeki hesaplarını veya herhangi bir kuruluş dizinindeki ve kişisel Microsoft hesapları.  |
| Yeniden yönlendirme URI'si (isteğe bağlı) | Oluşturmakta olduğunuza, uygulama türünü seçin **Web** veya **genel istemci (Mobil ve Masaüstü)** ve ardından uygulamanızın yeniden yönlendirme URI'si (veya yanıt URL'si) girin. Web uygulamaları için, uygulamanızın temel URL'sini girin. Örneğin http://localhost:31544 yerel makinenizde çalışan bir web uygulamasının URL'si olabilir. Kullanıcılar, bir web istemci uygulamasında oturum açmak için bu URL'yi kullanır. Genel istemci uygulamaları için, Azure AD'nin belirteç yanıtlarını döndürmek üzere kullandığı URI'yi girin. Myapp://auth gibi uygulamanıza özgü bir değer girin. Web uygulamalarına veya yerel uygulamalara özgü örnekler görmek için, [hızlı başlangıçlara](https://docs.microsoft.com/azure/active-directory/develop) göz atın.|

Yukarıdaki alanları doldurduktan sonra Azure portalında uygulama kaydedilir ve uygulama genel bakış sayfasına yönlendirilirsiniz. Sol bölmede altında ayarları sayfalarında **Yönet** uygulamanızı özelleştirmek size daha fazla alana sahip. Aşağıdaki tablolarda tüm alanları açıklar. Yalnızca, bir web uygulaması veya bir ortak istemci uygulaması oluşturduğunuz bağlı olarak, bu alanların bir alt kümesini görür.

### <a name="overview"></a>Genel Bakış
| Alan           | Açıklama        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Uygulama Kimliği  | Bir uygulamayı kaydettiğinizde, Azure AD uygulamanızı bir uygulama kimliği atar Azure ad kimlik doğrulama isteklerini uygulamanıza benzersiz olarak tanımlanabilmesi gibi kaynaklara erişmek için kimlik kullanılabilir uygulama Graph API'sini ister.                                                          |
| Uygulama Kimliği URI'si      | Bu genellikle formun benzersiz bir URI olmalıdır **https://&lt;Kiracı\_adı&gt;/&lt;uygulama\_adı&gt;.** Yetkilendirme verme akışı sırasında bu belirteç için yayınlanması gereken kaynak belirtmek için benzersiz bir tanımlayıcı olarak kullanılır. Ayrıca, verilen erişim belirtecinde 'aud' talep olur. |

### <a name="branding"></a>Markalama

| Alan           | Açıklama        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Karşıya yeni logo yükle | Bu, uygulamanız için bir logosu yüklemek için kullanabilirsiniz. Logo, .bmp, .jpg veya .png biçiminde olmalıdır ve dosya boyutu 100 KB'tan küçük olmalıdır. Görüntü boyutları ile merkez resim boyutları: 94 x 94 piksel 215 x 215 piksel olmalıdır.|
| Giriş sayfası URL'si   | Uygulama kaydı sırasında belirtilen oturum açma URL'si budur.|

### <a name="authentication"></a>Authentication

| Alan           | Açıklama        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Oturum Kapatma URL'si      | Çoklu oturum kapatma oturum kapatma URL'si budur. Azure AD ile kullanıcı oturumuna temizlediğinde azure AD'ye kayıtlı herhangi bir uygulamayı kullanarak bu URL'ye oturum kapatma isteği gönderir.|
| Desteklenen hesap türleri  | Bu anahtar, birden fazla Kiracı tarafından uygulamanın kullanılıp kullanılamayacağını belirtir. Bu genellikle, dış kuruluşlar kendilerine ait kiracıda kaydedip kuruluş verilerine erişimi verme uygulamanızın kullanabileceği anlamına gelir.|
| Yeniden yönlendirme URL'leri      | Yeniden yönlendirme veya yanıt URL'leri Azure AD, uygulamanız tarafından istenen belirteçleri döndürdüğü uç noktalardır. Yerel uygulamalar için kullanıcı başarılı yetkilendirme sonrasında burada gönderilen budur. Azure AD yeniden yönlendirme URI'si, uygulamanız OAuth 2.0 isteğine sağlayan Portalı'nda kayıtlı değerler biriyle eşleşen denetler.|

### <a name="certificates-and-secrets"></a>Sertifikaları ve parolaları

| Alan           | Açıklama        |
|-----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| İstemci gizli dizileri            | İstemci gizli dizileri veya programlama yoluyla erişim web güvenliği herhangi bir kullanıcı etkileşimi olmadan Azure AD tarafından sağlanan API'leri, anahtarları oluşturabilirsiniz. Gelen **yeni gizli** sayfasında, bir anahtar açıklaması ve sona erme tarihini girin ve anahtarı oluşturmak için kaydedin. Daha sonra erişmek mümkün olmayacaktır gibi güvenli bir yere kaydettiğinizden emin olun.             |

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](../manage-apps/what-is-application-management.md)
