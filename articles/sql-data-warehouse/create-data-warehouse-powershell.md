---
title: 'Hızlı Başlangıç: Azure SQL veri ambarı - Azure Powershell oluşturun. | Microsoft Docs'
description: Hızlı bir şekilde bir SQL Database mantıksal sunucusu, sunucu düzeyinde güvenlik duvarı kuralı ve veri ambarı Azure PowerShell ile oluşturun.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: e0bb014ec0706d458ff2f38e409efba5d66aaf18
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="quickstart-create-and-query-an-azure-sql-data-warehouse-with-azure-powershell"></a>Hızlı Başlangıç: Oluşturun ve Azure PowerShell ile bir Azure SQL veri ambarı sorgulama

Hızlı bir şekilde Azure PowerShell kullanarak bir Azure SQL data warehouse oluşturma.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

Bu öğreticide Azure PowerShell modülü sürümü 5.1.1 gerektirir veya sonraki bir sürümü. Çalıştırma `Get-Module -ListAvailable AzureRM` sürümünü bulmak için şu anda yok. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps). 


> [!NOTE]
> Bir SQL Veri Ambarı'nın oluşturulması ek hizmet ücretlerinin alınmasına neden olabilir.  Ayrıntılı bilgi için bkz. [SQL Veri Ambarı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).
>
>

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) komutunu kullanarak Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Add-AzureRmAccount
```

Kullanmakta olduğunuz abonelik görmek için çalıştırın [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription).

```powershell
Get-AzureRmSubscription
```

Varsayılandan farklı bir abonelik kullanmanız gerekiyorsa, çalıştırmak [Select-AzureRmSubscription](/powershell/module/azurerm.profile/select-azurermsubscription).

```powershell
Select-AzureRmSubscription -SubscriptionName "MySubscription"
```


## <a name="create-variables"></a>Değişken oluşturma

Bu hızlı başlangıçtaki betiklerde kullanılacak değişkenleri tanımlayacaksınız.

```powershell
# The data center and resource name for your resources
$resourcegroupname = "myResourceGroup"
$location = "WestEurope"
# The logical server name: Use a random value or replace with your own value (do not capitalize)
$servername = "server-$(Get-Random)"
# Set an admin login and password for your database
# The login information for the server
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# The ip address range that you want to allow to access your server - change as appropriate
$startip = "0.0.0.0"
$endip = "0.0.0.0"
# The database name
$databasename = "mySampleDataWarehosue"
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) komutunu kullanarak yeni bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek `westeurope` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur.

```powershell
New-AzureRmResourceGroup -Name $resourcegroupname -Location $location
```
## <a name="create-a-logical-server"></a>Mantıksal sunucu oluşturma

Oluşturma bir [Azure SQL mantıksal sunucusuna](../sql-database/sql-database-servers-databases.md#what-is-an-azure-sql-logical-server) kullanarak [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) komutu. Mantıksal sunucu, grup olarak yönetilen bir veritabanı grubu içerir. Aşağıdaki örnek, kaynak grubunuzda `ServerAdmin` yönetici oturum açma bilgileri ve `ChangeYourAdminPassword1` parolası ile rastgele olarak adlandırılmış bir sunucu oluşturur. Bu önceden tanımlı değerleri istediğiniz gibi değiştirin.

```powershell
New-AzureRmSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## <a name="configure-a-server-firewall-rule"></a>Sunucu güvenlik duvarı kurallarını yapılandırma

Oluşturma bir [Azure SQL sunucu düzeyinde güvenlik duvarı kuralı](../sql-database/sql-database-firewall-configure.md) kullanarak [yeni AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) komutu. Sunucu düzeyinde güvenlik duvarı kuralı SQL data warehouse SQL Data Warehouse hizmeti güvenlik duvarı üzerinden bağlanmak için SQL Server Management Studio veya SQLCMD yardımcı programını gibi harici bir uygulama sağlar. Aşağıdaki örnekte, güvenlik duvarı yalnızca diğer Azure kaynakları için açılır. Dışarıdan bağlantı kurulabilmesi için IP adresini ortamınız için uygun bir adres olarak değiştirin. Tüm IP adreslerini açmak için başlangıç IP adresi olarak 0.0.0.0’ı, bitiş adresi olaraksa 255.255.255.255’i kullanın.

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> SQL Database ve SQL Data Warehouse 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Bir kurumsal ağ içerisinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda, BT departmanınızın 1433 numaralı bağlantı noktasını açar sürece Azure SQL sunucunuza bağlanmak mümkün olmaz.
>


## <a name="create-a-data-warehouse-with-sample-data"></a>Bir veri ambarına örnek veri oluşturma
Bu örnek daha önce tanımlanan değişkenler kullanarak bir veri ambarını oluşturur.  Hizmet hedefi daha düşük maliyetli başlangıç noktası, veri ambarı için DW400 olarak belirtir. 

```Powershell
New-AzureRmSqlDatabase `
    -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -Edition "DataWarehouse" `
    -RequestedServiceObjectiveName "DW400" `
    -CollationName "SQL_Latin1_General_CP1_CI_AS" `
    -MaxSizeBytes 10995116277760
```

Gerekli Parametreler şunlardır:

* **RequestedServiceObjectiveName**: miktarını [veri ambarı birimlerini](what-is-a-data-warehouse-unit-dwu-cdwu.md) isteğinde bulunduğunuzu. Bu miktar artırılması işlem maliyetini artırır. Desteklenen değerler listesi için bkz: [bellek ve eşzamanlılık sınırları](memory-and-concurrency-limits.md).
* **DatabaseName**: Oluşturduğunuz SQL Data Warehouse'un adı.
* **ServerName**: oluşturmak için kullandığınız sunucunun adı.
* **ResourceGroupName**: Kullandığınız kaynak grubu. Aboneliğinizdeki kullanılabilir kaynak gruplarını bulmak için Get-AzureResource komutunu kullanın.
* **Edition**: SQL Veri Ambarı oluşturmak için "DataWarehouse" olmalıdır.

İsteğe Bağlı Parametreler şunlardır:

- **CollationName**: Belirtilmezse varsayılan harmanlama SQL_Latin1_General_CP1_CI_AS şeklindedir. Bir veritabanı üzerinde harmanlama değiştirilemez.
- **MaxSizeBytes**: Bir veritabanının varsayılan en büyük boyutu 10 GB'dir.

Parametre seçenekleri hakkında daha fazla bilgi için bkz: [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase).


## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer hızlı başlangıç öğreticileri, bu hızlı başlangıcı temel alır. 

> [!TIP]
> Sonraki hızlı başlangıç öğreticileriyle çalışmaya devam etmeyi planlıyorsanız bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız Azure portalında bu hızlı başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.
>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Sonraki adımlar

Şimdi bir veri ambarı oluşturdunuz, bir güvenlik duvarı kuralı oluşturdunuz, veri ambarınıza bağlandınız ve birkaç sorgu çalıştırdınız. Azure SQL Veri Ambarı hakkında daha fazla bilgi edinmek için, veri yükleme öğreticisiyle devam edin.
> [!div class="nextstepaction"]
>[SQL veri ambarına veri yükleme](load-data-from-azure-blob-storage-using-polybase.md)
