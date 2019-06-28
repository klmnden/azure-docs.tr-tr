---
title: Azure İzleyici'den Azure Cosmos DB ölçümleri alın
description: ''
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/18/2019
ms.openlocfilehash: 2eb61a6b9afa3cabf1733be120dfbdacb7de4534
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276801"
---
# <a name="monitor-and-debug-azure-cosmos-db-metrics-from-azure-monitor"></a>İzleme ve Azure Cosmos DB Azure İzleyici ölçümleri hatalarını ayıklama

Azure İzleyici API'sinden Azure Cosmos DB ölçümleri görüntüleyebilir. Azure İzleyici, Azure portalında da dahil olmak üzere, REST API aracılığıyla erişmesini ve bunları sorgulama ölçümleri ile etkileşim kurmak için çeşitli yollar sağlar PowerShell veya CLI kullanarak. Azure Cosmos DB ölçümleri bir dakikalık sıklığında varsayılan olarak toplanan düşük gecikme süreli sayısal değerler, bu ölçümleri de toplayabilirsiniz. Bu ölçümleri gerçek zamanlı senaryoları destekleme özelliğine sahiptir.  

Bu makalede Azure portalını kullanarak Azure İzleyici'den farklı Azure Cosmos DB ölçümlerini görüntüleyebilirsiniz. Ortak ilgileniyorsanız, kullanım ve bakın sorunları analiz etmek ve bunların hatalarını ayıklamak için Azure Cosmos DB ölçümleri yeniden kullanılma [İzleyici ve Azure Cosmos DB'de ölçümlerle hata ayıklama](use-metrics.md) makalesi. Var olan Azure Cosmos hesaplarınızı birini kullanın ve veritabanı, kapsayıcı, bölge, istek veya işlem düzeylerini farklı ölçümleri görüntüleyin. Bu nedenle, örnek verilerle bir Azure Cosmos hesabınız varsa ve bu verileri CRUD işlemleri emin olun.

## <a name="view-metrics-from-azure-portal"></a>Azure portalından ölçümlerini görüntüleme

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. Seçin **İzleyici** sol gezinme çubuğunda ve select **ölçümleri**.

   ![Azure İzleyici ölçümleri bölmesinde](./media/cosmos-db-azure-monitor-metrics/monitor-metrics-blade.png)

1. Gelen **ölçümleri** bölmesinde > **bir kaynak seçin** > gerekli seçin **abonelik**, ve **kaynak grubu**. İçin **kaynak türü**seçin **Azure Cosmos DB hesapları**, var olan Azure Cosmos hesaplarınızı birini seçip **Uygula**. 

   ![Ölçümleri görüntülemek için bir Cosmos DB hesabı seçin](./media/cosmos-db-azure-monitor-metrics/select-cosmosdb-account.png)

1. Sonraki kullanılabilir ölçümler listesinden bir ölçüm seçebilirsiniz. İstek birimleri, depolama, gecikme süresi, kullanılabilirlik, Cassandra ve diğerleri için belirli ölçümleri seçebilirsiniz. Bu listedeki tüm kullanılabilir ölçümler hakkında ayrıntılı bilgi edinmek için [kategoriye göre ölçümleri](#metrics-by-category) bu makalenin. Bu örnekte, seçelim **istek birimi** ve **ortalama** toplama değeri. 

   Bu ayrıntılar yanı sıra de seçebilirsiniz **zaman aralığı** ve **zaman ayrıntı düzeyi** ölçümleri. En fazla son 30 güne ilişkin ölçümleri görüntüleyebilir.  Filtre uygulandıktan sonra filtresine göre bir grafik görüntülenir. İstek birimleri, seçilen süre için dakika başına tüketilen ortalama sayısını görebilirsiniz.  

   ![Azure portalından bir ölçüm seçin](./media/cosmos-db-azure-monitor-metrics/metric-types.png)

## <a name="add-filters-to-metrics"></a>Ölçümler için filtreleri ekleyin

Ayrıca ölçümleri ve belirli bir tarafından görüntülenen grafik filtreleyebilirsiniz **CollectionName**, **DatabaseName**, **OperationType**, **bölge**, ve **StatusCode**. Ölçümler filtre uygulamak için seçin **Filtre Ekle** ve gerekli özelliği gibi seçin **OperationType** ve gibi bir değer seçin **sorgu**. Grafik, seçilen süre için sorgu işlemi için kullanılan istek birimleri ardından görüntüler. Saklı yordam yürütülen işlemler OperationType Ölçüm altında kullanılabilir olmayan şekilde günlüğe kaydedilmez.

![Ölçüm ayrıntı düzeyi seçmek için bir filtre Ekle](./media/cosmos-db-azure-monitor-metrics/add-metrics-filter.png)

Ölçümleri kullanarak gruplayabilirsiniz **uygulamak bölme** seçeneği. Örneğin, işlem türü başına istek birimi grubunda ve aynı anda aşağıdaki görüntüde gösterildiği gibi tüm işlemler için grafı görüntülemek: 

![Ekleme bölme Filtre Uygula](./media/cosmos-db-azure-monitor-metrics/apply-metrics-splitting.png)


## <a id="metrics-by-category"></a>Kategoriye göre ölçümleri

### <a name="request-metrics"></a>İstek ölçümleri
            
|Ölçüm (ölçüm görünen adı)|Birim (toplama türü) |Açıklama|Boyutlar| Zaman ayrıntı düzeyi| Eski ölçüm eşleme | Kullanım |
|---|---|---|---| ---| ---| ---|
| TotalRequests (toplam istek sayısı) | Count (sayı) | Yapılan istek sayısı| DatabaseName, CollectionName, bölge, StatusCode| Tümü | Hizmet kullanılamıyor, istekler daraltıldı, saniye başına ortalama istek Http 2xx, 3xx, Http 400, Http 401 Http TotalRequests, iç sunucu hatası | Durum kodu, bir dakikalık ayrıntı düzeyi, koleksiyon başına istekleri izlemek için kullanılır. Saniye başına ortalama istek almak için dakika sayısı toplama kullanın ve 60 bölün. |
| MetadataRequests (meta veri isteği) |Count (sayı) | Meta veri isteği sayısı. Azure Cosmos DB koleksiyonları, veritabanları vb. listeleme olanak tanır, her hesap için sistem meta veri koleksiyonu tutar ve bunların yapılandırmalarının ücretsiz. | DatabaseName, CollectionName, bölge, StatusCode| Tümü| |Meta veri isteği nedeniyle kısıtlamalar izlemek için kullanılır.|
| MongoRequests (Mongo istek) | Count (sayı) | Mongo istekleri sayısı | DatabaseName CollectionName, bölgesi CommandName, hata kodu| Tümü |Mongo sorgusu istek hızı, Mongo güncelleştirme isteği oranı oranı, Mongo silmek, Mongo istek hızı Ekle istek Mongo sayısı istek oranı| Mongo istek hataları izlemek için kullanılan komut başına kullanımları yazın. |

### <a name="request-unit-metrics"></a>İstek birimi ölçümleri

|Ölçüm (ölçüm görünen adı)|Birim (toplama türü)|Açıklama|Boyutlar| Zaman ayrıntı düzeyi| Eski ölçüm eşleme | Kullanım |
|---|---|---|---| ---| ---| ---|
| MongoRequestCharge (Mongo istek ücret) | Sayısı (toplam) |Tüketilen mongo istek birimleri| DatabaseName CollectionName, bölgesi CommandName, hata kodu| Tümü |Mongo sorgusu istek yükü, Mongo güncelleştirme ücreti, Mongo istek yükü silin, istek yükü, Mongo Ekle Mongo sayısı isteği ücret iste| Bir dakika içinde Mongo kaynak RU'ları izlemek için kullanılır.|
| TotalRequestUnits (toplam istek birimleri)| Sayısı (toplam) | Tüketilen birimler iste| DatabaseName, CollectionName, bölge, StatusCode |Tümü| TotalRequestUnits| Dakikalık bir ayrıntı düzeyi toplam RU kullanımını izlemek için kullanılır. Tüketilen ortalama RU almak için toplam toplama dakikada kullanın ve 60 bölün.|
| ProvisionedThroughput (sağlanan aktarım hızı)| Count (maksimum) |Koleksiyon ayrıntı düzeyinde sağlanan aktarım hızı| DatabaseName, CollectionName| 5 DK| | Koleksiyon başına sağlanan aktarım hızı izlemek için kullanılır.|

### <a name="storage-metrics"></a>Depolama ölçümleri

|Ölçüm (ölçüm görünen adı)|Birim (toplama türü)|Açıklama|Boyutlar| Zaman ayrıntı düzeyi| Eski ölçüm eşleme | Kullanım |
|---|---|---|---| ---| ---| ---|
| AvailableStorage (kullanılabilir depolama alanı) |Bayt sayısı (toplam) | Bölge başına 5 dakika ayrıntı düzeyi, raporlanan toplam kullanılabilir depolama alanı| DatabaseName, CollectionName, bölge| 5 DK| Kullanılabilir depolama alanı| Kullanılabilir depolama alanı izlemek için kullanılan kapasite (yalnızca sabit depolama koleksiyonlar için geçerlidir) en az ayrıntı düzeyi 5 dakika olmalıdır.| 
| DataUsage (veri kullanımı) |Bayt sayısı (toplam) |Bölge başına 5 dakika ayrıntı düzeyi, rapor edilen toplam veri kullanımı| DatabaseName, CollectionName, bölge| 5 DK |Veri boyutu | En düşük ayrıntı düzeyi, koleksiyon ve bölge toplam veri kullanımı izlemek için kullanılan, 5 dakika olmalıdır.|
| IndexUsage (dizin kullanım) | Bayt sayısı (toplam) |Bölge başına 5 dakika ayrıntı düzeyi, toplam dizin kullanım raporlama| DatabaseName, CollectionName, bölge| 5 DK| Dizin boyutu| En düşük ayrıntı düzeyi, koleksiyon ve bölge toplam veri kullanımı izlemek için kullanılan, 5 dakika olmalıdır. |
| DocumentQuota (belge kota) | Bayt sayısı (toplam) | Bölge başına 5 dakika ayrıntı düzeyi, toplam depolama kotası bildirdi.| DatabaseName, CollectionName, bölge| 5 DK |Depolama Kapasitesi| En düşük ayrıntı düzeyi, koleksiyon ve bölge toplam kota izlemek için kullanılan, 5 dakika olmalıdır.|
| DocumentCount (belge sayısı) | Sayısı (toplam) |Bölge başına 5 dakika ayrıntı düzeyi, raporlanan toplam belge sayısı| DatabaseName, CollectionName, bölge| 5 DK |Belge sayısı|En düşük ayrıntı düzeyi, koleksiyon ve bölge belge sayısı izlemek için kullanılan, 5 dakika olmalıdır.|

### <a name="latency-metrics"></a>Gecikme süresi ölçülerini

|Ölçüm (ölçüm görünen adı)|Birim (toplama türü)|Açıklama|Boyutlar| Zaman ayrıntı düzeyi| Kullanım |
|---|---|---|---| ---| ---|
| ReplicationLatency (çoğaltma gecikmesi)| Milisaniye (düşük, en yüksek, ortalama) | Kaynak ve hedef bölgede coğrafi özellikli hesabının P99 çoğaltma gecikmesi| SourceRegion, TargetRegion| Tümü | Coğrafi olarak çoğaltılmış bir hesap için herhangi iki bölgeleri arasında P99 çoğaltma gecikmesi izlemek için kullanılır. |


### <a name="availability-metrics"></a>Kullanılabilirlik ölçümlerini

|Ölçüm (ölçüm görünen adı) |Birim (toplama türü)|Açıklama| Zaman ayrıntı düzeyi| Eski ölçüm eşleme | Kullanım |
|---|---|---|---| ---| ---|
| ServiceAvailability (hizmet kullanılabilirlik)| Yüzde (Minimum, maksimum) | Hesap istekleri kullanılabilirliğine bir saat ayrıntı düzeyi| 1H | Hizmet kullanılabilirliği | İstekleri geçirilen toplam yüzde temsil eder. Bir isteği 410, durum kodu ise, sistem hatası nedeniyle başarısız olarak kabul edilir 500 veya 503 saat ayrıntı düzeyi hesabında kullanılabilirliğini izlemek için kullanılır. |


### <a name="cassandra-api-metrics"></a>Cassandra API ölçümleri

|Ölçüm (ölçüm görünen adı)|Birim (toplama türü)|Açıklama|Boyutlar| Zaman ayrıntı düzeyi| Kullanım |
|---|---|---|---| ---| ---|
| CassandraRequests (Cassandra istek) | Count (sayı) | Yapılan Cassandra API isteklerinin sayısı| DatabaseName, CollectionName, hata kodu, bölge, OperationType, kaynak türü| Tümü| Dakikalık bir ayrıntı düzeyi Cassandra istekleri izlemek için kullanılır. Saniye başına ortalama istek almak için dakika sayısı toplama kullanın ve 60 bölün.|
| CassandraRequestCharges (Cassandra isteği ücretleri) | Sayısı (toplam, Min, Max, Avg) | İstek birimleri Cassandra API istekleri tarafından kullanılan| DatabaseName, CollectionName, bölge, OperationType, kaynak türü| Tümü| Dakika başına Cassandra API hesabı tarafından kullanılan RU'ları izlemek için kullanılır.|
| CassandraConnectionClosures (Cassandra bağlantı kapanışlar) |Count (sayı) |Cassandra kapatılan bağlantılar sayısı| ClosureReason, bölge| Tümü | İstemcileri ve Azure Cosmos DB Cassandra API'SİNİN arasındaki bağlantıyı izlemek için kullanılır.|

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB hesabı ölçümleri bölmesi ölçümleri görüntüleyin ve izleyin](use-metrics.md)

* [Azure Cosmos DB'de tanılama günlüğüne kaydetme](logging.md)
