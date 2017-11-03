---
title: "Bir Linux VM özelleştirmek için bulut init kullanın | Microsoft Docs"
description: "Azure CLI 2.0 ile oluşturma sırasında bir Linux VM özelleştirmek için bulut init kullanma"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 10/03/2017
ms.author: iainfou
ms.openlocfilehash: 5559f258f5c29b07edb5e61be4755d67173019e0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-cloud-init-to-customize-a-linux-vm-in-azure"></a>Bulut init azure'da bir Linux VM özelleştirmek için kullanın
Bu makalede nasıl kullanılacağı gösterilmektedir [bulut init](https://cloudinit.readthedocs.io) ana bilgisayar adı ayarlamak için güncelleştirme paketleri ve azure'da bir sanal makinede (VM) kullanıcı hesaplarını yönetin. Bir VM ile Azure CLI 2.0 oluşturduğunuzda, bu bulut başlatma komut dosyaları önyükleme çalıştırın. Uygulamaları yükleme, yapılandırma dosyaları yazma ve anahtar kasası anahtarları ekleme hakkında daha ayrıntılı bir bakış için bkz: [Bu öğretici](tutorial-automate-vm-deployment.md). Bu adımları [Azure CLI 1.0](using-cloud-init-nodejs.md) ile de gerçekleştirebilirsiniz.


## <a name="cloud-init-overview"></a>Bulut init genel bakış
[Bulut init](https://cloudinit.readthedocs.io) ilk kez önyükleme gibi bir Linux VM özelleştirmek için yaygın olarak kullanılan bir yaklaşımdır. Bulut init paketleri yüklemek ve dosyaları yazma veya kullanıcılar ve güvenlik yapılandırmak için kullanabilirsiniz. Bulut init ilk önyükleme işlemi sırasında çalışırken, ek adımlar veya yapılandırmanızı uygulamak için gerekli aracıların yok.

Bulut init dağıtımları üzerinde de çalışır. Örneğin, kullanmadığınız **get apt yükleme** veya **yum yükleme** bir paketi yüklemek için. Bunun yerine, yüklemek için paketlerin listesini tanımlayabilirsiniz. Bulut init otomatik olarak seçtiğiniz distro için yerel paket Yönetim Aracı'nı kullanır.

Bulut dahil ve Azure'a sağladıkları görüntülerinde çalışma başlatma almak için ortaklarımızın ile çalışıyoruz. Aşağıdaki tabloda Azure platform görüntüleri geçerli bulut init kullanılabilirliğine özetlenmektedir:

| Diğer ad | Yayımcı | Sunduğu | SKU | Sürüm |
|:--- |:--- |:--- |:--- |:--- |:--- |
| UbuntuLTS |Canonical |UbuntuServer |16.04 LTS |en son |
| UbuntuLTS |Canonical |UbuntuServer |14.04.5-LTS |en son |
| CoreOS |CoreOS |CoreOS |Dengeli |en son |


## <a name="set-the-hostname-with-cloud-init"></a>Bulut init ile ana bilgisayar adı ayarlama
Bulut init dosyaları dilini [YAML](http://www.yaml.org). Azure ile VM oluşturduğunuzda, bir bulut başlatma komut dosyasını çalıştırmak için [az vm oluşturma](/cli/azure/vm#create), bulut init dosyasıyla belirtin `--custom-data` geçin. Bir bulut init dosyasıyla yapılandırabilirsiniz ilişkin bazı örnekler bakalım. Yaygın bir senaryo, ana bilgisayar adını bir VM oluşturmaktır. Varsayılan olarak, ana bilgisayar adı VM adı ile aynıdır. 

İlk olarak, bir kaynak grubu ile oluşturmak [az grubu oluşturma](/cli/azure/group#create). Aşağıdaki örnek adlı kaynak grubunu oluşturur *myResourceGroup* içinde *eastus* konumu:

```azurecli
az group create --name myResourceGroup --location eastus
```

Geçerli kabuğunuzu adlı bir dosya oluşturun *cloud_init_hostname.txt* ve aşağıdaki yapılandırma yapıştırın. Örneğin, yerel makinenizde olmayan bulut kabuğunda dosyası oluşturun. İstediğiniz herhangi bir düzenleyicisini kullanabilirsiniz. Bulut Kabuğu'nda girin `sensible-editor cloud_init_hostname.txt` dosyası oluşturun ve kullanılabilir düzenleyicileri listesini görmek için. Tüm bulut init dosyanın doğru şekilde kopyalandığından emin olun özellikle ilk satırı:

```yaml
#cloud-config
hostname: myhostname
```

Şimdi, bir VM oluşturmak [az vm oluşturma](/cli/azure/vm#create) ve bulut init dosyasıyla belirtin `--custom-data cloud_init_hostname.txt` gibi:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVMHostname \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_init_hostname.txt
```

Oluşturduktan sonra Azure CLI VM hakkında bilgi gösterir. Kullanım `publicIpAddress` SSH, VM için. Aşağıdaki gibi kendi adresini girin:

```bash
ssh azureuser@publicIpAddress
```

VM adı görmek için `hostname` gibi komut:

```bash
hostname
```

VM ana bilgisayar adı aşağıdaki örnek çıktıda görüldüğü gibi bulut init dosyasında ayarlanan değeri olarak rapor:

```bash
myhostname
```

## <a name="update-a-vm-with-cloud-init"></a>VM bulut init ile güncelleştirme
Güvenlik nedeniyle, ilk önyükleme en son güncelleştirmeleri uygulamak için bir VM yapılandırmak isteyebilirsiniz. Bulut init arasında farklı Linux distro'lar çalışırken belirtmek için gerek yoktur `apt` veya `yum` Paket Yöneticisi için. Bunun yerine, tanımladığınız `package_upgrade` ve kullanımdaki distro için uygun mekanizma belirlemek bulut başlatma işlemi sağlar. Bu iş akışı, aynı bulut başlatma komut dosyaları distro'lar kullanmanıza olanak sağlar.

Eylem yükseltme işleminde görmek için adlı bir bulut init dosyası oluşturun *cloud_init_upgrade.txt* ve aşağıdaki yapılandırma yapıştırın:

```yaml
#cloud-config
package_upgrade: true
```

Şimdi, bir VM oluşturmak [az vm oluşturma](/cli/azure/vm#create) ve bulut init dosyasıyla belirtin `--custom-data cloud_init_upgrade.txt` gibi:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVMUpgrade \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_init_upgrade.txt
```

Yukarıdaki komut çıktısı gösterilen VM ortak IP adresine SSH. Aşağıdaki gibi kendi ortak IP adresini girin:

```bash
ssh azureuser@publicIpAddress
```

Güncelleştirmeleri denetle ve paket yönetim aracını çalıştırın. Aşağıdaki örnek kullanır `apt-get` bir Ubuntu VM üzerinde:

```bash
sudo apt-get upgrade
```

Bulut init denetlediği ve önyükleme yüklü güncelleştirmeleri uygulamak için güncelleştirme yoktur aşağıdaki örnek çıktıda görüldüğü gibi:

```bash
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="add-a-user-to-a-vm-with-cloud-init"></a>VM bulut init ile kullanıcı ekleme
Yeni bir Linux VM üzerinde ilk görevlerden biridir kendiniz kullanımını önlemek bir kullanıcı eklemek için *kök*. SSH anahtarları, güvenlik ve kullanılabilirlik için en iyi uygulamalardan biridir. Anahtarları eklenir *~/.ssh/authorized_keys* bu bulut init komut dosyasıyla.

Bir Linux VM için bir kullanıcı eklemek için adlı bir bulut init dosyası oluşturun *cloud_init_add_user.txt* ve aşağıdaki yapılandırma yapıştırın. Ortak anahtarınız sağlayın (içeriğini gibi *~/.ssh/id_rsa.pub*) için *ssh yetkili-anahtarlar*:

```yaml
#cloud-config
users:
  - name: myadminuser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>
```

Şimdi, bir VM oluşturmak [az vm oluşturma](/cli/azure/vm#create) ve bulut init dosyasıyla belirtin `--custom-data cloud_init_add_user.txt` gibi:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVMUser \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_init_add_user.txt
```

Yukarıdaki komut çıktısı gösterilen VM ortak IP adresine SSH. Aşağıdaki gibi kendi ortak IP adresini girin:

```bash
ssh myadminuser@publicIpAddress
```

Kullanıcı VM ve belirtilen gruplara eklenen onaylamak için içeriğini görüntülemek */etc/grup* gibi dosya:

```bash
cat /etc/group
```

Aşağıdaki örnek çıkış kullanıcıdan gösterir *cloud_init_add_user.txt* dosya VM ve uygun grubuna eklendi:

```bash
root:x:0:
<snip />
sudo:x:27:myadminuser
<snip />
myadminuser:x:1000:
```

## <a name="next-steps"></a>Sonraki adımlar
Bulut init Linux Vm'leriniz önyükleme değiştirmek için standart yollardan biridir. Azure'da, Linux VM'NİZDE önyükleme veya çalışmaya başladıktan sonra değiştirmek için VM uzantıları kullanabilirsiniz. Örneğin, size Azure VM uzantısı üzerinde çalışan bir VM bir komut dosyası yürütme, ilk değil yalnızca üzerinde önyükleme için kullanabilirsiniz. VM uzantıları hakkında daha fazla bilgi için bkz: [VM uzantıları ve özellikleri](extensions-features.md), veya uzantısı kullanma hakkında daha fazla örnekler için bkz. [kullanıcıları, SSH, yönetmek ve denetlemek veya onarınVMAccessuzantısınıkullanarakAzureLinuxVM'lerdisklerde](using-vmaccess-extension.md).