---
title: Azure sayfa blobları genel bakış | Microsoft Docs
description: Azure sayfa blobları ve kullanım örnekleri ile örnek betikler de dahil olmak üzere, kendi avantajları genel bakış.
services: storage
author: anasouma
ms.service: storage
ms.topic: article
ms.date: 01/03/2019
ms.author: wielriac
ms.subservice: blobs
ms.openlocfilehash: b03da04c97475dcb9ce15f2ed69d7ca333d6f431
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61428399"
---
# <a name="overview-of-azure-page-blobs"></a>Azure sayfa blobları genel bakış

Azure depolama üç tür blob depolama sunar: Blok Blobları, ekleme Blobları ve sayfa blobları. Blok blobları bloklarını oluşan ve büyük dosyaları verimli bir şekilde karşıya yükleme ve metin veya ikili dosyaları depolamak için idealdir. Ekleme blobları da yapılan bloklarını, ancak için iyileştirilmiş ekleme işlemleri, bunları günlük kaydı senaryoları için idealdir. Sayfa blobları yapılan 512 baytlık sayfaların toplam boyutu ve 8 TB'ye kadar sık rastgele okuma/yazma işlemleri için tasarlanmıştır. Sayfa blobları, Azure Iaas diskleri temelidir. Bu makalede, sayfa blobları avantajları ve özellikleri açıklayan üzerinde odaklanır.

Sayfa blobları, rastgele bayt aralıkları okuma/yazma olanağı sağlayan 512 baytlık sayfalarının bir koleksiyondur. Bu nedenle, sayfa blobları, sanal makineler ve veritabanları için işletim sistemi ve veri diskleri gibi dizin tabanlı ve seyrek veri yapılarını depolamak için idealdir. Örneğin, sayfa blobları Azure SQL DB veritabanları için temel alınan kalıcı depolama alanı olarak kullanır. Ayrıca, sayfa blobları, genellikle de Range-Based güncelleştirmeleri ile dosyaları için kullanılır.  

Azure sayfa blobları anahtar özellikleri, REST arabirimi, temel alınan depolama alanı ve azure'da sorunsuz geçiş yeteneklerine dayanıklılık şunlardır. Bu özellikler, sonraki bölümde daha ayrıntılı ele alınmıştır. Ayrıca, Azure sayfa blobları, şu anda iki tür depolama desteklenir: Premium depolama ve standart depolama. Premium depolama, özellikle tutarlı, yüksek performanslı ve düşük gecikme süreli veri depolama veritabanları için yüksek performanslı premium sayfa blobları idealdir gerektiren iş yükleri için tasarlanmıştır.  Standart depolama gecikmeye duyarlı olmayan iş yüklerini çalıştırmak için uygun maliyetlidir.

## <a name="sample-use-cases"></a>Örnek kullanım durumları

Şimdi birkaç sayfa BLOB'ları ile Azure Iaas diskleri başlatma için kullanım durumları ele alınmıştır. Azure sayfa blobları, Azure Iaas sanal diskler platformunu temel. Hem Azure işletim sistemi ve veri diskleri, verilerin depolanmasına da Azure depolama platformu kalıcı ve en yüksek performans için sanal makinelere teslim edilen sanal diskler olarak uygulanır. Azure diskleri Hyper-V kalıcı [VHD biçimi](https://technet.microsoft.com/library/dd979539.aspx) ve olarak depolanan bir [sayfa blobu](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs#about-page-blobs) Azure depolamadaki. Azure Iaas Vm'leri için sanal diskleri kullanmaya ek olarak, sayfa BLOB'ları gibi hızlı rastgele okuma-yazma işlemleri için veritabanını etkinleştirme, SQL veri depolamak için sayfa BLOB'ları şu anda kullandığı Azure SQL veritabanı hizmeti, PaaS ve DBaaS senaryoları da etkinleştirin. Başka bir örnek, sayfa blobları, paylaşılan bir medya erişimi işbirliğine dayalı video düzenleme uygulamaları için bir PaaS hizmeti varsa, ortam içinde rastgele konumlara hızlı erişime olanak tanımak olacaktır. Ayrıca, hızlı ve etkili düzenleme ve birleştirme birden çok kullanıcı tarafından aynı ortam sağlar. 

Azure Site Recovery, Azure Backup yanı sıra çok sayıda üçüncü taraf geliştiriciler gibi birinci taraf Microsoft hizmetlerine, sayfa blob'ın REST arabirimini kullanarak sektör lideri yeniliklerden uyguladınız. Azure üzerinde uygulanan benzersiz senaryolardan bazıları şunlardır: 
* Uygulama yönelik artımlı anlık görüntü Yönetimi: Uygulamalar sayfası blob anlık görüntüleri ve REST API'lerinin veri maliyetli çoğaltma olmaksızın uygulama kontrol noktalarını kaydetmek için yararlanabilirsiniz. Azure depolama, tüm blob kopyalama gerektirmeyen sayfa blobları için yerel anlık görüntüleri destekler. Bu genel bir anlık görüntü, API erişme ve anlık görüntü arasındaki farkları kopyalamayı etkinleştirin.
* Uygulama ve şirket içi verileri buluta dinamik geçişini: Şirket içi veri kopyalama ve doğrudan şirket içi sanal makine çalışmaya devam ederken bir Azure sayfa blobu yazma için REST API'lerini kullanın. Hedef zachytila výjimku sonra hızlı bir şekilde bu verileri kullanarak Azure VM yük devretme gerçekleştirebilirsiniz. Bu şekilde, Vm'lerinizin geçişini yapabilirsiniz ve şirket içi VM ve yük devretme için gerekli kapalı kalma süresi kullanmaya devam ederken veri geçişi, arka planda gerçekleşir. bu yana en düşük kapalı kalma süresi ile buluta sanal diskleri (dakika) kısa olacaktır.
* [SAS tabanlı](../common/storage-dotnet-shared-access-signature-part-1.md) paylaşılan erişim, birden çok okuyucu ve eşzamanlılık denetimi desteğiyle tek yazıcı gibi senaryolara olanak sağlar.

## <a name="page-blob-features"></a>Sayfa blobu özellikleri

### <a name="rest-api"></a>REST API
Kullanmaya başlamak için şu belgeye başvurun [sayfa BLOB'ları kullanarak geliştirme](storage-dotnet-how-to-use-blobs.md). Örneğin, sayfa BLOB'ları için .NET depolama istemci kitaplığı kullanarak nasıl bakın. 

Aşağıdaki diyagramda, hesap, kapsayıcılar ve sayfa BLOB'ları arasındaki genel ilişkileri açıklar.

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure1.png)

#### <a name="creating-an-empty-page-blob-of-a-specified-size"></a>Belirli bir boyuttaki bir boş sayfa blobu oluşturma
Bir sayfa blobu oluşturmak için önce oluşturduğumuz bir **CloudBlobClient** nesneyle depolama hesabınız için blob depolama alanına erişim için temel URI'sini (*pbaccount* Şekil 1'içinde) ile birlikte  **StorageCredentialsAccountAndKey** aşağıdaki örnekte gösterildiği gibi nesne. Örnek için bir başvuru oluşturmanın ardından gösterir bir **CloudBlobContainer** nesnesi ve ardından bir kapsayıcı oluşturma (*testvhds*) zaten mevcut değilse. Ardından kullanarak **CloudBlobContainer** nesne, bir başvuru bir **CloudPageBlob** erişim sayfa blob adı (os4.vhd) belirterek nesne. Sayfa blobu oluşturmak için arama [CloudPageBlob.Create](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.create?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudPageBlob_Create_System_Int64_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) blob oluşturmak için en büyük boyutu tümleştirilmesidir. *BlobSize* 512 baytın katlarından biri olmalıdır.

```csharp
using Microsoft.WindowsAzure.StorageClient;
long OneGigabyteAsBytes = 1024 * 1024 * 1024;
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("testvhds");

// Create the container if it doesn't already exist.
container.CreateIfNotExists();

CloudPageBlob pageBlob = container.GetPageBlobReference("os4.vhd");
pageBlob.Create(16 * OneGigabyteAsBytes);
```

#### <a name="resizing-a-page-blob"></a>Bir sayfa blobu yeniden boyutlandırma
Oluşturulduktan sonra bir sayfa blob'yeniden boyutlandırmak için [yeniden boyutlandırma](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.resize?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudPageBlob_Resize_System_Int64_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_) API. İstenen boyutu 512 baytın katlarından biri olmalıdır.
```csharp
pageBlob.Resize(32 * OneGigabyteAsBytes); 
```

#### <a name="writing-pages-to-a-page-blob"></a>Bir sayfa blobu yazma sayfalarına
Sayfaları yazmak için kullanın [CloudPageBlob.WritePages](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.beginwritepages?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudPageBlob_BeginWritePages_System_IO_Stream_System_Int64_System_String_Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_System_AsyncCallback_System_Object_) yöntemi.  Bu sayfaları 4MBs kadar sıralı bir dizi yazmanıza olanak sağlar. Yazılmakta olan uzaklığı bir 512 bayt sınırlarında başlaması gerekir (startingOffset % 512 == 0) ve 512 sınır - 1 sonlandır.  Aşağıdaki kod örneğinde nasıl çağrılacağını gösterir **WritePages** bir blob için:

```csharp
pageBlob.WritePages(dataStream, startingOffset); 
```

Sıralı bir dizi sayfası yazma isteği blob hizmetinde başarılı olur ve dayanıklılık ve dayanıklılık sağlamak amacıyla çoğaltılır hemen sonra yazma tamamlandığı ve başarı istemciye döndürülür.  

Diyagramda gösterildiği 2 ayrı yazma işlemleri:

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure2.png)

1.  Başlayan bir yazma işlemi 0 uzunluğu 1024 bayt uzaklığı. 
2.  1024 uzunluğu 4096 başlayan bir yazma işlemi uzaklığı 

#### <a name="reading-pages-from-a-page-blob"></a>Bir sayfa blobu okuma sayfaları
Sayfaları okumak için kullandığınız [CloudPageBlob.DownloadRangeToByteArray](/dotnet/api/microsoft.windowsazure.storage.blob.icloudblob.downloadrangetobytearray?view=azure-dotnet) sayfa blobu bayt aralığı okumak için yöntem. Bu, tam blob veya herhangi bir uzaklık BLOB başlayarak bir bayt aralığı indirmenizi sağlar. Okuma sırasında katlarından biri 512 başlatmak uzaklık yok. Hizmeti, NUL sayfasından bayt okuma, sıfır bayt döndürür.
```csharp
byte[] buffer = new byte[rangeSize];
pageBlob.DownloadRangeToByteArray(buffer, bufferOffset, pageBlobOffset, rangeSize); 
```
Aşağıdaki şekilde BlobOffSet 256 ve rangeSize 4352, bir okuma işlemi gösterilmektedir. Döndürülen veriler turuncu vurgulanır. NUL sayfaları için sıfır döndürülür

![](./media/storage-blob-pageblob-overview/storage-blob-pageblob-overview-figure3.png)

Seyrek doldurulmuş bir blob varsa, yalnızca sıfır bayt egressing için ödeme yapmaktan kaçınmak üzere ve indirme gecikme süresini azaltmak için geçerli bir sayfa bölgeleri indirmek isteyebilirsiniz.  Hangi sayfaların veri tarafından desteklenen belirlemek için [CloudPageBlob.GetPageRanges](/dotnet/api/microsoft.windowsazure.storage.blob.cloudpageblob.getpageranges?view=azure-dotnet). Ardından, döndürülen aralıkları numaralandırır ve her aralıkta verilerin indirin. 
```csharp
IEnumerable<PageRange> pageRanges = pageBlob.GetPageRanges();

foreach (PageRange range in pageRanges)
{
    // Calculate the rangeSize
    int rangeSize = (int)(range.EndOffset + 1 - range.StartOffset);

    byte[] buffer = new byte[rangeSize];

    // Read from the correct starting offset in the page blob and place the data in the bufferOffset of the buffer byte array
    pageBlob.DownloadRangeToByteArray(buffer, bufferOffset, range.StartOffset, rangeSize); 

    // TODO: use the buffer for the page range just read
} 
```

#### <a name="leasing-a-page-blob"></a>Sayfa blob kiralama
Blob kira işlemi oluşturur ve yazma için bir blob kilit yönetir ve silme işlemleri. Bu işlem yalnızca bir istemci aynı anda bloba yazabilirsiniz emin olmak için birden çok istemciden gelen bir sayfa blobu burada erişiliyor senaryolarda yararlıdır. Azure diskleri gibi yararlanır bu kiralama mekanizması disk yalnızca tek bir VM tarafından yönetildiğinden emin olmak için. Kilit süresi 15 ila 60 saniye olabilir veya sonsuz olabilir. Belgelere bakın [burada](/rest/api/storageservices/lease-blob) daha fazla ayrıntı için.

Zengin REST API'lerine ek olarak, sayfa blobları paylaşılan erişim, dayanıklılık ve Gelişmiş güvenlik sağlar. Biz bu avantajlar daha ayrıntılı sonraki paragraf olarak ele alınacaktır. 

### <a name="concurrent-access"></a>Eş zamanlı erişim
REST API sayfa blobları ve onun bir kiralama mekanizmasının uygulamaların birden çok istemcilerden sayfa blob'u erişmesine izin verir. Örneğin, birden çok kullanıcının bulunduğu depolama nesneleri paylaşan bir dağıtılmış bulut hizmeti oluşturmak ihtiyacınız varsayalım. Büyük bir görüntü koleksiyonunu birkaç kullanıcıya hizmet veren bir web uygulaması olabilir. Bu uygulama için bir seçenek, bağlı diskleri olan bir sanal makine kullanmaktır. Bu ekleme downsides (i) bir disk böylece sınırlama ölçeklenebilirlik, esneklik ve riskleri artırılması tek bir VM için yalnızca eklenebilecek kısıtlaması. Sanal makine veya sanal makinede çalışan hizmeti ile ilgili bir sorun varsa, kiralama süresi dolana veya sözleşme bozuk kadar ardından kira nedeniyle görüntü erişilemez; ve (ii) sahip bir Iaas VM ek maliyeti. 

Alternatif bir seçenek, Azure depolama REST API'leri aracılığıyla doğrudan sayfa BLOB'ları kullanmaktır. Bu seçenek maliyetli Iaas Vm'leri ihtiyacını ortadan kaldırır, birden çok istemcilerden doğrudan erişim tam esneklik sunar, diskleri İliştir/Ayır gereksinimini ortadan kaldırarak Klasik dağıtım modeli basitleştirir ve VM'de sorunları riskini ortadan kaldırır. Ve aynı disk olarak rastgele okuma/yazma işlemleri için performans düzeyini sağlar

### <a name="durability-and-high-availability"></a>Dayanıklılık ve yüksek kullanılabilirlik
Standart ve premium depolama olan dayanıklı depolama burada sayfa blob verileri her zaman dayanıklılık ve yüksek kullanılabilirlik sağlamak için çoğaltılır. Azure depolama Yedekliliği hakkında daha fazla bilgi için bkz. Bu [belgeleri](../common/storage-redundancy.md). Azure tutarlı bir şekilde teslim Kurumsal düzeyde dayanıklılık Iaas diskler ve sayfa blobları, sektör lideri ile sıfır % [değer yıllık hata oranı](https://en.wikipedia.org/wiki/Annualized_failure_rate). Diğer bir deyişle, Azure, bir müşterinin sayfa blob verilerini hiçbir zaman kaybetti. 

### <a name="seamless-migration-to-azure"></a>Azure'a sorunsuz geçiş
Müşteriler ve kendi özelleştirilmiş yedekleme çözümü uygulamak istiyorsanız geliştiriciler için Azure yalnızca deltaları tutun artımlı anlık görüntüleri de sunar. Bu özellik, büyük ölçüde yedekleme maliyetini düşürür ilk tam kopya maliyetini ortadan kaldırır. Verimli bir şekilde okunur ve kopyalama fark verilere becerisinin yanı sıra, bu, Azure üzerinde bir sınıfının en iyi yedekleme ve olağanüstü durum kurtarma (DR) deneyimi baştaki geliştiricilerden çok daha fazla yeniliklerini sağlayan başka bir güçlü bir özelliktir. Kullanarak Azure üzerinde sanal makineleriniz için kendi yedekleme veya DR çözümü ayarlayabilirsiniz [Blob anlık görüntüsü](/rest/api/storageservices/snapshot-blob) ile birlikte [alma sayfası aralıkları](/rest/api/storageservices/get-page-ranges) API ve [artımlı kopya blob'u](/rest/api/storageservices/incremental-copy-blob) yapabileceğiniz API DR için kolayca artımlı veri kopyalamak için kullanın. 

Ayrıca, birçok kuruluş zaten şirket içi veri merkezlerinde çalışan kritik iş yükleri vardır. İş yükünü buluta geçirme için başlıca endişelerinden biri kapalı kalma süresini veriler ve öngörülemeyen sorunları riskini sonra geçiş kopyalamak için gerekli olacaktır. Çoğu durumda, buluta geçiş için bir showstopper kapalı kalma süresi olabilir. REST API kullanarak sayfa blobları, Azure, kritik iş yükleri için en az kesinti ile buluta geçiş sağlayarak bu sorunu çözüyor. 

Bir anlık görüntünün nasıl alınacağı ve bir sayfa blobu anlık görüntüden geri yükleme hakkında daha fazla örnek için bkz [bir yedekleme işlemi artımlı anlık görüntülerini kullanarak kurulum](../../virtual-machines/windows/incremental-snapshots.md) makalesi.
