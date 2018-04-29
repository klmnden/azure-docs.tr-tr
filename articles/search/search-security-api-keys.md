---
title: Oluşturmak, yönetmek ve Azure Search için güvenli yönetici ve sorgu api anahtarlarından | Microsoft Docs
description: api anahtarlarından hizmet uç noktası erişimi denetler. Yönetici anahtarlarına yazma erişimi verin. Sorgu anahtarları salt okunur erişim için oluşturulabilir.
author: HeidiSteen
manager: cgronlun
tags: azure-portal
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.date: 03/20/2018
ms.author: heidist
ms.openlocfilehash: 4215795b7cd2a25427a3ce9b3cde16bfc69cb009
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="create-and-manage-api-keys-for-an-azure-search-service"></a>Oluşturma ve Azure Search hizmeti için API anahtarları Yönet

Bir arama hizmeti için tüm istekleri özellikle hizmetinizin api oluşturulan anahtarı gerekir. Bu API anahtarı, Arama Hizmeti uç noktanızı erişimi kimlik doğrulaması için tek bir mekanizmadır. 

Bir API anahtarı rastgele oluşturulmuş sayılar ve harflerden oluşan bir dizedir. Aracılığıyla [role dayalı izinleri](search-security-rbac.md), silebilir veya anahtarları okumak, ancak bir anahtarı kullanıcı tanımlı bir parolayla değiştirmek veya Active Directory arama işlemleri erişmek için birincil kimlik doğrulama yöntemi kullanın. 

Arama hizmetinize erişmek için kullanılan iki tür anahtarları: Yönetici (okuma-yazma) ve sorgu (salt okunur).

|Anahtar|Açıklama|Sınırlar|  
|---------|-----------------|------------|  
|Yönetim Bölgesi|Hizmet yönetme özelliği de dahil tüm işlemleri tam hakkı verir oluşturun ve dizinler, dizin oluşturucular ve veri kaynaklarını silin.<br /><br /> İki yönetici anahtarları denir *birincil* ve *ikincil* anahtarları portalında, hizmet oluşturulduğunda ve tek tek isteğe bağlı olarak yeniden oluşturulur. İki anahtarına sahip bir anahtar sürekli Erişim hizmeti için ikinci anahtar kullanırken alma izin verir.<br /><br /> Yönetici anahtarları yalnızca HTTP istek üst bilgilerinde belirtilir. Bir URL yönetici api anahtarı yerleştirilemiyor.|En çok hizmeti başına 2|  
|Sorgu|Dizinler ve belgeler için salt okunur erişim verir ve genellikle, arama istekleri gönderen istemci uygulamalarına dağıtılır.<br /><br /> Sorgu anahtarları isteğe bağlı olarak oluşturulur. Bunları el ile Portalı'nda veya programlama aracılığıyla oluşturabilirsiniz [Yönetimi REST API](https://docs.microsoft.com/rest/api/searchmanagement/).<br /><br /> Sorgu anahtarları, arama, öneri veya arama işlemi için bir HTTP istek üstbilgisi olarak belirtilebilir. Alternatif olarak, bir sorgu anahtarı üzerinde bir URL parametre olarak geçirebilirsiniz. İstemci uygulamanızı istek nasıl formulates bağlı olarak, anahtar sorgu parametresi olarak geçirmek daha kolay olabilir:<br /><br /> `GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2017-11-11&api-key=[query key]`|hizmeti başına 50|  

 Görsel olarak, bir yönetici anahtarını veya sorgu anahtarını arasında fark yoktur. Her iki anahtarı 32 rastgele oluşan dizeleri alfa sayısal karakter oluşturulur. Hangi türde bir anahtar uygulamanızda belirtildi, izleme kaybederseniz, şunları yapabilirsiniz [anahtar portal denetleyin](https://portal.azure.com) veya [REST API](https://docs.microsoft.com/rest/api/searchmanagement/) anahtar türü ve değeri döndürmek için.  

> [!NOTE]  
>  Hassas verileri gibi geçirmek için zayıf güvenlik uygulaması olarak kabul edilir bir `api-key` istek URI'SİNDEKİ. Bu nedenle, Azure Search yalnızca bir sorgu anahtarı olarak kabul eden bir `api-key` sorgu dizesi ve sürece dizininizi içeriğinin genel kullanıma açık olması gerekir böylece kaçınmalısınız. Genel kural olarak, geçirme öneririz, `api-key` bir istek üstbilgisi olarak.  

## <a name="find-api-keys-for-your-service"></a>Hizmetinizin api anahtarlarından Bul

Erişim tuşları portalında veya aracılığıyla elde edebilirsiniz [Yönetimi REST API](https://docs.microsoft.com/rest/api/searchmanagement/). Daha fazla bilgi için bkz: [yönetici ve sorgu API anahtarlarını Yönet](search-security-api-keys.md).

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Liste [arama hizmetleri](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) aboneliğiniz için.
3. hizmeti seçin ve hizmet sayfasında bulabilirsiniz **ayarları** >**anahtarları** yönetici ve sorgu anahtarları görüntülemek için.

![Portal sayfasında, ayarları, anahtarları bölümüne](media/search-security-overview/settings-keys.png)

## <a name="regenerate-admin-keys"></a>Yönetici anahtarlarını yeniden oluştur

Böylece sürekli erişim için ikincil anahtarı kullanarak bir birincil anahtar geçişi yapabilirsiniz iki yönetici anahtarları her hizmet için oluşturulur.

Birincil ve ikincil anahtarlar aynı anda oluşturursanız, hizmet işlemleri erişmek için her iki anahtarı kullanarak herhangi bir uygulama artık hizmete erişebilir.

1. İçinde **ayarları** >**anahtarları** sayfasında, ikincil anahtarı kopyalayın.
2. Tüm uygulamalar için ikincil anahtarı kullanmak üzere API anahtarı ayarlarını güncelleştirin.
3. Birincil anahtarı yeniden oluşturun.
4. Yeni birincil anahtarı kullanmak üzere tüm uygulamalar güncelleştirin.

## <a name="secure-api-keys"></a>Güvenli API anahtarları
Anahtar güvenlik portal ya da Resource Manager arabirimleri (PowerShell veya komut satırı arabirimi) aracılığıyla erişimi kısıtlayarak sağlamış. Belirtildiği gibi abonelik yöneticileri görüntülemek ve tüm API anahtarlarını yeniden oluştur. Önlem olarak, yönetici anahtarlarına kimlerin erişebileceğini anlamak için rol atamalarını gözden geçirin.

+ Hizmet panosunda tıklatın **erişim denetimi (IAM)** hizmetiniz için rol atamalarını görüntülemek için.

Aşağıdaki rolleri üyeleri görüntülemek ve yeniden anahtarları: sahibi, katkıda bulunan, [arama hizmeti Katkıda Bulunanlar](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#search-service-contributor)

> [!Note]
> Kimlik tabanlı arama sonuçları üzerinden erişim için istek sahibinin erişim olmamalıdır Belgeler kaldırılıyor sonuçları kimliği tarafından kırpma için güvenlik filtreler oluşturabilirsiniz. Daha fazla bilgi için bkz: [güvenlik filtreleri](search-security-trimming-for-azure-search.md) ve [güvenli Active Directory ile](search-security-trimming-for-azure-search-with-aad.md).

## <a name="see-also"></a>Ayrıca bkz.

+ [Azure Search'te rol tabanlı erişim denetimi](search-security-rbac.md)
+ [PowerShell’i kullanarak yönetme](search-manage-powershell.md) 
+ [Performans ve en iyi duruma getirme makale](search-performance-optimization.md)