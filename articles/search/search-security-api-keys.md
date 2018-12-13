---
title: Yönetici ve sorgu api anahtarlarından - Azure Search güvenli oluşturma ve yönetme
description: API anahtarları hizmet uç noktası erişimi denetler. Yönetici anahtarları, yazma erişimi verin. Sorgu anahtarları, salt okunur erişim için oluşturulabilir.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 6ba63fa776e92dd2f8035cfbbdb8cea2860d106f
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316943"
---
# <a name="create-and-manage-api-keys-for-an-azure-search-service"></a>API anahtarları için Azure Search hizmeti oluşturma ve yönetme

Bir arama hizmeti için tüm istekleri özellikle hizmetiniz için bir salt okunur api oluşturulan anahtarı gerekir. API anahtarı Arama Hizmeti uç noktanızı erişimi kimlik doğrulaması için tek mekanizmasıdır ve her isteği dahil edilmelidir. İçinde [REST çözümleri](search-get-started-nodejs.md#update-the-configjs-with-your-search-service-url-and-api-key), API anahtarını genellikle bir istek üst bilgisinde belirtilen. İçinde [.NET çözümlerini](search-howto-dotnet-sdk.md#core-scenarios), bir anahtar genellikle bir yapılandırma ayarı olarak belirtilen ve ardından geçirilen [kimlik bilgilerini](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient.credentials) (Yönetici anahtarı) veya [SearchCredentials](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient.searchcredentials) (sorgu anahtarı) üzerinde[SearchServiceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient).

Anahtarları, arama hizmetiniz, hizmet sağlanması sırasında oluşturulur. Görüntüleyebilir ve anahtar değerlerini almak [Azure portalında](https://portal.azure.com).

![Portal sayfasında, ayarları, anahtarları bölümüne](media/search-manage/azure-search-view-keys.png)

## <a name="what-is-an-api-key"></a>Bir API anahtarı nedir

Bir API anahtarı, rastgele oluşturulmuş bir sayı ile harflerden oluşan bir dizedir. Aracılığıyla [role dayalı izinleri](search-security-rbac.md), silebilir veya anahtarlarının okuyun, ancak bir anahtar kullanıcı tanımlı bir parolayla değiştirin veya Active Directory arama işlemleri erişmek için birincil kimlik doğrulama yöntemi kullanın. 

Anahtarları iki tür arama hizmetinize erişmek için kullanılır: Yönetici (okuma-yazma) ve sorgu (salt okunur).

|Anahtar|Açıklama|Sınırlar|  
|---------|-----------------|------------|  
|Yönetim Bölgesi|Hizmeti yönetme olanağı dahil olmak üzere tüm işlemler için tüm hakları verir, oluşturun ve dizin, dizin oluşturucular ve veri kaynaklarını silin.<br /><br /> Olarak iki yönetici anahtarı, adlandırılan *birincil* ve *ikincil* Portalı'ndaki anahtarları hizmet oluşturulduğunda ve ayrı ayrı isteğe bağlı olarak yeniden oluşturulur. İki anahtarın kullanılması hizmetine sürekli erişim için ikinci anahtarı kullanılırken bir anahtarını başa döndürmek sağlar.<br /><br /> Yönetici anahtarları, yalnızca HTTP istek üst bilgilerinde belirtilir. Bir yönetici API anahtarını URL'de yerleştirilemiyor.|2 hizmet başına en fazla|  
|Sorgu|Dizinler ve belgeler için salt okunur erişim verir ve genellikle, arama istekleri gönderen istemci uygulamalarına dağıtılır.<br /><br /> Sorgu anahtarları, isteğe bağlı olarak oluşturulur. Bunları el ile portalında veya programlama aracılığıyla oluşturabilirsiniz [Yönetimi REST API'si](https://docs.microsoft.com/rest/api/searchmanagement/).<br /><br /> Sorgu anahtarları, arama, öneri veya arama işlemi için bir HTTP istek üst bilgisinde belirtilebilir. Alternatif olarak, bir parametre olarak bir URL üzerinde bir sorgu anahtarı geçirebilirsiniz. İstemci uygulamanızı istek nasıl formulates bağlı olarak, anahtarın bir sorgu parametresi olarak geçirmek daha kolay olabilir:<br /><br /> `GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2017-11-11&api-key=[query key]`|Hizmet başına 50|  

 Görsel olarak, bir yönetici anahtarı veya sorgu anahtarı arasında bir ayrım yoktur. Her iki anahtarı 32 rastgele oluşan dizeler alfasayısal karakter oluşturulur. Uygulamanızda ne tür bir anahtarı belirtilir, izleme kaybederseniz, şunları yapabilirsiniz [portalında anahtar değerleri kontrol](https://portal.azure.com) veya [REST API](https://docs.microsoft.com/rest/api/searchmanagement/) anahtar türü ve değeri döndürmek için.  

> [!NOTE]  
>  Gibi hassas verileri geçirmek için zayıf güvenlik uygulaması olarak kabul edilir bir `api-key` istek URI'SİNDEKİ. Bu nedenle, Azure Search yalnızca bir sorgu anahtarı olarak kabul eden bir `api-key` sorgu dizesi ve dizininizin içeriğini herkese sürece bunu kaçınmanız gerekir. Genel bir kural olarak geçirme öneririz, `api-key` isteği başlığı.  

## <a name="find-api-keys-for-your-service"></a>Hizmetinizin api anahtarlarını bulmak

Erişim anahtarlarını portalında veya aracılığıyla edinebilirsiniz [Yönetimi REST API'si](https://docs.microsoft.com/rest/api/searchmanagement/). Daha fazla bilgi için [yönetici ve sorgu api anahtarlarını yönetme](search-security-api-keys.md).

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Liste [arama hizmetleri](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) aboneliğiniz için.
3. hizmeti seçin ve hizmet sayfasında bulma **ayarları** >**anahtarları** yönetici ve sorgu anahtarları görüntülemek için.

![Portal sayfasında, ayarları, anahtarları bölümüne](media/search-security-overview/settings-keys.png)

## <a name="regenerate-admin-keys"></a>Yönetici anahtarları yeniden oluştur

Böylece sürekli erişim için ikincil anahtar kullanımı bir birincil anahtar geçişi için her hizmet için iki yönetici anahtarı oluşturulur.

Aynı zamanda birincil ve ikincil anahtarları yeniden oluştur, hizmet işlemleri erişim için iki anahtarı kullanan tüm uygulamaları artık hizmete erişebilir.

1. İçinde **ayarları** >**anahtarları** sayfasında, ikincil anahtarını kopyalayın.
2. Tüm uygulamalar için ikincil anahtarı kullanmak için API anahtarı ayarlarını güncelleştirin.
3. Birincil anahtarı yeniden oluştur.
4. Tüm uygulamaları yeni birincil anahtarı kullanacak şekilde güncelleştirin.

## <a name="secure-api-keys"></a>Api anahtarlarını güvenli hale getirme
Temel güvenlik, portalı veya Resource Manager arabirimleri (PowerShell veya komut satırı arabirimi) aracılığıyla erişimi kısıtlayarak oldunuz. Belirtildiği gibi abonelik yöneticileri görüntüleyebilir ve tüm API anahtarları yeniden oluştur. Önlem olarak, yönetici anahtarlarına erişimi olanların anlamak için rol atamalarını gözden geçirin.

+ Hizmet panosunda, tıklayın **erişim denetimi (IAM)** ardından **rol atamaları** hizmetiniz için rol atamalarını görüntülemek için sekmesinde.

Aşağıdaki rollerinin üyeleri görüntüleyebilir ve anahtarları yeniden oluştur: Sahip, katkıda bulunan, [arama hizmeti Katkıda Bulunanlar](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#search-service-contributor)

> [!Note]
> Kimlik tabanlı arama sonuçları üzerinden erişim için istek sahibine erişim olmamalıdır belgelerin kaldırılması kimlik tarafından sonuçları kırpmak için güvenlik filtreler oluşturabilirsiniz. Daha fazla bilgi için [güvenlik filtreleri](search-security-trimming-for-azure-search.md) ve [Active Directory ile güvenli](search-security-trimming-for-azure-search-with-aad.md).

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Search'te rol tabanlı erişim denetimi](search-security-rbac.md)
+ [PowerShell’i kullanarak yönetme](search-manage-powershell.md) 
+ [Performans ve iyileştirme makale](search-performance-optimization.md)