---
title: Klasik Azure CLI kullanarak Azure Redis Cache yönetme | Microsoft Docs
description: Herhangi bir platformda Klasik Azure CLI yükleme, Azure hesabınıza bağlanmak için kullanma ve oluşturma ve bir Redis cache Klasik CLI'dan Yönet öğrenin.
services: redis-cache
documentationcenter: ''
author: wesmc7777
manager: cfowler
editor: ''
ms.assetid: 964ff245-859d-4bc1-bccf-62e4b3c1169f
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: wesmc
ms.openlocfilehash: 0e8bbaad920f35028c51641779a3272f73f81f37
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978446"
---
# <a name="how-to-create-and-manage-azure-redis-cache-using-the-azure-classic-cli"></a>Oluşturma ve Azure Klasik CLI kullanarak Azure Redis Cache'ni Yönet
> [!div class="op_single_selector"]
> * [PowerShell](cache-howto-manage-redis-cache-powershell.md)
> * [Klasik Azure CLI](cache-manage-cli.md)
>

Klasik Azure CLI, Azure altyapınıza herhangi bir platformdan yönetmek için harika bir yoludur. Bu makalede, Klasik Azure CLI kullanarak, Azure Redis Cache örnekleri oluşturma ve yönetme işlemini göstermektedir.

[!INCLUDE [outdated-cli-content](../../includes/contains-classic-cli-content.md)]
> [!NOTE]
> En son Azure CLI örnek betikler için bkz: [CLI Azure Redis önbelleği örnekleri](cli-samples.md).

## <a name="prerequisites"></a>Önkoşullar
Klasik Azure CLI kullanarak Azure Redis Cache örnekleri oluşturma ve yönetme hakkında bilgi için aşağıdaki adımları tamamlamanız gerekir.

* Azure hesabınız olmalıdır. Yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika sonra.
* [Klasik Azure CLI yükleme](../cli-install-nodejs.md).
* Bir iş veya kişisel bir Azure hesabı ile Azure CLI yüklemenizi bağlanmak veya Azure hesabı Okul ve oturum Klasik kullanarak CLI `azure login` komutu.
* Aşağıdaki komutlardan herhangi birini çalıştırmadan önce Klasik CLI'yı Resource Manager moduna çalıştırarak geçiş `azure config mode arm` komutu. Daha fazla bilgi için [Azure kaynaklarını ve kaynak gruplarını yönetmek için Azure Klasik CLI kullanma](../xplat-cli-azure-resource-manager.md).

## <a name="redis-cache-properties"></a>Redis önbelleği özellikleri
Aşağıdaki özellikleri oluşturma ve Redis Cache örnekleri güncelleştirme olduğunda kullanılır.

| Özellik | Anahtar | Açıklama |
| --- | --- | --- |
| ad |n-,--adı |Redis Cache adı. |
| kaynak grubu |g-,--resource-group |Kaynak grubunun adı. |
| location |m-,--konum |Önbellek oluşturmak için konum. |
| boyut |z-,--boyutu |Redis önbelleği boyutu. Geçerli değerler: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4] |
| SKU |-x, --sku |Redis SKU. Şunlardan biri olmalıdır: [temel, standart, Premium] |
| enableNonSslPort |e-,--enable-olmayan-ssl-bağlantı noktası |Redis Cache EnableNonSslPort özelliği. Bu bayrağı olmayan SSL bağlantı noktası önbellek hesabınız için etkinleştirmek istiyorsanız ekleyin |
| Redis yapılandırması |c-,--redis yapılandırması |Redis yapılandırması. Yapılandırma anahtarları ve değerleri burada içeren bir JSON biçimli dizeyi girin. Biçim: "{" ":""," ":" "}" |
| Redis yapılandırması |f-,--redis yapılandırma dosyası |Redis yapılandırması. Yapılandırma anahtarları ve değerleri burada içeren bir dosyanın yolunu girin. Dosya giriş biçimi: {"": "","": ""} |
| Parça sayısı |r-,--parça sayısı |Kümeleme ile Premium küme önbelleği oluşturmak için parça sayısı. |
| Sanal Ağ |v-,--sanal ağ |Bir vnet'teki önbelleğinizi barındırırken, redis önbelleği içinde dağıtmayı tam ARM kaynak kimliği sanal ağın belirtir. Örnek Biçim: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Anahtar türü |t-,--anahtar türü |Yenilemek için anahtar türü. Geçerli değerler: [birincil, ikincil] |
| StaticIP |-p,--statik IP < statik IP > |Bir vnet'teki önbelleğinizi barındırırken, benzersiz bir IP adresi alt ağ için önbellek belirtir. Sağlanmazsa, bir, alt ağdan seçilir. |
| Alt ağ |t,--alt ağ <subnet> |Bir vnet'teki önbelleğinizi barındırırken, önbellek dağıtacağınız bir alt ağ adını belirtir. |
| VirtualNetwork |v-,--sanal ağ < sanal ağ > |Bir vnet'teki önbelleğinizi barındırırken, redis önbelleği içinde dağıtmayı tam ARM kaynak kimliği sanal ağın belirtir. Örnek Biçim: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Abonelik |s-,--abonelik |Abonelik tanımlayıcısı. |

## <a name="see-all-redis-cache-commands"></a>Redis Cache komutların bakın
Redis Cache komutların ve parametreleri görmek için `azure rediscache -h` komutu.

    C:\>azure rediscache -h
    help:    Commands to manage your Azure Redis Cache(s)
    help:
    help:    Create a Redis Cache
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Redis Cache
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Redis Caches within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Redis Cache
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Redis Cache
    help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Renew the authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a>Redis Cache oluşturma
Bir Redis önbelleği oluşturmak için aşağıdaki komutu kullanın:

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

Bu komut hakkında daha fazla bilgi için `azure rediscache create -h` komutu.

    C:\>azure rediscache create -h
    help:    Create a Redis Cache
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -l, --location <location>                                Location to create cache.
    help:      -z, --size <size>                                        Size of the Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of the Redis Cache. Add this flag if you want to enable the Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here. Format for the file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards to create on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a>Mevcut bir Redis önbelleği Sil
Bir Redis Cache'in silmek için aşağıdaki komutu kullanın:

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

Bu komut hakkında daha fazla bilgi için `azure rediscache delete -h` komutu.

    C:\>azure rediscache delete -h
    help:    Delete an existing Redis Cache
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a>Abonelik veya kaynak grubundaki tüm Redis önbellekleri listesi
Abonelik veya kaynak grubundaki tüm Redis önbellekleri listelemek için aşağıdaki komutu kullanın:

    azure rediscache list [options]

Bu komut hakkında daha fazla bilgi için `azure rediscache list -h` komutu.

    C:\>azure rediscache list -h
    help:    List all Redis Caches within your Subscription or Resource Group
    help:
    help:    Usage: rediscache list [options]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a>Mevcut bir Redis Cache'in özelliklerini göster
Mevcut bir Redis Cache'in özelliklerini göstermek için aşağıdaki komutu kullanın:

    azure rediscache show [--name <name> --resource-group <resource-group>]

Bu komut hakkında daha fazla bilgi için `azure rediscache show -h` komutu.

    C:\>azure rediscache show -h
    help:    Show properties of an existing Redis Cache
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>

## <a name="change-settings-of-an-existing-redis-cache"></a>Mevcut bir Redis Cache'in ayarlarını değiştir
Mevcut bir Redis Cache'in ayarlarını değiştirmek için aşağıdaki komutu kullanın:

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

Bu komut hakkında daha fazla bilgi için `azure rediscache set -h` komutu.

    C:\>azure rediscache set -h
    help:    Change settings of an existing Redis Cache
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-the-authentication-key-for-an-existing-redis-cache"></a>Mevcut bir Redis Cache için kimlik doğrulama anahtarını Yenile
Mevcut bir Redis Cache için kimlik doğrulama anahtarını yenilemek için aşağıdaki komutu kullanın:

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

Belirtin `Primary` veya `Secondary` için `key-type`.

Bu komut hakkında daha fazla bilgi için `azure rediscache renew-key -h` komutu.

    C:\>azure rediscache renew-key -h
    help:    Renew the authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key to renew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a>Mevcut bir Redis Cache'in listesi birincil ve ikincil anahtarları
Birincil ve ikincil anahtarlarını Listele mevcut bir Redis Cache'in için aşağıdaki komutu kullanın:

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

Bu komut hakkında daha fazla bilgi için `azure rediscache list-keys -h` komutu.

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)
