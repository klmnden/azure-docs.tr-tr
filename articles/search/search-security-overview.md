---
title: Verilerin ve işlemlerin Azure Search'te güvenli | Microsoft Docs
description: Azure arama güvenlik SOC 2 uyumluluk, şifreleme, kimlik doğrulama ve kullanıcı ve grup güvenlik tanımlayıcıları Azure Search filtrelerde aracılığıyla kimlik erişimi temel alır.
services: search
documentationcenter: ''
author: HeidiSteen
manager: cgronlun
editor: ''
ms.assetid: ''
ms.service: search
ms.devlang: ''
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/19/2018
ms.author: heidist
ms.openlocfilehash: 95c1f209d51093c3f2bf2555f987983a85f2bf09
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="security-and-controlled-access-in-azure-search"></a>Güvenlik ve Azure Search'te denetimli erişim

Azure arama [SOC 2 uyumlu](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports)kapsamlı güvenlik mimarisi yayılan fiziksel güvenlik, şifrelenmiş iletimler, şifrelenmiş depolama ve platform genelinde yazılım koruması. İşletimsel olarak, Azure Search yalnızca kimliği doğrulanmış istekler kabul eder. İsteğe bağlı olarak, içerik üzerinde kullanıcı başına erişim denetimleri ekleyebilirsiniz. Bu makalede her katmanda güvenlik dokunur, ancak öncelikle nasıl Azure Search'te verilerin ve işlemlerin güvenlidir odaklanmıştır.

![Güvenlik katmanları Blok Diyagramı](media/search-security-overview/azsearch-security-diagram.png)

## <a name="physical-security"></a>Fiziksel güvenlik

Microsoft veri merkezleri endüstri lideri fiziksel güvenlik sağlar ve kapsamlı bir standartlar ve düzenlemelere yelpazesini ile uyumludur. Daha fazla bilgi edinmek için şu adrese gidin [küresel veri merkezleri](https://www.microsoft.com/cloud-platform/global-datacenters) sayfa veya Güvenlik Merkezi veri hakkında kısa bir video izleyin.

> [!VIDEO https://www.youtube.com/embed/r1cyTL8JqRg]

## <a name="encrypted-transmission-and-storage"></a>Şifrelenmiş iletim ve depolama

Şifreleme tüm dizini oluşturma ardışık düzeni genişletir: bağlantılardan, iletim aracılığıyla ve Azure Search'te depolanan dizinlenmiş veri aşağı kaydırın.

| Güvenlik katmanı | Açıklama |
|----------------|-------------|
| Aktarımdaki şifreleme | Azure arama HTTPS bağlantı noktası 443 üzerinde dinler. Platform arasında Azure hizmetlerine bağlantıları şifrelenir. |
| Bekleme sırasında şifreleme | Şifreleme dizin oluşturma işleminde, dizin oluşturma süresi tamamlama veya dizin boyutu ölçülebilir bir etki olmadan tam olarak internalized. Otomatik olarak tüm dizin üzerinde (Ocak 2018 önce oluşturulan) tam olarak şifrelenmemiş bir dizine artımlı güncelleştirmeler dahil olmak üzere oluşur.<br><br>Dahili olarak, şifreleme dayanır [Azure depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption), 256 bit kullanarak [AES şifreleme](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard).|
| [SOC 2 uyumluluk](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/aicpasoc2report.html) | Tüm arama hizmetleri tam olarak AICPA SOC 2 uyumlu, Azure Search sağlayan tüm veri merkezlerinde oluşturulur. Tam raporu gözden geçirmek için Git [Azure - ve Azure kamu SOC 2 türü II rapor](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports). |

Şifreleme sertifikaları ve dahili olarak Microsoft tarafından yönetilen ve evrensel uygulanması şifreleme anahtarları ile Azure arama için dahili kullanım içindir. Olamaz şifreleme Aç veya Kapat, yönetme veya kendi anahtarları yerine veya portal veya program aracılığıyla şifreleme ayarları görüntüleyin. 

Bekleyen şifreleme 24 Ocak 2018 içinde duyurulan ve tüm bölgelerde paylaşılan (ücretsiz) Hizmetleri dahil olmak üzere tüm hizmet katmanları için geçerlidir. Tam şifreleme için bu tarihten önce oluşturulan dizinleri bırakılan ve şifrelemenin gerçekleşmesi için sırayla yeniden. Aksi takdirde, 24 Ocak sonra eklenen yalnızca yeni veriler şifrelenir.

## <a name="azure-wide-logical-security"></a>Azure genelinde mantıksal güvenliği

Birkaç güvenlik mekanizmaları Azure yığın üzerinden kullanılabilen ve bu nedenle otomatik olarak oluşturduğunuz Azure Search kaynaklar için kullanılabilir.

+ [Abonelik ya da silinmesini önlemek için kaynak düzeyi kilitleri](../azure-resource-manager/resource-group-lock-resources.md)
+ [Rol tabanlı erişim denetimi (bilgi ve yönetim işlemleri erişimi denetlemek için RBAC)](../role-based-access-control/overview.md)

Tüm Azure hizmetlerine erişim düzeyleri tutarlı bir şekilde tüm hizmetler arasında ayarlamak için rol tabanlı erişim denetimi (RBAC) destekler. Hizmet durumunu görüntüleyerek herhangi bir rol, üyelerine kullanılabilir iken Örneğin, yönetici anahtarı gibi hassas verileri görüntüleme sahibi ve katkıda bulunan rollere sınırlıdır. RBAC sahibi, katkıda bulunan ve okuyucu rolü sağlar. Varsayılan olarak, tüm hizmet yöneticileri sahip rolünün bir üyesi.

## <a name="service-access-and-authentication"></a>Hizmet erişim ve doğrulama

Azure Search Azure platformu güvenlik önlemleri devralır olsa da, ayrıca kendi anahtar tabanlı kimlik doğrulaması sağlar. Bir API anahtarı rastgele oluşturulmuş sayılar ve harflerden oluşan bir dizedir. (Yönetici veya sorgu) anahtar türü erişim düzeyini belirler. Geçerli bir anahtar teslimini güvenilir bir varlıktan kanıt İsteğin kaynaklandığı olarak kabul edilir. İki tür anahtarları, arama hizmetinize erişmek için kullanılır:

* Yönetici (hizmet karşı herhangi bir okuma-yazma işlemi için geçerli)
* Sorgu (geçerli bir dizin sorgular gibi salt okunur işlemler için)

Hizmet sağladığında yönetici anahtarları oluşturulur. Olarak belirtilen iki yönetici anahtarları vardır *birincil* ve *ikincil* bunları tutmak için doğrudan, ancak aslında bunlar birbirinin yerine kullanılabilir. Böylece, bir hizmete erişimi kaybetmeden dönebilirsiniz her hizmetin iki yönetici anahtarları vardır. Her iki yönetici anahtarını yeniden oluşturmak, ancak toplam yönetici anahtar sayısı ekleyemezsiniz. En fazla arama hizmeti başına iki yönetici anahtarları yoktur.

Sorgu anahtarları gerektiği oluşturulur ve arama doğrudan çağıran istemci uygulamalar için tasarlanmıştır. En fazla 50 sorgu anahtarları oluşturabilirsiniz. Uygulama kodunda arama URL'sini ve bir sorgu api anahtarını hizmet salt okunur erişime izin vermek için belirtin. Uygulama kodunuz, ayrıca, uygulamanız tarafından kullanılan dizini belirtir. Birlikte, uç noktası, salt okunur erişim için bir API anahtarı ve bir hedef dizin istemci uygulamanızı bağlantısından kapsam ve erişim düzeylerini tanımlayın.

Her istekte her istek zorunlu bir anahtar, bir işlem ve bir nesne burada oluşur, kimlik doğrulaması gereklidir. Birbirine zincirlenmiş, iki izin düzeyleri (tam veya salt okunur) artı (örneğin, sorgu üzerinde bir işlemi dizin) bağlam hizmet işlemleri üzerinde tam spektrumun güvenlik sağlamak için yeterlidir. Anahtarlar hakkında daha fazla bilgi için bkz: [oluşturma ve API anahtarlarını Yönet](search-security-api-keys.md).

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
| Sorgu anahtarlarını yönet |  RBAC sahibi veya katkıda kaynak üzerinde yönetici anahtarı.  |


## <a name="see-also"></a>Ayrıca bkz.

+ [Al (gösteren bir dizin oluşturmak için bir yönetici anahtarını kullanarak) .NET başlatıldı](search-create-index-dotnet.md)
+ [Al (gösteren bir dizin oluşturmak için bir yönetici anahtarını kullanarak) REST başlatıldı](search-create-index-rest-api.md)
+ [Azure Search filtreleri kullanarak kimlik tabanlı erişim denetimi](search-security-trimming-for-azure-search.md)
+ [Azure Search filtreleri kullanarak active Directory kimlik tabanlı erişim denetimi](search-security-trimming-for-azure-search-with-aad.md)
+ [Azure Search'te filtreleri](search-filters.md)