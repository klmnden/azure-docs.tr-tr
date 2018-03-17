---
title: Data Warehouse birimlerinde (Dwu, cDWUs) Azure SQL Data Warehouse nedir? | Microsoft Docs
description: "Azure SQL veri ambarı özellikleri genişletme performans. Dwu, cDWUs, ayarlayarak ölçeğini veya duraklatıp işlem kaynakları maliyet tasarrufu sağlamak."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 03/15/2018
ms.author: jrj;barbkess
ms.openlocfilehash: f634bdde2c71f7563df11f686d7ce217311df81d
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="data-warehouse-units-dwus-and-compute-data-warehouse-units-cdwus"></a>Veri ambarı birimlerini (Dwu'lar) ve bilgi işlem Data Warehouse birimleri (cDWUs)
Veri ambarı birimlerini (cDWUS) Azure SQL Data Warehouse için işlem ve veri ambarı birimlerini (Dwu'lar) açıklanmaktadır. Data warehouse birimleri ve bunları sayısını değiştirmek nasıl ideal sayısını seçmeye ilişkin öneriler içerir. 

## <a name="what-are-data-warehouse-units"></a>Data Warehouse birimleri nelerdir?
Veri ambarı birimlerini (Dwu'lar) olarak adlandırılan hesaplama ölçek birimleri, SQL veri ambarı CPU, bellek ve g/ç ile paketlenmiştir. Bir DWU soyut, normalleştirilmiş bir ölçü birimi performans ve işlem kaynaklarını temsil eder. Hizmet düzeyi değiştirerek sırayla performansını ve maliyetini sisteminizi ayarlar sistemine ayrılan Dwu sayısı değiştirin. 

Daha yüksek performans için ödeme için veri ambarı birim sayısını artırabilirsiniz. Daha az performans için ödeme için data warehouse birimleri azaltın. Veri ambarı birimlerini değiştirme depolama maliyetleri etkilemesini depolama ve işlem maliyetleri ayrı olarak faturalandırılır.

Performans veri ambarı birimleri için bu veri ambarı iş yükü ölçümlerinin dayanır:

- Ne kadar hızlı standart veri ambarı sorgusu çok sayıda satır tarayabilir ve karmaşık bir toplama gerçekleştirmek? Bu işlem, g/ç ve CPU yoğun olur.
- Ne kadar hızlı veri ambarı verileri Azure Storage Bloblarında veya Azure Data Lake işleyebilen? Bu işlem, ağ ve CPU yoğun olur. 
- Ne kadar hızlı yapabilirsiniz [CREATE TABLE AS SELECT](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) T-SQL komutunu kopyalayın tablo? Bu işlem, verilerin depolama alanından okunmasını, gerecin tüm düğümlerine dağıtılmasını ve yeniden depolama alanına yazılmasını içerir. Bu CPU, g/ç ve ağ yoğunluklu bir işlemdir.

Dwu artırma:
- Doğrusal olarak taramaları, toplamalar ve CTAS deyimleri için sistem performansını değiştirir
- Okuyucular ve PolyBase yükleme işlemleri için yazarları sayısını artırır
- Eş zamanlı sorgular ve eşzamanlılık yuvaları en fazla sayısını artırır.

## <a name="performance-tiers-and-data-warehouse-units"></a>Performans katmanı ve Data Warehouse birimleri

Her performans katmanı biraz farklı bir ölçü kendi data warehouse birimleri için kullanır. Ölçek birimi için faturalama doğrudan çevirir bu fark fatura üzerinde yansıtılır.

- Esneklik performans katmanı Data Warehouse birimlerinde (Dwu) ölçülür için en iyi duruma getirilmiş.
- İşlem performans katmanı ölçülür için en iyi duruma getirilmiş Data Warehouse birimleri (cDWUs) işlem. 

Dwu ve cDWUs yukarı veya aşağı ölçeklendirme işlem desteği ve duraklatma işlem veri ambarını kullan gerekmiyorsa. Bu işlemlerin tümü isteğe bağlı. En iyi duruma getirilmiş işlem için performans katmanı ayrıca yerel bir disk tabanlı önbellek işlem düğümlerinde performansını artırmak için kullanır. Ölçeklendirme veya sistem duraklatmak, önbellekte geçersiz kılınan ve önbellek ısıtma, bir süre en iyi performans elde önce böylece gereklidir.  

Data warehouse birimleri arttıkça, bilgi işlem kaynakları doğrusal olarak artmaktadır. En iyi duruma getirilmiş işlem için en iyi sorgu performansını performans katmanı sağlar ve en yüksek ölçek ancak daha yüksek bir giriş fiyat vardır. Performans için sabit bir isteğe bağlı olan işletmeler için tasarlanmıştır. Bu sistemler önbellek çoğu kullanılmasını sağlamak. 

### <a name="capacity-limits"></a>Kapasite sınırları
Her bir SQL server (örneğin, myserver.database.windows.net) sahip bir [veritabanı işlem birimi (DTU)](../sql-database/sql-database-what-is-a-dtu.md) kota data warehouse birimleri belirli bir sayıda izin verir. Daha fazla bilgi için bkz: [iş yükü yönetim kapasite limitlerini](sql-data-warehouse-service-capacity-limits.md#workload-management).


## <a name="how-many-data-warehouse-units-do-i-need"></a>Kaç tane data warehouse birimleri ihtiyacım var mı?
Data warehouse birimleri ideal sayısı çok yükünüzü ve sisteme yüklenen veri miktarına bağlıdır.

İş yükü için en iyi DWU bulmak için adımlar:

1. Geliştirme sırasında esneklik performans katmanı için en iyi duruma getirilmiş kullanarak daha küçük bir DWU seçerek başlayın.  Bu aşamada işlevsel doğrulama etkinleştirildiğinde bu yana esneklik performans katmanı için iyileştirilmiş makul bir seçenektir. İyi bir başlangıç noktası DW200 ' dir. 
2. Seçili Dwu sayısı, gözlemlemek performans karşılaştırıldığında Gözlemleme sisteme, veri yüklerini test gibi uygulama performansını izleyin.
3. Tüm ek gereksinimleri için en yüksek etkinlik düzenli dönemleri tanımlayın. İş yükünü önemli yükselmeleri ve troughs etkinliği gösterir ve sık ölçeklendirmek için iyi bir neden yoktur, en iyi duruma getirilmiş için esneklik performans katmanı ayrıcalık tanıma.
4. 1000'den fazla DWU gerekiyorsa, bu en iyi performansı sağlandığından sonra işlem performans katmanı için iyileştirilmiş ayrıcalık tanıma.

SQL Data Warehouse, işlem ve sorgu oldukça büyük miktarda veri çok büyük miktarda kaynak sağlayabilirsiniz genişleme sistemidir. Özellikle büyük Dwu ölçeklendirmeye yönelik gerçek kapasitesini görmek için veri kümesi, CPU'lar akış için yeterli veri sağlamak için ölçeklendirmenize göre ölçeklendirme önerilir. Ölçek test etmek için en az 1 TB kullanmanızı öneririz.

> [!NOTE]
>
> İş işlem düğümleri arasında bölünebilir, sorgu performansı ile daha fazla paralelleştirme yalnızca artırır. Ölçeklendirme performansınızı değişmeyen olduğunu fark ederseniz, tablo tasarımı ve/veya sorgularınızı ayarlamak gerekebilir. Aşağıdaki yönergeler ayarlama sorgu için başvurun [performans](sql-data-warehouse-overview-manage-user-queries.md) makaleleri. 

## <a name="permissions"></a>İzinler

Veri ambarı birimlerini değiştirme açıklanan izinleri gerektirir [ALTER DATABASE][ALTER DATABASE]. 

## <a name="view-current-dwu-settings"></a>Geçerli DWU ayarlarını görüntüle

Geçerli DWU ayarını görüntülemek için:

1. SQL Server Nesne Gezgini Visual Studio'da açın.
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

## <a name="change-data-warehouse-units"></a>Veri ambarı birimlerini değiştirme

### <a name="azure-portal"></a>Azure portalına
Dwu veya cDWUs değiştirmek için:

1. Açık [Azure portal](https://portal.azure.com), veritabanınızı açın ve'ı tıklatın **ölçek**.

2. Altında **ölçek**, kaydırıcıyı sola veya DWU ayarını değiştirmek için sağ.

3. **Kaydet**’e tıklayın. Bir onay iletisi görüntülenir. Tıklatın **Evet** onaylamak için veya **hiçbir** iptal etmek için.

### <a name="powershell"></a>PowerShell
Dwu veya cDWUs değiştirmek için [Set-AzureRmSqlDatabase] [Set-AzureRmSqlDatabase] PowerShell cmdlet'ini kullanın. Aşağıdaki örnek, MyServer sunucusunda barındırılan MySQLDW veritabanı için DW1000 için hizmet düzeyi hedefi ayarlar.

```Powershell
Set-AzureRmSqlDatabase -DatabaseName "MySQLDW" -ServerName "MyServer" -RequestedServiceObjectiveName "DW1000"
```

### <a name="t-sql"></a>T-SQL
T-SQL ile geçerli DWU veya cDWU ayarlarını görüntülemek, ayarlarını değiştirin ve ilerleme durumunu denetleyin. 

Dwu veya cDWUs değiştirmek için:

1. Mantıksal SQL veritabanı sunucunuzla ilişkili ana veritabanına bağlanın.
2. Kullanım [ALTER DATABASE] [ ALTER DATABASE] TSQL deyimi. Aşağıdaki örnek, DW1000 için MySQLDW veritabanı için hizmet düzeyi hedefi ayarlar. 

```Sql
ALTER DATABASE MySQLDW
MODIFY (SERVICE_OBJECTIVE = 'DW1000')
;
```

### <a name="rest-apis"></a>REST API'leri

Dwu değiştirmek için [oluşturma veya güncelleştirme veritabanı] kullanma [oluşturma veya güncelleştirme veritabanı] REST API. Aşağıdaki örnek, MyServer sunucusunda barındırılan MySQLDW veritabanı için DW1000 için hizmet düzeyi hedefi ayarlar. Sunucu ResourceGroup1 adlı bir Azure kaynak grubunda yer alıyor.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```


## <a name="check-status-of-dwu-changes"></a>DWU değişiklikleri durumunu denetleme

DWU değişiklikleri tamamlanması birkaç dakika sürebilir. Otomatik ölçeklendirme, belirli işlemleri başka bir eylem işlemine devam etmeden önce tamamlandığından emin olmak için mantığı uygulayın. 

Çeşitli uç noktaları aracılığıyla veritabanı durumunu denetleme, doğru Otomasyon uygulamaya olanak tanır. Portal bir işlem ve veritabanları geçerli durumunun tamamlandıktan sonra bildirim sağlar ancak programlı durumunu denetlemek için izin vermiyor. 

Azure portal ile ölçek genişletme işlemleri için veritabanı durumunu denetleyemezsiniz.

DWU değişiklikleri durumunu denetlemek için:

1. Mantıksal SQL veritabanı sunucunuzla ilişkili ana veritabanına bağlanın.
2. Veritabanı durumunu denetlemek için aşağıdaki sorguyu gönderin.


```sql
SELECT    *
FROM      sys.databases
;
```

3. İşlemin durumunu denetlemek için aşağıdaki sorguyu Gönder

```sql
SELECT    *
FROM      sys.dm_operation_status
WHERE     resource_type_desc = 'Database'
AND       major_resource_id = 'MySQLDW'
;
```

Bu DMV çeşitli yönetimi hakkında bilgi işlem ve da işlemin durumunu gibi SQL veri ambarı işlemleri IN_PROGRESS olması döndürür veya tamamlandı.

## <a name="the-scaling-workflow"></a>Ölçeklendirme iş akışı

Bir ölçeklendirme işlemi başlattığınızda sistem önce açık olan tüm oturumları, tutarlı bir duruma emin olmak için açık işlemler geri alma devre dışı bırakır. Bu işlem geri alma işlemi tamamlandıktan sonra ölçeklendirme ölçek işlemleri için yalnızca oluşur.  

- Bir ölçek büyütme işleminin sistem hükümleri ek işlem ve depolama katmanı reattaches. 
- Gereksiz ölçek azaltma işlemi için düğümleri depolama biriminden ayırmak ve geri kalan düğümlerde yeniden ekleyin.

## <a name="next-steps"></a>Sonraki adımlar
Bazı ek anahtar performans kavramlarını anlamanıza yardımcı olması için aşağıdaki makalelere bakın:

* [İş yükü ve eşzamanlılık Yönetimi][Workload and concurrency management]
* [Tablo Tasarım genel bakış][Table design overview]
* [Tablo dağıtım][Table distribution]
* [Tablo dizin oluşturma][Table indexing]
* [Tablo bölümleme][Table partitioning]
* [Tablo istatistikleri][Table statistics]
* [En iyi uygulamalar][Best practices]

<!--Image reference-->

<!--Article references-->

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md


[Check database state with T-SQL]: ./sql-data-warehouse-manage-compute-tsql.md#check-database-state-and-operation-progress
[Check database state with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#check-database-state
[Check database state with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#check-database-state

[Workload and concurrency management]: ./resource-classes-for-workload-management.md
[Table design overview]: ./sql-data-warehouse-tables-overview.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Table indexing]: ./sql-data-warehouse-tables-index.md
[Table partitioning]: ./sql-data-warehouse-tables-partition.md
[Table statistics]: ./sql-data-warehouse-tables-statistics.md
[Best practices]: ./sql-data-warehouse-best-practices.md
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL DB Contributor]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
