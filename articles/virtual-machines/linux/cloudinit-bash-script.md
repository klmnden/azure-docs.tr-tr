---
title: Azure'da bir Linux VM bash betiğini çalıştırmak için cloud-init kullanma | Microsoft Docs
description: Azure CLI ile oluşturma sırasında bir Linux VM'de bir bash betiğini çalıştırmak için cloud-init kullanma
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
ms.openlocfilehash: 4f65ebfd2e1ce508c5cf9b224871102a35b55fe0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60627852"
---
# <a name="use-cloud-init-to-run-a-bash-script-in-a-linux-vm-in-azure"></a>Azure'da bir Linux sanal makinesi bir bash betiğini çalıştırmak için cloud-init kullanma
Bu makalede nasıl kullanılacağını gösterir [cloud-init](https://cloudinit.readthedocs.io) çalıştırmak için mevcut bir bash betiği bir Linux sanal makinesine (VM) veya sanal makine ölçek kümeleri (VMSS) zaman azure'da sağlama sırasında. Kaynakları Azure tarafından sağlanan sonra ilk önyüklemede bu cloud-init betikleri çalıştırın. Cloud-init yerel olarak desteklenen Linux dağıtımları ve Azure ile işleyişi hakkında daha fazla bilgi için bkz. [cloud-init genel bakış](using-cloud-init.md)

## <a name="run-a-bash-script-with-cloud-init"></a>Cloud-init ile bir bash betiğini çalıştırma
Bulut-Bulut yapılandırmasını mevcut betiklerinizi dönüştürme gerekmez init ile cloud-init biri olan bir bash betiği, birden çok giriş türleri kabul eder.

Komut dosyalarınızı çalıştırmak için Linux özel betik Azure uzantısı kullanıyorsanız, bunları cloud-init kullanma geçirebilirsiniz. Ancak, Azure uzantıları raporlama kod hataları uyarı olarak tümleştirdik, betik başarısız olursa bir cloud-init yansıma dağıtımının başarısız olmasına neden olmaz.

Bu işlevi iş başında görmek için test etmek için basit bir bash komut dosyası oluşturun. Cloud-init gibi `#cloud-config` dosya, bu komut dosyası olması gerekir, sanal makinenize kaynak sağlamak için Azureclı komutları çalışacağı için yerel.  Bu örnekte, dosyayı yerel makinenizde değil Cloud shell'de oluşturun. İstediğiniz düzenleyiciyi kullanabilirsiniz. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor simple_bash.sh` adını girin. Kullanılacak #1 seçin **nano** Düzenleyici. Tüm cloud-init dosyası doğru bir şekilde kopyalandığından emin olun başta birinci satır.  

```bash
#!/bin/sh
echo "this has been written via cloud-init" + $(date) >> /tmp/myScript.txt
```

Bu görüntü dağıtmadan önce bir kaynak grubu oluşturmak için ihtiyacınız [az grubu oluşturma](/cli/azure/group) komutu. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Şimdi bir VM oluşturun [az vm oluşturma](/cli/azure/vm) ve bash komut dosyasıyla belirtin `--custom-data simple_bash.sh` gibi:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data simple_bash.sh \
  --generate-ssh-keys 
```
## <a name="verify-bash-script-has-run"></a>Bash betiği çalıştırıldığını doğrulayın
SSH için yukarıdaki komut çıktısında gösterilen sanal makinenizin genel IP adresi. Kendi girin **Publicıpaddress** gibi:

```bash
ssh <publicIpAddress>
```

Değiştirin **/tmp** dizin ve myScript.txt osyanın var ve içindeki uygun metnine sahip olun.  Kullanmıyorsa, denetleyebilirsiniz **/var/log/cloud-init.log** daha fazla ayrıntı için.  Şu girişi için arama yapın:

```bash
Running config-scripts-user using lock Running command ['/var/lib/cloud/instance/scripts/part-001']
```

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişiklikleri ek cloud-init örnekleri için aşağıdakilere bakın:
 
- [Bir VM'ye ek Linux kullanıcı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede mevcut paketlerini güncelleştirmek için bir paket Yöneticisi'ni çalıştırın](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirin](cloudinit-update-vm-hostname.md) 
- [Bir uygulama paketi yükleme, yapılandırma dosyasını güncelleyin ve anahtarları ekleme](tutorial-automate-vm-deployment.md)
