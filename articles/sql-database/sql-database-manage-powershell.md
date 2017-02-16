---
title: "PowerShell ile Azure SQL Veritabanı yönetimi | Microsoft Belgeleri"
description: "PowerShell ile Azure SQL Veritabanı yönetimi."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: 3f21ad5e-ba99-4010-b244-5e5815074d31
ms.service: sql-database
ms.custom: overview
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/15/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: adad6b8e27e0996559d5e6dacb8dd60fbf52a631
ms.openlocfilehash: 0c1ce1c29e447d9db4ef0df7873ef89cb835abee


---
# <a name="managing-azure-sql-database-using-powershell"></a>PowerShell kullanarak Azure SQL Veritabanını yönetme
> [!div class="op_single_selector"]
> * [Azure Portal](sql-database-manage-portal.md)
> * [Transact-SQL (SSMS)](sql-database-manage-azure-ssms.md)
> * [PowerShell](sql-database-manage-powershell.md)
> 
> 

Bu konu başlığında, birçok Azure SQL veritabanı görevini gerçekleştirmek için kullanabileceğiniz PowerShell cmdlet'leri gösterilmektedir. Tam liste için bkz. [Azure SQL Veritabanı Cmdlet'leri](https://msdn.microsoft.com/library/mt574084\(v=azure.300\).aspx).

> [!TIP]
> Sunucu oluşturma, sunucu tabanlı güvenlik duvarı oluşturma, sunucu özelliklerini görüntüleme, bağlanma ve ana veritabanını sorgulama, örnek veritabanı ve boş veritabanı oluşturma, veritabanı özelliklerini sorgulama, bağlanma ve örnek veritabanını sorgulama adımlarını gösteren bir öğreticiye ihtiyacınız varsa bkz. [Başlangıç Öğreticisi](sql-database-get-started-powershell.md).
>

## <a name="how-do-i-create-a-resource-group"></a>Nasıl kaynak grubu oluşturabilirim?
SQL veritabanınız ve ilgili Azure kaynakları için kaynak grubu oluşturmak istiyorsanız [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt759837\(v=azure.300\).aspx) cmdlet'ini kullanabilirsiniz.

```
$resourceGroupName = "resourcegroup1"
$resourceGroupLocation = "northcentralus"
New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
```

Daha fazla bilgi için bkz. [Azure PowerShell'i Azure Resource Manager ile kullanma](../powershell-azure-resource-manager.md).
Tam öğretici için bkz. [Azure PowerShell aracılığıyla Azure SQL Veritabanı sunucularını, veritabanlarını ve güvenlik duvarı kurallarını kullanmaya başlama](sql-database-get-started-powershell.md).

## <a name="how-do-i-create-a-sql-database-server"></a>Bir SQL veritabanı sunucusunu nasıl oluşturabilirim?
Bir SQL veritabanı sunucusu oluşturmak için [New-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603715\(v=azure.300\).aspx) cmdlet'ini kullanın. *server1* yerine sunucunuzun adını yazın. Sunucu adlarının tüm Azure SQL veritabanı sunucuları arasında benzersiz olması gerekir. Sunucu adı zaten alınmışsa hata alırsınız. Bu komutun tamamlanması birkaç dakika sürebilir. Kaynak grubunun aboneliğinizde mevcut olması gerekir.

```
$resourceGroupName = "resourcegroup1"

$sqlServerName = "server1"
$sqlServerVersion = "12.0"
$sqlServerLocation = "northcentralus"
$serverAdmin = "loginname"
$serverPassword = "password" 
$securePassword = ConvertTo-SecureString -String $serverPassword -AsPlainText -Force
$creds = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $serverAdmin, $securePassword


$sqlServer = New-AzureRmSqlServer -ServerName $sqlServerName `
 -SqlAdministratorCredentials $creds -Location $sqlServerLocation `
 -ResourceGroupName $resourceGroupName -ServerVersion $sqlServerVersion
```

Sunucular hakkında daha fazla bilgi için bkz. [SQL Veritabanı özellikleri](sql-database-features.md). Tam öğretici için bkz. [Azure PowerShell aracılığıyla Azure SQL Veritabanı sunucularını, veritabanlarını ve güvenlik duvarı kurallarını kullanmaya başlama](sql-database-get-started-powershell.md).

## <a name="how-do-i-create-a-sql-database-server-firewall-rule"></a>Bir SQL veritabanı sunucusu güvenlik duvarı kuralını nasıl oluşturabilirim?
Sunucuya erişmek için bir güvenlik duvarı kuralı oluşturmak istiyorsanız [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860\(v=azure.300\).aspx) cmdlet'ini kullanmanız gerekir. Başlangıç IP adresini ve bitiş IP adresini istemciniz için geçerli değerlerle değiştirerek aşağıdaki komutu çalıştırın. Kaynak grubunun ve sunucunun aboneliğinizde mevcut olması gerekir.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$firewallRuleName = "firewallrule1"
$firewallStartIp = "0.0.0.0"
$firewallEndIp = "255.255.255.255"

New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -FirewallRuleName $firewallRuleName `
 -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
```

Diğer Azure hizmetlerinin sunucunuza erişmesine izin vermek için bir güvenlik duvarı kuralı oluşturun ve hem `-StartIpAddress` hem de `-EndIpAddress` değerini **0.0.0.0** olarak ayarlayın. Bu özel güvenlik duvarı kuralı, sunucuya tüm Azure trafiğinin ulaşmasını sağlar.

Daha fazla bilgi için bkz. [Azure SQL Database Güvenlik Duvarı](https://msdn.microsoft.com/library/azure/ee621782.aspx). Tam öğretici için bkz. [Azure PowerShell aracılığıyla Azure SQL Veritabanı sunucularını, veritabanlarını ve güvenlik duvarı kurallarını kullanmaya başlama](sql-database-get-started-powershell.md).

## <a name="how-do-i-create-a-sql-database"></a>Bir SQL veritabanını nasıl oluşturabilirim?
Bir SQL veritabanı oluşturmak için [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339\(v=azure.300\).aspx) cmdlet'ini kullanın. Kaynak grubunun ve sunucunun aboneliğinizde mevcut olması gerekir. 

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Standard"
$databaseServiceLevel = "S0"

$currentDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Daha fazla bilgi için bkz. [SQL Veritabanı nedir?](sql-database-technical-overview.md). Tam öğretici için bkz. [Azure PowerShell aracılığıyla Azure SQL Veritabanı sunucularını, veritabanlarını ve güvenlik duvarı kurallarını kullanmaya başlama](sql-database-get-started-powershell.md).

## <a name="how-do-i-change-the-performance-level-of-a-sql-database"></a>Bir SQL veritabanının performans düzeyini nasıl değiştirebilirim?
Performans düzeyini değiştirmek için [Set-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433\(v=azure.300\).aspx) cmdlet'ini kullanarak veritabanınızı yukarı veya aşağı ölçeklendirin. Kaynak grubunun, sunucunun ve veritabanının aboneliğinizde mevcut olması gerekir. Temel katman için `-RequestedServiceObjectiveName` değerini tek boşluğa (aşağıdaki kod parçacığı gibi) ayarlayın. Diğer katmanlar için önceki örnekte olduğu gibi *S0*, *S1*, *P1*, *P6* gibi değerlere ayarlayın.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Basic"
$databaseServiceLevel = " "

Set-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Daha fazla bilgi için bkz. [SQL Veritabanı seçenekleri ve performansı: Her hizmet katmanında nelerin kullanılabildiğini anlama](sql-database-service-tiers.md). Örnek betik için bkz. [SQL veritabanınızın hizmet katmanını ve performans düzeyini değiştirmek için örnek PowerShell betiği](sql-database-scale-up-powershell.md#sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database).

## <a name="how-do-i-copy-a-sql-database-to-the-same-server"></a>Bir SQL veritabanını aynı sunucuya nasıl kopyalayabilirim?
Bir SQL veritabanını aynı sunucuya kopyalamak için [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/azure/mt603644\(v=azure.300\).aspx) cmdlet'ini kullanın. `-CopyServerName` ve `-CopyResourceGroupName` değerlerini kaynak veritabanı sunucunuz ve kaynak grubuyla aynı değerlere ayarlayın.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

$copyDatabaseName = "database1_copy"
$copyServerName = $sqlServerName
$copyResourceGroupName = $resourceGroupName

New-AzureRmSqlDatabaseCopy -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName `
 -CopyDatabaseName $copyDatabaseName -CopyServerName $sqlServerName `
 -CopyResourceGroupName $copyResourceGroupName
```

Daha fazla bilgi için bkz. [Azure SQL Veritabanını kopyalama](sql-database-copy.md). Örnek betik için bkz. [SQL veritabanını kopyalama PowerShell betiği](sql-database-copy-powershell.md#example-powershell-script).

## <a name="how-do-i-delete-a-sql-database"></a>Bir SQL veritabanını nasıl silebilirim?
Bir SQL veritabanını silmek için. [Remove-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619368\(v=azure.300\).aspx) cmdlet'ini kullanın. Kaynak grubunun, sunucunun ve veritabanının aboneliğinizde mevcut olması gerekir.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

Remove-AzureRmSqlDatabase -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="how-do-i-delete-a-sql-database-server"></a>Bir SQL veritabanını sunucusunu nasıl silebilirim?
Bir SQL veritabanı sunucusunu silmek için [Remove-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603488\(v=azure.300\).aspx) cmdlet'ini kullanın.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

Remove-AzureRmSqlServer -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="how-do-i-create-and-manage-elastic-pools-using-powershell"></a>PowerShell'i kullanarak elastik havuzları nasıl oluşturabilir ve yönetebilirim?
PowerShell kullanarak elastik havuz oluşturma hakkında ayrıntılar için bkz. [PowerShell ile yeni bir elastik havuz oluşturma](sql-database-elastic-pool-create-powershell.md).

PowerShell kullanarak elastik havuzları yönetme hakkında ayrıntılar için bkz. [PowerShell ile bir elastik havuzu izleme ve yönetme](sql-database-elastic-pool-manage-powershell.md).

## <a name="related-information"></a>İlgili bilgiler
* [Azure SQL Veritabanı Cmdlet’leri](https://msdn.microsoft.com/library/azure/mt574084\(v=azure.300\).aspx)
* [Azure Cmdlet Başvurusu](https://msdn.microsoft.com/library/azure/dn708514\(v=azure.300\).aspx)




<!--HONumber=Dec16_HO3-->


