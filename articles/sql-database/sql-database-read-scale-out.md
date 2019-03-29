---
title: Azure SQL veritabanı sorguları çoğaltmalarındaki okuma - | Microsoft Docs
description: Azure SQL veritabanı kullanarak okuma ölçeği genişletme adlı salt okunur çoğaltmalar - kapasitesini dengelemek salt okunur iş yüklerini yükleme olanağı sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 03/28/2019
ms.openlocfilehash: d9ad859ef24b51dc337dc23281d2fe4e1eada1e6
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58619900"
---
# <a name="use-read-only-replicas-to-load-balance-read-only-query-workloads"></a>Yük Dengeleme salt okunur sorgu iş yükleri için salt okunur çoğaltmalar kullanın

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

**Okuma ölçeği genişletme** , bir salt okunur çoğaltma kapasitesini kullanarak Azure SQL veritabanı salt okunur dengelemeye sağlar.

Her bir veritabanında Premium katman ([DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md)) veya iş açısından kritik katmanında ([sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md)) için birden fazla AlwaysON kopyaları olan otomatik olarak sağlandı Kullanılabilirlik SLA'sı destekler. Bu işlem tarafından Aşağıdaki diyagramda gösterilmiştir.

![Salt okunur çoğaltmalar](media/sql-database-read-scale-out/business-critical-service-tier-read-scale-out.png)

İkincil çoğaltmaları birincil çoğaltması olarak aynı işlem boyutu ile sağlanır. **Okuma ölçeği genişletme** özelliği okuma-yazma çoğaltma paylaşmak yerine kapasitesi salt okunur çoğaltmalarından birini kullanarak SQL veritabanı salt okunur dengelemeye yüklemenizi sağlar. Bu şekilde salt okunur iş yükü ana okuma-yazma iş yükünden yalıtılmış ve performansını etkilemez. Özellik içeren mantıksal uygulamalar, salt okunur gibi iş yükleri, analiz, ayrılmış ve bu nedenle bu ek kapasite olmaksızın kullanarak performans avantajlarının geçirebilir öngörülmüştür ek bir maliyet.

Okuma ölçeği genişletme özelliği ile belirli bir veritabanını kullanmak için açık bir şekilde veritabanı oluşturulurken veya daha sonra çağırarak PowerShell kullanarak yapılandırmasını değiştirmeyi etkinleştirmeniz gerekir [kümesi AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) veya [Yeni AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) cmdlet'leri veya Azure Resource Manager REST API aracılığıyla [veritabanları - oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/sql/databases/createorupdate) yöntemi.

Okuma-yazma çoğaltmaya veya salt okunur bir çoğaltmaya göre veritabanının ağ geçidi tarafından bir veritabanı için okuma ölçeği genişletme etkinleştirildikten sonra o veritabanına bağlanan uygulamalar yönlendirilirsiniz `ApplicationIntent` yapılandırılan özelliği uygulamanın bağlantı dizesi. Hakkında bilgi için `ApplicationIntent` özelliğine bakın [belirterek uygulama amacı](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent).

Okuma ölçeği genişletme devre dışı bırakıldı veya bir desteklenmeyen hizmet katmanında ReadScale özelliğini ayarlamanız, okuma / yazma kopyasına, bağımsız olarak tüm bağlantıları yönlendirilmiş `ApplicationIntent` özelliği.

> [!NOTE]
> Sorgu veri Store, genişletilmiş olaylar, SQL Profiler ve denetim özellikleri salt okunur çoğaltmalarda desteklenmiyor. 
## <a name="data-consistency"></a>Veri tutarlılığı

Çoğaltmaların her zaman işlemsel olarak tutarlı bir durumda olduğundan, ancak farklı zaman noktalarında olabilir bazı küçük gecikme süresi arasında farklı kopyaya çoğaltmaları avantajlarından biridir. Oturum düzeyinde tutarlılık okuma ölçeği genişletme destekler. Geldiğini, salt okunur oturumu sonra bir bağlantı hatası nedeniyle çoğaltma kullanılamazlık tarafından bağlanırsa, % 100 okuma-yazma çoğaltma ile güncel değil bir çoğaltmaya yönlendirilebilir. Benzer şekilde, uygulamanın bir okuma-yazma oturumu kullanarak verileri yazar ve hemen bir salt okunur oturumu kullanarak okur, olası en son güncelleştirmeleri çoğaltmaya hemen görünür değildir. Gecikme süresi, bir zaman uyumsuz işlem günlüğü Yinele işlemi tarafından neden olur.

> [!NOTE]
> Bölge içinde çoğaltma gecikmeleri düşüktür ve bu nadir bir durumdur.

## <a name="connect-to-a-read-only-replica"></a>Salt okunur kopyaya bağlanın

Bir veritabanı için okuma ölçeği genişletme etkinleştirdiğinizde `ApplicationIntent` istemci tarafından sağlanan bağlantı dizesi seçeneğinde belirleyen bağlantı yazma çoğaltmaya veya salt okunur bir çoğaltmaya yönlendirilir. Özellikle, `ApplicationIntent` değer `ReadWrite` (varsayılan değer), bağlantı veritabanının okuma / yazma kopyasına yönlendirilirsiniz. Bu, var olan davranışı için aynıdır. Varsa `ApplicationIntent` değer `ReadOnly`, bağlantı salt okunur bir çoğaltmaya yönlendirilir.

Örneğin, aşağıdaki bağlantı dizesi (açılı ayraçlar içindeki öğeler, ortamınız için doğru değerlerle değiştirerek ve açılı ayraçlar bırakarak) salt okunur bir çoğaltması için istemci bağlanır:

```SQL
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

Aşağıdaki bağlantı dizelerinden biri (açılı ayraçlar içindeki öğeler, ortamınız için doğru değerlerle değiştirerek ve açılı ayraçlar bırakarak) bir okuma-yazma çoğaltmaya istemci bağlanır:

```SQL
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadWrite;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;

Server=tcp:<server>.database.windows.net;Database=<mydatabase>;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

## <a name="verify-that-a-connection-is-to-a-read-only-replica"></a>Salt okunur bir çoğaltması için bir bağlantı olduğundan emin olun

Aşağıdaki sorguyu çalıştırarak salt okunur bir kopyasına bağlanır olup olmadığını doğrulayabilirsiniz. Bu salt okunur kopyaya bağlıyken READ_ONLY döndürür.

```SQL
SELECT DATABASEPROPERTYEX(DB_NAME(), 'Updateability')
```

> [!NOTE]
> Belirli bir zamanda tek AlwaysON çoğaltmalarının salt okunur oturumları tarafından erişilebilir.

## <a name="monitoring-and-troubleshooting-read-only-replica"></a>İzleme ve salt okunur çoğaltma sorunlarını giderme

Salt okunur kopyaya bağlandığınızda, performans ölçümleri kullanarak erişebileceğiniz `sys.dm_db_resource_stats` DMV. Sorgu planı istatistikleri erişmek için `sys.dm_exec_query_stats`, `sys.dm_exec_query_plan` ve `sys.dm_exec_sql_text` Dmv'leri.

> [!NOTE]
> DMV `sys.resource_stats` mantıksal ana veritabanında CPU kullanımı ve depolama, birincil çoğaltma döndürür.


## <a name="enable-and-disable-read-scale-out"></a>Enable ve disable okuma ölçeği genişletme

Okuma ölçeği genişletme, varsayılan olarak etkindir [yönetilen örneği](sql-database-managed-instance.md) iş açısından kritik katmanı. İçinde açıkça etkinleştirilmelidir [veritabanı, SQL veritabanı sunucusuna yerleştirilen](sql-database-servers.md) Premium ve iş açısından kritik katmanları. Okuma ölçeği genişletme devre dışı bırakma ve etkinleştirme yöntemlerini burada açıklanmıştır.

### <a name="powershell-enable-and-disable-read-scale-out"></a>PowerShell: Enable ve disable okuma ölçeği genişletme

Azure PowerShell'de okuma ölçeği genişletilmiş yönetmek, aralık 2016 gerektirir Azure PowerShell sürümü veya daha yeni. En yeni PowerShell sürümüyle bkz [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

Etkinleştirmek veya Azure PowerShell okuma genişleme çağırarak devre dışı [kümesi AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) cmdlet'i ve istenen değer: geçen `Enabled` veya `Disabled` --için `-ReadScale` parametresi. Alternatif olarak, kullanabilir [yeni AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) ile yeni bir veritabanı oluşturmak için cmdlet Genişleme etkin okuyun.

Mevcut bir veritabanı için (açılı ayraçlar içindeki öğeler, ortamınız için doğru değerlerle değiştirerek ve açılı ayraçlar bırakarak) gibi etkinleştirmek için ölçek genişletme okuyun:

```powershell
Set-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled
```

Okuma ölçeği genişletme (açılı ayraçlar içindeki öğeler, ortamınız için doğru değerlerle değiştirerek ve açılı ayraçlar bırakarak) mevcut bir veritabanı için devre dışı bırakmak için:

```powershell
Set-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Disabled
```

Okuma ölçeği genişletme ile yeni bir veritabanı oluşturmak için (açılı ayraçlar içindeki öğeler, ortamınız için doğru değerlerle değiştirerek ve açılı ayraçlar bırakarak) etkin:

```powershell
New-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled -Edition Premium
```

### <a name="rest-api-enable-and-disable-read-scale-out"></a>REST API: Enable ve disable okuma ölçeği genişletme

Okuma etkin ölçeklendirme ile veritabanı oluşturma veya etkinleştirmek veya mevcut bir veritabanı için okuma genişleme devre dışı bırakmak için oluşturma veya karşılık gelen veritabanı varlığı ile güncelleştirme `readScale` özelliğini `Enabled` veya `Disabled` olarak örnek aşağıda İstek.

```rest
Method: PUT
URL: https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{GroupName}/providers/Microsoft.Sql/servers/{ServerName}/databases/{DatabaseName}?api-version= 2014-04-01-preview
Body:
{
   "properties":
   {
      "readScale":"Enabled"
   }
}
```

Daha fazla bilgi için [veritabanları - oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/sql/databases/createorupdate).

## <a name="using-tempdb-on-read-only-replica"></a>TempDB üzerinde salt okunur çoğaltma kullanma

TempDB veritabanı salt okunur kopyaya çoğaltılmaz. Her yineleme kendi yineleme oluşturulduğu sırada oluşturduğunuz TempDB veritabanı sürümüne sahiptir. TempDB güncelleştirilebilir ve, sorgu yürütme işlemi sırasında değiştirilebilir sağlar. Salt okunur iş yükünüz TempDB nesneleri kullanarak bağlıysa, sorgu betiğinizin bir parçası olarak bu nesneleri oluşturmanız gerekir. 

## <a name="using-read-scale-out-with-geo-replicated-databases"></a>Coğrafi olarak çoğaltılmış veritabanlarıyla okuma ölçeği genişletme kullanma

Yük Dengeleme salt okunur iş yükleri coğrafi olarak yedekli (örneğin, bir bir yük devretme grubunun üyesi olarak) bir veritabanı üzerinde okuma ölçeği genişletme kullanıyorsanız, bu salt okunur genişleme hem birincil hem de coğrafi olarak çoğaltılmış ikincil veritabanlarıyla etkin olduğundan emin olun. Bu yapılandırma, uygulamanızın yük devretmeden sonra yeni birincil siteye bağlandığında, aynı yük dengeleyici deneyimini devam etmesini sağlar. Coğrafi çoğaltmalı ikincil veritabanına okuma-etkin, Ölçek ile bağlanıyorsanız, oturumları `ApplicationIntent=ReadOnly` çoğaltmalarından birini birincil veritabanı üzerinde şu yol bağlantıları aynı şekilde yönlendirilmez.  Oturumlarının olmadan `ApplicationIntent=ReadOnly` aynı zamanda salt okunur olan, coğrafi çoğaltmalı ikincil birincil çoğaltmaya yönlendirilir. Coğrafi çoğaltmalı ikincil veritabanının birincil veritabanı farklı bir uç nokta olduğundan, daha önce ikincil erişmek için bunu ayarlamak için gerekli değildi `ApplicationIntent=ReadOnly`. Geriye dönük uyumluluk sağlamak için `sys.geo_replication_links` DMV gösterir `secondary_allow_connections=2` (tüm istemci bağlantıya izin verilir).

> [!NOTE]
> Yerel ikincil veritabanı çoğaltmalarını arasında yönlendirme hepsini veya diğer yük dengeli desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar

- Okuma ölçeği genişletme ayarlamak için PowerShell kullanma hakkında daha fazla bilgi için bkz: [kümesi AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) veya [yeni AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) cmdlet'leri.
- Okuma ölçeği genişletme ayarlamak için REST API kullanma hakkında daha fazla bilgi için bkz: [veritabanları - oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/sql/databases/createorupdate).
