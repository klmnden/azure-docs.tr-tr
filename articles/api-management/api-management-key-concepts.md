---
title: "Azure API Management - Genel bakış ve temel kavramlar | Microsoft Docs"
description: "API'ler, ürünler, roller, gruplar ve diğer API Management temel kavramları hakkında bilgi edinin."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: e71da405-835a-48f3-956f-45c1a85698d7
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: becffc6011ef1dd49e07d22880d3346036629393
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2017
---
# <a name="what-is-api-management"></a>API Management nedir?
API Management, kuruluşların kendi veri ve hizmet potansiyellerini ortaya çıkarmak üzere API’leri dış, iş ortağı ve iç geliştiricilere yayımlamalarına yardımcı olur. Her yerdeki işletmeler, bir dijital platform olarak işlemlerini genişletmek, yeni kanallar oluşturmak, yeni müşteriler bulmak ve mevcut müşterilerle daha derin etkileşimi yürütmeyi amaçlar. API Management; geliştirici katılımı, iş öngörüleri, analizler, güvenlik ve koruma aracılığıyla başarılı bir API programı yürütmeye ilişkin temel yetkinlikler sağlar.

Azure API Management’e genel bakış için aşağıdaki videoyu izleyin ve erişim denetimi, hız sınırlaması, izleme, olay günlüğüne kaydetme ve yanıt önbelleğe alma gibi birçok özelliği minimum çabayla API’nize eklemek için API Management’i kullanmayı öğrenin.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-API-Management-Overview/player]
> 
> 

Yöneticiler API Management’i kullanmak için API'ler oluşturur. Her API bir veya daha fazla işlemden oluşur ve her API bir veya daha fazla ürüne eklenebilir. Bir API kullanmak için, geliştiriciler bu API’yi içeren ürüne abone olur ve böylece geçerli olabilecek kullanım ilkelerine tabi olarak API işlemini çağırabilir.

Bu konuda API Management temel kavramlarına ilişkin genel bir bakış verilmiştir.

> [!NOTE]
> Daha fazla bilgi için bkz. [Bulut tabanlı API Management: API'lerin Gücünden Yararlanma](http://j.mp/ms-apim-whitepaper) PDF teknik incelemesi. API Management hakkında CITO Research tarafından hazırlanan tanıtım amaçlı bu teknik inceleme şunları içerir: 
> 
> * Yaygın API gereksinimleri ve sorunları
> * API'leri ayırma ve cepheleri sunma
> * Geliştiricilerin hızla hazır olmalarını sağlama
> * Erişimi güvenli hale getirme
> * Analizler ve ölçümler
> * API Management platformuyla denetim ve öngörü elde etme
> * Bulut ile şirket içi çözüm kullanımının karşılaştırması
> * Azure API Management
> 
> 

## <a name="apis"> </a>API’ler ve işlemler
API'ler bir API Management hizmet örneğinin temelini oluşturur. Her API geliştiricilere sunulan bir işlemler kümesini temsil eder. Her API, API’yi uygulayan arka uç hizmetine başvuru içerir ve bunun işlemleri arka uç hizmeti tarafından uygulanan işlemlere eşlenir. API Management işlemleri; URL eşleme, sorgu ve yol parametreleri, istek ve yanıt içeriği ve işlem yanıtını önbelleğe alma üzerinde sahip olunan denetim sayesinde yüksek oranda yapılandırılabilir niteliktedir. Hızı sınırı, kotalar ve IP kısıtlama ilkeleri de API veya tek işlem düzeyinde uygulanabilir.

Daha fazla bilgi için bkz. [API oluşturma][How to create APIs] ve [API'ye işlem ekleme][How to add operations to an API].

## <a name="products"> </a> Ürünler
Ürünler API'lerin geliştiricilerin kullanımına nasıl sunulduğudur. API Management ürünleri bir ya da daha fazla API’ye sahiptir. Başlık, açıklama ve kullanım koşulları ile yapılandırılırlar. Ürünler **Açık** veya **Korumalı** olabilir. Korumalı ürünleri kullanabilmek için bunlara abone olmak gerekir, açık ürünler abonelik olmadan kullanılabilir. Bir ürün geliştiriciler tarafından kullanılmaya hazır olduğunda yayımlanabilir. Yayımlandıktan sonra, geliştiriciler tarafından görüntülenebilir (ve abone olunan korumalı ürünler söz konusu olduğunda). Abonelik onayı ürün düzeyinde yapılandırılır ve yönetici onayı gerektirebilir ya da otomatik olarak onaylanır.

Gruplar, ürünlerin geliştiricilere görünürlüğünü yönetmek için kullanılır. Ürünler gruplara görünürlük sağlar ve geliştiriciler ait oldukları gruplar tarafından görünür olan ürünleri görüntüleyip bunlara abone olabilir. 

Daha fazla bilgi için [Ürün oluşturma ve yayımlama][How to create and publish a product] konusunu ve aşağıdaki videoyu inceleyin.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Using-Products/player]
> 
> 

## <a name="groups"> </a> Gruplar
Gruplar, ürünlerin geliştiricilere görünürlüğünü yönetmek için kullanılır. API Management aşağıdaki sabit sistem gruplarına sahiptir.

* **Yöneticiler**: Azure aboneliği yöneticileri bu grubun üyesidir. Yöneticiler, geliştiriciler tarafından kullanılan API’leri, işlemleri ve ürünleri oluşturarak API Management hizmet örneklerini yönetir.
* **Geliştiriciler**: Kimliği doğrulanmış geliştirici portalı kullanıcıları bu gruba girer. Geliştiriciler, API'lerinizi kullanarak uygulama oluşturan müşterilerdir. Geliştiriciler, geliştirici portalına erişim iznine sahiptir ve bir API’nin işlemlerini çağıran uygulamalar oluşturur.
* **Konuklar**: Kimliği doğrulanmamış geliştirici portalı kullanıcıları (bir API Management örneğinin geliştirici portalını ziyaret eden olası müşteriler gibi) bu gruba girer. Bunlara API’leri görüntüleyebilme ancak çağıramama gibi bazı salt okunur erişimler verilebilir.

Bu sistem gruplarına ek olarak, yöneticiler özel gruplar oluşturabilir veya [ilişkili Azure Active Directory kiracılarındaki dış grupları kullanabilir](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group). Özel ve dış gruplar, geliştiricilere görünürlük ve API ürünlerine erişim sağlayan sistem gruplarıyla birlikte kullanılabilir. Örneğin, belirli bir iş ortağı kuruluşla ilişkili geliştiriciler için özel bir grup oluşturabilir ve bunlara yalnızca ilgili API'leri içeren bir üründen API'lere erişim izni verebilirsiniz. Bir kullanıcı birden fazla grubun üyesi olabilir.

Daha fazla bilgi için bkz. [Grupları oluşturma ve kullanma][How to create and use groups].

## <a name="developers"> </a> Geliştiriciler
Geliştiriciler API Management hizmet örneğindeki kullanıcı hesaplarını temsil eder. Geliştiriciler yöneticiler tarafından oluşturulabilir ve davet edilebilir ya da [Geliştirici portalı][Developer portal] üzerinden kaydolabilir. Her geliştirici bir veya daha fazla grubun üyesidir ve bu gruplara görünürlük sağlayan ürünlere abone olabilir.

Bir ürüne abone olan geliştiricilere ürün için birincil ve ikincil anahtar verilir. Bu anahtar ürün API’lerine çağrılar yapılırken kullanılır.

Daha fazla bilgi için bkz. [Geliştirici oluşturma ve davet etme][How to create or invite developers] ve [Grupları geliştiricilerle ilişkilendirme][How to associate groups with developers].

## <a name="policies"> </a> İlkeler
İlkeler, yayımcının API’nin davranışını yapılandırma yoluyla değiştirmesini sağlayan güçlü API Management özellikleridir. İlkeler, bir API isteği veya yanıtı üzerinde sırayla yürütülen deyimlerin bir koleksiyonudur. Sık kullanılan deyimler, XML’den JSON’a biçim dönüştürmeyi ve bir geliştiriciden gelen çağrıların sayısını sınırlamak üzere çağrı hızını sınırlamayı ve çeşitli ilkeleri içerir.

İlke ifadeleri herhangi bir API Management ilkesinde, ilke aksini belirtmedikçe, öznitelik değerleri ya da metin değerleri olarak kullanılabilir. [Akışı denetle](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) ve [Değişken ayarla](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable) gibi bazı ilkeler ilke ifadelerini temel alır. Daha fazla bilgi için [Gelişmiş İlkeler](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies) ve [İlke ifadeleri](https://msdn.microsoft.com/library/azure/dn910913.aspx) bölümleriyle aşağıdaki videoyu inceleyin.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

API Management ilkelerinin tam listesi için bkz. [İlke başvurusu][Policy reference]. İlkeleri yapılandırma ve kullanma hakkında daha fazla bilgi için bkz. [API Management ilkeleri][API Management policies]. Hız sınırı ve kota ilkeleri içeren bir ürün oluşturmaya ilişkin öğretici için bkz. [Gelişmiş ürün ayarları oluşturma ve yapılandırma][How create and configure advanced product settings]. Gösteri için aşağıdaki videoya bakın.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

## <a name="developer-portal"> </a> Geliştirici portalı
Geliştirici portalı, geliştiricilerin API’leriniz hakkında bilgi alabileceği, işlemleri görüntüleyebileceği ve çağırabileceği ve ürünlere abone olabileceği yerdir. Müşteri adayları, geliştirici portalını ziyaret edebilir, API’lerle işlemleri görüntüleyebilir ve portala kaydolabilir. Geliştirici portalınızın URL’si, API Management hizmet örneğinizin Klasik Azure Portalı’ndaki panoda yer alır.

Özel içerik ekleyerek, stilleri özelleştirerek ve marka bilgilerinizi ekleyerek, geliştirici portalınızın genel görünümünü özelleştirebilirsiniz.

## <a name="api-management-and-the-api-economy"></a>API Management ve API ekonomisi
API Management hakkında daha fazla bilgi için Microsoft Ignite 2015 konferansına ait aşağıdaki sunuyu izleyin.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3708/player]
> 
> 

[APIs and operations]: #apis
[Products]: #products
[Groups]: #groups
[Developers]: #developers
[Policies]: #policies
[Developer portal]: #developer-portal

[How to create APIs]: api-management-howto-create-apis.md
[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer
[How create and configure advanced product settings]: api-management-howto-product-with-rules.md
[How to create or invite developers]: api-management-howto-create-or-invite-developers.md
[Policy reference]: api-management-policy-reference.md
[API Management policies]: api-management-howto-policies.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance




