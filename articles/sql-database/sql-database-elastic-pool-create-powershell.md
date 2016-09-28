<properties
    pageTitle="PowerShell ile yeni bir esnek veritabanı havuzu oluşturma | Microsoft Azure"
    description="Birden fazla veritabanını yönetmek üzere ölçeklenebilir bir esnek veritabanı havuzu oluşturarak Azure SQL Database kaynaklarının ölçeklerini genişletmek üzere PowerShell'i nasıl kullanacağınızı öğrenin."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="05/27/2016"
    ms.author="srinia"/>


# PowerShell ile yeni bir esnek veritabanı havuzu oluşturma

> [AZURE.SELECTOR]
- [Azure Portal](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


PowerShell cmdlet'lerini kullanarak [esnek veritabanı havuzu](sql-database-elastic-pool.md) oluşturmayı öğrenin. 

Genel hata kodları için bkz. [SQL Database istemci uygulamaları için SQL hata kodları: Veritabanı bağlantı hatası ve diğer sorunlar](sql-database-develop-error-messages.md).

> [AZURE.NOTE] Esnek havuzlar şu anda önizleme aşamasında oldukları Orta Kuzey ABD ve Batı Hindistan dışında tüm Azure bölgelerinde genel olarak kullanılabilir (GA) durumdadır.  Bu bölgelerdeki esnek havuzların GA durumları en kısa sürede bildirilecektir. Ayrıca, esnek havuzlar şu anda [bellek içi OLTP veya bellek içi analiz](sql-database-in-memory.md) kullanan veritabanlarını desteklememektedir.


Azure PowerShell 1.0 sürümünü veya sonraki bir sürümünü çalıştırmanız gerekir. Ayrıntılı bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](../powershell-install-configure.md).

## Yeni bir havuz oluşturma

[New-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt619378.aspx) cmdlet'i yeni bir havuz oluşturur. Havuz başına eDTU değerlerinin yanı sıra minimum ve maksimum DTU değerleri, hizmet katmanı değerine (temel, standart veya premium) göre kısıtlanır. Bkz. [Esnek havuzlar ve esnek veritabanları için eDTU ve depolama sınırları](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases).

    New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100


## Havuzda yeni bir esnek veritabanı oluşturma

[New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx) cmdlet komutunu kullanın ve hedef havuz için **ElasticPoolName** parametresini ayarlayın. Var olan bir veritabanını bir havuza taşımak için bkz. [Veritabanını bir esnek havuza taşıma](sql-database-elastic-pool-manage-powershell.md#Move-a-database-into-an-elastic-pool).

    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## Havuz oluşturma ve birden çok veritabanıyla doldurma 

Bir havuzda çok sayıda veritabanı oluşturma işlemi, tek seferde yalnızca bir veritabanı oluşturan portal veya PowerShell cmdlet'leri kullanılarak gerçekleştirildiğinde uzun sürebilir. Oluşturma işlemini yeni bir havuzda gerçekleşecek şekilde otomatikleştirmek için bkz. [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).   

## Örnek: PowerShell'i kullanarak havuz oluşturma 

Bu betik, yeni bir Azure kaynak grubu ve yeni bir sunucu oluşturur. İstendiğinde, yeni sunucu için bir yönetici kullanıcı adı ve parola (Azure kimlik bilgilerinizi değil) sağlayın.

    $subscriptionId = '<your Azure subscription id>'
    $resourceGroupName = '<resource group name>'
    $location = '<datacenter location>'
    $serverName = '<server name>'
    $poolName = '<pool name>'
    $databaseName = '<database name>'

    Login-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $subscriptionId

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $location -ServerVersion "12.0"
    New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName "rule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

    New-AzureRmSqlElasticPool -ResourceGroupName $resourceGroupName -ServerName $serverName -ElasticPoolName $poolName -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100

    New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -ElasticPoolName $poolName -MaxSizeBytes 10GB



## Sonraki adımlar

- [Havuzunuzu yönetme](sql-database-elastic-pool-manage-powershell.md)
- [Esnek iş oluşturma](sql-database-elastic-jobs-overview.md): Esnek işler, havuzda bulunan herhangi bir sayıdaki veritabanı için T-SQL betiklerini çalıştırmanızı sağlar.
- [Azure SQL Database ile ölçek genişletme](sql-database-elastic-scale-introduction.md): Ölçek genişletmek için esnek veritabanı araçlarını kullanın.




<!--HONumber=Sep16_HO3-->


