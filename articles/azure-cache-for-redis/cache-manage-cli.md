---
title: Azure Cache için Redis Klasik Azure CLI kullanarak yönetme | Microsoft Docs
description: Herhangi bir platformda Klasik Azure CLI yükleme, Azure hesabınıza bağlanmak için kullanma ve oluşturmak ve bir Azure önbelleği için Redis Klasik clı'dan yönetmek nasıl öğrenin.
services: cache
documentationcenter: ''
author: yegu-ms
manager: jhubbard
editor: ''
ms.assetid: 964ff245-859d-4bc1-bccf-62e4b3c1169f
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: yegu
ms.openlocfilehash: 1d7a18f3f46cec73d70389b82eed5a85e440d340
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62119087"
---
# <a name="how-to-create-and-manage-azure-cache-for-redis-using-the-azure-classic-cli"></a>Oluşturma ve Azure Cache için Redis Klasik Azure CLI kullanarak yönetme
> [!div class="op_single_selector"]
> * [PowerShell](cache-howto-manage-redis-cache-powershell.md)
> * [Azure klasik CLI](cache-manage-cli.md)
>

Klasik Azure CLI, Azure altyapınıza herhangi bir platformdan yönetmek için harika bir yoludur. Bu makalede Klasik Azure CLI kullanarak Redis örneği için Azure önbelleğinizi oluşturmak ve yönetmek nasıl gösterir.

[!INCLUDE [outdated-cli-content](../../includes/contains-classic-cli-content.md)]
> [!NOTE]
> En son Azure CLI örnek betikler için bkz: [Azure CLI Azure önbelleği için Redis örnekleri](cli-samples.md).

## <a name="prerequisites"></a>Önkoşullar
Önbelleğinizi oluşturmak ve Azure Klasik Azure CLI kullanarak Redis örneği için yönetmek için aşağıdaki adımları tamamlamanız gerekir.

* Azure hesabınız olmalıdır. Yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) yalnızca birkaç dakika sonra.
* [Klasik Azure CLI yükleme](../cli-install-nodejs.md).
* Bir iş veya kişisel bir Azure hesabı ile Azure CLI yüklemenizi bağlanmak veya Azure hesabı Okul ve oturum Klasik kullanarak CLI `azure login` komutu.
* Aşağıdaki komutlardan herhangi birini çalıştırmadan önce Klasik CLI'yı Resource Manager moduna çalıştırarak geçiş `azure config mode arm` komutu. Daha fazla bilgi için [Azure kaynaklarını ve kaynak gruplarını yönetmek için Azure Klasik CLI kullanma](../xplat-cli-azure-resource-manager.md).

## <a name="azure-cache-for-redis-properties"></a>Azure önbelleği için Redis özellikleri
Aşağıdaki özellikler, oluşturma ve Azure Cache, Redis örneği için güncelleştirme olduğunda kullanılır.

| Özellik | Anahtar | Açıklama |
| --- | --- | --- |
| name |-n, --name |Adı Azure önbelleği için Redis. |
| kaynak grubu |g-,--resource-group |Kaynak grubunun adı. |
| location |m-,--konum |Önbellek oluşturmak için konum. |
| size |-z, --size |Azure önbelleği için boyutunu Redis. Geçerli değerler: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4] |
| SKU |-x, --sku |Redis SKU. Şunlardan biri olmalıdır: [temel, standart, Premium] |
| EnableNonSslPort |-e, --enable-non-ssl-port |Azure önbelleği için Redis EnableNonSslPort özelliği. Bu bayrağı olmayan SSL bağlantı noktası önbellek hesabınız için etkinleştirmek istiyorsanız ekleyin |
| Redis yapılandırması |-c, --redis-configuration |Redis yapılandırması. Yapılandırma anahtarları ve değerleri burada içeren bir JSON biçimli dizeyi girin. Biçim: "{" ":""," ":" "}" |
| Redis yapılandırması |f-,--redis yapılandırma dosyası |Redis yapılandırması. Yapılandırma anahtarları ve değerleri burada içeren bir dosyanın yolunu girin. Dosya giriş biçimi: {"": "","": ""} |
| Parça sayısı |r-,--parça sayısı |Kümeleme ile Premium küme önbelleği oluşturmak için parça sayısı. |
| Sanal Ağ |-v, --virtual-network |Azure Cache için dağıtmak için tam ARM kaynak kimliği sanal ağın belirtir bir VNET önbelleğinize barındırırken, Redis. Örnek Biçim: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Anahtar türü |t-,--anahtar türü |Yenilemek için anahtar türü. Geçerli değerler: [birincil, ikincil] |
| StaticIP |-p, --static-ip \<static-ip\> |Bir vnet'teki önbelleğinizi barındırırken, benzersiz bir IP adresi alt ağ için önbellek belirtir. Sağlanmazsa, bir, alt ağdan seçilir. |
| Alt ağ |t,--alt \<alt ağ\> |Bir vnet'teki önbelleğinizi barındırırken, önbellek dağıtacağınız bir alt ağ adını belirtir. |
| VirtualNetwork |v-,--sanal ağ \<sanal ağ\> |Azure Cache için dağıtmak için tam ARM kaynak kimliği sanal ağın belirtir bir VNET önbelleğinize barındırırken, Redis. Örnek Biçim: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Abonelik |s-,--abonelik |Abonelik tanımlayıcısı. |

## <a name="see-all-azure-cache-for-redis-commands"></a>Bkz: tüm Azure önbelleği için Redis komutları
Tüm Azure önbelleği için Redis komutları ve parametreleri görmek için `azure rediscache -h` komutu.

    C:\>azure rediscache -h
    help:    Commands to manage your Azure Cache for Redis(s)
    help:
    help:    Create an Azure Cache for Redis
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Azure Cache for Redis
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Azure Cache for Redis within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Azure Cache for Redis
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Azure Cache for Redis
    help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Renew the authentication key for an existing Azure Cache for Redis
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Azure Cache for Redis
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-an-azure-cache-for-redis"></a>Bir Azure önbelleği için Redis oluşturma
Bir Azure önbelleği için Redis oluşturmak için aşağıdaki komutu kullanın:

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

Bu komut hakkında daha fazla bilgi için `azure rediscache create -h` komutu.

    C:\>azure rediscache create -h
    help:    Create an Azure Cache for Redis
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Azure Cache for Redis.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -l, --location <location>                                Location to create cache.
    help:      -z, --size <size>                                        Size of the Azure Cache for Redis. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of the Azure Cache for Redis. Add this flag if you want to enable the Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here. Format for the file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards to create on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  The exact ARM resource ID of the virtual network to deploy the Azure Cache for Redis in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying an Azure Cache for Redis inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying an Azure Cache for Redis inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-azure-cache-for-redis"></a>Mevcut bir Azure önbelleği için Redis Sil
Bir Azure önbelleği için Redis silmek için aşağıdaki komutu kullanın:

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

Bu komut hakkında daha fazla bilgi için `azure rediscache delete -h` komutu.

    C:\>azure rediscache delete -h
    help:    Delete an existing Azure Cache for Redis
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Azure Cache for Redis.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-azure-cache-for-redis-within-your-subscription-or-resource-group"></a>Tüm Azure Cache, Redis abonelik veya kaynak grubu için liste
Tüm Azure Cache, Redis abonelik veya kaynak grubu için listelemek için aşağıdaki komutu kullanın:

    azure rediscache list [options]

Bu komut hakkında daha fazla bilgi için `azure rediscache list -h` komutu.

    C:\>azure rediscache list -h
    help:    List all Azure Cache for Redis within your Subscription or Resource Group
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

## <a name="show-properties-of-an-existing-azure-cache-for-redis"></a>Özellikler var olan bir Azure önbelleği için Redis Göster
Özellikler var olan bir Azure önbelleği için Redis göstermek için aşağıdaki komutu kullanın:

    azure rediscache show [--name <name> --resource-group <resource-group>]

Bu komut hakkında daha fazla bilgi için `azure rediscache show -h` komutu.

    C:\>azure rediscache show -h
    help:    Show properties of an existing Azure Cache for Redis
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Azure Cache for Redis.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>

## <a name="change-settings-of-an-existing-azure-cache-for-redis"></a>Mevcut bir Azure önbelleği için Redis ayarlarını değiştirme
Mevcut bir Azure önbelleği için Redis ayarlarını değiştirmek için aşağıdaki komutu kullanın:

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

Bu komut hakkında daha fazla bilgi için `azure rediscache set -h` komutu.

    C:\>azure rediscache set -h
    help:    Change settings of an existing Azure Cache for Redis
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Azure Cache for Redis.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-the-authentication-key-for-an-existing-azure-cache-for-redis"></a>Kimlik doğrulama anahtarı, mevcut bir Azure önbelleği için Redis için yenileyin.
Kimlik doğrulama anahtarı için mevcut bir Azure önbelleği için Redis yenilemek için aşağıdaki komutu kullanın:

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

Belirtin `Primary` veya `Secondary` için `key-type`.

Bu komut hakkında daha fazla bilgi için `azure rediscache renew-key -h` komutu.

    C:\>azure rediscache renew-key -h
    help:    Renew the authentication key for an existing Azure Cache for Redis
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Azure Cache for Redis.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key to renew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-azure-cache-for-redis"></a>Redis için mevcut bir Azure önbelleği listesi birincil ve ikincil anahtarları
Birincil ve ikincil anahtarlarını Listele var olan bir Azure önbelleği için Redis için aşağıdaki komutu kullanın:

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

Bu komut hakkında daha fazla bilgi için `azure rediscache list-keys -h` komutu.

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Azure Cache for Redis
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Azure Cache for Redis.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)
