---
title: Bir Linux VM üzerinde bir takas dosyası yapılandırma için cloud-init kullanma | Microsoft Docs
description: Azure CLI ile oluşturma sırasında bir Linux VM'de bir takas dosyası yapılandırma için cloud-init kullanma
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
ms.openlocfilehash: 626fd4739daf2506854c42f16ac986a361ebab38
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60729826"
---
# <a name="use-cloud-init-to-configure-a-swapfile-on-a-linux-vm"></a>Bir Linux VM üzerinde bir takas dosyası yapılandırma için cloud-init kullanma
Bu makalede nasıl kullanılacağını gösterir [cloud-init](https://cloudinit.readthedocs.io) takas dosyası üzerinde çeşitli Linux dağıtımları yapılandırmak için. Takas dosyası tarafından Linux Aracısı (hangi dağıtımların bir gerekli üzerinde temel WALA) geleneksel olarak yapılandırıldı.  Bu belge, cloud-init kullanarak sağlama süresi sırasında isteğe bağlı takas dosyası oluşturma işlemine özetler.  Cloud-init yerel olarak desteklenen Linux dağıtımları ve Azure ile işleyişi hakkında daha fazla bilgi için bkz. [cloud-init genel bakış](using-cloud-init.md)

## <a name="create-swapfile-for-ubuntu-based-images"></a>Ubuntu tabanlı görüntüler için takas dosyası oluşturma
Azure üzerinde varsayılan olarak, Ubuntu galeri görüntüleri takas dosyaları oluşturmayın. Takas dosyası yapılandırma VM cloud-init kullanarak sağlama sırasında enable - Lütfen görmek için [AzureSwapPartitions belge](https://wiki.ubuntu.com/AzureSwapPartitions) Ubuntu Wiki.

## <a name="create-swapfile-for-red-hat-and-centos-based-images"></a>Red Hat ve CentOS tabanlı görüntüler için takas dosyası oluşturma

Geçerli kabuğunuzda adlı bir dosya oluşturun *cloud_init_swapfile.txt* oluşturup aşağıdaki yapılandırmayı yapıştırın. Bu örnekte, dosyayı yerel makinenizde değil Cloud shell'de oluşturun. İstediğiniz düzenleyiciyi kullanabilirsiniz. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor cloud_init_swapfile.txt` adını girin. Kullanılacak #1 seçin **nano** Düzenleyici. Tüm cloud-init dosyası doğru bir şekilde kopyalandığından emin olun başta birinci satır.  

```yaml
#cloud-config
disk_setup:
  ephemeral0:
    table_type: gpt
    layout: [66, [33,82]]
    overwrite: true
fs_setup:
  - device: ephemeral0.1
    filesystem: ext4
  - device: ephemeral0.2
    filesystem: swap
mounts:
  - ["ephemeral0.1", "/mnt"]
  - ["ephemeral0.2", "none", "swap", "sw", "0", "0"]
```

Bu görüntü dağıtmadan önce bir kaynak grubu oluşturmak için ihtiyacınız [az grubu oluşturma](/cli/azure/group) komutu. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Şimdi bir VM oluşturun [az vm oluşturma](/cli/azure/vm) ve cloud-init dosyası ile `--custom-data cloud_init_swapfile.txt` gibi:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_swapfile.txt \
  --generate-ssh-keys 
```

## <a name="verify-swapfile-was-created"></a>Takas dosyası oluşturuldu doğrulayın
SSH için yukarıdaki komut çıktısında gösterilen sanal makinenizin genel IP adresi. Kendi girin **Publicıpaddress** gibi:

```bash
ssh <publicIpAddress>
```

SSH'ed VM'de oturum açtıktan sonra takas dosyası oluşturulup oluşturulmadığını denetleyin.

```bash
swapon -s
```

Bu komutun çıktısı, şöyle görünmelidir:

```text
Filename                Type        Size    Used    Priority
/dev/sdb2  partition   2494440 0   -1
```

> [!NOTE] 
> Yapılandırılan bir takas dosyasına sahip mevcut bir Azure görüntüsünü varsa ve yeni görüntüleri için takas dosyası yapılandırmasını değiştirmek istiyorsanız, mevcut takas dosyası kaldırmanız gerekir. Lütfen 'Özelleştir sağlamak için cloud-init tarafından görüntüleri' daha fazla ayrıntı için bkz.

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişiklikleri ek cloud-init örnekleri için aşağıdakilere bakın:
 
- [Bir VM'ye ek Linux kullanıcı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede mevcut paketlerini güncelleştirmek için bir paket Yöneticisi'ni çalıştırın](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirin](cloudinit-update-vm-hostname.md) 
- [Bir uygulama paketi yükleme, yapılandırma dosyasını güncelleyin ve anahtarları ekleme](tutorial-automate-vm-deployment.md)
