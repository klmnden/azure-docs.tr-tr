---
title: Oluşturma ve Azure Cosmos DB PowerShell kullanarak yönetme
description: Azure Powershell kullanarak, Azure Cosmos DB hesapları, veritabanları, kapsayıcıları ve aktarım hızı yönetin.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/09/2019
ms.author: mjbrown
ms.custom: seodec18
ms.openlocfilehash: b61c7bbc06d8d265e5dd5dddd31aceadce1f623b
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797049"
---
# <a name="manage-azure-cosmos-db-sql-api-resources-using-powershell"></a>PowerShell kullanarak Azure Cosmos DB SQL API kaynaklarını yönetme

Aşağıdaki kılavuzda, PowerShell komut dosyasını kullanın ve hesabı, veritabanı, kapsayıcı ve aktarım hızı dahil olmak üzere, Azure Cosmos DB kaynaklarını yönetimini otomatikleştirmek açıklar. Azure Cosmos DB'nin yönetim AzResource cmdlet'e doğrudan Azure Cosmos DB kaynak sağlayıcısı aracılığıyla işlenir. Tüm Azure Cosmos DB kaynak sağlayıcısı için PowerShell kullanılarak yönetilebilir özelliklerini görüntülemek için bkz: [Azure Cosmos DB kaynak sağlayıcısı şeması](/azure/templates/microsoft.documentdb/allversions)

Azure Cosmos DB, platformlar arası yönetimi için kullandığınız [Azure CLI](manage-with-cli.md), [REST API][rp-rest-api], veya [Azure portalında](create-sql-api-dotnet.md#create-account).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="getting-started"></a>Başlarken

Bölümündeki yönergeleri [Azure PowerShell'i yükleme ve yapılandırma işlemini][powershell-install-configure] yükleyip PowerShell'de Azure hesabınızda oturum açın.

* Kullanıcı onayı gerekmeden aşağıdaki komutları yürütün, eklemek istediğiniz, `-Force` bayrağı komutu.
* Aşağıdaki komutlar zaman uyumlu.

## <a name="azure-cosmos-accounts"></a>Azure Cosmos hesapları

Aşağıdaki bölümlerde Azure Cosmos hesabın nasıl yönetileceği gösterilmektedir dahil olmak üzere:

* [Bir Azure Cosmos hesabı oluşturma](#create-account)
* [Bir Azure Cosmos hesabı güncelleştirme](#update-account)
* [Bir Abonelikteki tüm Azure Cosmos hesaplarını listeleme](#list-accounts)
* [Bir Azure Cosmos hesabı edinin](#get-account)
* [Bir Azure Cosmos hesabını Sil](#delete-account)
* [Bir Azure Cosmos hesap için etiketleri güncelleştirin](#update-tags)
* [Bir Azure Cosmos hesap anahtarlarını Listele](#list-keys)
* [Azure Cosmos hesabınız için anahtarları yeniden oluştur](#regenerate-keys)
* [Azure Cosmos hesabınız için bağlantı dizelerini listesi](#list-connection-strings)
* [Bir Azure Cosmos hesap için yük devretme önceliklerini değiştirebilir](#modify-failover-priority)

### <a id="create-account"></a> Bir Azure Cosmos hesabı oluşturma

Bu komut, bir Azure Cosmos DB veritabanı hesabıyla oluşturur [birden çok bölgede][distribute-data-globally], sınırlanmış eskime [tutarlılık İlkesi](consistency-levels.md).

```azurepowershell-interactive
# Create an Azure Cosmos Account for Core (SQL) API
$resourceGroupName = "myResourceGroup"
$location = "West US 2"
$accountName = "mycosmosaccount" # must be lower case.

$locations = @(
    @{ "locationName"="West US 2"; "failoverPriority"=0 },
    @{ "locationName"="East US 2"; "failoverPriority"=1 }
)

$consistencyPolicy = @{
    "defaultConsistencyLevel"="BoundedStaleness";
    "maxIntervalInSeconds"=300;
    "maxStalenessPrefix"=100000
}

$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$locations;
    "consistencyPolicy"=$consistencyPolicy;
    "enableMultipleWriteLocations"="false"
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Location $location `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

* `$accountName` Azure Cosmos hesabı adı. Küçük harf, alfasayısal kabul eder ve '-' karakteri ve 3 ila 31 karakter.
* `$location` Azure Cosmos hesabı kaynağı için konum.
* `$locations` Veritabanı hesabı için çoğaltma bölgeleri. Bir yük devretme öncelik değeri 0 ile veritabanı hesabı başına bir yazma bölgesi olması gerekir.
* `$consistencyPolicy` Azure Cosmos hesabı varsayılan tutarlılık düzeyi. Daha fazla bilgi için [Azure Cosmos DB'deki tutarlılık düzeyleri](consistency-levels.md).
* `$CosmosDBProperties` Özellik değerleri, Cosmos DB Azure Resource Manager hesabı sağlama sağlayıcısına geçirildi.

Sanal ağ yanı sıra IP Güvenlik Duvarı ile hesapları yapılandırılabilir azure Cosmos uç noktaları hizmeti. Azure Cosmos DB için IP Güvenlik Duvarı yapılandırma hakkında daha fazla bilgi için bkz: [IP Güvenlik Duvarı Yapılandırma](how-to-configure-firewall.md).  Azure Cosmos DB için hizmet uç noktalarını etkinleştirme hakkında daha fazla bilgi için bkz. [sanal ağlardan erişimi yapılandırma](how-to-configure-vnet-service-endpoint.md).

### <a id="list-accounts"></a> Bir Abonelikteki tüm Azure Cosmos hesaplarını listeleme

Bu komutu bir Abonelikteki tüm Azure Cosmos hesabında listelemenize olanak sağlar.

```azurepowershell-interactive
# List Azure Cosmos Accounts

Get-AzResource -ResourceType Microsoft.DocumentDb/databaseAccounts | ft
```

### <a id="get-account"></a> Bir Azure Cosmos hesap özelliklerini alma

Bu komut, mevcut bir Azure Cosmos hesabı özelliklerini alır sağlar.

```azurepowershell-interactive
# Get the properties of an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName | Select-Object Properties
```

### <a id="update-account"></a> Bir Azure Cosmos hesabı güncelleştirme

Bu komut, Azure Cosmos DB veritabanı hesabı özelliklerinizi güncelleştirmenizi sağlar. Güncelleştirilebilir özellikleri şunlardır:

* Ekleme veya kaldırma bölgeleri
* Varsayılan tutarlılık ilkesini değiştirme
* Yük devretme İlkesi değiştirme
* IP aralığı filtresi değiştirme
* Sanal ağ yapılandırmasını değiştirme
* Çok yöneticili etkinleştirme

> [!NOTE]
> Bu komut bölgeleri ekleyip izin verir, ancak yük devretme önceliklerini değiştirmeye izin vermez. Yük devretme önceliğini değiştirmek için bkz: [bir Azure Cosmos hesap için yük devretme önceliklerini değiştirebilir](#modify-failover-priority).

```azurepowershell-interactive
# Update an Azure Cosmos Account and set Consistency level to Session

$resourceGroupName = "myResourceGroup"
$accountName = "myaccountname"

$account = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $accountName

$consistencyPolicy = @{ "defaultConsistencyLevel"="Session" }

$account.Properties.consistencyPolicy = $consistencyPolicy
$CosmosDBProperties = $account.Properties

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -PropertyObject $CosmosDBProperties
```

### <a id="delete-account"></a> Bir Azure Cosmos hesabını Sil

Bu komut var olan bir Azure Cosmos hesabını silmenize olanak sağlar.

```azurepowershell-interactive
# Delete an Azure Cosmos Account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName
```

### <a id="update-tags"></a> Azure Cosmos hesabın etiketleri güncelleştirin

Aşağıdaki örnek nasıl ayarlanacağı açıklanır [Azure kaynak etiketleri][azure-resource-tags] için bir Azure Cosmos hesabı.

> [!NOTE]
> Bu komut oluşturma veya güncelleştirme komutlarla ekleyerek birleştirilebilir `-Tags` bayrağıyla karşılık gelen parametre.

```azurepowershell-interactive
# Update tags for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$tags = @{
    "dept" = "Finance";
    "environment" = "Production"
}

Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $accountName -Tags $tags
```

### <a id="list-keys"></a> Hesap anahtarlarını Listele

Azure Cosmos DB hesabı oluşturduğunuzda, hizmet, Azure Cosmos DB hesabına erişim sağlandığında kimlik doğrulaması için kullanılan iki ana erişim anahtarları oluşturur. İki erişim tuşu sağlayarak, Azure Cosmos DB, kesinti olmadan Azure Cosmos DB hesabınız için anahtarları yeniden sağlar. Salt okunur anahtarlar, salt okunur işlemler kimlik doğrulaması için de kullanılabilir. Var. iki okuma-yazma anahtarları (birincil ve ikincil) ve iki salt okunur anahtarlar (birincil ve ikincil)

```azurepowershell-interactive
# List keys for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$keys = Invoke-AzResourceAction -Action listKeys `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName

Select-Object $keys
```

### <a id="list-connection-strings"></a> Liste bağlantı dizeleri

MongoDB uygulamanızı hesabınıza veritabanı hesabına bağlanacak bağlantı dizesini, MongoDB hesabı için aşağıdaki komutu kullanarak alınabilir.

```azurepowershell-interactive
# List connection strings for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$keys = Invoke-AzResourceAction -Action listConnectionStrings `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName

Select-Object $keys
```

### <a id="regenerate-keys"></a> Hesap anahtarlarını yeniden oluştur

Bağlantılar daha güvenli olmasını sağlamak için bir Azure Cosmos hesap erişim anahtarlarını düzenli aralıklarla yeniden. Birincil ve ikincil erişim anahtarlarını hesabına atanır. Bu, diğer yeniden sırada erişimi sürdürmek etmesine olanak tanır. Anahtarlar (birincil, ikincil, PrimaryReadonly ve SecondaryReadonly) bir Azure Cosmos hesap için dört çeşit vardır.

```azurepowershell-interactive
# Regenerate the primary key for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$keyKind = @{ "keyKind"="Primary" }

$keys = Invoke-AzResourceAction -Action regenerateKey `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName -Parameters $keyKind

Select-Object $keys
```

### <a id="modify-failover-priority"></a> Yük devretme önceliklerini değiştirebilir

Çoklu bölge veritabanı hesapları için bölgesel yük devretme yazma birincil Çoğaltmada gerçekleşmelidir ikincil okuma çoğaltmaları Cosmos hesabı yükseltmez sırasını değiştirebilirsiniz. Zaman bölgeyle `failoverPriority=0` olan değiştirilmiş, bu komut ayrıca olağanüstü durum kurtarma planlaması test etmek için bir olağanüstü durum kurtarma tatbikatı başlatmak için kullanılabilir.

Aşağıdaki örnekte, varsayar için hesabın geçerli bir yük devretme öncelik westus, sahip = 0 ve eastus = 1 ve bölge çevir.

> [!CAUTION]
> Değiştirme `locationName` için `failoverPriority=0` el ile bir yük devretme için bir Azure Cosmos hesabı tetikler. Herhangi bir öncelik değişiklik, bir yük devretme işlemini tetiklemez.

```azurepowershell-interactive
# Change the failover priority for an Azure Cosmos Account

$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"

$failoverPolicies = @(
    @{ "locationName"="East US"; "failoverPriority"=0 },
    @{ "locationName"="West US"; "failoverPriority"=1 }
)

Invoke-AzResourceAction -Action failoverPriorityChange `
    -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" `
    -ResourceGroupName $resourceGroupName -Name $accountName -Parameters $failoverPolicies
```

## <a name="azure-cosmos-database"></a>Azure Cosmos veritabanı

Aşağıdaki bölümlerde, Azure Cosmos veritabanı, yönetilecek göstermektedir dahil olmak üzere:

* [Bir Azure Cosmos veritabanı oluşturma](#create-db)
* [Paylaşılan aktarım hızı ile bir Azure Cosmos veritabanı oluşturma](#create-db-ru)
* [Bir Azure Cosmos veritabanı, aktarım hızı alma](#get-db-ru)
* [Bir hesaptaki tüm Azure Cosmos veritabanlarını listeleyin](#list-db)
* [Tek bir Azure Cosmos veritabanı Al](#get-db)
* [Bir Azure Cosmos veritabanı Sil](#delete-db)

### <a id="create-db"></a>Bir Azure Cosmos veritabanı oluşturma

```azurepowershell-interactive
# Create an Azure Cosmos database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName

$DataBaseProperties = @{
    "resource"=@{"id"=$databaseName}
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $DataBaseProperties
```

### <a id="create-db-ru"></a>Paylaşılan aktarım hızı ile bir Azure Cosmos veritabanı oluşturma

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database2"
$resourceName = $accountName + "/sql/" + $databaseName

$DataBaseProperties = @{
    "resource"=@{ "id"=$databaseName };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $DataBaseProperties
```

### <a id="get-db-ru"></a>Bir Azure Cosmos veritabanı, aktarım hızı alma

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$databaseThroughputResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/databases/settings"
$databaseThroughputResourceName = $accountName + "/sql/" + $databaseName + "/throughput"

Get-AzResource -ResourceType $databaseThroughputResourceType `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $databaseThroughputResourceName  | Select-Object Properties
```

### <a id="list-db"></a>Tüm Azure Cosmos veritabanı içinde bir hesap alın

```azurepowershell-interactive
# Get all databases in an Azure Cosmos account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$resourceName = $accountName + "/sql/"

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName  | Select-Object Properties
```

### <a id="get-db"></a>Tek bir Azure Cosmos veritabanı Al

```azurepowershell-interactive
# Get a single database in an Azure Cosmos account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName | Select-Object Properties
```

### <a id="delete-db"></a>Bir Azure Cosmos veritabanı Sil

```azurepowershell-interactive
# Delete a database in an Azure Cosmos account
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName
Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $resourceName
```

## <a name="azure-cosmos-container"></a>Azure Cosmos kapsayıcı

Aşağıdaki bölümlerde Azure Cosmos kapsayıcısı yönetme göstermek dahil olmak üzere:

* [Bir Azure Cosmos kapsayıcısı oluşturma](#create-container)
* [Büyük bir bölüm anahtarı ile bir Azure Cosmos kapsayıcısı oluşturma](#create-container-big-pk)
* [Bir Azure Cosmos kapsayıcısının aktarım hızı alma](#get-container-ru)
* [Paylaşılan aktarım hızı ile bir Azure Cosmos kapsayıcısı oluşturma](#create-container-ru)
* [Özel dizin ile bir Azure Cosmos kapsayıcısı oluşturma](#create-container-custom-index)
* [Kapalı dizin ile bir Azure Cosmos kapsayıcısı oluşturma](#create-container-no-index)
* [Benzersiz anahtar ve TTL ile bir Azure Cosmos kapsayıcısı oluşturma](#create-container-unique-key-ttl)
* [Çakışma çözümü ile bir Azure Cosmos kapsayıcısı oluşturma](#create-container-lww)
* [Bir veritabanındaki tüm Azure Cosmos kapsayıcıları listesi](#list-containers)
* [Bir veritabanında tek bir Azure Cosmos kapsayıcısı Al](#get-container)
* [Bir Azure Cosmos kapsayıcısını silme](#delete-container)

### <a id="create-container"></a>Bir Azure Cosmos kapsayıcısı oluşturma

```azurepowershell-interactive
# Create an Azure Cosmos container with default indexes and throughput at 400 RU
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName;
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        }
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-big-pk"></a>Bir büyük bölüm anahtar boyutu ile bir Azure Cosmos kapsayıcısı oluşturma

```azurepowershell-interactive
# Create an Azure Cosmos container with a large partition key value (version = 2)
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName;
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash";
            "version" = 2
        }
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="get-container-ru"></a>Bir Azure Cosmos kapsayıcısının aktarım hızı alma

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$containerThroughputResourceType = "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers/settings"
$containerThroughputResourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName + "/throughput"

Get-AzResource -ResourceType $containerThroughputResourceType `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $containerThroughputResourceName  | Select-Object Properties
```

### <a id="create-container-ru"></a>Paylaşılan aktarım hızı ile bir Azure Cosmos kapsayıcısı oluşturma

```azurepowershell-interactive
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container2"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        }
    };
    "options"=@{}
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties 
```

### <a id="create-container-custom-index"></a>Özel dizin İlkesi ile bir Azure Cosmos kapsayıcısı oluşturma

```azurepowershell-interactive
# Create a container with a custom indexing policy
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container3"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey"); 
            "kind"="Hash"
        }; 
        "indexingPolicy"=@{
            "indexingMode"="Consistent";
            "includedPaths"= @(@{
                "path"="/*";
                "indexes"= @()
            });
            "excludedPaths"= @(@{
                "path"="/myPathToNotIndex/*"
            })
        }
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-no-index"></a>Kapalı dizin ile bir Azure Cosmos kapsayıcısı oluşturma

```azurepowershell-interactive
# Create an Azure Cosmos container with no indexing
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container4"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName; 
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        };
        "indexingPolicy"=@{
            "indexingMode"="none"
        }
    };
    "options"=@{ "Throughput"="400" }
} 

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-unique-key-ttl"></a>Benzersiz anahtar ilkesi ve TTL ile bir Azure Cosmos kapsayıcısı oluşturma

```azurepowershell-interactive
# Create a container with a unique key policy and TTL
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container5"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName;
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        }; 
        "indexingPolicy"=@{
            "indexingMode"="Consistent";
            "includedPaths"= @(@{
                "path"="/*";
                "indexes"= @()
            });
            "excludedPaths"= @()
        };
        "uniqueKeyPolicy"= @{
            "uniqueKeys"= @(@{
                "paths"= @(
                    "/myUniqueKey1";
                    "/myUniqueKey2"
                )
            })
        };
        "defaultTtl"= 100;
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="create-container-lww"></a>Çakışma çözümü ile bir Azure Cosmos kapsayıcısı oluşturma

Bir saklı yordam kullanmak için bir çakışma çözüm ilkesi oluşturmak için `"mode"="custom"` ve saklı yordam adı olarak çözümleme yolu ayarla `"conflictResolutionPath"="myResolverStoredProcedure"`. Tüm çakışmaları için ConflictsFeed yazma ve ayrı olarak işlemek için ayarlanmış `"mode"="custom"` ve `"conflictResolutionPath"=""`

```azurepowershell-interactive
# Create container with last-writer-wins conflict resolution policy
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container6"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

$ContainerProperties = @{
    "resource"=@{
        "id"=$containerName;
        "partitionKey"=@{
            "paths"=@("/myPartitionKey");
            "kind"="Hash"
        };
        "conflictResolutionPolicy"=@{
            "mode"="lastWriterWins";
            "conflictResolutionPath"="/myResolutionPath"
        }
    };
    "options"=@{ "Throughput"="400" }
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName -PropertyObject $ContainerProperties
```

### <a id="list-containers"></a>Bir veritabanındaki tüm Azure Cosmos kapsayıcıları listesi

```azurepowershell-interactive
# List all Azure Cosmos containers in a database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$resourceName = $accountName + "/sql/" + $databaseName

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName | Select-Object Properties
```

### <a id="get-container"></a>Bir veritabanında tek bir Azure Cosmos kapsayıcısı Al

```azurepowershell-interactive
# Get a single Azure Cosmos container in a database
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container5"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName `
    -Name $resourceName | Select-Object Properties
```

### <a id="delete-container"></a>Bir Azure Cosmos kapsayıcısını silme

```azurepowershell-interactive
# Delete an Azure Cosmos container
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$databaseName = "database1"
$containerName = "container1"
$resourceName = $accountName + "/sql/" + $databaseName + "/" + $containerName

Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts/apis/databases/containers" `
    -ApiVersion "2015-04-08" -ResourceGroupName $resourceGroupName -Name $resourceName
```

## <a name="next-steps"></a>Sonraki adımlar

* [Tüm PowerShell örnekleri](powershell-samples.md)
* [Azure Cosmos hesabını yönetme](how-to-manage-database-account.md)
* [Bir Azure Cosmos kapsayıcısı oluşturma](how-to-create-container.md)
* [Azure Cosmos DB'de yaşam süresi yapılandırma](how-to-time-to-live.md)

<!--Reference style links - using these makes the source content way more readable than using inline links-->

[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/
