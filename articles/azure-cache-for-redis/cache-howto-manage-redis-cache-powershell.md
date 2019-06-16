---
title: Azure önbelleği için Redis Azure PowerShell ile yönetme | Microsoft Docs
description: Azure Cache için Azure PowerShell kullanarak Redis için yönetim görevlerini gerçekleştirmeyi öğreneceksiniz.
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 1136efe5-1e33-4d91-bb49-c8e2a6dca475
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: yegu
ms.openlocfilehash: f7f4f9ae6a80052e06b2cafa68cb5c11dfa1333a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62097938"
---
# <a name="manage-azure-cache-for-redis-with-azure-powershell"></a>Azure önbelleği için Redis Azure PowerShell ile yönetme
> [!div class="op_single_selector"]
> * [PowerShell](cache-howto-manage-redis-cache-powershell.md)
> * [Azure CLI](cache-manage-cli.md)
> 
> 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu konuda, oluşturma gibi ortak görevleri gerçekleştirmek için nasıl güncelleştirmek ve, Azure önbelleği için Redis önbelleklerinizi hakkındaki bilgileri görüntülemek nasıl örnekleri ve erişim anahtarlarını yeniden oluşturmak nasıl ölçeklendirme gösterilmektedir. Bir tam listesi Azure önbelleği için Redis PowerShell cmdlet'leri için bkz: [Azure önbelleği için Redis cmdlet'leri](https://docs.microsoft.com/powershell/module/az.rediscache).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

Klasik dağıtım modeli hakkında daha fazla bilgi için bkz: [Azure Resource Manager ve klasik dağıtım: Dağıtım modellerini ve kaynaklarınızın durumunu anlama](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="prerequisites"></a>Önkoşullar
Azure PowerShell'i zaten yüklediyseniz, Azure PowerShell sürüm 1.0.0 olmalıdır veya üzeri. Azure PowerShell, Azure PowerShell komut isteminde bu komutu ile yüklü olan sürümü denetleyebilirsiniz.

    Get-Module Az | format-table version


İlk olarak, Azure'a bu komutla oturum açmalısınız.

    Connect-AzAccount

Microsoft Azure oturum açma iletişim kutusunda Azure hesabınızı ve kendi parola e-posta adresini belirtin.

Ardından, birden çok Azure aboneliğiniz varsa, Azure aboneliğinizi ayarlama gerekir. Geçerli aboneliklerinizin bir listesini görmek için şu komutu çalıştırın.

    Get-AzSubscription | sort SubscriptionName | Select SubscriptionName

Aboneliği belirtmek için aşağıdaki komutu çalıştırın. Aşağıdaki örnekte, abonelik addır `ContosoSubscription`.

    Select-AzSubscription -SubscriptionName ContosoSubscription

Azure Resource Manager ile Windows PowerShell kullanmadan önce aşağıdakiler gerekir:

* Windows PowerShell sürüm 3.0 veya 4.0. Windows PowerShell sürümünü bulmak için şunu yazın:`$PSVersionTable` ve değerini doğrulayın `PSVersion` 3.0 veya 4.0. Uyumlu bir sürümünü yüklemek için bkz: [Windows Management Framework 3.0](https://www.microsoft.com/download/details.aspx?id=34595) veya [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855).

Bu öğreticide gördüğünüz herhangi bir cmdlet'in ayrıntılı yardım almak için Get-Help cmdlet'ini kullanın.

    Get-Help <cmdlet-name> -Detailed

Örneğin, Yardım almak için `New-AzRedisCache` cmdlet'i, türü:

    Get-Help New-AzRedisCache -Detailed

### <a name="how-to-connect-to-other-clouds"></a>Diğer bulutlara bağlanma
Azure varsayılan olarak ortamıdır `AzureCloud`, Azure genel bulut örneği temsil eder. Farklı bir örneğine bağlanmak için `Connect-AzAccount` komutunu `-Environment` veya -`EnvironmentName` komut satırı anahtarıyla istenen ortama veya ortam adı.

Kullanabileceğiniz ortamların listesini görmek için şunu çalıştırın `Get-AzEnvironment` cmdlet'i.

### <a name="to-connect-to-the-azure-government-cloud"></a>Azure kamu Bulutuna bağlanmak için
Azure kamu Bulutuna bağlanmak için aşağıdaki komutlardan birini kullanın.

    Connect-AzAccount -EnvironmentName AzureUSGovernment

or

    Connect-AzAccount -Environment (Get-AzEnvironment -Name AzureUSGovernment)

Azure kamu bulutunda bir önbellek oluşturmak için aşağıdaki konumlardan birini kullanın.

* USGov Virginia
* USGov Iowa

Azure kamu Bulutu hakkında daha fazla bilgi için bkz: [Microsoft Azure kamu](https://azure.microsoft.com/features/gov/) ve [Microsoft Azure kamu Geliştirici Kılavuzu](../azure-government-developer-guide.md).

### <a name="to-connect-to-the-azure-china-cloud"></a>Azure Çin buluta bağlama
Azure Çin buluta bağlamak için aşağıdaki komutlardan birini kullanın.

    Connect-AzAccount -EnvironmentName AzureChinaCloud

or

    Connect-AzAccount -Environment (Get-AzEnvironment -Name AzureChinaCloud)

Azure Çin bulutunda bir önbellek oluşturmak için aşağıdaki konumlardan birini kullanın.

* Çin Doğu
* Çin Kuzey

Azure Çin bulut hakkında daha fazla bilgi için bkz. [AzureChinaCloud için Azure tarafından Çin'de 21Vianet tarafından işletilen](http://www.windowsazure.cn/).

### <a name="to-connect-to-microsoft-azure-germany"></a>Microsoft Azure Almanya için bağlanmak için
Microsoft Azure Almanya için bağlanmak için aşağıdaki komutlardan birini kullanın.

    Connect-AzAccount -EnvironmentName AzureGermanCloud


or

    Connect-AzAccount -Environment (Get-AzEnvironment -Name AzureGermanCloud)

Microsoft Azure Almanya'da bir önbellek oluşturmak için aşağıdaki konumlardan birini kullanın.

* Almanya Orta
* Almanya Kuzeydoğu

Microsoft Azure Almanya hakkında daha fazla bilgi için bkz: [Microsoft Azure Almanya](https://azure.microsoft.com/overview/clouds/germany/).

### <a name="properties-used-for-azure-cache-for-redis-powershell"></a>Azure önbelleği için Redis PowerShell için kullanılan özellikleri
Aşağıdaki tabloda, özellikleri ve oluşturma ve yönetme, Azure önbelleği için Redis örneği Azure PowerShell kullanarak, yaygın olarak kullanılan parametre açıklamalarını içerir.

| Parametre | Açıklama | Varsayılan |
| --- | --- | --- |
| Ad |Önbellek adı | |
| Location |Önbellek konumu | |
| ResourceGroupName |Önbellek oluşturulacağı kaynak grubu adı | |
| Boyut |Önbellek boyutu. Geçerli değerler şunlardır: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2,5 GB, 6 GB, 13 GB, 26 GB, 53 GB |1 GB |
| ShardCount |Premium önbellek kümeleme özellikli oluştururken oluşturmak için parça sayısı. Geçerli değerler şunlardır: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 | |
| SKU |Önbelleğinin SKU belirtir. Geçerli değerler şunlardır: Temel, Standart, Premium |Standart |
| RedisConfiguration |Redis yapılandırma ayarlarını belirtir. Her ayar hakkında daha fazla bilgi için aşağıdakilere bakın [RedisConfiguration özellikleri](#redisconfiguration-properties) tablo. | |
| EnableNonSslPort |SSL olmayan bağlantı noktasının etkinleştirilip etkinleştirilmeyeceğini gösterir. |False |
| MaxMemoryPolicy |Bu parametre kullanım - RedisConfiguration kullanın. | |
| StaticIP |Bir vnet'teki önbelleğinizi barındırırken, benzersiz bir IP adresi alt ağ için önbellek belirtir. Sağlanmazsa, bir, alt ağdan seçilir. | |
| Alt ağ |Bir vnet'teki önbelleğinizi barındırırken, önbellek dağıtacağınız bir alt ağ adını belirtir. | |
| VirtualNetwork |Bir vnet'teki önbelleğinizi barındırırken, önbellek dağıtacağınız sanal ağın kaynak Kimliğini belirtir. | |
| KeyType |Erişim anahtarlarını yenilenirken yeniden oluşturmak için hangi erişim anahtarını belirtir. Geçerli değerler şunlardır: Birincil, ikincil | |

### <a name="redisconfiguration-properties"></a>RedisConfiguration properties
| Özellik | Açıklama | Fiyatlandırma katmanları |
| --- | --- | --- |
| RDB yedekleme etkin |Olmadığını [Redis veri kalıcılığı](cache-how-to-premium-persistence.md) etkin |Yalnızca Premium |
| RDB depolama bağlantı dizesi |Depolama hesabı için bağlantı dizesi [Redis veri kalıcılığı](cache-how-to-premium-persistence.md) |Yalnızca Premium |
| RDB yedekleme sıklığı |Yedekleme sıklığı için [Redis veri kalıcılığı](cache-how-to-premium-persistence.md) |Yalnızca Premium |
| maxmemory-ayrılmış |Yapılandırır [ayrılan bellek](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) önbellek olmayan işlemler için |Standart ve Premium |
| Maks bellek politikası |Yapılandırır [çıkarma İlkesi](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) önbelleği |Tüm fiyatlandırma katmanları |
| bildirim-anahtar alanı-olayları |Yapılandırır [anahtar alanı bildirimleri](cache-configure.md#keyspace-notifications-advanced-settings) |Standart ve Premium |
| en fazla ziplist girişlerini karması |Yapılandırır [bellek iyileştirme](https://redis.io/topics/memory-optimization) küçük toplam veri türleri |Standart ve Premium |
| en fazla ziplist değeri karması |Yapılandırır [bellek iyileştirme](https://redis.io/topics/memory-optimization) küçük toplam veri türleri |Standart ve Premium |
| en fazla intset Girişlerini Ayarla |Yapılandırır [bellek iyileştirme](https://redis.io/topics/memory-optimization) küçük toplam veri türleri |Standart ve Premium |
| zset-max-ziplist-entries |Yapılandırır [bellek iyileştirme](https://redis.io/topics/memory-optimization) küçük toplam veri türleri |Standart ve Premium |
| zset-max-ziplist-value |Yapılandırır [bellek iyileştirme](https://redis.io/topics/memory-optimization) küçük toplam veri türleri |Standart ve Premium |
| veritabanları |Veritabanı sayısı yapılandırır. Bu özellik yalnızca önbellek oluşturma sırasında yapılandırılabilir. |Standart ve Premium |

## <a name="to-create-an-azure-cache-for-redis"></a>Bir Azure önbelleği için Redis oluşturmak için
Yeni Azure önbelleği için Redis örneği kullanılarak oluşturulur [yeni AzRedisCache](https://docs.microsoft.com/powershell/module/az.rediscache/new-azrediscache) cmdlet'i.

> [!IMPORTANT]
> İlk kez bir Azure önbelleği için Redis Azure portalını kullanarak bir aboneliği oluşturmak, portal kaydeder `Microsoft.Cache` Bu abonelik için ad alanı. İlk Azure önbelleği için Redis PowerShell kullanarak bir Abonelikteki oluşturmaya çalışırsanız, aşağıdaki komutu kullanarak bu ad alanı kaydetmeniz gerekir; Aksi takdirde cmdlet'leri gibi `New-AzRedisCache` ve `Get-AzRedisCache` başarısız.
> 
> `Register-AzResourceProvider -ProviderNamespace "Microsoft.Cache"`
> 
> 

Kullanılabilir parametrelerin ve Tanımlamaların için bir listesini görmek için `New-AzRedisCache`, aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help New-AzRedisCache -detailed

    NAME
        New-AzRedisCache

    SYNOPSIS
        Creates a new Azure Cache for Redis.


    SYNTAX
        New-AzRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]


    DESCRIPTION
        The New-AzRedisCache cmdlet creates a new Azure Cache for Redis.


    PARAMETERS
        -Name <String>
            Name of the Azure Cache for Redis to create.

        -ResourceGroupName <String>
            Name of resource group in which to create the Azure Cache for Redis.

        -Location <String>
            Location in which to create the Azure Cache for Redis.

        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.

        -Size <String>
            Size of the Azure Cache for Redis. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of Azure Cache for Redis. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Cache for Redis. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the Azure Cache for Redis in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}

        -Subnet <String>
            Required when deploying an Azure Cache for Redis inside an existing Azure Virtual Network.

        -StaticIP <String>
            Required when deploying an Azure Cache for Redis inside an existing Azure Virtual Network.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

Varsayılan parametreleri bir önbellek oluşturmak için aşağıdaki komutu çalıştırın.

    New-AzRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, ve `Location` Gerekli Parametreler, ancak geri kalan isteğe bağlıdır ve varsayılan değerlere sahip. Önceki komut çalıştıran standart bir SKU Azure Cache Redis örneği için SSL olmayan bağlantı noktası devre dışı boyutu 1 GB olan belirtilen ad, konum ve kaynak grubu oluşturur.

Premium önbellek oluşturmak, P1'in (6 GB - 60 GB), ö2 (13 GB - 130 GB'a), bir boyut belirlemek için P3 (26 GB - 260 GB) veya P4 (53 GB - 530 GB). Kümeleme'ı etkinleştirmek üzere, kullanarak bir parça sayısı belirtin `ShardCount` parametresi. Aşağıdaki örnek, P1 premium önbellek ile 3 parçalar oluşturur. P1 premium önbellek 6 GB boyutundadır ve üç parça belirlemiş bu yana toplam boyutu 18 (3 x 6 GB) GB'dir.

    New-AzRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

Değerlerini belirtmek için `RedisConfiguration` parametresi içindeki değerleri içine `{}` anahtar/değer çiftleri ister `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. Aşağıdaki örnek, bir standart 1 GB önbellek ile oluşturur `allkeys-random` maxmemory İlkesi ve anahtar alanı bildirimleri ile yapılandırılmış `KEA`. Daha fazla bilgi için [(Gelişmiş ayarlar) anahtar alanı bildirimleri](cache-configure.md#keyspace-notifications-advanced-settings) ve [bellek ilkeleri](cache-configure.md#memory-policies).

    New-AzRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>

## <a name="to-configure-the-databases-setting-during-cache-creation"></a>Önbellek oluşturma işlemi sırasında ayarlama veritabanlarını yapılandırmak için
`databases` Ayarı yalnızca önbellek oluşturma işlemi sırasında yapılandırılabilir. Aşağıdaki örnek, bir premium P3 oluşturur (26 GB) önbellek kullanarak 48 veritabanlarıyla [yeni AzRedisCache](https://docs.microsoft.com/powershell/module/az.rediscache/New-azRedisCache) cmdlet'i.

    New-AzRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

Daha fazla bilgi için `databases` özelliğine bakın [varsayılan Azure önbelleği için Redis sunucu yapılandırması](cache-configure.md#default-redis-server-configuration). Kullanarak bir önbellek oluşturma hakkında daha fazla bilgi için [yeni AzRedisCache](https://docs.microsoft.com/powershell/module/az.rediscache/new-azrediscache) cmdlet'in önceki bir Azure önbelleği için Redis bölüm oluşturmak için bkz.

## <a name="to-update-an-azure-cache-for-redis"></a>Bir Azure önbelleği için Redis güncelleştirmek için
Azure önbelleği için Redis örneği kullanılarak güncelleştirilir [kümesi AzRedisCache](https://docs.microsoft.com/powershell/module/az.rediscache/Set-azRedisCache) cmdlet'i.

Kullanılabilir parametrelerin ve Tanımlamaların için bir listesini görmek için `Set-AzRedisCache`, aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Set-AzRedisCache -detailed

    NAME
        Set-AzRedisCache

    SYNOPSIS
        Set Azure Cache for Redis updatable parameters.

    SYNTAX
        Set-AzRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]

    DESCRIPTION
        The Set-AzRedisCache cmdlet sets Azure Cache for Redis parameters.

    PARAMETERS
        -Name <String>
            Name of the Azure Cache for Redis to update.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        -Size <String>
            Size of the Azure Cache for Redis. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of Azure Cache for Redis. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Cache for Redis. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

`Set-AzRedisCache` Cmdlet'i gibi özellikleri güncelleştirmek için kullanılabilir `Size`, `Sku`, `EnableNonSslPort`ve `RedisConfiguration` değerleri. 

Aşağıdaki komut politikası myCache adlı Redis için Azure Cache için güncelleştirir.

    Set-AzRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>

## <a name="to-scale-an-azure-cache-for-redis"></a>Bir Azure önbelleği için Redis ölçeklendirme
`Set-AzRedisCache` bir Azure önbelleği için Redis ölçeklendirmek için kullanılabilir olduğunda örnek `Size`, `Sku`, veya `ShardCount` özellikleri değiştirilemez. 

> [!NOTE]
> PowerShell kullanarak bir önbellek ölçeklendirme aynı sınırlamaları ve yönergeleri, Azure portalından bir önbellek ölçeklendirme olarak tabidir. Aşağıdaki kısıtlamalarla birlikte farklı bir fiyatlandırma katmanına ölçeklendirebilirsiniz.
> 
> * Daha yüksek bir fiyatlandırma Katmanı'ndan daha düşük bir fiyatlandırma katmanına ölçeklendirilemez.
> * Ölçek genişletilemiyor bir **Premium** aşağı önbelleğe bir **standart** veya **temel** önbellek.
> * Ölçek genişletilemiyor bir **standart** aşağı önbelleğe bir **temel** önbellek.
> * Değerine ölçeklendirme bir **temel** için önbellek bir **standart** önbellek ancak değiştiremez boyutu aynı anda. Başka bir boyutu gerekiyorsa, istediğiniz boyuta izleyen bir ölçeklendirme işlemi yapabilirsiniz.
> * Ölçek genişletilemiyor bir **temel** doğrudan önbelleğe bir **Premium** önbellek. Değerine ölçeklendirme **temel** için **standart** bir ölçeklendirme işlemi ve ardından gelen **standart** için **Premium** , sonraki ölçeklendirme işlem.
> * Büyük bir değerden aşağı ölçeklendirilemez **C0 (250 MB)** boyutu.
> 
> Daha fazla bilgi için [ölçek Azure Cache, Redis için nasıl](cache-how-to-scale.md).
> 
> 

Aşağıdaki örnekte adlı önbellek ölçeklendirme gösterilmiştir `myCache` 2,5 GB önbellek için. Bu komut bir temel veya standart bir önbellek için çalıştığını unutmayın.

    Set-AzRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Bu komut verildikten sonra önbellek durumu döndürülür (arama benzer `Get-AzRedisCache`). Unutmayın `ProvisioningState` olduğu `Scaling`.

    PS C:\> Set-AzRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB


    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

Ölçeklendirme işlemi tamamlandığında, `ProvisioningState` değişikliklerini `Succeeded`. Temel katmandan standart olarak değiştirmeyi ve ardından boyutu değiştirme gibi sonraki bir ölçeklendirme işlemi yapmanız gerekirse aşağıdakine benzer bir hata alırsanız veya önceki işlemi tamamlanana kadar beklemeniz gerekir.

    Set-AzRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-an-azure-cache-for-redis"></a>Redis için bir Azure önbelleği hakkında bilgi almak için
Bir önbellek kullanma hakkında bilgi alabilirsiniz [Get-AzRedisCache](https://docs.microsoft.com/powershell/module/az.rediscache/get-azrediscache) cmdlet'i.

Kullanılabilir parametrelerin ve Tanımlamaların için bir listesini görmek için `Get-AzRedisCache`, aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Get-AzRedisCache -detailed

    NAME
        Get-AzRedisCache

    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.

    SYNTAX
        Get-AzRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]

    DESCRIPTION
        The Get-AzRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzRedisCache will return details about the
        specific cache name provided.

        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.

        If no parameters are given than it will return details about all caches the current subscription.

    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzRedisCache
            returns the details for the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

Geçerli abonelikte tüm önbellekleri hakkında bilgi döndürmek için çalıştırın `Get-AzRedisCache` hiçbir parametre olmadan.

    Get-AzRedisCache

Belirli bir kaynak grubu tüm önbellekleri hakkında bilgi döndürmek için çalıştırın `Get-AzRedisCache` ile `ResourceGroupName` parametresi.

    Get-AzRedisCache -ResourceGroupName myGroup

Belirli bir önbellek hakkında bilgi döndürmek için çalıştırma `Get-AzRedisCache` ile `Name` önbelleğin adını içeren bir parametre ve `ResourceGroupName` önbelleğin içeren kaynak grubunu parametresiyle.

    PS C:\> Get-AzRedisCache -Name myCache -ResourceGroupName myGroup

    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="to-retrieve-the-access-keys-for-an-azure-cache-for-redis"></a>Bir Azure önbelleği için Redis için erişim anahtarlarını alınamadı
Önbelleğinizi erişim anahtarlarını almak için kullanabileceğiniz [Get-AzRedisCacheKey](https://docs.microsoft.com/powershell/module/az.rediscache/Get-azRedisCacheKey) cmdlet'i.

Kullanılabilir parametrelerin ve Tanımlamaların için bir listesini görmek için `Get-AzRedisCacheKey`, aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Get-AzRedisCacheKey -detailed

    NAME
        Get-AzRedisCacheKey

    SYNOPSIS
        Gets the accesskeys for the specified Azure Cache for Redis.


    SYNTAX
        Get-AzRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]

    DESCRIPTION
        The Get-AzRedisCacheKey cmdlet gets the access keys for the specified cache.

    PARAMETERS
        -Name <String>
            Name of the Azure Cache for Redis.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

Önbelleğinizi anahtarlarını almak için arama `Get-AzRedisCacheKey` cmdlet'i ve önbelleği içeren kaynak grubunu adını önbelleğinizi adını geçirin.

    PS C:\> Get-AzRedisCacheKey -Name myCache -ResourceGroupName myGroup

    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-azure-cache-for-redis"></a>Erişim anahtarları, Azure önbelleği için Redis için yeniden oluşturmak için
Önbelleğinizi için erişim anahtarlarını yeniden oluşturmak için kullanabileceğiniz [yeni AzRedisCacheKey](https://docs.microsoft.com/powershell/module/az.rediscache/New-azRedisCacheKey) cmdlet'i.

Kullanılabilir parametrelerin ve Tanımlamaların için bir listesini görmek için `New-AzRedisCacheKey`, aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help New-AzRedisCacheKey -detailed

    NAME
        New-AzRedisCacheKey

    SYNOPSIS
        Regenerates the access key of an Azure Cache for Redis.

    SYNTAX
        New-AzRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]

    DESCRIPTION
        The New-AzRedisCacheKey cmdlet regenerate the access key of an Azure Cache for Redis.

    PARAMETERS
        -Name <String>
            Name of the Azure Cache for Redis.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        -KeyType <String>
            Specifies whether to regenerate the primary or secondary access key. Possible values are Primary or Secondary.

        -Force
            When the Force parameter is provided, the specified access key is regenerated without any confirmation prompts.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

Önbellek hesabınız için birincil veya ikincil anahtarı yeniden oluşturmak için çağrı `New-AzRedisCacheKey` cmdlet'i seçeneklerinden birini belirtin ve kaynak grubu adı olarak oldfashionedgoat `Primary` veya `Secondary` için `KeyType` parametresi. Aşağıdaki örnekte, bir önbellek için ikincil erişim anahtarını yeniden oluşturulur.

    PS C:\> New-AzRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary

    Confirm
    Are you sure you want to regenerate Secondary key for Azure Cache for Redis 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-an-azure-cache-for-redis"></a>Bir Azure önbelleği için Redis silmek için
Bir Azure önbelleği için Redis silmek için kullanın [Remove-AzRedisCache](https://docs.microsoft.com/powershell/module/az.rediscache/remove-azrediscache) cmdlet'i.

Kullanılabilir parametrelerin ve Tanımlamaların için bir listesini görmek için `Remove-AzRedisCache`, aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Remove-AzRedisCache -detailed

    NAME
        Remove-AzRedisCache

    SYNOPSIS
        Remove Azure Cache for Redis if exists.

    SYNTAX
        Remove-AzRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>

    DESCRIPTION
        The Remove-AzRedisCache cmdlet removes an Azure Cache for Redis if it exists.

    PARAMETERS
        -Name <String>
            Name of the Azure Cache for Redis to remove.

        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.

        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.

        -PassThru
            By default Remove-AzRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzRedisCache returns a boolean value indicating the success of the operatio

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).

Aşağıdaki örnekte, önbellek adlı `myCache` kaldırılır.

    PS C:\> Remove-AzRedisCache -Name myCache -ResourceGroupName myGroup

    Confirm
    Are you sure you want to remove Azure Cache for Redis 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-an-azure-cache-for-redis"></a>Bir Azure önbelleği için Redis içeri aktarmak için
Bir Azure Cache, Redis örneğinin kullanılması için veri içe aktarabilirsiniz `Import-AzRedisCache` cmdlet'i.

> [!IMPORTANT]
> İçeri/dışarı aktarma için kullanılabilir, yalnızca [premium katmanı](cache-premium-tier-intro.md) önbelleğe alır. İçeri/dışarı aktarma hakkında daha fazla bilgi için bkz: [içeri ve dışarı aktarma verileri Azure önbelleği için Redis](cache-how-to-import-export-data.md).
> 
> 

Kullanılabilir parametrelerin ve Tanımlamaların için bir listesini görmek için `Import-AzRedisCache`, aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Import-AzRedisCache -detailed

    NAME
        Import-AzRedisCache

    SYNOPSIS
        Import data from blobs to Azure Cache for Redis.


    SYNTAX
        Import-AzRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Import-AzRedisCache cmdlet imports data from the specified blobs into Azure Cache for Redis.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -Files <String[]>
            SAS urls of blobs whose content should be imported into the cache.

        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.

        -Force
            When the Force parameter is provided, import will be performed without any confirmation prompts.

        -PassThru
            By default Import-AzRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzRedisCache returns a boolean value indicating the success of the
            operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).


Aşağıdaki komut SAS URI tarafından Azure önbelleğine Redis için belirtilen blob verileri alır.

    PS C:\>Import-AzRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-an-azure-cache-for-redis"></a>Bir Azure önbelleği için Redis dışarı aktarmak için
Redis örneği kullanmak için bir Azure önbelleğindeki verileri dışarı aktarabilirsiniz `Export-AzRedisCache` cmdlet'i.

> [!IMPORTANT]
> İçeri/dışarı aktarma için kullanılabilir, yalnızca [premium katmanı](cache-premium-tier-intro.md) önbelleğe alır. İçeri/dışarı aktarma hakkında daha fazla bilgi için bkz: [içeri ve dışarı aktarma verileri Azure önbelleği için Redis](cache-how-to-import-export-data.md).
> 
> 

Kullanılabilir parametrelerin ve Tanımlamaların için bir listesini görmek için `Export-AzRedisCache`, aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Export-AzRedisCache -detailed

    NAME
        Export-AzRedisCache

    SYNOPSIS
        Exports data from Azure Cache for Redis to a specified container.


    SYNTAX
        Export-AzRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Export-AzRedisCache cmdlet exports data from Azure Cache for Redis to a specified container.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -Prefix <String>
            Prefix to use for blob names.

        -Container <String>
            SAS url of container where data should be exported.

        -Format <String>
            Format for the blob.  Currently "rdb" is the only supported, with other formats expected in the future.

        -PassThru
            By default Export-AzRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzRedisCache returns a boolean value indicating the success of the operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).


Aşağıdaki komutu, bir Azure önbelleği için SAS URI tarafından belirtilen kapsayıcı içine bir Redis örneğinden gelen verileri dışarı aktarır.

        PS C:\>Export-AzRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-an-azure-cache-for-redis"></a>Bir Azure önbelleği için Redis yeniden başlatmak için
Azure Redis örneği kullanmak için önbelleğinizi başlatmanız `Reset-AzRedisCache` cmdlet'i.

> [!IMPORTANT]
> Yeniden başlatma, yalnızca kullanılabilir [premium katmanı](cache-premium-tier-intro.md) önbelleğe alır. Önbelleğinizi yeniden başlatma hakkında daha fazla bilgi için bkz. [önbelleğe alma, yönetim - reboot](cache-administration.md#reboot).
> 
> 

Kullanılabilir parametrelerin ve Tanımlamaların için bir listesini görmek için `Reset-AzRedisCache`, aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Reset-AzRedisCache -detailed

    NAME
        Reset-AzRedisCache

    SYNOPSIS
        Reboot specified node(s) of an Azure Cache for Redis instance.


    SYNTAX
        Reset-AzRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Reset-AzRedisCache cmdlet reboots the specified node(s) of an Azure Cache for Redis instance.


    PARAMETERS
        -Name <String>
            The name of the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache.

        -RebootType <String>
            Which node to reboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".

        -ShardId <Integer>
            Which shard to reboot when rebooting a premium cache with clustering enabled.

        -Force
            When the Force parameter is provided, reset will be performed without any confirmation prompts.

        -PassThru
            By default Reset-AzRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzRedisCache returns a boolean value indicating the success of the operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (https://go.microsoft.com/fwlink/?LinkID=113216).


Aşağıdaki komut, belirtilen önbellek her iki düğüm yeniden başlatır.

        PS C:\>Reset-AzRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force


## <a name="next-steps"></a>Sonraki adımlar
Azure ile Windows PowerShell kullanma hakkında daha fazla bilgi edinmek için aşağıdaki kaynaklara bakın:

* [Azure önbelleği için Redis cmdlet belgeleri MSDN](https://docs.microsoft.com/powershell/module/az.rediscache)
* [Azure Resource Manager cmdlet'leri](https://go.microsoft.com/fwlink/?LinkID=394765): Azure Resource Manager modüldeki cmdlet'ler kullanmayı öğrenin.
* [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-template-deploy-portal.md): Azure portalında kaynak gruplarını oluşturma ve yönetme hakkında bilgi edinin.
* [Azure blogu](https://azure.microsoft.com/blog/): Azure'da yeni özellikler hakkında bilgi edinin.
* [Windows PowerShell Web günlüğü](https://blogs.msdn.com/powershell): Windows PowerShell'de yeni özellikler hakkında bilgi edinin.
* ["Hey, betik yazarı!" Blog](https://blogs.technet.com/b/heyscriptingguy/): Windows PowerShell Topluluğu'ndan gerçek ipuçları ve püf noktaları alın.

