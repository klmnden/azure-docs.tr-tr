---
title: Azure Linux VM'de bir kullanıcı eklemek için cloud-init kullanma | Microsoft Docs
description: Azure CLI ile oluşturma sırasında bir Linux VM için bir kullanıcı eklemek için cloud-init kullanma
services: virtual-machines-linux
documentationcenter: ''
author: rickstercdn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: bcea130652789a84d332247445d8e25b2f7ac42e
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67671785"
---
# <a name="use-cloud-init-to-add-a-user-to-a-linux-vm-in-azure"></a>Azure'da bir Linux sanal makinesi için bir kullanıcı eklemek için cloud-init kullanma
Bu makalede nasıl kullanılacağını gösterir [cloud-init](https://cloudinit.readthedocs.io) bir sanal makine (VM) veya sanal makinede bir kullanıcı eklemek için ölçek kümeleri (VMSS zaman azure'da sağlama sırasında). Kaynakları Azure tarafından sağlanan sonra ilk önyüklemede bu cloud-init betiği çalıştırır. Cloud-init yerel olarak desteklenen Linux dağıtımları ve Azure ile işleyişi hakkında daha fazla bilgi için bkz. [cloud-init genel bakış](using-cloud-init.md).

## <a name="add-a-user-to-a-vm-with-cloud-init"></a>Cloud-init ile VM kullanıcı ekleme
Yeni bir Linux VM üzerinde ilk görevlerden biridir kendiniz kullanımını önlemek için ek bir kullanıcı eklemek için *kök*. SSH anahtarları, güvenlik ve kullanılabilirlik için en iyi uygulamalardan biridir. Anahtarları eklenir *~/.ssh/authorized_keys* bu cloud-init betik dosyası.

Bir Linux VM için bir kullanıcı eklemek için geçerli kabuğunuzda adlı bir dosya oluşturmak *cloud_init_add_user.txt* oluşturup aşağıdaki yapılandırmayı yapıştırın. Bu örnekte, dosyayı yerel makinenizde değil Cloud shell'de oluşturun. İstediğiniz düzenleyiciyi kullanabilirsiniz. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor cloud_init_add_user.txt` adını girin. Kullanılacak #1 seçin **nano** Düzenleyici. Tüm cloud-init dosyası doğru bir şekilde kopyalandığından emin olun başta birinci satır.  Ortak anahtarınızı sağlamanız gerekir (içeriğini gibi *~/.ssh/id_rsa.pub*) değerini `ssh-authorized-keys:` -alan örneği basitleştirmek amacıyla buraya kısaltıldı.

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
> #Cloud-config dosyası içerir `- default` parametresi dahil. Bu kullanıcı için sağlama sırasında oluşturulan mevcut yönetici kullanıcı sonuna ekler. Bir kullanıcı olmaksızın oluşturursanız `- default` parametre - Azure platformu tarafından oluşturulan otomatik oluşturulan yönetici kullanıcı olacak üzerine. 

Bu görüntü dağıtmadan önce bir kaynak grubu oluşturmak için ihtiyacınız [az grubu oluşturma](/cli/azure/group) komutu. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Şimdi bir VM oluşturun [az vm oluşturma](/cli/azure/vm) ve cloud-init dosyası ile `--custom-data cloud_init_add_user.txt` gibi:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_add_user.txt \
  --generate-ssh-keys 
```

SSH için yukarıdaki komut çıktısında gösterilen sanal makinenizin genel IP adresi. Kendi girin **Publicıpaddress** gibi:

```bash
ssh <publicIpAddress>
```

Kullanıcı VM ve belirtilen gruplara eklendiğini doğrulamak için içeriğini görüntülemek */etc/grup* aşağıdaki gibi:

```bash
cat /etc/group
```

Aşağıdaki örnek çıktı gösterilmektedir kullanıcıdan *cloud_init_add_user.txt* VM ve uygun grubu dosyası eklendi:

```bash
root:x:0:
<snip />
sudo:x:27:myadminuser
<snip />
myadminuser:x:1000:
```

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişiklikleri ek cloud-init örnekleri için aşağıdakilere bakın:
 
- [Bir VM'ye ek Linux kullanıcı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede mevcut paketlerini güncelleştirmek için bir paket Yöneticisi'ni çalıştırın](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirin](cloudinit-update-vm-hostname.md) 
- [Bir uygulama paketi yükleme, yapılandırma dosyasını güncelleyin ve anahtarları ekleme](tutorial-automate-vm-deployment.md)
