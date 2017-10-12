---
title: "Azure Depolamaya Giriş | Microsoft Docs"
description: "Microsoft’un bulutta veri depolama alanı Azure Storage’a giriş."
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: a4a1bc58-ea14-4bf5-b040-f85114edc1f1
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: tamram
ms.openlocfilehash: e7b32aa2de5d6501e8d7894a936e9ab8b2f4f42f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-microsoft-azure-storage"></a>Microsoft Azure Storage’a Giriş

Microsoft Azure Depolama, yüksek oranda kullanılabilir, güvenli, dayanıklı, ölçeklenebilir ve yedekli depolama sağlayan, Microsoft tarafından yönetilen bir bulut hizmetidir. Microsoft bakımı üstlenir ve kritik sorunları sizin yerinize çözer. 

Azure Depolama üç veri hizmetinden oluşur: Blob depolama, Dosya depolama ve Kuyruk depolama. Blob depolama, standart ve premium depolama seçeneklerini destekler. Premium depolamada olabilecek en yüksek performans için yalnızca SSD kullanılır. Seyrek erişimli depolama, seyrek erişilen büyük miktarlarda verileri düşük maliyetli depolama olanağı sunan başka bir özelliktir.

Bu makalede, aşağıdakiler hakkında bilgi edinirsiniz:
* Azure Depolama hizmetleri
* Depolama hesabı türleri
* Bloblara, kuyruklara ve dosyalara erişme
* şifreleme
* çoğaltma 
* Depolama içine veya dışına veri aktarma
* kullanabileceğiniz çok sayıda depolama istemcisi. 

Azure Depolama ile hemen çalışmaya başlamak için aşağıdaki hızlı başlangıç belgelerini inceleyin:
* [PowerShell kullanarak depolama hesabı oluşturma](storage-quickstart-create-storage-account-powershell.md)
* [CLI kullanarak depolama hesabı oluşturma](storage-quickstart-create-storage-account-cli.md)

## <a name="introducing-the-azure-storage-services"></a>Azure Depolama hizmetlerine giriş

Azure Depolama tarafından sağlanan hizmetlerden (Blob depolama, Dosya depolama ve Kuyruk depolama) birini kullanmak için önce bir depolama hesabı oluşturmanız gerekir. Ardından o depolama hesabındaki belirli bir hizmete/hizmetten veri aktarabilirsiniz. 

## <a name="blob-storage"></a>Blob depolama

Bloblar, temelde bilgisayarda (veya tablet, mobil cihaz ve benzeri) depoladığınız dosyalara benzer dosyalardır. Bu dosyalar resimler, Microsoft Excel dosyaları, HTML dosyaları, sanal sabit diskler (VHD) olabileceği gibi, günlükler, veritabanı yedeklemeleri gibi büyük veriler de dahil olmak üzere neredeyse her şey olabilir. Bloblar klasörlere benzer kapsayıcılarda depolanır. 

Blob depolamada depoladığınız dosyalara URL'leri, REST arabirimi ya da Azure SDK'sı depolama istemci kitaplıklarından birini kullanarak tüm dünyadan erişebilirsiniz. Depolama istemcisi kitaplıkları, Node.js, Java, PHP, Ruby, Python ve .NET dahil olmak üzere birden çok dil için kullanılabilir. 

Blok bloblar, sayfa blobları (VHD dosyaları için kullanılır) ve ekleme blobları olmak üzere üç tür blob vardır.

* Blok blobları, yaklaşık 4,7 TB’a kadar boyutta sıradan dosyaları saklamak için kullanılır. 
* Sayfa blobları, 8 TB’a kadar boyutta rastgele erişimli dosyaları saklamak için kullanılır. Bunlar, sanal makineleri içeren VHD dosyaları için kullanılır.
* Ekleme blobları blok bloblarına benzer bloklardan oluşur ancak ekleme işlemleri için en iyi duruma getirilmiştir. Bunlar, aynı bloba birden çok VM'den günlük bilgileri kaydı gibi şeyler için kullanılır.

Ağ kısıtlamalarının kablo üzerinden Blob depolamaya veri yükleme veya indirme yapmayı kullanışsız hale getirdiği çok büyük veri kümelerinde verileri doğrudan veri merkezinden içeri veya dışarı aktarmak için Microsoft’a sabit sürücüler gönderebilirsiniz. Bkz: [Blob Storage’a Veri Aktarmak için Microsoft Azure İçeri/Dışarı Aktarma Hizmeti Kullanma](../storage-import-export-service.md).

## <a name="azure-files"></a>Azure Dosyaları
[Azure Dosyaları](../files/storage-files-introduction.md), standart Sunucu İleti Bloğu (SMB) protokolü kullanılarak erişilebilen yüksek oranda kullanılabilir ağ dosya paylaşımları oluşturmanıza olanak tanır. Bu durum, birden fazla VM’nin hem okuma hem de yazma erişimi ile aynı dosyaları paylaşabildiği anlamına gelir. Dosyaları REST arabirimi veya depolama istemci kitaplıkları kullanarak da okuyabilirsiniz. 

Dosyalara dünyanın herhangi bir yerinden dosyayı işaret eden ve paylaşılan erişim imzası (SAS) belirteci içeren bir URL kullanarak erişebilme olanağı, Azure Dosyaları'nı kurumsal bir dosya paylaşımındaki dosyalardan ayıran özelliklerden biridir. SAS belirteçleri oluşturabilirsiniz; bunlar, belirli bir süre için özel bir varlığa belirli bir erişim izni verir. 

Dosya paylaşımları için birçok yaygın senaryoda kullanılabilir: 

* Birçok şirket içi uygulama, dosya paylaşımlarını kullanır. Bu özellik, veri paylaşan uygulamaları Azure’a geçirmeyi kolaylaştırır. Dosya paylaşımını şirket içi uygulamanın kullandığı sürücü harfine bağlarsanız, uygulamanızın dosya paylaşımına erişen kısmı, varsa minimum değişikliklerle çalışır.

* Yapılandırma dosyaları bir dosya paylaşımında depolanabilir ve birden çok VM tarafından bu dosyalara erişilebilir. Bir gruptaki birden fazla geliştirici tarafından kullanılan araç ve yardımcı programlar, herkesin bulabilmesi ve aynı sürümü kullandıklarından emin olunması için bir dosya paylaşımında depolanabilir.

* Tanılama günlükleri, ölçümler ve kilitlenme bilgi dökümleri, bir dosya paylaşımına yazılıp daha sonra işlenebilen veya çözümlenebilen verilerin yalnızca üç örneğidir.

Şu anda Active Directory tabanlı kimlik doğrulaması ve erişim denetimi listeleri (ACL) desteklenmemektedir, ancak gelecekte desteklenecektir. Depolama hesabı kimlik bilgileri, dosya paylaşımına erişim için kimlik doğrulaması sağlamak üzere kullanılır. Bu durum, paylaşımın bağlı olduğu herkesin paylaşıma okuma/yazma erişimi elde edeceği anlamına gelir.

## <a name="queue-storage"></a>Kuyruk depolama

Azure Kuyruk hizmeti, iletileri depolamak ve almak için kullanılır. Kuyruk iletilerinin boyutu 64 KB'ye kadar olabilir ve bir kuyruk, milyonlarca ileti içerebilir. Kuyruklar, genellikle zaman uyumsuz olarak işlenecek ileti listelerini depolamak için kullanılır. 

Örneğin, müşterilerinizin resimleri karşıya yükleyebilmesini ve her resmin küçük resimlerini oluşturabilmesini istediğinizi düşünelim. Müşterinizin resimleri karşıya yüklerken küçük resimleri oluşturmanızı beklemesini sağlayabilirsiniz. Alternatif olarak bir kuyruk kullanabilirsiniz. Müşteri, karşıya yüklemeyi tamamladığında, kuyruğa bir ileti yazın. Ardından Azure İşlevinin iletiyi kuyruktan alıp ve küçük resimleri oluşturmasını sağlayın. Bu işlemin tüm bölümleri ayrıca ölçeklendirilebileceğinden kullanımınız için ayarlarken daha fazla kontrol sunar.

## <a name="table-storage"></a>Table Storage

Standart Azure Tablo Depolama artık Cosmos DB’nin bir parçasıdır. Belgeleri görmek için bkz. [Azure Tablo Depolamaya Genel Bakış](../../cosmos-db/table-storage-overview.md). Ayrıca Azure Tablo depolama için aktarım hızı iyileştirilmiş tablolar, global dağıtım ve otomatik ikincil dizinler sunan Premium Tablolar seçeneği vardır. Daha fazla bilgi edinmek ve yeni premium deneyimi denemek için lütfen [Azure Cosmos DB: Tablo API’si](https://aka.ms/premiumtables) konusunu inceleyin.

## <a name="disk-storage"></a>Disk depolama

Azure Depolama ayrıca sanal makineler tarafından kullanılan, yönetilen ve yönetilmeyen disk özelliklerine sahiptir. Bu özellikler hakkında daha fazla bilgi için bkz. [İşlem Hizmetleri belgeleri](https://docs.microsoft.com/azure/#pivot=services&panel=Compute).

## <a name="types-of-storage-accounts"></a>Depolama hesabı türleri 

Bu tabloda, çeşitli depolama hesapları ve hangi nesnelerin bunlarda kullanılabildiği gösterilmektedir.

|**Depolama hesabı türü**|**Genel amaçlı Standart**|**Genel amaçlı Premium**|**Blob depolama, sık erişimli ve seyrek erişimli erişim katmanları**|
|-----|-----|-----|-----|
|**Desteklenen hizmetler**| Blob, Dosya, Kuyruk Hizmetleri | Blob Hizmeti | Blob Hizmeti|
|**Desteklenen blob türleri**|Blok blobları, sayfa blobları ve ek bloblar | Sayfa blobları | Blok blobları ve ek blobları|

### <a name="general-purpose-storage-accounts"></a>Genel amaçlı depolama hesapları

Genel amaçlı depolama hesaplarının iki türü vardır. 

#### <a name="standard-storage"></a>Standart depolama 

En yaygın olarak kullanılan depolama hesapları tüm veri türleri için kullanılabilen standart depolama hesaplarıdır. Standart depolama hesapları, manyetik ortam verilerini depolamak için kullanılır.

#### <a name="premium-storage"></a>Premium depolama

Premium depolama hesapları öncelikle VHD dosyaları için kullanılan sayfa blobları için yüksek performanslı depolama sağlar. Premium depolama hesapları verileri depolamak için SSD kullanır. Microsoft, tüm VM'leriniz için Premium depolama kullanmanızı önerir.

### <a name="blob-storage-accounts"></a>Blob Depolama Hesapları

Blob Depolama hesabı, blok blobları ve ek bloblar depolamak için kullanılan bir özel depolama hesabıdır. Sayfa bloblarını bu hesaplarda depolayacağınızdan, VHD dosyalarını da depolayamazsınız. Bu hesaplar, erişim katmanını sık veya seyrek erişimli olarak ayarlamanıza olanak verir; katman herhangi bir zamanda değiştirilebilir. 

Sık erişim katmanı sık erişilen dosyalar için kullanılır.-Depolama için daha yüksek bir maliyet ödersiniz, ancak bloblara erişme maliyeti çok daha düşüktür. Seyrek erişimli erişim katmanında depolanan bloblara erişmek için daha yüksek bir maliyet ödersiniz, ancak depolama maliyeti çok daha düşüktür.

## <a name="accessing-your-blobs-files-and-queues"></a>Bloblara, kuyruklara ve dosyalara erişme

Her depolama hesabı herhangi bir işlem için kullanılabilen iki kimlik doğrulama anahtarına sahiptir. İki anahtar olduğundan güvenliği artırmak için anahtarları değiştirebilirsiniz. Hesap adı ile birlikte depolama hesabındaki tüm verilere sınırsız erişim olanağı verdiğinden bu anahtarların korunması önemlidir. 

Bu bölümde, depolama hesabını ve verilerini güvenceye almak için iki yöntem incelenir. Depolama hesabınız ve verilerinizin güvenliğini sağlama hakkında ayrıntılı bilgi için bkz. [Azure Depolama güvenlik kılavuzu](storage-security-guide.md).

### <a name="securing-access-to-storage-accounts-using-azure-ad"></a>Azure AD kullanarak depolama hesaplarında erişim güvenliğini sağlama

Depolama verilerinize erişim güvenliğini sağlamak için seçeneklerden biri depolama hesabı anahtarlarına erişimi denetlemektir. Kaynak Yöneticisi Rol Tabanlı Erişim Denetimi (RBAC) ile kullanıcılar, gruplar veya uygulamalar için roller atayabilirsiniz. Bu roller, belirli bir izin verilen veya izin verilmeyen eylemler kümesine bağlıdır. RBAC kullanarak bir depolama hesabına erişim izni vermek, yalnızca bu depolama hesabı için erişim katmanını değiştirme işlemi gibi yönetim işlemlerini işler. RBAC’yi belirli bir kapsayıcı veya dosya paylaşımı gibi veri nesnelerine erişim vermek için kullanamazsınız. Ancak RBAC ile depolama hesabı anahtarlarına erişim verebilirsiniz, bu anahtarlar veri nesnelerini okumak için kullanılabilir. 

### <a name="securing-access-using-shared-access-signatures"></a>Paylaşılan erişim imzalarını kullanarak erişim güvenliğini sağlama 

Veri nesnelerini güvenli hale getirmek için paylaşılan erişim imzalarını ve depolanan erişim ilkelerini kullanabilirsiniz. Paylaşılan erişim imzası (SAS), belirli depolama nesnelerine erişim yetkisi vermenizi, erişim için izinleri ve erişim tarih/zaman aralığı kısıtlamaları belirlemenizi sağlayan bir varlığın URI’sına eklenebilen bir güvenlik belirteci içeren bir dizedir. Bu özellik kapsamlı olanaklar sağlar. Ayrıntılı bilgi için bkz. [Paylaşılan Erişim İmzaları (SAS) kullanma](storage-dotnet-shared-access-signature-part-1.md).

### <a name="public-access-to-blobs"></a>Bloblara genel erişim

Blob Hizmeti, bir kapsayıcıya ve bloblarına veya belirli bir bloba genel erişim sağlamanıza izin verir. Bir kapsayıcı veya bir blobun genel erişime açıldığını belirttiğinizde herkes anonim olarak okuyabilir, herhangi bir kimlik doğrulama gerekli değildir. Görüntü, video veya Blob depolama biriminden belgeler kullanan bir web siteniz varsa bunu yapmak isteyebilirsiniz. Daha fazla bilgi için bkz. [Kapsayıcılar ve bloblar için anonim okuma erişimini yönetme](../blobs/storage-manage-access-to-resources.md). 

## <a name="encryption"></a>Şifreleme

Depolama hizmetleri için birkaç temel şifreleme seçeneği vardır. 

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme 

Depolama Hizmeti Şifrelemesini bir Azure depolama hesabının Dosyalar hizmetinde (önizleme) veya Blob hizmetinde (SSE) etkinleştirebilirsiniz. Etkinleştirilirse, ilgili hizmete yazılan tüm veriler yazılmadan önce şifrelenir. Okunmadan önce verilerin şifresi çözülür. 

### <a name="client-side-encryption"></a>İstemci Tarafında Şifreleme

Depolama istemcisi kitaplıklarında, verileri istemciden Azure'a göndermeden önce programlı olarak şifrelemek için çağırabileceğiniz yöntemler vardır. Şifreli olarak depolandığından, bekleme sırasında da şifrelenmiş olacaktır. Veriler geri okunurken bilgileri aldıktan sonra şifresini çözersiniz. 

### <a name="encryption-in-transit-with-azure-file-shares"></a>Azure Dosya paylaşımları ile aktarım sırasında şifreleme

Paylaşılan erişim imzaları ile ilgili daha fazla bilgi edinmek için bkz. [Paylaşılan Erişim İmzaları (SAS) kullanma](../storage-dotnet-shared-access-signature-part-1.md). Depolama hesabınıza güvenli erişim ile ilgili daha fazla bilgi için bkz. [Kapsayıcılar ve bloblara anonim okuma erişimini yönetme](../blobs/storage-manage-access-to-resources.md) ve [Azure Storage Hizmetleri için Kimlik Doğrulama](https://msdn.microsoft.com/library/azure/dd179428.aspx) 

Depolama hesabınızın ve verilerinizin güvenliğini sağlama hakkında daha fazla bilgi için bkz. [Azure Depolama güvenlik kılavuzu](storage-security-guide.md).

## <a name="replication"></a>Çoğaltma

Azure Depolama, verilerinizin güvende olmasını sağlamak için verilerinizin birden çok kopyasını tutma (ve yönetme) özelliğine sahiptir. Buna çoğaltma veya yedekleme denir. Depolama hesabınızı ayarladığınızda, çoğaltma türünü seçersiniz. Çoğu durumda, depolama hesabı ayarlandıktan sonra bu ayar değiştirilebilir. 

Tüm depolama hesaplarında **yerel olarak yedekli depolama (LRS)** vardır. Bu, verilerinizin üç kopyasının, depolama hesabı ayarlandığı zaman belirtilen veri merkezinde Azure depolama veri merkezi tarafından yönetildiği anlamına gelir. Bir kopyada değişiklik yapıldığında, başarılı sonuç döndürülmeden önce diğer iki kopya güncelleştirilir. Bu, üç kopyanın her zaman eşitlenmiş durumda olduğu anlamına gelir. Ayrıca, üç kopya ayrı hata etki alanları ve yükseltme etki alanlarında bulunur, böylece verilerinizin bulunduğu bir depolama düğümü arızalanır veya güncelleştirme amacıyla devreden çıkarılırsa bile verileriniz kullanılabilir. 

**Yerel olarak yedekli depolama (LRS)**

Yukarıda da açıklandığı şekilde, LRS ile tek bir veri merkezinde verilerinizin üç kopyasına sahip olursunuz. Bu da verilerinizin, bir depolama düğümü arızalanır veya güncelleştirme amacıyla devreden çıkarılırsa bile kullanılabileceği, ancak veri merkezinin tamamı devre dışı olduğunda kullanılamayacağı anlamına gelir.

**Bölgesel olarak yedekli depolama (ZRS)**

Bölgesel olarak yedekli depolama (ZRS) verilerinizin üç yerel kopyasını ve ek bir üç kopyasını tutar. İkinci küme üç kopya, bir veya iki bölgedeki veri merkezleri arasında zaman uyumsuz olarak çoğaltılır. ZRS’nin yalnızca genel amaçlı depolama hesaplarındaki blok bloblar için kullanılabilir olduğunu unutmayın. Ayrıca, Depolama hesabınızı oluşturup ZRS’yi seçtiğinizde farklı bir tür çoğaltma seçeneği kullanmak üzere dönüştüremezsiniz; tersi durumda da aynısı söz konusudur.

ZRS hesapları LRS’ye göre daha fazla dayanıklılık sağlar, ancak ZRS hesaplarının ölçüm ve günlüğe kaydetme özelliği yoktur. 

**Coğrafi olarak yedekli depolama (GRS)**

Coğrafi olarak yedekli depolama (GRS), verilerinizin bir birincil bölgede üç yerel kopyasını, ayrıca birincil bölgeden yüzlerce mil uzaktaki ikincil bir bölgede üç kopyasını tutar. Birincil bölgede bir arıza olması durumunda Azure Storage ikincil bölgeye yük devredecektir. 

**Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)** 

Okuma erişimli coğrafi olarak yedekli depolama, ikincil konumdaki verilere yalnızca okuma erişimi sağlaması dışında aynı GRS gibidir. Birincil veri merkezi geçici olarak kullanılamaz duruma gelirse, verileri ikincil konumdan okumaya devam edebilirsiniz. Bu çok yararlı olabilir. Örneğin, bir web uygulamanız salt okunur moda geçip ikincil kopyaya başvurabilir. Böylece güncelleştirmeler kullanılabilir olmasa da belirli bir düzey erişim sağlanır. 

> [!IMPORTANT]
> Hesabınızı oluştururken ZRS seçmediyseniz, depolama hesabınız oluşturulduktan sonra verilerinizin çoğaltılma yöntemini değiştirebilirsiniz. Buna karşın LRS’den GRS’ye veya RA-GRS’ye geçiş yaparsanız tek seferlik veri aktarımı ücreti ödemeniz gerekebileceğini unutmayın.
>

Çoğaltma hakkında daha fazla bilgi için bkz. [Azure Depolama çoğaltma](storage-redundancy.md).

Olağanüstü durum kurtarma için bkz. [Azure Depolama kesinti oluşursa yapmanız gerekenler](storage-disaster-recovery-guidance.md).

Yüksek kullanılabilirlik sağlamak için RA-GRS depolamadan yararlanma hakkında bir örnek için bkz. [RA-GRS’yi kullanarak yüksek kullanılabilirliğe sahip uygulamalar tasarlama](storage-designing-ha-apps-with-ragrs.md).

## <a name="transferring-data-to-and-from-azure-storage"></a>Azure Depolama’da veri alışverişi

Depolama hesabınızda veya depolama hesaplarınız arasında blob ve dosya verilerinizi kopyalamak için AzCopy komut satırı yardımcı programını kullanabilirsiniz. Yardım için şu makalelerden birine bakın:

* [Windows için AzCopy ile veri aktarma](storage-use-azcopy.md)
* [Linux için AzCopy ile veri aktarma](storage-use-azcopy-linux.md)

AzCopy şu anda önizlemede yer alan [Azure Veri Taşıma Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/) üzerine kurulmuştur.

Azure İçeri/Dışarı Aktarma hizmeti, büyük miktarlarda blob verilerini depolama hesabınızda içeri veya dışarı aktarmak için kullanılabilir. Bir Azure veri merkezine birden çok sabit sürücü hazırlayıp gönderirsiniz, burada da veri aktarımı tamamlanır ve sabit sürücüler size geri gönderilir. İçeri/Dışarı Aktarma hizmeti hakkında daha fazla bilgi için bkz. [Blob Storage Veri Aktarma için Microsoft Azure İçeri/Dışarı Aktarma Hizmeti Kullanma](../storage-import-export-service.md).

## <a name="pricing"></a>Fiyatlandırma

Azure Depolama için fiyatlandırma hakkında ayrıntılı bilgi için bkz. [Fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="storage-apis-libraries-and-tools"></a>Depolama API'leri, kitaplıklar ve araçlar
Azure Storage kaynakları HTTP/HTTPS isteği yapabilen her dil ile erişilebilir. Ayrıca, Azure Storage birkaç popüler dilde programlama kitaplıkları sunar. Bu kitaplıklar zaman uyumlu ve zaman uyumsuz çağrılar, işlemleri gruplama, özel durum yönetimi, otomatik yeniden denemeler, işlemsel davranışlar vb. ayrıntıları ele alarak Azure Storage ile çalışmanın çoğu yönünü basitleştirir. Kitaplıklar şu anda aşağıdaki diller için mevcuttur ve yeni kitaplıklar geliştirme aşamasındadır:

### <a name="azure-storage-data-services"></a>Azure Depolama veri hizmetleri
* [Depolama Hizmetleri REST API'si](/rest/api/storageservices/)
* [.NET için Depolama İstemcisi Kitaplığı](https://docs.microsoft.com/dotnet/api/?view=azurestorage-8.1.1)
* [C++ için Depolama İstemcisi Kitaplığı](https://github.com/Azure/azure-storage-cpp)
* [Java/Android için Depolama İstemcisi Kitaplığı](https://azure.microsoft.com/develop/java/)
* [Node.js için Depolama İstemcisi Kitaplığı](http://dl.windowsazure.com/nodestoragedocs/index.html)
* [PHP için Depolama İstemcisi Kitaplığı](https://azure.microsoft.com/develop/php/)
* [Python için Depolama İstemcisi Kitaplığı](https://azure.microsoft.com/develop/python/)
* [Ruby için Depolama İstemcisi Kitaplığı](https://azure.microsoft.com/develop/ruby/)
* [PowerShell için Depolama Cmdlet'leri](/powershell/module/azure.storage/?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0)
* [CLI 2.0 için Depolama Komutları](/cli/azure/storage)

## <a name="next-steps"></a>Sonraki adımlar

* [Blob depolama hakkında daha fazla bilgi](../blobs/storage-blobs-introduction.md)
* [Dosya depolama hakkında daha fazla bilgi](../storage-files-introduction.md)
* [Kuyruk depolama hakkında daha fazla bilgi](../queues/storage-queues-introduction.md)

Azure Depolama ile hemen çalışmaya başlamak için aşağıdaki hızlı başlangıç belgelerini inceleyin:
* [PowerShell kullanarak depolama hesabı oluşturma](storage-quickstart-create-storage-account-powershell.md)
* [CLI kullanarak depolama hesabı oluşturma](storage-quickstart-create-storage-account-cli.md)

<!-- FIGURE OUT WHAT TO DO WITH ALL THESE LINKS.

Azure Storage resources can be accessed by any language that can make HTTP/HTTPS requests. Additionally, Azure Storage offers programming libraries for several popular languages. These libraries simplify many aspects of working with Azure Storage by handling details such as synchronous and asynchronous invocation, batching of operations, exception management, automatic retries, operational behavior and so forth. Libraries are currently available for the following languages and platforms, with others in the pipeline:

### Azure Storage data services
* [Storage Services REST API](https://docs.microsoft.com/rest/api/storageservices/)
* [Storage Client Library for .NET](https://docs.microsoft.com/dotnet/api/?view=azurestorage-8.1.1)
* [Storage Client Library for C++](https://github.com/Azure/azure-storage-cpp)
* [Storage Client Library for Java/Android](https://azure.microsoft.com/develop/java/)
* [Storage Client Library for Node.js](http://dl.windowsazure.com/nodestoragedocs/index.html)
* [Storage Client Library for PHP](https://azure.microsoft.com/develop/php/)
* [Storage Client Library for Python](https://azure.microsoft.com/develop/python/)
* [Storage Client Library for Ruby](https://azure.microsoft.com/develop/ruby/)
* [Storage Cmdlets for PowerShell](/powershell/module/azure.storage/?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0)

### Azure Storage management services
* [Storage Resource Provider REST API Reference](/rest/api/storagerp/)
* [Storage Resource Provider Client Library for .NET](/dotnet/api/microsoft.azure.management.storage)
* [Storage Resource Provider Cmdlets for PowerShell 1.0](/powershell/module/azure.storage)
* [Storage Service Management REST API (Classic)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### Azure Storage data movement services
* [Storage Import/Export Service REST API](../storage-import-export-service.md)
* [Storage Data Movement Client Library for .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### Tools and utilities
* [Microsoft Azure Storage Explorer](../../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.
* [Azure Storage Client Tools](../storage-explorers.md)
* [Azure SDKs and Tools](https://azure.microsoft.com/tools/)
* [Azure Storage Emulator](http://www.microsoft.com/download/details.aspx?id=43709)
* [Azure PowerShell](/powershell/azure/overview)
* [AzCopy Command-Line Utility](http://aka.ms/downloadazcopy)

## Next steps
To learn more about Azure Storage, explore these resources:

### Documentation
* [Azure Storage Documentation](https://azure.microsoft.com/documentation/services/storage/)
* [Create a storage account](../storage-create-storage-account.md)

-->

### <a name="for-administrators"></a>Yöneticiler için
* [Azure Depolama ile Azure PowerShell’i kullanma](storage-powershell-guide-full.md)
* [Azure Depolama ile Azure CLI'sini kullanma](../storage-azure-cli.md)

### <a name="for-net-developers"></a>.NET geliştiricileri için
* [.NET kullanarak Azure Blob Depolama ile çalışmaya başlama](../blobs/storage-dotnet-how-to-use-blobs.md)
* [.NET ile Azure Dosyaları için geliştirme](../files/storage-dotnet-how-to-use-files.md)
* [.NET kullanarak Azure Tablo Depolama ile çalışmaya başlama](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [.NET kullanarak Azure Kuyruk Depolama ile çalışmaya başlama](../storage-dotnet-how-to-use-queues.md)

### <a name="for-javaandroid-developers"></a>Java/Android geliştiricileri için
* [Java'da Blob Depolama'yı kullanma](../blobs/storage-java-how-to-use-blob-storage.md)
* [Java ile Azure Dosyaları için geliştirme](../files/storage-java-how-to-use-file-storage.md)
* [Java'da Tablo Depolama'yı kullanma](../../cosmos-db/table-storage-how-to-use-java.md)
* [Java'da Kuyruk Depolama'yı kullanma](../storage-java-how-to-use-queue-storage.md)

### <a name="for-nodejs-developers"></a>Node.js geliştiricileri için
* [Node.js'de Blob Depolama'yı kullanma](../blobs/storage-nodejs-how-to-use-blob-storage.md)
* [Node.js'de Tablo Depolama'yı kullanma](../../cosmos-db/table-storage-how-to-use-nodejs.md)
* [Node.js'de Kuyruk Depolama'yı kullanma](../storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>PHP geliştiricileri için
* [PHP'de Blob Depolama'yı kullanma](../blobs/storage-php-how-to-use-blobs.md)
* [PHP'de Tablo Depolama'yı kullanma](../../cosmos-db/table-storage-how-to-use-php.md)
* [PHP'de Kuyruk Depolama'yı kullanma](../storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>Ruby geliştiricileri için
* [Ruby'de Blob Depolama'yı kullanma](../blobs/storage-ruby-how-to-use-blob-storage.md)
* [Ruby'de Tablo Depolama'yı kullanma](../../cosmos-db/table-storage-how-to-use-ruby.md)
* [Ruby'de Kuyruk Depolama'yı kullanma](../storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>Python geliştiricileri için
* [Python'da Blob Depolama'yı kullanma](../blobs/storage-python-how-to-use-blob-storage.md)
* [Python ile Azure Dosyaları için geliştirme](../files/storage-python-how-to-use-file-storage.md)
* [Python'da Tablo Depolama'yı kullanma](../../cosmos-db/table-storage-how-to-use-python.md)
* [Python'da Kuyruk Depolama'yı kullanma](../storage-python-how-to-use-queue-storage.md)