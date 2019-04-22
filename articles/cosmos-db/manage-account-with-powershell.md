---
title: PowerShell kullanarak Azure Cosmos DB kaynaklarını oluşturmak ve yönetmek
description: Azure Powershell kullanarak Azure Cosmos DB hesaplarınızı yönetin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: 01c351ad08399c0b42e831e325b3f818741d1d83
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58904381"
---
# <a name="manage-azure-cosmos-resources-using-powershell"></a>PowerShell kullanarak Azure Cosmos kaynaklarını yönetme

Aşağıdaki kılavuzda, Azure Powershell kullanarak Azure Cosmos DB veritabanı hesaplarınızı yönetimini otomatikleştirmek için komutları açıklanır. Ayrıca bir hesap anahtarları ve yük devretme önceliklerini olarak yönetmek için komutlar içerir [çoklu bölge veritabanı hesapları][distribute-data-globally]. Veritabanı hesabınız güncelleştiriliyor, tutarlılık ilkeleri değiştirebilirsiniz ve bölge Ekle/Kaldır olanak tanır. Azure Cosmos DB hesabınızın platformlar arası yönetimi için ya da kullanabilirsiniz [Azure CLI](cli-samples.md), [kaynak sağlayıcısı REST API'si][rp-rest-api], veya [Azure portalı ](create-sql-api-dotnet.md#create-account).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="getting-started"></a>Başlarken

Bölümündeki yönergeleri [Azure PowerShell'i yükleme ve yapılandırma işlemini] [ powershell-install-configure] yükleme ve Azure Resource Manager Powershell hesabınızda oturum açın.

### <a name="notes"></a>Notlar

* Kullanıcı onayı gerekmeden aşağıdaki komutları yürütün, eklemek istediğiniz, `-Force` bayrağı komutu.
* Aşağıdaki komutlar zaman uyumlu.

## <a id="create-documentdb-account-powershell"></a> Bir Azure Cosmos DB hesabı oluşturma

Bu komut, bir Azure Cosmos DB veritabanı hesabı oluşturmanıza olanak sağlar. Yeni veritabanı hesabınız ya da tek bölge yapılandırın veya [çok bölgeli] [ distribute-data-globally] ile belirli bir [tutarlılık İlkesi](consistency-levels.md).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name>  -Location "<resource-group-location>" -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>` Veritabanı hesabı yazma bölgesi konum adı. Bu konum, bir yük devretme öncelik değeri 0 sahip olması gereklidir. Veritabanı hesabı başına tam olarak bir yazma bölgesi olması gerekir.
* `<read-region-location>` Veritabanı hesabı okuma bölgesi konum adı. Bu konum yük devretme öncelik değeri 0'dan büyük olması gerekiyor. Veritabanı hesabı başına birden çok okuma bölgesini olabilir.
* `<ip-range-filter>` İstemci Ip'lerine belirli bir veritabanı hesabı için izin verilen listesi olarak dahil edilecek CIDR formunda IP adresleri veya IP adresi aralıkları kümesini belirtir. IP adresi/aralığı, virgülle ayrılmış ve boşluk içermemelidir olması gerekir. Daha fazla bilgi için [Azure Cosmos DB güvenlik duvarı desteği](firewall-support.md)
* `<default-consistency-level>` Azure Cosmos DB hesabının varsayılan tutarlılık düzeyi. Daha fazla bilgi için [Azure Cosmos DB'deki tutarlılık düzeyleri](consistency-levels.md).
* `<max-interval>` Sınırlanmış eskime durumu tutarlılık ile kullanıldığında, bu değer kaydırmadan kaçınma şansınız süresi (saniye cinsinden) eskime durumu temsil eder. Bu değer için kabul edilebilir aralık 1-100'dür.
* `<max-staleness-prefix>` Sınırlanmış eskime durumu tutarlılık ile kullanıldığında, bu değer kaydırmadan kaçınma şansınız eski istek sayısını temsil eder. Bu değer için kabul edilebilir aralık 1 – 2,147,483,647 şeklindedir.
* `<resource-group-name>` Adını [Azure kaynak grubu] [ azure-resource-groups] yeni Azure Cosmos DB veritabanı hesabına ait olduğu.
* `<resource-group-location>` Yeni Azure Cosmos DB veritabanı hesabının ait olduğu Azure kaynak grubu konumu.
* `<database-account-name>` Oluşturulacak Azure Cosmos DB veritabanı hesabının adı. Yalnızca küçük harf, rakam, kullanabileceğiniz '-' karakteri ve 3 ila 50 karakter arasında olmalıdır.

Örnek: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Location "West US" -Name "docdb-test" -Properties $CosmosDBProperties

### <a name="notes"></a>Notlar
* Yukarıdaki örnekte, iki bölgeleri ile bir veritabanı hesabı oluşturur. (Bu yazma bölgesi atanır ve yük devretme öncelik değeri 0 olan) tek bir bölge veya iki bölgeleri ile bir veritabanı hesabı oluşturmanız da mümkündür. Daha fazla bilgi için [çoklu bölge veritabanı hesapları][distribute-data-globally].
* Azure Cosmos DB genel olarak kullanılabildiği bölgeler konumları olmalıdır. Geçerli bölgelerin listesi üzerinde sağlanan [Azure bölgeleri sayfa](https://azure.microsoft.com/regions/#services).

## <a id="update-documentdb-account-powershell"></a> Bir Azure Cosmos DB veritabanı hesabını güncelleştirme

Bu komut, Azure Cosmos DB veritabanı hesabı özelliklerinizi güncelleştirmenizi sağlar. Bu tutarlılık İlkesi ve veritabanı hesabı var. konumları içerir.

> [!NOTE]
> Bu komut bölgeleri ekleyip izin verir, ancak yük devretme önceliklerini değiştirmeye izin vermez. Yük devretme önceliklerini değiştirmek için bkz: [aşağıda](#modify-failover-priority-powershell).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name> -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>` Veritabanı hesabı yazma bölgesi konum adı. Bu konum, bir yük devretme öncelik değeri 0 sahip olması gereklidir. Veritabanı hesabı başına tam olarak bir yazma bölgesi olması gerekir.
* `<read-region-location>` Veritabanı hesabı okuma bölgesi konum adı. Bu konum yük devretme öncelik değeri 0'dan büyük olması gerekiyor. Veritabanı hesabı başına birden çok okuma bölgesini olabilir.
* `<default-consistency-level>` Azure Cosmos DB hesabının varsayılan tutarlılık düzeyi. Daha fazla bilgi için [Azure Cosmos DB'deki tutarlılık düzeyleri](consistency-levels.md).
* `<ip-range-filter>` İstemci Ip'lerine belirli bir veritabanı hesabı için izin verilen listesi olarak dahil edilecek CIDR formunda IP adresleri veya IP adresi aralıkları kümesini belirtir. IP adresi/aralığı, virgülle ayrılmış ve boşluk içermemelidir olması gerekir. Daha fazla bilgi için [Azure Cosmos DB güvenlik duvarı desteği](firewall-support.md)
* `<max-interval>` Sınırlanmış eskime durumu tutarlılık ile kullanıldığında, bu değer kaydırmadan kaçınma şansınız süresi (saniye cinsinden) eskime durumu temsil eder. Bu değer için kabul edilebilir aralık 1-100'dür.
* `<max-staleness-prefix>` Sınırlanmış eskime durumu tutarlılık ile kullanıldığında, bu değer kaydırmadan kaçınma şansınız eski istek sayısını temsil eder. Bu değer için kabul edilebilir aralık 1 – 2,147,483,647 şeklindedir.
* `<resource-group-name>` Adını [Azure kaynak grubu] [ azure-resource-groups] yeni Azure Cosmos DB veritabanı hesabına ait olduğu.
* `<resource-group-location>` Yeni Azure Cosmos DB veritabanı hesabının ait olduğu Azure kaynak grubu konumu.
* `<database-account-name>` Güncelleştirilecek Azure Cosmos DB veritabanı hesabının adı.

Örnek: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Properties $CosmosDBProperties

## <a id="delete-documentdb-account-powershell"></a> Bir Azure Cosmos DB veritabanı hesabını Sil

Bu komut var olan bir Azure Cosmos DB veritabanı hesabını silmenize olanak sağlar.

    Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"
    
* `<resource-group-name>` Adını [Azure kaynak grubu] [ azure-resource-groups] yeni Azure Cosmos DB veritabanı hesabına ait olduğu.
* `<database-account-name>` Azure Cosmos DB veritabanı hesabının silinmesi adı.

Örnek:

    Remove-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="get-documentdb-properties-powershell"></a> Bir Azure Cosmos DB veritabanı hesabı özelliklerini alma

Bu komut, Azure Cosmos DB veritabanı hesabınız özelliklerini alır sağlar.

    Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>` Adını [Azure kaynak grubu] [ azure-resource-groups] yeni Azure Cosmos DB veritabanı hesabına ait olduğu.
* `<database-account-name>` Azure Cosmos DB veritabanı hesabının adıdır.

Örnek:

    Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="update-tags-powershell"></a> Bir Azure Cosmos DB veritabanı hesabını güncelleştirme etiketleri

Aşağıdaki örnek nasıl ayarlanacağı açıklanır [Azure kaynak etiketleri] [ azure-resource-tags] için Azure Cosmos DB veritabanı hesabı.

> [!NOTE]
> Bu komut oluşturma veya güncelleştirme komutlarla ekleyerek birleştirilebilir `-Tags` bayrağıyla karşılık gelen parametre.

Örnek:

    $tags = @{"dept" = "Finance"; environment = "Production"}
    Set-AzResource -ResourceType "Microsoft.DocumentDB/databaseAccounts"  -ResourceGroupName "rg-test" -Name "docdb-test" -Tags $tags

## <a id="list-account-keys-powershell"></a> Hesap anahtarlarını Listele

Azure Cosmos DB hesabı oluşturduğunuzda, hizmet, Azure Cosmos DB hesabına erişim sağlandığında kimlik doğrulaması için kullanılan iki ana erişim anahtarları oluşturur. İki erişim tuşu sağlayarak, Azure Cosmos DB, kesinti olmadan Azure Cosmos DB hesabınız için anahtarları yeniden sağlar. Salt okunur anahtarlar, salt okunur işlemler kimlik doğrulaması için de kullanılabilir. Var. iki okuma-yazma anahtarları (birincil ve ikincil) ve iki salt okunur anahtarlar (birincil ve ikincil)

    $keys = Invoke-AzResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>` Adını [Azure kaynak grubu] [ azure-resource-groups] yeni Azure Cosmos DB veritabanı hesabına ait olduğu.
* `<database-account-name>` Azure Cosmos DB veritabanı hesabının adıdır.

Örnek:

    $keys = Invoke-AzResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="list-connection-strings-powershell"></a> Liste bağlantı dizeleri

MongoDB uygulamanızı hesabınıza veritabanı hesabına bağlanacak bağlantı dizesini, MongoDB hesabı için aşağıdaki komutu kullanarak alınabilir.

    $keys = Invoke-AzResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>` Adını [Azure kaynak grubu] [ azure-resource-groups] yeni Azure Cosmos DB veritabanı hesabına ait olduğu.
* `<database-account-name>` Azure Cosmos DB veritabanı hesabının adıdır.

Örnek:

    $keys = Invoke-AzResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="regenerate-account-key-powershell"></a> Hesap anahtarını yeniden oluştur

Düzenli aralıklarla bağlantılarınızı daha güvenli olmasına yardımcı olmak için Azure Cosmos DB hesabınızın erişim anahtarlarını değiştirmeniz gerekir. Bir erişim anahtarı yeniden oluşturmak, bir erişim anahtarı kullanarak Azure Cosmos DB hesabına bağlantıları sağlamak için iki erişim tuşu atanır.

    Invoke-AzResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"keyKind"="<key-kind>"}

* `<resource-group-name>` Adını [Azure kaynak grubu] [ azure-resource-groups] yeni Azure Cosmos DB veritabanı hesabına ait olduğu.
* `<database-account-name>` Azure Cosmos DB veritabanı hesabının adıdır.
* `<key-kind>` Dört türlerinden anahtar: ["Birincil" | " İkincil "|" PrimaryReadonly "|" SecondaryReadonly"] yeniden oluşturmak istediğiniz.

Örnek:

    Invoke-AzResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"keyKind"="Primary"}

## <a id="modify-failover-priority-powershell"></a> Bir Azure Cosmos DB veritabanı hesabını yük devretme önceliklerini değiştirebilir

Çoklu bölge veritabanı hesapları için Azure Cosmos DB veritabanı hesabı, var olan çeşitli bölgelere yük devretme önceliğini değiştirebilirsiniz. Azure Cosmos DB veritabanı hesabınızda yük devretme hakkında daha fazla bilgi için bkz. [verileri Azure Cosmos DB ile küresel olarak dağıtma][distribute-data-globally].

    $failoverPolicies = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0},@{"locationName"="<read-region-location>"; "failoverPriority"=1})
    Invoke-AzResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"failoverPolicies"=$failoverPolicies}

* `<write-region-location>` Veritabanı hesabı yazma bölgesi konum adı. Bu konum, bir yük devretme öncelik değeri 0 sahip olması gereklidir. Veritabanı hesabı başına tam olarak bir yazma bölgesi olması gerekir.
* `<read-region-location>` Veritabanı hesabı okuma bölgesi konum adı. Bu konum yük devretme öncelik değeri 0'dan büyük olması gerekiyor. Veritabanı hesabı başına birden çok okuma bölgesini olabilir.
* `<resource-group-name>` Adını [Azure kaynak grubu] [ azure-resource-groups] yeni Azure Cosmos DB veritabanı hesabına ait olduğu.
* `<database-account-name>` Azure Cosmos DB veritabanı hesabının adıdır.

Örnek:

    $failoverPolicies = @(@{"locationName"="East US"; "failoverPriority"=0},@{"locationName"="West US"; "failoverPriority"=1})
    Invoke-AzResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"failoverPolicies"=$failoverPolicies}

## <a name="next-steps"></a>Sonraki adımlar

* .NET kullanarak bağlanmak için bkz: [.NET ile bağlanma ve sorgulama](create-sql-api-dotnet.md).
* Node.js kullanarak bağlanmak için bkz: [Node.js ve MongoDB uygulaması ile bağlanma ve sorgulama](create-mongodb-nodejs.md).

<!--Reference style links - using these makes the source content way more readable than using inline links-->

[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/
