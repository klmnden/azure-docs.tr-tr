---
title: 'Azure PowerShell: SQL veritabanı oluşturma | Microsoft Docs'
description: Azure portalında SQL Veritabanı mantıksal sunucusu, sunucu düzeyi güvenlik duvarı kuralı ve veritabanı oluşturmayı öğrenin.
keywords: sql veritabanı öğreticisi, sql veritabanı oluşturma
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.devlang: PowerShell
ms.topic: quickstart
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: 5daead37e77bb798e8c870ea3c071823e4d03dce
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="create-a-single-azure-sql-database-using-powershell"></a>PowerShell kullanarak tek Azure SQL veritabanı oluşturma

PowerShell komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu kılavuzda bir [Azure SQL Veritabanı mantıksal sunucusundaki](sql-database-features.md) [Azure kaynak grubuna](../azure-resource-manager/resource-group-overview.md) Azure SQL veritabanı dağıtmak için PowerShell kullanma işlemi ayrıntılı olarak açıklanmaktadır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

Bu öğretici için Azure PowerShell modülünün 4.0 veya daha sonraki bir sürümü gerekir. Sürümü bulmak için ` Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) komutunu kullanarak Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzureRmAccount
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
$databasename = "mySampleDatabase"
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) komutunu kullanarak yeni bir [Azure kaynak grubu](../azure-resource-manager/resource-group-overview.md) oluşturun. Kaynak grubu, Azure kaynaklarının grup olarak dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek `westeurope` konumunda `myResourceGroup` adlı bir kaynak grubu oluşturur.

```powershell
New-AzureRmResourceGroup -Name $resourcegroupname -Location $location
```
## <a name="create-a-logical-server"></a>Mantıksal sunucu oluşturma

[New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) komutunu kullanarak bir [Azure SQL Veritabanı mantıksal sunucusu](sql-database-features.md) oluşturun. Mantıksal sunucu, grup olarak yönetilen bir veritabanı grubu içerir. Aşağıdaki örnek, kaynak grubunuzda `ServerAdmin` yönetici oturum açma bilgileri ve `ChangeYourAdminPassword1` parolası ile rastgele olarak adlandırılmış bir sunucu oluşturur. Bu önceden tanımlı değerleri istediğiniz gibi değiştirin.

```powershell
New-AzureRmSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## <a name="configure-a-server-firewall-rule"></a>Sunucu güvenlik duvarı kurallarını yapılandırma

[New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) komutunu kullanarak bir [Azure SQL Veritabanı sunucu düzeyi güvenlik duvarı kuralı](sql-database-firewall-configure.md) oluşturun. Bir sunucu düzeyi güvenlik duvarı kuralı SQL Server Management Studio veya SQLCMD yardımcı programı gibi bir dış uygulamanın SQL Veritabanı hizmet güvenlik duvarı üzerinden bir SQL veritabanına bağlanmasına olanak sağlar. Aşağıdaki örnekte, güvenlik duvarı yalnızca diğer Azure kaynakları için açılır. Dışarıdan bağlantı kurulabilmesi için IP adresini ortamınız için uygun bir adres olarak değiştirin. Tüm IP adreslerini açmak için başlangıç IP adresi olarak 0.0.0.0’ı, bitiş adresi olaraksa 255.255.255.255’i kullanın.

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> SQL Veritabanı 1433 numaralı bağlantı noktası üzerinden iletişim kurar. Bir kurumsal ağ içerisinden bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe izin verilmiyor olabilir. Bu durumda, BT departmanınız 1433 numaralı bağlantı noktasını açmadığı sürece Azure SQL veritabanı sunucusuna bağlanamazsınız.
>

## <a name="create-a-database-in-the-server-with-sample-data"></a>Sunucuda örnek verilerle veritabanı oluşturma

[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) komutunu kullanarak sunucuda [S0 performans düzeyine](sql-database-service-tiers.md) sahip bir veritabanı oluşturun. Aşağıdaki örnek, `mySampleDatabase` adlı bir veritabanı oluşturur ve AdventureWorksLT örnek verilerini bu veritabanına yükler. Önceden tanımlanmış bu değerleri istediğiniz gibi değiştirin (bu koleksiyondaki diğer hızlı başlangıçlar, bu hızlı başlangıçtaki değerlere göre belirlenir).

```powershell
New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -SampleName "AdventureWorksLT" `
    -RequestedServiceObjectiveName "S0"
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer hızlı başlangıçlar, bu hızlı başlangıcı temel alır.

> [!TIP]
> Sonraki hızlı başlangıçlarla çalışmaya devam etmeyi planlıyorsanız bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız Azure portalında bu hızlı başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.
>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Sonraki adımlar

- Artık bir veritabanınız olduğuna göre, sık kullandığınız araçlarla ya da dillerle [bağlanabilir ve sorgulayabilirsiniz](sql-database-connect-query.md).
- İlk veritabanınızı tasarlamayı, tablolar oluşturmayı ve veri eklemeyi öğrenmek için, şu öğreticilerden birine bakın:
 - [SSMS kullanarak ilk Azure SQL veritabanınızı tasarlama](sql-database-design-first-database.md)
  - [C# ve ADO.NET ile bir Azure SQL veritabanı tasarlama ve bağlama](sql-database-design-first-database-csharp.md)

