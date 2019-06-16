---
title: Genel amaçlı v2 depolama hesabı - Azure depolama için yükseltme | Microsoft Docs
description: Genel amaçlı v2 depolama hesapları için yükseltin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/26/2019
ms.author: tamram
ms.openlocfilehash: 2d6a5c96bf99439520e26fc905668835944cee29
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66115636"
---
# <a name="upgrade-to-a-general-purpose-v2-storage-account"></a>Genel amaçlı v2 depolama hesabı için yükseltme

Genel amaçlı v2 depolama hesabı için en son Azure depolama özelliklerini desteklemek ve tüm işlevleri, genel amaçlı v1 ve Blob Depolama hesapları dahil edilip derecelendirilir. Genel amaçlı v2 hesapları çoğu depolama senaryoları için önerilir. Genel amaçlı v2 hesapları Azure depolama, ek olarak sektörde rekabetçi işlem fiyatları düşük gigabayt başına kapasite fiyatlar sunar.

Genel amaçlı v2 depolama hesabı, genel amaçlı v1'den veya Blob Depolama hesapları için yükseltme basit bir işlemdir. Azure portal, PowerShell veya Azure CLI kullanarak yükseltebilirsiniz.

> [!IMPORTANT]
> Genel amaçlı v1 veya Blob Depolama hesabı genel amaçlı v2'ye yükseltme kalıcıdır ve geri alınamaz.

## <a name="upgrade-using-the-azure-portal"></a>Azure portalını kullanarak yükseltme

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Depolama hesabınıza gidin.
3. İçinde **ayarları** bölümünde **yapılandırma**.
4. **Hesap Türü** altında **Yükselt**’e tıklayın.
5. **Yükseltmeyi Onayla** altında, hesabınızın adını yazın.
6. Tıklayın **yükseltme** dikey pencerenin alt kısmındaki.

    ![Yükseltme hesap türü](../blobs/media/storage-blob-account-upgrade/upgrade-to-gpv2-account.png)

## <a name="upgrade-with-powershell"></a>Powershell ile yükseltme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Genel amaçlı v1 hesabı PowerShell kullanarak bir genel amaçlı v2 hesabına yükseltmek için önce en son sürümünü kullanacak şekilde güncelleştirin **Az.Storage** modülü. PowerShell’i yükleme hakkında bilgi edinmek için bkz. [Azure PowerShell’i yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/install-Az-ps).

Ardından, depolama hesabı ve kaynak grubunuzun adını değiştirerek, hesabı yükseltmek için şu komuta çağrı yapın:

```powershell
Set-AzStorageAccount -ResourceGroupName <resource-group> -AccountName <storage-account> -UpgradeToStorageV2
```

## <a name="upgrade-with-azure-cli"></a>Azure CLI ile yükseltme

Genel amaçlı v1 hesabı, Azure CLI kullanarak bir genel amaçlı v2 hesabına yükseltmek için önce Azure CLI'nin en son sürümünü yükleyin. CLI yüklemesi hakkında bilgi için bkz. [Azure CLI 2.0’ı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

Ardından, depolama hesabı ve kaynak grubunuzun adını değiştirerek, hesabı yükseltmek için şu komuta çağrı yapın:

```cli
az storage account update -g <resource-group> -n <storage-account> --set kind=StorageV2
```

## <a name="specify-an-access-tier-for-blob-data"></a>Blob veri erişim katmanı belirlemesine

Genel amaçlı v2 hesapları, tüm Azure depolama hizmetleri ve veri nesneleri destekler, ancak yalnızca blok blobları olarak Blob Depolama için erişim katmanları mevcuttur. Genel amaçlı v2 depolama hesabı için yükselttiğinizde, erişim katmanı için blob verilerinizi belirtebilirsiniz.

Erişim katmanı, beklenen kullanım düzenlerini esas alarak en uygun maliyetli depolama seçmenize olanak sağlar. Blok blobları, sık erişimli, seyrek erişimli veya arşiv katmanında depolanabilir. Erişim katmanları hakkında daha fazla bilgi için bkz. [Azure Blob Depolama: Seyrek erişimli, seyrek ve Arşiv depolama katmanları](../blobs/storage-blob-storage-tiers.md).

Varsayılan olarak, sık erişimli erişim katmanında yeni bir depolama hesabı oluşturulur ve bir genel amaçlı v1 depolama hesabı için sık erişim katmanı yükseltilir. Veri sonrası yükseltme için kullanılacak hangi erişim katmanı araştırıyorsanız senaryonuz göz önünde bulundurun. Genel amaçlı v2 hesabına geçirmek için iki normal kullanıcı senaryosu vardır:

* Mevcut genel amaçlı v1 depolama hesabınız ve blob verilerini için doğru depolama erişim katmanı ile bir genel amaçlı v2 depolama hesabı için bir yükseltme değerlendirmek istiyorsunuz.
* Genel amaçlı v2 depolama hesabı kullanın veya zaten varsa ve sizin için blob verilerini sık veya seyrek erişimli depolama erişim katmanı kullanıp kullanmayacağınızı değerlendirmek istiyorsunuz karar verdik.

Her iki durumda da birinci öncelik depolanması, erişimi ve bir genel amaçlı v2 depolama hesabında depolanan verileriniz üzerinde işletim maliyetini tahmin etmek ve bu maliyetin mevcut maliyetlerinizle karşılaştırmak için olan.

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

V1 depolama hesabı genel amaçlı v2 hesabına yükseltmek ücretsizdir. Ancak, depolama erişim katmanını değiştirme değişiklikleri faturanıza neden olabilir. 

Tüm depolama hesapları, blob depolama için her blobun katmanını temel alan bir fiyatlandırma modelini kullanır. Bir depolama hesabını kullanırken aşağıdaki fatura değerlendirmeleri geçerlidir:

* **Depolama maliyetleri**: Depolanan veri miktarına ek olarak, veri depolamanın maliyeti depolama erişim katmanına bağlı olarak değişir. Katmanın erişim sıklığı düştükçe gigabayt başına ücret de azalır.

* **Veri erişim maliyetleri**: Düştükçe veri erişimi artış ücretleri. Seyrek erişimli ve Arşiv depolama erişim katmanındaki veriler için okuma gigabayt başına veri erişim ücreti ödersiniz.

* **İşlem maliyetleri**: Tüm katmanlar için düştükçe artıran işlem başına ücret yoktur.

* **Coğrafi çoğaltma veri aktarımı maliyetleri**: Bu ücret, yalnızca coğrafi yapılandırılmış, GRS ve RA-GRS dahil olmak üzere çoğaltma ile hesapları için geçerlidir. Coğrafi çoğaltma veri aktarımı gigabayt başına ücret doğurur.

* **Giden veri aktarımı maliyetleri**: Giden veri aktarımları (bir Azure bölgesinin dışına aktarılan veriler), genel amaçlı depolama hesapları ile tutarlı, gigabayt başına esaslı olarak bant genişliği kullanımı için fatura doğurur.

* **Depolama erişim katmanını değiştirme**: Depolama hesabı erişim katmanını seyrek erişimliden sık erişimliye değiştirmek depolama hesabında varolan tüm verilerin okunmasına eşit bir ücret doğurur. Ancak, hesap erişim katmanını sık erişilenden seyrek tüm verileri seyrek erişilen katmana (yalnızca GPv2 hesapları) yazma eşit bir ücret doğurur.

> [!NOTE]
> Depolama hesaplarına ilişkin fiyatlandırma modeli hakkında daha fazla bilgi için [Azure Depolama Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) sayfasına bakın. Giden veri aktarımı ücretlerine ilişkin daha fazla bilgi için [Veri Aktarımları Fiyatlandırma Bilgileri](https://azure.microsoft.com/pricing/details/data-transfers/) sayfasına bakın.

### <a name="estimate-costs-for-your-current-usage-patterns"></a>Geçerli, kullanım desenleri için tahmini maliyetleri

Depolama ve blob verilerini belirli bir katman genel amaçlı v2 depolama hesabındaki erişim maliyetini tahmin etmek için var olan kullanım modelinizi değerlendirmek veya, beklediğiniz kullanım modelini yaklaşık. Genel olarak, şunları bilmek istersiniz:

* Gigabayt dahil olmak üzere, kendi Blob Depolama tüketimi:
    - Depolama hesabınızda ne kadar veri depolanıyor?
    - Aylık temelde veri hacmi nasıl değişiyor; yeni veriler sürekli eski verilerin yerini alıyor mu?
* Birincil erişim düzeni dahil olmak üzere Blob Depolama veri:
    - Ne kadar veri okuma ve depolama hesabına yazılır?
    - Depolama hesabındaki veriler üzerinde işlemler gerçekleşir ve kaç okuma işlemleri yazma?

Gereksinimleriniz için en iyi erişim katmanına karar vermek için blob veri kapasitenizi ve bu verileri nasıl kullanıldığını belirlemek yararlı olabilir. Bu, hesabınız için izleme ölçümlere bakarak en iyi yapılabilir.

### <a name="monitoring-existing-storage-accounts"></a>Var olan depolama hesaplarını izleme

Var olan depolama hesaplarınızı izlemek ve bu verileri toplamak için, bir depolama hesabına yönelik günlüğe kaydetme işlemlerini gerçekleştiren ve ölçümler sağlayan Azure Depolama Analizi hizmetinden yararlanabilirsiniz. Depolama Analizi GPv1, GPv2 ve Blob depolama hesap türleri için toplu işlem istatistiklerini içeren ölçümleri ve depolama hizmetine yapılan isteklere ilişkin kapasite verilerini depolayabilir. Bu veriler aynı depolama hesabındaki iyi bilinen tablolara depolanır.

Daha fazla bilgi için bkz. [Storage Analytics Ölçümleri hakkında](https://msdn.microsoft.com/library/azure/hh343258.aspx) ve [Storage Analytics Ölçüm Tablosu Şeması](https://msdn.microsoft.com/library/azure/hh343264.aspx)

> [!NOTE]
> Blob depolama hesapları, Tablo hizmeti uç noktasını yalnızca ilgili hesabın ölçüm verilerini depolamak ve bunlara erişmek için kullanıma sunar.

Blob depolamada depolama tüketimini izlemek için kapasite ölçümlerini etkinleştirmeniz gerekir.
Bu özellik etkinleştirildiğinde bir depolama hesabının Blob hizmeti için kapasite verileri günlük olarak kaydedilir ve aynı depolama hesabı içindeki *$MetricsCapacityBlob* tablosuna yazılan bir tablo girişi olarak kaydedilir.

Blob depolama hizmetinin veri erişim desenlerini izlemek için API’den saatlik işlem ölçümlerini etkinleştirmeniz gerekir. Saatlik işlem ölçümleri etkinleştirildiğinde API başına işlemler saatte bir toplanır ve aynı depolama hesabındaki *$MetricsHourPrimaryTransactionsBlob* tablosuna yazılan bir tablo girişi olarak kaydedilir. RA-GRS depolama hesapları kullanılırken *$MetricsHourSecondaryTransactionsBlob* tablosu, işlemleri ikincil uç noktaya kaydeder.

> [!NOTE]
> Hangi sayfa bloblarını ve sanal makine disklerini veya Kuyrukları, dosyaları ya da blok yanı sıra tabloları depoladığınız ve ekleme blobu verileriyle bir genel amaçlı depolama hesabınız varsa bu tahmin işlemi geçerli değildir. Kapasite verileri blok bloblarını diğer türlerden ayırt etmez ve diğer veri türleri için kapasite verileri sunmaz. Bu türleri kullanıyorsanız alternatif bir yöntem de en son faturanızdaki miktarlara bakmaktır.

Veri tüketim ve erişim modelinizi yaklaşık olarak tahmin etmek için, ölçümler için düzenli kullanımınızı temsil eden bir elde tutma süresi seçmeniz ve tahmin etmeniz önerilir. Seçeneklerden biri son yedi güne ait ölçüm verilerinin tutulması ve verilerin ay sonunda analiz için haftada bir toplanmasıdır. Diğer bir seçenek ise son 30 güne ait ölçüm verilerinin tutulması ve verilerin 30 günlük süre sonunda toplanıp çözümlenmesidir.

Toplama ve ölçüm verilerini etkinleştirme hakkında daha fazla bilgi için bkz [Storage analytics ölçümleri](../common/storage-analytics-metrics.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

> [!NOTE]
> Analiz verilerinin depolanması, erişimi ve indirilmesi de normal kullanıcı verileri gibi ücretlendirilir.

### <a name="utilizing-usage-metrics-to-estimate-costs"></a>Kullanım ölçümlerinden yararlanarak maliyetleri tahmin etme

#### <a name="capacity-costs"></a>Kapasite maliyetleri

*$MetricsCapacityBlob* kapasite ölçüm tablosunda *'data'* satır anahtarını içeren en son giriş, kullanıcı verilerinin harcadığı kapasiteyi gösterir. *$MetricsCapacityBlob* kapasite ölçüm tablosunda *'analytics'* satır anahtarını içeren en son giriş, analiz günlüklerinin harcadığı kapasiteyi gösterir.

Hem kullanıcı verileri hem de analiz günlükleri (etkinse) tarafından kullanılan bu toplam kapasite, verileri depolama hesabına depolama maliyetini tahmin etmek için kullanılabilir. Aynı yöntem ayrıca GPv1 depolama hesaplarında depolanma maliyetlerini tahmin etmek için kullanılabilir.

#### <a name="transaction-costs"></a>İşlem maliyetleri

İşlem ölçüm tablosundaki bir API’nin tüm girişleri için *'TotalBillableRequests'* toplamı, ilgili API’nin toplam işlem sayısını belirtir. *Örneğin*, belirli bir süre içindeki *'GetBlob'* işlemlerinin toplam sayısı *'user;GetBlob'* satır anahtarını içeren tüm girişlere yönelik toplam faturalandırılabilir isteklerin toplamına göre hesaplanabilir.

Blob depolama hesaplarına ilişkin işlem maliyetlerini tahmin etmek için, farklı şekilde fiyatlandırıldıkları için işlemleri üç gruba ayırmanız gerekir.

* *'PutBlob'* , *'PutBlock'* , *'PutBlockList'* , *'AppendBlock'* , *'ListBlobs'* , *'ListContainers'* , *'CreateContainer'* , *'SnapshotBlob'* ve *'CopyBlob'* gibi yazma işlemleri.
* *'DeleteBlob'* ve *'DeleteContainer'* gibi silme işlemleri.
* Diğer tüm işlemler.

GPv1 depolama hesaplarının işlem maliyetlerini tahmin etmek için işlemden/API’den bağımsız olarak tüm işlemleri toplamanız gerekir.

#### <a name="data-access-and-geo-replication-data-transfer-costs"></a>Veri erişimi ve coğrafi çoğaltma veri aktarımı maliyetleri

Storage Analytics bir depolama hesabından okunan ve depolama hesabına yazılan veri miktarını belirtmese de, işlem ölçümleri tablosuna bakılarak bu değer kabaca tahmin edilebilir. İşlem ölçüm tablosundaki bir API’nin tüm girişleri için *'TotalIngress'* toplamı, ilgili API’nin toplam giriş verileri miktarını bayt cinsinden belirtir. Benzer şekilde, *'TotalEgress'* toplamı toplam çıkış verileri miktarını bayt cinsinden belirtir.

Blob depolama hesaplarına ilişkin veri erişimi maliyetlerini hesaplamak için işlemleri iki gruba ayırmanız gerekir.

* Depolama hesabından alınan veri miktarı birincil olarak *'GetBlob'* ve *'CopyBlob'* işlemleri için *'TotalEgress'* toplamına bakılarak tahmin edilebilir.

* Depolama hesabına yazılan veri miktarı birincil olarak *'PutBlob'* , *'PutBlock'* , *'CopyBlob'* ve *'AppendBlock'* işlemleri için *'TotalIngress'* toplamına bakılarak tahmin edilebilir.

Blob depolama hesaplarında coğrafi çoğaltma veri aktarımı maliyeti de bir GRS veya RA-GRS depolama hesabı kullanılırken yazılan veri miktarı tahmini kullanılarak hesaplanabilir.

> [!NOTE]
> Sık erişimli veya seyrek erişimli depolama erişim katmanını kullanma maliyetlerini hesaplama hakkında daha ayrıntılı bir örnek için başlıklı SSS Bölümüne göz atın *'sık ve seyrek erişimli erişim katmanları nelerdir ve hangisinin kullanılacağını nasıl belirlemeliyim?'* bkz. [Azure Depolama Fiyatlandırma Sayfası](https://azure.microsoft.com/pricing/details/storage/).

## <a name="next-steps"></a>Sonraki adımlar

- [Depolama hesabı oluşturma](storage-quickstart-create-account.md)
- [Azure depolama hesaplarını yönetme](storage-account-manage.md)
