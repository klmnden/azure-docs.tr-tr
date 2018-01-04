---
title: "Verilerin ve işlemlerin Azure Search'te güvenli | Microsoft Docs"
description: "Azure arama güvenlik SOC 2 uyumluluk, şifreleme, kimlik doğrulama ve kullanıcı ve grup güvenlik tanımlayıcıları Azure Search filtrelerde aracılığıyla kimlik erişimi temel alır."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
ms.assetid: 
ms.service: search
ms.devlang: 
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 12/14/2017
ms.author: heidist
ms.openlocfilehash: 23616c70a5fd336b743f5acfad2601a6c3e23fc4
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="data-security-and-controlled-access-to-azure-search-operations"></a>Veri güvenliği ve Azure Search işlemlerine denetimli erişim

Azure arama [SOC 2 uyumlu](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports)kapsamlı güvenlik mimarisi yayılan fiziksel güvenlik, şifrelenmiş iletimler, şifrelenmiş depolama ve platform genelinde yazılım koruması. İşletimsel olarak, Azure Search yalnızca kimliği doğrulanmış istekler kabul eder. İsteğe bağlı olarak, içerik üzerinde kullanıcı başına erişim denetimleri ekleyebilirsiniz. Bu makalede her katmanda güvenlik dokunur, ancak öncelikle nasıl Azure Search'te verilerin ve işlemlerin güvenlidir odaklanmıştır.

![Güvenlik katmanları Blok Diyagramı](media/search-security-overview/azsearch-security-diagram.png)

Azure Search korumalar ve Azure platformu korumaları devralır olsa da, hizmet tarafından kullanılan birincil anahtar tabanlı kimlik doğrulaması, burada anahtar türü erişim düzeyini belirler mekanizmadır. Bir, bir yönetici anahtarı veya salt okunur erişim için bir sorgu anahtarı anahtardır.

Hizmetinize erişim anahtarını (tam veya salt okunur) artı işlemleri kapsamını tanımlayan bir bağlam ilettiği izinleri kesitini temel alır. Her istek zorunlu bir anahtar, bir işlem ve bir nesne oluşur. Birbirine zincirlenmiş, iki izin düzeyleri artı bağlam hizmet işlemleri üzerinde tam spektrumun güvenlik sağlamak için yeterlidir. 

## <a name="physical-security"></a>Fiziksel güvenlik

Microsoft veri merkezleri endüstri lideri fiziksel güvenlik sağlar ve kapsamlı bir standartlar ve düzenlemelere yelpazesini ile uyumludur. Daha fazla bilgi edinmek için şu adrese gidin [küresel veri merkezleri](https://www.microsoft.com/cloud-platform/global-datacenters) sayfa veya Güvenlik Merkezi veri hakkında kısa bir video izleyin.

> [!VIDEO https://www.youtube.com/embed/r1cyTL8JqRg]

## <a name="encrypted-transmission-and-storage"></a>Şifrelenmiş iletim ve depolama

Azure arama HTTPS bağlantı noktası 443 üzerinde dinler. Platform arasında Azure hizmetlerine bağlantıları şifrelenir. 

Arka uç depolama dizinler ve diğer yapıları için kullanılan, Azure Search bu platformlar, şifreleme özelliklerinden yararlanır. Tam [AICPA SOC 2 Uyumluluk](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/aicpasoc2report.html) tüm hizmetleri (yeni ve mevcut), Azure Search sunan tüm veri merkezlerinde arama için kullanılabilir. Tam raporu gözden geçirmek için Git [Azure - ve Azure kamu SOC 2 türü II rapor](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports).

Şifreleme ile dahili olarak yönetilir ve evrensel geçerli şifreleme anahtarları, saydamdır. Belirli arama hizmetleri veya dizin, kapatmak veya anahtarları doğrudan yönetmek veya kendi tedarik olamaz. 

## <a name="azure-wide-logical-security"></a>Azure genelinde mantıksal güvenliği

Birkaç güvenlik mekanizmaları Azure yığın üzerinden kullanılabilen ve bu nedenle otomatik olarak oluşturduğunuz Azure Search kaynaklar için kullanılabilir.

+ [Abonelik ya da silinmesini önlemek için kaynak düzeyi kilitleri](../azure-resource-manager/resource-group-lock-resources.md)
+ [Rol tabanlı erişim denetimi (bilgi ve yönetim işlemleri erişimi denetlemek için RBAC)](../active-directory/role-based-access-control-what-is.md)

Tüm Azure hizmetlerine erişim düzeyleri tutarlı bir şekilde tüm hizmetler arasında ayarlamak için rol tabanlı erişim denetimi (RBAC) destekler. Hizmet durumunu görüntüleyerek herhangi bir rol, üyelerine kullanılabilir iken Örneğin, yönetici anahtarı gibi hassas verileri görüntüleme sahibi ve katkıda bulunan rollere sınırlıdır. RBAC sahibi, katkıda bulunan ve okuyucu rolü sağlar. Varsayılan olarak, tüm hizmet yöneticileri sahip rolünün bir üyesi.

## <a name="service-authentication"></a>Hizmet kimlik doğrulama

Azure arama kendi kimlik doğrulama yöntemleri sağlar. Kimlik doğrulama işlemleri kapsamını belirleyen bir erişim anahtarı temel alır ve her istekte oluşur. Geçerli erişim tuşu güvenilir bir varlıktan kanıt İsteğin kaynaklandığı olarak kabul edilir. 

Hizmeti başına kimlik doğrulama, iki düzeyde var: tam hakları, yalnızca sorgu. Anahtar türü, hangi düzeyde bir erişim etkindir belirler.

|Anahtar|Açıklama|Sınırlar|  
|---------|-----------------|------------|  
|Yönetici|Hizmet yönetme özelliği de dahil tüm işlemleri için tüm haklara oluşturma ve silme verir **dizinler**, **dizin oluşturucular**, ve **veri kaynakları**.<br /><br /> İki Yönetim **api anahtarlarından**olarak başvurulan *birincil* ve *ikincil* anahtarları portalında, hizmet oluşturulduğunda ve tek tek isteğe bağlı olarak yeniden oluşturulur . İki anahtarına sahip bir anahtar sürekli Erişim hizmeti için ikinci anahtar kullanırken alma izin verir.<br /><br /> Yönetici anahtarları yalnızca HTTP istek üst bilgilerinde belirtilir. Bir yönetici yerleştirilemiyor **api anahtarını** bir URL.|En çok hizmeti başına 2|  
|Sorgu|Dizinler ve belgeler için salt okunur erişim verir ve genellikle, arama istekleri gönderen istemci uygulamalarına dağıtılır.<br /><br /> Sorgu anahtarları isteğe bağlı olarak oluşturulur. Bunları el ile Portalı'nda veya programlama aracılığıyla oluşturabilirsiniz [Yönetimi REST API](https://docs.microsoft.com/rest/api/searchmanagement/).<br /><br /> Sorgu anahtarları, arama, öneri veya arama işlemi için bir HTTP istek üstbilgisi olarak belirtilebilir. Alternatif olarak, bir sorgu anahtarı üzerinde bir URL parametre olarak geçirebilirsiniz. İstemci uygulamanızı istek nasıl formulates bağlı olarak, anahtar sorgu parametresi olarak geçirmek daha kolay olabilir:<br /><br /> `GET /indexes/hotels/docs?search=*&$orderby=lastRenovationDate desc&api-version=2016-09-01&api-key=A8DA81E03F809FE166ADDB183E9ED84D`|hizmeti başına 50|  

 Görsel olarak, bir yönetici anahtarını veya sorgu anahtarını arasında fark yoktur. Her iki anahtarı 32 rastgele oluşan dizeleri alfa sayısal karakter oluşturulur. Hangi türde bir anahtar uygulamanızda belirtildi, izleme kaybederseniz, şunları yapabilirsiniz [anahtar portal denetleyin](https://portal.azure.com) veya [REST API](https://docs.microsoft.com/rest/api/searchmanagement/) anahtar türü ve değeri döndürmek için.  

> [!NOTE]  
>  Hassas verileri gibi geçirmek için zayıf güvenlik uygulaması olarak kabul edilir bir `api-key` istek URI'SİNDEKİ. Bu nedenle, Azure Search yalnızca bir sorgu anahtarı olarak kabul eden bir `api-key` sorgu dizesi ve sürece dizininizi içeriğinin genel kullanıma açık olması gerekir böylece kaçınmalısınız. Genel kural olarak, geçirme öneririz, `api-key` bir istek üstbilgisi olarak.  

### <a name="how-to-find-the-access-keys-for-your-service"></a>Hizmetiniz için erişim anahtarları bulma

Erişim tuşları portalında veya aracılığıyla elde edebilirsiniz [Yönetimi REST API](https://docs.microsoft.com/rest/api/searchmanagement/). Daha fazla bilgi için bkz: [anahtarları Yönet](search-manage.md#manage-api-keys).

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Liste [arama hizmetleri](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) aboneliğiniz için.
3. hizmeti seçin ve hizmet sayfasında bulabilirsiniz **ayarları** >**anahtarları** yönetici ve sorgu anahtarları görüntülemek için.

![Portal sayfasında, ayarları, anahtarları bölümüne](media/search-security-overview/settings-keys.png)

## <a name="index-access"></a>Dizin Erişimi

Azure Search'te tek bir dizin güvenliği sağlanabilir nesne değil. Bunun yerine, bir dizine erişim katmanında hizmet (okuma veya yazma erişimi), bir işlem bağlamında birlikte belirlenir.

Son kullanıcı erişimi söz konusu olduğunda, herhangi bir istek salt okunur yapar ve uygulamanız tarafından kullanılan belirli dizin içeren bir sorgu anahtarı kullanarak bağlanmak için uygulamanızın sorgu isteği yapısı. Bir sorgu isteği dizinleri katılma veya tüm istekleri tek bir dizin tanımı tarafından hedef için aynı anda birden çok dizin erişme hiçbir kavramı yoktur. Bu nedenle, güvenlik sınırı sorgu isteği kendisi (bir anahtar artı tek hedef dizin) yapısını tanımlar.

Yönetici ve geliştirici erişimini dizinler için protokole: her ikisi de oluşturma, silme ve hizmet tarafından yönetilen nesneleri güncelleştirmek için yazma erişimi gerekir. Hizmetiniz için bir yönetici anahtarı kimseyle okuma, değiştirmek veya aynı hizmetindeki tüm dizini silin. Dizinleri yanlışlıkla veya kötü amaçlı silinmesini karşı koruma için şirket içi kaynak kodu varlıklar için bir istenmeyen dizin silinmesi veya değiştirilmesi ters çevirme için remedy denetimdir. Azure arama kullanılabilirliğini sağlamak için küme içindeki yük devretme gerekiyor, ancak bunu depolamaz veya oluşturmak veya dizinleri yüklemek için kullanılan özel kodunuzu yürütün.

Dizin düzeyinde güvenlik sınırları gerektiren RRAS'nin çözümleri için bu tür çözümler genellikle dizin yalıtım işlemek için hangi müşterilerin kullanmak bir orta katman içerir. Çok müşterili kullanım durumu hakkında daha fazla bilgi için bkz: [tasarım desenleri çok kiracılı SaaS uygulamaları ve Azure Search](search-modeling-multitenant-saas-applications.md).

## <a name="admin-access-from-client-apps"></a>İstemci uygulamalarından yönetici erişimi

Azure Search Yönetimi REST API'si, Azure Kaynak Yöneticisi'nin bir uzantısıdır ve bağımlılıklarını paylaşır. Bu nedenle, Active Directory Azure Search Hizmeti Yönetimi için bir önkoşuldur. İstemci kodu gelen tüm yönetim istekleri kaynak yöneticisi isteği erişmeden önce Azure Active Directory'yi kullanarak kimlik doğrulaması gerekir.

Azure Search Hizmeti uç noktası, Create Index (Azure Search Hizmeti REST API'si) ya da Search belgeleri (Azure Search Hizmeti REST API'si) gibi veri istekleri, istek üstbilgisinde bir API anahtarı kullanır.

Uygulama kodunuz hizmet yönetim işlemleri ve bunun yanı sıra veri işlemleri arama dizinlerini ya da belgelerine işliyorsa, kodunuzda iki kimlik doğrulama yaklaşımını uygulayın: Azure Search ve Active Directory kimlik doğrulaması için yerel erişim tuşu Kaynak Yöneticisi tarafından gerekli yöntemi. 

Azure Search'te bir istek yapılandırılması hakkında daha fazla bilgi için bkz: [Azure Search Hizmeti REST](https://docs.microsoft.com/rest/api/searchservice/). Kimlik doğrulama gereksinimleri için Kaynak Yöneticisi hakkında daha fazla bilgi için bkz: [erişim abonelikler için Kaynak Yöneticisi'ni kullanın kimlik doğrulaması API'sini](../azure-resource-manager/resource-manager-api-authentication.md).

## <a name="user-access-to-index-content"></a>Dizin içeriği kullanıcı erişimi

Dizin içeriğini kullanıcı başına erişim belgeleri belirli güvenlik kimlikle ilişkili döndürme sorgularınızı güvenlik filtreleri aracılığıyla uygulanır. Önceden tanımlanmış roller ve rol atamalarını yerine kimlik tabanlı erişim denetimi boşlukları arama sonuçları belgelerin ve içerik kimliği temel filtre olarak kullanılır. Aşağıdaki tabloda kırpma arama sonuçları yetkisiz içerik için iki yaklaşım açıklanmaktadır.

| Yaklaşım | Açıklama |
|----------|-------------|
|[Kimlik filtrelere göre güvenlik kırpma](search-security-trimming-for-azure-search.md)  | Kullanıcı Kimliği erişim denetimi uygulamak için temel iş akışının belgelenmiştir. Bir dizine ekleme güvenlik tanımlayıcıları kapsar ve yasaklanan içerik sonuçlarını kırpma için o alanı karşı filtreleme açıklar. |
|[Azure Active Directory kimliği temel güvenlik kırpma](search-security-trimming-for-azure-search-with-aad.md)  | Bu makalede Azure Active Directory (AAD gelen), aşağıdakilerden birini kimliklerini almak için adımlar sağlar önceki makale üzerinde genişletir [serbest Hizmetleri](https://azure.microsoft.com/free/) Azure bulut Platform. |

## <a name="table-permissioned-operations"></a>Tablo: Permissioned işlemleri

Aşağıdaki tabloda Azure Search'te izin işlemleri özetler ve hangi anahtar erişimi belirli bir işlem kilidini açar.

| İşlem | İzinler |
|-----------|-------------------------|
| Hizmet oluşturma | Azure aboneliğinin sahibi|
| Bir hizmeti ölçeklendirmek | Yönetici anahtarını, RBAC sahibi veya katkıda kaynağı  |
| Bir hizmeti silin | Yönetici anahtarını, RBAC sahibi veya katkıda kaynağı |
| Oluşturma, değiştirme, hizmet nesnelerde Sil: <br>Dizinler ve bileşen bölümleri (dahil olmak üzere Çözümleyicisi tanımları, Puanlama profilleri, CORS seçenekleri), dizin oluşturucular, veri kaynakları, eş anlamlıları, ilgili. | Yönetici anahtarını, RBAC sahibi veya katkıda kaynağı  |
| Bir dizini sorgulama | Yönetici veya sorgu anahtarı (RBAC uygulanamaz) |
| Sorgu istatistiklerini, sayıları ve nesneleri listesi döndüren gibi sistem bilgisi | Yönetici anahtarını, kaynak (sahibi, katkıda bulunan, okuyucu) RBAC |
| Yönetici anahtarları Yönet | RBAC sahibi veya katkıda kaynak üzerinde yönetici anahtarı. |
| Sorgu anahtarlarını yönet |  RBAC sahibi veya katkıda kaynak üzerinde yönetici anahtarı. RBAC okuyucu sorgu anahtarları görüntüleyebilirsiniz. |


## <a name="see-also"></a>Ayrıca bkz.

+ [Al (gösteren bir dizin oluşturmak için bir yönetici anahtarını kullanarak) .NET başlatıldı](search-create-index-dotnet.md)
+ [Al (gösteren bir dizin oluşturmak için bir yönetici anahtarını kullanarak) REST başlatıldı](search-create-index-rest-api.md)
+ [Azure Search filtreleri kullanarak kimlik tabanlı erişim denetimi](search-security-trimming-for-azure-search.md)
+ [Azure Search filtreleri kullanarak active Directory kimlik tabanlı erişim denetimi](search-security-trimming-for-azure-search-with-aad.md)
+ [Azure Search'te filtreleri](search-filters.md)