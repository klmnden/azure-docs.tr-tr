---
title: "Azure CLI 1.0 kullanarak bir Linux VM üzerinde MongoDB yükleme | Microsoft Docs"
description: "Yükleme ve Resource Manager dağıtım modelini kullanarak azure'da bir Linux sanal makinede MongoDB yapılandırma öğrenin."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 3f55b546-86df-4442-9ef4-8a25fae7b96e
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: c97ade0a3d95824f723aad55776de861fe49441f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-install-and-configure-mongodb-on-a-linux-vm-using-the-azure-cli-10"></a>Yükleme ve Azure CLI 1.0 kullanarak bir Linux VM üzerinde MongoDB yapılandırın
[MongoDB](http://www.mongodb.org) bir popüler açık kaynak, yüksek performanslı NoSQL veritabanıdır. Bu makalede Resource Manager dağıtım modelini kullanarak yükleyin ve azure'da bir Linux VM üzerinde MongoDB yapılandırma gösterilmektedir. Örnekleri gösterilir, ayrıntı nasıl için:

* [El ile yükleyin ve temel bir MongoDB örneği yapılandırın](#manually-install-and-configure-mongodb-on-a-vm)
* [Resource Manager şablonu kullanarak temel MongoDB örneği oluşturma](#create-basic-mongodb-instance-on-centos-using-a-template)
* [Resource Manager şablonu kullanarak çoğaltma ile parçalı küme ayarlar karmaşık bir MongoDB oluşturma](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- Azure CLI 1.0: Klasik ve kaynak yönetimi dağıtım modellerine yönelik CLI’mız (bu makale)
- [Azure CLI 2.0](create-cli-complete-nodejs.md): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>El ile yükleyin ve bir VM üzerinde MongoDB yapılandırın
MongoDB [yükleme yönergelerinizi](https://docs.mongodb.com/manual/administration/install-on-linux/) Red Hat gibi Linux distro'lar için / CentOS, SUSE, Ubuntu ve Debian. Aşağıdaki örnekte bir *CentOS* depolandığı bir SSH anahtarı kullanarak VM *~/.ssh/id_rsa.pub*. Depolama hesabı adı, DNS adı ve yönetici kimlik bilgileri için istemleri yanıtlayın:

```azurecli
azure vm quick-create \
    --image-urn CentOS \
    --ssh-publickey-file ~/.ssh/id_rsa.pub 
```

Önceki VM Oluşturma adımı sonunda görüntülenen ortak IP adresi kullanarak VM oturum açın:

```bash
ssh azureuser@40.78.23.145
```

MongoDB için yükleme kaynakları eklemek için oluşturma bir **yum** şekilde depo dosyası:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.4.repo
```

MongoDB depodaki dosyayı düzenlemek için açın. Aşağıdaki satırları ekleyin:

```sh
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

MongoDB kullanarak yükleyin **yum** gibi:

```bash
sudo yum install -y mongodb-org
```

Varsayılan olarak, MongoDB erişmesini engeller CentOS görüntüleri SELinux uygulanır. İlke Yönetimi Araçları'nı yükleyin ve SELinux MongoDB, varsayılan TCP bağlantı noktası 27017 gibi çalışmasına izin verecek şekilde yapılandırın. 

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
Github'dan aşağıdaki Azure Hızlı Başlangıç şablonu kullanarak tek bir CentOS VM üzerinde temel bir MongoDB örneği oluşturabilirsiniz. Bu şablon eklemek için Linux özel betik uzantısı kullanan bir `yum` MongoDB yükleyin ve yeni oluşturulan CentOS VM deposuna.

* [CentOS temel MongoDB örneğinde](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Aşağıdaki örnek, bir kaynak grubu adıyla oluşturur `myResourceGroup` içinde `eastus` bölge. Aşağıdaki gibi kendi değerlerinizi girin:

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [!NOTE]
> Azure CLI dağıtım oluşturma birkaç saniye içinde bir istem verir, ancak yükleme ve yapılandırma tamamlanması birkaç dakika sürer. İle dağıtım durumunu denetleme `azure group deployment show myResourceGroup`, buna göre kaynak grubunuzun adını girmek. Bekle **ProvisioningState** gösterir *başarılı* VM SSH denemeden önce.

Dağıtım SSH VM'ye tamamlandıktan sonra. VM Olanağını kullanarak IP adresi elde `azure vm show` komutunu aşağıdaki örnekteki gibi:

```azurecli
azure vm show --resource-group myResourceGroup --name myLinuxVM
```

Çıktı sonuna yakın genel IP adresi görüntülenir. SSH, VM, VM IP adresine sahip:

```bash
ssh azureuser@138.91.149.74
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

* [MongoDB parçalama CentOS kümede](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) -https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [!WARNING]
> Bu karmaşık MongoDB parçalı Küme dağıtımı, 20'den fazla çekirdek, genellikle varsayılan çekirdek sayısı her bölge için bir abonelik olduğu gerektirir. Çekirdek sayısı artırmak için bir Azure destek isteği açın.

Aşağıdaki örnek, bir kaynak grubu adıyla oluşturur *myResourceGroup* içinde *eastus* bölge. Aşağıdaki gibi kendi değerlerinizi girin:

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [!NOTE]
> Azure CLI dağıtım oluşturma birkaç saniye içinde bir istem verir, ancak yüklemeyi ve yapılandırmayı tamamlamak için bir saatten fazla sürebilir. İle dağıtım durumunu denetleme `azure group deployment show myResourceGroup`, kaynak grubunuzun adını buna uygun olarak ayarlama. Bekle **ProvisioningState** gösterir *başarılı* Vm'lere bağlanmadan önce.


## <a name="next-steps"></a>Sonraki adımlar
Bu örneklerde, MongoDB yerel olarak sanal makineden bağlanın. Başka bir VM veya ağdan MongoDB örneğine bağlanmak isterseniz, uygun olun [ağ güvenlik grubu kuralları oluşturulur](nsg-quickstart.md).

Şablonları kullanarak oluşturma hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md).

Azure Resource Manager şablonları özel betik uzantısının indirip Vm'leriniz komut dosyaları çalıştırmak için kullanın. Daha fazla bilgi için bkz: [Azure özel betik uzantısı ile Linux sanal makineleri kullanarak](extensions-customscript.md).

