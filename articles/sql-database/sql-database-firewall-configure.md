---
title: Azure SQL veritabanı ve veri ambarı güvenlik duvarı kuralları | Microsoft Docs
description: SQL veritabanı ve SQL veri ambarı güvenlik duvarı erişimini yönetmek ve SQL veritabanı için veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma için sunucu düzeyinde güvenlik duvarı kuralları ile yapılandırmayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
manager: craigg
ms.date: 10/15/2018
ms.openlocfilehash: 4f6c98533a2ab1289ca5f1da25c44fe1a77a983c
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49353674"
---
# <a name="azure-sql-database-and-sql-data-warehouse-firewall-rules"></a>Azure SQL veritabanı ve SQL veri ambarı güvenlik duvarı kuralları

Microsoft Azure [SQL veritabanı](sql-database-technical-overview.md) ve [SQL veri ambarı](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) Azure ve diğer Internet tabanlı uygulamalar için bir ilişkisel veritabanı hizmeti sağlar. Güvenlik duvarları, verilerinizin korunmasına yardımcı olmak üzere, hangi bilgisayarların izinli olduğunu belirtmenize kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.

> [!NOTE]
> Bu konu başlığı, Azure SQL sunucusunun yanı sıra Azure SQL sunucusu üzerinde oluşturulmuş olan SQL Veritabanı ve SQL Veri Ambarı veritabanları için de geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır.

## <a name="virtual-network-rules-as-alternatives-to-ip-rules"></a>Sanal ağ kuralları alternatif IP kuralları

IP kurallarının yanı sıra, güvenlik duvarı da yönetir *sanal ağ kuralları*. Sanal ağ kuralları, sanal ağ hizmet uç noktaları üzerinde temel alır. Sanal ağ kuralları, bazı durumlarda IP kurallara tercih edilebilir. Daha fazla bilgi için bkz. [sanal ağ hizmet uç noktaları ve Azure SQL veritabanı için kuralları](sql-database-vnet-service-endpoint-rule-overview.md).

## <a name="overview"></a>Genel Bakış

Başlangıçta, Azure SQL sunucunuza tüm Transact-SQL erişimleri güvenlik duvarı tarafından engellenir. Azure SQL sunucunuzu kullanmaya başlamak için Azure SQL sunucunuza erişiminizi etkinleştirecek olan bir veya daha fazla sunucu düzeyinde güvenlik duvarı kuralı belirtmeniz gerekir. İnternet’ten gelen hangi IP adresi aralıklarının izinli olduğuna ve Azure uygulamalarının Azure SQL sunucunuza bağlanmaya çalışıp çalışamayacaklarına ilişkin güvenlik duvarı kurallarını belirleyin.

Azure SQL sunucunuzdaki veritabanlarından yalnızca birine seçmeli olarak erişim vermek için, gerekli veritabanına yönelik bir veritabanı düzeyinde kural oluşturmanız gerekir. Veritabanı güvenlik duvarı kuralı için, sunucu düzeyinde güvenlik duvarı kuralında belirtilen IP adres aralığının dışında olan bir IP adres aralığı belirtin ve istemci IP adresinin veritabanı düzeyindeki kuralda belirtilen aralığa denk geldiğinden emin olun.

> [!IMPORTANT]
> SQL veri ambarı, yalnızca sunucu düzeyinde güvenlik duvarı kuralı destekler ve veritabanı düzeyinde güvenlik duvarı kurallarını desteklemez.

İnternet’ten ve Azure’dan gelen bağlantı denemeleri, Azure SQL sunucunuza veya SQL Veritabanına ulaşmadan önce aşağıdaki diyagramda gösterildiği gibi ilk olarak güvenlik duvarından geçmelidir:

   ![Güvenlik duvarı yapılandırmasını açıklayan diyagram.][1]

- **Sunucu düzeyinde güvenlik duvarı kuralları:**

  Bu kurallar, diğer bir deyişle, tüm aynı mantıksal sunucu içindeki veritabanlarına istemcilerin tüm Azure SQL sunucunuza erişmesini sağlar. Bu kurallar **ana** veritabanına depolanır. Sunucu düzeyinde güvenlik duvarı kuralları, portal ya da Transact-SQL deyimleri kullanılarak yapılandırılabilir. Azure portalı veya PowerShell kullanarak sunucu düzeyinde güvenlik duvarı kuralları oluşturmak için abonelik sahibi veya abonelik katkıda bulunanı olmanız gerekir. Transact-SQL kullanarak sunucu düzeyinde güvenlik duvarı kuralı oluşturmak için SQL Veritabanı örneğine sunucu düzeyi asıl oturum açma bilgileriyle veya Azure Active Directory yöneticisi olarak bağlanmanız gerekir (başka bir deyişle, sunucu düzeyi güvenlik duvarı kuralının önce Azure düzeyi izinlere sahip bir kullanıcı tarafından oluşturulması gerekir).

- **Veritabanı düzeyinde güvenlik duvarı kuralları:**

  Bu kurallar istemcilerin aynı mantıksal sunucu içindeki bazı (güvenli) veritabanlarına erişmesini sağlar. Her veritabanı için kurallar oluşturabilirsiniz (dahil olmak üzere **ana** veritabanı) ve bunlar tek veritabanlarına depolanır. Ana ve kullanıcı veritabanları için veritabanı düzeyinde güvenlik duvarı kuralları yalnızca oluşturulur ve yalnızca ilk sunucu düzeyinde Güvenlik Duvarı'nı yapılandırdıktan sonra ve Transact-SQL deyimleri kullanılarak yönetilir. Veritabanı düzeyinde güvenlik kuralı için, sunucu düzeyinde güvenlik duvarı kuralında belirtilen aralığın dışındaki bir IP adresi aralığını belirtirseniz, yalnızca veritabanı düzeyi aralığındaki IP adreslerine sahip istemciler veritabanına erişebilir. Bir veritabanı için en fazla 128 veritabanı düzeyinde güvenlik duvarı kuralınız olabilir. Veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma hakkında daha fazla bilgi için makale ve bkz örnek ilerleyen bölümlerinde bkz [sp_set_database_firewall_rule (Azure SQL veritabanı)](https://msdn.microsoft.com/library/dn270010.aspx).

### <a name="recommendation"></a>Öneri

Microsoft, mümkün olduğunda güvenliği artırmak ve veritabanınızı daha taşınabilir yapmak için veritabanı düzeyinde güvenlik duvarı kurallarının kullanılmasını önerir. Aynı erişim gereksinimlerine sahip birçok veritabanınız varsa ve her veritabanını ayrı ayrı yapılandırmaya zaman harcamak istemiyorsanız sunucu düzeyinde güvenlik duvarı kurallarını yöneticiler için kullanabilirsiniz.

> [!Important]
> Windows Azure SQL veritabanı en fazla 128 güvenlik duvarı kuralını destekler.
> [!Note]
> İş sürekliliği bağlamında taşınabilir veritabanları hakkında bilgi edinmek için bkz. [Olağanüstü durum kurtarma için kimlik doğrulama gereksinimleri](sql-database-geo-replication-security-config.md).

### <a name="connecting-from-the-internet"></a>İnternet'ten bağlanma

Bir bilgisayar İnternet'ten veritabanı sunucunuza bağlanmaya çalıştığında, güvenlik duvarı ilk olarak isteğin kaynak IP adresini, bağlantının istediği veritabanı için veritabanı düzeyi güvenlik duvarı kurallarına karşı denetler:

- İsteğin IP adresi veritabanı düzeyinde güvenlik duvarı kurallarında belirtilen aralıklardan biri içindeyse, kuralı içeren SQL Veritabanı’na bağlantı izni verilir.
- İsteğin IP adresi veritabanı düzeyinde güvenlik duvarı kuralında belirtilen aralıklardan biri içinde değilse, sunucu düzeyinde güvenlik duvarı kuralları denetlenir. İstek IP adresi sunucu düzeyinde güvenlik duvarı kurallarında belirtilen aralıklardan biri içindeyse, bağlantı izni verilir. Sunucu düzeyinde güvenlik duvarı kuralları, Azure SQL server üzerindeki tüm SQL veritabanları için geçerlidir.  
- İsteğin IP adresi veritabanı düzeyinde veya sunucu düzeyinde güvenlik duvarı kuralları hiçbirinde belirtilen aralıklar dahilinde değilse, bağlantı isteği başarısız olur.

> [!NOTE]
> Azure SQL Veritabanına yerel bilgisayarınızdan erişmek için, ağ ve yerel bilgisayarınız üzerindeki güvenlik duvarının TCP bağlantı noktası 1433 üzerinde giden iletişime izin verdiğinden emin olun.

### <a name="connecting-from-azure"></a>Azure'dan bağlanma

Azure’daki uygulamaların Azure SQL sunucunuza bağlanmasına izin verebilmeniz için Azure bağlantılarının etkinleştirilmesi gerekir. Azure’dan bir uygulama, veritabanı sunucunuza bağlanmayı denediğinizde güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Başlangıç ve bitiş adresi 0.0.0.0’a eşit olan bir güvenlik duvarı ayarı, bu bağlantılara izin verildiğini gösterir. Bağlantı denemesine izin verilmezse, istek Azure SQL Veritabanı sunucusuna ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.

## <a name="creating-and-managing-firewall-rules"></a>Oluşturma ve güvenlik duvarı kurallarını yönetme

İlk sunucu düzeyinde güvenlik duvarı ayarı kullanılarak oluşturulabilir. [Azure portalında](https://portal.azure.com/) veya program aracılığıyla kullanarak [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql), [Azure CLI](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create), veya [ REST API](https://docs.microsoft.com/rest/api/sql/firewallrules/firewallrules_createorupdate). Sunucu düzeyinde sonraki güvenlik duvarı kuralları ise bu yöntemler kullanılarak ve Transact-SQL aracılığıyla oluşturulup yönetilebilir.

> [!IMPORTANT]
> Veritabanı düzeyinde güvenlik duvarı kuralları yalnızca oluşturulabilir ve Transact-SQL kullanılarak yönetilebilir.

Performansı artırmak için sunucu düzeyinde güvenlik duvarı kuralları veritabanı düzeyinde geçici olarak önbelleğe alınır. Önbelleği yenilemek için bkz. [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx).

> [!TIP]
> Kullanabileceğiniz [SQL Database Auditing](sql-database-auditing.md) sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı değişiklikleri denetlemek için.

## <a name="manage-firewall-rules-using-the-azure-portal"></a>Azure portalını kullanarak güvenlik duvarı kurallarını yönetme

Azure portalında sunucu düzeyinde güvenlik duvarı kuralını ayarlamak için ya da Genel Bakış sayfasına, Azure SQL veritabanınızı veya genel bakış sayfası için Azure veritabanı mantıksal sunucusu için gidebilirsiniz.

> [!TIP]
> Bir öğretici için bkz. [Azure portalını kullanarak bir veritabanı oluşturma](sql-database-get-started-portal.md).

### <a name="from-database-overview-page"></a>Veritabanına genel bakış sayfasından

1. Veritabanı genel bakış sayfasında sunucu düzeyinde güvenlik duvarı kuralını ayarlamak için **sunucu güvenlik duvarını Ayarla** aşağıdaki görüntüde gösterildiği gibi araç çubuğundaki: **Güvenlik Duvarı ayarları** SQL veritabanı sunucusu için sayfa açılır.

      ![sunucu güvenlik duvarı kuralı](./media/sql-database-get-started-portal/server-firewall-rule.png)

2. Tıklayın **istemci IP'si Ekle** bilgisayarın IP adresini eklemek için araç çubuğunda kullanmakta olduğunuz ve ardından **Kaydet**. Geçerli IP adresiniz için bir sunucu düzeyi güvenlik duvarı kuralı oluşturulur.

      ![sunucu güvenlik duvarı kuralı ayarla](./media/sql-database-get-started-portal/server-firewall-rule-set.png)

### <a name="from-server-overview-page"></a>Sunucu genel bakış sayfasından

Sunucunuzun genel bakış sayfası açılır ve tam sunucu adını gösteren (gibi **mynewserver20170403.database.windows.net**) ve daha fazla yapılandırma seçenekleri sağlar.

1. Server genel bakış sayfasında sunucu düzeyinde kural ayarlamak için **Güvenlik Duvarı** sol menüdeki ayarlar altında:

2. Tıklayın **istemci IP'si Ekle** bilgisayarın IP adresini eklemek için araç çubuğunda kullanmakta olduğunuz ve ardından **Kaydet**. Geçerli IP adresiniz için bir sunucu düzeyi güvenlik duvarı kuralı oluşturulur.

## <a name="manage-firewall-rules-using-transact-sql"></a>Transact-SQL kullanarak güvenlik duvarı kurallarını yönetme

| Katalog Görünümü veya Saklı Yordam | Düzey | Açıklama |
| --- | --- | --- |
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx) |Sunucu |Sunucu düzeyinde geçerli güvenlik duvarı kurallarını gösterir |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) |Sunucu |Sunucu düzeyinde güvenlik duvarı kuralları oluşturur veya güncelleştirir |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx) |Sunucu |Sunucu düzeyinde güvenlik duvarı kurallarını kaldırır |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx) |Database |Veritabanı düzeyinde geçerli güvenlik duvarı kurallarını gösterir |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) |Database |Veritabanı düzeyinde güvenlik duvarı kuralları oluşturur veya güncelleştirir |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) |Veritabanları |Veritabanı düzeyinde güvenlik duvarı kurallarını kaldırır |

Aşağıdaki örnekler, mevcut kuralları gözden geçirin, bir IP adresi aralığı Contoso sunucusunda etkinleştirin ve bir güvenlik duvarı kuralını siler:

```sql
SELECT * FROM sys.firewall_rules ORDER BY name;
```

Daha sonra bir güvenlik duvarı kuralı ekleyin.

```sql
EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
   @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'
```

Sunucu düzeyindeki bir güvenlik duvarı kuralını silmek için sp_delete_firewall_rule saklı yordamını yürütün. Aşağıdaki örnekte, ContosoFirewallRule adlı kural silinmektedir:

```sql
EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
```

## <a name="manage-firewall-rules-using-azure-powershell"></a>Azure PowerShell kullanarak güvenlik duvarı kurallarını yönetme

| Cmdlet | Düzey | Açıklama |
| --- | --- | --- |
| [Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule) |Sunucu |Sunucu düzeyinde geçerli güvenlik duvarı kurallarını döndürür |
| [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) |Sunucu |Sunucu düzeyinde yeni bir güvenlik duvarı kuralı oluşturur |
| [Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule) |Sunucu |Sunucu düzeyinde mevcut güvenlik duvarı kuralının özelliklerini güncelleştirir |
| [Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule) |Sunucu |Sunucu düzeyinde güvenlik duvarı kurallarını kaldırır |

Aşağıdaki örnek, PowerShell kullanarak sunucu düzeyinde güvenlik duvarı kuralı ayarlar:

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
```

> [!TIP]
> PowerShell örneklerini Hızlı Başlangıç için bağlamında, [DB oluşturma - PowerShell](sql-database-powershell-samples.md) ve [tek veritabanı oluşturma ve PowerShell kullanarak bir güvenlik duvarı kuralı yapılandırma](scripts/sql-database-create-and-configure-database-powershell.md)

## <a name="manage-firewall-rules-using-azure-cli"></a>Azure CLI kullanarak güvenlik duvarı kurallarını yönetme

| Cmdlet | Düzey | Açıklama |
| --- | --- | --- |
|[az sql server güvenlik duvarı kuralı oluşturma](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create)|Sunucu|Sunucu güvenlik duvarı kuralı oluşturur.|
|[az sql server güvenlik duvarı kuralı listesi](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-list)|Sunucu|Bir sunucudaki güvenlik duvarı kurallarını listeler|
|[az sql server güvenlik duvarı-rule show](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-show)|Sunucu|Bir güvenlik duvarı kuralı ayrıntılarını gösterir|
|[az sql server güvenlik duvarı kuralı güncelleştirme](/cli/azure/sql/server/firewall-rule##az-sql-server-firewall-rule-update)|Sunucu|Bir güvenlik duvarı kuralını güncelleştirir|
|[az sql server güvenlik duvarı kuralını Sil](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-delete)|Sunucu|Bir güvenlik duvarı kuralını siler|

Aşağıdaki örnekte Azure CLI kullanarak sunucu düzeyinde güvenlik duvarı kuralı ayarlar:

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
-n AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP]
> Azure CLI örneği bir hızlı başlangıç için bağlamında, bkz: [DB oluşturma - Azure CLI](sql-database-cli-samples.md) ve [tek veritabanı oluşturma ve Azure CLI kullanarak bir güvenlik duvarı kuralı yapılandırma](scripts/sql-database-create-and-configure-database-cli.md)

## <a name="manage-firewall-rules-using-rest-api"></a>REST API kullanarak güvenlik duvarı kurallarını yönetme

| API | Düzey | Açıklama |
| --- | --- | --- |
| [Güvenlik Duvarı Kurallarını Listele](https://docs.microsoft.com/rest/api/sql/firewallrules/firewallrules_listbyserver) |Sunucu |Sunucu düzeyinde geçerli güvenlik duvarı kurallarını gösterir |
| [Güvenlik Duvarı Kuralı Oluşturma veya Güncelleştirme](https://docs.microsoft.com/rest/api/sql/firewallrules/firewallrules_createorupdate) |Sunucu |Sunucu düzeyinde güvenlik duvarı kuralları oluşturur veya güncelleştirir |
| [Güvenlik Duvarı Kuralını Sil](https://docs.microsoft.com/rest/api/sql/firewallrules/firewallrules_delete) |Sunucu |Sunucu düzeyinde güvenlik duvarı kurallarını kaldırır |
| [Güvenlik duvarı kurallarını Al](https://docs.microsoft.com/rest/api/sql/firewallrules/firewallrules_get) | Sunucu | Sunucu düzeyinde güvenlik duvarı kuralları alır |

## <a name="server-level-firewall-rule-versus-a-database-level-firewall-rule"></a>Sunucu düzeyinde güvenlik duvarı kuralı ve veritabanı düzeyinde güvenlik duvarı kuralı

S. Kullanıcılar bir veritabanının başka bir veritabanından tamamen yalıtılmış olmalıdır?
Yanıt Evet ise, veritabanı düzeyinde güvenlik duvarı kurallarını kullanarak erişim verin. Bu, savunmalarınıza derinliğini azaltarak, tüm veritabanları için güvenlik duvarı üzerinden erişime izin veren sunucu düzeyinde güvenlik duvarı kurallarını kullanarak önler.

S. Kullanıcı veya IP adresinin, tüm veritabanlarına erişim gerekiyor mu?
Güvenlik duvarı kuralları yapılandırmalısınız sayısını azaltmak için sunucu düzeyinde güvenlik duvarı kurallarını kullanın.

S. Kişi veya güvenlik duvarı kuralları yalnızca yapılandırma takım erişim Azure portalı, PowerShell veya REST API aracılığıyla var mı?
Sunucu düzeyinde güvenlik duvarı kurallarını kullanmanız gerekir. Veritabanı düzeyinde güvenlik duvarı kuralları yalnızca Transact-SQL kullanarak de yapılandırılabilir.  

S. Kişi veya güvenlik duvarı kuralları veritabanı düzeyinde üst düzey iznine sahip yasaklanmış yapılandırıyor?
Sunucu düzeyinde güvenlik duvarı kurallarını kullanın. Transact-SQL kullanarak veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma en azından `CONTROL DATABASE` veritabanı düzeyinde izin.  

S. Merkezi olarak birçok için güvenlik duvarı kurallarını yönetme kişi veya yapılandırma ya da güvenlik duvarı kuralları, Denetim takım olup (belki de 100'lük) veritabanlarının?
Bu seçim, gereksinimleri ve ortam bağlıdır. Sunucu düzeyinde güvenlik duvarı kurallarını yapılandırmak daha kolay olabilir, ancak komut dosyası kuralları veritabanı düzeyinde yapılandırabilirsiniz. Ve sunucu düzeyinde güvenlik duvarı kuralları kullansanız bile, olmadığını veritabanı-güvenlik duvarı kuralları, Denetim gerekebilir kullanıcılarla `CONTROL` izni veritabanında veritabanı düzeyinde güvenlik duvarı kuralları oluşturdunuz.

S. Her iki sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları bir karışımını kullanabilir miyim?
Evet. Bazı kullanıcılar, yöneticileri gibi sunucu düzeyi güvenlik duvarı kuralları gerekebilir. Bir veritabanı uygulama kullanıcıları gibi diğer kullanıcıların, veritabanı düzeyinde güvenlik duvarı kuralları gerekebilir.

## <a name="troubleshooting-the-database-firewall"></a>Veritabanı güvenlik duvarı sorunlarını giderme

Microsoft Azure SQL Veritabanı hizmetine erişim beklediğiniz gibi davranmadığında aşağıdaki noktaları göz önünde bulundurun:

- **Yerel güvenlik duvarı yapılandırması:**

  Bilgisayarınızın Azure SQL veritabanı erişmeden önce bilgisayarınızdaki 1433 numaralı TCP bağlantı noktası için bir güvenlik duvarı özel durumu oluşturmak gerekebilir. Azure bulut limitleri içerisinde bağlantı oluşturuyorsanız başka bağlantı noktalarını da açmanız gerekebilir. Daha fazla bilgi için **SQL veritabanı: dış ve iç karşılaştırması** bölümünü [ADO.NET 4.5 ve SQL veritabanı için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md).

- **Ağ adresi çevirisi (NAT):**

  NAT nedeniyle, bilgisayarınızın Azure SQL veritabanı'na bağlanmak için kullanılan IP adresini bilgisayar IP yapılandırma ayarlarında gösterilen IP adresiyle farklı olabilir. Bilgisayarınızın Azure’a bağlanmak için kullandığı IP adresini görüntülemek üzere portalda oturum açın ve veritabanınızı barındıran sunucudaki **Yapılandır** sekmesine gidin. **İzin Verilen IP Adresleri** bölümü altında **Geçerli İstemci IP Adresi** gösterilir. Bu bilgisayarın sunucuya erişmesine izin vermek için **İzin Verilen IP Adresleri** bölümünde **Ekle**’ye tıklayın.

- **İzin verilenler listesine değişiklikler henüz etkili olmamıştır:**

  Bir beş dakikaya kadar yapılan değişikliklerin etkili olması için Azure SQL veritabanı güvenlik duvarı yapılandırması olabilir.

- **Oturum açma yetkili değil veya yanlış parola kullanıldı:**

  Azure SQL veritabanı sunucusunda bir oturum açma izni yok veya kullanılan parola yanlışsa, Azure SQL veritabanı sunucusuyla bağlantı reddedilir. Bir güvenlik duvarı ayarının oluşturulması yalnızca istemcilere sunucunuzla bağlantı kurmayı deneme fırsatı sunar; her istemci gerekli güvenlik kimlik bilgilerini belirtmek zorundadır. Oturumları hazırlama hakkında daha fazla bilgi için bkz. [veritabanlarını yönetme, oturum açma bilgileri ve Azure SQL veritabanı'nda kullanıcıların](sql-database-manage-logins.md).

- **Dinamik IP adresi:**

  Dinamik IP adresiyle kurulmuş bir Internet bağlantısı varsa ve güvenlik duvarını aşmakta sorun yaşıyorsanız aşağıdaki çözümlerden birini deneyebilirsiniz:
  
  - Azure SQL Veritabanı sunucusuna erişen istemci bilgisayarlarınıza atanmış IP adresi aralığını İnternet Servis Sağlayıcınıza (ISS) sorun ve IP adresi aralığını güvenlik duvarı kuralı olarak ekleyin.
  - İstemci bilgisayarlarınız için bunun yerine statik IP adresi alın ve IP adreslerini güvenlik duvarı kuralları olarak ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

- Bir veritabanı ve sunucu düzeyinde güvenlik duvarı kuralı oluşturma Hızlı Başlangıç için bkz. [bir Azure SQL veritabanı oluşturma](sql-database-get-started-portal.md).
- Açık kaynak veya üçüncü taraf uygulamalardan bir Azure SQL veritabanına bağlanma konusunda yardım için bkz. [SQL Veritabanına yönelik istemci hızlı başlatma kod örnekleri](https://msdn.microsoft.com/library/azure/ee336282.aspx).
- Açmak için ihtiyacınız olan ek bağlantı noktaları hakkında daha fazla bilgi için bkz: **SQL veritabanı: dış ve iç karşılaştırması** bölümünü [ADO.NET 4.5 ve SQL veritabanı için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md)
- Azure SQL veritabanı güvenliğine genel bakış için bkz. [veritabanınızı güvenli hale getirme](sql-database-security-overview.md)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
