---
title: Azure PowerShell ile Azure Redis önbelleğini yönetme | Microsoft Docs
description: Azure Redis önbelleği Azure PowerShell kullanarak için yönetim görevlerini gerçekleştirmek öğrenin.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 1136efe5-1e33-4d91-bb49-c8e2a6dca475
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: wesmc
ms.openlocfilehash: 38b2f57811b0e952d3020c06d39350918f2f0391
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="manage-azure-redis-cache-with-azure-powershell"></a>Azure PowerShell ile Azure Redis önbelleği Yönet
> [!div class="op_single_selector"]
> * [PowerShell](cache-howto-manage-redis-cache-powershell.md)
> * [Azure CLI](cache-manage-cli.md)
> 
> 

Bu konu, oluşturma gibi genel görevleri gerçekleştirmek için nasıl güncelleştirmek ve Azure Redis önbelleği örnekleri, erişim anahtarlarını yeniden nasıl ve önbellekleri hakkındaki bilgileri görüntülemek nasıl ölçeği gösterir. Azure Redis önbelleği PowerShell cmdlet'lerinin tam listesi için bkz: [Azure Redis önbelleği cmdlet'leri](https://msdn.microsoft.com/library/azure/mt634513.aspx).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

Klasik dağıtım modeli hakkında daha fazla bilgi için bkz: [Azure Resource Manager ve klasik dağıtım: dağıtım modelleri ve kaynaklarınızın durumunu anlamak](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="prerequisites"></a>Önkoşullar
Azure PowerShell'i zaten yüklediyseniz, Azure PowerShell sürümü 1.0.0 olmalıdır veya sonraki bir sürümü. Azure PowerShell komut isteminde bu komutla yüklediğiniz Azure PowerShell sürümü kontrol edebilirsiniz.

    Get-Module azure | format-table version


İlk olarak, Azure için bu komutla oturum gerekir.

    Connect-AzureRmAccount

Oturum açma iletişim kutusunda Microsoft Azure e-posta adresi, Azure hesabınızı ve parolasını belirtin.

Ardından, birden çok Azure aboneliğiniz varsa Azure aboneliğinizi ayarlamak gerekir. Geçerli Aboneliklerin listesini görmek için bu komutu çalıştırın.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

Aboneliği belirtmek için aşağıdaki komutu çalıştırın. Aşağıdaki örnekte, abonelik addır `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

Azure Resource Manager ile Windows PowerShell kullanmadan önce aşağıdakiler gerekir:

* Windows PowerShell sürüm 3.0 veya 4.0. Windows PowerShell sürümü bulmak için şunu yazın:`$PSVersionTable` ve değerini doğrulayın `PSVersion` 3.0 veya 4.0. Uyumlu bir sürümünü yüklemek için bkz: [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) veya [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

Bu öğreticide gördüğünüz herhangi bir cmdlet'in ayrıntılı yardım almak için Get-Help cmdlet'ini kullanın.

    Get-Help <cmdlet-name> -Detailed

Örneğin, Yardım almak için `New-AzureRmRedisCache` cmdlet, türü:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-to-connect-to-other-clouds"></a>Diğer bulutlara bağlanma
Varsayılan olarak Azure ortamıdır `AzureCloud`, genel Azure bulut örneği temsil eder. Farklı bir örneğine bağlanmak için `Connect-AzureRmAccount` komutunu `-Environment` veya -`EnvironmentName` komut satırı anahtarıyla istenen ortama veya ortam adı.

Kullanılabilir ortamlar listesini görmek için Çalıştır `Get-AzureRmEnvironment` cmdlet'i.

### <a name="to-connect-to-the-azure-government-cloud"></a>Azure Bulutu bağlanmak için
Azure Bulutu bağlanmak için aşağıdaki komutlardan birini kullanın.

    Connect-AzureRmAccount -EnvironmentName AzureUSGovernment

or

    Connect-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

Azure Bulutu önbellek oluşturmak için aşağıdaki konumlardan birini kullanın.

* USGov Virginia
* USGov Iowa

Azure Bulutu hakkında daha fazla bilgi için bkz: [Microsoft Azure kamu](https://azure.microsoft.com/features/gov/) ve [Microsoft Azure kamu Geliştirici Kılavuzu](../azure-government-developer-guide.md).

### <a name="to-connect-to-the-azure-china-cloud"></a>Azure Çin buluta bağlamak için
Azure Çin buluta bağlanmak için aşağıdaki komutlardan birini kullanın.

    Connect-AzureRmAccount -EnvironmentName AzureChinaCloud

or

    Connect-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

Önbellek Azure Çin bulut oluşturmak için aşağıdaki konumlardan birini kullanın.

* Çin Doğu
* Çin Kuzey

Azure Çin bulut hakkında daha fazla bilgi için bkz: [AzureChinaCloud Azure Çin'de 21Vianet tarafından işletilen](http://www.windowsazure.cn/).

### <a name="to-connect-to-microsoft-azure-germany"></a>Microsoft Azure Almanya bağlanmak için
Microsoft Azure Almanya bağlanmak için aşağıdaki komutlardan birini kullanın.

    Connect-AzureRmAccount -EnvironmentName AzureGermanCloud


or

    Connect-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureGermanCloud)

Microsoft Azure Almanya önbellek oluşturmak için aşağıdaki konumlardan birini kullanın.

* Almanya Orta
* Almanya Kuzeydoğu

Microsoft Azure Almanya hakkında daha fazla bilgi için bkz: [Microsoft Azure Almanya](https://azure.microsoft.com/overview/clouds/germany/).

### <a name="properties-used-for-azure-redis-cache-powershell"></a>Azure Redis önbelleği PowerShell için kullanılan özellikleri
Aşağıdaki tabloda, özellikleri ve açıklamaları oluştururken ve Azure PowerShell kullanarak Azure Redis önbelleği örneklerinizi yönetme yaygın olarak kullanılan parametreleri içerir.

| Parametre | Açıklama | Varsayılan |
| --- | --- | --- |
| Ad |Önbellek adı | |
| Konum |Önbellek konumu | |
| ResourceGroupName |Kaynak grubu adı, önbellek oluşturmak için | |
| Boyut |Önbellek boyutu. Geçerli değerler: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1GB, 2.5 GB, 6 GB, 13 GB, 26 GB, 53 GB'a |1 GB |
| ShardCount |Premium önbelleği kümeleme özelliği etkinleştirilmiş oluştururken oluşturmak için parça sayısı. Geçerli değerler: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 | |
| SKU |Önbellek SKU'su belirtir. Geçerli değerler: temel, standart ve Premium |Standart |
| RedisConfiguration |Redis yapılandırma ayarlarını belirtir. Her ayarlama hakkında daha fazla bilgi için aşağıdakilere bakın [RedisConfiguration özellikleri](#redisconfiguration-properties) tablo. | |
| EnableNonSslPort |SSL olmayan bağlantı noktası etkinleştirilip etkinleştirilmeyeceğini gösterir. |False |
| MaxMemoryPolicy |Bu parametre kullanım dışı - RedisConfiguration kullanın. | |
| StaticIP |Önbelleğiniz bir VNET içindeki barındırma alt önbelleği için benzersiz bir IP adresi belirtir. Sağlanmazsa, bir sizin için alt ağ üzerinden seçilir. | |
| Alt ağ |Önbelleğinizi bir VNET içindeki barındırdığında, önbellek dağıtmak alt ağ adını belirtir. | |
| VirtualNetwork |Önbelleğinizi bir VNET içindeki barındırdığında önbellek dağıtmak üzere VNET kaynak Kimliğini belirtir. | |
| keyType |Erişim tuşları yenilenirken yeniden oluşturmak için hangi erişim anahtarını belirtir. Geçerli değerler: birincil, ikincil | |

### <a name="redisconfiguration-properties"></a>RedisConfiguration özellikleri
| Özellik | Açıklama | Fiyatlandırma katmanları |
| --- | --- | --- |
| RDB yedekleme etkin |Olup olmadığını [Redis veri kalıcılığını](cache-how-to-premium-persistence.md) etkin |Yalnızca Premium |
| RDB depolama bağlantı dizesi |Depolama hesabı bağlantı dizesi [Redis veri kalıcılığını](cache-how-to-premium-persistence.md) |Yalnızca Premium |
| RDB yedekleme sıklığı |Yedekleme sıklığı için [Redis veri kalıcılığını](cache-how-to-premium-persistence.md) |Yalnızca Premium |
| maxmemory-ayrılmış |Yapılandırır [ayrılan bellek](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) önbellek olmayan işlemler için |Standart ve Premium |
| maxmemory İlkesi |Yapılandırır [çıkarma İlkesi](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) önbelleği |Tüm fiyatlandırma katmanlarına |
| bildirim-keyspace-olayları |Yapılandırır [keyspace bildirimleri](cache-configure.md#keyspace-notifications-advanced-settings) |Standart ve Premium |
| max ziplist girişlerini karma |Yapılandırır [belleği en iyi duruma getirme](http://redis.io/topics/memory-optimization) küçük toplam veri türleri |Standart ve Premium |
| karma-max-ziplist-değer |Yapılandırır [belleği en iyi duruma getirme](http://redis.io/topics/memory-optimization) küçük toplam veri türleri |Standart ve Premium |
| max intset girişlerini ayarlama |Yapılandırır [belleği en iyi duruma getirme](http://redis.io/topics/memory-optimization) küçük toplam veri türleri |Standart ve Premium |
| zset-max-ziplist-entries |Yapılandırır [belleği en iyi duruma getirme](http://redis.io/topics/memory-optimization) küçük toplam veri türleri |Standart ve Premium |
| zset-max-ziplist-value |Yapılandırır [belleği en iyi duruma getirme](http://redis.io/topics/memory-optimization) küçük toplam veri türleri |Standart ve Premium |
| veritabanları |Veritabanı sayısı yapılandırır. Bu özellik yalnızca önbellek oluşturma sırasında yapılandırılabilir. |Standart ve Premium |

## <a name="to-create-a-redis-cache"></a>Redis önbelleği oluşturma
Yeni Azure Redis önbelleği örnekleri kullanarak oluşturulur [yeni AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet'i.

> [!IMPORTANT]
> Azure portalını kullanarak bir abonelikte Redis önbelleği oluşturma ilk kez portal kaydeder `Microsoft.Cache` Bu abonelik için ad alanı. PowerShell kullanarak bir abonelikte ilk Redis önbelleği oluşturmayı denerseniz, aşağıdaki komutu kullanarak bu ad kaydetmeniz gerekir; Aksi takdirde cmdlet'leri gibi `New-AzureRmRedisCache` ve `Get-AzureRmRedisCache` başarısız.
> 
> `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`
> 
> 

Kullanılabilir parametrelerin ve açıklamaları için listesini görmek için `New-AzureRmRedisCache`, aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed

    NAME
        New-AzureRmRedisCache

    SYNOPSIS
        Creates a new redis cache.


    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]


    DESCRIPTION
        The New-AzureRmRedisCache cmdlet creates a new redis cache.


    PARAMETERS
        -Name <String>
            Name of the redis cache to create.

        -ResourceGroupName <String>
            Name of resource group in which to create the redis cache.

        -Location <String>
            Location in which to create the redis cache.

        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.

        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, the default value is false and the
            non-SSL port will be disabled. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        -VirtualNetwork <String>
            The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}

        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Varsayılan parametrelere sahip bir önbellek oluşturmak için aşağıdaki komutu çalıştırın.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, ve `Location` Gerekli Parametreler, ancak geri kalan isteğe bağlıdır ve varsayılan değerlere sahip. Önceki komutu çalıştıran standart SKU Azure Redis önbelleği örneği belirtilen adını, konumunu ve devre dışı SSL olmayan bağlantı noktası ile boyutu 1 GB olan kaynak grubunu oluşturur.

Premium önbelleği oluşturmak için P1 (6 GB - 60 GB), P2 (13 GB - 130 GB), boyutunu belirtmek P3 (26 GB - 260 GB), ya da P4 (53 GB'a - 530 GB). Kümeleme etkinleştirmek için bir parça sayısı kullanarak belirtin `ShardCount` parametresi. Aşağıdaki örnek P1 premium önbelleği ile 3 parça oluşturur. Premium P1 önbelleği 6 GB boyutunda, ve şu üç parça belirtilen bu yana toplam boyutu 18 GB (3 x 6 GB).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

Değerlerini belirtmek için `RedisConfiguration` parametresi içindeki değerleri içine `{}` anahtar/değer çiftleri ister `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. Aşağıdaki örnek, bir standart 1 GB önbellekle oluşturur `allkeys-random` ile yapılandırılmış maxmemory İlkesi ve keyspace bildirimleri `KEA`. Daha fazla bilgi için bkz: [Keyspace bildirimleri (Gelişmiş ayarları)](cache-configure.md#keyspace-notifications-advanced-settings) ve [bellek ilkeleri](cache-configure.md#memory-policies).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>

## <a name="to-configure-the-databases-setting-during-cache-creation"></a>Önbelleği oluşturma sırasında ayarı veritabanlarını yapılandırmak için
`databases` Ayarı yalnızca önbellek oluşturma sırasında yapılandırılabilir. Aşağıdaki örnek, premium P3 oluşturur (26 GB) önbellek kullanarak 48 veritabanlarıyla [yeni AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet'i.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

Daha fazla bilgi için `databases` özelliği, bkz: [Azure Redis önbelleği varsayılan sunucu yapılandırma](cache-configure.md#default-redis-server-configuration). Kullanarak bir önbellek oluşturma hakkında daha fazla bilgi için [yeni AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) cmdlet'in önceki bkz [bir Redis önbelleği oluşturmak için](#to-create-a-redis-cache) bölümü.

## <a name="to-update-a-redis-cache"></a>Redis önbelleği güncelleştirmek için
Azure Redis önbelleği örnekleri kullanarak güncel [kümesi AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet'i.

Kullanılabilir parametrelerin ve açıklamaları için listesini görmek için `Set-AzureRmRedisCache`, aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed

    NAME
        Set-AzureRmRedisCache

    SYNOPSIS
        Set redis cache updatable parameters.

    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]

    DESCRIPTION
        The Set-AzureRmRedisCache cmdlet sets redis cache parameters.

    PARAMETERS
        -Name <String>
            Name of the redis cache to update.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        -Size <String>
            Size of the redis cache. The default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. The default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            The 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting to set
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. The default value is null and no change will be made to the
            currently configured value. Possible values are true and false.

        -ShardCount <Integer>
            The number of shards to create on a Premium Cluster Cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

`Set-AzureRmRedisCache` Cmdlet gibi özellikleri güncelleştirmek için kullanılabilir `Size`, `Sku`, `EnableNonSslPort`ve `RedisConfiguration` değerleri. 

Aşağıdaki komut maxmemory İlkesi myCache adlı Redis Önbelleği'için güncelleştirir.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>

## <a name="to-scale-a-redis-cache"></a>Redis önbelleği ölçeklendirme
`Set-AzureRmRedisCache` bir Azure Redis önbelleği ölçeklendirmek için kullanılan zaman örnek `Size`, `Sku`, veya `ShardCount` özellikleri değiştirilemez. 

> [!NOTE]
> PowerShell kullanarak önbellek ölçeklendirme aynı sınırları ve Azure portalından bir önbellek ölçeklendirme olarak yönergeleri tabidir. Aşağıdaki kısıtlamalarla farklı bir fiyatlandırma katmanı için ölçeklendirebilirsiniz.
> 
> * Daha yüksek bir fiyatlandırma Katmanı'ndan daha düşük bir fiyatlandırma katmanı ölçek olamaz.
> * Ölçeklendirme olamaz bir **Premium** aşağı önbelleğe bir **standart** veya **temel** önbelleği.
> * Ölçeklendirme olamaz bir **standart** aşağı önbelleğe bir **temel** önbelleği.
> * Ölçeklendirme yapılabilir bir **temel** için önbelleğe bir **standart** önbellek ancak değiştiremiyor boyutu aynı anda. Farklı bir boyut gerekiyorsa, istenen boyuta sonraki bir ölçeklendirme işlemi yapabilirsiniz.
> * Ölçeklendirme olamaz bir **temel** doğrudan önbelleğe bir **Premium** önbelleği. Ölçeklendirme gerekir **temel** için **standart** bir ölçeklendirme işlemi ve ardından gelen **standart** için **Premium** bir sonraki ölçeklendirme işleminde.
> * Büyük bir değerden aşağı ölçeklendirme olamaz **C0 (250 MB)** boyutu.
> 
> Daha fazla bilgi için bkz: [ölçek Azure Redis önbelleği nasıl](cache-how-to-scale.md).
> 
> 

Aşağıdaki örnek adlı bir önbellek ölçeklendirmek nasıl gösterir `myCache` 2,5 GB önbelleğe. Bu komut bir temel veya standart bir önbellek için çalıştığını unutmayın.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Bu komutu verildikten sonra önbelleği durumunu döndürülür (arama benzer `Get-AzureRmRedisCache`). Unutmayın `ProvisioningState` olan `Scaling`.

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB


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

Ölçeklendirme işlemi tamamlandığında, `ProvisioningState` değişikliklerini `Succeeded`. Temel standart değiştirme ve boyutunu değiştirme gibi sonraki bir ölçeklendirme işlemi yapmanız gerekirse aşağıdakine benzer bir hata iletisi veya önceki işlem tamamlanana kadar beklemeniz gerekir.

    Set-AzureRmRedisCache : Conflict: The resource '...' is not in a stable state, and is currently unable to accept the update request.

## <a name="to-get-information-about-a-redis-cache"></a>Redis önbelleği hakkında bilgi almak için
Önbellek kullanma hakkında bilgi alabilir [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) cmdlet'i.

Kullanılabilir parametrelerin ve açıklamaları için listesini görmek için `Get-AzureRmRedisCache`, aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed

    NAME
        Get-AzureRmRedisCache

    SYNOPSIS
        Gets details about a single cache or all caches in the specified resource group or all caches in the current
        subscription.

    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]

    DESCRIPTION
        The Get-AzureRmRedisCache cmdlet gets the details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.

        If only ResourceGroupName is provided than it will return details about all caches in the specified resource group.

        If no parameters are given than it will return details about all caches the current subscription.

    PARAMETERS
        -Name <String>
            The name of the cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns the details for the cache.

        -ResourceGroupName <String>
            The name of the resource group that contains the cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns the details of the cache specified by Name. If only the ResourceGroup
            parameter is provided, then details for all caches in the resource group are returned.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Geçerli abonelikte tüm önbellekleri hakkında bilgi döndürmek için Çalıştır `Get-AzureRmRedisCache` hiçbir parametre olmadan.

    Get-AzureRmRedisCache

Belirli bir kaynak grubundaki tüm önbellekleri hakkında bilgi döndürmek için Çalıştır `Get-AzureRmRedisCache` ile `ResourceGroupName` parametresi.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

Belirli bir önbellek hakkında bilgi döndürmek için Çalıştır `Get-AzureRmRedisCache` ile `Name` önbellek adını içeren parametre ve `ResourceGroupName` bu önbelleği içeren kaynak grubunu parametresiyle.

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

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

## <a name="to-retrieve-the-access-keys-for-a-redis-cache"></a>Redis önbelleği erişim anahtarlarını almak için
Önbellek hesabınız için erişim tuşlarını almak için kullanabileceğiniz [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) cmdlet'i.

Kullanılabilir parametrelerin ve açıklamaları için listesini görmek için `Get-AzureRmRedisCacheKey`, aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed

    NAME
        Get-AzureRmRedisCacheKey

    SYNOPSIS
        Gets the accesskeys for the specified redis cache.


    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]

    DESCRIPTION
        The Get-AzureRmRedisCacheKey cmdlet gets the access keys for the specified cache.

    PARAMETERS
        -Name <String>
            Name of the redis cache.

        -ResourceGroupName <String>
            Name of the resource group for the cache.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Önbelleğinizi tuşları almak için arama `Get-AzureRmRedisCacheKey` cmdlet'i ve önbelleğiniz adına önbellek içeren kaynak grubunun adını geçirin.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup

    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="to-regenerate-access-keys-for-your-redis-cache"></a>Redis önbelleği erişim anahtarlarını yeniden oluşturmak için
Önbelleğinizi erişim anahtarlarını yeniden oluşturmak için kullanabileceğiniz [yeni AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) cmdlet'i.

Kullanılabilir parametrelerin ve açıklamaları için listesini görmek için `New-AzureRmRedisCacheKey`, aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed

    NAME
        New-AzureRmRedisCacheKey

    SYNOPSIS
        Regenerates the access key of a redis cache.

    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]

    DESCRIPTION
        The New-AzureRmRedisCacheKey cmdlet regenerate the access key of a redis cache.

    PARAMETERS
        -Name <String>
            Name of the redis cache.

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
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Önbellek hesabınız için birincil veya ikincil anahtarı yeniden oluşturmak için arama `New-AzureRmRedisCacheKey` cmdlet adı, kaynak grubu, geçirin ve belirtin `Primary` veya `Secondary` için `KeyType` parametresi. Aşağıdaki örnekte, bir önbellek için ikincil erişim anahtarını yeniden oluşturur.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary

    Confirm
    Are you sure you want to regenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="to-delete-a-redis-cache"></a>Redis önbelleğini silmek için
Redis önbelleği silmek için kullanın [Kaldır AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) cmdlet'i.

Kullanılabilir parametrelerin ve açıklamaları için listesini görmek için `Remove-AzureRmRedisCache`, aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed

    NAME
        Remove-AzureRmRedisCache

    SYNOPSIS
        Remove redis cache if exists.

    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>

    DESCRIPTION
        The Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.

    PARAMETERS
        -Name <String>
            Name of the redis cache to remove.

        -ResourceGroupName <String>
            Name of the resource group of the cache to remove.

        -Force
            When the Force parameter is provided, the cache is removed without any confirmation prompts.

        -PassThru
            By default Remove-AzureRmRedisCache removes the cache and does not return any value. If the PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating the success of the operatio

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Aşağıdaki örnekte, önbelleğe adlı `myCache` kaldırılır.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Confirm
    Are you sure you want to remove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="to-import-a-redis-cache"></a>Redis önbelleği içeri aktarmak için
Bir Azure Redis önbelleği örneği kullanarak içine veri aktarabilirsiniz `Import-AzureRmRedisCache` cmdlet'i.

> [!IMPORTANT]
> İçeri/dışarı aktarma için kullanılabilir yalnızca [premium katmanı](cache-premium-tier-intro.md) önbelleğe alır. İçeri/dışarı aktarma hakkında daha fazla bilgi için bkz: [içeri ve dışarı aktarma Azure Redis önbelleği verilerde](cache-how-to-import-export-data.md).
> 
> 

Kullanılabilir parametrelerin ve açıklamaları için listesini görmek için `Import-AzureRmRedisCache`, aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed

    NAME
        Import-AzureRmRedisCache

    SYNOPSIS
        Import data from blobs to Azure Redis Cache.


    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Import-AzureRmRedisCache cmdlet imports data from the specified blobs into Azure Redis Cache.


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
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If the PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating the success of the
            operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Aşağıdaki komutu Azure Redis önbelleğine SAS URI tarafından belirtilen blob verileri alır.

    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="to-export-a-redis-cache"></a>Redis önbelleği dışarı aktarmak için
Verileri kullanarak bir Azure Redis önbelleği örneği verebilirsiniz `Export-AzureRmRedisCache` cmdlet'i.

> [!IMPORTANT]
> İçeri/dışarı aktarma için kullanılabilir yalnızca [premium katmanı](cache-premium-tier-intro.md) önbelleğe alır. İçeri/dışarı aktarma hakkında daha fazla bilgi için bkz: [içeri ve dışarı aktarma Azure Redis önbelleği verilerde](cache-how-to-import-export-data.md).
> 
> 

Kullanılabilir parametrelerin ve açıklamaları için listesini görmek için `Export-AzureRmRedisCache`, aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed

    NAME
        Export-AzureRmRedisCache

    SYNOPSIS
        Exports data from Azure Redis Cache to a specified container.


    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache to a specified container.


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
            By default Export-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating the success of the operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Aşağıdaki komutu, SAS URI tarafından belirtilen kapsayıcı Azure Redis önbelleği örneğine gelen verileri verir.

        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="to-reboot-a-redis-cache"></a>Redis önbelleği yeniden başlatmak için
Azure Redis önbelleği örneği kullanarak yeniden `Reset-AzureRmRedisCache` cmdlet'i.

> [!IMPORTANT]
> Yeniden başlatma için kullanılabilir yalnızca [premium katmanı](cache-premium-tier-intro.md) önbelleğe alır. Önbelleğinizi yeniden başlatma hakkında daha fazla bilgi için bkz: [önbelleğe yönetim - yeniden başlatma](cache-administration.md#reboot).
> 
> 

Kullanılabilir parametrelerin ve açıklamaları için listesini görmek için `Reset-AzureRmRedisCache`, aşağıdaki komutu çalıştırın.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed

    NAME
        Reset-AzureRmRedisCache

    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.


    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        The Reset-AzureRmRedisCache cmdlet reboots the specified node(s) of an Azure Redis Cache instance.


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
            By default Reset-AzureRmRedisCache does not return any value. If the PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating the success of the operation.

        <CommonParameters>
            This cmdlet supports the common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


Aşağıdaki komutu her iki düğüm belirtilen önbelleğin yeniden başlatır.

        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force


## <a name="next-steps"></a>Sonraki adımlar
Azure ile Windows PowerShell kullanma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [MSDN'deki Azure Redis önbelleği cmdlet belgeleri](https://msdn.microsoft.com/library/azure/mt634513.aspx)
* [Azure Resource Manager cmdlet'lerini](http://go.microsoft.com/fwlink/?LinkID=394765): Azure Resource Manager modülünde cmdlet'leri kullanmayı öğrenin.
* [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-template-deploy-portal.md): Azure portalındaki kaynak gruplarını oluşturma ve yönetme öğrenin.
* [Azure blogu](http://blogs.msdn.com/windowsazure): Azure yeni özellikler hakkında bilgi edinin.
* [Windows PowerShell Web günlüğü](http://blogs.msdn.com/powershell): Windows PowerShell'de yeni özellikler hakkında bilgi edinin.
* ["Hey, betik yazarı!" Blog](http://blogs.technet.com/b/heyscriptingguy/): Windows PowerShell Topluluğu'ndan gerçek ipuçları ve püf noktaları alın.

