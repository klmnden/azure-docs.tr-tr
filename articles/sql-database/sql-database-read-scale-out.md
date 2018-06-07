---
title: Azure SQL veritabanı sorguları çoğaltmalar üzerinde okuma - | Microsoft Docs
description: Azure SQL veritabanı salt okunur çoğaltmaları - okunur genişleme adlı kapasitesini kullanarak salt okunur dengelemeye yükleme yeteneği sağlar.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: sashan
ms.openlocfilehash: 8de70c01f4c04d6df85c2f5acfe9efe18ff59c0b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34649695"
---
# <a name="use-read-only-replicas-to-load-balance-read-only-query-workloads-preview"></a>Salt okunur çoğaltmaların salt okunur sorgu dengelemeye (Önizleme) yüklemek için kullanın

**Okuma genişleme** , salt okunur çoğaltmaları kapasitesini kullanarak Azure SQL veritabanı salt okunur dengelemeye yüklemenizi sağlar. 

## <a name="overview-of-read-scale-out"></a>Okuma genişleme genel bakış

Her veritabanı Premium katmanındaki ([DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md)) veya iş kritik katmanındaki ([vCore tabanlı satın alma modeli (Önizleme)](sql-database-service-tiers-vcore.md)) birkaç Always ON ile otomatik olarak sağlanır Kullanılabilirlik SLA'sı desteklemek için çoğaltmalar. Bu çoğaltmalar normal veritabanı bağlantılar tarafından kullanılan okuma-yazma çoğaltma aynı performans düzeyiyle sağlanır. **Okuma genişleme** özelliği okuma-yazma çoğaltma Paylaşımı yerine salt okunur çoğaltmaları kapasitesini kullanarak SQL veritabanı salt okunur dengelemeye yüklemenizi sağlar. Bu şekilde salt okunur iş yükü ana okuma-yazma yükünden yalıtılması ve kendi performansını etkilemez. Mantıksal olarak içeren uygulamaları salt okunur gibi iş yükleri analytics, ayrılmış ve bu ek kapasite Hayır kullanarak performans avantajı dolayısıyla geçirebilir özellik öngörülmüştür ekstra maliyet.

Belirli bir veritabanıyla okuma genişleme özelliği kullanmak için açık bir şekilde veritabanı oluşturulurken veya daha sonra çağırarak PowerShell kullanarak yapılandırmasını değiştirmeyi etkinleştirmeniz gerekir [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) veya [ AzureRmSqlDatabase yeni](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlet'leri kullanarak Azure Resource Manager REST API'si aracılığıyla veya [veritabanları - oluştur veya Güncelleştir](/rest/api/sql/databases/createorupdate) yöntemi. 

Bir veritabanı için okuma genişleme etkinleştirildikten sonra o veritabanına bağlanan uygulamaları okuma-yazma çoğaltmaya veya salt okunur çoğaltmaya göre veritabanının yönlendirilirsiniz `ApplicationIntent` uygulamanın içinde yapılandırılmış özelliği bağlantı dizesi. Hakkında bilgi için `ApplicationIntent` özelliği, bkz: [belirtme uygulama amacı](https://docs.microsoft.com/sql/relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery#specifying-application-intent).

Okuma genişleme devre dışıysa veya desteklenmeyen hizmet katmanında ReadScale özelliği ayarlarsanız okuma-yazma çoğaltmaya, bağımsız olarak tüm bağlantılar yönlendirilir `ApplicationIntent` özelliği.

> [!NOTE]
> Önizleme sırasında sorgu veri deposu ve genişletilmiş olaylar salt okunur çoğaltmaları desteklenmez.

## <a name="data-consistency"></a>Veri tutarlılığı

Always ON yararları çoğaltmaların her zaman işlemsel olarak tutarlı bir durumdadır, ancak zaman içinde farklı noktalarda olabilir bazı küçük gecikme süresi arasında farklı çoğaltmaları biridir. Okuma genişleme oturum düzeyi tutarlılık destekler. Bu anlamına gelir, salt okunur oturum çoğaltma kullanılabilir olmamasından nedeni bağlantı hatası sonra bağlanırsa, % 100 okuma-yazma çoğaltma ile güncel olmayan bir çoğaltma yönlendirilebilir. Bir uygulama bir okuma-yazma oturumu kullanarak verileri yazar ve hemen bir salt okunur oturumu kullanarak okur, benzer şekilde, en son güncelleştirmeleri hemen görünmez mümkündür. İşlem günlüğü Yinele çoğaltmaları için zaman uyumsuz olmasıdır.

> [!NOTE]
> Çoğaltma gecikmeleri bölge içindeki düşüktür ve bu nadir bir durumdur.


## <a name="connecting-to-a-read-only-replica"></a>Salt okunur bir kopyasına bağlanma

Bir veritabanı için okuma genişleme etkinleştirdiğinizde `ApplicationIntent` istemci tarafından sağlanan bağlantı dizesi seçeneğinde belirleyen bağlantı yazma çoğaltmaya veya salt okunur bir yineleme yönlendirilir. Özellikle, `ApplicationIntent` değer `ReadWrite` (varsayılan değer) bağlantı veritabanının okuma-yazma çoğaltmaya yönlendirilir. Bu, var olan davranışı aynıdır. Varsa `ApplicationIntent` değer `ReadOnly`, okunabilir bir çoğaltma bağlantısı yönlendirilir.

Örneğin, aşağıdaki bağlantı dizesi (ortamınız için doğru değerlerle köşeli öğeleri değiştirmek ve köşeli bırakma) salt okunur bir çoğaltma için istemci bağlanır:

```SQL
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadOnly;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

Aşağıdaki bağlantı dizeleri ya da bir okuma-yazma çoğaltma (ortamınız için doğru değerlerle köşeli öğeleri değiştirmek ve köşeli bırakma) istemci bağlanır:

```SQL
Server=tcp:<server>.database.windows.net;Database=<mydatabase>;ApplicationIntent=ReadWrite;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;

Server=tcp:<server>.database.windows.net;Database=<mydatabase>;User ID=<myLogin>;Password=<myPassword>;Trusted_Connection=False; Encrypt=True;
```

Aşağıdaki sorguyu çalıştırarak salt okunur bir kopyasına bağlanan olup olmadığını doğrulayabilirsiniz. Salt okunur bir kopyasına bağlıyken READ_ONLY döndürür.

```SQL
SELECT DATABASEPROPERTYEX(DB_NAME(), 'Updateability')
```

## <a name="enable-and-disable-read-scale-out-using-azure-powershell"></a>Etkinleştirme ve Azure PowerShell kullanarak okunur genişleme devre dışı

Okuma genişleme Azure PowerShell'de yönetme gerektirir aralık 2016 Azure PowerShell sürüm veya daha yeni. En yeni PowerShell sürüm için bkz: [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

Etkinleştirmek veya Azure PowerShell'de okuma genişleme çağırarak devre dışı [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) cmdlet'i ve istenen değerindeki – geçen `Enabled` veya `Disabled` --için `-ReadScale` parametresi. Alternatif olarak, kullanabilir [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) ile yeni bir veritabanı oluşturmak için cmdlet'i Genişleme etkin okuyun.

Var olan veritabanı için (ortamınız için doğru değerlerle köşeli öğeleri değiştirmek ve köşeli bırakma) Örneğin, etkinleştirmek için genişletme okuyun:

```powershell
Set-AzureRmSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled
```

(Köşeli öğelerde ortamınız için doğru değerlerle değiştirerek ve köşeli bırakma) var olan bir veritabanı için okuma genişleme devre dışı bırakmak için:

```powershell
Set-AzureRmSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Disabled
```

Okuma genişletme ile yeni bir veritabanı oluşturmak için (ortamınız için doğru değerlerle köşeli öğeleri değiştirmek ve köşeli bırakma) etkin:

```powershell
New-AzureRmSqlDatabase -ResourceGroupName <myresourcegroup> -ServerName <myserver> -DatabaseName <mydatabase> -ReadScale Enabled -Edition Premium
```

## <a name="enabling-and-disabling-read-scale-out-using-the-azure-sql-database-rest-api"></a>Etkinleştirme ve Azure SQL Database REST API'sini kullanarak okunur genişleme devre dışı bırakma

Okuma etkinse, genişletme ile bir veritabanı oluşturmak veya etkinleştirmek veya mevcut bir veritabanı için okuma genişleme devre dışı bırakmak için oluşturma veya güncelleştirme karşılık gelen veritabanı varlıkla `readScale` özelliğini `Enabled` veya `Disabled` gibi örnek aşağıda İstek.

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

Daha fazla bilgi için bkz: [veritabanları - oluştur veya Güncelleştir](/rest/api/sql/databases/createorupdate).

## <a name="next-steps"></a>Sonraki adımlar

- Okuma genişleme ayarlamak için PowerShell kullanma hakkında daha fazla bilgi için bkz: [Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase) veya [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) cmdlet'leri.
- Okuma genişleme ayarlamak için REST API kullanma hakkında daha fazla bilgi için bkz: [veritabanları - oluştur veya Güncelleştir](/rest/api/sql/databases/createorupdate).
