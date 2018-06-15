---
title: Azure üzerinde bir Linux VM ana bilgisayar adı ayarlamak için bulut init kullanın | Microsoft Docs
description: Azure CLI 2.0 ile oluşturma sırasında bir Linux VM özelleştirmek için bulut init kullanma
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
ms.openlocfilehash: a858a12ec81db7ae1c0a7b7cfea06fa2abdcdcc6
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
ms.locfileid: "29124033"
---
# <a name="use-cloud-init-to-set-hostname-for-a-linux-vm-in-azure"></a>Azure'da bir Linux VM ana bilgisayar adı ayarlamak için bulut init kullanın
Bu makalede nasıl kullanılacağı gösterilmektedir [bulut init](https://cloudinit.readthedocs.io) belirli bir ana bilgisayar adı bir sanal makine (VM) veya sanal makine yapılandırmak için ölçek (VMSS) Azure zamanında sağlama sırasında ayarlar. Kaynakları Azure tarafından sağlanan sonra bu bulut başlatma komut dosyaları ilk önyükleme çalıştırın. Bulut init yerel olarak Azure ve desteklenen Linux distro'lar işleyişi hakkında daha fazla bilgi için bkz: [bulut init genel bakış](using-cloud-init.md)

## <a name="set-the-hostname-with-cloud-init"></a>Bulut init ile ana bilgisayar adı ayarlama
Azure'da yeni bir sanal makine oluşturduğunuzda varsayılan olarak, ana bilgisayar adı VM adıyla aynıdır.  Azure ile bir VM oluştururken bu varsayılan konak adını değiştirmek için bir bulut başlatma komut dosyasını çalıştırmak için [az vm oluşturma](/cli/azure/vm#az_vm_create), bulut init dosyasıyla belirtin `--custom-data` geçin.  

Eylem yükseltme işleminde görmek için bir dosya adlı geçerli kabuğunda oluşturma *cloud_init_hostname.txt* ve aşağıdaki yapılandırma yapıştırın. Bu örnekte, yerel makinenizde olmayan bulut kabuğunda dosyası oluşturun. İstediğiniz herhangi bir düzenleyicisini kullanabilirsiniz. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor cloud_init_hostname.txt` adını girin. # 1'ı kullanmayı seçin **nano** Düzenleyici. Tüm bulut init dosyanın doğru şekilde kopyalandığından emin olun özellikle ilk satırı.  

```yaml
#cloud-config
hostname: myhostname
```

Bu görüntü dağıtmadan önce sahip bir kaynak grubu oluşturmak ihtiyacınız [az grubu oluşturma](/cli/azure/group#az_group_create) komutu. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Şimdi, bir VM oluşturmak [az vm oluşturma](/cli/azure/vm#az_vm_create) ve bulut init dosyasıyla belirtin `--custom-data cloud_init_hostname.txt` gibi:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_hostname.txt \
  --generate-ssh-keys 
```

Oluşturduktan sonra Azure CLI VM hakkında bilgi gösterir. Kullanım `publicIpAddress` SSH, VM için. Aşağıdaki gibi kendi adresini girin:

```bash
ssh <publicIpAddress>
```

VM adı görmek için `hostname` gibi komut:

```bash
hostname
```

VM ana bilgisayar adı aşağıdaki örnek çıktıda görüldüğü gibi bulut init dosyasında ayarlanan değeri olarak rapor:

```bash
myhostname
```

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişiklikleri ek bulut init örnekleri için aşağıdakilere bakın:
 
- [Bir VM için ek Linux kullanıcı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede mevcut paketleri güncelleştirmek için bir paket Yöneticisi'ni çalıştırın](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirme](cloudinit-update-vm-hostname.md) 
- [Bir uygulama paketi yükleme, yapılandırma dosyalarını güncelleştirmek ve anahtarları Ekle](tutorial-automate-vm-deployment.md)