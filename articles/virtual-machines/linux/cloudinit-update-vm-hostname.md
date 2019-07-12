---
title: Azure'da bir Linux VM için ana bilgisayar adı ayarlamak için cloud-init kullanma | Microsoft Docs
description: Azure CLI ile oluşturma sırasında bir Linux VM özelleştirmek için cloud-init kullanma
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
ms.openlocfilehash: 1e437200ec6af22d104f9878e7bdfd20141759fb
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67668196"
---
# <a name="use-cloud-init-to-set-hostname-for-a-linux-vm-in-azure"></a>Azure'da bir Linux sanal makinesi için ana bilgisayar adı ayarlamak için cloud-init kullanma
Bu makalede nasıl kullanılacağını gösterir [cloud-init](https://cloudinit.readthedocs.io) belirli bir ana bilgisayar adı, bir sanal makine (VM) veya sanal makinede yapılandırmak için ölçek kümeleri (VMSS zaman azure'da sağlama sırasında). Kaynakları Azure tarafından sağlanan sonra ilk önyüklemede bu cloud-init betikleri çalıştırın. Cloud-init yerel olarak desteklenen Linux dağıtımları ve Azure ile işleyişi hakkında daha fazla bilgi için bkz. [cloud-init genel bakış](using-cloud-init.md)

## <a name="set-the-hostname-with-cloud-init"></a>Cloud-init ile ana bilgisayar adı ayarlayın
Azure'da yeni bir sanal makine oluşturduğunuzda varsayılan olarak, ana bilgisayar adı VM adıyla aynıdır.  Azure ile bir VM oluşturduğunuzda, bu varsayılan konak adını değiştirmek için cloud-init betiğini çalıştırmak için [az vm oluşturma](/cli/azure/vm), cloud-init dosyası ile belirtin `--custom-data` geçin.  

Yükseltme işlemi uygulamada görmek için geçerli kabuğunuzda adlı bir dosya oluşturmak *cloud_init_hostname.txt* oluşturup aşağıdaki yapılandırmayı yapıştırın. Bu örnekte, dosyayı yerel makinenizde değil Cloud shell'de oluşturun. İstediğiniz düzenleyiciyi kullanabilirsiniz. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor cloud_init_hostname.txt` adını girin. Kullanılacak #1 seçin **nano** Düzenleyici. Tüm cloud-init dosyası doğru bir şekilde kopyalandığından emin olun başta birinci satır.  

```yaml
#cloud-config
hostname: myhostname
```

Bu görüntü dağıtmadan önce bir kaynak grubu oluşturmak için ihtiyacınız [az grubu oluşturma](/cli/azure/group) komutu. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Şimdi bir VM oluşturun [az vm oluşturma](/cli/azure/vm) ve cloud-init dosyası ile `--custom-data cloud_init_hostname.txt` gibi:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_hostname.txt \
  --generate-ssh-keys 
```

Oluşturulduktan sonra Azure CLI VM hakkında bilgi gösterir. Kullanım `publicIpAddress` sanal makinenize yönelik SSH için. Kendi adresinizi aşağıdaki gibi girin:

```bash
ssh <publicIpAddress>
```

VM adı görmek için `hostname` komutuyla şu şekilde:

```bash
hostname
```

VM konak adı, cloud-init dosyası içinde aşağıdaki örnek çıktıda gösterildiği gibi ayarlayın. Bu değer olarak raporlamalıdır:

```bash
myhostname
```

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişiklikleri ek cloud-init örnekleri için aşağıdakilere bakın:
 
- [Bir VM'ye ek Linux kullanıcı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede mevcut paketlerini güncelleştirmek için bir paket Yöneticisi'ni çalıştırın](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirin](cloudinit-update-vm-hostname.md) 
- [Bir uygulama paketi yükleme, yapılandırma dosyasını güncelleyin ve anahtarları ekleme](tutorial-automate-vm-deployment.md)
