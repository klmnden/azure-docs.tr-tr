---
title: Güvenlik ve veri gizlilik - Azure Search
description: Azure arama, SOC 2, HIPAA ve diğer sertifikaları ile uyumludur. Bağlantı ve veri şifreleme, kimlik doğrulaması ve kimlik erişimi aracılığıyla kullanıcı ve grup güvenlik tanımlayıcıları Azure Search'te filtreler.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 55558f1483a576e7ac3b9ce027588eceabd5db70
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53311720"
---
# <a name="security-and-data-privacy-in-azure-search"></a>Azure Search'teki güvenlik ve veri gizliliği

Bu şekilde içeriğin özel kalmasını sağlamak için Azure Search'e kapsamlı güvenlik özellikleri ve erişim denetimleri oluşturulur. Bu makalede, Azure Search'e yerleşik güvenlik özellikleri ve standartları uyumluluk numaralandırır.

Azure arama güvenlik mimarisi, fiziksel güvenlik, şifrelenmiş iletimleri, şifrelenmiş depolama ve platform genelinde standartlara uyum yayılır. İşletimsel olarak, Azure Search yalnızca kimliği doğrulanmış istekleri kabul eder. İsteğe bağlı olarak, güvenlik filtreleri içerikler üzerinde kullanıcı başına erişim denetimlerini ekleyebilirsiniz. Bu makalede güvenlik her katmanında ele alınır, ancak öncelikle nasıl Azure Search'te verileri ve işlemleri güvenlidir odaklanmıştır.

## <a name="standards-compliance-iso-27001-soc-2-hipaa"></a>Standartları uyumluluğu: ISO 27001, SOC 2, HIPAA

Azure Search'ü sertifika aşağıdaki standartları, olarak [Haziran 2018'de duyurulan](https://azure.microsoft.com/blog/azure-search-is-now-certified-for-several-levels-of-compliance/):

+ [ISO 27001: 2013](https://www.iso.org/isoiec-27001-information-security.html) 
+ [SOC 2 tip 2 Uyumluluk](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/aicpasoc2report.html) tam raporu için Git [- Azure ve Azure kamu SOC 2 tür II raporu](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports). 
+ [Sağlık Sigortası Taşınabilirlik ve Sorumluluk Yasası (HIPAA)](https://en.wikipedia.org/wiki/Health_Insurance_Portability_and_Accountability_Act)
+ [GxP (21 CFR Kısım 11)](https://en.wikipedia.org/wiki/Title_21_CFR_Part_11)
+ [HITRUST](https://en.wikipedia.org/wiki/HITRUST)
+ [PCI DSS düzey 1](https://en.wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard)
+ [Avustralya IRAP Sınıflandırılmamış DLM](https://asd.gov.au/infosec/irap/certified_clouds.htm)

Standartlara uyum genel kullanıma sunulan özellikleri için geçerlidir. Önizleme özellikleri, genel kullanılabilirliğe geçiş ve katı standartlar gereksinimlerine sahip çözümler kullanılmamalıdır onaylanır. Uyumluluk sertifikası belirtilmiştir [Microsoft Azure genel bakış Uyumluluk](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) ve [Güven Merkezi](https://www.microsoft.com/en-us/trustcenter). 

## <a name="encrypted-transmission-and-storage"></a>Şifrelenmiş iletim ve depolama

Şifreleme tüm dizinleme işlem hattına genişletir: bağlantılardan iletim ve Azure Search'te depolanan dizinlenmiş verileri gösteriyor.

| Güvenlik katmanı | Açıklama |
|----------------|-------------|
| Aktarım sırasında şifreleme <br>(HTTPS/SSL/TLS) | Azure arama, HTTPS bağlantı noktası 443'ü dinler. Bir platform Azure hizmetlerine bağlantılar şifrelenir. <br/><br/>Tüm istemci hizmeti Azure Search etkileşimleri SSL/TLS 1.2 özellikli olan.  Hizmetinize SSL bağlantıları için TLSv1.2 kullandığınızdan emin olun.|
| Bekleme sırasında şifreleme | Şifreleme tam dizin oluşturma işleminde, dizin oluşturma zamanı tamamlama veya dizin boyutu ölçülebilir etkilemeden internalized. Otomatik olarak tüm dizin üzerinde (Ocak 2018'den önce oluşturulan) olmayan tam olarak şifrelendiğinden bir dizin için artımlı güncelleştirmeleri dahil olmak üzere oluşur.<br><br>Dahili olarak, şifreleme dayanır [Azure depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption), 256 bit kullanarak [AES şifreleme](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard).|

Şifreleme sertifikaları ve dahili olarak Microsoft tarafından yönetilen ve evrensel uygulanan şifreleme anahtarları ile Azure Search için dahili kullanım içindir. Olamaz şifreleme Aç veya Kapat, yönetme veya kendi anahtarlarınızı değiştirin veya portalı veya programlama yoluyla şifreleme ayarlarını görüntüleyin. 

Bekleme sırasında şifreleme 24 Ocak 2018'de duyurulan ve paylaşılan (ücretsiz) Hizmetleri tüm bölgelerde de dahil olmak üzere tüm hizmet katmanları için geçerlidir. Tam şifreleme için bu tarihten önce oluşturulan dizinleri bırakılan ve şifrelemenin gerçekleşmesi için sırayla yeniden. Aksi takdirde, Ocak 24 sonra eklenen yalnızca yeni veriler şifrelenir.

## <a name="azure-wide-user-access-controls"></a>Azure genelinde kullanıcı erişim denetimleri

Kullanılabilir Azure genelinde birden fazla güvenlik mekanizmasıdır ve bu nedenle otomatik olarak kullanılabilir olan Azure Search kaynaklara oluşturun.

+ [Abonelik veya kaynak düzeyinde silinmesini engellemek için kilitler](../azure-resource-manager/resource-group-lock-resources.md)
+ [Rol tabanlı erişim denetimi (bilgilerine ve yönetim işlemlerini erişimi denetlemek için RBAC)](../role-based-access-control/overview.md)

Tüm Azure Hizmetleri, erişim düzeylerini sürekli olarak tüm hizmetler arasında ayarlamak için rol tabanlı erişim denetimlerine (RBAC) destekler. Hizmet durumunu görüntüleyerek herhangi bir rol, üyelerine kullanılabilir ise, yönetici anahtarı gibi hassas verilerin görüntüleme sahibi ve katkıda bulunan rollerine sınırlıdır. RBAC, sahibi, katkıda bulunan ve okuyucu rolleri sağlar. Varsayılan olarak, tüm hizmet yöneticileri sahip rolünün üyesidir.

## <a name="service-access-and-authentication"></a>Hizmet erişim ve kimlik doğrulaması

Azure Search, Azure platformunun güvenlik önlemlerinin devralır, ancak aynı zamanda kendi anahtar tabanlı kimlik doğrulamasını sağlar. Bir API anahtarı, rastgele oluşturulmuş bir sayı ile harflerden oluşan bir dizedir. (Yönetici veya sorgu) anahtar türünü erişim düzeyini belirler. Geçerli bir anahtar teslimini kavram isteği güvenilir bir varlıktan kaynağı olarak kabul edilir. Anahtarları iki tür arama hizmetinize erişmek için kullanılır:

* Yönetici (hizmet karşı herhangi bir okuma-yazma işlemi için geçerli)
* Sorgu (geçerli dizin sorguları gibi salt okunur işlemler için)

Hizmetin sağlandığı yönetici anahtarları oluşturulur. Olarak belirlenen iki yönetici anahtarı mevcuttur *birincil* ve *ikincil* bunları tutmak düz, ancak aslında bunlar değiştirilebilir. Böylece, bir hizmete erişimi kaybetmeden dönebileceğinizden her hizmetin iki yönetici anahtarı vardır. İki yönetici anahtarı yeniden oluşturabilirsiniz ancak yönetici toplam anahtar sayısı için ekleyemezsiniz. İki yönetici anahtarı arama hizmeti başına en fazla yoktur.

Sorgu anahtarları gerektiğinde oluşturulur ve arama doğrudan çağıran istemci uygulamalar için tasarlanmıştır. En çok 50 sorgu anahtarları oluşturabilirsiniz. Uygulama kodunda hizmeti salt okunur erişime izin vermek için arama URL'sini ve bir sorgu api anahtarını belirtin. Uygulama kodunuz aynı zamanda uygulamanız tarafından kullanılan dizinini belirtir. Uç nokta, salt okunur erişim için bir API anahtarı ve bir hedef dizin birlikte, istemci uygulamanızın bağlantı kapsamı ve erişim düzeyini tanımlayın.

Her istekte zorunlu bir anahtar, bir işlem ve bir nesne her isteğin burada oluşur, kimlik doğrulaması gereklidir. Birbirine zincirlenmiş, iki izin düzeyleri (tam veya salt okunur) artı (örneğin, bir dizin üzerinde sorgu işlemi) bağlam hizmet işlemleri üzerinde tam spektrumlu güvenlik sağlamak için yeterlidir. Anahtarları hakkında daha fazla bilgi için bkz. [oluştur ve api anahtarlarını yönetebilirsiniz](search-security-api-keys.md).

## <a name="index-access"></a>Dizin Erişimi

Azure Search'te, tek bir dizin güvenli kılınabilir nesne değil. Bunun yerine, bir dizin için erişim, bir işlem bağlamında yanı sıra bir hizmet katmanında (okuma veya yazma erişimini) belirlenir.

Son kullanıcı erişimi için herhangi bir istek salt okunur yapar ve uygulamanız tarafından kullanılan belirli dizin içeren bir sorgu anahtarı kullanarak bağlanmak için sorgu isteği yapısını. Bir sorgu istekte dizinleri katılma veya tüm istekler tek bir dizin tanımı tarafından hedef için aynı anda birden çok dizin erişme konsepti yoktur. Bu nedenle, oluşumunu sorgu isteği kendisi (bir anahtarı ve tek bir hedef dizin) güvenlik sınırı tanımlar.

Yönetici ve geliştirici erişimini dizinler için protokole: her ikisini de oluşturma, silme ve güncelleştirme hizmet tarafından yönetilen nesneler için yazma erişimi gerekir. Hizmetinize bir yönetici anahtarı kimseyle okuma, değiştirme veya silme aynı hizmetindeki herhangi bir dizinde. Dizinleri kötü amaçlı veya yanlışlıkla silinmeye karşı koruma için şirket içi kaynak kod varlıklarına için istenmeyen dizin silinmesi veya değiştirilmesi gibi bir ters için telafi denetiminizdir. Azure Search'ü kullanılabilirliğini sağlamak için küme içindeki yük devretme sahiptir, ancak bunu depolamaz veya oluşturmak veya dizinleri yüklemek için kullanılan özel kodunuzun yürütme neden olabilir.

Dizin düzeyinde güvenlik sınırları gerektiren çoklu müşteri mimarisi çözümleri için bu tür çözümler genellikle dizin yalıtım işlemek için hangi müşteriler kullanmak, bir orta katman içerir. Çok kiracılı bir kullanım örneği hakkında daha fazla bilgi için bkz: [çok kiracılı SaaS uygulamaları ve Azure Search için Tasarım Düzenleri](search-modeling-multitenant-saas-applications.md).

## <a name="admin-access-from-client-apps"></a>İstemci uygulamalardan yönetici erişimi

Azure arama yönetimi REST API'si, Azure Resource Manager'ın bir uzantısıdır ve bağımlılıklarını paylaşır. Bu nedenle, Active Directory, Azure Search Hizmeti Yönetimi için bir önkoşuldur. İstemci kodundan tüm yönetimsel istekler Resource Manager istek ulaşmadan önce Azure Active Directory'yi kullanarak kimlik doğrulaması gerekir.

Azure Search Hizmeti uç noktası, dizin oluşturma (Azure Search Hizmeti REST API'si) ya da Search belgeleri (Azure Search Hizmeti REST API'si) gibi veri istekler istek üst bilgisinde bir API anahtarı kullanın.

Uygulama kodunuz, hizmet yönetim işlemleri ve bunun yanı sıra arama dizinleri veya belgeler üzerinde veri işlemleri işliyorsa, kodunuzda iki kimlik doğrulama yaklaşımını uygulamak: Azure Search ve Active Directory kimlik doğrulaması için yerel erişim anahtarı Resource Manager tarafından istenen yöntemi. 

Azure Search'te bir istek yapılandırılması hakkında daha fazla bilgi için bkz: [Azure arama hizmeti REST](https://docs.microsoft.com/rest/api/searchservice/). Resource Manager için kimlik doğrulama gereksinimleri hakkında daha fazla bilgi için bkz. [aboneliklere erişmek için Kaynak Yöneticisi'ni kullanın kimlik doğrulama API'si](../azure-resource-manager/resource-manager-api-authentication.md).

## <a name="user-access-to-index-content"></a>Dizin içeriğe kullanıcı erişimi

Dizin içeriğini kullanıcı başına erişim belgeleri belirli güvenlik kimlikle ilişkili döndüren sorgularınızı güvenlik filtreleriyle aracılığıyla uygulanır. Önceden tanımlanmış roller ve rol atamalarını yerine kimlik tabanlı erişim denetimi, arama sonuçlarını, belgeleri ve içerik kimliği temel kırpar filtre olarak uygulanır. Aşağıdaki tabloda, yetkisiz içeriği kırpma arama sonuçları için iki yaklaşım açıklanmaktadır.

| Yaklaşım | Açıklama |
|----------|-------------|
|[Kimlik filtreleri temel alarak güvenlik kırpma](search-security-trimming-for-azure-search.md)  | Kullanıcı Kimliği erişim denetimi uygulamak için temel iş akışı belgeler. Bir dizine eklemeyi güvenlik tanımlayıcıları kapsar ve ardından sonuçları yasaklanmış içeriğinin kırpmak için bu alanı karşı filtreleme açıklar. |
|[Azure Active Directory kimliği temel güvenlik kırpma](search-security-trimming-for-azure-search-with-aad.md)  | Bu makalede Azure Active Directory (AAD dan), biri kimliklerini almak için adımlar sağlar. önceki bir makalede genişletir. [Ücretsiz Hizmetler](https://azure.microsoft.com/free/) Azure bulut platformunda. |

## <a name="table-permissioned-operations"></a>Tablo: Permissioned işlemleri

Aşağıdaki tablo, Azure Search'te izin verilen işlemleri özetler ve hangi anahtar erişimi belirli bir işlem kilidini açar.

| İşlem | İzinler |
|-----------|-------------------------|
| Hizmet oluşturma | Azure aboneliği sahibi|
| Hizmet ölçeklendirme | RBAC sahibi veya katkıda bulunan kaynak üzerinde yönetici anahtarı  |
| Bir hizmeti Sil | RBAC sahibi veya katkıda bulunan kaynak üzerinde yönetici anahtarı |
| Oluşturma, değiştirme, hizmette nesneleri silin: <br>Dizinleri ve bileşen parçalarına (dahil olmak üzere Çözümleyicisi tanımları, Puanlama profillerini, CORS seçenekleri), dizin oluşturucular, veri kaynakları, eş anlamlılar, öneri araçları. | RBAC sahibi veya katkıda bulunan kaynak üzerinde yönetici anahtarı  |
| Dizin sorgulama | Yönetici veya sorgu anahtarı (RBAC uygulanamaz) |
| İstatistikleri, sayıları ve nesnelerin bir listesini döndüren gibi sistem bilgilerini sorgulayın. | Yönetici anahtarı, RBAC kaynak (sahibi, katkıda bulunan, okuyucu) |
| Yönetici anahtarları yönetme | Yönetici anahtarı, RBAC sahibi veya katkıda bulunan kaynaklar. |
| Sorgu anahtarlarını yönet |  Yönetici anahtarı, RBAC sahibi veya katkıda bulunan kaynaklar.  |

## <a name="physical-security"></a>Fiziksel güvenlik

Microsoft veri merkezleri, sektör öncüsü fiziksel güvenlik sağlayın ve standartları ve düzenlemeleriyle, kapsamlı bir portföyüyle uyumludur. Daha fazla bilgi için Git [küresel veri merkezleri](https://www.microsoft.com/cloud-platform/global-datacenters) sayfasında veya Güvenlik Merkezi veri çubuğunda kısa bir video izleyin.

> [!VIDEO https://www.youtube.com/embed/r1cyTL8JqRg]


## <a name="see-also"></a>Ayrıca bkz.

+ [Al (gösteren bir dizin oluşturmak için bir yönetici anahtarını kullanarak) .NET başlatıldı](search-create-index-dotnet.md)
+ [Al (gösteren bir dizin oluşturmak için bir yönetici anahtarını kullanarak) REST başlatıldı](search-create-index-rest-api.md)
+ [Azure arama filtreleri kullanarak kimlik tabanlı erişim denetimi](search-security-trimming-for-azure-search.md)
+ [Azure arama filtreleri kullanarak active Directory kimlik tabanlı erişim denetimi](search-security-trimming-for-azure-search-with-aad.md)
+ [Azure Search'te filtreler](search-filters.md)