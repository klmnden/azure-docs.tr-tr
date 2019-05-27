---
title: Azure cosmos DB'de sınırları
description: Bu makalede, Azure Cosmos DB'de sınırları açıklanır.
author: arramac
ms.author: arramac
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/19/2019
ms.openlocfilehash: 0086327661df637dc0ae60208ed9424b4610ef0e
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969500"
---
# <a name="limits-in-azure-cosmos-db"></a>Azure cosmos DB'de sınırları

Bu makalede, Azure Cosmos DB hizmetini sınırları genel bir bakış sağlar.

## <a name="storage-and-throughput"></a>Depolama ve aktarım hızı

Aboneliğiniz kapsamındaki bir Azure Cosmos hesabı oluşturduktan sonra hesabınızdaki tarafından verileri yönetebilirsiniz [veritabanları, kapsayıcılar ve öğeleri oluşturma](databases-containers-items.md). Kapsayıcı düzeyinde veya veritabanı düzeyinde açısından aktarım hızı sağlayabileceğiniz [istek birimi (RU/sn veya RU)](request-units.md). Aşağıdaki tabloda, depolama ve kapsayıcı/veritabanı başına aktarım hızı için sınırlar listelenmektedir.

| Resource | Varsayılan limit |
| --- | --- |
| Kapsayıcı başına en fazla RU ([adanmış aktarım hızı sağlanmış modu](databases-containers-items.md#azure-cosmos-containers)) | Varsayılan olarak 1.000.000. Tarafından artırabilirsiniz [bir Azure destek bileti](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) veya aracılığıyla bizimle iletişime geçtiğiniz [Cosmos DB isteyin](mailto:askcosmosdb@microsoft.com) |
| Veritabanı başına en fazla RU ([paylaşılan aktarım hızı sağlanmış modu](databases-containers-items.md#azure-cosmos-containers)) | Varsayılan olarak 1.000.000. Tarafından artırabilirsiniz [bir Azure destek bileti](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) veya aracılığıyla bizimle iletişime geçtiğiniz [Cosmos DB isteyin](mailto:askcosmosdb@microsoft.com) |
| (Mantıksal) bölüm anahtarı başına en fazla ru | 10,000 |
| Tüm öğelerde (mantıksal) bölüm anahtarı başına en fazla depolama| 10 GB |
| En çok sayıda farklı (mantıksal) bölüm anahtarı | Sınırsız |
| Kapsayıcı başına en fazla depolama | Sınırsız |
| Veritabanı başına en fazla depolama | Sınırsız |

> [!NOTE]
> Bölüm anahtarları, depolama veya işleme için daha yüksek limitlere ihtiyacınız olan iş yüklerini yönetmek en iyi yöntemler için bkz: [sıcak bölüm anahtarları için tasarlama](synthetic-partition-keys.md)
>

En az bir aktarım hızını 400 RU, Cosmos kapsayıcı (veya paylaşılan aktarım hızı veritabanı) olmalıdır. Kapsayıcı büyüdükçe, desteklenen en düşük aktarım hızı da aşağıdaki etkenlere bağlıdır:

* Kapsayıcı içinde kullanılan en fazla depolama alanına 40 RU başına tüketilen depolama GB'lık artışlarla ölçülür. Bir kapsayıcı 100 GB veri içeriyorsa, örneğin, ardından aktarım hızı en az 4000 RU olmalıdır
* Hiç olmadığı kadar kapsayıcıdaki sağlanmış en fazla aktarım hızı. Hizmet, bir kapsayıcı, sağlanan en fazla %10 azaltmayı verimini destekler. Aktarım için 10000 RU artırıldı. Örneğin, ardından olası en düşük sağlanan işleme 1000 RU olacaktır
* Bir paylaşılan aktarım hızı veritabanında hiç olmadığı kadar oluşturduğunuz kapsayıcı toplam sayısı, kapsayıcı başına 100 RU ölçülür. Beş kapsayıcılara paylaşılan aktarım hızı veritabanı oluşturduysanız, örneğin, ardından aktarım hızı en az 500 RU olmalıdır

Bir kapsayıcı veya bir veritabanının güncel ve en düşük aktarım hızı, Azure portalını ya da SDK'ları alınabilir. Daha fazla bilgi için [kapsayıcılar ve veritabanları sağlama aktarım hızını](set-throughput.md). Özet olarak, en düşük sağlanan RU sınırları şunlardır. 

| Resource | Varsayılan limit |
| --- | --- |
| Kapsayıcı başına en az RU ([adanmış aktarım hızı sağlanmış modu](databases-containers-items.md#azure-cosmos-containers)) | 400 |
| Veritabanı başına en az RU ([paylaşılan aktarım hızı sağlanmış modu](databases-containers-items.md#azure-cosmos-containers)) | 400 |
| Kapsayıcı içinde paylaşılan aktarım hızı veritabanı başına en az ru | 100 |
| Tüketilen depolama GB başına en az ru | 40 |

Cosmos DB, kapsayıcı ya da SDK'ları veya portal aracılığıyla veritabanı başına aktarım hızına (RU) esnek ölçeklendirmeyi destekler. Her kapsayıcı, minimum ve maksimum değerler arasında 100 kez 10 ölçek aralığı içindeki zaman uyumlu ve hemen ölçeklendirebilirsiniz. İstenen işleme değer aralığı dışında ise ölçeklendirme zaman uyumsuz olarak gerçekleştirilir. Zaman uyumsuz ölçeklendirme dakika istenen aktarım hızı ve veri depolama boyutu kapsayıcıdaki bağlı olarak saat sürebilir.  

## <a name="control-plane-operations"></a>Denetim düzlemi işlemleri

Yapabilecekleriniz [sağlamak ve Azure Cosmos hesabınızı yönetin](how-to-manage-database-account.md) Azure portalı, Azure PowerShell, Azure CLI ve Azure Resource Manager şablonlarını kullanarak. Aşağıdaki tabloda, abonelik ve hesap işlemlerinin sayısı başına sınırlar listelenmektedir.

| Resource | Varsayılan limit |
| --- | --- |
| Abonelik başına en fazla veritabanı hesabı | Varsayılan olarak 50. Tarafından artırabilirsiniz [bir Azure destek bileti](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) veya aracılığıyla bizimle iletişime geçtiğiniz [Cosmos DB isteyin](mailto:askcosmosdb@microsoft.com)|
| Bölgesel yük devretme işlemleri sayısı | Varsayılan olarak 1/saat. Tarafından artırabilirsiniz [bir Azure destek bileti](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) veya aracılığıyla bizimle iletişime geçtiğiniz [Cosmos DB isteyin](mailto:askcosmosdb@microsoft.com)|

> [!NOTE]
> Bölgesel yük devretme yalnızca tek bölge yazma hesapları için de geçerlidir. Çok bölgeli yazma hesapları gerektirmez veya yazma bölgesini değiştirmek üzerinde herhangi bir sınır vardır.

Cosmos DB, verilerinizin yedeklerini düzenli aralıklarla otomatik olarak alır. Yedekleme bekletme aralıkları ve windows üzerinde daha fazla ayrıntı için bkz. [çevrimiçi yedekleme ve isteğe bağlı veri geri yükleme, Azure Cosmos DB'de](online-backup-and-restore.md).

## <a name="per-container-limits"></a>Kapsayıcı başına limitler

Kullandığınız API bağlı olarak, bir Azure Cosmos kapsayıcı ya da bir koleksiyon, bir tablo göstermek veya grafik. Kapsayıcılar için yapılandırmaları destekler [benzersiz anahtar kısıtlamaları](unique-keys.md), [saklı yordamlar, tetikleyiciler ve UDF'ler](stored-procedures-triggers-udfs.md), ve [dizin oluşturma ilkesi](how-to-manage-indexing-policy.md). Aşağıdaki tabloda, bir kapsayıcı içindeki yapılandırmaları için belirli sınırlar listelenmektedir. 

| Resource | Varsayılan limit |
| --- | --- |
| Veritabanı veya kapsayıcı adının uzunluk üst sınırı | 255 |
| Kapsayıcı başına en fazla saklı yordamlar | 100 <sup>*</sup>|
| Kapsayıcı başına en fazla UDF'leri | 25 <sup>*</sup>|
| Dizin oluşturma ilkesi içindeki yol sayısı| 100 <sup>*</sup>|
| Kapsayıcı başına benzersiz anahtar sayısı|10 <sup>*</sup>|
| Benzersiz anahtar kısıtlaması başına yol sayısı|16 <sup>*</sup>|

<sup>*</sup> Azure destek ile iletişim kurarak veya aracılığıyla bizimle iletişime geçtiğiniz herhangi bir kapsayıcı başına limitler artırabilirsiniz [isteyin Cosmos DB](mailto:askcosmosdb@microsoft.com).

## <a name="per-item-limits"></a>Öğe başına limitler

Kullandığınız API bağlı olarak, bir Azure Cosmos öğesi belgeye bir koleksiyondaki bir satır, tablo veya bir düğüm veya kenar grafikteki temsil edebilir. Aşağıdaki tabloda, Cosmos DB'de öğesi başına sınırlar gösterilmektedir. 

| Resource | Varsayılan limit |
| --- | --- |
| Bir öğenin en büyük boyutu | 2 MB (JSON gösterimi uzunluğunu UTF-8) |
| Bölüm anahtarı değeri uzunluğu en fazla | 2048 bayt |
| ID değeri uzunluğu en fazla | 1024 bayt |
| Özellik öğesi başına en yüksek sayısı | Pratik sınır |
| En fazla iç içe geçme derinliği | Pratik sınır |
| Özellik adının uzunluk üst sınırı | Pratik sınır |
| Özellik değeri uzunluğu en fazla | Pratik sınır |
| Dize özelliği değerinde en fazla uzunluğu | Pratik sınır |
| Sayısal özellik değeri uzunluğu en fazla | Ieee754 çift duyarlıklı 64 bit |

Hiçbir öğe yüklerini hariç uzunluk kısıtlamaları bölüm anahtarı kimliği değerleri ve genel özellikleri ve iç içe geçirme derinliği sayısı gibi sınırlamalar vardır boyutu 2 MB'lık sınırlama. RU kullanımını azaltmak için büyük veya karmaşık öğesi yapıları ile kapsayıcılar için dizin oluşturma ilkesini yapılandırmanız gerekebilir. Bkz: [öğeleri Cosmos DB'de modelleme](how-to-model-partition-example.md) gerçek örnek ve büyük öğelerini yönetmek için desenler için.

## <a name="per-request-limits"></a>İstek başına limitler

Cosmos DB destekleyen [CRUD ve sorgu işlemleri](https://docs.microsoft.com/rest/api/cosmos-db/) kapsayıcılar, öğeleri ve veritabanları gibi kaynaklarda.  

| Resource | Varsayılan limit |
| --- | --- |
| (Bir saklı yordam yürütme veya bir tek sorgu sayfası alma gibi) tek bir işlem için en fazla yürütme zamanı| 5 sn |
| En fazla istek boyutu (saklı yordam, CRUD)| 2 MB |
| En büyük yanıt boyutu (örneğin, sayfalandırılmış sorgusu) | 4 MB |

Bir kez gibi sorgu yürütme zaman aşımı veya yanıt boyutu sınırına ulaştığında bir işlem sonuçlar ve bir devamlılık belirteci bir sayfa yürütme devam etmek için istemciye döndürür. Pratik sınır, tek bir sorgu sayfaları/devamlılıklar arasında çalıştırabilirsiniz süre yoktur.

Cosmos DB için yetkilendirme HMAC kullanır. Ya da bir ana anahtar, kullanabilirsiniz veya [kaynak belirteçleri](secure-access-to-data.md) anahtarları veya öğeleri kapsayıcıları gibi kaynaklarına ayrıntılı erişim denetimi için bölümleme. Aşağıdaki tabloda, Cosmos DB yetkilendirme belirteçlerinde yönelik sınırlar listelenir.

| Resource | Varsayılan limit |
| --- | --- |
| En fazla ana belirteç süre sonu zamanı | 15 dakika  |
| En düşük kaynak belirteci süre sonu zamanı | 10 dakikalık  |
| En fazla kaynak belirteci süre sonu zamanı | Varsayılan olarak 24 h. Tarafından artırabilirsiniz [bir Azure destek bileti](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) veya aracılığıyla bizimle iletişime geçtiğiniz [Cosmos DB isteyin](mailto:askcosmosdb@microsoft.com)|
| Maksimum saat eğriltme belirteci yetkilendirme için| 15 dakika |

Cosmos DB tetikleyici yürütülürken yazma işlemleri destekler. Hizmet, bir ön tetikleyici ve bir sonrası tetikleyici yazma işlemi başına en fazla destekler. 

## <a name="sql-query-limits"></a>SQL sorgu sınırları

Cosmos DB destekleyen kullanarak öğeleri sorgulama [SQL](how-to-sql-query.md). Aşağıdaki tabloda, örneğin yan tümcesi ya da sorgu Uzunluk sayısı bakımından sorgu ifadelerine kısıtlamaları açıklamaktadır.

| Resource | Varsayılan limit |
| --- | --- |
| SQL sorgusu uzunluğunun üst sınırı| 256 KB <sup>*</sup>|
| Sorgu başına en fazla birleştirmeler| 5 <sup>*</sup>|
| Sorgu başına en fazla Equal| 2000 <sup>*</sup>|
| Sorgu başına en fazla ORs| 2000 <sup>*</sup>|
| Sorgu başına en fazla UDF'leri| 10 <sup>*</sup>|
| Başına en fazla bağımsız değişken İFADESİNDEKİ| 6000 <sup>*</sup>|
| Çokgen başına en fazla puan| 4096 <sup>*</sup>|

<sup>*</sup> Azure destek ile iletişim kurarak veya aracılığıyla bizimle iletişime geçtiğiniz herhangi bir SQL sorgusu limitler artırabilirsiniz [isteyin Cosmos DB](mailto:askcosmosdb@microsoft.com).

## <a name="mongodb-api-specific-limits"></a>MongoDB API özgü sınırları

Cosmos DB, MongoDB kablo protokolüne karşı MongoDB yazılmış uygulamalar için destekler. Desteklenen komutlar bulun ve protokol sürümlerin [desteklenen MongoDB özellikleri ve söz dizimi](mongodb-feature-support.md).

Aşağıdaki tabloda, MongoDB özellik desteği için belirli sınırlar listelenmektedir. (Temel) API SQL için belirtilen diğer hizmet sınırları, MongoDB API için de geçerlidir.

| Resource | Varsayılan limit |
| --- | --- |
| En fazla MongoDB sorgu bellek boyutu | 40 MB |
| MongoDB işlemleri için en uzun yürütme süresi| 30 saniye |

## <a name="try-cosmos-db-free-limits"></a>Cosmos DB ücretsiz sınırların deneyin

Aşağıdaki tabloda yönelik sınırlar listelenir [Azure Cosmos DB'yi ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/) deneme.

| Resource | Varsayılan limit |
| --- | --- |
| Deneme süresi | 30 gün (olabilir herhangi sayıda yenilenir) |
| Abonelik (SQL, Gremlin, tablo API'si) başına en fazla kapsayıcılar | 1 |
| Abonelik (MongoDB API'SİYLE) başına en fazla kapsayıcılar | 3 |
| Kapsayıcı başına en fazla aktarım hızı | 5000 |
| İşleme düzeyi paylaşılan veritabanı başına en fazla aktarım hızı | 20000 |
| Hesap başına toplam depolama | 10 GB |

Try Cosmos DB genel dağıtım, yalnızca orta ABD, Kuzey Avrupa ve Güneydoğu Asya bölgelerinde destekler. Azure Cosmos DB'yi deneyin hesapları için Azure destek bileti oluşturulamıyor. Bununla birlikte, mevcut destek planları ile aboneleri için destek sağlanır.

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB hakkında daha fazla temel kavramları [genel dağıtım](distribute-data-globally.md) ve [bölümleme](partitioning-overview.md) ve [sağlanan aktarım hızı](request-units.md).

Dört hızlı başlangıçtan biriyle Azure Cosmos DB kullanmaya başlayın:

* [Azure Cosmos DB SQL API’yi kullanmaya başlama](create-sql-api-dotnet.md)
* [MongoDB için Azure Cosmos DB'nin API'sini kullanmaya başlama](create-mongodb-nodejs.md)
* [Azure Cosmos DB Cassandra API’yi kullanmaya başlama](create-cassandra-dotnet.md)
* [Azure Cosmos DB Graph API’yi kullanmaya başlama](create-graph-dotnet.md)
* [Azure Cosmos DB Tablo API’yi kullanmaya başlama](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Azure Cosmos DB’yi ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/)
