---
title: Azure'da oluşturma sırasında bir Linux VM özelleştirmek için bulut init kullanma | Microsoft Docs
description: Azure CLI 1.0 ile oluşturma sırasında bir Linux VM özelleştirmek için bulut init kullanma
services: virtual-machines-linux
documentationcenter: ''
author: vlivech
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/26/2016
ms.author: v-livech
ms.openlocfilehash: 2e9182a18a2827ed7f54f5fd042e5934b3b1fd5c
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="use-cloud-init-to-customize-a-linux-vm-during-creation-with-the-azure-cli-10"></a>Azure CLI 1.0 ile oluşturma sırasında bir Linux VM özelleştirmek için bulut init kullanın
Bu makalede ayarlamak yüklü güncelleştirme paketleri, ana bilgisayar adı ve kullanıcı hesaplarını yönetmek için bir bulut init betik yapılacağını gösterir.  Bulut başlatma komut dosyaları, Azure clı'dan VM oluşturma sırasında çağrılır.  Bu makale için şunlar gereklidir:

* bir Azure hesabı ([ücretsiz deneme sürümü edinin](https://azure.microsoft.com/pricing/free-trial/)).
* `azure login` ile oturum açılmış [Azure CLI'si](../../cli-install-nodejs.md).
* Azure CLI'si, `azure config mode arm` Azure Resource Manager modunda *olmalıdır*.

## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure CLI 1.0](#quick-commands) – bizim CLI Klasik ve kaynak yönetimi dağıtım modeline (Bu makalede)
- [Azure CLI 2.0](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız

## <a name="quick-commands"></a>Hızlı Komutlar
Ana bilgisayar adını ayarlar, tüm paketleri güncelleştirir ve bir sudo kullanıcı için Linux ekleyen bir bulut init.txt komut dosyası oluşturun.

```sh
#cloud-config
hostname: myVMhostname
apt_upgrade: true
users:
  - name: myNewAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myVM
```
Sanal makineleri içine başlatmak için bir kaynak grubu oluşturun.

```azurecli
azure group create myResourceGroup westus
```

Önyükleme sırasında yapılandırmak için bulut init kullanarak bir Linux VM oluşturun.

```azurecli
azure vm create \
  -g myResourceGroup \
  -n myVM \
  -l westus \
  -y Linux \
  -f myVMnic \
  -F myVNet \
  -P 10.0.0.0/22 \
  -j mySubnet \
  -k 10.0.0.0/24 \
  -Q canonical:ubuntuserver:14.04.2-LTS:latest \
  -M ~/.ssh/id_rsa.pub \
  -u myAdminUser \
  -C cloud-init.txt
```

## <a name="detailed-walkthrough"></a>Ayrıntılı kılavuz
### <a name="introduction"></a>Giriş
Yeni bir Linux VM başlattığında, özelleştirilmiş veya gereksinimleriniz için hazır herhangi bir şeyin standart bir Linux VM aldıklarından. [Bulut init](https://cloudinit.readthedocs.org) için ilk kez önyükleme gibi bir komut dosyası veya yapılandırma ayarlarını, Linux VM içine eklemesine standart bir yoludur.

Azure üzerinde bir üç farklı yolu vardır olarak bir Linux VM üzerine değişiklik yapmak için dağıtılan veya önyüklendiğinde.

* Bulut init kullanarak komut dosyalarını yerleştirir.
* Azure kullanarak komut dosyalarını Ekle [VMAccess uzantısını](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Bulut init kullanarak bir Azure şablonu.
* Bir Azure şablonu kullanarak [CustomScriptExtention](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Komut dosyaları önyükleme sonra herhangi bir zamanda eklemesine:

* SSH komutları doğrudan çalıştırmak için
* Azure kullanarak komut dosyalarını Ekle [VMAccess uzantısını](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), imperatively veya bir Azure şablonu
* Yapılandırma yönetimi araçları Ansible, Salt, Chef, Puppet gibi ve.

> [!NOTE]
> : Kök SSH kullanarak aynı şekilde gibi bir komut dosyası VMAccess uzantısını yürütür.  Ancak, VM uzantısını kullanarak çeşitli özellikler senaryonuza bağlı olarak yararlı olabilecek Azure sağladığı sağlar.
> 
> 

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Azure VM bulut init kullanılabilirliğine hızlı Oluştur görüntü diğer adları:
| Diğer ad | Yayımcı | Sunduğu | SKU | Sürüm | Bulut başlatma |
|:--- |:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |en son |hayır |
| CoreOS |CoreOS |CoreOS |Dengeli |en son |evet |
| Debian |credativ |Debian |8 |en son |hayır |
| openSUSE |SUSE |openSUSE |13.2 |en son |hayır |
| RHEL |RedHat |RHEL |7.2 |en son |hayır |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |en son |evet |

Microsoft bulut dahil ve Azure'a sağladıkları görüntülerinde çalışma başlatma almak için iş ortakları ile çalışmaktadır.

## <a name="adding-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a>Azure CLI ile VM oluşturmak için bir bulut init komut dosyası ekleme
Azure üzerinde bir VM oluştururken, bir bulut başlatma komut dosyasını başlatmak için Azure CLI kullanarak bulut init dosyasını belirtin `--custom-data` geçin.

Sanal makineleri içine başlatmak için bir kaynak grubu oluşturun.

```azurecli
azure group create myResourceGroup westus
```

Önyükleme sırasında yapılandırmak için bulut init kullanarak bir Linux VM oluşturun.

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubnet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud-init.txt
```

## <a name="creating-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a>Bir Linux VM konak adı ayarlamak için bir bulut init komut dosyası oluşturma
Tüm Linux VM için basit ve en önemli ayarlardan birini ana bilgisayar adı olacaktır. Biz kolayca bu bulut init ile bu komut dosyası kullanarak ayarlayabilirsiniz.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Adlı örnek bulut başlatma komut dosyasını `cloud_config_hostname.txt`.
```sh
#cloud-config
hostname: myservername
```

VM ilk başlatma sırasında bu bulut init komut dosyası ana bilgisayar adını ayarlar `myservername`.

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud_config_hostname.txt
```

Oturum açma ve ana bilgisayar yeni bir VM adını doğrulayın.

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-to-update-linux"></a>Linux güncelleştirmek için bir bulut init komut dosyası oluşturma
Güvenlik için ilk önyüklemede güncelleştirmek için Ubuntu VM istiyor.  Bulut init, kullandığınız Linux dağıtım bağlı olarak izleme komut dosyalarıyla yapabiliriz kullanma.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a>Örnek bulut init betik `cloud_config_apt_upgrade.txt` Debian ailesi için
```sh
#cloud-config
apt_upgrade: true
```

Linux önyüklendikten sonra yüklü olan paketlerin aracılığıyla güncelleştirilir `apt-get`.

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud_config_apt_upgrade.txt
```

Oturum açma ve tüm paketler güncelleştirilir doğrulayın.

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="creating-a-cloud-init-script-to-add-a-user-to-linux"></a>Linux için bir kullanıcı eklemek için bir bulut init komut dosyası oluşturma
Yeni bir Linux VM ilk görevlerde biri kendiniz için bir kullanıcı eklemek veya kullanmaktan kaçınmak için `root`. SSH anahtarları olan kullanılabilirlik ve güvenlik için en iyi uygulama ve eklendikten `~/.ssh/authorized_keys` bu bulut init komut dosyasıyla.

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a>Örnek bulut init betik `cloud_config_add_users.txt` Debian ailesi için
```sh
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

Linux önyüklendikten sonra listelenen tüm kullanıcılar oluşturulur ve sudo grubuna eklenir.

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud_config_add_users.txt
```

Oturum açma ve yeni oluşturulan kullanıcı doğrulayın.

```bash
ssh myVM
cat /etc/group
```

Çıktı

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a>Sonraki Adımlar
Bulut init Linux VM'NİZDE önyüklemede değiştirmek için bir standart yolu durumundadır. Ayrıca Azure önyükleme veya çalışırken, LinuxVM değiştirmenize izin VM uzantıları sahiptir. Örneğin, VM çalışırken SSH veya kullanıcı bilgilerini sıfırlama Azure VMAccessExtension kullanabilirsiniz. Bulut başlatma ile parolayı sıfırlamak için bir yeniden başlatma gerekir.

[Sanal makine uzantıları ve özellikleri hakkında](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Kullanıcılar, SSH ve onay yönetmek veya VMAccess uzantısını kullanarak Azure Linux VM'ler disklerde onarın](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

