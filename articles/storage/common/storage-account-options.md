---
title: "Azure Depolama hesabı seçenekleri | Microsoft Docs"
description: "Azure Depolama kullanma seçeneklerini anlama."
services: storage
documentationcenter: 
author: jirwin
manager: jwillis
editor: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/11/2017
ms.author: jirwin
ms.openlocfilehash: 7f07734433694999d38429ca264c58c5f3c619e1
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="azure-storage-account-options"></a>Azure Depolama hesabı seçenekleri

## <a name="overview"></a>Genel Bakış
Azure Depolama, farklı fiyatlar ve desteklenen özelliklerle üç ayrı hesap seçeneği sağlar. Kullanıcıların uygulamaları için en iyi seçeneği belirlemek üzere bu farklılıkları dikkate almaları önemlidir.  Bu üç farklı seçenek aşağıdaki gibidir:

* **Genel Amaçlı v2 (GPv2)** hesapları, en son özelliklerin tümünü sağlar; Blob, Dosya, Kuyruk ve Tabloları destekler. Günümüzde, bu en yeni özellikler blob düzeyinde katman ayarlamayı, arşive depolamayı, daha yüksek ölçekli hesap sınırlarını ve depolama olaylarını içerir. Fiyatlandırma, en düşük GB fiyatları ve sektörde rekabetçi işlem fiyatları sunmak üzere tasarlanmıştır.

* **Blob Depolama** hesapları, blok bloblarına yönelik en son özelliklerin tümünü sağlar, ancak yalnızca Blok Bloblarını destekler.  Fiyatlandırma, Genel Amaçlı v2 fiyatına büyük ölçüde benzer. Çoğu kullanıcıya Blob Depolama hesaplarını kullanmak yerine Genel Amaçlı v2’yi kullanmasını öneririz.

* **Genel Amaçlı v1 (GPv1)** hesapları, tüm Azure Depolama Hizmetlerinin kullanılmasını sağlar, ancak en son özellikleri veya en düşük GB fiyatını içermeyebilir. Örneğin, seyrek ve arşiv depolama GPv1’de desteklenmez.  İşlemlerin ücreti daha düşük olduğundan yüksek karmaşıklık veya yüksek okuma oranlı iş yükleri bu hesap türünden yararlanabilir.

### <a name="changing-account-kind"></a>Hesap türünü değiştirme
Kullanıcılar GPv1 veya Blob Depolama hesaplarından GPv2 hesabına istedikleri zaman portal, CLI veya PowerShell aracılığıyla yükseltme yapabilirler. Bu değişiklik geri alınamaz ve başka değişikliklere izin verilmez.

## <a name="general-purpose-v2"></a>Genel Amaçlı v2
**Genel Amaçlı v2 (GPv2)** hesapları Bloblar, Dosyalar, Kuyruklar ve Tablolar dahil olmak üzere depolama hizmetlerinin tamamına yönelik tüm özellikleri destekleyen depolama hesaplarıdır. Blok Blobları için hesap düzeyinde sık ve seyrek erişimli depolama katmanlarından birini, blob düzeyinde ise erişim düzenleri temelinde sık erişimli, seyrek erişimli ve arşiv katmanlarından birini seçebilirsiniz. Maliyetleri iyileştirmek için sık, seyrek ve nadiren erişilen verileri sırasıyla sık, seyrek ve arşiv depolama katmanlarında depolayın. Hepsinden önemlisi, her GPv1 hesabı portal, CLI veya PowerShell aracılığıyla GPv2 hesabına yükseltilebilir. GPv2 hesapları tüm API'leri ve Blob Depolama ile GPv1 hesaplarında desteklenen özellikleri destekler, bu hesap türlerinde bulunan tüm o harika dayanıklılık, kullanılabilirlik, ölçeklenebilirlik ve performans özelliklerini paylaşır.

Blob Depolama hesapları, **Erişim Katmanı** özniteliğini hesap düzeyinde kullanıma sunar ve böylece varsayılan depolama hesabı katmanı **Sık Erişimli** veya **Seyrek Erişimli** olarak tanımlanır. Varsayılan depolama hesabı katmanı, blob düzeyinde ayarlanmış açık bir katmanı olmayan tüm bloblara uygulanır. Verilerinizin kullanım düzeninde bir değişiklik olursa herhangi bir zamanda bu depolama katmanları arasında geçiş yapabilirsiniz. **Arşiv katmanı** yalnızca blob düzeyinde uygulanabilir.

> [!NOTE]
> Depolama katmanının değiştirilmesi ek ücretlere neden olabilir. Lütfen daha fazla bilgi için [Fiyatlandırma ve faturalama](#pricing-and-billing) bölümüne bakın.

## <a name="blob-storage-accounts"></a>Blob Depolama Hesapları

**Blob Depolama hesapları** GPv2 ile aynı Blok Blobu özelliklerinin tümünü destekler, ancak yalnızca Blok Bloblarını desteklemekle sınırlıdır. Müşteriler Blob Depolama hesapları ile GPv2 arasındaki fiyat farklarını gözden geçirmeli ve GPv2’ye yükseltmeyi göz önünde bulundurmalıdır. Bu yükseltmenin geri alınamadığını unutmayın.

> [!NOTE]
> Blob Depolama hesapları, yalnızca blok ve ekleme bloblarını destekler, sayfa bloblarını desteklemez.

## <a name="general-purpose-v1"></a>Genel Amaçlı v1
**Genel Amaçlı v1 (GPv1)**, en eski depolama hesabıdır ve klasik dağıtım modelinde kullanılabilen tek türdür. Seyrek erişimli ve arşiv depolama gibi özellikler GPv1’de kullanılamaz. GPv1, genellikle hem GPv2’den hem de Blob Depolama hesaplarından daha yüksek GB depolama maliyetlerine ancak daha düşük işlem maliyetlerine sahiptir.

## <a name="recommendations"></a>Öneriler

Depolama hesapları hakkıında daha fazla bilgi için bkz. [Azure Storage hesapları hakkında](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

Yalnızca blok veya ekleme blobu depolamayı gerektiren uygulamalarda katmanlı depolamanın farklı fiyat modelinden yararlanmak için GPv2 depolama hesapları kullanılmasını öneriyoruz. Ancak, bunun GPv1 depolama hesapları kullanılmasının önerilebileceği aşağıdaki gibi bazı durumlarda mümkün olmayabileceğini de anlıyoruz:

* Hala klasik dağıtım modelini kullanmanız gerekiyor. Blob Depolama hesapları yalnızca Azure Resource Manager dağıtım modeli aracılığıyla kullanılabiliyor.

* Yüksek hacimlerde işlemler yapıyor veya coğrafi çoğaltma bant genişliği kullanıyorsunuz, bunların ikisi de GPv2 ve Blob Depolama hesaplarında GPv1’e göre daha pahalı ve ayrıca düşük maliyetli GB depolamadan yararlanmak için yeterli depolama alanınız yok.

* 2014-02-14 tarihinden önceki [Storage Services REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) sürümünü veya 4.x’ten düşük bir istemci kitaplığı sürümü ile kullanmanız ve uygulamanızı güncelleştirememeniz.

> [!NOTE]
> Blob Depolama hesapları şu anda Azure bölgelerinin tümünde desteklenmektedir.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama
Tüm depolama hesapları, blob depolama için her blobun katmanını temel alan bir fiyatlandırma modelini kullanır. Bir depolama hesabını kullanırken aşağıdaki fatura değerlendirmeleri geçerlidir:

* **Depolama maliyetleri**: Depolanan veri miktarına ek olarak, veri depolamanın maliyeti depolama katmanına bağlı olarak değişir. Katmanın erişim sıklığı düştükçe gigabayt başına ücret de azalır.

* **Veri erişimi maliyetleri**: Katmanın erişimi sıklığı düştükçe veri erişimi ücretleri artar. Seyrek erişimli depolama ve arşiv depolama katmanındaki verilerde, okuma işlemleri için erişilen gigabayt veri başına ücretlendirilirsiniz.

* **İşlem maliyetleri**: Tüm katmanlarda, erişim sıklığı düştükçe artan bir işlem başına ücret uygulanır.

* **Coğrafi Çoğaltma veri aktarımı maliyetleri**: Bu, yalnızca GRS ve RA-GRS dahil, coğrafi çoğaltma yapılandırılmış hesaplara uygulanır. Coğrafi çoğaltma veri aktarımı gigabayt başına ücret doğurur.

* **Giden veri aktarımı maliyetleri**: Giden veri aktarımları (bir Azure bölgesinin dışına aktarılan veriler), genel amaçlı depolama hesapları ile tutarlı şekilde gigabayt başına esaslı olarak bant genişliği kullanımı için fatura doğurur.

* **Depolama katmanını değiştirme**: Hesap depolama katmanını seyrek erişimliden sık erişimliye değiştirmek, depolama hesabında mevcut tüm verilerin okunmasına eşit bir ücret doğurur. Ancak, hesap depolama katmanını sık erişilenden seyrek erişilene değiştirmek, tüm verileri seyrek erişilen katmana yazma (yalnızca GPv2 hesapları) maliyetine eşit bir ücret yansıtır.

> [!NOTE]
> Blob Depolama hesaplarına ilişkin fiyatlandırma modeli hakkında daha fazla bilgi için bkz. [Azure Depolama Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) sayfası. Giden veri aktarımı ücretlerine ilişkin daha fazla bilgi için bkz. [Veri Aktarımları Fiyatlandırma Bilgileri](https://azure.microsoft.com/pricing/details/data-transfers/) sayfası.

## <a name="quickstart-scenarios"></a>Hızlı Başlangıç senaryoları

Bu bölümde Azure portalı kullanarak aşağıdaki senaryolar gösterilmektedir:

* GPv2 depolama hesabı oluşturma.
* GPv1 veya Blob Depolama hesabını GPv2 depolama hesabına dönüştürme.
* GPv2 depolama hesabında hesap ve blob katmanı ayarlama.

Bu ayar tüm depolama hesabına uygulandığından aşağıdaki örneklerde erişim katmanı arşiv olarak ayarlayamazsınız. Arşiv katmanını yalnızca belirli bir blob için ayarlayabilirsiniz.

### <a name="create-a-gpv2-storage-account-using-the-azure-portal"></a>Azure portalını kullanarak GPv2 depolama hesabı oluşturma

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Hub menüsünde, **Yeni** > **Veri + Depolama** > **Depolama hesabı**’nı seçin.

3. Depolama hesabınız için bir ad girin.

    Bu ad genel olarak benzersiz olmalıdır; depolama hesabındaki nesnelere erişmek için kullanılan URL’nin bir parçası olarak kullanılır.  

4. Dağıtım modeli olarak **Kaynak Yöneticisi**’ni seçin.

    Katmanlı depolama yalnızca Resource Manager depolama hesaplarıyla birlikte kullanılabilir; yeni kaynaklar için önerilen dağıtım modeli budur. Daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md).  

5. Hesap Türü açılan listesinde **Genel Amaçlı v2**’yi seçin.

    Depolama hesabının türünü buradan seçebilirsiniz. Katmanlı depolama genel amaçlı depolamada kullanılamaz; yalnızca Blob Depolama türündeki hesapta kullanılabilir.     

    Bunu seçtiğinizde performans katmanı Standart olarak ayarlanır. Katmanlı depolama, Premium performans katmanı ile kullanılamaz.

6. Depolama hesabı için çoğaltma seçeneğini seçin: **LRS**, **GRS** veya **RA-GRS**. Varsayılan seçenek **RA-GRS**’dir.

    LRS = yerel olarak yedekli depolama; GRS = coğrafi olarak yedekli depolama (iki bölge); RA-GRS okuma erişimli, coğrafi olarak yedekli depolama (ikincisine okuma erişiminin bulunduğu 2 bölge).

    Azure Depolama çoğaltma seçenekleri ile ilgili ayrıntılar için bkz. [Azure Depolama çoğaltma](../common/storage-redundancy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

7. Gereksinimlerinize uygun depolama katmanını seçin: **Erişim katmanı** ayarını **Seyrek Erişimli** veya **Sık Erişimli** olarak belirleyin. Varsayılan seçenek **Sık Erişimli**’dir.

8. Yeni depolama hesabını oluşturmak istediğiniz aboneliği seçin.

9. Yeni bir kaynak grubu belirtin veya varolan bir kaynak grubunu seçin. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../../azure-resource-manager/resource-group-overview.md).

10. Depolama hesabınız için bölge seçin.

11. Depolama hesabını oluşturmak için **Oluştur**’a tıklayın.

### <a name="convert-a-gpv1-or-blob-storage-account-to-a-gpv2-storage-account-using-the-azure-portal"></a>Azure Portalı'nı kullanarak GPv1 veya Blob Depolama hesabını GPv2 depolama hesabına dönüştürme

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Depolama hesabınıza gitmek için Tüm Kaynaklar’ı ve ardından depolama hesabınızı seçin.

3. Ayarlar dikey penceresinde **Yapılandırma**’ya tıklayın.

4. Hesap Türü altında **Yükselt**’e tıklayın.

5. Sağda yeni bir dikey pencere onaylama için görünür. Yükseltmeyi Onayla altında, hesabınızın adını yazın. 

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
> Depolama katmanının değiştirilmesi ek ücretlere neden olabilir. Lütfen daha fazla bilgi için [Fiyatlandırma ve Faturalama](#pricing-and-billing) bölümüne bakın.


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

Var olan depolama hesaplarınızı izlemek ve bu verileri toplamak için, bir depolama hesabına yönelik günlüğe kaydetme işlemlerini gerçekleştiren ve ölçümler sağlayan Azure Depolama Analizi hizmetinden yararlanabilirsiniz. Depolama Analizi GPv1, GPv2 ve Blob Depolama hesap türleri için toplu işlem istatistiklerini içeren ölçümleri ve depolama hizmetine yapılan isteklere ilişkin kapasite verilerini depolayabilir. Bu veriler aynı depolama hesabındaki iyi bilinen tablolara depolanır.

Daha fazla bilgi için bkz. [Storage Analytics Ölçümleri hakkında](https://msdn.microsoft.com/library/azure/hh343258.aspx) ve [Storage Analytics Ölçüm Tablosu Şeması](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [!NOTE]
> Blob Depolama hesapları, tablo hizmeti uç noktasını yalnızca ilgili hesabın ölçüm verilerini depolamak ve bunlara erişmek için ortaya çıkarır. GPv1 ZRS depolama hesapları ölçüm verilerini desteklemez.

Blob Depolama hizmetinin depolama tüketimini izlemek için kapasite ölçümlerini etkinleştirmeniz gerekir.
Bu özellik etkinleştirildiğinde bir depolama hesabının Blob hizmeti için kapasite verileri günlük olarak kaydedilir ve aynı depolama hesabı içindeki *$MetricsCapacityBlob* tablosuna yazılan bir tablo girişi olarak kaydedilir.

Blob Depolama hizmetinin veri erişim modelini izlemek için saatlik işlem ölçümlerini bir API düzeyinde etkinleştirmeniz gerekir. Bu özellik etkinleştirildiğinde API başına işlemler saatte bir toplanır ve aynı depolama hesabındaki *$MetricsHourPrimaryTransactionsBlob* tablosuna yazılan bir tablo girişi olarak kaydedilir. RA-GRS depolama hesapları kullanılırken *$MetricsHourSecondaryTransactionsBlob* tablosu, işlemleri ikincil uç noktaya kaydeder.

> [!NOTE]
> Blok ve ekleme blobu verileriyle birlikte sayfa bloblarını ve sanal makine disklerini veya kuyrukları, dosyaları ya da tabloları depoladığınız genel amaçlı bir depolama hesabınız varsa bu tahmin işlemi geçerli değildir. Bunun nedeni, kapasite verilerinin blok bloblarını diğer türlerden ayırt etmemesi ve diğer veri türleri için kapasite verileri vermemesidir. Bu türleri kullanıyorsanız alternatif bir yöntem de en son faturanızdaki miktarlara bakmaktır.

Veri tüketim ve erişim modelinizi yaklaşık olarak tahmin etmek için, ölçümler için düzenli kullanımınızı temsil eden bir elde tutma süresi seçmeniz ve tahmin etmeniz önerilir. Seçeneklerden biri son yedi güne ait ölçüm verilerinin tutulması ve verilerin ay sonunda analiz için haftada bir toplanmasıdır. Diğer bir seçenek ise son 30 güne ait ölçüm verilerinin tutulması ve verilerin 30 günlük süre sonunda toplanıp çözümlenmesidir.

Ölçüm verilerini etkinleştirme, toplama ve görüntüleme hakkında bilgi için bkz. [Azure Depolama ölçümlerini etkinleştirme ve ölçüm verilerini görüntüleme](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

> [!NOTE]
> Analiz verilerinin depolanması, erişimi ve indirilmesi de normal kullanıcı verileri gibi ücretlendirilir.

### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Kullanım ölçümlerinden yararlanarak maliyetleri tahmin etme

### <a name="storage-costs"></a>Depolama maliyetleri

*$MetricsCapacityBlob* kapasite ölçüm tablosunda *'data'* satır anahtarını içeren en son giriş, kullanıcı verilerinin harcadığı kapasiteyi gösterir. *$MetricsCapacityBlob* kapasite ölçüm tablosunda *'analytics'* satır anahtarını içeren en son giriş, analiz günlüklerinin harcadığı kapasiteyi gösterir.

Hem kullanıcı verileri hem de analiz günlükleri (etkinse) tarafından kullanılan bu toplam kapasite, verileri depolama hesabına depolama maliyetini tahmin etmek için kullanılabilir. Aynı yöntem ayrıca GPv1 depolama hesaplarında depolanma maliyetlerini tahmin etmek için kullanılabilir.

### <a name="transaction-costs"></a>İşlem maliyetleri

İşlem ölçüm tablosundaki bir API’nin tüm girişleri için *'TotalBillableRequests'* toplamı, ilgili API’nin toplam işlem sayısını belirtir. *Örneğin*, belirli bir süre içindeki *'GetBlob'* işlemlerinin toplam sayısı *'user;GetBlob'* satır anahtarını içeren tüm girişlere yönelik toplam faturalandırılabilir isteklerin toplamına göre hesaplanabilir.

Blob Depolama hesaplarına ilişkin işlem maliyetlerini tahmin etmek için farklı şekilde fiyatlandırıldıkları için işlemleri üç gruba ayırmanız gerekir.

* *'PutBlob'*, *'PutBlock'*, *'PutBlockList'*, *'AppendBlock'*, *'ListBlobs'*, *'ListContainers'*, *'CreateContainer'*, *'SnapshotBlob'* ve *'CopyBlob'* gibi yazma işlemleri.
* *'DeleteBlob'* ve *'DeleteContainer'* gibi silme işlemleri.
* Diğer tüm işlemler.

GPv1 depolama hesaplarının işlem maliyetlerini tahmin etmek için işlemden/API’den bağımsız olarak tüm işlemleri toplamanız gerekir.

### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Veri erişimi ve coğrafi çoğaltma veri aktarımı maliyetleri

Storage Analytics bir depolama hesabından okunan ve depolama hesabına yazılan veri miktarını belirtmese de, işlem ölçümleri tablosuna bakılarak bu değer kabaca tahmin edilebilir. İşlem ölçüm tablosundaki bir API’nin tüm girişleri için *'TotalIngress'* toplamı, ilgili API’nin toplam giriş verileri miktarını bayt cinsinden belirtir. Benzer şekilde, *'TotalEgress'* toplamı toplam çıkış verileri miktarını bayt cinsinden belirtir.

Blob Depolama hesaplarına ilişkin veri erişimi maliyetlerini tahmin etmek için işlemleri iki gruba ayırmanız gerekir.

* Depolama hesabından alınan veri miktarı birincil olarak *'GetBlob'* ve *'CopyBlob'* işlemleri için *'TotalEgress'* toplamına bakılarak tahmin edilebilir.

* Depolama hesabına yazılan veri miktarı birincil olarak *'PutBlob'*, *'PutBlock'*, *'CopyBlob'* ve *'AppendBlock'* işlemleri için *'TotalIngress'* toplamına bakılarak tahmin edilebilir.

Blob Depolama hesaplarında coğrafi çoğaltma veri aktarımı maliyeti de GRS veya RA-GRS depolama hesabı kullanılırken yazılan veri miktarı tahmin edilerek hesaplanabilir.

> [!NOTE]
> Seyrek veya sık erişimli bir depolama katmanını kullanma maliyetlerini hesaplama hakkında daha ayrıntılı bir örnek için *'Sık ve Seyrek Erişimli erişim katmanları nelerdir ve hangisinin kullanılacağını nasıl belirlemeliyim?'* başlıklı SSS bölümüne bakın bkz. [Azure Depolama Fiyatlandırma Sayfası](https://azure.microsoft.com/pricing/details/storage/).

## <a name="migrating-existing-data"></a>Mevcut verileri geçirme

GPv1 veya Blob Depolama hesabı, kesinti veya API değişiklikleri olmadan ve verileri bir yere taşımak gerekmeden kolayca GPv2’ye yükseltilebilir. Bu, GPv2’nin Blob Depolama hesaplarına göre başlıca avantajlarından biridir.

Ancak, Blob Depolama hesabına geçmeniz gerekiyorsa aşağıdaki yönergeleri kullanabilirsiniz.

Blob Depolama hesabı yalnızca blok ve ekleme bloblarının depolanmasına yöneliktir. Blobların yanı sıra tablo, kuyruk, dosya ve diskleri de depolamanızı sağlayan genel amaçlı mevcut depolama hesapları Blob Depolama hesaplarına dönüştürülemez. Depolama katmanlarını kullanmak için yeni Blob Depolama hesapları oluşturmanız ve mevcut verilerinizi yeni oluşturulan bu hesaplara taşımanız gerekir.

Mevcut verileri şirket içi depolama cihazlarından, üçüncü taraf bulut depolama sağlayıcılarından ya da Azure’daki mevcut genel amaçlı depolama hesaplarınızdan Blob Depolama hesaplarına geçirmek için aşağıdaki yöntemleri kullanabilirsiniz:

### <a name="azcopy"></a>AzCopy

AzCopy, verilerin Azure Storage’a ve Azure Storage’dan yüksek performansla kopyalanması için tasarlanmış bir Windows komut satırı yardımcı programıdır. AzCopy yardımcı programını, verileri genel amaçlı depolama hesaplarınızdan Blob Depolama hesabınıza kopyalamak ya da şirket içi depolama cihazlarınızdaki verileri Blob Depolama hesabınıza yüklemek için kullanabilirsiniz.

Daha fazla bilgi için bkz. [AzCopy Komut Satırı Yardımcı Programı ile Veri Aktarma](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="data-movement-library"></a>Veri hareketi kitaplığı

.NET için Azure Storage veri hareketi kitaplığı AzCopy’yi çalıştıran çekirdek veri hareketi altyapısını temel alır. Kitaplık, AzCopy’ye benzer yüksek performanslı, güvenilir ve kolay veri aktarımı işlemleri için tasarlanmıştır. Bu, AzCopy’nin dış örneklerini çalıştırmanıza ve izlemenize gerek kalmadan, AzCopy tarafından uygulamanızda yerel olarak sağlanan özelliklerden tam olarak faydalanmanızı sağlar.

Daha fazla ayrıntı için bkz. [.Net için Azure Storage Veri Hareketi Kitaplığı](https://github.com/Azure/azure-storage-net-data-movement)

### <a name="rest-api-or-client-library"></a>REST API’si veya istemci kitaplığı

Azure istemci kitaplıklarından birini ya da Azure depolama hizmetleri REST API’sini kullanarak verilerinizi Blob Depolama hesabına geçirmek için özel bir uygulama oluşturabilirsiniz. Azure Storage NET, Java, C++, Node.JS, PHP, Ruby ve Python gibi birden fazla dilde ve platformda zengin istemci kitaplıkları sağlar. İstemci kitaplıkları yeniden deneme mantığı, günlüğe kaydetme ve paralel karşıya yüklemeler gibi gelişmiş özellikler sunar. HTTP/HTTPS istekleri yapan herhangi bir dil tarafından çağrılabilen REST API’sine karşı doğrudan da geliştirebilirsiniz.

Daha fazla bilgi için bkz. [Azure Blob Depolama’yı kullanmaya başlayın](../blobs/storage-dotnet-how-to-use-blobs.md).

> [!NOTE]
> Bloblar, blobla depolanan istemci tarafı şifreleme depolama şifrelemesiyle ilgili meta veriler kullanılarak depolanır. Tüm kopyalama mekanizmalarının blob verilerinin ve özellikle şifrelemeyle ilgili meta verilerin korunduğundan emin olması kesinlikle önemlidir. Blobları bu meta veriler olmadan kopyalarsanız, blob içeriği tekrar alınamaz. Şifrelemeyle ilgili meta veriler hakkında daha fazla bilgi için bkz. [Azure Depolama İstemci Tarafı Şifrelemesi](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="faq"></a>SSS

**Mevcut depolama hesapları hâlâ kullanılabilir mi?**

Evet, var olan depolama hesapları hala kullanılabilir ve fiyatlandırma veya işlev açısından bir farklılık göstermez.  Bunlar depolama katmanı seçme olanağına sahip değildir ve gelecekte katmanlama özelliğine sahip olmayacaktır.

**Neden ve ne zaman GPv2 depolama hesapları kullanmaya başlamalıyım?**

GPv2 depolama hesapları, sektörde rekabetçi işlem ve veri erişim maliyetleri sunarken en düşük GB depolama maliyetleri sağlamada uzmanlaşmıştır. Dahası, bu hesap türüne dayalı değişiklik bildirimleri gibi özellikler gelecekte sunulacağından GPv2 depolama hesapları blobları depolamak için önerilen yoldur. Ancak, iş gereksinimlerinize bağlı olarak ne zaman yükselteceğiniz size kalmıştır.  Örneğin, yükseltmeden önce işlem modellerinizi iyileştirmeyi seçebilirsiniz.

**Mevcut depolama hesabımı GPv2 depolama hesabına yükseltebilir miyim?**

Evet. GPv1 veya Blob Depolama hesapları, portalda kolayca GPv2’ye yükseltilebilir.

**Nesneleri aynı hesaptaki iki depolama katmanında depolayabilir miyim?**

Evet. Hesap düzeyinde ayarlanan **Erişim Katmanı** özniteliği, bu hesapta bulunan ve katmanı açıkça belirlenmemiş olan tüm nesneler için geçerli varsayılan katmandır. Ancak blob düzeyinde katman ayarlama, hesabın erişim katmanı ayarından bağımsız olarak nesne düzeyinde erişim katmanını açık olarak ayarlamanıza olanak tanır. Aynı hesapta, üç depolama katmanının (sık erişilen, seyrek erişilen veya arşiv) tümüne ait bloblar bulunabilir.

**GPv2 depolama hesabımdaki depolama katmanını değiştirebilir miyim?**

Evet, depolama hesabındaki **Erişim Katmanı** özniteliğini ayarlayarak hesap depolama katmanını değiştirebilirsiniz. Hesap depolama katmanının değiştirilmesi, hesapta depolanmış ve açıkça katmanı belirtilmemiş tüm nesneler için geçerlidir. Sık erişilen olan depolama katmanının seyrek erişilen olarak değiştirilmesi, yazma işlemi (10.000 başına) maliyetleri doğurur (yalnızca GPv2 depolama hesaplarında). Seyrek erişilen olan depolama katmanının sık erişilen olarak değiştirilmesi ise hesaptaki tüm verilerin okunması için hem okuma işlemi (10.000 başına) hem de veri alma (GB başına) maliyetleri doğurur.

**Blob Depolama hesabımdaki depolama katmanını hangi sıklıkta değiştirebilirim?**

Depolama katmanını değiştirme sıklığına ilişkin bir sınırlama koymuyoruz, ancak depolama katmanını seyrek erişimliden sık erişimliye değiştirmenin büyük maliyetler doğurduğuna dikkat edin. Depolama katmanını sık değiştirmeniz önerilmez.

**Seyrek erişimli depolama katmanındaki bloblar, sık erişimli depolama katmanındakilerden farklı mı davranır?**

GPv2 ve Blob Depolama hesaplarının sık erişimli depolama katmanındaki bloblar GPv1 depolama hesaplarındaki bloblarla aynı gecikme süresine sahiptir. Seyrek erişimli depolama katmanındaki bloblar sık erişimli katmandaki bloblarla benzer gecikme süresine (milisaniye olarak) sahiptir. Arşiv depolama katmanındaki bloblar, birkaç saatlik gecikme süresine sahiptir.

Seyrek erişimli depolama katmanındaki bloblar, sık erişimli depolama katmanında depolanan bloblara göre daha düşük kullanılabilirlik hizmet düzeyine (SLA) sahiptir. Daha fazla bilgi için bkz. [Depolama için SLA](https://azure.microsoft.com/support/legal/sla/storage).

**Sayfa bloblarını ve sanal makine disklerini Blob Depolama hesaplarında depolayabilir miyim?**

Hayır. Blob Depolama hesapları, yalnızca blok ve ekleme bloblarını destekler, sayfa bloblarını desteklemez. Azure sanal makine diskleri sayfa blobları tarafından yedeklenir ve bu nedenle sanal makine disklerini depolamak için Blob Depolama hesapları kullanılamaz. Ancak, sanal makine disklerinin yedeklerini blok blobları olarak Blob Depolama hesabında depolamak mümkündür. Blob Depolama hesapları yerine GPv2 kullanmayı dikkate alma nedenlerinden biri budur.

**GPv2 depolama hesaplarını kullanmak için mevcut uygulamalarımı değiştirmem gerekiyor mu?**

GPv2 depolama hesapları GPv1 ve Blob Depolama hesapları ile %100 API uyumludur. Uygulamanız blok veya ilave bloblarını kullandığı ve [Depolama Hizmetleri REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx)’nin 2014-02-14 sürümünü veya üstünü kullandığınız sürece, uygulamanız çalışmaya devam edecektir. Protokolün daha eski bir sürümünü kullanıyorsanız, her iki tür depolama hesabıyla sorunsuz çalışarak yeni sürümü kullanmak için uygulamanızı güncelleştirmeniz gerekir. Genel olarak, hangi depolama hesabını kullandığınızdan bağımsız olarak her zaman en son sürümü kullanmanızı öneriyoruz.

**Kullanıcı deneyiminde bir değişiklik olur mu?**

GPv2 depolama hesapları GPv1 depolama hesaplarına çok benzer ve yüksek dayanıklılık ve kullanılabilirlik, ölçeklenebilirlik, performans ve güvenlik dahil olmak üzere Azure Depolama’nın tüm temel özelliklerini destekler. GPv2’ye veya Blob Depolamaya yükseltirken Blob Depolama hesaplarına ve onun yukarıda bahsedilen depolama katmanlarına özgü özellikler ve kısıtlamalar dışındaki her şey aynı kalır.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="evaluate-blob-storage-accounts"></a>Blob Depolama hesaplarını değerlendirme

[Bölgeye göre Blob Depolama hesaplarının kullanılabilirliğini denetleme](https://azure.microsoft.com/regions/#services)

[Azure Depolama ölçümlerini etkinleştirerek geçerli depolama hesaplarınızın kullanımını değerlendirme](../common/storage-enable-and-view-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Bölgeye göre Blob Depolama fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/storage/)

[Veri aktarımı fiyatlandırmasını denetleme](https://azure.microsoft.com/pricing/details/data-transfers/)

### <a name="start-using-gpv2-storage-accounts"></a>GPv2 depolama hesaplarını kullanmaya başlama

[Azure Blob Depolamayı kullanmaya başlama](../blobs/storage-dotnet-how-to-use-blobs.md)

[Azure Depolama’ya ve Azure Depolama’da veri taşıma](../common/storage-moving-data.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[AzCopy Komut Satırı Yardımcı Programı ile veri aktarımı](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

[Depolama hesaplarınıza göz atma ve bu hesapları keşfetme](http://storageexplorer.com/)
