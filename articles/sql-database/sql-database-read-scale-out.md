---
title: Azure SQL veritabanı sorguları çoğaltmalarındaki okuma - | Microsoft Docs
description: Azure SQL veritabanı kullanarak okuma ölçeği genişletme adlı salt okunur çoğaltmalar - kapasitesini dengelemek salt okunur iş yüklerini yükleme olanağı sağlar.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: sashan
ms.openlocfilehash: d29886b5c8693e4465053c8816fc38376a51fafc
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363609"
---
# <a name="use-read-only-replicas-to-load-balance-read-only-query-workloads-preview"></a>Yük Dengeleme (Önizleme) salt okunur sorgu iş yükleri için salt okunur çoğaltmalar kullanın

**Okuma ölçeği genişletme** , salt okunur çoğaltmalar kapasitesini kullanarak Azure SQL veritabanı salt okunur dengelemeye sağlar. 

## <a name="overview-of-read-scale-out"></a>Okuma ölçeği genişletme genel bakış

Her bir veritabanında Premium katman ([DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md)) veya iş açısından kritik katmanında ([sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md)) için birden fazla AlwaysON kopyaları olan otomatik olarak sağlandı Kullanılabilirlik SLA'sı destekler.

![Salt okunur çoğaltmalar](media/sql-database-managed-instance/business-critical-service-tier.png)

Bu çoğaltmaların normal veritabanı bağlantıları tarafından kullanılan okuma-yazma çoğaltma olarak aynı işlem boyutu ile sağlanır. **Okuma ölçeği genişletme** özelliği okuma-yazma çoğaltma paylaşmak yerine kapasitesi salt okunur çoğaltmalarından birini kullanarak SQL veritabanı salt okunur dengelemeye yüklemenizi sağlar. Bu şekilde salt okunur iş yükü ana okuma-yazma iş yükünden yalıtılmış ve performansını etkilemez. Özellik içeren mantıksal uygulamalar, salt okunur gibi iş yükleri, analiz, ayrılmış ve bu nedenle bu ek kapasite olmaksızın kullanarak performans avantajlarının geçirebilir öngörülmüştür ek bir maliyet.

Okuma ölçeği genişletme özelliği ile belirli bir veritabanını kullanmak için açık bir şekilde veritabanı oluşturulurken veya daha sonra çağırarak PowerShell kullanarak yapılandırmasını değiştirmeyi etkinleştirmeniz gerekir [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) veya [ Yeni-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlet'leri veya Azure Resource Manager REST API aracılığıyla [veritabanları - oluşturma veya güncelleştirme](/rest/api/sql/databases/createorupdate) yöntemi. 

Bir veritabanı için okuma ölçeği genişletme etkinleştirildikten sonra o veritabanına bağlanan uygulamalar okuma-yazma çoğaltmaya veya salt okunur bir çoğaltmaya göre veritabanının yönlendirilirsiniz `ApplicationIntent` uygulamanın yapılandırılmış özelliği bağlantı dizesi. Hakkında bilgi için `ApplicationIntent` özelliğine bakın [belirterek uygulama amacı](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent).

Okuma ölçeği genişletme devre dışı bırakıldı veya bir desteklenmeyen hizmet katmanında ReadScale özelliğini ayarlamanız, okuma / yazma kopyasına, bağımsız olarak tüm bağlantıları yönlendirilmiş `ApplicationIntent` özelliği.

> [!NOTE]
> Önizleme sırasında sorgu Data Store ve genişletilmiş olaylar üzerinde salt okunur çoğaltmaların desteklenmez.

## <a name="data-consistency"></a>Veri tutarlılığı

Always ON avantajlarından biri, çoğaltmaların her zaman işlemsel olarak tutarlı bir durumda olduğundan, ancak farklı zaman noktalarında olabilir bazı küçük gecikme süresi arasında farklı kopyaya değildir. Oturum düzeyinde tutarlılık okuma ölçeği genişletme destekler. Geldiğini, salt okunur oturumu sonra bir bağlantı hatası nedeniyle çoğaltma kullanılamazlık tarafından bağlanırsa, % 100 okuma-yazma çoğaltma ile güncel değil bir çoğaltmaya yönlendirilebilir. Benzer şekilde, bir uygulama bir okuma-yazma oturumu kullanarak verileri yazar ve hemen bir salt okunur oturumu kullanarak okur, olası en son güncelleştirmeleri hemen görünmez. İşlem günlüğü Yinele çoğaltmaları için zaman uyumsuz olmasıdır.

> [!NOTE]
> Bölge içinde çoğaltma gecikmeleri düşüktür ve bu nadir bir durumdur.


## <a name="connecting-to-a-read-only-replica"></a>Salt okunur kopyaya bağlanma

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

Aşağıdaki sorguyu çalıştırarak salt okunur bir kopyasına bağlanır olup olmadığını doğrulayabilirsiniz. Bu salt okunur kopyaya bağlıyken READ_ONLY döndürür.


```SQL
SELECT DATABASEPROPERTYEX(DB_NAME(), 'Updateability')
```
> [!NOTE]
> Belirli bir zamanda tek AlwaysON çoğaltmalarının salt okunur oturumları tarafından erişilebilir.

## <a name="enable-and-disable-read-scale-out-using-azure-powershell"></a>Enable ve disable okuma Azure PowerShell kullanarak ölçeklendirme

Azure PowerShell'de okuma ölçeği genişletilmiş yönetmek, aralık 2016 gerektirir Azure PowerShell sürümü veya daha yeni. En yeni PowerShell sürümüyle bkz [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

Etkinleştirmek veya Azure PowerShell okuma genişleme çağırarak devre dışı [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) cmdlet'i ve istenen değer: geçen `Enabled` veya `Disabled` --için `-ReadScale` parametresi. Alternatif olarak, kullanabilir [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) ile yeni bir veritabanı oluşturmak için cmdlet Genişleme etkin okuyun.

Mevcut bir veritabanı için (açılı ayraçlar içindeki öğeler, ortamınız için doğru değerlerle değiştirerek ve açılı ayraçlar bırakarak) gibi etkinleştirmek için ölçek genişletme okuyun:

```powershell
Set-AzureRmSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled
```

Okuma ölçeği genişletme (açılı ayraçlar içindeki öğeler, ortamınız için doğru değerlerle değiştirerek ve açılı ayraçlar bırakarak) mevcut bir veritabanı için devre dışı bırakmak için:

```powershell
Set-AzureRmSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Disabled
```

Okuma ölçeği genişletme ile yeni bir veritabanı oluşturmak için (açılı ayraçlar içindeki öğeler, ortamınız için doğru değerlerle değiştirerek ve açılı ayraçlar bırakarak) etkin:

```powershell
New-AzureRmSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled -Edition Premium
```

## <a name="enabling-and-disabling-read-scale-out-using-the-azure-sql-database-rest-api"></a>Azure SQL veritabanı REST API'sini kullanarak okuma genişleme devre dışı bırakma ve etkinleştirme

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

Daha fazla bilgi için [veritabanları - oluşturma veya güncelleştirme](/rest/api/sql/databases/createorupdate).

## <a name="using-read-scale-out-with-geo-replicated-databases"></a>Coğrafi olarak çoğaltılmış veritabanlarıyla okuma ölçeği genişletme kullanma

Yük Dengeleme salt okunur iş yükleri coğrafi olarak yedekli (örneğin bir bir yük devretme grubunun üyesi olarak) bir veritabanı üzerinde okuma ölçeği genişletme kullanıyorsanız, bu salt okunur genişleme hem birincil hem de coğrafi olarak çoğaltılmış ikincil veritabanlarıyla etkin olduğundan emin olun. Uygulamanızın yük devretmeden sonra yeni birincil siteye bağlandığında, bu yük dengeleyici aynı etkiyi garanti eder. Coğrafi çoğaltmalı ikincil veritabanına okuma-etkin, Ölçek ile bağlanıyorsanız, oturumları `ApplicationIntent=ReadOnly` çoğaltmalarından birini birincil veritabanı üzerinde şu yol bağlantıları aynı şekilde yönlendirilmez.  Oturumlarının olmadan `ApplicationIntent=ReadOnly` aynı zamanda salt okunur olan, coğrafi çoğaltmalı ikincil birincil çoğaltmaya yönlendirilir. Coğrafi çoğaltmalı ikincil veritabanının birincil veritabanı farklı bir uç nokta olduğundan, daha önce ikincil erişmek için bunu ayarlamak için gerekli değildi `ApplicationIntent=ReadOnly`. Geriye dönük uyumluluk sağlamak için `sys.geo_replication_links` DMV gösterir `secondary_allow_connections=2` (tüm istemci bağlantıya izin verilir).

> [!NOTE]
> Önizleme, hepsini bir kez deneme veya başka bir yükleme sırasında yerel çoğaltmalarının ikincil veritabanı arasında dengeli yönlendirme desteklenmiyor. 


## <a name="next-steps"></a>Sonraki adımlar

- Okuma ölçeği genişletme ayarlamak için PowerShell kullanma hakkında daha fazla bilgi için bkz: [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) veya [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlet'leri.
- Okuma ölçeği genişletme ayarlamak için REST API kullanma hakkında daha fazla bilgi için bkz: [veritabanları - oluşturma veya güncelleştirme](/rest/api/sql/databases/createorupdate).
