---
title: Azure CLI ile bir Linux sanal makinesi üzerinde MongoDB yükleme | Microsoft Docs
description: Yükleme ve MongoDB bir Linux sanal makine iusing Azure CLI'yı yapılandırma hakkında bilgi edinin
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
ms.assetid: 3f55b546-86df-4442-9ef4-8a25fae7b96e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/15/2017
ms.author: cynthn
ms.openlocfilehash: 5fadf23cc1fc2e1a6092c48033580d398fc689a0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60542808"
---
# <a name="how-to-install-and-configure-mongodb-on-a-linux-vm"></a>Yükleme ve Linux sanal makinesine MongoDB yapılandırın

[MongoDB](https://www.mongodb.org) popüler açık kaynaklı, yüksek performanslı NoSQL veritabanıdır. Bu makalede, yükleme ve Azure CLI ile bir Linux sanal makinesi üzerinde MongoDB yapılandırma işlemini göstermektedir. Örnekleri gösterilir, ayrıntı nasıl için:

* [El ile yükleyin ve temel bir MongoDB örneğine yapılandırın](#manually-install-and-configure-mongodb-on-a-vm)
* [Resource Manager şablonu kullanarak temel bir MongoDB örneğine oluşturma](#create-basic-mongodb-instance-on-centos-using-a-template)
* [Resource Manager şablonu kullanarak çoğaltma ile parçalı kümesi ayarlar karmaşık bir MongoDB oluşturma](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>El ile yükleyin ve MongoDB üzerinde bir VM yapılandırma
MongoDB [yükleme yönergelerinizi](https://docs.mongodb.com/manual/administration/install-on-linux/) Red Hat gibi Linux dağıtım paketlerini için / CentOS, SUSE, Ubuntu ve Debian. Aşağıdaki örnek, oluşturur bir *CentOS* VM. Bu ortamı oluşturmak için en son gerekir [Azure CLI](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az login](/cli/azure/reference-index).

[az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

[az vm create](/cli/azure/vm) ile bir VM oluşturun. Aşağıdaki örnekte adlı bir VM oluşturur *myVM* adlı bir kullanıcı ile *azureuser* SSH ortak anahtarı kimlik doğrulaması kullanma

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Kendi kullanıcı adınızı kullanarak VM'ye SSH ve `publicIpAddress` önceki adımdan çıktısında listelenir:

```bash
ssh azureuser@<publicIpAddress>
```

MongoDB için yükleme kaynakları eklemek için oluşturun bir **yum** depo dosyası aşağıdaki gibi:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.6.repo
```

Düzenleme için MongoDB depo dosyası gibi açın ile `vi` veya `nano`. Aşağıdaki satırları ekleyin:

```sh
[mongodb-org-3.6]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.6/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.6.asc
```

MongoDB kullanarak yükleme **yum** gibi:

```bash
sudo yum install -y mongodb-org
```

Varsayılan olarak, MongoDB erişmesini engelleyen CentOS görüntülerindeki SELinux zorlanır. İlke Yönetimi Araçları'nı yükleyin ve SELinux MongoDB, varsayılan TCP bağlantı noktası 27017 gibi çalışmasına izin verecek şekilde yapılandırın:

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

MongoDB hizmeti şu şekilde başlatın:

```bash
sudo service mongod start
```

MongoDB yükleme yerel ile bağlanarak doğrulayın `mongo` istemci:

```bash
mongo
```

Artık bir MongoDB örneğine veri eklemek ve ardından arama test edin:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

İsterseniz, MongoDB, sistemin yeniden başlatılması sırasında otomatik olarak başlayacak şekilde yapılandırın:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Şablon kullanarak CentOS üzerinde temel MongoDB örneği oluşturma
Temel bir MongoDB örneğine github'dan aşağıdaki Azure Hızlı Başlangıç şablonu kullanarak tek bir CentOS VM'de oluşturabilirsiniz. Bu şablon eklemek için Linux için özel betik uzantısı kullanan bir **yum** depoya yeni oluşturulan CentOS VM ve MongoDB yükleyin.

* [CentOS temel MongoDB örneğinde](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Bu ortamı oluşturmak için en son gerekir [Azure CLI](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az login](/cli/azure/reference-index). Öncelikle [az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

Ardından, MongoDB şablon ile dağıtım [az grubu dağıtım oluşturma](/cli/azure/group/deployment). İstendiğinde, kendi benzersiz değerleri girin *newStorageAccountName*, *dnsNameForPublicIP*ve yönetici kullanıcı adı ve parola:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

Sanal makinenizin Genel DNS adres kullanarak sanal Makineye oturum açın. Genel DNS adresiyle görüntüleyebileceğiniz [az vm show](/cli/azure/vm):

```azurecli
az vm show -g myResourceGroup -n myLinuxVM -d --query [fqdns] -o tsv
```

Kullanıcı adınızı ve Genel DNS adresi kullanarak sanal makinenize yönelik SSH:

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

MongoDB yükleme yerel ile bağlanarak doğrulayın `mongo` aşağıdaki gibi istemci:

```bash
mongo
```

Şimdi örnek, bazı veri eklemeye ve arama gibi test edin:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Şablon kullanarak CentOS üzerinde karmaşık MongoDB parçalı küme oluşturma
Github'dan aşağıdaki Azure Hızlı Başlangıç şablonu kullanarak karmaşık bir MongoDB parçalı kümesi oluşturabilirsiniz. Bu şablon aşağıdaki [MongoDB parçalı küme en iyi](https://docs.mongodb.com/manual/core/sharded-cluster-components/) yedeklilik ve yüksek kullanılabilirlik sağlamak için. Şablon, her çoğaltma kümedeki üç düğüm ile iki parça oluşturur. Üç düğüm ile ayarlanmış bir yapılandırma sunucusu çoğaltma ayrıca oluşturulur, iki **mongos** parçalar arasında uygulamalardan tutarlılık sağlamak için yönlendirici sunucuları.

* [MongoDB parçalama kümede CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [!WARNING]
> Bu karmaşık MongoDB parçalara ayrılmış Küme dağıtımı, genellikle bir abonelik için bölge başına varsayılan çekirdek sayısı olan 20'den fazla çekirdek gerektirir. Çekirdek sayınız artırmak için Azure destek isteği açın.

Bu ortamı oluşturmak için en son gerekir [Azure CLI](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az login](/cli/azure/reference-index). Öncelikle [az group create](/cli/azure/group) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

Ardından, MongoDB şablon ile dağıtım [az grubu dağıtım oluşturma](/cli/azure/group/deployment). Kendi kaynak tanımlamak adları ve gibi gerektiğinde boyutları *mongoAdminUsername*, *sizeOfDataDiskInGB*, ve *configNodeVmSize*:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "mongoAdminUsername": {"value": "mongoadmin"},
    "mongoAdminPassword": {"value": "P@ssw0rd!"},
    "dnsNamePrefix": {"value": "mypublicdns"},
    "environment": {"value": "AzureCloud"},
    "numDataDisks": {"value": "4"},
    "sizeOfDataDiskInGB": {"value": 20},
    "centOsVersion": {"value": "7.0"},
    "routerNodeVmSize": {"value": "Standard_DS3_v2"},
    "configNodeVmSize": {"value": "Standard_DS3_v2"},
    "replicaNodeVmSize": {"value": "Standard_DS3_v2"},
    "zabbixServerIPAddress": {"value": "Null"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json \
  --name myMongoDBCluster \
  --no-wait
```

Bu dağıtım, tüm VM örnekleri yapılandırmak ve dağıtmak için bir saatten fazla sürebilir. `--no-wait` Denetimi, şablon dağıtımı Azure platformu tarafından kabul edildikten sonra komut istemine geri dönmek için önceki komutta sonunda bayrağı kullanılır. Dağıtım durumunu daha sonra görüntüleyebileceğiniz [az grubu dağıtım show](/cli/azure/group/deployment). Aşağıdaki örnek için durum görünümleri *myMongoDBCluster* dağıtımda *myResourceGroup* kaynak grubu:

```azurecli
az group deployment show \
    --resource-group myResourceGroup \
    --name myMongoDBCluster \
    --query [properties.provisioningState] \
    --output tsv
```

## <a name="next-steps"></a>Sonraki adımlar
Bu örneklerde, MongoDB örneği yerel olarak VM'den bağlanabilirsiniz. Başka bir VM veya ağ MongoDB örneğine bağlanmak istiyorsanız, uygun olmak [ağ güvenlik grubu kurallarını oluşturulur](nsg-quickstart.md).

Bu örnekler, çekirdek MongoDB ortamı geliştirme amacıyla dağıtın. Gerekli güvenlik yapılandırma seçenekleri, ortamınız için geçerlidir. Daha fazla bilgi için [MongoDB güvenlik docs](https://docs.mongodb.com/manual/security/).

Şablonları kullanarak oluşturma hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md).

Azure Resource Manager şablonları indirip Vm'lerinizde betiklerini yürütmek için özel betik uzantısı kullanın. Daha fazla bilgi için [Azure özel betik uzantısı ile Linux sanal makineleri kullanarak](extensions-customscript.md).

