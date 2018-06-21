---
title: Güvenlik ve veri gizlilik Azure Search'te | Microsoft Docs
description: Azure arama, SOC 2, HIPAA ve diğer sertifikaları ile uyumludur. Bağlantı ve veri şifreleme, kimlik doğrulama ve kimlik erişim kullanıcı ve grup güvenlik tanımlayıcıları Azure Search'te aracılığıyla filtreler.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: heidist
ms.openlocfilehash: 888f7c3ced0ef48cff222bffdbf0f278fa5f42b3
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36285738"
---
# <a name="security-and-data-privacy-in-azure-search"></a>Azure Search'te güvenlik ve veri gizliliği

Özel içerik bu şekilde kaldığından emin olmak için Azure Search kapsamlı güvenlik özellikleri ve erişim denetimleri oluşturulmuştur. Bu makalede, Azure Search yerleşik güvenlik özellikleri ve standartları uyumluluk numaralandırır.

Azure arama güvenlik mimarisi fiziksel güvenliği, şifrelenmiş iletimler, şifrelenmiş depolama ve platform genelinde standartları uyumluluğu yayar. İşletimsel olarak, Azure Search yalnızca kimliği doğrulanmış istekler kabul eder. İsteğe bağlı olarak, güvenlik filtreleri yoluyla içerik üzerinde kullanıcı başına erişim denetimleri ekleyebilirsiniz. Bu makalede her katmanda güvenlik dokunur, ancak öncelikle nasıl Azure Search'te verilerin ve işlemlerin güvenlidir odaklanmıştır.

## <a name="standards-compliance-iso-27001-soc-2-hipaa"></a>Standartları uyumluluğu: ISO 27001, SOC 2, HIPAA

Standartları uyumluluğu kısmi bir listesine SOC 2 Tür 2 ve HIPAA için genel olarak kullanılabilir özellikler içerir. Önizleme özellikleri genel kullanılabilirlik bir parçası olarak sertifikalı ve belirli standartlara gereksinimlerine sahip çözümlerinde kullanılmamalıdır. Uyumluluk sertifika bölümlerinde [Microsoft Azure genel bakış Uyumluluk](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) ve [Güven Merkezi](https://www.microsoft.com/en-us/trustcenter). 

Sertifika aşağıdaki standartları için olan [Haziran 2018 duyurdu](https://azure.microsoft.com/blog/azure-search-is-now-certified-for-several-levels-of-compliance/).

+ [ISO 27001: 2013](https://www.iso.org/isoiec-27001-information-security.html) 
+ [SOC 2 Tür 2 Uyumluluk](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/aicpasoc2report.html) tam rapor için Git [Azure - ve Azure kamu SOC 2 türü II rapor](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports). 
+ [Sağlık Sigortası Taşınabilirlik ve Sorumluluk Yasası (HIPAA)](https://en.wikipedia.org/wiki/Health_Insurance_Portability_and_Accountability_Act)
+ [GxP (21 CFR Kısım 11)](https://en.wikipedia.org/wiki/Title_21_CFR_Part_11)
+ [HITRUST](https://en.wikipedia.org/wiki/HITRUST)
+ [PCI DSS düzey 1](https://en.wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard)
+ [Avustralya IRAP Sınıflandırılmamış DLM](https://asd.gov.au/infosec/irap/certified_clouds.htm)

## <a name="encrypted-transmission-and-storage"></a>Şifrelenmiş iletim ve depolama

Şifreleme tüm dizini oluşturma ardışık düzeni genişletir: bağlantılardan, iletim aracılığıyla ve Azure Search'te depolanan dizinlenmiş veri aşağı kaydırın.

| Güvenlik katmanı | Açıklama |
|----------------|-------------|
| Aktarımdaki şifreleme | Azure arama HTTPS bağlantı noktası 443 üzerinde dinler. Platform arasında Azure hizmetlerine bağlantıları şifrelenir. |
| Bekleme sırasında şifreleme | Şifreleme dizin oluşturma işleminde, dizin oluşturma süresi tamamlama veya dizin boyutu ölçülebilir bir etki olmadan tam olarak internalized. Otomatik olarak tüm dizin üzerinde (Ocak 2018 önce oluşturulan) tam olarak şifrelenmemiş bir dizine artımlı güncelleştirmeler dahil olmak üzere oluşur.<br><br>Dahili olarak, şifreleme dayanır [Azure depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption), 256 bit kullanarak [AES şifreleme](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard).|

Şifreleme sertifikaları ve dahili olarak Microsoft tarafından yönetilen ve evrensel uygulanması şifreleme anahtarları ile Azure arama için dahili kullanım içindir. Olamaz şifreleme Aç veya Kapat, yönetme veya kendi anahtarları yerine veya portal veya program aracılığıyla şifreleme ayarları görüntüleyin. 

Bekleyen şifreleme 24 Ocak 2018 içinde duyurulan ve tüm bölgelerde paylaşılan (ücretsiz) Hizmetleri dahil olmak üzere tüm hizmet katmanları için geçerlidir. Tam şifreleme için bu tarihten önce oluşturulan dizinleri bırakılan ve şifrelemenin gerçekleşmesi için sırayla yeniden. Aksi takdirde, 24 Ocak sonra eklenen yalnızca yeni veriler şifrelenir.

## <a name="azure-wide-user-access-controls"></a>Azure genelinde kullanıcı erişim denetimleri

Birkaç güvenlik mekanizmaları kullanılabilir Azure kapsamındadır ve bu nedenle otomatik olarak kullanılabilir Azure Search kaynaklar için oluşturun.

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

Son kullanıcı erişimi için herhangi bir istek salt okunur yapar ve uygulamanız tarafından kullanılan belirli dizin içeren bir sorgu anahtarı kullanarak bağlanmak için bir sorgu isteği yapısı. Bir sorgu isteği dizinleri katılma veya tüm istekleri tek bir dizin tanımı tarafından hedef için aynı anda birden çok dizin erişme hiçbir kavramı yoktur. Bu nedenle, sorgu isteği kendisi (bir anahtar artı tek hedef dizin) yapımı güvenlik sınırını tanımlar.

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

## <a name="physical-security"></a>Fiziksel güvenlik

Microsoft veri merkezleri endüstri lideri fiziksel güvenlik sağlar ve kapsamlı bir standartlar ve düzenlemelere yelpazesini ile uyumludur. Daha fazla bilgi edinmek için şu adrese gidin [küresel veri merkezleri](https://www.microsoft.com/cloud-platform/global-datacenters) sayfa veya Güvenlik Merkezi veri hakkında kısa bir video izleyin.

> [!VIDEO https://www.youtube.com/embed/r1cyTL8JqRg]


## <a name="see-also"></a>Ayrıca bkz.

+ [Al (gösteren bir dizin oluşturmak için bir yönetici anahtarını kullanarak) .NET başlatıldı](search-create-index-dotnet.md)
+ [Al (gösteren bir dizin oluşturmak için bir yönetici anahtarını kullanarak) REST başlatıldı](search-create-index-rest-api.md)
+ [Azure Search filtreleri kullanarak kimlik tabanlı erişim denetimi](search-security-trimming-for-azure-search.md)
+ [Azure Search filtreleri kullanarak active Directory kimlik tabanlı erişim denetimi](search-security-trimming-for-azure-search-with-aad.md)
+ [Azure Search'te filtreleri](search-filters.md)