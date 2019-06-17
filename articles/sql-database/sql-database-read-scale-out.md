---
title: Azure SQL veritabanı sorguları çoğaltmalarındaki okuma - | Microsoft Docs
description: Azure SQL veritabanı, okuma ölçeği genişletme adlı salt okunur çoğaltmalar - kapasitesini kullanan salt okunur iş yükleri yük dengeleme olanağı sağlar.
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
ms.date: 06/03/2019
ms.openlocfilehash: 1b452fb0bac91429793f8d55e439c36c70784722
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66492715"
---
# <a name="use-read-only-replicas-to-load-balance-read-only-query-workloads"></a>Salt okunur çoğaltmalar Yük Dengeleme salt okunur sorgu iş yükleri için kullanın

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bir parçası olarak [yüksek kullanılabilirlik mimarisi](./sql-database-high-availability.md#premium-and-business-critical-service-tier-availability), her bir veritabanı Premium, iş açısından kritik veya hiper ölçekli hizmet katmanında birden fazla ikincil çoğaltma birincil çoğaltma ile otomatik olarak sağlanır. İkincil çoğaltmaları birincil çoğaltması olarak aynı işlem boyutu ile sağlanır. **Okuma ölçeği genişletme** özelliği okuma-yazma çoğaltma paylaşmak yerine kapasitesi salt okunur çoğaltmalarından birini kullanarak yük dengelemeyle SQL veritabanı salt okunur iş yüklerini, sağlar. Bu şekilde salt okunur iş yükü ana okuma-yazma iş yükünden yalıtılmış ve performansını etkilemez. Özelliği, mantıksal olarak ayrı salt okunur gibi iş yüklerini analiz içeren uygulamalar için tasarlanmıştır. Bu ek kapasite olmaksızın kullanarak performans avantajı elde edebilir ek bir maliyet.

Aşağıdaki diyagramda, iş açısından kritik veritabanı kullanma gösterilmektedir.

![Salt okunur çoğaltmalar](media/sql-database-read-scale-out/business-critical-service-tier-read-scale-out.png)

Okuma ölçeklendirme özelliği, yeni Premium, iş açısından kritik ve hiper ölçekli veritabanları varsayılan olarak etkindir. SQL bağlantı dizesi ile yapılandırılmışsa `ApplicationIntent=ReadOnly`, uygulama o veritabanının salt okunur bir çoğaltması için ağ geçidi tarafından yönlendirilir. Nasıl kullanılacağı hakkında daha fazla bilgi için `ApplicationIntent` özelliğine bakın [belirterek uygulama amacı](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent).

Uygulama birincil çoğaltmaya bakılmaksızın bağlandığını sağlamak istiyorsanız `ApplicationIntent` SQL bağlantı dizesinde ayarı, açıkça okuma ölçeği Genişletilmiş veritabanı oluşturulurken veya yapılandırmasını değiştirmeyi devre dışı bırakmanız gerekir. Örneğin, veritabanı katmanından standart veya genel amaçlı Premium, iş açısından kritik veya hiper ölçekli katmanına yükseltin ve tüm bağlantıları birincil çoğaltmaya gitmeye devam edersiniz emin olmak için okuma ölçeği genişletme devre dışı bırakın. Bunu devre dışı bırakma hakkında ayrıntılı bilgi için bkz. [Enable ve disable okuma ölçeği genişletme](#enable-and-disable-read-scale-out).

> [!NOTE]
> Sorgu veri Store, genişletilmiş olaylar, SQL Profiler ve denetim özellikleri salt okunur çoğaltmalarda desteklenmiyor. 

## <a name="data-consistency"></a>Veri tutarlılığı

Çoğaltmaların her zaman işlemsel olarak tutarlı bir durumda olduğundan, ancak farklı zaman noktalarında olabilir bazı küçük gecikme süresi arasında farklı kopyaya çoğaltmaları avantajlarından biridir. Oturum düzeyinde tutarlılık okuma ölçeği genişletme destekler. Geldiğini, salt okunur oturumu sonra bir bağlantı hatası nedeniyle çoğaltma kullanılamazlık tarafından bağlanırsa, % 100 okuma-yazma çoğaltma ile güncel değil bir çoğaltmaya yönlendirilebilirsiniz. Benzer şekilde, uygulamanın bir okuma-yazma oturumu kullanarak verileri yazar ve hemen bir salt okunur oturumu kullanarak okur, olası en son güncelleştirmeleri çoğaltmaya hemen görünür değildir. Gecikme süresi, bir zaman uyumsuz işlem günlüğü Yinele işlemi tarafından neden olur.

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

Okuma ölçeği genişletme, Premium, iş açısından kritik ve hiper ölçekli hizmet katmanları üzerinde varsayılan olarak etkindir. Hizmet katmanları temel, standart veya genel amaçlı okuma ölçeği genişletme etkinleştirilemez. Okuma ölçeği genişletme, otomatik olarak 0 çoğaltma ile yapılandırılmış hiper ölçekli veritabanları devre dışıdır. 

Devre dışı bırakabilir ve okuma ölçeği genişletme tek veritabanları ve elastik havuz aşağıdaki yöntemleri kullanarak Premium veya iş açısından kritik hizmet katmanındaki veritabanları yeniden etkinleştirin.

> [!NOTE]
> Okuma ölçeği genişletme devre dışı bırakma olanağı, geriye dönük uyumluluk için sağlanır.

### <a name="azure-portal"></a>Azure portal

Okuma Sacle genişletme ayarını yönetebileceğiniz **yapılandırma** veritabanı dikey penceresi. 

### <a name="powershell"></a>PowerShell

Azure PowerShell'de okuma ölçeği genişletilmiş yönetmek, aralık 2016 gerektirir Azure PowerShell sürümü veya daha yeni. En yeni PowerShell sürümüyle bkz [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

Azure PowerShell'de okuma ölçeği genişletilmiş çağırarak yeniden etkinleştirmek veya devre dışı [kümesi AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) cmdlet'i ve istenen değer: geçen `Enabled` veya `Disabled` --için `-ReadScale` parametresi. 

Okuma ölçeği genişletme (açılı ayraçlar içindeki öğeler, ortamınız için doğru değerlerle değiştirerek ve açılı ayraçlar bırakarak) var olan bir veritabanında devre dışı bırakmak için:

```powershell
Set-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Disabled
```
(Ortamınız için doğru değerlerle açılı ayraçlar öğeleri değiştirmek ve açılı ayraçlar bırakarak) yeni bir veritabanı üzerinde okuma genişleme devre dışı bırakmak için:

```powershell
New-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Disabled -Edition Premium
```

Okuma ölçeği genişletme (açılı ayraçlar içindeki öğeler, ortamınız için doğru değerlerle değiştirerek ve açılı ayraçlar bırakarak) var olan bir veritabanında yeniden etkinleştirmek için:

```powershell
Set-AzSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled
```

### <a name="rest-api"></a>REST API

Okuma devre dışı ölçeklendirme ile veritabanı oluşturma veya mevcut bir veritabanı için ayarı değiştirmek için aşağıdaki yöntemi kullanın `readScale` özelliğini `Enabled` veya `Disabled` olarak örnek istek aşağıda.

```rest
Method: PUT
URL: https://management.azure.com/subscriptions/{SubscriptionId}/resourceGroups/{GroupName}/providers/Microsoft.Sql/servers/{ServerName}/databases/{DatabaseName}?api-version= 2014-04-01-preview
Body:
{
   "properties":
   {
      "readScale":"Disabled"
   }
}
```

Daha fazla bilgi için [veritabanları - oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/sql/databases/createorupdate).

## <a name="using-tempdb-on-read-only-replica"></a>TempDB üzerinde salt okunur çoğaltma kullanma

TempDB veritabanı salt okunur kopyaya çoğaltılmaz. Her yineleme kendi yineleme oluşturulduğu sırada oluşturduğunuz TempDB veritabanı sürümüne sahiptir. TempDB güncelleştirilebilir ve, sorgu yürütme işlemi sırasında değiştirilebilir sağlar. Salt okunur iş yükünüz TempDB nesneleri kullanarak bağlıysa, sorgu betiğinizin bir parçası olarak bu nesneleri oluşturmanız gerekir. 

## <a name="using-read-scale-out-with-geo-replicated-databases"></a>Coğrafi olarak çoğaltılmış veritabanlarıyla okuma ölçeği genişletme kullanma

Yük Dengeleme salt okunur iş yüklerini coğrafi olarak yedekli (örneğin, bir bir yük devretme grubunun üyesi olarak) bir veritabanı üzerinde okuma ölçeği genişletme kullanıyorsanız, bu salt okunur genişleme hem birincil hem de coğrafi olarak çoğaltılmış ikincil veritabanlarıyla etkin olduğundan emin olun. Bu yapılandırma, uygulamanızın yük devretmeden sonra yeni birincil siteye bağlandığında, aynı yük dengeleyici deneyimini devam etmesini sağlar. Coğrafi çoğaltmalı ikincil veritabanına okuma-etkin, Ölçek ile bağlanıyorsanız, oturumları `ApplicationIntent=ReadOnly` çoğaltmalarından birini birincil veritabanı üzerinde şu yol bağlantıları aynı şekilde yönlendirilmez.  Oturumlarının olmadan `ApplicationIntent=ReadOnly` aynı zamanda salt okunur olan, coğrafi çoğaltmalı ikincil birincil çoğaltmaya yönlendirilir. Coğrafi çoğaltmalı ikincil veritabanı birincil veritabanının farklı bir uç noktaya sahip olduğundan, daha önce ikincil erişmek için bunu ayarlamak için gerekli değildi `ApplicationIntent=ReadOnly`. Geriye dönük uyumluluk sağlamak için `sys.geo_replication_links` DMV gösterir `secondary_allow_connections=2` (tüm istemci bağlantıya izin verilir).

> [!NOTE]
> Hepsini bir kez deneme veya tüm diğer yük dengeli ikincil veritabanının yerel kopya yönlendirme desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar

- SQL veritabanı hiper ölçekli teklifi hakkında daha fazla bilgi için bkz. [hiper ölçekli hizmet katmanı](./sql-database-service-tier-hyperscale.md).
