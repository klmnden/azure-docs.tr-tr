---
title: "Azure CLI kullanarak Azure Redis önbelleği yönetme | Microsoft Docs"
description: "Herhangi bir platformda Azure CLI yükleme, nasıl Azure hesabınıza bağlamak için kullanılır ve oluşturmak ve Redis önbelleği Azure CLI üzerinden yönetmek nasıl öğrenin."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 964ff245-859d-4bc1-bccf-62e4b3c1169f
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: sdanie
ms.openlocfilehash: d3a425251035e09bb3163fbb052669d0a874806f
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="how-to-create-and-manage-azure-redis-cache-using-the-azure-command-line-interface-azure-cli"></a>Oluşturma ve Azure komut satırı arabirimi (Azure CLI) kullanarak Azure Redis önbelleği yönetme
> [!div class="op_single_selector"]
> * [PowerShell](cache-howto-manage-redis-cache-powershell.md)
> * [Azure CLI](cache-manage-cli.md)
>
>

Azure CLI, Azure altyapınıza herhangi bir platform yönetmek için harika bir yoludur. Bu makalede oluşturmak ve Azure CLI kullanarak Azure Redis önbelleği örneklerinizi yönetmek nasıl gösterir.

> [!NOTE]
> Bu makale, Azure CLI önceki bir sürümü için geçerlidir. En son Azure CLI 2.0 örnek komut dosyaları için bkz: [CLI Azure Redis önbelleği örnekleri](cli-samples.md).
> 
> 

## <a name="prerequisites"></a>Ön koşullar
Oluşturma ve Azure CLI kullanarak Azure Redis önbelleği örnekleri yönetmek için aşağıdaki adımları tamamlamanız gerekir.

* Bir Azure hesabınızın olması gerekir. Yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika sonra.
* [Azure CLI'yı yükleme](../cli-install-nodejs.md).
* Kişisel bir Azure hesabı veya bir iş ile Azure CLI yüklemenizi bağlanmak veya Okul Azure hesabı ve oturum Azure CLI kullanımından `azure login` komutu. Farkları anlamak ve seçmek için bkz: [bir Azure aboneliğine bağlanma Azure komut satırı arabirimi (Azure CLI) gelen](/cli/azure/authenticate-azure-cli).
* Aşağıdaki komutlardan herhangi birini çalıştırmadan önce Azure CLI Resource Manager moduna çalıştırarak geçiş `azure config mode arm` komutu. Daha fazla bilgi için bkz: [Azure kaynakları ve kaynak gruplarını yönetmek için Azure CLI kullanma](../xplat-cli-azure-resource-manager.md).

## <a name="redis-cache-properties"></a>Redis önbelleği özellikleri
Aşağıdaki özellikleri oluşturma ve Redis önbelleği örnekleri güncelleştirme olduğunda kullanılır.

| Özellik | Anahtar | Açıklama |
| --- | --- | --- |
| ad |-n,--adı |Redis önbelleği adı. |
| kaynak grubu |-g,--kaynak-grubu |Kaynak grubunun adı. |
| location |-l,--konum |Önbellek oluşturmak için konum. |
| boyut |-z,--boyutu |Redis önbelleği boyutu. Geçerli değerler: [C0 C1, C2, C3, C4, C5, C6, P1, P2, P3, P4] |
| SKU |-x,--sku |SKU redis. Şunlardan biri olmalıdır: [temel, standart, Premium] |
| EnableNonSslPort |-e,--enable-olmayan-ssl-bağlantı noktası |Redis önbelleği EnableNonSslPort özelliği. Bu bayrak olmayan SSL bağlantı noktası önbelleğiniz için etkinleştirmek istiyorsanız ekleyin |
| Yapılandırma redis |-c,--redis-yapılandırma |Yapılandırma redis. Yapılandırma anahtarları ve değerleri burada biçimlendirilmiş JSON dizesi girin. Biçim: "{" ":""," ":" "}" |
| Yapılandırma redis |-f,--redis yapılandırma dosyası |Yapılandırma redis. Yapılandırma anahtarları ve değerleri burada içeren dosyanın yolunu girin. Biçim dosyası girişi: {"": "","": ""} |
| Parça sayısı |-r,--parça sayısı |Kümeleme ile Premium küme önbelleği oluşturmak için parça sayısı. |
| Sanal Ağ |-v,--sanal ağ |Önbelleğiniz bir VNET içindeki barındırdığında redis önbelleğinde dağıtmak için tam ARM kaynak Kimliğini sanal ağın belirtir. Örnek Biçim: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Anahtar türü |-t,--anahtar türü |Yenilemek için anahtar türü. Geçerli değerler: [birincil, ikincil] |
| StaticIP |-p,--statik IP < statik IP > |Önbelleğiniz bir VNET içindeki barındırma alt önbelleği için benzersiz bir IP adresi belirtir. Sağlanmazsa, bir sizin için alt ağ üzerinden seçilir. |
| Alt ağ |t,--alt ağ<subnet> |Önbelleğinizi bir VNET içindeki barındırdığında, önbellek dağıtmak alt ağ adını belirtir. |
| VirtualNetwork |-v,--sanal ağ < sanal ağ > |Önbelleğiniz bir VNET içindeki barındırdığında redis önbelleğinde dağıtmak için tam ARM kaynak Kimliğini sanal ağın belirtir. Örnek Biçim: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Abonelik |-s,--abonelik |Abonelik tanımlayıcısı. |

## <a name="see-all-redis-cache-commands"></a>Tüm Redis önbelleği komutları bakın
Tüm Redis önbelleği komutları ve bunların parametrelerini görmek için `azure rediscache -h` komutu.

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

## <a name="delete-an-existing-redis-cache"></a>Varolan bir Redis önbelleği Sil
Redis önbelleğini silmek için aşağıdaki komutu kullanın:

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

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a>Abonelik veya kaynak grubu içindeki tüm Redis önbellekleri listesi
Abonelik veya kaynak grubu içindeki tüm Redis önbellekleri listelemek için aşağıdaki komutu kullanın:

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

## <a name="show-properties-of-an-existing-redis-cache"></a>Varolan bir Redis önbelleği özelliklerini göster
Varolan bir Redis önbelleği özelliklerini göstermek için aşağıdaki komutu kullanın:

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

## <a name="change-settings-of-an-existing-redis-cache"></a>Varolan bir Redis önbelleği ayarlarını değiştir
Varolan bir Redis önbelleği ayarlarını değiştirmek için aşağıdaki komutu kullanın:

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

## <a name="renew-the-authentication-key-for-an-existing-redis-cache"></a>Varolan bir Redis önbelleği için kimlik doğrulama anahtarı yenileme
Varolan bir Redis önbelleği için kimlik doğrulama anahtarını yenilemek için aşağıdaki komutu kullanın:

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

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a>Varolan bir Redis önbelleği listesi birincil ve ikincil anahtarları
Liste birincil ve ikincil anahtarları varolan bir Redis önbelleği için aşağıdaki komutu kullanın:

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
