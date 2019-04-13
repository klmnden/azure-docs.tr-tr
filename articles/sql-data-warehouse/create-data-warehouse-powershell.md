---
title: 'Hızlı Başlangıç: Bir Azure SQL veri ambarı - Azure Powershell oluşturma | Microsoft Docs'
description: Azure PowerShell ile bir SQL veritabanı mantıksal sunucusu, sunucu düzeyinde güvenlik duvarı kuralı ve veri ambarı hızlı bir şekilde oluşturun.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: quickstart
ms.subservice: manage
ms.date: 4/11/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: d76f7ac6c8b60e2dec7d7d95cf419e1352b97f15
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59545137"
---
# <a name="quickstart-create-and-query-an-azure-sql-data-warehouse-with-azure-powershell"></a>Hızlı Başlangıç: Oluşturma ve Azure PowerShell ile Azure SQL veri ambarı sorgulama

Azure PowerShell kullanarak Azure SQL veri ambarı hızlı bir şekilde oluşturun.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

> [!NOTE]
> Bir SQL Veri Ambarı'nın oluşturulması ek hizmet ücretlerinin alınmasına neden olabilir.  Ayrıntılı bilgi için bkz. [SQL Veri Ambarı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Oturum açmak için kullanarak Azure aboneliğinizde [Connect AzAccount](/powershell/module/az.accounts/connect-azaccount) izleyin ve komut ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

Kullanmakta olduğunuz aboneliği görmek için çalıştırma [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription).

```powershell
Get-AzSubscription
```

Varsayılandan farklı bir abonelik kullanmanız gerekiyorsa, çalıştırma [kümesi AzContext](/powershell/module/az.accounts/set-azcontext).

```powershell
Set-AzContext -SubscriptionName "MySubscription"
```


## <a name="create-variables"></a>Değişken oluşturma

Bu hızlı başlangıçtaki betiklerde kullanılacak değişkenleri tanımlayacaksınız.

```powershell
# The data center and resource name for your resources
$resourcegroupname = "myResourceGroup"
$location = "WestEurope"
# The logical server name: Use a random value or replace with your own value (don't capitalize)
$servername = "server-$(Get-Random)"
# Set an admin name and password for your database
# The sign-in information for the server
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# The ip address range that you want to allow to access your server - change as appropriate
$startip = "0.0.0.0"
$endip = "0.0.0.0"
# The database name
$databasename = "mySampleDataWarehosue"
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Oluşturma bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) kullanarak [yeni AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutu. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek `westeurope` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur.

```powershell
New-AzResourceGroup -Name $resourcegroupname -Location $location
```
## <a name="create-a-logical-server"></a>Mantıksal sunucu oluşturma

Oluşturma bir [Azure SQL mantıksal sunucusu](../sql-database/sql-database-logical-servers.md) kullanarak [yeni AzSqlServer](/powershell/module/az.sql/new-azsqlserver) komutu. Mantıksal sunucu, grup olarak yönetilen bir veritabanı grubu içerir. Aşağıdaki örnek rastgele olarak adlandırılmış bir sunucu, kaynak grubunuzda adlı yönetici kullanıcı ile oluşturur `ServerAdmin` ve bir parola `ChangeYourAdminPassword1`. Bu önceden tanımlı değerleri istediğiniz gibi değiştirin.

```powershell
New-AzSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## <a name="configure-a-server-firewall-rule"></a>Sunucu güvenlik duvarı kurallarını yapılandırma

Oluşturma bir [Azure SQL sunucu düzeyinde güvenlik duvarı kuralı](../sql-database/sql-database-firewall-configure.md) kullanarak [yeni AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) komutu. Sunucu düzeyinde güvenlik duvarı kuralı, SQL veri ambarı SQL veri ambarı hizmeti güvenlik duvarı üzerinden bağlanmak için SQL Server Management Studio veya SQLCMD yardımcı programı gibi bir dış uygulamanın sağlar. Aşağıdaki örnekte, güvenlik duvarı yalnızca diğer Azure kaynakları için açılır. Dışarıdan bağlantı kurulabilmesi için IP adresini ortamınız için uygun bir adres olarak değiştirin. Tüm IP adreslerini açmak için başlangıç IP adresi olarak 0.0.0.0’ı, bitiş adresi olaraksa 255.255.255.255’i kullanın.

```powershell
New-AzSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> SQL veritabanı ve SQL veri ambarı 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Kurumsal ağ içinden gelen bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe verilmez. Bu durumda BT departmanınız 1433 numaralı bağlantı noktasını açmadığı sürece Azure SQL sunucunuza bağlanmak mümkün olmayacaktır.
>


## <a name="create-a-data-warehouse"></a>Veri ambarı oluşturma
Bu örnekte önceden tanımlanmış değişkenler kullanarak bir veri ambarını oluşturur.  Hizmet hedefi, veri ambarınız için daha düşük maliyetli bir başlangıç noktası olan DW100c olarak belirtir. 

```Powershell
New-AzSqlDatabase `
    -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -Edition "DataWarehouse" `
    -RequestedServiceObjectiveName "DW100c" `
    -CollationName "SQL_Latin1_General_CP1_CI_AS" `
    -MaxSizeBytes 10995116277760
```

Gerekli Parametreler şunlardır:

* **RequestedServiceObjectiveName**: Miktarı [veri ambarı birimleri](what-is-a-data-warehouse-unit-dwu-cdwu.md) isteyen. Bu miktar artırılması, işlem maliyetini artırır. Desteklenen değerler listesi için bkz. [bellek ve eşzamanlılık sınırları](memory-and-concurrency-limits.md).
* **DatabaseName**: Oluşturmakta olduğunuz SQL veri ambarı'nın adı.
* **ServerName**: Oluşturmak için kullandığınız sunucunun adı.
* **ResourceGroupName**: Kullanmakta olduğunuz kaynak grubu. Aboneliğinizdeki kullanılabilir kaynak gruplarını bulmak için Get-AzureResource komutunu kullanın.
* **Sürüm**: "DataWarehouse" olmalıdır bir SQL veri ambarı oluşturma işlemi.

İsteğe Bağlı Parametreler şunlardır:

- **CollationName**: Belirtilmezse, varsayılan harmanlama sql_latin1_general_cp1_cı_as şeklindedir. Bir veritabanı üzerinde harmanlama değiştirilemez.
- **MaxSizeBytes**: Bir veritabanının varsayılan maksimum boyutunu 240 TB'dir. Rowstore veri en büyük boyutu sınırlar. Sınırsız sütunlu veri depolama yoktur.

Parametre seçenekleri hakkında daha fazla bilgi için bkz. [yeni AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase).


## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer hızlı başlangıç öğreticileri, bu hızlı başlangıcı temel alır. 

> [!TIP]
> Daha hızlı başlangıç öğreticileriyle çalışmaya devam etmeyi planlıyorsanız, kaynakları oluşturulan temizlemeyin Bu hızlı başlangıçta. Devam etmeyi planlamıyorsanız, Azure portalında Bu Hızlı Başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.
>

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Sonraki adımlar

Şimdi bir veri ambarı, veri ambarı'na bağlı bir güvenlik duvarı kuralı, oluşturduğunuz ve birkaç sorgu çalıştırdınız. Azure SQL Veri Ambarı hakkında daha fazla bilgi edinmek için, veri yükleme öğreticisiyle devam edin.
> [!div class="nextstepaction"]
>[SQL veri ambarına veri yükleme](load-data-from-azure-blob-storage-using-polybase.md)
