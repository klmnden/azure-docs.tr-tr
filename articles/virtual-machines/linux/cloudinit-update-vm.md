---
title: Güncelleştirmek ve bir Azure Linux VM'de paketleri yüklemek için cloud-init kullanma | Microsoft Docs
description: Cloud-init güncelleştirmek için nasıl kullanılacağını ve Azure CLI ile Linux VM oluşturma sırasında yükleme paketleri
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
ms.date: 04/20/2018
ms.author: rclaus
ms.openlocfilehash: d5f4dc7f4abc13f253a206a63e65faf1106f9c7c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60627750"
---
# <a name="use-cloud-init-to-update-and-install-packages-in-a-linux-vm-in-azure"></a>Güncelleştirmek ve azure'da bir Linux sanal makinesi, paketleri yüklemek için cloud-init kullanma
Bu makalede nasıl kullanılacağını gösterir [cloud-init](https://cloudinit.readthedocs.io) güncelleştirme paketleri bir Linux için sanal makine (VM) ya da sanal makine ölçek kümeleri (VMSS zaman azure'da sağlama sırasında). Kaynakları Azure tarafından sağlanan sonra ilk önyüklemede bu cloud-init betikleri çalıştırın. Cloud-init yerel olarak desteklenen Linux dağıtımları ve Azure ile işleyişi hakkında daha fazla bilgi için bkz. [cloud-init genel bakış](using-cloud-init.md)

## <a name="update-a-vm-with-cloud-init"></a>Cloud-init ile VM güncelleştirme
Güvenlik nedenleriyle ilk önyüklemede en son güncelleştirmeleri uygulamak için bir VM yapılandırmak isteyebilirsiniz. Cloud-init arasında farklı Linux dağıtım paketlerini çalıştığı belirtmek için gerek yoktur `apt` veya `yum` için Paket Yöneticisi. Bunun yerine, tanımladığınız `package_upgrade` ve kullanımdaki distro uygun mekanizması belirlemek cloud-init işlem izin verin. Bu iş akışı aynı cloud-init betikleri arasında dağıtım paketlerini kullanmanıza olanak tanır.

Yükseltme işlemi uygulamada görmek için geçerli kabuğunuzda adlı bir dosya oluşturmak *cloud_init_upgrade.txt* oluşturup aşağıdaki yapılandırmayı yapıştırın. Bu örnekte, dosyayı yerel makinenizde değil Cloud shell'de oluşturun. İstediğiniz düzenleyiciyi kullanabilirsiniz. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor cloud_init_upgrade.txt` adını girin. Kullanılacak #1 seçin **nano** Düzenleyici. Tüm cloud-init dosyası doğru bir şekilde kopyalandığından emin olun başta birinci satır.  

```yaml
#cloud-config
package_upgrade: true
packages:
- httpd
```

Bu görüntü dağıtmadan önce bir kaynak grubu oluşturmak için ihtiyacınız [az grubu oluşturma](/cli/azure/group) komutu. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Şimdi bir VM oluşturun [az vm oluşturma](/cli/azure/vm) ve cloud-init dosyası ile `--custom-data cloud_init_upgrade.txt` gibi:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_upgrade.txt \
  --generate-ssh-keys 
```

SSH için yukarıdaki komut çıktısında gösterilen sanal makinenizin genel IP adresi. Kendi girin **Publicıpaddress** gibi:

```bash
ssh <publicIpAddress>
```

Güncelleştirmeleri denetle ve paket yönetim aracını çalıştırın.

```bash
sudo yum update
```

Cloud-init kontrol ve önyükleme yüklü güncelleştirmeleri uygulamak için hiçbir ek güncelleştirme olmalıdır.  Yüklenmesini yanı sıra güncelleştirme işlemi, değiştirilen paket sayısı gördüğünüz `httpd` çalıştırarak `yum history` ve aşağıdakine benzer bir çıktı gözden geçirin.

```bash
Loaded plugins: fastestmirror, langpacks
ID     | Command line             | Date and time    | Action(s)      | Altered
-------------------------------------------------------------------------------
     3 | -t -y install httpd      | 2018-04-20 22:42 | Install        |    5
     2 | -t -y upgrade            | 2018-04-20 22:38 | I, U           |   65
     1 |                          | 2017-12-12 20:32 | Install        |  522
```

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişiklikleri ek cloud-init örnekleri için aşağıdakilere bakın:
 
- [Bir VM'ye ek Linux kullanıcı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede mevcut paketlerini güncelleştirmek için bir paket Yöneticisi'ni çalıştırın](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirin](cloudinit-update-vm-hostname.md) 
- [Bir uygulama paketi yükleme, yapılandırma dosyasını güncelleyin ve anahtarları ekleme](tutorial-automate-vm-deployment.md)
