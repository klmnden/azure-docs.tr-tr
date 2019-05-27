---
title: Azure Depolamaya Giriş - Azure’da bulut depolama | Microsoft Docs
description: Azure Depolama, Microsoft’un bulut depolama çözümüdür. Azure Depolama yüksek oranda kullanılabilir, güvenli, sağlam, yüksek düzeyde ölçeklenebilir ve yedekli veri nesneleri için depolama alanı sağlar.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: b03197fb0d4d97756e5a3fe88de78c11a816b5ec
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65951279"
---
# <a name="introduction-to-azure-storage"></a>Azure Depolama’ya Giriş

Azure Depolama, Microsoft’un modern veri depolama senaryolarına yönelik bulut depolama çözümüdür. Azure Depolama; veri nesneleri için yüksek düzeyde ölçeklenebilir bir nesne deposu, bulut için bir dosya sistemi hizmeti ve güvenilir mesajlaşma için mesajlaşma deposunun yanı sıra bir NoSQL deposu sunar. Azure Depolama şu özelliklere sahiptir:

- **Dayanıklı ve yüksek oranda kullanılabilir.** Yedeklilik, verilerinizin geçici donanım hataları durumunda güvende kalmasını sağlar. Ayrıca, yerel felaketler ya da doğal afetlere karşı ek koruma için verileri meri merkezleri ya da coğrafi bölgeler arasında çoğaltmayı seçebilirsiniz. Bu şekilde çoğaltılan veriler beklenmeyen bir kesinti durumunda yüksek oranda kullanılabilir olmaya devam eder. 
- **Güvenlik.** Azure Depolama’ya yazılan tüm veriler hizmet tarafından şifrelenir. Azure Depolama, verilerinize erişmesi gereken kişiler üzerinde ayrıntılı denetime sahip olmanızı sağlar.
- **Ölçeklenebilir.** Azure Depolama, günümüzün uygulamalarına ait veri depolama ve performans gereksinimlerini karşılamak için yüksek düzeyde ölçeklenebilir şekilde tasarlanmıştır. 
- **Yönetilen.** Microsoft Azure, donanım bakım, güncelleştirmeleri ve kritik sorunları sizin yerinize çözer.
- **Erişilebilir.** Azure Depolama’daki verilere dünyanın herhangi bir yerinden HTTP ya da HTTPS aracılığıyla erişilebilir. Microsoft, Azure Depolama için .NET, Java, Node.js, Python, PHP, Ruby, Go ve diğerleri gibi çeşitli dillerde SDK’lar ve olgun bir REST API’si sağlar. Azure Depolama, Azure PowerShell veya Azure CLI’de betik oluşturmayı destekler. Azure portalı ve Azure Depolama Gezgini ise verilerinizle çalışmaya yönelik kolay görsel çözümler sunar.  

## <a name="azure-storage-services"></a>Azure Depolama hizmetleri

Azure Depolama şu veri hizmetlerini içerir: 

- [Azure BLOB'ları](../blobs/storage-blobs-introduction.md): Metin ve ikili veriler için yüksek düzeyde ölçeklenebilir nesne deposu.
- [Azure dosyaları](../files/storage-files-introduction.md): Yönetilen dosya paylaşımları için bulut veya şirket içi dağıtımlar.
- [Azure kuyrukları](../queues/storage-queues-introduction.md): Uygulama bileşenleri arasında güvenilir Mesajlaşma için Mesajlaşma deposu. 
- [Azure tabloları](../tables/table-storage-overview.md): Yapılandırılmış verilerin şemasız depolanmasına yönelik bir NoSQL deposu.

Her hizmete bir depolama hesabı aracılığıyla erişilir. Başlamak için bkz. [Depolama hesabı oluşturma](storage-quickstart-create-account.md).

## <a name="blob-storage"></a>Blob depolama alanı

Azure Blob depolama, Microsoft’un buluta yönelik nesne depolama çözümüdür. Blob depolama, metin veya ikili veri gibi çok miktarda yapılandırılmamış veriyi depolamak için iyileştirilmiştir. 

Blob depolama aşağıdaki durumlar için idealdir:

* Görüntülerin veya belgelerin doğrudan bir tarayıcıya sunulması.
* Dağıtılan erişim için dosyaların depolanması.
* Video ve ses akışları.
* Yedekleme ve geri yükleme, olağanüstü durum kurtarma ve arşivleme için verilerin depolanması.
* Şirket içi veya Azure’da barındırılan bir hizmetle analiz için verilerin depolanması.

Blob depolamadaki nesnelere, dünyanın her yerinden HTTP veya HTTPS üzerinden erişilebilir. Kullanıcılar veya istemci uygulamalar, bloblara URL’ler, [Azure Depolama REST API’si](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage), [Azure CLI](https://docs.microsoft.com/cli/azure/storage) veya bir Azure Depolama istemci kitaplığı aracılığıyla erişebilir. Depolama istemci kitaplıkları [.NET](https://docs.microsoft.com/dotnet/api/overview/azure/storage/client), [Java](https://docs.microsoft.com/java/api/overview/azure/storage/client), [Node.js](https://azure.github.io/azure-storage-node), [Python](https://azure-storage.readthedocs.io/), [PHP](https://azure.github.io/azure-storage-php/) ve [Ruby](https://azure.github.io/azure-storage-ruby) dahil olmak üzere birden çok dil ile kullanılabilir.

Blob Depolama hakkında daha fazla bilgi için bkz. [Blob depolamaya giriş](../blobs/storage-blobs-introduction.md).

## <a name="azure-files"></a>Azure Dosyaları
[Azure Dosyaları](../files/storage-files-introduction.md), standart Sunucu İleti Bloğu (SMB) protokolü kullanılarak erişilebilen yüksek oranda kullanılabilir ağ dosya paylaşımları oluşturmanıza olanak tanır. Bu durum, birden fazla VM’nin hem okuma hem de yazma erişimi ile aynı dosyaları paylaşabildiği anlamına gelir. Dosyaları REST arabirimi veya depolama istemci kitaplıkları kullanarak da okuyabilirsiniz.

Dosyalara dünyanın herhangi bir yerinden dosyayı işaret eden ve paylaşılan erişim imzası (SAS) belirteci içeren bir URL kullanarak erişebilme olanağı, Azure Dosyaları'nı kurumsal bir dosya paylaşımındaki dosyalardan ayıran özelliklerden biridir. SAS belirteçleri oluşturabilirsiniz; bunlar, belirli bir süre için özel bir varlığa belirli bir erişim izni verir.

Dosya paylaşımları için birçok yaygın senaryoda kullanılabilir:

* Birçok şirket içi uygulama, dosya paylaşımlarını kullanır. Bu özellik, veri paylaşan uygulamaları Azure’a geçirmeyi kolaylaştırır. Dosya paylaşımını şirket içi uygulamanın kullandığı sürücü harfine bağlarsanız, uygulamanızın dosya paylaşımına erişen kısmı, varsa minimum değişikliklerle çalışır.

* Yapılandırma dosyaları bir dosya paylaşımında depolanabilir ve birden çok VM tarafından bu dosyalara erişilebilir. Bir gruptaki birden fazla geliştirici tarafından kullanılan araç ve yardımcı programlar, herkesin bulabilmesi ve aynı sürümü kullandıklarından emin olunması için bir dosya paylaşımında depolanabilir.

* Tanılama günlükleri, ölçümler ve kilitlenme bilgi dökümleri, bir dosya paylaşımına yazılıp daha sonra işlenebilen veya çözümlenebilen verilerin yalnızca üç örneğidir.

Şu anda Active Directory tabanlı kimlik doğrulaması ve erişim denetimi listeleri (ACL) desteklenmemektedir, ancak gelecekte desteklenecektir. Depolama hesabı kimlik bilgileri, dosya paylaşımına erişim için kimlik doğrulaması sağlamak üzere kullanılır. Bu durum, paylaşımın bağlı olduğu herkesin paylaşıma okuma/yazma erişimi elde edeceği anlamına gelir.

Azure Dosyaları hakkında daha fazla bilgi için bkz. [Azure Dosyaları'na Giriş](../files/storage-files-introduction.md).

## <a name="queue-storage"></a>Kuyruk depolama

Azure Kuyruk hizmeti, iletileri depolamak ve almak için kullanılır. Kuyruk iletilerinin boyutu 64 KB'ye kadar olabilir ve bir kuyruk, milyonlarca ileti içerebilir. Kuyruklar, genellikle zaman uyumsuz olarak işlenecek ileti listelerini depolamak için kullanılır.

Örneğin, müşterilerinizin resimleri karşıya yükleyebilmesini ve her resmin küçük resimlerini oluşturabilmesini istediğinizi düşünelim. Müşterinizin resimleri karşıya yüklerken küçük resimleri oluşturmanızı beklemesini sağlayabilirsiniz. Alternatif olarak bir kuyruk kullanabilirsiniz. Müşteri, karşıya yüklemeyi tamamladığında, kuyruğa bir ileti yazın. Ardından Azure İşlevinin iletiyi kuyruktan alıp ve küçük resimleri oluşturmasını sağlayın. Bu işlemin tüm bölümleri ayrıca ölçeklendirilebileceğinden kullanımınız için ayarlarken daha fazla kontrol sunar.

Azure Kuyrukları hakkında daha fazla bilgi için bkz. [Kuyruklara Giriş](../queues/storage-queues-introduction.md).

## <a name="table-storage"></a>Tablo depolama alanı

Azure Tablo depolama artık Azure Cosmos DB’nin bir parçasıdır. Azure Tablo depolama belgelerini görmek için bkz. [Azure Tablo Depolamaya Genel Bakış](../tables/table-storage-overview.md). Mevcut Azure Tablo depolama hizmetine ek olarak, aktarım hızı için iyileştirilmiş tablolar, genel dağıtım ve otomatik ikincil dizinler sağlayan yeni bir Azure Cosmos DB Tablo API'si teklifi vardır. Daha fazla bilgi edinmek ve yeni premium deneyimini denemek için lütfen [Azure Cosmos DB Tablo API’si](https://aka.ms/premiumtables) konusunu inceleyin.

Tablo depolama hakkında daha fazla bilgi için bkz. [Azure Tablo depolamaya genel bakış](../tables/table-storage-overview.md).

## <a name="disk-storage"></a>Disk depolama

Azure Depolama ayrıca sanal makineler tarafından kullanılan, yönetilen ve yönetilmeyen disk özelliklerine sahiptir. Bu özellikler hakkında daha fazla bilgi için bkz. [İşlem Hizmetleri belgeleri](https://docs.microsoft.com/azure/#pivot=products&panel=Compute).

## <a name="types-of-storage-accounts"></a>Depolama hesabı türleri

[!INCLUDE [storage-account-types-include](../../../includes/storage-account-types-include.md)]

Depolama hesabı türleri hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](storage-account-overview.md). 

## <a name="securing-access-to-storage-accounts"></a>Depolama hesapları için erişim güvenliğini sağlama

Azure depolama için her istek yetkili olması gerekir. Azure depolama, aşağıdaki yetkilendirme yöntemleri destekler:

- **Blob ve kuyruk verileri için Azure Active Directory (Azure AD) Tümleştirmesi.** Azure depolama, kimlik doğrulaması ve Azure AD kimlik bilgileriyle yetkilendirme için rol tabanlı erişim denetimi (RBAC) aracılığıyla Blob ve kuyruk hizmetlerine destekler. Azure AD ile istekleri yetkilendirme üst düzey güvenlik ve kullanım kolaylığı için önerilir. Daha fazla bilgi için [kimlik doğrulama erişim Azure blobları ve Azure Active Directory'yi kullanarak sıralar](storage-auth-aad.md).
- **Azure dosyaları (Önizleme) için Azure AD yetkilendirme SMB üzerinden.** Azure dosyaları, Azure Active Directory etki alanı Hizmetleri ile (sunucu ileti bloğu) SMB üzerinden kimlik tabanlı yetkilendirme destekler. Etki alanına katılmış Windows sanal makinelerinizi (VM) Azure dosya paylaşımlarını Azure AD kimlik bilgilerini kullanarak erişebilir. Daha fazla bilgi için [SMB üzerinden Azure dosyaları (Önizleme) genel bakış, Azure Active Directory yetkilendirme](../files/storage-files-active-directory-overview.md).
- **Paylaşılan anahtar ile yetkilendirme.** Azure depolama Blob, kuyruk ve tablo hizmetlerine ve Azure dosyaları ile paylaşılan bir depolama hesabı erişim anahtarı kullanılarak imzalandığını her istek üstbilgiyle yetkilendirme geçirir anahtar kullanılarak paylaşılan Key.A istemci yetkilendirme destekler. Daha fazla bilgi için [paylaşılan anahtar ile Authorize](https://docs.microsoft.com/rest/api/storageservices/authorize-with-shared-key).
- **Yetkilendirme kullanarak paylaşılan erişim imzaları (SAS).** Paylaşılan erişim imzası (SAS) depolama kaynağı için URI eklenecek bir güvenlik belirteci içeren bir dizedir. Güvenlik belirteci, izinler ve erişim aralığı gibi kısıtlamaları kapsüller. Daha fazla bilgi için [paylaşılan erişim imzaları (SAS) kullanma](storage-dotnet-shared-access-signature-part-1.md).
- **Kapsayıcılara ve blob'lara anonim erişimi.** Bir kapsayıcı ve bloblarını genel kullanıma açık olabilir. Bir kapsayıcı veya blob genel belirttiğinizde herkes anonim olarak okuyabilir; kimlik doğrulaması gereklidir. Daha fazla bilgi için bkz. [Kapsayıcılar ve bloblar için anonim okuma erişimini yönetme](../blobs/storage-manage-access-to-resources.md).

## <a name="encryption"></a>Şifreleme

Depolama hizmetleri için iki temel şifreleme seçeneği vardır. Güvenlik ve şifreleme hakkında daha fazla bilgi için bkz. [Azure Depolama güvenlik kılavuzu](storage-security-guide.md).

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

Azure Bekleyen Veri için Depolama Hizmeti Şifrelemesi (SSE), verilerinizi koruyarak kurumsal güvenlik ve uyumluluk taahhütlerinizi yerine getirmenize yardımcı olur. Bu özellik ile Azure Depolama, verilerinizi depolama alanında kalıcı hale gelmeden önce otomatik olarak şifreler ve alınmadan önce bunların şifresini çözer. Şifreleme, şifre çözme ve anahtar yönetimi, kullanıcılara tamamen şeffaf bir şekilde sunulur.


SSE tüm performans katmanları (Standart ve Premium), tüm dağıtım modelleri (Azure Resource Manager ve Klasik) ve tüm Azure Depolama hizmetlerinde (Blob, Kuyruk, Tablo ve Dosya) verileri otomatik olarak şifreler. SSE, Azure Depolama performansını etkilemez.

Bekleyen veri için SSE şifrelemesi hakkında daha fazla bilgi için bkz. [Bekleyen Veri için Azure Depolama Hizmeti Şifrelemesi](storage-service-encryption.md).

### <a name="client-side-encryption"></a>İstemci Tarafında Şifreleme

Depolama istemcisi kitaplıklarında, verileri istemciden Azure'a göndermeden önce programlı olarak şifrelemek için çağırabileceğiniz yöntemler vardır. Şifreli olarak depolandığından, bekleme sırasında da şifrelenmiş olacaktır. Veriler geri okunurken bilgileri aldıktan sonra şifresini çözersiniz.

İstemci tarafı şifreleme hakkında daha fazla bilgi için bkz. [Microsoft Azure Depolama için .NET ile İstemci Tarafı Şifreleme](storage-client-side-encryption.md).

## <a name="redundancy"></a>Yedeklilik

Verilerinizin güvende olmasını sağlamak için Azure Depolama, verilerinizin birden çok kopyasını çoğaltır. Depolama hesabınızı ayarladığınızda, bir yedeklik seçeneği seçin. 

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

Olağanüstü durum kurtarma hakkında daha fazla bilgi için bkz: [olağanüstü durum kurtarma ve depolama hesabı yük devretme (Önizleme) Azure Depolama'daki](storage-disaster-recovery-guidance.md).

## <a name="transferring-data-to-and-from-azure-storage"></a>Azure Depolama’da veri alışverişi

Verileri Azure depolama içine veya dışına taşımak için birkaç seçeneğiniz vardır. Hangi seçeneği, veri kümesi ve ağ bant genişliğiniz boyutuna bağlıdır. Daha fazla bilgi için [veri aktarımı için bir Azure çözümü seçin](storage-choose-data-transfer-solution.md).

## <a name="pricing"></a>Fiyatlandırma

Azure Depolama için fiyatlandırma hakkında ayrıntılı bilgi için bkz. [Fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="storage-apis-libraries-and-tools"></a>Depolama API'leri, kitaplıklar ve araçlar
Azure Storage kaynakları HTTP/HTTPS isteği yapabilen her dil ile erişilebilir. Ayrıca, Azure Storage birkaç popüler dilde programlama kitaplıkları sunar. Bu kitaplıklar zaman uyumlu ve zaman uyumsuz çağrılar, işlemleri gruplama, özel durum yönetimi, otomatik yeniden denemeler, işlemsel davranışlar vb. ayrıntıları ele alarak Azure Storage ile çalışmanın çoğu yönünü basitleştirir. Kitaplıklar şu anda aşağıdaki diller için mevcuttur ve yeni kitaplıklar geliştirme aşamasındadır:

### <a name="azure-storage-data-api-and-library-references"></a>Azure Depolama veri API’si ve kitaplık başvuruları
* [Depolama Hizmetleri REST API'si](https://docs.microsoft.com/rest/api/storageservices/)
* [.NET için Depolama İstemcisi Kitaplığı](https://docs.microsoft.com/dotnet/api/overview/azure/storage)
* [Java/Android için Depolama İstemcisi Kitaplığı](https://docs.microsoft.com/java/api/overview/azure/storage)
* [Node.js için Depolama İstemcisi Kitaplığı](https://docs.microsoft.com/javascript/api/azure-storage)
* [Python için Depolama İstemcisi Kitaplığı](https://github.com/Azure/azure-storage-python)
* [PHP için Depolama İstemcisi Kitaplığı](https://github.com/Azure/azure-storage-php)
* [Ruby için Depolama İstemcisi Kitaplığı](https://github.com/Azure/azure-storage-ruby)
* [C++ için Depolama İstemcisi Kitaplığı](https://github.com/Azure/azure-storage-cpp)

### <a name="azure-storage-management-api-and-library-references"></a>Azure Depolama yönetimi API’si ve kitaplık başvuruları
* [Depolama Kaynak Sağlayıcısı REST API’si](https://docs.microsoft.com/rest/api/storagerp/)
* [.NET için Depolama Kaynak Sağlayıcısı İstemci Kitaplığı](https://docs.microsoft.com/dotnet/api/overview/azure/storage/management)
* [Depolama Hizmet Yönetimi REST API'si (Klasik)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-api-and-library-references"></a>Azure Depolama veri taşıma API’si ve kitaplık başvuruları
* [Depolama İçeri/Dışarı Aktarma Hizmeti REST API'si](https://docs.microsoft.com/rest/api/storageimportexport/)
* [.NET için Depolama Veri Taşıma İstemcisi Kitaplığı](/dotnet/api/microsoft.azure.storage.datamovement)

### <a name="tools-and-utilities"></a>Araçlar ve Yardımcı Programlar
* [Depolama için Azure PowerShell Cmdlet'leri](https://docs.microsoft.com/powershell/module/az.storage)
* [Depolama için Azure CLI Cmdlet'leri](https://docs.microsoft.com/cli/azure/storage)
* [AzCopy Komut Satırı Yardımcı Programı](https://aka.ms/downloadazcopy)
* [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
* [Azure Depolama İstemcisi Araçları](../storage-explorers.md)
* [Azure Geliştirici Araçları](https://azure.microsoft.com/tools/)

## <a name="next-steps"></a>Sonraki adımlar

Çalışır durumda bir Azure Depolama edinmek için bkz. [Depolama hesabı oluşturma](storage-quickstart-create-account.md).
