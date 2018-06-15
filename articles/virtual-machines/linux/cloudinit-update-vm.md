---
title: Azure üzerinde bir Linux VM paketleri yüklemek ve güncelleştirmek için bulut init kullanın | Microsoft Docs
description: Bulut init güncelleştirmek için nasıl kullanılacağı ve bir Linux VM oluşturma sırasında Azure CLI 2.0 ile yükleme paketleri
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
ms.openlocfilehash: 8d5835b5d1b0c2f77bdf5e1a2b808478b8f4de22
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32186163"
---
# <a name="use-cloud-init-to-update-and-install-packages-in-a-linux-vm-in-azure"></a>Azure'da bir Linux VM paketleri yüklemek ve güncelleştirmek için bulut init kullanın
Bu makalede nasıl kullanılacağı gösterilmektedir [bulut init](https://cloudinit.readthedocs.io) güncelleştirme paketleri bir Linux sanal makine (VM) ya da sanal makine Ölçek (VMSS) Azure zamanında sağlama sırasında ayarlar. Kaynakları Azure tarafından sağlanan sonra bu bulut başlatma komut dosyaları ilk önyükleme çalıştırın. Bulut init yerel olarak Azure ve desteklenen Linux distro'lar işleyişi hakkında daha fazla bilgi için bkz: [bulut init genel bakış](using-cloud-init.md)

## <a name="update-a-vm-with-cloud-init"></a>VM bulut init ile güncelleştirme
Güvenlik nedeniyle, ilk önyükleme en son güncelleştirmeleri uygulamak için bir VM yapılandırmak isteyebilirsiniz. Bulut init arasında farklı Linux distro'lar çalışırken belirtmek için gerek yoktur `apt` veya `yum` Paket Yöneticisi için. Bunun yerine, tanımladığınız `package_upgrade` ve kullanımdaki distro için uygun mekanizma belirlemek bulut başlatma işlemi sağlar. Bu iş akışı, aynı bulut başlatma komut dosyaları distro'lar kullanmanıza olanak sağlar.

Eylem yükseltme işleminde görmek için bir dosya adlı geçerli kabuğunda oluşturma *cloud_init_upgrade.txt* ve aşağıdaki yapılandırma yapıştırın. Bu örnekte, yerel makinenizde olmayan bulut kabuğunda dosyası oluşturun. İstediğiniz düzenleyiciyi kullanabilirsiniz. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor cloud_init_upgrade.txt` adını girin. # 1'ı kullanmayı seçin **nano** Düzenleyici. Tüm bulut init dosyanın doğru şekilde kopyalandığından emin olun özellikle ilk satırı.  

```yaml
#cloud-config
package_upgrade: true
packages:
- httpd
```

Bu görüntü dağıtmadan önce sahip bir kaynak grubu oluşturmak ihtiyacınız [az grubu oluşturma](/cli/azure/group#az_group_create) komutu. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Şimdi, bir VM oluşturmak [az vm oluşturma](/cli/azure/vm#az_vm_create) ve bulut init dosyasıyla belirtin `--custom-data cloud_init_upgrade.txt` gibi:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_upgrade.txt \
  --generate-ssh-keys 
```

Yukarıdaki komut çıktısı gösterilen VM ortak IP adresine SSH. Kendi girin **Publicıpaddress** gibi:

```bash
ssh <publicIpAddress>
```

Güncelleştirmeleri denetle ve paket yönetim aracını çalıştırın.

```bash
sudo yum update
```

Bulut init denetlediği ve önyükleme yüklü güncelleştirmeleri uygulamak için hiçbir ek güncelleştirmeler olmalıdır.  Yüklemesini yanı sıra güncelleştirme işlemi, değiştirilen paket sayısı gördüğünüz `httpd` çalıştırarak `yum history` ve birine benzer bir çıktı gözden geçirin.

```bash
Loaded plugins: fastestmirror, langpacks
ID     | Command line             | Date and time    | Action(s)      | Altered
-------------------------------------------------------------------------------
     3 | -t -y install httpd      | 2018-04-20 22:42 | Install        |    5
     2 | -t -y upgrade            | 2018-04-20 22:38 | I, U           |   65
     1 |                          | 2017-12-12 20:32 | Install        |  522
```

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişiklikleri ek bulut init örnekleri için aşağıdakilere bakın:
 
- [Bir VM için ek Linux kullanıcı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede mevcut paketleri güncelleştirmek için bir paket Yöneticisi'ni çalıştırın](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirme](cloudinit-update-vm-hostname.md) 
- [Bir uygulama paketi yükleme, yapılandırma dosyalarını güncelleştirmek ve anahtarları Ekle](tutorial-automate-vm-deployment.md)