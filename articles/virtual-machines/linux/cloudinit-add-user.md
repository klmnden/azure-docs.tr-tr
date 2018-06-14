---
title: Azure'da bir Linux VM için bir kullanıcı eklemek için bulut init kullanın | Microsoft Docs
description: Azure CLI 2.0 ile oluşturma sırasında bir Linux VM için bir kullanıcı eklemek için bulut init kullanma
services: virtual-machines-linux
documentationcenter: ''
author: rickstercdn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: ce4421fc8276f215564cb7a171a215cc166c8517
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
ms.locfileid: "29123472"
---
# <a name="use-cloud-init-to-add-a-user-to-a-linux-vm-in-azure"></a>Azure'da bir Linux VM için bir kullanıcı eklemek için bulut init kullanın
Bu makalede nasıl kullanılacağı gösterilmektedir [bulut init](https://cloudinit.readthedocs.io) bir sanal makine (VM) veya sanal makinede bir kullanıcı eklemek için ölçek (VMSS) Azure zamanında sağlama sırasında ayarlar. Kaynakları Azure tarafından sağlanan sonra ilk önyükleme bu bulut başlatma komut dosyasını çalıştırır. Bulut init yerel olarak Azure ve desteklenen Linux distro'lar işleyişi hakkında daha fazla bilgi için bkz: [bulut init genel bakış](using-cloud-init.md).

## <a name="add-a-user-to-a-vm-with-cloud-init"></a>VM bulut init ile kullanıcı ekleme
Yeni bir Linux VM üzerinde ilk görevlerden biridir kendiniz kullanımını önlemek ek bir kullanıcı eklemek için *kök*. SSH anahtarları, güvenlik ve kullanılabilirlik için en iyi uygulamalardan biridir. Anahtarları eklenir *~/.ssh/authorized_keys* bu bulut init komut dosyasıyla.

Bir Linux VM için bir kullanıcı eklemek için bir dosya adlı geçerli kabuğunda oluşturma *cloud_init_add_user.txt* ve aşağıdaki yapılandırma yapıştırın. Bu örnekte, yerel makinenizde olmayan bulut kabuğunda dosyası oluşturun. İstediğiniz herhangi bir düzenleyicisini kullanabilirsiniz. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor cloud_init_add_user.txt` adını girin. # 1'ı kullanmayı seçin **nano** Düzenleyici. Tüm bulut init dosyanın doğru şekilde kopyalandığından emin olun özellikle ilk satırı.  Ortak anahtarınızı sağlamanız gerekir (içeriğini gibi *~/.ssh/id_rsa.pub*) değerini `ssh-authorized-keys:` -örneği basitleştirmek için burada kısaltıldı.

```yaml
#cloud-config
users:
  - default
  - name: myadminuser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>
```
> [!NOTE] 
> #Cloud-config dosyasına içeren `- default` dahil parametresi. Bu sağlama işlemi sırasında oluşturulan mevcut yönetici kullanıcı kullanıcı eklenir. Bir kullanıcı olmaksızın oluşturursanız `- default` parametre - Azure platformu tarafından oluşturulan otomatik oluşturulan yönetici kullanıcı olacaktır üzerine. 

Bu görüntü dağıtmadan önce sahip bir kaynak grubu oluşturmak ihtiyacınız [az grubu oluşturma](/cli/azure/group#az_group_create) komutu. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Şimdi, bir VM oluşturmak [az vm oluşturma](/cli/azure/vm#az_vm_create) ve bulut init dosyasıyla belirtin `--custom-data cloud_init_add_user.txt` gibi:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_add_user.txt \
  --generate-ssh-keys 
```

Yukarıdaki komut çıktısı gösterilen VM ortak IP adresine SSH. Kendi girin **Publicıpaddress** gibi:

```bash
ssh <publicIpAddress>
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
Yapılandırma değişiklikleri ek bulut init örnekleri için aşağıdakilere bakın:
 
- [Bir VM için ek Linux kullanıcı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede mevcut paketleri güncelleştirmek için bir paket Yöneticisi'ni çalıştırın](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirme](cloudinit-update-vm-hostname.md) 
- [Bir uygulama paketi yükleme, yapılandırma dosyalarını güncelleştirmek ve anahtarları Ekle](tutorial-automate-vm-deployment.md)