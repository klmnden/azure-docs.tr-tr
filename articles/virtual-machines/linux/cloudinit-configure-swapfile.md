---
title: "Bir Linux VM üzerinde bir takas yapılandırmak için bulut init kullanın | Microsoft Docs"
description: "Azure CLI 2.0 ile oluşturma sırasında bir Linux VM bir takas yapılandırmak için bulut init kullanma"
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: 7f9defc1f414819cf856fc92f5eb51eafdc67be9
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="use-cloud-init-to-configure-a-swapfile-on-a-linux-vm"></a>Bir Linux VM üzerinde bir takas yapılandırmak için bulut init kullanın
Bu makalede nasıl kullanılacağı gösterilmektedir [bulut init](https://cloudinit.readthedocs.io) swapfile üzerinde çeşitli Linux dağıtımları yapılandırmak için. Takas tarafından Linux Aracısı (bir hangi dağıtımları gerekli temel WALA) geleneksel olarak yapılandırıldı.  Bu belge, bulut init kullanarak sağlama süresi sırasında isteğe bağlı swapfile oluşturma işlemi özetler.  Bulut init yerel olarak Azure ve desteklenen Linux distro'lar işleyişi hakkında daha fazla bilgi için bkz: [bulut init genel bakış](using-cloud-init.md)

## <a name="create-swapfile-for-ubuntu-based-images"></a>Takas dayalı Ubuntu görüntüleri oluşturma
Azure üzerinde varsayılan olarak, Ubuntu galeri görüntüleri takas dosyaları oluşturmayın. Takas dosyası yapılandırması VM bulut init kullanarak zaman sağlama sırasında enable - Lütfen görmek için [AzureSwapPartitions belge](https://wiki.ubuntu.com/AzureSwapPartitions) Ubuntu Wiki.

## <a name="create-swapfile-for-redhat-and-centos-based-images"></a>Takas RedHat ve CentOS tabanlı görüntüleri oluşturma

Bir dosya adı, geçerli Kabuğu'nda oluşturma *cloud_init_swapfile.txt* ve aşağıdaki yapılandırma yapıştırın. Bu örnekte, yerel makinenizde olmayan bulut kabuğunda dosyası oluşturun. İstediğiniz herhangi bir düzenleyicisini kullanabilirsiniz. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor cloud_init_swapfile.txt` adını girin. # 1'ı kullanmayı seçin **nano** Düzenleyici. Tüm bulut init dosyanın doğru şekilde kopyalandığından emin olun özellikle ilk satırı.  

```yaml
#cloud-config
disk_setup:
ephemeral0:
table_type: gpt
layout: [66, [33,82]]
overwrite: True
fs_setup:
- device: ephemeral0.1
filesystem: ext4
- device: ephemeral0.2
filesystem: swap
mounts:
- ["ephemeral0.1", "/mnt"]
- ["ephemeral0.2", "none", "swap", "sw", "0", "0"]
```

Bu görüntü dağıtmadan önce sahip bir kaynak grubu oluşturmak ihtiyacınız [az grubu oluşturma](/cli/azure/group#az_group_create) komutu. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Şimdi, bir VM oluşturmak [az vm oluşturma](/cli/azure/vm#az_vm_create) ve bulut init dosyasıyla belirtin `--custom-data cloud_init_swapfile.txt` gibi:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_swapfile.txt \
  --generate-ssh-keys 
```

## <a name="verify-swapfile-was-created"></a>Takas oluşturulduğu doğrulayın
Yukarıdaki komut çıktısı gösterilen VM ortak IP adresine SSH. Kendi girin **Publicıpaddress** gibi:

```bash
ssh <publicIpAddress>
```

Vm SSH'ed sahip olduğunda, takas oluşturulup oluşturulmadığını denetleyin

```bash
swapon -s
```

Bu komutun çıktısı aşağıdaki gibi görünmelidir:

```text
Filename                Type        Size    Used    Priority
/dev/sdb2  partition   2494440 0   -1
```

> [!NOTE] 
> Yapılandırılan bir takas dosyasına sahip mevcut bir Azure görüntüsünü varsa ve yeni görüntüleri takas dosyası yapılandırmasını değiştirmek istediğiniz varolan takas dosyası kaldırmanız gerekir. Lütfen daha fazla ayrıntı için 'Özelleştir sağlamak için bulut-Init görüntüleri' belgesine bakın.

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişiklikleri ek bulut init örnekleri için aşağıdakilere bakın:
 
- [Bir VM için ek Linux kullanıcı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede mevcut paketleri güncelleştirmek için bir paket Yöneticisi'ni çalıştırın](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirme](cloudinit-update-vm-hostname.md) 
- [Bir uygulama paketi yükleme, yapılandırma dosyalarını güncelleştirmek ve anahtarları Ekle](tutorial-automate-vm-deployment.md)