---
title: "Azure Depolama hesabı seçenekleri | Microsoft Docs"
description: "Azure Depolama kullanma seçeneklerini anlama."
services: storage
author: jirwin
manager: jwillis
ms.service: storage
ms.workload: storage
ms.topic: get-started-article
ms.date: 01/17/2018
ms.author: jirwin
ms.openlocfilehash: bdbcdc7d46d5395b28cf9ba7066703ce5da900a5
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="azure-storage-account-options"></a>Azure Depolama hesabı seçenekleri

## <a name="overview"></a>Genel Bakış
Azure Depolama, farklı fiyatlar ve desteklenen özelliklerle üç ayrı hesap seçeneği sağlar. Uygulamalarınıza yönelik en iyi seçeneği belirlemek için depolama hesabı oluşturmadan önce bu farklılıkları göz önünde bulundurun. Bu üç farklı depolama hesabı seçeneği şunlardır:

* **Genel amaçlı v2 (GPv2)** hesapları 
* **Genel amaçlı v1 (GPv1)** hesapları
* **Blob depolama** hesapları

Aşağıdaki bölümde her hesap türü daha ayrıntılı olarak açıklanmıştır:

## <a name="storage-account-options"></a>Depolama hesabı seçenekleri

### <a name="general-purpose-v2"></a>Genel amaçlı v2

Genel amaçlı v2 (GPv2) hesapları bloblar, dosyalar, kuyruklar ve tablolar için en yeni özelliklerin tümünü destekleyen depolama hesaplarıdır. GPv2 hesapları, GPv1 ve Blob depolama hesaplarında desteklenen tüm API’leri ve özellikleri destekler. Bunlar, ilgili hesap türlerindeki dayanıklılık, kullanılabilirlik, ölçeklenebilirlik ve performans özelliklerini de destekler. GPv2 hesapları için fiyatlandırma, gigabayt başına en düşük fiyatları ve sektörle rekabet edebilecek düzeyde işlem fiyatları sunmak üzere tasarlanmıştır.

PowerShell veya Azure CLI kullanarak GPv1 hesabınızı bir GPv2 hesabına yükseltebilirsiniz. 

Bir GPv2 depolama hesabındaki blok blobları için hesap düzeyinde sık ve seyrek erişimli depolama katmanlarından birini, blob düzeyinde ise erişim düzenleri temelinde sık erişimli, seyrek erişimli ve arşiv katmanlarından birini seçebilirsiniz. Maliyetleri iyileştirmek için sık, seyrek ve nadiren erişilen verileri sırasıyla sık, seyrek ve arşiv depolama katmanlarında depolayın. 

GPv2 depolama hesapları, **Erişim Katmanı** özniteliğini hesap düzeyinde kullanıma sunar ve böylece varsayılan depolama hesabı katmanı **Sık Erişimli** veya **Seyrek Erişimli** olarak tanımlanır. Varsayılan depolama hesabı katmanı, blob düzeyinde ayarlanmış açık bir katmanı olmayan tüm bloblara uygulanır. Verilerinizin kullanım düzeninde bir değişiklik olursa herhangi bir zamanda bu depolama katmanları arasında geçiş yapabilirsiniz. **Arşiv katmanı** yalnızca blob düzeyinde uygulanabilir.

> [!NOTE]
> Depolama katmanının değiştirilmesi ek ücretlere neden olabilir. Daha fazla bilgi için [Fiyatlandırma ve faturalandırma](#pricing-and-billing) bölümüne bakın.
>
> Microsoft, çoğu senaryoda Blob depolama hesaplarındansa genel amaçlı v2 depolama hesaplarının kullanılmasını önerir.

### <a name="upgrade-a-storage-account-to-gpv2"></a>Bir depolama hesabını GPv2’ye yükseltme

Kullanıcılar, istedikleri zaman PowerShell veya Azure CLI aracılığıyla bir GPv1 hesabını GPv2 hesabına yükseltebilir. Bu değişiklik geri alınamaz ve başka değişikliklere izin verilmez.

#### <a name="upgrade-with-powershell"></a>Powershell ile yükseltme

PowerShell kullanarak bir GPv1 hesabını GPv2 hesabına yükseltmek için önce PowerShell’i **AzureRm.Storage** modülünün en son sürümünü kullanacak şekilde güncelleştirin. PowerShell’i yükleme hakkında bilgi edinmek için bkz. [Azure PowerShell’i yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Sonra, hesabı yükseltmek için kaynak grubunuzun ve depolama hesabınızın adını değiştirerek şu komuta çağrı yapın:

```powershell
Set-AzureRmStorageAccount -ResourceGroupName <resource-group> -AccountName <storage-account> -UpgradeToStorageV2
```

#### <a name="upgrade-with-azure-cli"></a>Azure CLI ile yükseltme

Azure CLI kullanarak bir GPv1 hesabını GPv2’ye yükseltmek için önce en son Azure CLI sürümünü yükleyin. CLI yüklemesi hakkında bilgi için bkz. [Azure CLI 2.0’ı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Sonra, hesabı yükseltmek için kaynak grubunuzun ve depolama hesabınızın adını değiştirerek şu komuta çağrı yapın:

```cli
az storage account update -g <resource-group> -n <storage-account> --set kind=StorageV2
```` 

### <a name="general-purpose-v1"></a>Genel amaçlı v1

Genel amaçlı v1 (GPv1) hesapları, tüm Azure Depolama Hizmetlerine erişim sağlar, ancak en son özelliklere veya gigabayt başına en düşük fiyatlandırmaya sahip olmayabilir. Örneğin, GPv1’de seyrek erişimli depolama ve arşiv depolama desteklenmez. GPv1 işlemlerinin ücreti daha düşük olduğundan, yüksek karmaşıklık veya yüksek okuma oranlı iş yükleri bu hesap türünden yararlanabilir.

Genel amaçlı v1 (GPv1) depolama hesapları, en eski depolama hesabı türüdür ve klasik dağıtım modeliyle kullanılabilen tek türdür. 

### <a name="blob-storage-accounts"></a>Blob Storage hesapları

Blob depolama hesapları, GPv2 hesaplarındaki tüm blok blobu özelliklerini destekler, ancak yalnızca blok bloblarını desteklemekle sınırlıdır. Fiyatlandırma, büyük ölçüde genel amaçlı v2 hesaplarının fiyatlandırması gibidir. Müşteriler Blob depolama hesapları ile GPv2 arasındaki fiyat farklarını gözden geçirmeli ve GPv2’ye yükseltmeyi göz önünde bulundurmalıdır. Bu yükseltme geri alınamaz.

Blob depolama hesaplarını GPv2’ye yükseltme olanağı yakında sunulacaktır.

> [!NOTE]
> Blob Storage hesapları yalnızca blok ve ilave bloblarını destekler, sayfa bloblarını desteklemez.

## <a name="recommendations"></a>Öneriler

Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesapları hakkında](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

Yalnızca blok veya ekleme blobu depolamayı gerektiren uygulamalarda katmanlı depolamanın farklı fiyat modelinden yararlanmak için GPv2 depolama hesapları kullanılması önerilir. Bununla birlikte, aşağıdaki gibi belirli senaryolar için GPv1 kullanmak isteyebilirsiniz:

* Hala klasik dağıtım modelini kullanmanız gerekiyor. Blob Storage hesapları yalnızca Azure Resource Manager dağıtım modeli aracılığıyla kullanılabilir.

* Yüksek hacimlerde işlemler yapıyor veya coğrafi çoğaltma bant genişliği kullanıyorsunuz, bunların ikisi de GPv2 ve Blob depolama hesaplarında GPv1’e göre daha pahalı ve ayrıca düşük maliyetli GB depolamadan yararlanmak için yeterli depolama alanınız yok.

* 2014-02-14 tarihinden önceki [Storage Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) sürümünü veya 4.x’ten düşük bir istemci kitaplığı sürümü ile kullanmanız ve uygulamanızı güncelleştirememeniz.

> [!NOTE]
> Blob depolama hesapları şu anda çoğu Azure bölgesinde desteklenmektedir.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama
Tüm depolama hesapları, blob depolama için her blobun katmanını temel alan bir fiyatlandırma modelini kullanır. Bir depolama hesabını kullanırken aşağıdaki fatura değerlendirmeleri geçerlidir:

* **Depolama maliyetleri**: Depolanan veri miktarına ek olarak, veri depolamanın maliyeti depolama katmanına bağlı olarak değişir. Katmanın erişim sıklığı düştükçe gigabayt başına ücret de azalır.

* **Veri erişimi maliyetleri**: Katmanın erişimi sıklığı düştükçe veri erişimi ücretleri artar. Seyrek erişimli depolama ve arşiv depolama katmanındaki verilerde, okuma işlemleri için erişilen gigabayt veri başına ücretlendirilirsiniz.

* **İşlem maliyetleri**: Tüm katmanlarda, erişim sıklığı düştükçe artan bir işlem başına ücret uygulanır.

* **Coğrafi Çoğaltma veri aktarımı maliyetleri**: Bu ücret, GRS ve RA-GRS dahil olmak üzere yalnızca coğrafi çoğaltma yapılandırılmış hesaplara uygulanır. Coğrafi çoğaltma veri aktarımı gigabayt başına ücret doğurur.

* **Giden veri aktarımı maliyetleri**: Giden veri aktarımları (bir Azure bölgesinin dışına aktarılan veriler), genel amaçlı depolama hesapları ile tutarlı şekilde gigabayt başına esaslı olarak bant genişliği kullanımı için fatura doğurur.

* **Depolama katmanını değiştirme**: Hesap depolama katmanını seyrek erişimliden sık erişimliye değiştirmek, depolama hesabında mevcut tüm verilerin okunmasına eşit bir ücret doğurur. Ancak, hesap depolama katmanını sık erişilenden seyrek erişilene değiştirmek, tüm verileri seyrek erişilen katmana yazma (yalnızca GPv2 hesapları) maliyetine eşit bir ücret yansıtır.

> [!NOTE]
> Blob depolama hesaplarına ilişkin fiyatlandırma modeli hakkında daha fazla bilgi için [Azure Depolama Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) sayfasına bakın. Giden veri aktarımı ücretlerine ilişkin daha fazla bilgi için [Veri Aktarımları Fiyatlandırma Bilgileri](https://azure.microsoft.com/pricing/details/data-transfers/) sayfasına bakın.

## <a name="quickstart-scenarios"></a>Hızlı Başlangıç senaryoları

Bu bölümde Azure portalı kullanarak aşağıdaki senaryolar gösterilmektedir:

* GPv2 depolama hesabı oluşturma.
* GPv1 veya Blob depolama hesabını GPv2 depolama hesabına dönüştürme.
* GPv2 depolama hesabında hesap ve blob katmanı ayarlama.

Bu ayar tüm depolama hesabına uygulandığından aşağıdaki örneklerde erişim katmanı arşiv olarak ayarlayamazsınız. Arşiv katmanını yalnızca belirli bir blob için ayarlayabilirsiniz.

### <a name="create-a-gpv2-storage-account-using-the-azure-portal"></a>Azure portalını kullanarak GPv2 depolama hesabı oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Hub menüsünde, **Yeni** > **Veri + Depolama** > **Depolama hesabı**’nı seçin.

3. Depolama hesabınız için bir ad girin.

    Bu ad genel olarak benzersiz olmalıdır; depolama hesabındaki nesnelere erişmek için kullanılan URL’nin bir parçası olarak kullanılır.  

4. Dağıtım modeli olarak **Kaynak Yöneticisi**’ni seçin.

    Katmanlı depolama yalnızca Resource Manager depolama hesaplarıyla birlikte kullanılabilir; yeni kaynaklar için Resource Manager dağıtım modeli önerilir. Daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../../azure-resource-manager/resource-group-overview.md).  

5. **Hesap Türü** açılan listesinde **Genel amaçlı v2**’yi seçin.

    GPv2’yi seçtiğinizde performans katmanı Standart olarak ayarlanır. Katmanlı depolama, Premium performans katmanı ile kullanılamaz.

6. Depolama hesabı için çoğaltma seçeneğini seçin: **LRS**, **ZRS**, **GRS** veya **RA-GRS**. Varsayılan seçenek **RA-GRS**’dir.

    LRS = yerel olarak yedekli depolama; ZRS = bölgesel olarak yedekli depolama; GRS = coğrafi olarak yedekli depolama (iki bölge); RA-GRS okuma erişimli, coğrafi olarak yedekli depolama (ikincisine okuma erişiminin bulunduğu 2 bölge).

    Azure Storage çoğaltma seçenekleri ile ilgili ayrıntılar için bkz. [Azure Storage çoğaltma](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

7. Gereksinimlerinize uygun depolama katmanını seçin: **Erişim katmanı** ayarını **Seyrek Erişimli** veya **Sık Erişimli** olarak belirleyin. Varsayılan seçenek **Sık Erişimli**’dir.

8. Yeni depolama hesabını oluşturmak istediğiniz aboneliği seçin.

9. Yeni bir kaynak grubu belirtin veya varolan bir kaynak grubunu seçin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../../azure-resource-manager/resource-group-overview.md).

10. Depolama hesabınız için bölge seçin.

11. Depolama hesabını oluşturmak için **Oluştur**’a tıklayın.

### <a name="convert-a-gpv1-account-to-a-gpv2-storage-account-using-the-azure-portal"></a>Azure portalını kullanarak bir GPv1 hesabını GPv2 depolama hesabına dönüştürme

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Depolama hesabınıza gitmek için Tüm Kaynaklar’ı ve ardından depolama hesabınızı seçin.

3. Ayarlar bölümünde **Yapılandırma**’ya tıklayın.

4. **Hesap Türü** altında **Yükselt**’e tıklayın.

5. **Yükseltmeyi Onayla** altında, hesabınızın adını yazın. 

5. Dikey pencerenin alt kısmında Yükselt’e tıklayın.

### <a name="change-the-storage-tier-of-a-gpv2-storage-account-using-the-azure-portal"></a>Azure portalını kullanarak GPv2 depolama hesabının depolama katmanını değiştirme

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Depolama hesabınıza gitmek için Tüm Kaynaklar’ı ve ardından depolama hesabınızı seçin.

3. Ayarlar dikey penceresinde **Yapılandırma**’ya tıklayarak hesap yapılandırmasını görüntüleyin ve/veya değiştirin.

4. Gereksinimlerinize uygun depolama katmanını seçin: **Erişim katmanı** ayarını **Seyrek Erişimli** veya **Sık Erişimli** olarak belirleyin.

5. Dikey pencerenin en üstündeki Kaydet seçeneğine tıklayın.

### <a name="change-the-storage-tier-of-a-blob-using-the-azure-portal"></a>Azure portalı kullanarak blobun depolama katmanını değiştirme

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Depolama hesabınızdaki blobunuza gitmek için Tüm Kaynaklar’ı seçin, depolama hesabınızı seçin, kapsayıcınızı seçin ve ardından blobunuzu seçin.

3. Blob özellikleri dikey penceresinde **Erişim Katmanı** açılan menüsüne tıklayarak **Sık Erişilen**, **Seyrek Erişilen** veya **Arşiv** depolama katmanını seçin.

5. Dikey pencerenin en üstündeki Kaydet seçeneğine tıklayın.

> [!NOTE]
> Depolama katmanının değiştirilmesi ek ücretlere neden olabilir. Daha fazla bilgi için [Fiyatlandırma ve Faturalandırma](#pricing-and-billing) bölümüne bakın.


## <a name="evaluating-and-migrating-to-gpv2-storage-accounts"></a>GPv2 depolama hesaplarını değerlendirme ve geçiş yapma
Bu bölümün amacı, kullanıcıların GPv2 depolama hesaplarını (GPv1 yerine) kullanmaya sorunsuz bir geçiş yapmasına yardımcı olmaktır. İki kullanıcı senaryosu vardır:

* Hazır bir GPv1 depolama hesabınız var ve doğru depolama katmanı ile GPv2 depolama hesabına geçişi değerlendirmek istiyorsunuz.
* GPv2 depolama hesabı kullanmaya karar verdiniz veya zaten bir hesabınız var ve sık veya seyrek erişimli depolama katmanlarından hangisini kullanacağınıza karar vermek istiyorsunuz.

Her iki durumda da birinci öncelik GPv2 depolama hesabında depolanan verilerinizin depolama ve erişim maliyetini tahmin etmek ve bu maliyeti mevcut maliyetlerinizle karşılaştırmaktır.

## <a name="evaluating-gpv2-storage-account-tiers"></a>GPv2 depolama hesabı katmanlarını değerlendirme

GPv2 depolama hesabına depolanan verilerin depolama ve erişim maliyetini tahmin etmek için var olan kullanım modelinizi değerlendirmeniz ya da beklenen kullanım modelinizi tahmin etmeniz gerekir. Genel olarak, şunları bilmek istersiniz:

* Depolama tüketiminiz – Ne kadar veri depolanıyor ve bu aylık olarak nasıl değişiyor?

* Depolama erişim modelleriniz - Hesaptan ne kadar veri okunuyor ve hesaba ne kadar veri yazılıyor (yeni veriler dahil)? Veri erişimi için kaç tane işlem kullanılıyor ve bunlar ne tür işlemler?

## <a name="monitoring-existing-storage-accounts"></a>Var olan depolama hesaplarını izleme

Var olan depolama hesaplarınızı izlemek ve bu verileri toplamak için, bir depolama hesabına yönelik günlüğe kaydetme işlemlerini gerçekleştiren ve ölçümler sağlayan Azure Depolama Analizi hizmetinden yararlanabilirsiniz. Depolama Analizi GPv1, GPv2 ve Blob depolama hesap türleri için toplu işlem istatistiklerini içeren ölçümleri ve depolama hizmetine yapılan isteklere ilişkin kapasite verilerini depolayabilir. Bu veriler aynı depolama hesabındaki iyi bilinen tablolara depolanır.

Daha fazla bilgi için bkz. [Storage Analytics Ölçümleri hakkında](https://msdn.microsoft.com/library/azure/hh343258.aspx) ve [Storage Analytics Ölçüm Tablosu Şeması](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [!NOTE]
> Blob depolama hesapları, Tablo hizmeti uç noktasını yalnızca ilgili hesabın ölçüm verilerini depolamak ve bunlara erişmek için kullanıma sunar. Bölgesel olarak yedekli depolama (ZRS) hesapları ölçüm verilerinin toplanmasını desteklerken ZRS Klasik depolama hesapları desteklemez. ZRS hakkında daha fazla bilgi için bkz. [Bölgesel olarak yedekli depolama](storage-redundancy.md#zone-redundant-storage). 

Blob depolamada depolama tüketimini izlemek için kapasite ölçümlerini etkinleştirmeniz gerekir.
Bu özellik etkinleştirildiğinde bir depolama hesabının Blob hizmeti için kapasite verileri günlük olarak kaydedilir ve aynı depolama hesabı içindeki *$MetricsCapacityBlob* tablosuna yazılan bir tablo girişi olarak kaydedilir.

Blob depolama hizmetinin veri erişim desenlerini izlemek için API’den saatlik işlem ölçümlerini etkinleştirmeniz gerekir. Saatlik işlem ölçümleri etkinleştirildiğinde API başına işlemler saatte bir toplanır ve aynı depolama hesabındaki *$MetricsHourPrimaryTransactionsBlob* tablosuna yazılan bir tablo girişi olarak kaydedilir. RA-GRS depolama hesapları kullanılırken *$MetricsHourSecondaryTransactionsBlob* tablosu, işlemleri ikincil uç noktaya kaydeder.

> [!NOTE]
> Blok ve ekleme blobu verileriyle birlikte sayfa bloblarını ve sanal makine disklerini veya kuyrukları, dosyaları ya da tabloları depoladığınız genel amaçlı bir depolama hesabınız olması durumunda bu tahmin işlemi geçerli değildir. Kapasite verileri blok bloblarını diğer türlerden ayırt etmediğinden, diğer veri türleri için kapasite verileri sunmaz. Bu türleri kullanıyorsanız alternatif bir yöntem de en son faturanızdaki miktarlara bakmaktır.

Veri tüketim ve erişim modelinizi yaklaşık olarak tahmin etmek için, ölçümler için düzenli kullanımınızı temsil eden bir elde tutma süresi seçmeniz ve tahmin etmeniz önerilir. Seçeneklerden biri son yedi güne ait ölçüm verilerinin tutulması ve verilerin ay sonunda analiz için haftada bir toplanmasıdır. Diğer bir seçenek ise son 30 güne ait ölçüm verilerinin tutulması ve verilerin 30 günlük süre sonunda toplanıp çözümlenmesidir.

Ölçüm verilerini etkinleştirme, toplama ve görüntüleme hakkında bilgi için bkz. [Azure Depolama ölçümlerini etkinleştirme ve ölçüm verilerini görüntüleme](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

> [!NOTE]
> Analiz verilerinin depolanması, erişimi ve indirilmesi de normal kullanıcı verileri gibi ücretlendirilir.

### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Kullanım ölçümlerinden yararlanarak maliyetleri tahmin etme

#### <a name="storage-costs"></a>Depolama maliyetleri

*$MetricsCapacityBlob* kapasite ölçüm tablosunda *'data'* satır anahtarını içeren en son giriş, kullanıcı verilerinin harcadığı kapasiteyi gösterir. *$MetricsCapacityBlob* kapasite ölçüm tablosunda *'analytics'* satır anahtarını içeren en son giriş, analiz günlüklerinin harcadığı kapasiteyi gösterir.

Hem kullanıcı verileri hem de analiz günlükleri (etkinse) tarafından kullanılan bu toplam kapasite, verileri depolama hesabına depolama maliyetini tahmin etmek için kullanılabilir. Aynı yöntem ayrıca GPv1 depolama hesaplarında depolanma maliyetlerini tahmin etmek için kullanılabilir.

#### <a name="transaction-costs"></a>İşlem maliyetleri

İşlem ölçüm tablosundaki bir API’nin tüm girişleri için *'TotalBillableRequests'* toplamı, ilgili API’nin toplam işlem sayısını belirtir. *Örneğin*, belirli bir süre içindeki *'GetBlob'* işlemlerinin toplam sayısı *'user;GetBlob'* satır anahtarını içeren tüm girişlere yönelik toplam faturalandırılabilir isteklerin toplamına göre hesaplanabilir.

Blob depolama hesaplarına ilişkin işlem maliyetlerini tahmin etmek için, farklı şekilde fiyatlandırıldıkları için işlemleri üç gruba ayırmanız gerekir.

* *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'* ve *'CopyBlob'* gibi yazma işlemleri.
* *'DeleteBlob'* ve *'DeleteContainer'* gibi silme işlemleri.
* Diğer tüm işlemler.

GPv1 depolama hesaplarının işlem maliyetlerini tahmin etmek için işlemden/API’den bağımsız olarak tüm işlemleri toplamanız gerekir.

### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Veri erişimi ve coğrafi çoğaltma veri aktarımı maliyetleri

Storage Analytics bir depolama hesabından okunan ve depolama hesabına yazılan veri miktarını belirtmese de, işlem ölçümleri tablosuna bakılarak bu değer kabaca tahmin edilebilir. İşlem ölçüm tablosundaki bir API’nin tüm girişleri için *'TotalIngress'* toplamı, ilgili API’nin toplam giriş verileri miktarını bayt cinsinden belirtir. Benzer şekilde, *'TotalEgress'* toplamı toplam çıkış verileri miktarını bayt cinsinden belirtir.

Blob depolama hesaplarına ilişkin veri erişimi maliyetlerini hesaplamak için işlemleri iki gruba ayırmanız gerekir.

* Depolama hesabından alınan veri miktarı birincil olarak *'GetBlob'* ve *'CopyBlob'* işlemleri için *'TotalEgress'* toplamına bakılarak tahmin edilebilir.

* Depolama hesabına yazılan veri miktarı birincil olarak *'PutBlob'*, *'PutBlock'*, *'CopyBlob'* ve *'AppendBlock'* işlemleri için *'TotalIngress'* toplamına bakılarak tahmin edilebilir.

Blob depolama hesaplarında coğrafi çoğaltma veri aktarımı maliyeti de bir GRS veya RA-GRS depolama hesabı kullanılırken yazılan veri miktarı tahmini kullanılarak hesaplanabilir.

> [!NOTE]
> Seyrek veya sık erişimli bir depolama katmanını kullanma maliyetlerini hesaplama hakkında daha ayrıntılı bir örnek için *'Sık ve Seyrek Erişimli erişim katmanları nelerdir ve hangisinin kullanılacağını nasıl belirlemeliyim?'* başlıklı SSS bölümüne bakın bkz. [Azure Depolama Fiyatlandırma Sayfası](https://azure.microsoft.com/pricing/details/storage/).

## <a name="migrating-existing-data"></a>Mevcut verileri geçirme

Bir GPv1 hesabı, kesinti veya API değişiklikleri olmadan ve verilerin geçirilmesi gerekmeden kolayca GPv2’ye yükseltilebilir. Bu nedenle, GPv1 hesaplarını Blob depolama hesapları yerine GPv2 hesaplarına geçirmeniz önerilir.

Ancak, Blob depolama hesabına geçmeniz gerekiyorsa aşağıdaki yönergeleri kullanabilirsiniz.

Bir Blob Storage hesabı yalnızca blok ve ilave bloblarının depolanmasına yöneliktir. Blobların yanı sıra tablo, kuyruk, dosya ve diskleri de depolamanızı sağlayan mevcut genel amaçlı depolama hesapları Blob depolama hesaplarına dönüştürülemez. Depolama katmanlarını kullanmak için, yeni Blob depolama hesapları oluşturmanız ve mevcut verilerinizi yeni oluşturulan hesaplara taşımanız gerekir.

Mevcut verileri şirket içi depolama aygıtlarından, üçüncü taraf bulut depolama sağlayıcılardan ya da Azure’daki mevcut genel amaçlı depolama hesaplarınızdan Blob depolama hesaplarına geçirmek için aşağıdaki yöntemleri kullanabilirsiniz:

### <a name="azcopy"></a>AzCopy

AzCopy, verilerin Azure Storage’a ve Azure Storage’dan yüksek performansla kopyalanması için tasarlanmış bir Windows komut satırı yardımcı programıdır. AzCopy yardımcı programını, verileri genel amaçlı depolama hesaplarınızdan Blob Storage hesabınıza kopyalamak ya da şirket içi depolama aygıtlarınızdaki verileri Blob Storage hesabınıza yüklemek için kullanabilirsiniz.

Daha fazla bilgi için bkz. [AzCopy Komut Satırı Yardımcı Programı ile Veri Aktarma](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="data-movement-library"></a>Veri hareketi kitaplığı

.NET için Azure Depolama veri taşıma kitaplığı, AzCopy’yi çalıştıran çekirdek veri taşıma altyapısını temel alır. Kitaplık, AzCopy’ye benzer yüksek performanslı, güvenilir ve kolay veri aktarımı işlemleri için tasarlanmıştır. Bunu kullanarak AzCopy’nin dış örneklerini çalıştırmanıza ve izlemenize gerek kalmadan, AzCopy tarafından uygulamanızda yerel olarak sağlanan özelliklerden tam olarak faydalanabilirsiniz.

Daha fazla bilgi için bkz. [.Net için Azure Storage Veri Hareketi Kitaplığı](https://github.com/Azure/azure-storage-net-data-movement)

### <a name="rest-api-or-client-library"></a>REST API’si veya istemci kitaplığı

Azure istemci kitaplıklarından birini ya da Azure Storage hizmetleri REST API’sini kullanarak verilerinizi Blob Storage hesabına geçirmek için özel bir uygulama oluşturabilirsiniz. Azure Storage NET, Java, C++, Node.JS, PHP, Ruby ve Python gibi birden fazla dilde ve platformda zengin istemci kitaplıkları sağlar. İstemci kitaplıkları yeniden deneme mantığı, günlüğe kaydetme ve paralel karşıya yüklemeler gibi gelişmiş özellikler sunar. HTTP/HTTPS istekleri yapan herhangi bir dil tarafından çağrılabilen REST API’sine karşı doğrudan da geliştirebilirsiniz.

Daha fazla bilgi için bkz. [Azure Blob depolamayı kullanmaya başlayın](../blobs/storage-dotnet-how-to-use-blobs.md).

> [!NOTE]
> Bloblar, blobla depolanan istemci tarafı şifreleme depolama şifrelemesiyle ilgili meta veriler kullanılarak depolanır. Tüm kopyalama mekanizmalarının blob verilerinin ve özellikle şifrelemeyle ilgili meta verilerin korunduğundan emin olması önemlidir. Blobları bu meta veriler olmadan kopyalarsanız, blob içeriği tekrar alınamaz. Şifrelemeyle ilgili meta veriler hakkında daha fazla bilgi için bkz. [Azure Depolama İstemci Tarafı Şifrelemesi](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="faq"></a>SSS

**Mevcut depolama hesapları hâlâ kullanılabilir mi?**

Evet, mevcut depolama hesaplarınız (GPv1) hala kullanılabilir ve fiyatlandırma veya işlev açısından bir farklılık göstermez. GPv1 hesapları depolama katmanı seçme olanağına sahip değildir ve gelecekte katmanlama özelliğine sahip olmayacaktır.

**Neden ve ne zaman GPv2 depolama hesapları kullanmaya başlamalıyım?**

GPv2 depolama hesapları, sektörde rekabetçi işlem ve veri erişim maliyetleri sunarken en düşük GB depolama maliyetleri sağlamada uzmanlaşmıştır. Dahası, bu hesap türüne dayalı değişiklik bildirimleri gibi özellikler gelecekte sunulacağından GPv2 depolama hesapları blobları depolamak için önerilen yoldur. Ancak, iş gereksinimlerinize bağlı olarak ne zaman yükselteceğiniz size kalmıştır. Örneğin, yükseltmeden önce işlem modellerinizi iyileştirmeyi seçebilirsiniz.

GPv2’den indirgeme desteklenmediğinden, hesaplarınızı GPv2’ye yükseltmeden önce tüm fiyatlandırma etkilerini göz önünde bulundurun.

**Mevcut depolama hesabımı GPv2 depolama hesabına yükseltebilir miyim?**

Evet. GPv1 hesapları, portalda veya PowerShell ya da CLI kullanılarak kolayca GPv2’ye yükseltilebilir. Blob depolama hesapları PowerShell veya CLI kullanılarak GPv2’ye yükseltilebilir. Blob depolama hesaplarını portalda GPv2’ye yükseltme olanağı yakında sunulacaktır.

GPv2’den indirgeme desteklenmediğinden, hesaplarınızı GPv2’ye yükseltmeden önce tüm fiyatlandırma etkilerini göz önünde bulundurun.

**Nesneleri aynı hesaptaki iki depolama katmanında depolayabilir miyim?**

Evet. Hesap düzeyinde ayarlanan **Erişim Katmanı** özniteliği, bu hesapta bulunan ve katmanı açıkça belirlenmemiş olan tüm nesneler için geçerli varsayılan katmandır. Ancak blob düzeyinde katman ayarlama, hesabın erişim katmanı ayarından bağımsız olarak nesne düzeyinde erişim katmanını açık olarak ayarlamanıza olanak tanır. Aynı hesapta, üç depolama katmanının (sık erişilen, seyrek erişilen veya arşiv) tümüne ait bloblar bulunabilir.

**GPv2 depolama hesabımın depolama katmanını değiştirebilir miyim?**

Evet, depolama hesabındaki **Erişim Katmanı** özniteliğini ayarlayarak hesap depolama katmanını değiştirebilirsiniz. Hesap depolama katmanının değiştirilmesi, hesapta depolanmış ve açıkça katmanı belirtilmemiş tüm nesneler için geçerlidir. Sık erişilen olan depolama katmanının seyrek erişilen olarak değiştirilmesi, yazma işlemi (10.000 başına) maliyetleri doğurur (yalnızca GPv2 depolama hesaplarında). Seyrek erişilen olan depolama katmanının sık erişilen olarak değiştirilmesi ise hesaptaki tüm verilerin okunması için hem okuma işlemi (10.000 başına) hem de veri alma (GB başına) maliyetleri doğurur.

**Blob depolama hesabımdaki depolama katmanını hangi sıklıkta değiştirebilirim?**

Depolama katmanını değiştirme sıklığına ilişkin bir sınırlama koymuyoruz, ancak depolama katmanını seyrek erişimliden sık erişimliye değiştirmenin büyük maliyetler doğurduğuna dikkat edin. Depolama katmanını sık değiştirmeniz önerilmez.

**Seyrek erişimli depolama katmanındaki bloblar, sık erişimli depolama katmanındakilerden farklı mı davranır?**

GPv2 ve Blob depolama hesaplarının sık erişimli depolama katmanındaki bloblar, GPv1 depolama hesaplarındaki bloblarla aynı gecikme süresine sahiptir. Seyrek erişimli depolama katmanındaki bloblar sık erişimli katmandaki bloblarla benzer gecikme süresine (milisaniye olarak) sahiptir. Arşiv depolama katmanındaki bloblar, birkaç saatlik gecikme süresine sahiptir.

Seyrek erişimli depolama katmanındaki bloblar, sık erişimli depolama katmanında depolanan bloblara göre daha düşük kullanılabilirlik hizmet düzeyine (SLA) sahiptir. Daha fazla bilgi için bkz. [depolama SLA’sı](https://azure.microsoft.com/support/legal/sla/storage).

**Sayfa bloblarını ve sanal makine disklerini Blob depolama hesaplarında depolayabilir miyim?**

Hayır. Blob Storage hesapları yalnızca blok ve ilave bloblarını destekler, sayfa bloblarını desteklemez. Azure Virtual Machine diskleri sayfa blobları tarafından yedeklenir ve bu nedenle sanal makine disklerini depolamak için Blob Storage hesapları kullanılamaz. Ancak, sanal makine disklerinin yedeklerini blok blobları olarak Blob Storage hesabında depolamak mümkündür. Blob depolama hesapları yerine GPv2 kullanmayı dikkate alma nedenlerinden biri budur.

**GPv2 depolama hesaplarını kullanmak için mevcut uygulamalarımı değiştirmem gerekiyor mu?**

GPv2 depolama hesapları GPv1 ve Blob depolama hesapları ile %100 API uyumludur. Uygulamanız blok veya ilave bloblarını kullandığı ve [Depolama Hizmetleri REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx)’nin 2014-02-14 sürümünü veya üstünü kullandığınız sürece, uygulamanız çalışmaya devam edecektir. Protokolün daha eski bir sürümünü kullanıyorsanız, her iki tür depolama hesabıyla sorunsuz çalışarak yeni sürümü kullanmak için uygulamanızı güncelleştirmeniz gerekir. Genel olarak, hangi depolama hesabını kullandığınızdan bağımsız olarak her zaman en son sürümü kullanmanızı öneriyoruz.

İşlemler ve bant genişliği için GPv2 fiyatlandırması genellikle GPv1’den yüksektir. Bu nedenle, toplam faturanızın artmaması için yükseltmeden önce işlem modellerinizi iyileştirmeniz gerekebilir.

**Kullanıcı deneyiminde bir değişiklik olur mu?**

GPv2 depolama hesapları GPv1 depolama hesaplarına çok benzer ve yüksek dayanıklılık ve kullanılabilirlik, ölçeklenebilirlik, performans ve güvenlik dahil olmak üzere Azure Depolama’nın tüm temel özelliklerini destekler. GPv2’ye veya Blob depolamaya yükseltirken Blob depolama hesaplarına ve onun yukarıda bahsedilen depolama katmanlarına özgü özellikler ve kısıtlamalar dışındaki her şey aynı kalır.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="evaluate-blob-storage-accounts"></a>Blob Storage hesaplarını değerlendirme

[Bölgeye göre Blob depolama hesaplarının kullanılabilirliğini denetleme](https://azure.microsoft.com/regions/#services)

[Azure Depolama ölçümlerini etkinleştirerek geçerli depolama hesaplarınızın kullanımını değerlendirme](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Bölgeye göre Blob depolama fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/storage/)

[Veri aktarımı fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-gpv2-storage-accounts"></a>GPv2 depolama hesaplarını kullanmaya başlama

[Azure Blob depolamayı kullanmaya başlama](../blobs/storage-dotnet-how-to-use-blobs.md)

[Azure Depolama’ya ve Azure Depolama’da veri taşıma](../common/storage-moving-data.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[AzCopy Komut Satırı Yardımcı Programı ile veri aktarımı](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Depolama hesaplarınıza göz atma ve bu hesapları keşfetme](http://storageexplorer.com/)
