---
title: "Azure Cosmos DB Otomasyon - Powershell ile Yönetimi | Microsoft Docs"
description: "Kullanım Azure PowerShell'i Azure Cosmos DB hesaplarınızı yönetin."
services: cosmos-db
author: dmakwana
manager: jhubbard
editor: 
tags: azure-resource-manager
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: dimakwan
ms.openlocfilehash: 3bdf30dad5e729ae1e028be2d917b6c38e1bebaf
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="create-an-azure-cosmos-db-account-using-powershell"></a>PowerShell kullanarak bir Azure Cosmos DB hesabı oluşturma

Aşağıdaki kılavuz, Azure Powershell kullanarak Azure Cosmos DB veritabanı hesaplarınızı yönetimini otomatikleştirmek için komutları açıklar. Ayrıca hesabı anahtarları ve yük devretme öncelikleri yönetmek için komutlar içerir [bölgeli veritabanı hesaplarını][scaling-globally]. Veritabanı hesabını güncelleştirme tutarlılık ilkeleri değiştirebilirsiniz ve bölgeler Ekle/Kaldır izin verir. Ya da kullanmak için platformlar arası yönetim Azure Cosmos DB hesabınızın [Azure CLI](cli-samples.md), [kaynak sağlayıcısı REST API][rp-rest-api], veya [Azure portal](create-documentdb-dotnet.md#create-account).

## <a name="getting-started"></a>Başlarken

' Ndaki yönergeleri izleyin [Azure PowerShell'i yükleme ve yapılandırma nasıl] [ powershell-install-configure] yükleme ve Powershell Azure Resource Manager hesabınızda oturum açın.

### <a name="notes"></a>Notlar

* İsterseniz kullanıcı onayı gerektirmeden aşağıdaki komutları yürütün, ekleme `-Force` bayrağı komutu.
* Aşağıdaki komutların tümünü zaman uyumlu.

## <a id="create-documentdb-account-powershell"></a>Bir Azure Cosmos DB hesabı oluşturma

Bu komut, bir Azure Cosmos DB veritabanı hesabı oluşturmanızı sağlar. Yeni veritabanı hesabınız ya da tek bölge yapılandırmak veya [bölgeli] [ scaling-globally] belirli bir ile [tutarlılık İlkesi](consistency-levels.md).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name>  -Location "<resource-group-location>" -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>`Veritabanı hesabı yazma bölgesini konum adı. Bu konum bir yük devretme öncelik değeri 0 olması gerekir. Veritabanı hesabı başına tam olarak bir yazma bölge olması gerekir.
* `<read-region-location>`Veritabanı hesabının okuma bölge konum adı. Bu konum yük devretme öncelik değeri 0'dan büyük olması gerekir. Veritabanı hesabı başına birden fazla okuma bölgeler olabilir.
* `<ip-range-filter>`İstemci IP verilen veritabanı hesabı için izin verilen listesi olarak dahil edilecek CIDR formunda IP adreslerini veya IP adresi aralıkları kümesini belirtir. IP adresleri/aralıklarına virgülle ayrılmış ve boşluk içermemelidir olması gerekir. Daha fazla bilgi için bkz: [Azure Cosmos DB güvenlik duvarı desteği](firewall-support.md)
* `<default-consistency-level>`Azure Cosmos DB hesap varsayılan tutarlılık düzeyi. Daha fazla bilgi için bkz: [Azure Cosmos veritabanı tutarlılık düzeylerini](consistency-levels.md).
* `<max-interval>`Sınırlanmış eskime durumu tutarlılığı ile kullanıldığında, bu değer izin süre miktarı (saniye cinsinden) eskime durumu temsil eder. Bu değer için kabul edilebilir aralık 1-100'dür.
* `<max-staleness-prefix>`Sınırlanmış eskime durumu tutarlılığı ile kullanıldığında, bu değer izin eski istek sayısını temsil eder. Bu değer için kabul edilebilir aralık 1 – 2,147,483,647 şeklindedir.
* `<resource-group-name>`Adını [Azure kaynak grubu] [ azure-resource-groups] yeni Azure Cosmos DB veritabanı hesabının ait olduğu.
* `<resource-group-location>`Yeni Azure Cosmos DB veritabanı hesabının ait olduğu Azure kaynak grubu konumu.
* `<database-account-name>`Oluşturulacak Azure Cosmos DB veritabanı hesabının adı. Yalnızca küçük harfler, rakamlar, kullanabilirsiniz '-' karakteri ve 3-50 karakter arasında olmalıdır.

Örnek: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    New-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Location "West US" -Name "docdb-test" -Properties $CosmosDBProperties

### <a name="notes"></a>Notlar
* Önceki örnekte ile iki bölgede bir veritabanı hesabı oluşturur. Veritabanı hesabı (hangi yazma bölge atanır ve bir yük devretme öncelik değeri 0 olan) bir bölge veya ikiden fazla bölgeleri ile oluşturmak mümkündür. Daha fazla bilgi için bkz: [bölgeli veritabanı hesaplarını][scaling-globally].
* Konumlarına bölgeler Azure Cosmos DB genel olarak kullanılabilir olması gerekir. Geçerli bölgelerin listesi üzerinde sağlanan [Azure bölgeleri sayfa](https://azure.microsoft.com/regions/#services).

## <a id="update-documentdb-account-powershell"></a>Bir Azure Cosmos DB veritabanı hesabını güncelleştirme

Bu komut, Azure Cosmos DB veritabanı hesabı özelliklerinizi güncelleştirmenizi sağlar. Bu tutarlılık ilke ve veritabanı hesabı bulunmaktadır konumları içerir.

> [!NOTE]
> Bu komut bölgeler ekleyip olanak tanır ancak yük devretme önceliklerini değiştirmek izin vermez. Yük devretme önceliklerini değiştirmek için bkz: [aşağıda](#modify-failover-priority-powershell).

    $locations = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0}, @{"locationName"="<read-region-location>"; "failoverPriority"=1})
    $iprangefilter = "<ip-range-filter>"
    $consistencyPolicy = @{"defaultConsistencyLevel"="<default-consistency-level>"; "maxIntervalInSeconds"="<max-interval>"; "maxStalenessPrefix"="<max-staleness-prefix>"}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <resource-group-name> -Name <database-account-name> -Properties $CosmosDBProperties
    
* `<write-region-location>`Veritabanı hesabı yazma bölgesini konum adı. Bu konum bir yük devretme öncelik değeri 0 olması gerekir. Veritabanı hesabı başına tam olarak bir yazma bölge olması gerekir.
* `<read-region-location>`Veritabanı hesabının okuma bölge konum adı. Bu konum yük devretme öncelik değeri 0'dan büyük olması gerekir. Veritabanı hesabı başına birden fazla okuma bölgeler olabilir.
* `<default-consistency-level>`Azure Cosmos DB hesap varsayılan tutarlılık düzeyi. Daha fazla bilgi için bkz: [Azure Cosmos veritabanı tutarlılık düzeylerini](consistency-levels.md).
* `<ip-range-filter>`İstemci IP verilen veritabanı hesabı için izin verilen listesi olarak dahil edilecek CIDR formunda IP adreslerini veya IP adresi aralıkları kümesini belirtir. IP adresleri/aralıklarına virgülle ayrılmış ve boşluk içermemelidir olması gerekir. Daha fazla bilgi için bkz: [Azure Cosmos DB güvenlik duvarı desteği](firewall-support.md)
* `<max-interval>`Sınırlanmış eskime durumu tutarlılığı ile kullanıldığında, bu değer izin süre miktarı (saniye cinsinden) eskime durumu temsil eder. Bu değer için kabul edilebilir aralık 1-100'dür.
* `<max-staleness-prefix>`Sınırlanmış eskime durumu tutarlılığı ile kullanıldığında, bu değer izin eski istek sayısını temsil eder. Bu değer için kabul edilebilir aralık 1 – 2,147,483,647 şeklindedir.
* `<resource-group-name>`Adını [Azure kaynak grubu] [ azure-resource-groups] yeni Azure Cosmos DB veritabanı hesabının ait olduğu.
* `<resource-group-location>`Yeni Azure Cosmos DB veritabanı hesabının ait olduğu Azure kaynak grubu konumu.
* `<database-account-name>`Güncelleştirilecek Azure Cosmos DB veritabanı hesabının adı.

Örnek: 

    $locations = @(@{"locationName"="West US"; "failoverPriority"=0}, @{"locationName"="East US"; "failoverPriority"=1})
    $iprangefilter = ""
    $consistencyPolicy = @{"defaultConsistencyLevel"="BoundedStaleness"; "maxIntervalInSeconds"=5; "maxStalenessPrefix"=100}
    $CosmosDBProperties = @{"databaseAccountOfferType"="Standard"; "locations"=$locations; "consistencyPolicy"=$consistencyPolicy; "ipRangeFilter"=$iprangefilter}
    Set-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Properties $CosmosDBProperties

## <a id="delete-documentdb-account-powershell"></a>Bir Azure Cosmos DB veritabanı hesabını silme

Bu komut, var olan bir Azure Cosmos DB veritabanı hesabını silmenize olanak sağlar.

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"
    
* `<resource-group-name>`Adını [Azure kaynak grubu] [ azure-resource-groups] yeni Azure Cosmos DB veritabanı hesabının ait olduğu.
* `<database-account-name>`Silinecek Azure Cosmos DB veritabanı hesabının adı.

Örnek:

    Remove-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="get-documentdb-properties-powershell"></a>Bir Azure Cosmos DB veritabanı hesabının özelliklerini al

Bu komut, var olan bir Azure Cosmos DB veritabanı hesabının özelliklerini almak sağlar.

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`Adını [Azure kaynak grubu] [ azure-resource-groups] yeni Azure Cosmos DB veritabanı hesabının ait olduğu.
* `<database-account-name>`Azure Cosmos DB veritabanı hesabının adı.

Örnek:

    Get-AzureRmResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="update-tags-powershell"></a>Bir Azure Cosmos DB veritabanı hesabını güncelleştirme etiketleri

Aşağıdaki örnekte nasıl ayarlanacağını açıklar [Azure kaynak etiketleri] [ azure-resource-tags] Azure Cosmos DB için veritabanı hesabı.

> [!NOTE]
> Bu komut ekleyerek oluştur veya Güncelleştir komutları ile birleştirilebilir `-Tags` bayrağı karşılık gelen bir parametre ile.

Örnek:

    $tags = @{"dept" = "Finance”; environment = “Production”}
    Set-AzureRmResource -ResourceType “Microsoft.DocumentDB/databaseAccounts”  -ResourceGroupName "rg-test" -Name "docdb-test" -Tags $tags

## <a id="list-account-keys-powershell"></a>Liste hesabı anahtarları

Bir Azure Cosmos DB hesabı oluşturduğunuzda, hizmet Azure Cosmos DB hesap erişildiğinde, kimlik doğrulaması için kullanılan iki ana erişim tuşu oluşturur. İki erişim tuşu sağlayarak Azure Cosmos DB kesinti olmadan Azure Cosmos DB hesabınıza anahtarları yeniden sağlar. Salt okunur işlemler kimlik doğrulaması için salt okunur anahtar de kullanılabilir. Var. iki okuma-yazma anahtarları (birincil ve ikincil) ve iki salt okunur anahtarları (birincil ve ikincil)

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`Adını [Azure kaynak grubu] [ azure-resource-groups] yeni Azure Cosmos DB veritabanı hesabının ait olduğu.
* `<database-account-name>`Azure Cosmos DB veritabanı hesabının adı.

Örnek:

    $keys = Invoke-AzureRmResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="list-connection-strings-powershell"></a>Liste bağlantı dizeleri

MongoDB hesapları için aşağıdaki komutu kullanarak MongoDB uygulamanızı veritabanı hesabınıza bağlanmak için bağlantı dizesi alınabilir.

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>"

* `<resource-group-name>`Adını [Azure kaynak grubu] [ azure-resource-groups] yeni Azure Cosmos DB veritabanı hesabının ait olduğu.
* `<database-account-name>`Azure Cosmos DB veritabanı hesabının adı.

Örnek:

    $keys = Invoke-AzureRmResourceAction -Action listConnectionStrings -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test"

## <a id="regenerate-account-key-powershell"></a>Hesap anahtarını yeniden oluşturma

Düzenli aralıklarla bağlantılarınızı daha güvenli tutmaya yardımcı olmak için Azure Cosmos DB hesabınıza erişim tuşlarını değiştirmeniz gerekir. Bir erişim anahtarı kullanırken, bir erişim anahtarı yeniden Azure Cosmos DB hesabına bağlantılar sağlamanıza olanak tanıyan iki erişim tuşu atanır.

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"keyKind"="<key-kind>"}

* `<resource-group-name>`Adını [Azure kaynak grubu] [ azure-resource-groups] yeni Azure Cosmos DB veritabanı hesabının ait olduğu.
* `<database-account-name>`Azure Cosmos DB veritabanı hesabının adı.
* `<key-kind>`Anahtarların dört türlerinden birini: ["Birincil" | " İkincil "|" PrimaryReadonly "|" SecondaryReadonly"], yeniden oluşturmak istiyor musunuz.

Örnek:

    Invoke-AzureRmResourceAction -Action regenerateKey -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"keyKind"="Primary"}

## <a id="modify-failover-priority-powershell"></a>Bir Azure Cosmos DB veritabanı hesabının yük devretme öncelik değiştirme

Bölgeli veritabanı hesapları için de Azure Cosmos DB veritabanı hesabı var olan çeşitli bölgelere yük devretme önceliğini değiştirebilirsiniz. Azure Cosmos DB veritabanı hesabınızda yük devretme hakkında daha fazla bilgi için bkz: [Azure Cosmos DB genel verilerle dağıtmak][distribute-data-globally].

    $failoverPolicies = @(@{"locationName"="<write-region-location>"; "failoverPriority"=0},@{"locationName"="<read-region-location>"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "<resource-group-name>" -Name "<database-account-name>" -Parameters @{"failoverPolicies"=$failoverPolicies}

* `<write-region-location>`Veritabanı hesabı yazma bölgesini konum adı. Bu konum bir yük devretme öncelik değeri 0 olması gerekir. Veritabanı hesabı başına tam olarak bir yazma bölge olması gerekir.
* `<read-region-location>`Veritabanı hesabının okuma bölge konum adı. Bu konum yük devretme öncelik değeri 0'dan büyük olması gerekir. Veritabanı hesabı başına birden fazla okuma bölgeler olabilir.
* `<resource-group-name>`Adını [Azure kaynak grubu] [ azure-resource-groups] yeni Azure Cosmos DB veritabanı hesabının ait olduğu.
* `<database-account-name>`Azure Cosmos DB veritabanı hesabının adı.

Örnek:

    $failoverPolicies = @(@{"locationName"="East US"; "failoverPriority"=0},@{"locationName"="West US"; "failoverPriority"=1})
    Invoke-AzureRmResourceAction -Action failoverPriorityChange -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName "rg-test" -Name "docdb-test" -Parameters @{"failoverPolicies"=$failoverPolicies}

## <a name="next-steps"></a>Sonraki adımlar

* .NET kullanarak bağlanmak için bkz: [bağlanma ve sorgu .NET ile](create-documentdb-dotnet.md).
* .NET Core kullanarak bağlanmak için bkz: [Connect ve .NET Core sorguyla](create-documentdb-dotnet-core.md).
* Node.js kullanarak bağlanmak için bkz: [Connect ve Node.js ve MongoDB uygulama sorguyla](create-mongodb-nodejs.md).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[powershell-install-configure]: https://docs.microsoft.com/azure/powershell-install-configure
[scaling-globally]: distribute-data-globally.md#EnableGlobalDistribution
[distribute-data-globally]: distribute-data-globally.md
[azure-resource-groups]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups
[azure-resource-tags]: https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags
[rp-rest-api]: https://docs.microsoft.com/rest/api/documentdbresourceprovider/
