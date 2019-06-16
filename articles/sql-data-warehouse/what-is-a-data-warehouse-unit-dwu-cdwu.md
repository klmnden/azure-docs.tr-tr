---
title: Azure SQL veri ambarı veri ambarı birimleri (Dwu'lar cDWUs) | Microsoft Docs
description: Veri ambarı birimi (dwu'ları, cDWUs) fiyat ve performans ve birim sayısını değiştirmek nasıl en iyi duruma getirmek için ideal sayısını seçme önerileri.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: design
ms.date: 05/30/2019
ms.author: martinle
ms.reviewer: igorstan
mscustom: sqlfreshmay19
ms.openlocfilehash: d20a600951a0fe586e981adf12127072df1b744c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66428017"
---
# <a name="data-warehouse-units-dwus-and-compute-data-warehouse-units-cdwus"></a>Veri ambarı birimi (Dwu) ve işlem veri ambarı birimi (cDWUs)

Veri ambarı birimi (dwu'ları, cDWUs) fiyat ve performans ve birim sayısını değiştirmek nasıl en iyi duruma getirmek için ideal sayısını seçme önerileri.

## <a name="what-are-data-warehouse-units"></a>Veri ambarı birimleri nedir

Azure SQL veri ambarı CPU, bellek ve GÇ veri ambarı birimi (Dwu) adı verilen işlem ölçek birimlerine paketlenir. DWU soyut, normalleştirilmiş bir ölçü performans ve işlem kaynaklarını temsil eder. Bir değişiklik, hizmet düzeyi için performans ve maliyet, sisteminizin sırayla ayarlar sistem kullanılabilir Dwu sayısı değiştirir.

Daha yüksek performans için veri ambarı birimleri sayısını artırabilirsiniz. Daha az performans için veri ambarı birimleri azaltın. Veri ambarı birimlerini değiştirmek depolama maliyetlerini etkilemez için depolama ve işlem maliyetleri ayrı olarak faturalandırılır.

Performans veri ambarı birimleri için bu veri ambarı iş yükü ölçümleri temel alır:

- Ne kadar hızlı bir standart veri ambarı sorgusu büyük sayıda satırı taraması ve ardından karmaşık bir toplama gerçekleştirebilirsiniz. Bu işlem, yoğun g/ç ve CPU olur.
- Ne kadar hızlı veri ambarı, verileri Azure depolama Blobları veya Azure Data Lake içe alabilir. Bu işlem, ağ ve CPU yoğun olur.
- Ne kadar hızlı [ `CREATE TABLE AS SELECT` ](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) T-SQL komutu, bir tablo kopyalayabilir. Bu işlem, verilerin depolama alanından okunmasını, gerecin tüm düğümlerine dağıtılmasını ve yeniden depolama alanına yazılmasını içerir. Bu CPU, GÇ ve ağ yoğunluklu bir işlemdir.

Dwu'lar artırma:

- Taramaların ve toplamalar CTAS deyimleri için sistemin performansını doğrusal olarak değiştirir
- Okuyucular ve yazıcılar için PolyBase yükleme işlemleri sayısı artar
- En fazla bir eş zamanlı sorguları ve eşzamanlılık yuvaları sayısını artırır.

## <a name="service-level-objective"></a>Hizmet düzeyi hedefi

Hizmet düzeyi hedefi (SLO), veri Ambarınızı maliyet ve performans düzeyini belirler ölçeklenebilirlik ayarıdır. Hizmet düzeyleri için 2. nesil işlem data warehouse birimlerinde (cDWU), örneğin DW2000c ölçülür. Dwu'lar, örneğin DW2000 Gen1 hizmet düzeyleri ölçülür.
  > [!NOTE]
  > Azure SQL veri ambarı Gen2, son 100 cDWU en düşük bilgi işlem katmanını desteklemek için ek ölçek özellikleri eklendi. Alt katmanları işlem gerektiren varolan veri ambarları Gen1 üzerinde şu anda, artık şu an için ek ücret ödemeden kullanılabilir bölgelerde Gen2'ye yükseltebilirsiniz.  Bölgeniz henüz desteklenmiyor, desteklenen bir bölge için hala yükseltebilirsiniz. Daha fazla bilgi için [yükseltmek için 2. nesil](upgrade-to-latest-generation.md).

T-SQL, hizmet düzeyi ve performans katmanını veri ambarınız için servıce_objectıve ayarı belirler.

```sql
--Gen1
CREATE DATABASE myElasticSQLDW
WITH
(    SERVICE_OBJECTIVE = 'DW1000'
)
;

--Gen2
CREATE DATABASE myComputeSQLDW
WITH
(    SERVICE_OBJECTIVE = 'DW1000c'
)
;
```

## <a name="performance-tiers-and-data-warehouse-units"></a>Performans katmanları ve veri ambarı birimleri

Her performans katmanı, veri ambarı birimleri için biraz daha farklı bir ölçü kullanır. Ölçek birimi için faturalandırma doğrudan çevirir. Bu fark faturada yansıtılır.

- Gen1 veri ambarları, veri ambarı birimi (Dwu) ölçülür.
- 2\. nesil veri ambarları ölçülür, veri ambarı birimi (cDWUs) işlem.

Dwu'lar hem cDWUs yukarı veya aşağı ölçeklendirme işlem desteği ve duraklatma işlem, veri ambarı kullanmanız gerekmez. Bu işlemlerin tümü isteğe bağlı. Gen2 performansını artırmak için işlem düğümlerinde disk tabanlı yerel önbelleği kullanır. Ölçeklendirin veya sistem duraklatmak, önbellekte geçersiz kılınan ve önbellek hazırlanıyor, bir süre önce en iyi performans elde edilir şekilde gereklidir.  

Veri ambarı birimleri arttıkça, bilgi işlem kaynaklarını doğrusal olarak artmaktadır. 2\. nesil yüksek ölçek ve en iyi sorgu performansını sağlar. Gen2 sistemleri de çoğu önbellek kullanımını olun.

### <a name="capacity-limits"></a>Kapasite sınırları

Her bir SQL server (örn. myserver.database.windows.net) sahip bir [veritabanı işlem birimi (DTU)](../sql-database/sql-database-what-is-a-dtu.md) belirli bir veri ambarı birimleri sayısını veren kota. Daha fazla bilgi için [iş yükü yönetimi kapasite sınırları](sql-data-warehouse-service-capacity-limits.md#workload-management).

## <a name="how-many-data-warehouse-units-do-i-need"></a>Kaç tane veri ambarı birimleri ihtiyacım var

Veri ambarı birimleri ideal sayısını çok iş yükünüzü ve sisteme yüklediğiniz veri miktarına bağlıdır.

İş yükünüz için en iyi DWU bulmaya yönelik adımlar:

1. Daha küçük DWU seçerek başlayın.
2. Siz gözleyin performans karşılaştırma seçildi Dwu sayısı gözleme sisteme veri yüklerini test, uygulama performansını izleme.
3. Tüm ek gereksinimleri düzenli yüksek etkinlik dönemlerini için belirleyin. Önemli gösteren iş yükleri Zirve ve etkinlik troughs sık ölçeklendirilmesi gerekebilir.

SQL veri ambarı, işlem ve sorgu büyük miktarlarda veri çok büyük miktarda sağlayabilirsiniz genişleme sistemidir. Özellikle büyük Dwu'lar ölçeklendirmeye yönelik gerçek kapasitesini görmek için CPU akış için yeterli veri olmasını sağlamak için ölçeği gibi veri kümesini ölçeklendirin öneririz. Ölçek test etmek için en az 1 TB'ı kullanmanızı öneririz.

> [!NOTE]
>
> İş işlem düğümleri arasında bölünmesi, sorgu performansı daha fazla paralelleştirme sayesinde yalnızca artırır. Ölçeklendirme performansınızı değişmeyen olduğunu fark ederseniz, tablo tasarımı ve/veya sorgularınızı ayarlamak gerekebilir. Sorgu ayarlama kılavuzu için bkz. [kullanıcı sorguları yönetme](sql-data-warehouse-overview-manage-user-queries.md).

## <a name="permissions"></a>İzinler

Veri ambarı birimlerini değiştirmek, açıklanan izinleri gerektirir [ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql).

SQL DB Katılımcısı ve SQL Server katılımcısı gibi Azure kaynakları için yerleşik roller DWU ayarlarını değiştirebilirsiniz.

## <a name="view-current-dwu-settings"></a>Geçerli DWU ayarlarını görüntüle

Geçerli DWU ayarını görüntülemek için:

1. SQL Server Nesne Gezgini, Visual Studio'da açın.
2. Mantıksal SQL veritabanı sunucusu ile ilişkili ana veritabanına bağlanın.
3. Sys.database_service_objectives dinamik yönetim görünümünden seçin. Örnek aşağıda verilmiştir:

```sql
SELECT  db.name [Database]
,       ds.edition [Edition]
,       ds.service_objective [Service Objective]
FROM    sys.database_service_objectives   AS ds
JOIN    sys.databases                     AS db ON ds.database_id = db.database_id
;
```

## <a name="change-data-warehouse-units"></a>Veri ambarı birimlerini değiştirmek

### <a name="azure-portal"></a>Azure portal

Dwu'lar veya cDWUs değiştirmek için:

1. Açık [Azure portalında](https://portal.azure.com), veritabanınızı açın ve tıklayın **ölçek**.

2. Altında **ölçek**, kaydırıcıyı sola veya ettirerek DWU ayarını değiştirmek için sağ.

3. **Kaydet**’e tıklayın. Bir onay iletisi görüntülenir. Onaylamak için **evet**’e, iptal etmek için **hayır**’a tıklayın.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Dwu'lar veya cDWUs değiştirmek için kullanın [kümesi AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) PowerShell cmdlet'i. Aşağıdaki örnek, DW1000 için MyServer sunucuda barındırılan MySQLDW veritabanı için hizmet düzeyi hedefi ayarlar.

```Powershell
Set-AzSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

Daha fazla bilgi için [SQL veri ambarı için PowerShell cmdlet'leri](sql-data-warehouse-reference-powershell-cmdlets.md)

### <a name="t-sql"></a>T-SQL

T-SQL ile geçerli DWU veya cDWU ayarlarını görüntülemek, ayarlarını değiştirin ve ilerleme durumunu denetleyin.

Dwu'lar veya cDWUs değiştirmek için:

1. Mantıksal SQL veritabanı sunucunuzla ilişkili ana veritabanına bağlanın.
2. Kullanım [ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql) TSQL deyimi. Aşağıdaki örnekte, hizmet düzeyi hedefi için DW1000 MySQLDW veritabanı için ayarlar.

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

### <a name="rest-apis"></a>REST API'leri

Dwu'ları değiştirmek için kullanın [oluşturma veya güncelleştirme veritabanı](/rest/api/sql/databases/createorupdate) REST API. Aşağıdaki örnek, DW1000 için MyServer sunucuda barındırılan MySQLDW, veritabanı için hizmet düzeyi hedefi ayarlar. ResourceGroup1 adlı bir Azure kaynak grubunda sunucusudur.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

Daha fazla REST API örnekleri için bkz [SQL veri ambarı için REST API'leri](sql-data-warehouse-manage-compute-rest-api.md).

## <a name="check-status-of-dwu-changes"></a>DWU değişiklikleri durumunu denetleyin

DWU değişiklikleri tamamlanması birkaç dakika sürebilir. Otomatik ölçeklendirme, başka bir eylem ile devam etmeden önce belirli işlemleri tamamlandığından emin olmak için mantıksal uygulama düşünün.

Çeşitli uç noktaları aracılığıyla veritabanı durumu denetleniyor Otomasyon doğru olanak tanır. Portal tamamlandıktan sonra bildirim işlem ve veritabanı geçerli durumunun sağlar ancak programlı durumunu denetlemek için izin vermiyor.

Azure portalı ile ölçek genişletme işlemleri için veritabanı durumunu denetleyemezsiniz.

DWU değişiklikleri durumunu denetlemek için:

1. Mantıksal SQL veritabanı sunucunuzla ilişkili ana veritabanına bağlanın.
2. Veritabanı durumunu kontrol etmek için aşağıdaki sorguyu gönderin.

```sql
SELECT    *
FROM      sys.databases
;
```

1. İşlem durumunu denetlemek için aşağıdaki sorguyu Gönder

```sql
SELECT    *
FROM      sys.dm_operation_status
WHERE     resource_type_desc = 'Database'
AND       major_resource_id = 'MySQLDW'
;
```

Bu DMV SQL veri ambarınızın işlem ve geçerli işlemin ın_progress veya COMPLETED işlemi, durumu gibi çeşitli yönetim işlemleriyle ilgili bilgileri döndürür.

## <a name="the-scaling-workflow"></a>Ölçeklendirme iş akışı

Bir ölçeklendirme işlemi başlattığınızda sistem önce tüm açık oturum, tutarlı bir duruma emin olmak için herhangi bir açık işlemleri geri devre dışı bırakır. Bu işlem geri alma tamamlandıktan sonra ölçeklendirme ölçek işlemleri için yalnızca gerçekleşir.  

- Bir ölçek artırma işleminin sistem tüm işlem düğümleri, hükümlerine ek işlem düğümleri ayırır ve ardından depolama katmanı için etkilenebilecek.
- Bir ölçek azaltma işlemi için sistem tüm işlem düğümleri ayırır ve ardından depolama katmanı yalnızca gerekli düğümlere etkilenebilecek.

## <a name="next-steps"></a>Sonraki adımlar

Performansı yönetme hakkında daha fazla bilgi için bkz: [iş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md) ve [bellek ve eşzamanlılık sınırları](memory-and-concurrency-limits.md).
