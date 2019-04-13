---
title: Azure SQL veritabanı ve veri ambarı IP güvenlik duvarı kuralları | Microsoft Docs
description: Erişimini yönetmek ve tek veya havuza alınmış veritabanının veritabanı düzeyinde IP güvenlik duvarı kuralları ile yapılandırmak için sunucu düzeyinde IP güvenlik duvarı kuralları ile bir SQL veritabanı veya SQL veri ambarı güvenlik duvarı yapılandırmayı öğrenin.
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
ms.date: 03/12/2019
ms.openlocfilehash: 513836257a292069da709ad7a71e480f2b4d069d
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59549738"
---
# <a name="azure-sql-database-and-sql-data-warehouse-ip-firewall-rules"></a>Azure SQL veritabanı ve SQL veri ambarı IP güvenlik duvarı kuralları

Microsoft Azure [SQL veritabanı](sql-database-technical-overview.md) ve [SQL veri ambarı](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) Azure ve diğer Internet tabanlı uygulamalar için bir ilişkisel veritabanı hizmeti sağlar. Güvenlik duvarları, verilerinizin korunmasına yardımcı olmak üzere, hangi bilgisayarların izinli olduğunu belirtmenize kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.

> [!NOTE]
> Bu makale, Azure SQL server ve Azure SQL sunucusu üzerinde oluşturulmuş olan hem SQL veritabanı ve SQL veri ambarı veritabanları için geçerlidir. Kolaylık açısından, hem SQL Veritabanı hem de SQL Veri Ambarı için SQL Veritabanı terimi kullanılmaktadır.
> [!IMPORTANT]
> Bu makale *değil* uygulamak **Azure SQL veritabanı yönetilen örneği**. Üzerinde lütfen şu makaleye bakın [yönetilen örneğe bağlanma](sql-database-managed-instance-connect-app.md) gereken ağ yapılandırması hakkında daha fazla bilgi.

## <a name="virtual-network-rules-as-alternatives-to-ip-rules"></a>Sanal ağ kuralları alternatif IP kuralları

IP kurallarının yanı sıra, güvenlik duvarı da yönetir *sanal ağ kuralları*. Sanal ağ kuralları, sanal ağ hizmet uç noktaları üzerinde temel alır. Sanal ağ kuralları, bazı durumlarda IP kurallara tercih edilebilir. Daha fazla bilgi için bkz. [sanal ağ hizmet uç noktaları ve Azure SQL veritabanı için kuralları](sql-database-vnet-service-endpoint-rule-overview.md).

## <a name="overview"></a>Genel Bakış

Başlangıçta, Azure SQL sunucunuza tüm erişimi SQL veritabanı güvenlik duvarı tarafından engellenir. Bir veritabanı sunucusuna erişmek için Azure SQL sunucunuza erişiminizi etkinleştirecek olan bir veya daha fazla sunucu düzeyinde IP güvenlik duvarı kurallarını belirtmeniz gerekir. IP güvenlik duvarı kuralları, İnternet'ten gelen hangi IP adresi aralıklarının izinli olduğuna ve Azure uygulamalarının Azure SQL sunucunuza bağlanmak mi çalışamayacaklarına belirtmek için kullanın.

Azure SQL sunucunuzdaki veritabanlarından yalnızca birine seçmeli olarak erişim vermek için, gerekli veritabanına yönelik bir veritabanı düzeyinde kural oluşturmanız gerekir. Sunucu düzeyinde IP güvenlik duvarı kuralında belirtilen IP adresi aralığı dışındadır veritabanı IP güvenlik duvarı kuralı için bir IP adresi aralığı belirtin ve istemci IP adresi veritabanı düzeyindeki kuralda Belirtilen aralıktaki denk geldiğinden emin olun.

> [!IMPORTANT]
> SQL veri ambarı, yalnızca sunucu düzeyinde IP güvenlik duvarı kuralı destekler ve veritabanı düzeyinde IP güvenlik duvarı kurallarını desteklemez.

İnternet’ten ve Azure’dan gelen bağlantı denemeleri, Azure SQL sunucunuza veya SQL Veritabanına ulaşmadan önce aşağıdaki diyagramda gösterildiği gibi ilk olarak güvenlik duvarından geçmelidir:

   ![Güvenlik duvarı yapılandırmasını açıklayan diyagram.][1]

- **Sunucu düzeyinde IP güvenlik duvarı kuralları:**

  Bu kurallar, diğer bir deyişle, tüm veritabanlarının aynı SQL veritabanı sunucusu istemcilerin tüm Azure SQL sunucunuza erişmesini sağlar. Bu kurallar **ana** veritabanına depolanır. Portalı kullanarak veya Transact-SQL deyimlerini kullanarak sunucu düzeyinde IP güvenlik duvarı kuralları yapılandırılabilir. Azure portal veya PowerShell kullanarak sunucu düzeyinde IP güvenlik duvarı kuralları oluşturmak için abonelik sahibi veya abonelik katkıda bulunanı olmalıdır. Transact-SQL kullanarak sunucu düzeyinde IP güvenlik duvarı kuralı oluşturmak için SQL veritabanı örneğine sunucu düzeyi asıl oturum açma veya Azure Active Directory Yöneticisi olarak bağlanmanız gerekir (bir sunucu düzeyinde IP güvenlik duvarı kuralı tarafından oluşturulması gerekir yani bir Kullanıcı Azure düzeyi izinlere sahip).

- **IP güvenlik duvarı kuralları veritabanı düzeyinde:**

  Bu kurallar istemcilerin aynı SQL veritabanı sunucu içindeki bazı (güvenli) veritabanlarına erişmesini sağlar. Her veritabanı için kurallar oluşturabilirsiniz (dahil olmak üzere **ana** veritabanı) ve bunlar tek veritabanlarına depolanır. Ana ve kullanıcı veritabanları için veritabanı düzeyinde IP güvenlik duvarı kuralları yalnızca oluşturulur ve yalnızca ilk sunucu düzeyinde Güvenlik Duvarı'nı yapılandırdıktan sonra ve Transact-SQL deyimleri kullanılarak yönetilir. Sunucu düzeyinde IP güvenlik duvarı kuralında belirtilen aralığın dışında veritabanı düzeyinde IP güvenlik duvarı kuralında bir IP adresi aralığı belirtirseniz, yalnızca veritabanı düzeyi aralığındaki IP adreslerine sahip istemciler veritabanına erişebilir. En fazla 128 veritabanı düzeyinde IP güvenlik duvarı kuralları için bir veritabanı olabilir. Veritabanı düzeyinde IP güvenlik duvarı kurallarını yapılandırma hakkında daha fazla bilgi için makale ve bkz örnek ilerleyen bölümlerinde bkz [sp_set_database_firewall_rule (Azure SQL veritabanı)](https://msdn.microsoft.com/library/dn270010.aspx).

### <a name="recommendation"></a>Öneri

Microsoft, mümkün olduğunda güvenliği artırmak ve veritabanınızı daha taşınabilir yapmak için veritabanı düzeyinde IP güvenlik duvarı kurallarının kullanılmasını önerir. Sunucu düzeyinde IP güvenlik duvarı kurallarını Yöneticiler ve aynı erişim gereksinimlerine sahip birçok veritabanınız varsa ve her veritabanını ayrı ayrı yapılandırmaya zaman harcamak istemiyorsanız kullanın.

> [!IMPORTANT]
> Windows Azure SQL veritabanı en fazla 128 IP güvenlik duvarı kurallarını destekler.
> [!NOTE]
> İş sürekliliği bağlamında taşınabilir veritabanları hakkında bilgi edinmek için bkz. [Olağanüstü durum kurtarma için kimlik doğrulama gereksinimleri](sql-database-geo-replication-security-config.md).

### <a name="connecting-from-the-internet"></a>İnternet'ten bağlanma

Bir bilgisayar Internet'ten veritabanı sunucunuza bağlanmaya çalıştığında güvenlik duvarı ilk isteğin kaynak IP adresini bağlantının istediği veritabanı için veritabanı düzeyinde IP güvenlik duvarı kurallarını karşı isteğin denetler:

- İsteğin IP adresi veritabanı düzeyinde IP güvenlik duvarı kurallarında belirtilen aralıklardan biri içindeyse kuralı içeren SQL veritabanına bağlantı verilir.
- İsteğin IP adresi veritabanı düzeyinde IP güvenlik duvarı kuralında belirtilen aralıklardan biri içinde değilse, sunucu düzeyinde IP güvenlik duvarı kuralları denetlenir. İsteğin IP adresi sunucu düzeyinde IP güvenlik duvarı kurallarında belirtilen aralıklardan biri içindeyse, bağlantı izni verilir. Azure SQL sunucusu üzerindeki tüm SQL veritabanları için sunucu düzeyinde IP güvenlik duvarı kuralları uygulayın.  
- İsteğin IP adresi içinde değilse aralıklardan herhangi bir veritabanı düzeyinde belirtilen veya sunucu düzeyinde IP güvenlik duvarı kuralları, bağlantı isteği başarısız olur.

> [!NOTE]
> Azure SQL Veritabanına yerel bilgisayarınızdan erişmek için, ağ ve yerel bilgisayarınız üzerindeki güvenlik duvarının TCP bağlantı noktası 1433 üzerinde giden iletişime izin verdiğinden emin olun.

### <a name="connecting-from-azure"></a>Azure'dan bağlanma

Azure’daki uygulamaların Azure SQL sunucunuza bağlanmasına izin verebilmeniz için Azure bağlantılarının etkinleştirilmesi gerekir. Azure’dan bir uygulama, veritabanı sunucunuza bağlanmayı denediğinizde güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Başlangıç ve bitiş adresi 0.0.0.0'a eşit ayarı bir güvenlik duvarı Azure bağlantılarına izin verilip verilmeyeceğini gösterir. Bağlantı denemesine izin verilmezse, istek Azure SQL Veritabanı sunucusuna ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.

## <a name="creating-and-managing-ip-firewall-rules"></a>Oluşturma ve IP güvenlik duvarı kurallarını yönetme

İlk sunucu düzeyinde güvenlik duvarı ayarı kullanılarak oluşturulabilir. [Azure portalında](https://portal.azure.com/) veya program aracılığıyla kullanarak [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.sql), [Azure CLI](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create), veya [ REST API](https://docs.microsoft.com/rest/api/sql/firewallrules/createorupdate). Sonraki sunucu düzeyinde IP güvenlik duvarı kuralları oluşturulabilir ve bu yöntemler kullanılarak yönetilen ve Transact-SQL aracılığıyla.

> [!IMPORTANT]
> Veritabanı düzeyinde IP güvenlik duvarı kuralları yalnızca oluşturulabilir ve Transact-SQL kullanılarak yönetilebilir.

Performansı artırmak için sunucu düzeyinde IP güvenlik duvarı kurallarını geçici olarak veritabanı düzeyinde önbelleğe alınır. Önbelleği yenilemek için bkz. [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx).

> [!TIP]
> Kullanabileceğiniz [SQL Database Auditing](sql-database-auditing.md) sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı değişiklikleri denetlemek için.

## <a name="manage-server-level-ip-firewall-rules-using-the-azure-portal"></a>Azure portalını kullanarak sunucu düzeyinde IP güvenlik duvarı kurallarını yönetme

Azure portalında sunucu düzeyinde IP güvenlik duvarı kuralını ayarlamak için ya da Genel Bakış sayfasına, Azure SQL veritabanınızı veya genel bakış sayfası için SQL veritabanı sunucunuz için gidebilirsiniz.

> [!TIP]
> Bir öğretici için bkz. [Azure portalını kullanarak bir veritabanı oluşturma](sql-database-single-database-get-started.md).

### <a name="from-database-overview-page"></a>Veritabanına genel bakış sayfasından

1. Veritabanına genel bakış sayfasından bir sunucu düzeyinde IP güvenlik duvarı kuralını ayarlamak için **sunucu güvenlik duvarını Ayarla** aşağıdaki görüntüde gösterildiği gibi araç çubuğundaki: SQL Veritabanı sunucusu için **Güvenlik duvarı ayarları** sayfası açılır.

      ![Sunucu IP güvenlik duvarı kuralı](./media/sql-database-get-started-portal/server-firewall-rule.png)

2. Tıklayın **istemci IP'si Ekle** bilgisayarın IP adresini eklemek için araç çubuğunda kullanmakta olduğunuz ve ardından **Kaydet**. Geçerli IP adresiniz için sunucu düzeyinde IP güvenlik duvarı kuralı oluşturulur.

      ![sunucu düzeyinde IP güvenlik duvarı kuralı Ayarla](./media/sql-database-get-started-portal/server-firewall-rule-set.png)

### <a name="from-server-overview-page"></a>Sunucu genel bakış sayfasından

Sunucunuzun genel bakış sayfası açılır ve tam sunucu adını gösteren (gibi **mynewserver20170403.database.windows.net**) ve daha fazla yapılandırma seçenekleri sağlar.

1. Server genel bakış sayfasında sunucu düzeyinde kural ayarlamak için **Güvenlik Duvarı** sol menüdeki ayarlar altında:

2. Tıklayın **istemci IP'si Ekle** bilgisayarın IP adresini eklemek için araç çubuğunda kullanmakta olduğunuz ve ardından **Kaydet**. Geçerli IP adresiniz için sunucu düzeyinde IP güvenlik duvarı kuralı oluşturulur.

## <a name="manage-ip-firewall-rules-using-transact-sql"></a>Transact-SQL kullanarak IP güvenlik duvarı kurallarını yönetme

| Katalog Görünümü veya Saklı Yordam | Düzey | Açıklama |
| --- | --- | --- |
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx) |Sunucu |Geçerli sunucu düzeyinde IP güvenlik duvarı kurallarını gösterir |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) |Sunucu |Sunucu düzeyinde IP güvenlik duvarı kuralları oluşturur veya güncelleştirir |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx) |Sunucu |Sunucu düzeyinde IP güvenlik duvarı kurallarını kaldırır |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx) |Database |Geçerli veritabanı düzeyinde IP güvenlik duvarı kurallarını gösterir |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) |Database |Veritabanı düzeyinde IP güvenlik duvarı kuralları oluşturur veya güncelleştirir |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) |Veritabanları |Kaldırır veritabanı düzeyinde IP güvenlik duvarı kuralları |

Aşağıdaki örnekler, mevcut kuralları gözden geçirin, bir IP adresi aralığı Contoso sunucusunda etkinleştirin ve bir IP güvenlik duvarı kuralını siler:

```sql
SELECT * FROM sys.firewall_rules ORDER BY name;
```

Ardından, sunucu düzeyinde IP güvenlik duvarı kuralı ekleyin.

```sql
EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
   @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'
```

Sunucu düzeyinde IP güvenlik duvarı kuralı silmek için sp_delete_firewall_rule saklı yordamını yürütün. Aşağıdaki örnekte, ContosoFirewallRule adlı kural silinmektedir:

```sql
EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
```

## <a name="manage-server-level-ip-firewall-rules-using-azure-powershell"></a>Azure PowerShell kullanarak sunucu düzeyinde IP güvenlik duvarı kurallarını yönetme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

| Cmdlet | Düzey | Açıklama |
| --- | --- | --- |
| [Get-AzSqlServerFirewallRule](/powershell/module/az.sql/get-azsqlserverfirewallrule) |Sunucu |Sunucu düzeyinde geçerli güvenlik duvarı kurallarını döndürür |
| [Yeni AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) |Sunucu |Sunucu düzeyinde yeni bir güvenlik duvarı kuralı oluşturur |
| [Set-AzSqlServerFirewallRule](/powershell/module/az.sql/set-azsqlserverfirewallrule) |Sunucu |Sunucu düzeyinde mevcut güvenlik duvarı kuralının özelliklerini güncelleştirir |
| [Remove-AzSqlServerFirewallRule](/powershell/module/az.sql/remove-azsqlserverfirewallrule) |Sunucu |Sunucu düzeyinde güvenlik duvarı kurallarını kaldırır |

Aşağıdaki örnek, PowerShell kullanarak sunucu düzeyinde IP güvenlik duvarı kuralı ayarlar:

```powershell
New-AzSqlServerFirewallRule -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
```

> [!TIP]
> PowerShell örneklerini Hızlı Başlangıç için bağlamında, [DB oluşturma - PowerShell](sql-database-powershell-samples.md) ve [tek veritabanı oluşturma ve PowerShell kullanarak SQL veritabanı sunucu düzeyinde IP güvenlik duvarı kuralı yapılandırma](scripts/sql-database-create-and-configure-database-powershell.md)

## <a name="manage-server-level-ip-firewall-rules-using-azure-cli"></a>Azure CLI kullanarak sunucu düzeyinde IP güvenlik duvarı kurallarını yönetme

| Cmdlet | Düzey | Açıklama |
| --- | --- | --- |
|[az sql server güvenlik duvarı kuralı oluşturma](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-create)|Sunucu|Sunucu IP güvenlik duvarı kuralı oluşturur.|
|[az sql server güvenlik duvarı kuralı listesi](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-list)|Sunucu|Bir sunucudaki IP güvenlik duvarı kurallarını listeler|
|[az sql server güvenlik duvarı-rule show](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-show)|Sunucu|Bir IP güvenlik duvarı kuralı ayrıntılarını gösterir|
|[az sql server güvenlik duvarı kuralı güncelleştirme](/cli/azure/sql/server/firewall-rule##az-sql-server-firewall-rule-update)|Sunucu|Bir IP güvenlik duvarı kuralını güncelleştirir|
|[az sql server güvenlik duvarı kuralını Sil](/cli/azure/sql/server/firewall-rule#az-sql-server-firewall-rule-delete)|Sunucu|Bir IP güvenlik duvarı kuralını siler|

Aşağıdaki örnekte Azure CLI kullanarak bir sunucu düzeyinde IP güvenlik duvarı kuralı ayarlar:

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
-n AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP]
> Azure CLI örneği bir hızlı başlangıç için bağlamında, bkz: [DB oluşturma - Azure CLI](sql-database-cli-samples.md) ve [tek veritabanı oluşturma ve Azure CLI kullanarak bir SQL veritabanı IP güvenlik duvarı kuralı yapılandırma](scripts/sql-database-create-and-configure-database-cli.md)

## <a name="manage-server-level-ip-firewall-rules-using-rest-api"></a>REST API kullanarak sunucu düzeyinde IP güvenlik duvarı kurallarını yönetme

| API | Düzey | Açıklama |
| --- | --- | --- |
| [Güvenlik Duvarı Kurallarını Listele](https://docs.microsoft.com/rest/api/sql/firewallrules/listbyserver) |Sunucu |Geçerli sunucu düzeyinde IP güvenlik duvarı kurallarını gösterir |
| [Güvenlik Duvarı Kuralı Oluşturma veya Güncelleştirme](https://docs.microsoft.com/rest/api/sql/firewallrules/createorupdate) |Sunucu |Sunucu düzeyinde IP güvenlik duvarı kuralları oluşturur veya güncelleştirir |
| [Güvenlik Duvarı Kuralını Sil](https://docs.microsoft.com/rest/api/sql/firewallrules/delete) |Sunucu |Sunucu düzeyinde IP güvenlik duvarı kurallarını kaldırır |
| [Güvenlik duvarı kurallarını Al](https://docs.microsoft.com/rest/api/sql/firewallrules/get) | Sunucu | Sunucu düzeyinde IP güvenlik duvarı kuralları alır |

## <a name="server-level-versus-database-level-ip-firewall-rules"></a>Sunucu düzeyinde ve veritabanı düzeyinde IP güvenlik duvarı kuralları

S. Kullanıcılar bir veritabanının başka bir veritabanından tamamen yalıtılmış olmalıdır?
Yanıt Evet ise, veritabanı düzeyinde IP güvenlik duvarı kurallarını kullanarak erişim verin. Bu, tüm veritabanları, savunmalarınıza derinliğini azaltmak için güvenlik duvarı üzerinden erişime izin sunucu düzeyinde IP güvenlik duvarı kurallarının kullanılmasını önler.

S. Kullanıcı veya IP adresinin, tüm veritabanlarına erişim gerekiyor mu?
IP güvenlik duvarı kurallarını yapılandırmalısınız sayısını azaltmak için sunucu düzeyinde IP güvenlik duvarı kurallarını kullanın.

S. IP güvenlik duvarı kuralları yalnızca yapılandırma olan kişi veya Azure portalı, PowerShell veya REST API aracılığıyla erişimi var mı?
Sunucu düzeyinde IP güvenlik duvarı kurallarını kullanmanız gerekir. Veritabanı düzeyinde IP güvenlik duvarı kuralları yalnızca Transact-SQL kullanarak de yapılandırılabilir.  

S. Kişi veya ekip veritabanı düzeyinde üst düzey iznine sahip yasaklanmış IP güvenlik duvarı kurallarını yapılandıran?
Sunucu düzeyinde IP güvenlik duvarı kurallarını kullanın. Transact-SQL kullanarak veritabanı düzeyinde IP güvenlik duvarı kuralları yapılandırma en azından `CONTROL DATABASE` veritabanı düzeyinde izin.  

S. Birçok için merkezi olarak IP güvenlik duvarı kurallarını yönetme kişi veya yapılandırma ya da IP güvenlik duvarı kuralları, Denetim takım olup (belki de 100'lük) veritabanlarının?
Bu seçim, gereksinimleri ve ortam bağlıdır. Sunucu düzeyinde IP güvenlik duvarı kurallarını yapılandırmak daha kolay olabilir, ancak komut dosyası kuralları veritabanı düzeyinde yapılandırabilirsiniz. Ve sunucu düzeyinde IP güvenlik duvarı kurallarını kullansanız bile olup veritabanı düzeyinde IP güvenlik duvarı kuralları, Denetim gerekebilir kullanıcılarla `CONTROL` izni veritabanında veritabanı düzeyi IP güvenlik duvarı kuralları oluşturdunuz.

S. IP güvenlik duvarı kuralları hem sunucu düzeyinde ve veritabanı düzeyinde bir karışımını kullanabilir miyim?
Evet. Bazı kullanıcılar, yöneticileri gibi sunucu düzeyi IP güvenlik duvarı kurallarını gerekebilir. Bir veritabanı uygulama kullanıcıları gibi diğer kullanıcıların veritabanı IP güvenlik duvarı kurallarını düzeyinde.

## <a name="troubleshooting-the-database-firewall"></a>Veritabanı güvenlik duvarı sorunlarını giderme

Microsoft Azure SQL Veritabanı hizmetine erişim beklediğiniz gibi davranmadığında aşağıdaki noktaları göz önünde bulundurun:

- **Yerel güvenlik duvarı yapılandırması:**

  Bilgisayarınızın Azure SQL veritabanı erişmeden önce bilgisayarınızdaki 1433 numaralı TCP bağlantı noktası için bir güvenlik duvarı özel durumu oluşturmak gerekebilir. Azure bulut limitleri içerisinde bağlantı oluşturuyorsanız başka bağlantı noktalarını da açmanız gerekebilir. Daha fazla bilgi için **SQL veritabanı: Dış ve iç karşılaştırması** bölümünü [ADO.NET 4.5 ve SQL veritabanı için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md).

- **Ağ adresi çevirisi (NAT):**

  NAT nedeniyle, bilgisayarınızın Azure SQL veritabanı'na bağlanmak için kullanılan IP adresini bilgisayar IP yapılandırma ayarlarında gösterilen IP adresiyle farklı olabilir. Bilgisayarınızın Azure’a bağlanmak için kullandığı IP adresini görüntülemek üzere portalda oturum açın ve veritabanınızı barındıran sunucudaki **Yapılandır** sekmesine gidin. **İzin Verilen IP Adresleri** bölümü altında **Geçerli İstemci IP Adresi** gösterilir. Bu bilgisayarın sunucuya erişmesine izin vermek için **İzin Verilen IP Adresleri** bölümünde **Ekle**’ye tıklayın.

- **İzin verilenler listesine değişiklikler henüz etkili olmamıştır:**

  Bir beş dakikaya kadar yapılan değişikliklerin etkili olması için Azure SQL veritabanı güvenlik duvarı yapılandırması olabilir.

- **Oturum açma yetkili değil veya yanlış parola kullanıldı:**

  Azure SQL veritabanı sunucusunda bir oturum açma izni yok veya kullanılan parola yanlışsa, Azure SQL veritabanı sunucusuyla bağlantı reddedilir. Bir güvenlik duvarı ayarının oluşturulması yalnızca istemcilere sunucunuzla bağlantı kurmayı deneme fırsatı sunar; her istemci gerekli güvenlik kimlik bilgilerini belirtmek zorundadır. Oturumları hazırlama hakkında daha fazla bilgi için bkz. [veritabanlarını yönetme, oturum açma bilgileri ve Azure SQL veritabanı'nda kullanıcıların](sql-database-manage-logins.md).

- **Dinamik IP adresi:**

  Dinamik IP adresiyle kurulmuş bir Internet bağlantısı varsa ve güvenlik duvarını aşmakta sorun yaşıyorsanız aşağıdaki çözümlerden birini deneyebilirsiniz:
  
  - Azure SQL veritabanı sunucusuna erişen istemci bilgisayarlarınıza atanmış IP adresi aralığını Internet servis sağlayıcınıza (ISS) sorun ve IP adresi aralığını bir IP güvenlik duvarı kuralı olarak ekleyin.
  - Statik IP yerine istemci bilgisayarlarınız için adresi alın ve sonra IP adresleri IP güvenlik duvarı kuralları ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

- Kurumsal ağ ortamınıza gelen iletişim Microsoft Azure veri merkezleri tarafından kullanılan işlem IP adresi aralıklarını (SQL aralıkları dahil) gelen sağlar onaylayın. Beyaz listeye gerekli olabilir bu IP adreslerini görmek [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653)  
- Sunucu düzeyinde IP güvenlik duvarı kuralı oluşturma Hızlı Başlangıç için bkz. [bir Azure SQL veritabanı oluşturma](sql-database-single-database-get-started.md).
- Açık kaynak veya üçüncü taraf uygulamalardan bir Azure SQL veritabanına bağlanma konusunda yardım için bkz. [SQL Veritabanına yönelik istemci hızlı başlatma kod örnekleri](https://msdn.microsoft.com/library/azure/ee336282.aspx).
- Açmak için ihtiyacınız olan ek bağlantı noktaları hakkında daha fazla bilgi için bkz: **SQL veritabanı: Dış ve iç karşılaştırması** bölümünü [ADO.NET 4.5 ve SQL veritabanı için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md)
- Azure SQL veritabanı güvenliğine genel bakış için bkz. [veritabanınızı güvenli hale getirme](sql-database-security-overview.md)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
