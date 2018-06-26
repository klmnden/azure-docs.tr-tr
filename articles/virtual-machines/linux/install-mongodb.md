---
title: Bir Linux VM Azure CLI ile MongoDB yükleyin. | Microsoft Docs
description: MongoDB bir Linux sanal makine iusing Azure CLI 2.0 yükleyip öğrenin
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: 3f55b546-86df-4442-9ef4-8a25fae7b96e
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/15/2017
ms.author: iainfou
ms.openlocfilehash: a47c0e2f655f51444dc586f696c26caa63ab6cac
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36937591"
---
# <a name="how-to-install-and-configure-mongodb-on-a-linux-vm"></a>Yükleme ve bir Linux VM MongoDB yapılandırın
[MongoDB](http://www.mongodb.org) bir popüler açık kaynak, yüksek performanslı NoSQL veritabanıdır. Bu makalede yükleme ve Azure CLI 2.0 ile bir Linux VM üzerinde MongoDB yapılandırma gösterilmektedir. Örnekleri gösterilir, ayrıntı nasıl için:

* [El ile yükleyin ve temel bir MongoDB örneği yapılandırın](#manually-install-and-configure-mongodb-on-a-vm)
* [Resource Manager şablonu kullanarak temel MongoDB örneği oluşturma](#create-basic-mongodb-instance-on-centos-using-a-template)
* [Resource Manager şablonu kullanarak çoğaltma ile parçalı küme ayarlar karmaşık bir MongoDB oluşturma](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>El ile yükleyin ve bir VM üzerinde MongoDB yapılandırın
MongoDB [yükleme yönergelerinizi](https://docs.mongodb.com/manual/administration/install-on-linux/) Red Hat gibi Linux distro'lar için / CentOS, SUSE, Ubuntu ve Debian. Aşağıdaki örnekte bir *CentOS* VM. Bu ortamı oluşturmak için en son gereksinim [Azure CLI 2.0](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az oturum açma](/cli/azure/reference-index#az_login).

[az group create](/cli/azure/group#az_group_create) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

[az vm create](/cli/azure/vm#az_vm_create) ile bir VM oluşturun. Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVM* adlı bir kullanıcı ile *azureuser* SSH ortak anahtar kimlik doğrulaması kullanma

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Kendi kullanıcı adı kullanarak VM SSH ve `publicIpAddress` önceki adımdan çıktıda listelenen:

```bash
ssh azureuser@<publicIpAddress>
```

MongoDB için yükleme kaynakları eklemek için oluşturma bir **yum** şekilde depo dosyası:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.6.repo
```

Düzenlemek için MongoDB depodaki dosyasını gibi açın ile `vi` veya `nano`. Aşağıdaki satırları ekleyin:

```sh
[mongodb-org-3.6]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.6/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.6.asc
```

MongoDB kullanarak yükleyin **yum** gibi:

```bash
sudo yum install -y mongodb-org
```

Varsayılan olarak, MongoDB erişmesini engeller CentOS görüntüleri SELinux uygulanır. İlke Yönetimi Araçları'nı yükleyin ve SELinux MongoDB, varsayılan TCP bağlantı noktası 27017 gibi çalışmasına izin verecek şekilde yapılandırın:

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

MongoDB hizmetini şu şekilde başlatın:

```bash
sudo service mongod start
```

MongoDB yükleme yerel kullanarak bağlanarak doğrulayın `mongo` istemci:

```bash
mongo
```

Şimdi MongoDB örneği, bazı veriler ekleme ve ardından arama test edin:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

İsterseniz, otomatik olarak bir sistem yeniden başlatma sırasında başlatmak için MongoDB yapılandırın:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Bir şablon kullanarak CentOS üzerinde temel MongoDB örneği oluşturma
Github'dan aşağıdaki Azure Hızlı Başlangıç şablonu kullanarak tek bir CentOS VM üzerinde temel bir MongoDB örneği oluşturabilirsiniz. Bu şablon eklemek için Linux özel betik uzantısı kullanan bir **yum** MongoDB yükleyin ve yeni oluşturulan CentOS VM deposuna.

* [CentOS temel MongoDB örneğinde](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Bu ortamı oluşturmak için en son gereksinim [Azure CLI 2.0](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az oturum açma](/cli/azure/reference-index#az_login). Öncelikle [az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

Ardından, MongoDB şablonla dağıtmak [az grup dağıtımı oluşturmak](/cli/azure/group/deployment#az_group_deployment_create). İstendiğinde, kendi benzersiz değerleri girin *newStorageAccountName*, *dnsNameForPublicIP*ve yönetici kullanıcı adı ve parola:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

VM Genel DNS adresi kullanarak VM oturum açın. Genel DNS adresi ile görüntüleyebilirsiniz [az vm Göster](/cli/azure/vm#az_vm_show):

```azurecli
az vm show -g myResourceGroup -n myLinuxVM -d --query [fqdns] -o tsv
```

SSH kullanıcı adı ve Genel DNS adresi kullanarak, VM için:

```bash
ssh azureuser@mypublicdns.eastus.cloudapp.azure.com
```

MongoDB yükleme yerel kullanarak bağlanarak doğrulayın `mongo` şekilde istemci:

```bash
mongo
```

Şimdi örnek bazı veri ekleme ve aşağıdaki gibi arama test edin:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Bir şablon kullanarak CentOS üzerinde karmaşık MongoDB parçalı küme oluşturma
Github'dan aşağıdaki Azure Hızlı Başlangıç şablonu kullanarak karmaşık MongoDB parçalı kümesi oluşturabilirsiniz. Bu şablon izleyen [MongoDB parçalı küme en iyi yöntemler](https://docs.mongodb.com/manual/core/sharded-cluster-components/) artıklık ve yüksek kullanılabilirlik sağlamak için. Şablon iki parça, her çoğaltma kümesinde üç düğümü oluşturur. Bir yapılandırma sunucusu çoğaltma ile üç düğüm kümesi de oluşturulur, iki **mongos** parça uygulamalardan tutarlılık sağlamak için yönlendirici sunucuları.

* [MongoDB parçalama CentOS kümede](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) - https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [!WARNING]
> Bu karmaşık MongoDB parçalı Küme dağıtımı, 20'den fazla çekirdek, genellikle varsayılan çekirdek sayısı her bölge için bir abonelik olduğu gerektirir. Çekirdek sayısı artırmak için bir Azure destek isteği açın.

Bu ortamı oluşturmak için en son gereksinim [Azure CLI 2.0](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az oturum açma](/cli/azure/reference-index#az_login). Öncelikle [az group create](/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli
az group create --name myResourceGroup --location eastus
```

Ardından, MongoDB şablonla dağıtmak [az grup dağıtımı oluşturmak](/cli/azure/group/deployment#az_group_deployment_create). Kendi kaynak tanımlamak adları ve gibi gerektiğinde boyutları *mongoAdminUsername*, *sizeOfDataDiskInGB*, ve *configNodeVmSize*:

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

Bu dağıtım, tüm VM örnekleri yapılandırmak ve dağıtmak için bir saatten fazla sürebilir. `--no-wait` Bayrağı şablon dağıtımı Azure platformu tarafından kabul edildikten sonra denetim komut istemini döndürmek için yukarıdaki komut sonunda kullanılır. Dağıtım durumu ile sonra görüntüleyebileceğiniz [az grubu dağıtım Göster](/cli/azure/group/deployment#az_group_deployment_show). Aşağıdaki örnek için durum görünümleri *myMongoDBCluster* dağıtımında *myResourceGroup* kaynak grubu:

```azurecli
az group deployment show \
    --resource-group myResourceGroup \
    --name myMongoDBCluster \
    --query [properties.provisioningState] \
    --output tsv
```

## <a name="next-steps"></a>Sonraki adımlar
Bu örneklerde, MongoDB yerel olarak sanal makineden bağlanın. Başka bir VM veya ağdan MongoDB örneğine bağlanmak isterseniz, uygun olun [ağ güvenlik grubu kuralları oluşturulur](nsg-quickstart.md).

Bu örnekler çekirdek MongoDB ortamı geliştirme amacıyla dağıtın. Gerekli güvenlik yapılandırma seçenekleri, ortamınız için geçerlidir. Daha fazla bilgi için bkz: [MongoDB güvenlik belgeleri](https://docs.mongodb.com/manual/security/).

Şablonları kullanarak oluşturma hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md).

Azure Resource Manager şablonları özel betik uzantısının indirip Vm'leriniz komut dosyaları çalıştırmak için kullanın. Daha fazla bilgi için bkz: [Azure özel betik uzantısı ile Linux sanal makineleri kullanarak](extensions-customscript.md).

