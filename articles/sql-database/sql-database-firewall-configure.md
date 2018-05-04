---
title: Azure SQL veritabanı güvenlik duvarı kuralları | Microsoft Docs
description: Erişimi yönetmek amacıyla, sunucu düzeyinde ve veritabanı düzeyinde güvenlik kuralları kullanarak SQL veritabanı güvenlik duvarını yapılandırma hakkında bilgi edinin.
keywords: veritabanı güvenlik duvarı
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: security
ms.topic: article
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: f43e380d1af846a0c77d61b4e8827c8b45fb08a6
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="azure-sql-database-server-level-and-database-level-firewall-rules"></a>Azure SQL veritabanı sunucusu ve veritabanı düzeyi güvenlik duvarı kuralları 

Microsoft Azure SQL Veritabanı, Azure ile diğer İnternet tabanlı uygulamalar için ilişkisel veritabanı hizmeti sunar. Güvenlik duvarları, verilerinizin korunmasına yardımcı olmak üzere, hangi bilgisayarların izinli olduğunu belirtmenize kadar veritabanı sunucunuza tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.

#### <a name="virtual-network-rules-as-alternatives-to-ip-rules"></a>Sanal ağ kuralları IP kuralları alternatifleri olarak

IP kurallarının yanı sıra, Güvenlik Duvarı'nı da yönetir *sanal ağ kuralları*. Sanal ağ kuralları üzerindeki sanal ağ hizmet uç noktaları temel alır. Sanal ağ kuralları bazı durumlarda IP kuralları tercih edilebilir. Daha fazla bilgi için bkz: [sanal ağ hizmet uç noktaları ve Azure SQL veritabanı için kuralları](sql-database-vnet-service-endpoint-rule-overview.md).

## <a name="overview"></a>Genel Bakış

Başlangıçta, Azure SQL sunucunuza tüm Transact-SQL erişimleri güvenlik duvarı tarafından engellenir. Azure SQL server'ı kullanmaya başlamak için Azure SQL sunucunuza erişimi sağlayan bir veya daha fazla sunucu düzeyinde güvenlik duvarı kuralları belirtmeniz gerekir. İnternet’ten gelen hangi IP adresi aralıklarının izinli olduğuna ve Azure uygulamalarının Azure SQL sunucunuza bağlanmaya çalışıp çalışamayacaklarına ilişkin güvenlik duvarı kurallarını belirleyin.

Azure SQL sunucunuzdaki veritabanlarından yalnızca birine seçmeli olarak erişim vermek için, gerekli veritabanına yönelik bir veritabanı düzeyinde kural oluşturmanız gerekir. Veritabanı güvenlik duvarı kuralı için, sunucu düzeyinde güvenlik duvarı kuralında belirtilen IP adres aralığının dışında olan bir IP adres aralığı belirtin ve istemci IP adresinin veritabanı düzeyindeki kuralda belirtilen aralığa denk geldiğinden emin olun.

İnternet’ten ve Azure’dan gelen bağlantı denemeleri, Azure SQL sunucunuza veya SQL Veritabanına ulaşmadan önce aşağıdaki diyagramda gösterildiği gibi ilk olarak güvenlik duvarından geçmelidir:

   ![Güvenlik duvarı yapılandırmasını açıklayan diyagram.][1]

* **Sunucu düzeyinde güvenlik duvarı kuralları:** Bu kurallar istemcilerin tüm Azure SQL sunucusuna, yani aynı mantıksal sunucu içindeki tüm veritabanlarına erişmesini sağlar. Bu kurallar **ana** veritabanına depolanır. Sunucu düzeyinde güvenlik duvarı kuralları, portal ya da Transact-SQL deyimleri kullanılarak yapılandırılabilir. Azure portalı veya PowerShell kullanarak sunucu düzeyinde güvenlik duvarı kuralları oluşturmak için abonelik sahibi veya abonelik katkıda bulunanı olmanız gerekir. Transact-SQL kullanarak sunucu düzeyinde güvenlik duvarı kuralı oluşturmak için SQL Veritabanı örneğine sunucu düzeyi asıl oturum açma bilgileriyle veya Azure Active Directory yöneticisi olarak bağlanmanız gerekir (başka bir deyişle, sunucu düzeyi güvenlik duvarı kuralının önce Azure düzeyi izinlere sahip bir kullanıcı tarafından oluşturulması gerekir).
* **Veritabanı düzeyi güvenlik duvarı kuralları:** aynı mantıksal sunucu içinde (güvenli) belirli veritabanlarına erişmek istemciler bu kuralları etkinleştirin. Her veritabanı için bu kurallar oluşturabilirsiniz (de dahil olmak üzere **ana** veritabanı) ve tek tek veritabanlarında depolanır. Ana ve kullanıcı veritabanları için veritabanı düzeyinde güvenlik duvarı kuralları yalnızca oluşturulur ve Transact-SQL deyimi kullanarak ve yalnızca ilk sunucu düzeyinde Güvenlik Duvarı'nı yapılandırdıktan sonra yönetilir. Veritabanı düzeyinde güvenlik kuralı için, sunucu düzeyinde güvenlik duvarı kuralında belirtilen aralığın dışındaki bir IP adresi aralığını belirtirseniz, yalnızca veritabanı düzeyi aralığındaki IP adreslerine sahip istemciler veritabanına erişebilir. Bir veritabanı için en fazla 128 veritabanı düzeyinde güvenlik duvarı kuralınız olabilir. Makale ve bakın örnekte daha sonra bu veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma hakkında daha fazla bilgi için bkz [sp_set_database_firewall_rule (Azure SQL veritabanları)](https://msdn.microsoft.com/library/dn270010.aspx).

**Öneri:** Microsoft, güvenliği artırmak ve veritabanınızı daha taşınabilir hale getirmek açısından, mümkün olan durumlarda veritabanı düzeyinde güvenlik duvarı kurallarının kullanılmasını önerir. Aynı erişim gereksinimlerine sahip birçok veritabanınız varsa ve her veritabanını ayrı ayrı yapılandırmaya zaman harcamak istemiyorsanız sunucu düzeyinde güvenlik duvarı kurallarını yöneticiler için kullanabilirsiniz.

> [!Important]
> Windows Azure SQL veritabanı en fazla 128 güvenlik duvarı kurallarını destekler.
>

> [!Note]
> İş sürekliliği bağlamında taşınabilir veritabanları hakkında bilgi edinmek için bkz. [Olağanüstü durum kurtarma için kimlik doğrulama gereksinimleri](sql-database-geo-replication-security-config.md).
>

### <a name="connecting-from-the-internet"></a>İnternet'ten bağlanma

Bir bilgisayar İnternet'ten veritabanı sunucunuza bağlanmaya çalıştığında, güvenlik duvarı ilk olarak isteğin kaynak IP adresini, bağlantının istediği veritabanı için veritabanı düzeyi güvenlik duvarı kurallarına karşı denetler:

* İsteğin IP adresi veritabanı düzeyinde güvenlik duvarı kurallarında belirtilen aralıklardan biri içindeyse, kuralı içeren SQL Veritabanı’na bağlantı izni verilir.
* İsteğin IP adresi veritabanı düzeyinde güvenlik duvarı kuralında belirtilen aralıklardan biri içinde değilse, sunucu düzeyinde güvenlik duvarı kuralları denetlenir. İstek IP adresi sunucu düzeyinde güvenlik duvarı kurallarında belirtilen aralıklardan biri içindeyse, bağlantı izni verilir. Sunucu düzeyinde güvenlik duvarı kuralları, Azure SQL server üzerindeki tüm SQL veritabanları için geçerlidir.  
* IP adresi isteğin herhangi bir veritabanı düzeyi veya sunucu düzeyinde güvenlik duvarı kuralları içinde belirtilen aralık içinde değil bağlantı isteği başarısız olur.

> [!NOTE]
> Azure SQL Veritabanına yerel bilgisayarınızdan erişmek için, ağ ve yerel bilgisayarınız üzerindeki güvenlik duvarının TCP bağlantı noktası 1433 üzerinde giden iletişime izin verdiğinden emin olun.
> 

### <a name="connecting-from-azure"></a>Azure'dan bağlanma
Azure’daki uygulamaların Azure SQL sunucunuza bağlanmasına izin verebilmeniz için Azure bağlantılarının etkinleştirilmesi gerekir. Azure’dan bir uygulama, veritabanı sunucunuza bağlanmayı denediğinizde güvenlik duvarı Azure bağlantılarına izin verildiğini doğrular. Başlangıç ve bitiş adresi 0.0.0.0’a eşit olan bir güvenlik duvarı ayarı, bu bağlantılara izin verildiğini gösterir. Bağlantı denemesine izin verilmezse, istek Azure SQL Veritabanı sunucusuna ulaşmaz.

> [!IMPORTANT]
> Bu seçenek, diğer müşterilerin aboneliklerinden gelen bağlantılar dahil Azure’dan tüm bağlantılara izin verecek şekilde güvenlik duvarınızı yapılandırır. Bu seçeneği belirlerken, oturum açma ve kullanıcı izinlerinizin erişimi yalnızca yetkili kullanıcılarla sınırladığından emin olun.
> 

## <a name="creating-and-managing-firewall-rules"></a>Güvenlik duvarı kuralları oluşturmayı ve yönetmeyi
İlk sunucu düzeyinde güvenlik duvarı ayarı kullanılarak oluşturulabilir [Azure portal](https://portal.azure.com/) veya program aracılığıyla kullanarak [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql), [Azure CLI](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create), veya [ REST API](https://docs.microsoft.com/rest/api/sql/firewallrules). Sunucu düzeyinde sonraki güvenlik duvarı kuralları ise bu yöntemler kullanılarak ve Transact-SQL aracılığıyla oluşturulup yönetilebilir. 

> [!IMPORTANT]
> Veritabanı düzeyinde güvenlik duvarı kuralları yalnızca oluşturulabilir ve Transact-SQL kullanılarak yönetilebilir. 
>

Performansı artırmak için sunucu düzeyinde güvenlik duvarı kuralları veritabanı düzeyinde geçici olarak önbelleğe alınır. Önbelleği yenilemek için bkz. [DBCC FLUSHAUTHCACHE](https://msdn.microsoft.com/library/mt627793.aspx). 

> [!TIP]
> Kullanabileceğiniz [SQL veritabanı denetimi](sql-database-auditing.md) sunucu ve veritabanı düzeyi güvenlik duvarı değişiklikleri denetlemek için.
>

## <a name="manage-firewall-rules-using-the-azure-portal"></a>Azure Portalı'nı kullanarak güvenlik duvarı kurallarını yönet

Azure portalında bir sunucu düzeyinde güvenlik duvarı kuralı ayarlamak için ya da Genel Bakış sayfasına Azure SQL veritabanınızı veya genel bakış sayfası için Azure veritabanı mantıksal sunucunuz için gidebilirsiniz.

> [!TIP]
> Bir öğretici için bkz: [Azure portalını kullanarak bir DB Oluştur](sql-database-get-started-portal.md).
>

**Veritabanı genel bakış sayfasında**

1. Veritabanı genel bakış sayfasında sunucu düzeyinde güvenlik duvarı kuralı ayarlamak için tıklatın **ayarlayın sunucu Güvenlik Duvarı** aşağıdaki görüntüde gösterildiği gibi araç çubuğunda: **Güvenlik Duvarı ayarları** SQL veritabanı sunucusu için sayfası açılır.

      ![sunucu güvenlik duvarı kuralı](./media/sql-database-get-started-portal/server-firewall-rule.png) 

2. Tıklatın **istemci IP'si Ekle** bilgisayarın IP adresi eklemek için araç çubuğundaki kullanmakta olduğunuz ve ardından **kaydetmek**. Geçerli IP adresiniz için bir sunucu düzeyi güvenlik duvarı kuralı oluşturulur.

      ![sunucu güvenlik duvarı kuralı ayarla](./media/sql-database-get-started-portal/server-firewall-rule-set.png) 

**Server genel bakış sayfasında**

Tam sunucu adını gösteren sunucunuz için genel bakış sayfasında açılır (gibi **mynewserver20170403.database.windows.net**) ve diğer yapılandırmalar için seçenekler sağlar.

1. Server genel bakış sayfasında sunucu düzeyi kural kümesi için tıklatın **Güvenlik Duvarı** ayarlar altında sol menüde: 

2. Tıklatın **istemci IP'si Ekle** bilgisayarın IP adresi eklemek için araç çubuğundaki kullanmakta olduğunuz ve ardından **kaydetmek**. Geçerli IP adresiniz için bir sunucu düzeyi güvenlik duvarı kuralı oluşturulur.

## <a name="manage-firewall-rules-using-transact-sql"></a>Transact-SQL kullanarak güvenlik duvarı kurallarını yönet
| Katalog Görünümü veya Saklı Yordam | Düzey | Açıklama |
| --- | --- | --- |
| [sys.firewall_rules](https://msdn.microsoft.com/library/dn269980.aspx) |Sunucu |Sunucu düzeyinde geçerli güvenlik duvarı kurallarını gösterir |
| [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) |Sunucu |Sunucu düzeyinde güvenlik duvarı kuralları oluşturur veya güncelleştirir |
| [sp_delete_firewall_rule](https://msdn.microsoft.com/library/dn270024.aspx) |Sunucu |Sunucu düzeyinde güvenlik duvarı kurallarını kaldırır |
| [sys.database_firewall_rules](https://msdn.microsoft.com/library/dn269982.aspx) |Database |Veritabanı düzeyinde geçerli güvenlik duvarı kurallarını gösterir |
| [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) |Database |Veritabanı düzeyinde güvenlik duvarı kuralları oluşturur veya güncelleştirir |
| [sp_delete_database_firewall_rule](https://msdn.microsoft.com/library/dn270030.aspx) |Veritabanları |Veritabanı düzeyinde güvenlik duvarı kurallarını kaldırır |


Aşağıdaki örnekler varolan kuralları gözden geçirin, bir IP adresi aralığı Contoso sunucuda etkinleştirmek ve bir güvenlik duvarı kuralını siler:
   
```sql
SELECT * FROM sys.firewall_rules ORDER BY name;
```
  
Daha sonra bir güvenlik duvarı kuralı ekleyin.
   
```sql
EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
   @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'
```

Sunucu düzeyindeki bir güvenlik duvarı kuralını silmek için sp_delete_firewall_rule saklı yordamını yürütün. Aşağıdaki örnek, ContosoFirewallRule adlı kuralını siler:
   
```sql
EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'
```   

## <a name="manage-firewall-rules-using-azure-powershell"></a>Azure PowerShell'i kullanarak güvenlik duvarı kurallarını yönet
| Cmdlet | Düzey | Açıklama |
| --- | --- | --- |
| [Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule) |Sunucu |Sunucu düzeyinde geçerli güvenlik duvarı kurallarını döndürür |
| [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) |Sunucu |Sunucu düzeyinde yeni bir güvenlik duvarı kuralı oluşturur |
| [Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule) |Sunucu |Sunucu düzeyinde mevcut güvenlik duvarı kuralının özelliklerini güncelleştirir |
| [Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule) |Sunucu |Sunucu düzeyinde güvenlik duvarı kurallarını kaldırır |


Aşağıdaki örnek PowerShell kullanarak bir sunucu düzeyinde güvenlik duvarı kuralı ayarlar:

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName "myResourceGroup" `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
```

> [!TIP]
> PowerShell ilişkin örnekler için hızlı başlangıç bağlamında bkz [oluşturma DB - PowerShell](sql-database-get-started-powershell.md) ve [tek bir veritabanı oluşturabilirsiniz ve PowerShell kullanarak bir güvenlik duvarı kuralı yapılandırma](scripts/sql-database-create-and-configure-database-powershell.md)
>

## <a name="manage-firewall-rules-using-azure-cli"></a>Azure CLI kullanarak güvenlik duvarı kurallarını yönet
| Cmdlet | Düzey | Açıklama |
| --- | --- | --- |
|[az sql server güvenlik duvarı kuralı oluşturma](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_create)|Sunucu|Sunucu güvenlik duvarı kuralı oluşturur|
|[az sql server güvenlik duvarı kuralı listesi](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_list)|Sunucu|Bir sunucudaki güvenlik duvarı kurallarını listeler|
|[az sql server güvenlik duvarı kuralı Göster](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_show)|Sunucu|Bir güvenlik duvarı kuralı ayrıntılarını gösterir|
|[az sql server güvenlik duvarı kuralı güncelleştirme](/cli/azure/sql/server/firewall-rule##az_sql_server_firewall_rule_update)|Sunucu|Bir güvenlik duvarı kuralını güncelleştirir|
|[az sql server güvenlik duvarı kuralını Sil](/cli/azure/sql/server/firewall-rule#az_sql_server_firewall_rule_delete)|Sunucu|Bir güvenlik duvarı kuralını siler|

Aşağıdaki örnek, Azure CLI kullanarak bir sunucu düzeyinde güvenlik duvarı kuralı ayarlar: 

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server $servername \
    -n AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP]
> Bir Azure CLI örneği hızlı bir başlangıç için bağlamında bkz [oluşturma Çiftazalanbakiye - Azure CLI](sql-database-get-started-cli.md) ve [tek bir veritabanı oluşturun ve Azure CLI kullanarak bir güvenlik duvarı kuralı yapılandırma](scripts/sql-database-create-and-configure-database-cli.md)
>

## <a name="manage-firewall-rules-using-rest-api"></a>REST API kullanarak güvenlik duvarı kurallarını yönet
| API | Düzey | Açıklama |
| --- | --- | --- |
| [Güvenlik Duvarı Kurallarını Listele](https://docs.microsoft.com/rest/api/sql/FirewallRules/ListByServer) |Sunucu |Sunucu düzeyinde geçerli güvenlik duvarı kurallarını gösterir |
| [Güvenlik Duvarı Kuralı Oluşturma veya Güncelleştirme](https://docs.microsoft.com/rest/api/sql/FirewallRules/CreateOrUpdate) |Sunucu |Sunucu düzeyinde güvenlik duvarı kuralları oluşturur veya güncelleştirir |
| [Güvenlik Duvarı Kuralını Sil](https://docs.microsoft.com/rest/api/sql/FirewallRules/Delete) |Sunucu |Sunucu düzeyinde güvenlik duvarı kurallarını kaldırır |

## <a name="server-level-firewall-rule-versus-a-database-level-firewall-rule"></a>Sunucu düzeyinde güvenlik duvarı kuralı veritabanı düzeyinde güvenlik duvarı kuralı karşılaştırması
Q. Bir veritabanının kullanıcıları başka bir veritabanından tamamen yalıtılmış olması gerekiyor mu?   
  Yanıt Evet ise, veritabanı düzeyinde güvenlik duvarı kurallarını kullanarak erişim izni. Bu, savunma derinliğini azaltma, tüm veritabanları için güvenlik duvarı üzerinden erişime sunucu düzeyinde güvenlik duvarı kurallarını kullanarak önler.   
 
Q. Kullanıcılar IP adresindeki ait tüm veritabanlarına erişim gerekiyor mu?   
  Güvenlik duvarı kuralları yapılandırmalısınız sayısını azaltmak için sunucu düzeyinde güvenlik duvarı kurallarını kullanın.   

Q. Kişi ya da güvenlik duvarı kuralları yalnızca yapılandırma takım Azure portalı, PowerShell veya REST API aracılığıyla erişimi var mı?   
  Sunucu düzeyinde güvenlik duvarı kuralları kullanmanız gerekir. Veritabanı düzeyinde güvenlik duvarı kuralları yalnızca Transact-SQL kullanılarak yapılandırılabilir.  

Q. Kişi veya takım veritabanı düzeyinde üst düzey iznine sahip yasaklanmış güvenlik duvarı kuralları yapılandırma?   
  Sunucu düzeyinde güvenlik duvarı kurallarını kullanın. Transact-SQL kullanarak veritabanı düzeyinde güvenlik duvarı kuralları yapılandırma en azından `CONTROL DATABASE` veritabanı düzeyinde izni.  

Q. Merkezi olarak çoğu için güvenlik duvarı kurallarını yönetme kişi ya da yapılandırma veya güvenlik duvarı kuralları denetim takım olduğu (belki de 100s) veritabanlarının?   
  Bu seçim, gereksinimleri ve ortam bağlıdır. Sunucu düzeyinde güvenlik duvarı kurallarını yapılandırmak daha kolay olabilir, ancak komut dosyası kuralları veritabanı düzeyinde yapılandırabilirsiniz. Ve sunucu düzeyinde güvenlik duvarı kuralları kullansanız bile, olmadığını görmek için veritabanı güvenlik duvarı kurallarını denetleme gerekebilir kullanıcılarla `CONTROL` izni veritabanında veritabanı düzeyinde güvenlik duvarı kuralları oluşturdunuz.   

Q. Her iki sunucu ve veritabanı düzeyi güvenlik duvarı kuralları bir karışımını kullanabilir miyim?   
  Evet. Yöneticiler gibi bazı kullanıcıların sunucu düzeyinde güvenlik duvarı kuralları gerekebilir. Kullanıcıların bir veritabanı uygulamasının gibi diğer kullanıcıların veritabanı düzeyinde güvenlik duvarı kuralları gerekebilir.   

## <a name="troubleshooting-the-database-firewall"></a>Veritabanı güvenlik duvarı sorunlarını giderme
Microsoft Azure SQL Veritabanı hizmetine erişim beklediğiniz gibi davranmadığında aşağıdaki noktaları göz önünde bulundurun:

* **Yerel güvenlik duvarı yapılandırması:** Bilgisayarınızın Azure SQL Veritabanına erişebilmesi için bilgisayarınızda TCP bağlantı noktası 1433 için bir güvenlik duvarı özel durumu oluşturmanız gerekir. Azure bulut limitleri içerisinde bağlantı oluşturuyorsanız başka bağlantı noktalarını da açmanız gerekebilir. Daha fazla bilgi için bkz: **SQL Database: içinde vs dışındaki** bölümünü [ADO.NET 4.5 ve SQL veritabanı için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md).
* **Ağ adresi çevirisi (NAT):** NAT nedeniyle, bilgisayarınızın Azure SQL Veritabanına bağlanmak için kullandığı IP adresi, bilgisayarınızın IP yapılandırma ayarlarında gösterilen IP adresinden farklı olabilir. Bilgisayarınızın Azure’a bağlanmak için kullandığı IP adresini görüntülemek üzere portalda oturum açın ve veritabanınızı barındıran sunucudaki **Yapılandır** sekmesine gidin. **İzin Verilen IP Adresleri** bölümü altında **Geçerli İstemci IP Adresi** gösterilir. Bu bilgisayarın sunucuya erişmesine izin vermek için **İzin Verilen IP Adresleri** bölümünde **Ekle**’ye tıklayın.
* **İzin verilenler listesindeki değişiklikler henüz uygulanmadı:** Azure SQL Veritabanı güvenlik duvarı yapılandırmasındaki değişikliklerin etkili olması beş dakikaya kadar sürebilir.
* **Kullanıcı adı yetkili değil veya yanlış parola kullanıldı:** Azure SQL Veritabanı sunucusunda bir kullanıcı adı yetkili değilse veya kullanılan parola yanlışsa, Azure SQL Veritabanı sunucusuyla bağlantı reddedilir. Bir güvenlik duvarı ayarının oluşturulması yalnızca istemcilere sunucunuzla bağlantı kurmayı deneme fırsatı sunar; her istemci gerekli güvenlik kimlik bilgilerini belirtmek zorundadır. Oturum açma bilgileri hazırlama hakkında daha fazla bilgi için bkz: [yönetme veritabanları, oturumları ve kullanıcıları Azure SQL veritabanında](sql-database-manage-logins.md).
* **Dinamik IP adresi:** Dinamik IP adresiyle kurulmuş bir İnternet bağlantınız varsa ve güvenlik duvarını aşmakta sorun yaşıyorsanız aşağıdaki çözümlerden birini deneyebilirsiniz:
  
  * Azure SQL Veritabanı sunucusuna erişen istemci bilgisayarlarınıza atanmış IP adresi aralığını İnternet Servis Sağlayıcınıza (ISS) sorun ve IP adresi aralığını güvenlik duvarı kuralı olarak ekleyin.
  * İstemci bilgisayarlarınız için bunun yerine statik IP adresi alın ve IP adreslerini güvenlik duvarı kuralları olarak ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

- Hızlı bir başlangıç bir veritabanı ve sunucu düzeyinde güvenlik duvarı kuralı oluşturma hakkında bilgi için bkz: [bir Azure SQL veritabanı oluşturma](sql-database-get-started-portal.md).
- Açık kaynak veya üçüncü taraf uygulamalardan bir Azure SQL veritabanına bağlanma konusunda yardım için bkz. [SQL Veritabanına yönelik istemci hızlı başlatma kod örnekleri](https://msdn.microsoft.com/library/azure/ee336282.aspx).
- Açmak için gereken ek bağlantı noktaları hakkında daha fazla bilgi için bkz: **SQL veritabanı: içinde vs dışındaki** bölümünü [ADO.NET 4.5 ve SQL veritabanı için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md)
- Azure SQL Database güvenliğine genel bakış için bkz: [veritabanınızın güvenliğini sağlama](sql-database-security-overview.md)

<!--Image references-->
[1]: ./media/sql-database-firewall-configure/sqldb-firewall-1.png
