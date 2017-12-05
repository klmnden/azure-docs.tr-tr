---
title: "Azure üzerinde bir Linux VM bash komut dosyasını çalıştırmak için bulut init kullanın | Microsoft Docs"
description: "Azure CLI 2.0 ile oluşturma sırasında bir Linux VM bir bash komut dosyasını çalıştırmak için bulut init kullanma"
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
ms.openlocfilehash: 1fcc432e8437a7fd284a75aa40454848a2af3006
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="use-cloud-init-to-run-a-bash-script-in-a-linux-vm-in-azure"></a>Azure'da bir Linux VM bash komut dosyasını çalıştırmak için bulut init kullanın
Bu makalede nasıl kullanılacağı gösterilmektedir [bulut init](https://cloudinit.readthedocs.io) çalıştırmak için mevcut bir bash bir Linux sanal makinede (VM) komut dosyası veya sanal makine ölçek Azure zamanında sağlama sırasında (VMSS) ayarlar. Kaynakları Azure tarafından sağlanan sonra bu bulut başlatma komut dosyaları ilk önyükleme çalıştırın. Bulut init yerel olarak Azure ve desteklenen Linux distro'lar işleyişi hakkında daha fazla bilgi için bkz: [bulut init genel bakış](using-cloud-init.md)

## <a name="run-a-bash-script-with-cloud-init"></a>Bulut başlatma ile bir bash komut dosyasını çalıştır
Bulut mevcut komut dosyalarınızı bulut-config Dönüştür gerekmez başlatma ile bulut init biri bash komut dosyası olan birden çok giriş türleri kabul eder.

Komut çalıştırmak için Linux özel komut dosyası Azure uzantısı kullanıyorsanız bulut init kullanmalarını geçirebilirsiniz. Ancak, Azure uzantıları betik hatalarına uyarı raporlama Tümleştirildi, komut dosyası başarısız olursa bir bulut init görüntü dağıtımı başarısız olmaz.

Bu eylem işlevselliğindeki görmek için test etmek için bir basit bash komut dosyası oluşturun. Gibi bulut init `#cloud-config` dosyası, bu komut dosyası olmalıdır burada, sanal makinenize kaynak sağlamak için AzureCLI komutları çalıştırırsınız için yerel.  Bu örnekte, yerel makinenizde olmayan bulut kabuğunda dosyası oluşturun. İstediğiniz herhangi bir düzenleyicisini kullanabilirsiniz. Girin `sensible-editor simple_bash.sh` dosyası oluşturun ve kullanılabilir düzenleyicileri listesini görmek için. # 1'ı kullanmayı seçin **nano** Düzenleyici. Tüm bulut init dosyanın doğru şekilde kopyalandığından emin olun özellikle ilk satırı.  

```bash
#!/bin/sh
echo "this has been written via cloud-init" + $(date) >> /tmp/myScript.txt
```

Bu görüntü dağıtmadan önce sahip bir kaynak grubu oluşturmak ihtiyacınız [az grubu oluşturma](/cli/azure/group#create) komutu. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Şimdi, bir VM oluşturmak [az vm oluşturma](/cli/azure/vm#create) ve bash komut dosyasıyla belirtin `--custom-data simple_bash.sh` gibi:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data simple_bash.sh \
  --generate-ssh-keys 
```
## <a name="verify-bash-script-has-run"></a>Bash betik çalıştırıldığını doğrulayın
Yukarıdaki komut çıktısı gösterilen VM ortak IP adresine SSH. Kendi girin **Publicıpaddress** gibi:

```bash
ssh <publicIpAddress>
```

Dönüştür **tmp** dizini ve bu myScript.txt dosyanın var olduğundan ve onu içinde uygun metni içeren doğrulayın.  Yoksa, kontrol edebilirsiniz **/var/log/cloud-init.log** daha fazla ayrıntı için.  İçin şu girdiyi arayın:

```bash
Running config-scripts-user using lock Running command ['/var/lib/cloud/instance/scripts/part-001']
```

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişiklikleri ek bulut init örnekleri için aşağıdakilere bakın:
 
- [Bir VM için ek Linux kullanıcı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede mevcut paketleri güncelleştirmek için bir paket Yöneticisi'ni çalıştırın](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirme](cloudinit-update-vm-hostname.md) 
- [Bir uygulama paketi yükleme, yapılandırma dosyalarını güncelleştirmek ve anahtarları Ekle](tutorial-automate-vm-deployment.md)