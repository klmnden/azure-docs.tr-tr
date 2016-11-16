---
title: "PowerShell ile yeni bir esnek veritabanı havuzu oluşturma | Microsoft Belgeleri"
description: "Birden fazla veritabanını yönetmek üzere ölçeklenebilir bir esnek veritabanı havuzu oluşturarak Azure SQL Database kaynaklarının ölçeklerini genişletmek üzere PowerShell&quot;i nasıl kullanacağınızı öğrenin."
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 37a707ee-9223-43ae-8c35-1ccafde8b83e
ms.service: sql-database
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: powershell
ms.workload: data-management
ms.date: 05/27/2016
ms.author: srinia
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: daf8bd6421ae563e542b0874a6e7748a3ca52738


---
# <a name="create-a-new-elastic-database-pool-with-powershell"></a>PowerShell ile yeni bir esnek veritabanı havuzu oluşturma
> [!div class="op_single_selector"]
> * [Azure Portal](sql-database-elastic-pool-create-portal.md)
> * [PowerShell](sql-database-elastic-pool-create-powershell.md)
> * [C#](sql-database-elastic-pool-create-csharp.md)
> 
> 

PowerShell cmdlet'lerini kullanarak [esnek veritabanı havuzu](sql-database-elastic-pool.md) oluşturmayı öğrenin. 

Genel hata kodları için bkz. [SQL Database istemci uygulamaları için SQL hata kodları: Veritabanı bağlantı hatası ve diğer sorunlar](sql-database-develop-error-messages.md).

> [!NOTE]
> Esnek havuzlar şu anda önizleme aşamasında oldukları Orta Kuzey ABD ve Batı Hindistan dışında tüm Azure bölgelerinde genel olarak kullanılabilir (GA) durumdadır.  Bu bölgelerdeki esnek havuzların GA durumları en kısa sürede bildirilecektir. Ayrıca, esnek havuzlar şu anda [bellek içi OLTP veya bellek içi analiz](sql-database-in-memory.md) kullanan veritabanlarını desteklememektedir.
> 
> 

Azure PowerShell 1.0 sürümünü veya sonraki bir sürümünü çalıştırmanız gerekir. Ayrıntılı bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](../powershell-install-configure.md).

## <a name="create-a-new-pool"></a>Yeni bir havuz oluşturma
[New-AzureRmSqlElasticPool](https://msdn.microsoft.com/library/azure/mt619378\(v=azure.300\).aspx) cmdlet'i yeni bir havuz oluşturur. Havuz başına eDTU değerlerinin yanı sıra minimum ve maksimum DTU değerleri, hizmet katmanı değerine (temel, standart veya premium) göre kısıtlanır. Bkz. [Esnek havuzlar ve esnek veritabanları için eDTU ve depolama sınırları](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases).

    New-AzureRmSqlElasticPool -ResourceGroupName "resourcegroup1" -ServerName "server1" -ElasticPoolName "elasticpool1" -Edition "Standard" -Dtu 400 -DatabaseDtuMin 10 -DatabaseDtuMax 100


## <a name="create-a-new-elastic-database-in-a-pool"></a>Havuzda yeni bir esnek veritabanı oluşturma
[New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339\(v=azure.300\).aspx) cmdlet komutunu kullanın ve hedef havuz için **ElasticPoolName** parametresini ayarlayın. Var olan bir veritabanını bir havuza taşımak için bkz. [Veritabanını bir esnek havuza taşıma](sql-database-elastic-pool-manage-powershell.md#Move-a-database-into-an-elastic-pool).

    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroup1" -ServerName "server1" -DatabaseName "database1" -ElasticPoolName "elasticpool1"

## <a name="create-a-pool-and-populate-it-with-multiple-new-databases"></a>Havuz oluşturma ve birden çok veritabanıyla doldurma
Bir havuzda çok sayıda veritabanı oluşturma işlemi, tek seferde yalnızca bir veritabanı oluşturan portal veya PowerShell cmdlet'leri kullanılarak gerçekleştirildiğinde uzun sürebilir. Oluşturma işlemini yeni bir havuzda gerçekleşecek şekilde otomatikleştirmek için bkz. [CreateOrUpdateElasticPoolAndPopulate](https://gist.github.com/billgib/d80c7687b17355d3c2ec8042323819ae).   

## <a name="example-create-a-pool-using-powershell"></a>Örnek: PowerShell'i kullanarak havuz oluşturma
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



## <a name="next-steps"></a>Sonraki adımlar
* [Havuzunuzu yönetme](sql-database-elastic-pool-manage-powershell.md)
* [Esnek iş oluşturma](sql-database-elastic-jobs-overview.md): Esnek işler, havuzda bulunan herhangi bir sayıdaki veritabanı için T-SQL betiklerini çalıştırmanızı sağlar.
* [Azure SQL Database ile ölçek genişletme](sql-database-elastic-scale-introduction.md): Ölçek genişletmek için esnek veritabanı araçlarını kullanın.




<!--HONumber=Nov16_HO2-->


