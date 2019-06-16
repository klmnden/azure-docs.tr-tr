---
title: Azure CLI komutları Resource Manager modunda | Microsoft Docs
description: Resource Manager dağıtım modelinde kaynaklarınızı yönetmek için azure komut satırı arabirimi (CLI) komutlarını
services: virtual-machines-linux,virtual-machines-windows,virtual-network,mobile-services,cloud-services
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: be37da5b-72fe-41a1-9fa0-8937b69464ec
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: command-line-interface
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: danlep
ms.openlocfilehash: ac1a6136eae79700c427b27c8869bc9241c62abc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64717792"
---
# <a name="azure-cli-commands-in-resource-manager-mode"></a>Resource Manager modunda Azure CLI komutları
Bu makalede, Azure komut satırı arabirimi (CLI) komutlarını Azure Resource Manager dağıtım modelinde Azure kaynaklarını oluşturmak ve yönetmek için yaygın olarak kullanacağınız için sözdizimini ve seçenekleri sağlar. Bu komutlar, CLI'yı Resource Manager (arm) modunda çalıştırarak erişin. Bu tam bir başvuru değildir ve CLI sürümünüzü biraz farklı komutları ya da parametreler gösterebilir. Azure kaynaklarını ve kaynak gruplarını genel bir bakış için bkz. [Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md).  

> [!NOTE]
> Bu makalede Resource Manager modunda komutları Azure CLI'da gösterilmektedir Azure Klasik CLI olarak da adlandırılır. Resource Manager modelinde çalışmak için de deneyebilirsiniz [Azure CLI](/cli/azure/install-az-cli2), bizim İleri nesil çok platformlu CLI.
>Hakkında daha fazla bilgi edinin [eski ve yeni Azure Clı'yi](/cli/azure/old-and-new-clis).
>

, İlk kez kullanmaya başlamak için [Azure CLI'yı yükleme](../cli-install-nodejs.md) ve [Azure aboneliğinize bağlanma](/cli/azure/authenticate-azure-cli).

Geçerli komut sözdizimi ve Resource Manager modunda komut satırı seçenekleri için yazın `azure help` veya belirli bir komut için Yardım görüntülemek üzere `azure help [command]`. Ayrıca CLI örnekleri oluşturmak ve belirli Azure hizmetlerinin yönetilmesi için belgelerde bulun.

İsteğe bağlı parametreler köşeli ayraç içinde gösterilir (örneğin, `[parameter]`). Diğer tüm parametreler gereklidir.

Burada belgelenmektedir komuta özgü isteğe bağlı parametrelerin yanı sıra, istek seçenekleri ve durum kodları gibi ayrıntılı çıkışını görüntülemek için kullanılan üç isteğe bağlı parametre yok. `-v` Ayrıntılı çıkış parametresi sağlar ve `-vv` parametresini sağlayan daha da ayrıntılı ayrıntılı çıkış. `--json` Seçeneği sonucu ham json biçiminde çıkarır.

## <a name="setting-the-resource-manager-mode"></a>Resource Manager modunu ayarlama
Azure CLI ve Resource Manager modunda komutları etkinleştirmek için aşağıdaki komutu kullanın.

    azure config mode arm

> [!NOTE]
> CLI'ın Azure Resource Manager moduna ve Azure Hizmet Yönetimi modunda birbirini dışlar. Diğer bir deyişle, bir modda oluşturulan kaynakları diğer modundan yönetilemez.
> 
> 

## <a name="azure-account-manage-your-account-information"></a>Azure hesabı: Hesap bilgilerini yönetme
Azure abonelik bilgilerinizi hesabınıza bağlanmak için araç tarafından kullanılır.

**İçeri aktarılan abonelikleri listeleyin**

    account list [options]

**Bir abonelik ayrıntılarını göster**  

    account show [options] [subscriptionNameOrId]

**Geçerli abonelik ayarlayın**

    account set [options] <subscriptionNameOrId>

**Bir abonelik veya ortam kaldırın veya tüm saklı hesabınızı ve ortamınızı bilgilerini Temizle**  

    account clear [options]

**Hesap ortamınızı yönetmek için komutlar**  

    account env list [options]
    account env show [options] [environment]
    account env add [options] [environment]
    account env set [options] [environment]
    account env delete [options] [environment]

## <a name="azure-ad-commands-to-display-active-directory-objects"></a>Azure ad: Active Directory nesneleri görüntülemek için komutları
**Active directory uygulamaları görüntülemek için komutları**

    ad app create [options]
    ad app delete [options] <object-id>

**Active directory gruplarını görüntülemek için komutları**

    ad group list [options]
    ad group show [options]

**Bir active directory alt grubu veya üye bilgilerini sağlamak için komutları**

    ad group member list [options] [objectId]

**Active directory hizmet sorumluları görüntülenecek komutları**

    ad sp list [options]
    ad sp show [options]
    ad sp create [options] <application-id>
    ad sp delete [options] <object-id>

**Active directory kullanıcıları görüntülemek için komutları**

    ad user list [options]
    ad user show [options]

## <a name="azure-availset-commands-to-manage-your-availability-sets"></a>Azure availset: komutları, kullanılabilirlik kümelerini Yönet
**Bir kullanılabilirlik kümesi içinde bir kaynak grubu oluşturur**

    availset create [options] <resource-group> <name> <location> [tags]

**Bir kaynak grubu içindeki kullanılabilirlik kümelerini listeler**

    availset list [options] <resource-group>

**Bir kullanılabilirlik kümesi içinde bir kaynak grubu alır**

    availset show [options] <resource-group> <name>

**Bir kullanılabilirlik kümesi içinde bir kaynak grubu siler**

    availset delete [options] <resource-group> <name>

## <a name="azure-config-commands-to-manage-your-local-settings"></a>Azure yapılandırma: yerel ayarlarınızı yönetmek için komutlar
**Azure CLI yapılandırma ayarları listesi**

    config list [options]

**Bir yapılandırma ayarı Sil**

    config delete [options] <name>

**Bir yapılandırma ayarını güncelleştirme**

    config set <name> <value>

**Azure CLI çalışma modu için ya da ayarlar `arm` veya `asm`**

    config mode [options] <modename>


## <a name="azure-feature-commands-to-manage-account-features"></a>Azure özelliği: hesap özellikleri yönetmek için komutlar
**Aboneliğiniz için kullanılabilir olan tüm özellikleri listesi**

    feature list [options]

**Bir özellik gösterir**

    feature show [options] <providerName> <featureName>

**Bir kaynak sağlayıcısının önizlenen bir özelliğini kaydeder**

    feature register [options] <providerName> <featureName>

## <a name="azure-group-commands-to-manage-your-resource-groups"></a>Azure Grup: Kaynak gruplarınızı yönetmek için komutlar
**Bir kaynak grubu oluşturur**

    group create [options] <name> <location>

**Bir kaynak grubuna etiketlerini ayarlama**

    group set [options] <name> <tags>

**Bir kaynak grubunu siler**

    group delete [options] <name>

**Aboneliğiniz için kaynak gruplarını listeler**

    group list [options]

**Aboneliğiniz için bir kaynak grubu gösterir**

    group show [options] <name>

**Kaynak grubu günlüklerini yönetmek için kullanılan komutlar**

    group log show [options] [name]

**Bir kaynak grubu, dağıtımınızdaki yönetecek komutlar**

    group deployment create [options] [resource-group] [name]
    group deployment list [options] <resource-group> [state]
    group deployment show [options] <resource-group> [deployment-name]
    group deployment stop [options] <resource-group> [deployment-name]

**Yerel veya galeri kaynak grubu şablonu yönetecek komutlar**

    group template list [options]
    group template show [options] <name>
    group template download [options] [name] [file]
    group template validate [options] <resource-group>

## <a name="azure-hdinsight-commands-to-manage-your-hdinsight-clusters"></a>Azure hdınsight: HDInsight kümelerinizi yönetmek için komutlar
**Komutları oluşturmak veya bir küme yapılandırma dosyası eklemek için**

    hdinsight config create [options] <configFilePath> <overwrite>
    hdinsight config add-config-values [options] <configFilePath>
    hdinsight config add-script-action [options] <configFilePath>

Örnek: Bir küme oluşturma sırasında çalıştırmak için betik eylemi içeren bir yapılandırma dosyası oluşturun.

    hdinsight config create "C:\myFiles\configFile.config"
    hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

**Bir kaynak grubunda bir küme oluşturmak için komutu**

    hdinsight cluster create [options] <clusterName>

Örnek: Bir Storm Linux kümesi üzerinde oluşturma

    azure hdinsight cluster create -g myarmgroup -l westus -y Linux --clusterType Storm --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 myNewCluster01

    info:    Executing command hdinsight cluster create
    + Submitting the request to create cluster...
    info:    hdinsight cluster create command OK

Örnek: Betik eylemi ile küme oluşturma

    azure hdinsight cluster create -g myarmgroup -l westus -y Linux --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01

    info:    Executing command hdinsight cluster create
    + Submitting the request to create cluster...
    info:    hdinsight cluster create command OK

Parametre seçenekleri:

    -h, --help                                                 output usage information
    -v, --verbose                                              use verbose output
    -vv                                                        more verbose with debug output
    --json                                                     use json output
    -g --resource-group <resource-group>                       The name of the resource group
    -c, --clusterName <clusterName>                            HDInsight cluster name
    -l, --location <location>                                  Data center location for the cluster
    -y, --osType <osType>                                      HDInsight cluster operating system
    'Windows' or 'Linux'
    --version <version>                                        HDInsight cluster version
    --clusterType <clusterType>                                HDInsight cluster type.
    Hadoop | HBase | Spark | Storm
    --defaultStorageAccountName <storageAccountName>           Storage account url to use for default HDInsight storage
    --defaultStorageAccountKey <storageAccountKey>             Key to the storage account to use for default HDInsight storage
    --defaultStorageContainer <storageContainer>               Container in the storage account to use for HDInsight default storage
    --headNodeSize <headNodeSize>                              (Optional) Head node size for the cluster
    --workerNodeCount <workerNodeCount>                        Number of worker nodes to use for the cluster
    --workerNodeSize <workerNodeSize>                          (Optional) Worker node size for the cluster)
    --zookeeperNodeSize <zookeeperNodeSize>                    (Optional) Zookeeper node size for the cluster
    --userName <userName>                                      Cluster username
    --password <password>                                      Cluster password
    --sshUserName <sshUserName>                                SSH username (only for Linux clusters)
    --sshPassword <sshPassword>                                SSH password (only for Linux clusters)
    --sshPublicKey <sshPublicKey>                              SSH public key (only for Linux clusters)
    --rdpUserName <rdpUserName>                                RDP username (only for Windows clusters)
    --rdpPassword <rdpPassword>                                RDP password (only for Windows clusters)
    --rdpAccessExpiry <rdpAccessExpiry>                        RDP access expiry.
    For example 12/12/2015 (only for Windows clusters)
    --virtualNetworkId <virtualNetworkId>                      (Optional) Virtual network ID for the cluster.
    Value is a GUID for Windows cluster and ARM resource ID for Linux cluster)
    --subnetName <subnetName>                                  (Optional) Subnet for the cluster
    --additionalStorageAccounts <additionalStorageAccounts>    (Optional) Additional storage accounts.
    Can be multiple.
    In the format of 'accountName#accountKey'.
    For example, --additionalStorageAccounts "acc1#key1;acc2#key2"
    --hiveMetastoreServerName <hiveMetastoreServerName>        (Optional) SQL Server name for the external metastore for Hive
    --hiveMetastoreDatabaseName <hiveMetastoreDatabaseName>    (Optional) Database name for the external metastore for Hive
    --hiveMetastoreUserName <hiveMetastoreUserName>            (Optional) Database username for the external metastore for Hive
    --hiveMetastorePassword <hiveMetastorePassword>            (Optional) Database password for the external metastore for Hive
    --oozieMetastoreServerName <oozieMetastoreServerName>      (Optional) SQL Server name for the external metastore for Oozie
    --oozieMetastoreDatabaseName <oozieMetastoreDatabaseName>  (Optional) Database name for the external metastore for Oozie
    --oozieMetastoreUserName <oozieMetastoreUserName>          (Optional) Database username for the external metastore for Oozie
    --oozieMetastorePassword <oozieMetastorePassword>          (Optional) Database password for the external metastore for Oozie
    --configurationPath <configurationPath>                    (Optional) HDInsight cluster configuration file path
    -s, --subscription <id>                                    The subscription id
    --tags <tags>                                              Tags to set to the cluster.
    Can be multiple.
    In the format of 'name=value'.
    Name is required and value is optional.
    For example, --tags tag1=value1;tag2


**Komutunu kullanarak küme silme**

    hdinsight cluster delete [options] <clusterName>

**Küme Ayrıntıları Göster komutu**

    hdinsight cluster show [options] <clusterName>

**Tüm kümelerde (sağlanırsa, belirli bir kaynak grubu,) listelemek için komut**

    hdinsight cluster list [options]

**Bir küme yeniden boyutlandırmak için komutu**

    hdinsight cluster resize [options] <clusterName> <targetInstanceCount>

**Bir küme için HTTP erişimi etkinleştirmek için komutu**

    hdinsight cluster enable-http-access [options] <clusterName> <userName> <password>

**Bir küme için HTTP erişimi devre dışı bırakma komutu**

    hdinsight cluster disable-http-access [options] <clusterName>

**Bir kümenin RDP erişimini etkinleştirmek için komutu**

    hdinsight cluster enable-rdp-access [options] <clusterName> <rdpUserName> <rdpPassword> <rdpExpiryDate>

**Bir küme için HTTP erişimi devre dışı bırakma komutu**

    hdinsight cluster disable-rdp-access [options] <clusterName>

## <a name="azure-insights-commands-related-to-monitoring-insights-events-alert-rules-autoscale-settings-metrics"></a>Azure ınsights: Öngörüler (olayları, uyarı kuralları, otomatik ölçeklendirme ayarları, ölçümleri) izleme ile ilgili komutları
**Bir abonelik, bir bağıntı kimliği, bir kaynak grubu, kaynak veya kaynak sağlayıcısı için işlem günlüklerini alma**

    insights logs list [options]

## <a name="azure-location-commands-to-get-the-available-locations-for-all-resource-types"></a>Azure konum: Tüm kaynak türleri için kullanılabilir konumları Al komutları
**Kullanılabilir konumların listesi**

    location list [options]

## <a name="azure-network-commands-to-manage-network-resources"></a>Azure ağı: Ağ kaynaklarını yönetmek için komutlar
**Sanal ağlar'ı yönetmek için komutlar**

    network vnet create [options] <resource-group> <name> <location>
Sanal ağ oluşturur. Aşağıdaki örnekte Batı ABD bölgesinde myresourcegroup kaynak grubu için newvnet adlı bir sanal ağ oluşturacağız.

    azure network vnet create myresourcegroup newvnet "west us"
    info:    Executing command network vnet create
    + Looking up virtual network "newvnet"
    + Creating virtual network "newvnet"
     Loading virtual network state
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet
    data:    Name:                 newvnet
    data:    Type:                 Microsoft.Network/virtualNetworks
    data:    Location:             westus
    data:    Tags:
    data:    Provisioning state:   Succeeded
    data:    Address prefixes:
    data:     10.0.0.0/8
    data:    DNS servers:
    data:    Subnets:
    data:
    info:    network vnet create command OK


Parametre seçenekleri:

     -h, --help                                 output usage information
     -v, --verbose                              use verbose output
    --json                                     use json output
     -g, --resource-group <resource-group>      the name of the resource group
     -n, --name <name>                          the name of the virtual network
     -l, --location <location>                  the location
     -a, --address-prefixes <address-prefixes>  the comma separated list of address prefixes for this virtual network
      For example -a 10.0.0.0/24,10.0.1.0/24.
      Default value is 10.0.0.0/8

    -d, --dns-servers <dns-servers>            the comma separated list of DNS servers IP addresses
     -t, --tags <tags>                          the tags set on this virtual network.
      Can be multiple. In the format of "name=value".
      Name is required and value is optional.
      For example, -t tag1=value1;tag2
     -s, --subscription <subscription>          the subscription identifier
<BR>

    network vnet set [options] <resource-group> <name>

Bir kaynak grubu içindeki bir sanal ağ yapılandırmasını güncelleştirir.

    azure network vnet set myresourcegroup newvnet

    info:    Executing command network vnet set
    + Looking up virtual network "newvnet"
    + Updating virtual network "newvnet"
    + Loading virtual network state
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet
    data:    Name:                 newvnet
    data:    Type:                 Microsoft.Network/virtualNetworks
    data:    Location:             westus
    data:    Tags:
    data:    Provisioning state:   Succeeded
    data:    Address prefixes:
    data:     10.0.0.0/8
    data:    DNS servers:
    data:    Subnets:
    data:
    info:    network vnet set command OK

Parametre seçenekleri:

       -h, --help                                 output usage information
       -v, --verbose                              use verbose output
       --json                                     use json output
       -g, --resource-group <resource-group>      the name of the resource group
       -n, --name <name>                          the name of the virtual network
       -a, --address-prefixes <address-prefixes>  the comma separated list of address prefixes for this virtual network.
        For example -a 10.0.0.0/24,10.0.1.0/24.
        This list will be appended to the current list of address prefixes.
        The address prefixes in this list should not overlap between them.
        The address prefixes in this list should not overlap with existing address prefixes in the vnet.

       -d, --dns-servers [dns-servers]            the comma separated list of DNS servers IP addresses.
        This list will be appended to the current list of DNS server IP addresses.

       -t, --tags <tags>                          the tags set on this virtual network.
        Can be multiple. In the format of "name=value".
        Name is required and value is optional. For example, -t tag1=value1;tag2.
        This list will be appended to the current list of tags

       --no-tags                                  remove all existing tags
       -s, --subscription <subscription>          the subscription identifier
<BR>

    network vnet list [options] <resource-group>

Komut bir kaynak grubundaki tüm sanal ağları listeler.

    C:\>azure network vnet list myresourcegroup

    info:    Executing command network vnet list
    + Listing virtual networks
        data:    ID
       Name      Location  Address prefixes  DNS servers
    data:    -------------------------------------------------------------------
    ------  --------  --------  ----------------  -----------
    data:    /subscriptions/###############################/resourceGroups/
    wvnet   newvnet   westus    10.0.0.0/8
    info:    network vnet list command OK

Parametre seçenekleri:

      -h, --help                             output usage information
      -v, --verbose                          use verbose output
      --json                                 use json output
      -g, --resource-group <resource-group>  the name of the resource group
      -s, --subscription <subscription>      the subscription identifier

<BR>

    network vnet show [options] <resource-group> <name>
Komutu, bir kaynak grubunda sanal ağ özelliklerini gösterir.

    azure network vnet show -g myresourcegroup -n newvnet

    info:    Executing command network vnet show
    + Looking up virtual network "newvnet"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet
    data:    Name:                 newvnet
    data:    Type:                 Microsoft.Network/virtualNetworks
    data:    Location:             westus
    data:    Tags:
    data:    Provisioning state:   Succeeded
    data:    Address prefixes:
    data:     10.0.0.0/8
    data:    DNS servers:
    data:    Subnets:
    data:
    info:    network vnet show command OK
<BR>

    network vnet delete [options] <resource-group> <name>
Komutu, bir sanal ağ kaldırır.

    azure network vnet delete myresourcegroup newvnetX

    info:    Executing command network vnet delete
    + Looking up virtual network "newvnetX"
    Delete virtual network newvnetX? [y/n] y
    + Deleting virtual network "newvnetX"
    info:    network vnet delete command OK

Parametre seçenekleri:

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  the name of the resource group
     -n, --name <name>                      the name of the virtual network
     -q, --quiet                            quiet mode, do not ask for delete confirmation
     -s, --subscription <subscription>      the subscription identifier


**Sanal yönetecek komutlar alt ağ**

    network vnet subnet create [options] <resource-group> <vnet-name> <name>

Başka bir alt ağ, mevcut bir sanal ağa ekler.

    azure network vnet subnet create -g myresourcegroup --vnet-name newvnet -n subnet --address-prefix 10.0.1.0/24

    info:    Executing command network vnet subnet create
    + Looking up the subnet "subnet"
    + Creating subnet "subnet"
    + Looking up the subnet "subnet"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet
    data:    Name:                      subnet
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet create command OK

Parametre seçenekleri:

     -h, --help                                                       output usage information
     -v, --verbose                                                    use verbose output
         --json                                                           use json output
     -g, --resource-group <resource-group>                            the name of the resource group
     -e, --vnet-name <vnet-name>                                      the name of the virtual network
     -n, --name <name>                                                the name of the subnet
     -a, --address-prefix <address-prefix>                            the address prefix
     -w, --network-security-group-id <network-security-group-id>      the network security group identifier.
           e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/networkSecurityGroups/<nsg-name>
     -o, --network-security-group-name <network-security-group-name>  the network security group name
     -s, --subscription <subscription>                                the subscription identifier

<BR>

    network vnet subnet set [options] <resource-group> <vnet-name> <name>

Bir kaynak grubu içindeki bir özel sanal ağ alt ayarlar.

    C:\>azure network vnet subnet set -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet set
    + Looking up the subnet "subnet1"
    + Setting subnet "subnet1"
    + Looking up the subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet set command OK
<BR>

    network vnet subnet list [options] <resource-group> <vnet-name>

Bir kaynak grubu içindeki belirli bir sanal ağ için tüm sanal ağ alt ağları listeler.

    azure network vnet subnet set -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet set
    + Looking up the subnet "subnet1"
    + Setting subnet "subnet1"
    + Looking up the subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet set command OK
<BR>

    network vnet subnet show [options] <resource-group> <vnet-name> <name>
Sanal ağ alt ağı özelliklerini görüntüler

    azure network vnet subnet show -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet show
    + Looking up the subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft
    .Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet show command OK

Parametre seçenekleri:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -e, --vnet-name <vnet-name>            the name of the virtual network
    -n, --name <name>                      the name of the subnet
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network vnet subnet delete [options] <resource-group> <vnet-name> <subnet-name>
Bir alt ağ, var olan bir sanal ağdan kaldırır.

    azure network vnet subnet delete -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet delete
    + Looking up the subnet "subnet1"
    Delete subnet "subnet1"? [y/n] y
    + Deleting subnet "subnet1"
    info:    network vnet subnet delete command OK

Parametre seçenekleri:

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  the name of the resource group
     -e, --vnet-name <vnet-name>            the name of the virtual network
     -n, --name <name>                      the subnet name
     -s, --subscription <subscription>      the subscription identifier
     -q, --quiet                            quiet mode, do not ask for delete confirmation

**Yük Dengeleyiciler yönetecek komutlar**

    network lb create [options] <resource-group> <name> <location>
Bir yük dengeleyici kümesi oluşturur.

    azure network lb create -g myresourcegroup -n mylb -l westus

    info:    Executing command network lb create
    + Looking up the load balancer "mylb"
    + Creating load balancer "mylb"
    + Looking up the load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
    data:    Name:                         mylb
    data:    Type:                         Microsoft.Network/loadBalancers
    data:    Location:                     westus
    data:    Provisioning state:           Succeeded
    info:    network lb create command OK

Parametre seçenekleri:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -n, --name <name>                      the name of the load balancer
    -l, --location <location>              the location
    -t, --tags <tags>                      the list of tags.
     Can be multiple. In the format of "name=value".
     Name is required and value is optional. For example, -t tag1=value1;tag2
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb list [options] <resource-group>
Bir kaynak grubu içindeki yük dengeleyici kaynakları listeler.

    azure network lb list myresourcegroup

    info:    Executing command network lb list
    + Getting the load balancers
    data:    Name  Location
    data:    ----  --------
    data:    mylb  westus
    info:    network lb list command OK

Parametre seçenekleri:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb show [options] <resource-group> <name>

Yük Dengeleyici bilgileri, bir kaynak grubu içindeki belirli bir yük dengeleyici görüntüler

    azure network lb show myresourcegroup mylb -v

    info:    Executing command network lb show
    verbose: Looking up the load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
    data:    Name:                         mylb
    data:    Type:                         Microsoft.Network/loadBalancers
    data:    Location:                     westus
    data:    Provisioning state:           Succeeded
    info:    network lb show command OK

Parametre seçenekleri:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -n, --name <name>                      the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier

<BR>

    network lb delete [options] <resource-group> <name>

Yük Dengeleyici kaynakları silin.

    azure network lb delete  myresourcegroup mylb

    info:    Executing command network lb delete
    + Looking up the load balancer "mylb"
    Delete load balancer "mylb"? [y/n] y
    + Deleting load balancer "mylb"
    info:    network lb delete command OK

Parametre seçenekleri:

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  the name of the resource group
     -n, --name <name>                      the name of the load balancer
     -q, --quiet                            quiet mode, do not ask for delete confirmation
     -s, --subscription <subscription>      the subscription identifier

**Bir yük dengeleyici araştırmalarını yönetecek komutlar**

    network lb probe create [options] <resource-group> <lb-name> <name>

Sistem durumu araştırması yapılandırması yük dengeleyicisi oluşturun. Bu komutu çalıştırmak için göz önünde bulundurun, load balancer'ınız bir ön uç IP kaynağı (yük dengeleyici için bir IP adresi atamak için "azure network frontend-ip" komutu denetimi) gerektirir.

    azure network lb probe create -g myresourcegroup --lb-name mylb -n mylbprobe --protocol tcp --port 80 -i 300

    info:    Executing command network lb probe create
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    info:    network lb probe create command OK

Parametre seçenekleri:

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the probe
    -p, --protocol <protocol>              the probe protocol
    -o, --port <port>                      the probe port
    -f, --path <path>                      the probe path
    -i, --interval <interval>              the probe interval in seconds
    -c, --count <count>                    the number of probes
    -s, --subscription <subscription>      the subscription identifier

<BR>

    network lb probe set [options] <resource-group> <lb-name> <name>

Mevcut bir yük dengeleyici araştırması için yeni değerleri ile güncelleştirir.

    azure network lb probe set -g myresourcegroup -l mylb -n mylbprobe -p mylbprobe1 -p TCP -o 443 -i 300

    info:    Executing command network lb probe set
        + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    info:    network lb probe set command OK

Parametre seçenekleri

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the probe
    -e, --new-probe-name <new-probe-name>  the new name of the probe
    -p, --protocol <protocol>              the new value for probe protocol
    -o, --port <port>                      the new value for probe port
    -f, --path <path>                      the new value for probe path
    -i, --interval <interval>              the new value for probe interval in seconds
    -c, --count <count>                    the new value for number of probes
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb probe list [options] <resource-group> <lb-name>

Bir yük dengeleyici kümesi araştırma özelliklerini listeler.

    C:\>azure network lb probe list -g myresourcegroup -l mylb

    info:    Executing command network lb probe list
    + Looking up the load balancer "mylb"
    data:    Name       Protocol  Port  Path  Interval  Count
    data:    ---------  --------  ----  ----  --------  -----
    data:    mylbprobe  Tcp       443         300       2
    info:    network lb probe list command OK

Parametre seçenekleri:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier


    network lb probe delete [options] <resource-group> <lb-name> <name>
Yük Dengeleyici için oluşturduğunuz araştırmayı kaldırır.

    azure network lb probe delete -g myresourcegroup -l mylb -n mylbprobe

    info:    Executing command network lb probe delete
    + Looking up the load balancer "mylb"
    Delete a probe "mylbprobe?" [y/n] y
    + Updating load balancer "mylb"
    info:    network lb probe delete command OK

**Bir yük dengeleyici ön uç IP yapılandırmalarını yönetecek komutlar**

    network lb frontend-ip create [options] <resource-group> <lb-name> <name>
Mevcut bir yük dengeleyici kümesi için bir ön uç IP yapılandırması oluşturur.

    azure network lb frontend-ip create -g myresourcegroup --lb-name mylb -n myfrontendip -o Dynamic -e subnet -m newvnet

    info:    Executing command network lb frontend-ip create
    + Looking up the load balancer "mylb"
    + Looking up the subnet "subnet"
    + Creating frontend IP configuration "myfrontendip"
    + Looking up the load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/Myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
    /frontendIPConfigurations/myfrontendip
    data:    Name:                         myfrontendip
    data:    Type:                         Microsoft.Network/loadBalancers/frontendIPConfigurations
    data:    Provisioning state:           Succeeded
    data:    Private IP allocation method: Dynamic
    data:    Private IP address:           10.0.1.4
    data:    Subnet:                       id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet
    data:    Public IP address:
    data:    Inbound NAT rules
    data:    Outbound NAT rules
    data:    Load balancing rules
    data:
    info:    network lb frontend-ip create command OK

<BR>

    network lb frontend-ip set [options] <resource-group> <lb-name> <name>

Mevcut bir ön uç IP yapılandırmasını güncelleştirir. Aşağıdaki komutu mypubip5 myfrontendip adlı var olan yük dengeleyici ön uç IP için adlı bir genel IP ekler.

    azure network lb frontend-ip set -g myresourcegroup --lb-name mylb -n myfrontendip -i mypubip5

    info:    Executing command network lb frontend-ip set
    + Looking up the load balancer "mylb"
    + Looking up the public ip "mypubip5"
    + Updating load balancer "mylb"
    + Looking up the load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Name:                         myfrontendip
    data:    Type:                         Microsoft.Network/loadBalancers/frontendIPConfigurations
    data:    Provisioning state:           Succeeded
    data:    Private IP allocation method: Dynamic
    data:    Private IP address:
    data:    Subnet:
    data:    Public IP address:            id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mypubip5
    data:    Inbound NAT rules
    data:    Outbound NAT rules
    data:    Load balancing rules
    data:
    info:    network lb frontend-ip set command OK

Parametre seçenekleri:

    -h, --help                                                         output usage information
    -v, --verbose                                                      use verbose output
    --json                                                             use json output
    -g, --resource-group <resource-group>                              the name of the resource group
    -l, --lb-name <lb-name>                                            the name of the load balancer
    -n, --name <name>                                                  the name of the frontend ip configuration
    -a, --private-ip-address <private-ip-address>                      the private ip address
    -o, --private-ip-allocation-method <private-ip-allocation-method>  the private ip allocation method [Static, Dynamic]
    -u, --public-ip-id <public-ip-id>                                  the public ip identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/publicIPAddresses/<public-ip-name>
    -i, --public-ip-name <public-ip-name>                              the public ip name.
    This public ip must exist in the same resource group as the lb.
    Please use public-ip-id if that is not the case.
    -b, --subnet-id <subnet-id>                                        the subnet id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/VirtualNetworks/<vnet-name>/subnets/<subnet-name>
    -e, --subnet-name <subnet-name>                                    the subnet name
    -m, --vnet-name <vnet-name>                                        the virtual network name.
    This virtual network must exist in the same resource group as the lb.
    Please use subnet-id if that is not the case.
    -s, --subscription <subscription>                                  the subscription identifier

<BR>

    network lb frontend-ip list [options] <resource-group> <lb-name>

Yük Dengeleyici için yapılandırılmış tüm ön uç IP kaynakları listeler.

    azure network lb frontend-ip list -g myresourcegroup -l mylb

    info:    Executing command network lb frontend-ip list
    + Looking up the load balancer "mylb"
    data:    Name         Provisioning state  Private IP allocation method  Subnet
    data:    -----------  ------------------  ----------------------------  ------
    data:    myprivateip  Succeeded           Dynamic
    info:    network lb frontend-ip list command OK

Parametre seçenekleri:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb frontend-ip delete [options] <resource-group> <lb-name> <name>
Yük Dengeleyici için ön uç IP nesnesini siler

    network lb frontend-ip delete -g myresourcegroup -l mylb -n myfrontendip
    info:    Executing command network lb frontend-ip delete
    + Looking up the load balancer "mylb"
    Delete frontend ip configuration "myfrontendip"? [y/n] y
    + Updating load balancer "mylb"

Parametre seçenekleri:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the frontend ip configuration
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier

**Bir yük dengeleyicinin arka uç adres havuzları yönetmek için komutlar**

    network lb address-pool create [options] <resource-group> <lb-name> <name>

Bir yük dengeleyici için arka uç adres havuzu oluşturun.

    azure network lb address-pool create -g myresourcegroup --lb-name mylb -n myaddresspool

    info:    Executing command network lb address-pool create
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up the load balancer "mylb"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourgroup/providers/Microso.Network/loadBalancers/mylb/backendAddressPools/myaddresspool
    data:    Name:                      myaddresspool
    data:    Type:                      Microsoft.Network/loadBalancers/backendAddressPools
    data:    Provisioning state:        Succeeded
    data:    Backend IP configurations:
    data:    Load balancing rules:
    data:
    info:    network lb address-pool create command OK

Parametre seçenekleri:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the backend address pool
    -s, --subscription <subscription>      the subscription identifier

<BR>

    network lb address-pool list [options] <resource-group> <lb-name>

Belirli bir kaynak grubu için arka uç IP adresi havuz aralığı listesi

    azure network lb address-pool list -g myresourcegroup -l mylb

    info:    Executing command network lb address-pool list
    + Looking up the load balancer "mylb"
    data:    Name           Provisioning state
    data:    -------------  ------------------
    data:    mybackendpool  Succeeded
    info:    network lb address-pool list command OK

Parametre seçenekleri:

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  the name of the resource group
     -l, --lb-name <lb-name>                the name of the load balancer
     -s, --subscription <subscription>      the subscription identifier

<BR>

    network lb address-pool delete [options] <resource-group> <lb-name> <name>

Arka uç IP havuzu aralığına kaynak yük dengeleyiciden kaldırır.

    azure network lb address-pool delete -g myresourcegroup -l mylb -n mybackendpool

    info:    Executing command network lb address-pool delete
    + Looking up the load balancer "mylb"
    Delete backend address pool "mybackendpool"? [y/n] y
    + Updating load balancer "mylb"
    info:    network lb address-pool delete command OK

Parametre seçenekleri:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the backend address pool
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier

**Yük Dengeleyici kuralları yönetmek için komutlar**

    network lb rule create [options] <resource-group> <lb-name> <name>
Yük Dengeleyici kuralları oluşturun.

Yük Dengeleyici ve gelen ağ trafiği almak için arka uç adres havuzu aralığı için ön uç uç nokta yapılandırma yük dengeleyici kuralı oluşturabilirsiniz. Ayarlar, arka uç adres havuzu aralığının bağlantı noktalarını ve ön uç IP uç noktası için bağlantı noktalarını da içerir.

Aşağıdaki örnek, bir yük dengeleyici kuralı, 80 TCP bağlantı noktası ve Yük Dengeleme ağ trafiğini arka uç adres havuzu aralığı için bağlantı noktası 8080 göndermek için dinleme ön uç uç noktası oluşturma işlemi gösterilmektedir.

    azure network lb rule create -g myresourcegroup -l mylb -n mylbrule -p tcp -f 80 -b 8080 -i 10


    info:    Executing command network lb rule create
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    + Loading rule state
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/loadBalancingRules/mylbrule
    data:    Name:                      mylbrule
    data:    Type:                      Microsoft.Network/loadBalancers/loadBalancingRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP configuration: /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend address pool:      id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
    data:    Protocol:                  Tcp
    data:    Frontend port:             80
    data:    Backend port:              8080
    data:    Enable floating IP:        false
    data:    Idle timeout in minutes:   10
    data:    Probes
    data:
    info:    network lb rule create command OK

<BR>

    network lb rule set [options] <resource-group> <lb-name> <name>

Belirli bir kaynak grubundaki var olan bir yük dengeleyici kuralı güncelleştirir. Aşağıdaki örnekte, kural adı mylbrule mynewlbrule değiştirdik.

    azure network lb rule set -g myresourcegroup -l mylb -n mylbrule -r mynewlbrule -p tcp -f 80 -b 8080 -i 10 -t myfrontendip -o mybackendpool

    info:    Executing command network lb rule set
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    + Loading rule state
    data:    Id:                        /subscriptions/###############################/resourceGroups/yresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/loadBalancingRules/mynewlbrule
    data:    Name:                      mynewlbrule
    data:    Type:                      Microsoft.Network/loadBalancers/loadBalancingRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP configuration: /subscriptions/###############################/resourceGroups/yresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend address pool:      id=/subscriptions/###############################/resourceGroups/yresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
    data:    Protocol:                  Tcp
    data:    Frontend port:             80
    data:    Backend port:              8080
    data:    Enable floating IP:        false
    data:    Idle timeout in minutes:   10
    data:    Probes
    data:
    info:    network lb rule set command OK

Parametre seçenekleri:

    -h, --help                                         output usage information
    -v, --verbose                                      use verbose output
    --json                                             use json output
    -g, --resource-group <resource-group>              the name of the resource group
    -l, --lb-name <lb-name>                            the name of the load balancer
    -n, --name <name>                                  the name of the rule
    -r, --new-rule-name <new-rule-name>                new rule name
    -p, --protocol <protocol>                          the rule protocol
    -f, --frontend-port <frontend-port>                the frontend port
    -b, --backend-port <backend-port>                  the backend port
    -e, --enable-floating-ip <enable-floating-ip>      enable floating point ip
    -i, --idle-timeout <idle-timeout>                  the idle timeout in minutes
    -a, --probe-name [probe-name]                      the name of the probe defined in the same load balancer
    -t, --frontend-ip-name <frontend-ip-name>          the name of the frontend ip configuration in the same load balancer
    -o, --backend-address-pool <backend-address-pool>  name of the backend address pool defined in the same load balancer
    -s, --subscription <subscription>                  the subscription identifier


    network lb rule list [options] <resource-group> <lb-name>

Tüm yük dengeleyici kuralları'ı bir yük dengeleyicide belirli bir kaynak grubu için yapılandırılmış.

    azure network lb rule list -g myresourcegroup -l mylb

    info:    Executing command network lb rule list
    + Looking up the load balancer "mylb"
    data:    Name         Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes  Backend address pool  Probe data

    data:    mynewlbrule  Succeeded           Tcp       80             8080          false               10                       /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
    info:    network lb rule list command OK

Parametre seçenekleri:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier

    network lb rule delete [options] <resource-group> <lb-name> <name>

Yük Dengeleyici kuralı siler.

    azure network lb rule delete -g myresourcegroup -l mylb -n mynewlbrule

    info:    Executing command network lb rule delete
    + Looking up the load balancer "mylb"
    Delete load balancing rule mynewlbrule? [y/n] y
    + Updating load balancer "mylb"
    info:    network lb rule delete command OK

Parametre seçenekleri:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the rule
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier

**Gelen NAT kuralları yük dengeleyici yönetecek komutlar**

    network lb inbound-nat-rule create [options] <resource-group> <lb-name> <name>
Yük Dengeleyici gelen NAT kuralı oluşturur.

Aşağıdaki örnekte bir NAT kuralı (Bu "azure network frontend-ip" komutunu kullanarak önceden tanımlanmış) ön uç IP bir gelen dinleme bağlantı noktası ve yük dengeleyici ağ trafiği göndermek için kullandığı giden bağlantı noktası ile oluşturduk.

    azure network lb inbound-nat-rule create -g myresourcegroup -l mylb -n myinboundnat -p tcp -f 80 -b 8080 -i myfrontendip

    info:    Executing command network lb inbound-nat-rule create
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up the load balancer "mylb"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/inboundNatRules/myinboundnat
    data:    Name:                      myinboundnat
    data:    Type:                      Microsoft.Network/loadBalancers/inboundNatRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP Configuration: id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend IP configuration
    data:    Protocol                   Tcp
    data:    Frontend port              80
    data:    Backend port               8080
    data:    Enable floating IP         false
    info:    network lb inbound-nat-rule create command OK

Parametre seçenekleri:

    -h, --help                                     output usage information
    -v, --verbose                                  use verbose output
    --json                                         use json output
    -g, --resource-group <resource-group>          the name of the resource group
    -l, --lb-name <lb-name>                        the name of the load balancer
    -n, --name <name>                              the name of the inbound NAT rule
    -p, --protocol <protocol>                      the rule protocol [tcp,udp]
    -f, --frontend-port <frontend-port>            the frontend port [0-65535]
    -b, --backend-port <backend-port>              the backend port [0-65535]
    -e, --enable-floating-ip <enable-floating-ip>  enable floating point ip [true,false]
    -i, --frontend-ip <frontend-ip>                the name of the frontend ip configuration
    -m, --vm-id <vm-id>                            the VM id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>
    -a, --vm-name <vm-name>                        the VM name.This VM must exist in the same resource group as the lb.
    Please use vm-id if that is not the case.
    this parameter will be ignored if --vm-id is specified
    -s, --subscription <subscription>              the subscription identifier
<BR>

    network lb inbound-nat-rule set [options] <resource-group> <lb-name> <name>
Varolan bir gelen nat kuralını güncelleştirir. Aşağıdaki örnekte, gelen dinleme bağlantı noktası 80'den için 81 değiştirdik.

    azure network lb inbound-nat-rule set -g group-1 -l mylb -n myinboundnat -p tcp -f 81 -b 8080 -i myfrontendip

    info:    Executing command network lb inbound-nat-rule set
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up the load balancer "mylb"
    data:    Id:                        /subscriptions/###############################/resourceGroups/group-1/providers/Microsoft.Network/loadBalancers/mylb/inboundNatRules/myinboundnat
    data:    Name:                      myinboundnat
    data:    Type:                      Microsoft.Network/loadBalancers/inboundNatRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP Configuration: id=/subscriptions/###############################/resourceGroups/group-1/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend IP configuration
    data:    Protocol                   Tcp
    data:    Frontend port              81
    data:    Backend port               8080
    data:    Enable floating IP         false
    info:    network lb inbound-nat-rule set command OK

Parametre seçenekleri:

    -h, --help                                     output usage information
    -v, --verbose                                  use verbose output
    --json                                         use json output
    -g, --resource-group <resource-group>          the name of the resource group
    -l, --lb-name <lb-name>                        the name of the load balancer
    -n, --name <name>                              the name of the inbound NAT rule
    -p, --protocol <protocol>                      the rule protocol [tcp,udp]
    -f, --frontend-port <frontend-port>            the frontend port [0-65535]
    -b, --backend-port <backend-port>              the backend port [0-65535]
    -e, --enable-floating-ip <enable-floating-ip>  enable floating point ip [true,false]
    -i, --frontend-ip <frontend-ip>                the name of the frontend ip configuration
    -m, --vm-id [vm-id]                            the VM id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>
    -a, --vm-name <vm-name>                        the VM name.
    This virtual machine must exist in the same resource group as the lb.
    Please use vm-id if that is not the case
    -s, --subscription <subscription>              the subscription identifier
<BR>

    network lb inbound-nat-rule list [options] <resource-group> <lb-name>

Tüm gelen nat kuralları yük dengeleyici için listeler.

    azure network lb inbound-nat-rule list -g myresourcegroup -l mylb

    info:    Executing command network lb inbound-nat-rule list
    + Looking up the load balancer "mylb"
    data:    Name          Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes  Backend IP configuration
    data:    ------------  ------------------  --------  -------------  ------------  ------------------  -----------------------  ---
    ---------------------
    data:    myinboundnat  Succeeded           Tcp       81             8080          false               4

    info:    network lb inbound-nat-rule list command OK

Parametre seçenekleri:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb inbound-nat-rule delete [options] <resource-group> <lb-name> <name>

Yük dengeleyicinin belirli bir kaynak grubu için NAT kuralını siler.

    azure network lb inbound-nat-rule delete -g myresourcegroup -l mylb -n myinboundnat

    info:    Executing command network lb inbound-nat-rule delete
    + Looking up the load balancer "mylb"
    Delete inbound NAT rule "myinboundnat?" [y/n] y
    + Updating load balancer "mylb"
    info:    network lb inbound-nat-rule delete command OK

Parametre seçenekleri:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the inbound NAT rule
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier

**Genel IP adreslerini yönetmek için komutlar**

    network public-ip create [options] <resource-group> <name> <location>
Bir genel IP kaynağı oluşturur. Bir genel IP kaynağı oluşturun ve bir etki alanı adına ilişkilendirin.

    azure network public-ip create -g myresourcegroup -n mytestpublicip1 -l eastus -d azureclitest -a "Dynamic"
    info:    Executing command network public-ip create
    + Looking up the public ip "mytestpublicip1"
    + Creating public ip address "mytestpublicip1"
    + Looking up the public ip "mytestpublicip1"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mytestpublicip1
    data:    Name:                 mytestpublicip1
    data:    Type:                 Microsoft.Network/publicIPAddresses
    data:    Location:             eastus
    data:    Provisioning state:   Succeeded
    data:    Allocation method:    Dynamic
    data:    Idle timeout:         4
    data:    Domain name label:    azureclitest
    data:    FQDN:                 azureclitest.eastus.cloudapp.azure.com
    info:    network public-ip create command OK


Parametre seçenekleri:

    -h, --help                                   output usage information
    -v, --verbose                                use verbose output
    --json                                       use json output
    -g, --resource-group <resource-group>        the name of the resource group
    -n, --name <name>                            the name of the public ip
    -l, --location <location>                    the location
    -d, --domain-name-label <domain-name-label>  the domain name label.
    This set DNS to <domain-name-label>.<location>.cloudapp.azure.com
    -a, --allocation-method <allocation-method>  the allocation method [Static][Dynamic]
    -i, --idletimeout <idletimeout>              the idle timeout in minutes
    -f, --reverse-fqdn <reverse-fqdn>            the reverse fqdn
    -t, --tags <tags>                            the list of tags.
    Can be multiple. In the format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    -s, --subscription <subscription>            the subscription identifier
<br>

    network public-ip set [options] <resource-group> <name>
Var olan bir genel IP kaynağı özelliklerini güncelleştirir. Aşağıdaki örnekte genel IP adresi dinamik olan statik olarak değiştirdik.

    azure network public-ip set -g group-1 -n mytestpublicip1 -d azureclitest -a "Static"
    info:    Executing command network public-ip set
    + Looking up the public ip "mytestpublicip1"
    + Updating public ip address "mytestpublicip1"
    + Looking up the public ip "mytestpublicip1"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mytestpublicip1
    data:    Name:                 mytestpublicip1
    data:    Type:                 Microsoft.Network/publicIPAddresses
    data:    Location:             eastus
    data:    Provisioning state:   Succeeded
    data:    Allocation method:    Static
    data:    Idle timeout:         4
    data:    IP Address:           (static IP address)
    data:    Domain name label:    azureclitest
    data:    FQDN:                 azureclitest.eastus.cloudapp.azure.com
    info:    network public-ip set command OK

Parametre seçenekleri:

    -h, --help                                   output usage information
    -v, --verbose                                use verbose output
    --json                                       use json output
    -g, --resource-group <resource-group>        the name of the resource group
    -n, --name <name>                            the name of the public ip
    -d, --domain-name-label [domain-name-label]  the domain name label.
    This set DNS to <domain-name-label>.<location>.cloudapp.azure.com
    -a, --allocation-method <allocation-method>  the allocation method [Static][Dynamic]
    -i, --idletimeout <idletimeout>              the idle timeout in minutes
    -f, --reverse-fqdn [reverse-fqdn]            the reverse fqdn
    -t, --tags <tags>                            the list of tags.
    Can be multiple. In the format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    --no-tags                                    remove all existing tags
    -s, --subscription <subscription>            the subscription identifier

<br>

    network public-ip list [options] <resource-group>
Bir kaynak grubu içindeki tüm genel IP kaynakları listeler.

    azure network public-ip list -g myresourcegroup

    info:    Executing command network public-ip list
    + Getting the public ip addresses
    data:    Name             Location  Allocation  IP Address    Idle timeout  DNS Name
    data:    ---------------  --------  ----------  ------------  ------------  -------------------------------------------
    data:    mypubip5         westus    Dynamic                   4             "domain name".westus.cloudapp.azure.com
    data:    myPublicIP       eastus    Dynamic                   4             "domain name".eastus.cloudapp.azure.com
    data:    mytestpublicip   eastus    Dynamic                   4             "domain name".eastus.cloudapp.azure.com
    data:    mytestpublicip1  eastus   Static (Static IP address) 4             azureclitest.eastus.cloudapp.azure.com

Parametre seçenekleri:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -s, --subscription <subscription>      the subscription identifier

<BR>

    network public-ip show [options] <resource-group> <name>

Bir kaynak grubu içindeki bir genel IP kaynağı için genel IP özellikleri görüntüler.

    azure network public-ip show -g myresourcegroup -n mytestpublicip

    info:    Executing command network public-ip show
    + Looking up the public ip "mytestpublicip1"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mytestpublicip
    data:    Name:                 mytestpublicip
    data:    Type:                 Microsoft.Network/publicIPAddresses
    data:    Location:             eastus
    data:    Provisioning state:   Succeeded
    data:    Allocation method:    Static
    data:    Idle timeout:         4
    data:    IP Address:           (static IP address)
    data:    Domain name label:    azureclitest
    data:    FQDN:                 azureclitest.eastus.cloudapp.azure.com
    info:    network public-ip show command OK

Parametre seçenekleri:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -n, --name <name>                      the name of the public IP
    -s, --subscription <subscription>      the subscription identifier


    network public-ip delete [options] <resource-group> <name>

Genel IP kaynağı siler.

    azure network public-ip delete -g group-1 -n mypublicipname
    info:    Executing command network public-ip delete
    + Looking up the public ip "mypublicipname"
    Delete public ip address "mypublicipname"? [y/n] y
    + Deleting public ip address "mypublicipname"
    info:    network public-ip delete command OK

Parametre seçenekleri:

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -n, --name <name>                      the name of the public IP
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier


**Ağ arabirimleri yönetecek komutlar**

    network nic create [options] <resource-group> <name> <location>
Veya bir sanal makineye ilişkilendirmek için yük Dengeleyiciler kullanılabilir ağ arabirimi (NIC) adlı bir kaynak oluşturur.

    azure network nic create -g myresourcegroup -l eastus -n testnic1 --subnet-name subnet-1 --subnet-vnet-name myvnet

    info:    Executing command network nic create
    + Looking up the network interface "testnic1"
    + Looking up the subnet "subnet-1"
    + Creating network interface "testnic1"
    + Looking up the network interface "testnic1"
    data:    Id:                     /subscriptions/c4a17ddf-aa84-491c-b6f9-b90d882299f7/resourceGroups/group-1/providers/Microsoft.Network/networkInterfaces/testnic1
    data:    Name:                   testnic1
    data:    Type:                   Microsoft.Network/networkInterfaces
    data:    Location:               eastus
    data:    Provisioning state:     Succeeded
    data:    IP configurations:
    data:       Name:                         NIC-config
    data:       Provisioning state:           Succeeded
    data:       Private IP address:           10.0.0.5
    data:       Private IP Allocation Method: Dynamic
    data:       Subnet:                       /subscriptions/c4a17ddf-aa84-491c-b6f9-b90d882299f7/resourceGroups/group-1/providers/Microsoft.Network/virtualNetworks/myVNET/subnets/Subnet-1

Parametre seçenekleri:

    -h, --help                                                       output usage information
    -v, --verbose                                                    use verbose output
    --json                                                           use json output
    -g, --resource-group <resource-group>                            the name of the resource group
    -n, --name <name>                                                the name of the network interface
    -l, --location <location>                                        the location
    -w, --network-security-group-id <network-security-group-id>      the network security group identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/networkSecurityGroups/<nsg-name>
    -o, --network-security-group-name <network-security-group-name>  the network security group name.
    This network security group must exist in the same resource group as the nic.
    Please use network-security-group-id if that is not the case.
    -i, --public-ip-id <public-ip-id>                                the public IP identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/publicIPAddresses/<public-ip-name>
    -p, --public-ip-name <public-ip-name>                            the public IP name.
    This public ip must exist in the same resource group as the nic.
    Please use public-ip-id if that is not the case.
    -a, --private-ip-address <private-ip-address>                    the private IP address
    -u, --subnet-id <subnet-id>                                      the subnet identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<vnet-name>/subnets/<subnet-name>
    --subnet-name <subnet-name>                                  the subnet name
    -m, --subnet-vnet-name <subnet-vnet-name>                        the vnet name under which subnet-name exists
    -t, --tags <tags>                                                the comma separated list of tags.
    Can be multiple. In the format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    -s, --subscription <subscription>                                the subscription identifier
    data:
    info:    network nic create command OK

<BR>

    network nic set [options] <resource-group> <name>

    network nic list [options] <resource-group>
    network nic show [options] <resource-group> <name>
    network nic delete [options] <resource-group> <name>

**Ağ güvenlik grupları yönetmek için komutlar**

    network nsg create [options] <resource-group> <name> <location>
    network nsg set [options] <resource-group> <name>
    network nsg list [options] <resource-group>
    network nsg show [options] <resource-group> <name>
    network nsg delete [options] <resource-group> <name>

**Komutları yönetmek için ağ güvenlik grubu kuralları**

    network nsg rule create [options] <resource-group> <nsg-name> <name>
    network nsg rule set [options] <resource-group> <nsg-name> <name>
    network nsg rule list [options] <resource-group> <nsg-name>
    network nsg rule show [options] <resource-group> <nsg-name> <name>
    network nsg rule delete [options] <resource-group> <nsg-name> <name>

**Traffic manager profili yönetecek komutlar**

    network traffic-manager profile create [options] <resource-group> <name>
    network traffic-manager profile set [options] <resource-group> <name>
    network traffic-manager profile list [options] <resource-group>
    network traffic-manager profile show [options] <resource-group> <name>
    network traffic-manager profile delete [options] <resource-group> <name>
    network traffic-manager profile is-dns-available [options] <resource-group> <relative-dns-name>

**Traffic manager uç noktası yönetecek komutlar**

    network traffic-manager profile endpoint create [options] <resource-group> <profile-name> <name> <endpoint-location>
    network traffic-manager profile endpoint set [options] <resource-group> <profile-name> <name>
    network traffic-manager profile endpoint delete [options] <resource-group> <profile-name> <name>

**Sanal yönetecek komutlar ağ ağ geçitleri**

    network gateway list [options] <resource-group>

## <a name="azure-provider-commands-to-manage-resource-provider-registrations"></a>Azure sağlayıcısı: Kaynak Sağlayıcısı kaydı yönetecek komutlar
**Resource Manager'da şu anda kayıtlı sağlayıcı listesi**

    provider list [options]

**İstenen sağlayıcı ad alanı ayrıntılarını göster**

    provider show [options] <namespace>

**Sağlayıcı aboneliği şununla Kaydet:**

    provider register [options] <namespace>

**Sağlayıcı ile abonelik kaydını sil**

    provider unregister [options] <namespace>

## <a name="azure-resource-commands-to-manage-your-resources"></a>Azure kaynak: Kaynaklarınızı yönetmek için komutlar
**Bir kaynak grubunda bir kaynak oluşturur**

    resource create [options] <resource-group> <name> <resource-type> <location> <api-version>

**Şablonları veya parametreleri olmadan bir kaynak grubundaki kaynak güncelleştirir**

    resource set [options] <resource-group> <name> <resource-type> <properties> <api-version>

**Kaynakları listeler**

    resource list [options] [resource-group]

**Bir kaynak grubuna veya aboneliğe içinde bir kaynak alır**

    resource show [options] <resource-group> <name> <resource-type> <api-version>

**Bir kaynak grubundaki kaynak siler**

    resource delete [options] <resource-group> <name> <resource-type> <api-version>

## <a name="azure-role-commands-to-manage-your-azure-roles"></a>Azure rol: Azure, rolleri yönetmek için komutlar
**Tüm kullanılabilir rol tanımlarını Al**

    role list [options]

**Kullanılabilir rol tanımı Al**

    role show [options] [name]

**Rol atamalarınızı yönetecek komutlar**

    role assignment create [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]
    role assignment list [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]
    role assignment delete [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]

## <a name="azure-storage-commands-to-manage-your-storage-objects"></a>Azure Depolama: Depolama nesnelerinizi yönetecek komutlar
**Depolama hesaplarınızı yönetme komutları**

    storage account list [options]
    storage account show [options] <name>
    storage account create [options] <name>
    storage account set [options] <name>
    storage account delete [options] <name>

**Depolama hesabı anahtarlarınızı yönetmek için kullanılan komutlar**

    storage account keys list [options] <name>
    storage account keys renew [options] <name>

**Depolama bağlantı dizenizi göstermek için komutları**

    storage account connectionstring show [options] <name>

**Depolama kapsayıcıları yönetmek için komutlar**

    storage container list [options] [prefix]
    storage container show [options] [container]
    storage container create [options] [container]
    storage container delete [options] [container]
    storage container set [options] [container]

**Depolama kapsayıcınızda imzalarını paylaşılan yönetmek için komutlara erişin**

    storage container sas create [options] [container] [permissions] [expiry]

**Erişim ilkeleri, depolama kapsayıcınızda saklı yönetecek komutlar**

    storage container policy create [options] [container] [name]
    storage container policy show [options] [container] [name]
    storage container policy list [options] [container]
    storage container policy set [options] [container] [name]
    storage container policy delete [options] [container] [name]

**Depolama BLOB'ları yönetmek için komutlar**

    storage blob list [options] [container] [prefix]
    storage blob show [options] [container] [blob]
    storage blob delete [options] [container] [blob]
    storage blob upload [options] [file] [container] [blob]
    storage blob download [options] [container] [blob] [destination]

**Kopyalama işlemlerini blobunuza yönetecek komutlar**

    storage blob copy start [options] [sourceUri] [destContainer]
    storage blob copy show [options] [container] [blob]
    storage blob copy stop [options] [container] [blob] [copyid]

**Paylaşılan yönetecek komutlar imzası Storage blobunuza erişim**

    storage blob sas create [options] [container] [blob] [permissions] [expiry]

**Depolama dosya paylaşımlarınızı yönetmek için kullanılan komutlar**

    storage share create [options] [share]
    storage share show [options] [share]
    storage share delete [options] [share]
    storage share list [options] [prefix]

**Depolama dosyalarınızı yönetmek için komutlar**

    storage file list [options] [share] [path]
    storage file delete [options] [share] [path]
    storage file upload [options] [source] [share] [path]
    storage file download [options] [share] [path] [destination]

**Depolama dosya dizininizi yönetmek için komutlar**

    storage directory create [options] [share] [path]
    storage directory delete [options] [share] [path]

**Depolama kuyrukları yönetmek için komutlar**

    storage queue create [options] [queue]
    storage queue list [options] [prefix]
    storage queue show [options] [queue]
    storage queue delete [options] [queue]

**Paylaşılan yönetecek komutlar, depolama kuyruğu imzalarını erişim**

    storage queue sas create [options] [queue] [permissions] [expiry]

**Erişim ilkeleri depolama kuyruğunuzun saklı yönetecek komutlar**

    storage queue policy create [options] [queue] [name]
    storage queue policy show [options] [queue] [name]
    storage queue policy list [options] [queue]
    storage queue policy set [options] [queue] [name]
    storage queue policy delete [options] [queue] [name]

**Depolama günlüğe kaydetme özelliklerini yönetmek için komutlar**

    storage logging show [options]
    storage logging set [options]

**Depolama ölçümleri özelliklerinizi yönetecek komutlar**

    storage metrics show [options]
    storage metrics set [options]

**Depolama tabloları yönetecek komutlar**

    storage table create [options] [table]
    storage table list [options] [prefix]
    storage table show [options] [table]
    storage table delete [options] [table]

**Depolama tablonuzun imzaları paylaşılan yönetmek için komutlara erişin**

    storage table sas create [options] [table] [permissions] [expiry]

**Erişim ilkeleri depolama tablonun saklı yönetecek komutlar**

    storage table policy create [options] [table] [name]
    storage table policy show [options] [table] [name]
    storage table policy list [options] [table]
    storage table policy set [options] [table] [name]
    storage table policy delete [options] [table] [name]

## <a name="azure-tag-commands-to-manage-your-resource-manager-tag"></a>Azure etiketi: Resource manager etiketinizi yönetecek komutlar
**Etiket Ekle**

    tag create [options] <name> <value>

**Tüm bir etiketi veya bir etiket değeri Kaldır**

    tag delete [options] <name> <value>

**Etiket bilgileri listeler**

    tag list [options]

**Bir etiketi Al**

    tag show [options] [name]

## <a name="azure-vm-commands-to-manage-your-azure-virtual-machines"></a>azure vm: Azure sanal makinelerinizi yönetmek için komutlar
**VM oluşturma**

    vm create [options] <resource-group> <name> <location> <os-type>

**Varsayılan kaynaklar ile VM oluşturma**

    vm quick-create [options] <resource-group> <name> <location> <os-type> <image-urn> <admin-username> <admin-password

> [!TIP]
> CLI Sürüm 0.10 ile başlayarak, "UbuntuLTS" veya "Win2012R2Datacenter" gibi kısa bir diğer ad olarak sağlayabilirsiniz `image-urn` bazı popüler Market görüntüleri için. Çalıştırma `azure help vm quick-create` seçenekleri. Ayrıca, 0.10 sürümünden itibaren `azure vm quick-create` seçili bölgesinde kullanılabilir durumdaysa, varsayılan olarak premium depolama kullanır.
> 
> 

**Bir hesap içindeki sanal makinelerin listesi**

    vm list [options]

**Bir kaynak grubu içindeki bir sanal makine Al**

    vm show [options] <resource-group> <name>

**Bir kaynak grubu içindeki bir sanal makineyi Sil**

    vm delete [options] <resource-group> <name>

**Bir kaynak grubu içindeki bir sanal makine kapatma**

    vm stop [options] <resource-group> <name>

**Bir kaynak grubu içindeki bir sanal makineyi yeniden başlatın**

    vm restart [options] <resource-group> <name>

**Bir kaynak grubu içindeki bir sanal makineyi Başlat**

    vm start [options] <resource-group> <name>

**Bir kaynak grubu ve işlem kaynaklarını serbest bırakır içinde bir sanal makine kapatma**

    vm deallocate [options] <resource-group> <name>

**Kullanılabilir sanal makine boyutlarını Listele**

    vm sizes [options]

**VM işletim sistemi görüntüsü veya VM görüntüsü yakalama**

    vm capture [options] <resource-group> <name> <vhd-name-prefix>

**Sanal makine durumunu Genelleştirmiş olarak ayarlayın.**

    vm generalize [options] <resource-group> <name>

**Sanal makinenin örnek görünümünü Al**

    vm get-instance-view [options] <resource-group> <name>

**Bir sanal makinede Uzak Masaüstü erişimi veya SSH ayarlarını sıfırlayın ve yönetici veya sudo yetkilisi sahip olan hesap için parolayı sıfırlamak için etkinleştirme**

    vm reset-access [options] <resource-group> <name>

**VM'yi yeni veriler ile güncelleştirin.**

    vm set [options] <resource-group> <name>

**Sanal makine veri disklerinizi yönetme komutları**

    vm disk attach-new [options] <resource-group> <vm-name> <size-in-gb> [vhd-name]
    vm disk detach [options] <resource-group> <vm-name> <lun>
    vm disk attach [options] <resource-group> <vm-name> [vhd-url]

**VM kaynak uzantıları yönetmek için komutlar**

    vm extension set [options] <resource-group> <vm-name> <name> <publisher-name> <version>
    vm extension get [options] <resource-group> <vm-name>

**Docker sanal makinenizi yönetecek komutlar**

    vm docker create [options] <resource-group> <name> <location> <os-type>

**VM görüntülerini yönetmek için komutlar**

    vm image list-publishers [options] <location>
    vm image list-offers [options] <location> <publisher>
    vm image list-skus [options] <location> <publisher> <offer>
    vm image list [options] <location> <publisher> [offer] [sku]
