---
title: Azure SQL veritabanı vCore tabanlı kaynak sınırları - esnek havuzlar | Microsoft Docs
description: Bu sayfa, Azure SQL veritabanında esnek havuzlar için bazı ortak vCore tabanlı kaynak sınırları açıklar.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 01f213c7cf5f6be3ef84601a50bb4455422faf22
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309921"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-elastic-pools-preview"></a>Azure SQL veritabanı vCore tabanlı esnek havuzlar (Önizleme) için model sınırları satın alma

Bu makale, Azure SQL Database esnek havuzlar ve vCore tabanlı satın alma modeli kullanarak havuza alınmış veritabanları için ayrıntılı kaynak sınırları sağlar.

DTU tabanlı satın alma modeli sınırları için bkz: [SQL veritabanı DTU tabanlı kaynak sınırları - esnek havuzlar](sql-database-dtu-resource-limits-elastic-pools.md).


## <a name="elastic-pool-storage-sizes-and-performance-levels"></a>Esnek havuz: depolama boyutlarına ve performans düzeyleri

SQL Database esnek havuzlar için aşağıdaki tablolarda her hizmeti katmanını ve performans düzeyinde kaynakları gösterir. Hizmet katmanı, performans düzeyi ve depolama miktarını kullanarak ayarlayabilirsiniz [Azure portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases), [Azure CLI](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases), veya [REST API](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases).

> [!NOTE]
> Esnek havuzlar bulunan tek veritabanlarını kaynak sınırları genellikle aynı performans düzeyine sahip havuzları dışında tek veritabanları ile aynıdır. Örneğin, bir GP_Gen4_1 veritabanı için en fazla eşzamanlı çalışan 200 çalışanları olur. Bu nedenle, GP_Gen4_1 havuzundaki bir veritabanı için en fazla eşzamanlı çalışan olduğu de 200 çalışanlar. Eşzamanlı çalışan GP_Gen4_1 havuzundaki toplam sayısı 210 olduğunu unutmayın.

### <a name="general-purpose-service-tier"></a>Genel amaçlı hizmet katmanı

#### <a name="generation-4-compute-platform"></a>Nesil 4 işlem platformu
|Performans düzeyi|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|H/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|1|2|4|8|16|24|
|Bellek (GB)|7|14|28|56|112|168|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|En büyük veri boyutu (GB)|512|756|1536|2048|3584|4096|
|En büyük günlük boyutu|154|227|461|614|1075|1229|
|TempDB size(DB)|32|64|128|256|384|384|
|Hedef IOPS (64 KB)|500|1000|2000|4000|7000|7000|
|G/ç gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|
|En fazla eşzamanlı çalışan (istek sayısı)|210|420|840|1680|3360|5040|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|
|En büyük havuz yoğunluğu|100|200|500|500|500|500|
|Min/max esnek havuz tıklatın-durdurur|0, 0.25, 0,5, 1|0, 0.25, 0,5, 1, 2|0, 0.25, 0,5, 1, 2, 4|0, 0.25, 0,5, 1, 2, 4, 8|0, 0.25, 0,5, 1, 2, 4, 8, 16|0, 0.25, 0,5, 1, 2, 4, 8, 16, 24|
|Çoğaltma sayısı|1|1|1|1|1|1|
|Birden çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Genişleme okuma|Yok|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||

#### <a name="generation-5-compute-platform"></a>Nesil 5 işlem platformu
|Performans düzeyi|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |--: |--: |--: |--: |
|H/W oluşturma|5|5|5|5|5|5|5|5|
|Sanal çekirdekler|2|4|8|16|24|32|40|80|
|Bellek (GB)|11|22|44|88|132|176|220|440|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|En büyük veri boyutu (GB)|512|756|1536|2048|3072|4096|4096|4096|
|En büyük günlük boyutu|154|227|461|614|922|1229|1229|1229|
|TempDB size(DB)|64|128|256|384|384|384|384|384|
|Hedef IOPS (64 KB)|500|1000|2000|4000|6000|7000|7000|7000|
|G/ç gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|
|En fazla eşzamanlı çalışan (istek sayısı)|210|420|840|1680|2520|3360|4200|8400
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|30000|30000|
|En büyük havuz yoğunluğu|100|200|500|500|500|500|500|500|
|Min/max esnek havuz tıklatın-durdurur|0, 0.25, 0,5, 1, 2|0, 0.25, 0,5, 1, 2, 4|0, 0.25, 0,5, 1, 2, 4, 8|0, 0.25, 0,5, 1, 2, 4, 8, 16|0, 0.25, 0,5, 1, 2, 4, 8, 16, 24|0, 0,5, 1, 2, 4, 8, 16, 24, 32|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40, 80|
|Çoğaltma sayısı|1|1|1|1|1|1|1|1|
|Birden çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Genişleme okuma|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||

### <a name="business-critical-service-tier"></a>İş kritik hizmet katmanı

#### <a name="generation-4-compute-platform"></a>Nesil 4 işlem platformu
|Performans düzeyi|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|H/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|1|2|4|8|16|24|
|Bellek (GB)|7|14|28|56|112|168|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|1|2|4|8|20|36|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|En büyük veri boyutu (GB)|1024|1024|1024|1024|1024|1024|
|En büyük günlük boyutu|307|307|307|307|307|307|
|TempDB size(DB)|32|64|128|256|384|384|
|Hedef IOPS (64 KB)|5000|10000|20000|40000|80000|120000|
|G/ç gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|
|En fazla eşzamanlı çalışan (istek sayısı)|210|420|840|1680|3360|5040|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|
|En büyük havuz yoğunluğu|Yok|50|100|100|100|100|
|Min/max esnek havuz tıklatın-durdurur|Yok|0, 0.25, 0,5, 1, 2|0, 0.25, 0,5, 1, 2, 4|0, 0.25, 0,5, 1, 2, 4, 8|0, 0.25, 0,5, 1, 2, 4, 8, 16|0, 0.25, 0,5, 1, 2, 4, 8, 16, 24|
|Çoğaltma sayısı|3|3|3|3|3|3|
|Birden çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Genişleme okuma|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||

#### <a name="generation-5-compute-platform"></a>Nesil 5 işlem platformu
|Performans düzeyi|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |--: |--: |--: |--: |
|H/W oluşturma|5|5|5|5|5|5|5|5|
|Sanal çekirdekler|2|4|8|16|24|32|40|80|
|Bellek (GB)|11|22|44|88|132|176|220|440|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|1.571|3,142|6.284|15.768|25.252|37.936|52.22|131.64|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|G/ç gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|
|En büyük veri boyutu (GB)|1024|1024|1024|1024|2048|4096|4096|4096|
|En büyük günlük boyutu|307|307|307|307|614|1229|1229|1229|
|TempDB size(DB)|64|128|256|384|384|384|384|384|
|Hedef IOPS (64 KB)|5000|10000|20000|40000|60000|80000|100000|200000
|En fazla eşzamanlı çalışan (istek sayısı)|210|420|840|1680|2520|3360|5040|8400|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|30000|30000|
|En büyük havuz yoğunluğu|Yok|50|100|100|100|100|100|100|
|Min/max esnek havuz tıklatın-durdurur|Yok|0, 0.25, 0,5, 1, 2, 4|0, 0.25, 0,5, 1, 2, 4, 8|0, 0.25, 0,5, 1, 2, 4, 8, 16|0, 0.25, 0,5, 1, 2, 4, 8, 16, 24|0, 0,5, 1, 2, 4, 8, 16, 24, 32|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40|0, 0,5, 1, 2, 4, 8, 16, 24, 32, 40, 80|
|Çoğaltma sayısı|3|3|3|3|3|3|3|3|
|Birden çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Genişleme okuma|Evet|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||

Bir esnek havuzun tüm vCores meşgul ise, havuzdaki her veritabanı sorguları işlemek için işlem kaynaklarını eşit miktarda alır. SQL Veritabanı hizmeti, eşit dilimlerde işlem süresi sunarak veritabanları arasında kaynak paylaşım eşitliğini sağlar. Her bir veritabanı için veritabanı başına vCore minimum sıfır olmayan bir değere ayarlandığında, aksi takdirde garanti kaynak herhangi bir sayıdaki yanı sıra eşitliği paylaşımı esnek havuz kaynak değil.

### <a name="database-properties-for-pooled-databases"></a>Havuza alınmış veritabanları için veritabanı özellikleri

Aşağıdaki tabloda, havuza alınmış veritabanları için özellikleri açıklar.

| Özellik | Açıklama |
|:--- |:--- |
| Veritabanı başına en fazla vCores |Havuzdaki diğer veritabanları tarafından kullanılabilir kullanımını temel alan varsa, havuzdaki herhangi bir veritabanı, kullanabilecek vCores maksimum sayısı. Veritabanı başına en fazla vCores bir veritabanı için kaynak garantisi değil. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı kullanımı genişliğindeki yükselmeleri işlemek için yüksek olan veritabanı başına en fazla vCores ayarlayın. Havuz genellikle tüm veritabanlarının eşzamanlı olarak en üst kullanım seviyesine çıkmadığı sıcak ve soğuk kullanım modellerini varsaydığından, bir miktar aşırı ayırma beklenir.|
| Veritabanı başına Min vCores |Havuzdaki herhangi bir veritabanı garanti vCores minimum sayısı. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı başına min vCores 0 olarak ayarlayın ve ayrıca varsayılan değerdir. Bu özellik 0 ve veritabanı başına ortalama vCores kullanımını arasında herhangi bir yere ayarlanır. Havuz ve veritabanı başına min vCores bulunan veritabanlarının sayısına ürün havuzu başına vCores aşamaz.|
| Veritabanı başına en fazla depolama |Bir veritabanı için kullanıcı tarafından bir havuzda ayarlanan en fazla veritabanı boyutu. Havuza alınmış veritabanları bir veritabanı ulaşmak boyutu sınırlı olacak şekilde ayrılmış havuz depolama paylaşmak küçük kalan depolama havuzu ve veritabanı boyutu. En büyük veritabanı boyutu, veri dosyalarının en büyük boyuta başvuruyor ve günlük dosyaları tarafından kullanılan alanı içermez. |
|||
 
## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [SQL veritabanı SSS](sql-database-faq.md) sık sorulan soruların yanıtları için.
- Genel Azure sınırları hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md).
