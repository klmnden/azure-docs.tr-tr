---
title: Cloud-init ile kullanmak için Azure VM görüntüsü hazırlama | Microsoft Docs
description: Cloud-init ile dağıtımı için önceden var olan bir Azure VM görüntüsü hazırlama
services: virtual-machines-linux
documentationcenter: ''
author: danis
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 06/24/2019
ms.author: danis
ms.openlocfilehash: 1f9f6042b52c722280a8227754960ffb270e94b8
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67668258"
---
# <a name="prepare-an-existing-linux-azure-vm-image-for-use-with-cloud-init"></a>Cloud-init ile kullanım için var olan bir Linux Azure VM görüntüsü hazırlama
Bu makalede, mevcut bir Azure sanal makinesini atın ve bilgisayarına ve cloud-init kullanıma hazır olması için hazırlamak üzere nasıl gösterir. Elde edilen görüntü, yeni bir sanal makine veya sanal makine ölçek kümeleri - bunların daha sonra daha fazla cloud-init tarafından dağıtım sırasında garantileyen dağıtmak için kullanılabilir.  Kaynakları Azure tarafından sağlanan sonra ilk önyüklemede bu cloud-init betikleri çalıştırın. Cloud-init yerel olarak desteklenen Linux dağıtımları ve Azure ile işleyişi hakkında daha fazla bilgi için bkz. [cloud-init genel bakış](using-cloud-init.md)

## <a name="prerequisites"></a>Önkoşullar
Bu belge Linux işletim sisteminin desteklenen bir sürümünü çalıştıran bir çalışan Azure sanal makine zaten sahip olduğunuz varsayılır. Makine, gereksinimlerinize uyacak şekilde zaten yapılandırılmış tüm gerekli modülleri yüklü, tüm gerekli güncelleştirmelerin işlenen ve gereksinimlerinizi karşıladığından emin olmak için test. 

## <a name="preparing-rhel-76--centos-76"></a>RHEL 7.6 hazırlama / CentOS 7.6
Cloud-init yüklemek için aşağıdaki komutları çalıştırın ve Linux VM'NİZDE SSH gerekir.

```bash
sudo yum makecache fast
sudo yum install -y gdisk cloud-utils-growpart
sudo yum install - y cloud-init 
```

Güncelleştirme `cloud_init_modules` konusundaki `/etc/cloud/cloud.cfg` aşağıdaki olan modülleri eklemek için:
```bash
- disk_setup
- mounts
```

Hangi genel amaçlı bir örnek aşağıdadır `cloud_init_modules` gibi görünen bölümü.
```bash
cloud_init_modules:
 - migrator
 - bootcmd
 - write-files
 - growpart
 - resizefs
 - disk_setup
 - mounts
 - set_hostname
 - update_hostname
 - update_etc_hosts
 - rsyslog
 - users-groups
 - ssh
```
Çok sayıda görev sağlama ve geçici diskler işleme ile ilgili olarak güncelleştirilmesi gereken `/etc/waagent.conf`. Uygun ayarları güncelleştirmek için aşağıdaki komutları çalıştırın. 
```bash
sed -i 's/Provisioning.Enabled=y/Provisioning.Enabled=n/g' /etc/waagent.conf
sed -i 's/Provisioning.UseCloudInit=n/Provisioning.UseCloudInit=y/g' /etc/waagent.conf
sed -i 's/ResourceDisk.Format=y/ResourceDisk.Format=n/g' /etc/waagent.conf
sed -i 's/ResourceDisk.EnableSwap=y/ResourceDisk.EnableSwap=n/g' /etc/waagent.conf
cloud-init clean
```

Yalnızca Azure, yeni bir dosya oluşturarak Azure Linux aracısı için bir veri kaynağı izin `/etc/cloud/cloud.cfg.d/91-azure_datasource.cfg` aşağıdaki satırla bir düzenleyiciyi kullanarak:

```bash
# Azure Data Source config
datasource_list: [ Azure ]
```

Yapılandırılmış bir takas dosyası, mevcut Azure görüntü varsa ve cloud-init kullanarak yeni görüntüleri takas dosyası yapılandırmasını değiştirmek istediğinizde, mevcut takas dosyası'nı kaldırmanız gerekir.

Red Hat görüntülerini - tabanlı için belge aşağıdaki Red Hat açıklayan içindeki yönergeleri izleyin nasıl [takas dosyası kaldırma](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/storage_administration_guide/swap-removing-file).

Etkin takas dosyası ile CentOS görüntüleri için swapfile etkinleştirmek için aşağıdaki komutu çalıştırabilirsiniz:
```bash
sudo swapoff /mnt/resource/swapfile
```

Takas dosyası başvurusu kaldırılır olun `/etc/fstab` -aşağıdaki çıktı gibi görünmelidir:
```text
# /etc/fstab
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
UUID=99cf66df-2fef-4aad-b226-382883643a1c / xfs defaults 0 0
UUID=7c473048-a4e7-4908-bad3-a9be22e9d37d /boot xfs defaults 0 0
```

Alandan kazanmak ve takas dosyası kaldırmak için aşağıdaki komutu çalıştırabilirsiniz:
```bash
rm /mnt/resource/swapfile
```
## <a name="extra-step-for-cloud-init-prepared-image"></a>Görüntü ek adım cloud-init için hazır
> [!NOTE]
> Görüntünüzü daha önce ise bir **cloud-init** hazırlanmış ve yapılandırılan görüntü gereken aşağıdaki adımları uygulayın.

Aşağıdaki üç komutları yalnızca yeni bir özel kaynak görüntü olacak şekilde özelleştirme VM, daha önce cloud-init tarafından sağlanan kullanılır.  Görüntünüzü Azure Linux Aracısı kullanılarak yapılandırıldıysa, bunları çalıştırmak gerekmez.

```bash
sudo cloud-init clean --logs
sudo waagent -deprovision+user -force
```

## <a name="finalizing-linux-agent-setting"></a>Linux aracısı sonlandırılıyor ayarı 
Tüm Azure platform görüntüleri, cloud-init tarafından veya yapılandırıldıysa, bakılmaksızın yüklü Azure Linux Aracısı var.  Linux makinesi kullanıcı sağlamayı tamamlamak için aşağıdaki komutu çalıştırın. 

```bash
sudo waagent -deprovision+user -force
```

Azure Linux Aracısı sağlamayı kaldırma komutları hakkında daha fazla bilgi için bkz. [Azure Linux Aracısı](../extensions/agent-linux.md) daha fazla ayrıntı için.

SSH oturumundan çıkın ve serbest bırakın, generalize ve yeni bir Azure VM görüntüsü oluşturmak için aşağıdaki Azureclı komutları, bash kabuğundan çalıştırın.  Değiştirin `myResourceGroup` ve `sourceVmName` , sourceVM yansıtan uygun bilgi ile.

```bash
az vm deallocate --resource-group myResourceGroup --name sourceVmName
az vm generalize --resource-group myResourceGroup --name sourceVmName
az image create --resource-group myResourceGroup --name myCloudInitImage --source sourceVmName
```

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişiklikleri ek cloud-init örnekleri için aşağıdakilere bakın:
 
- [Bir VM'ye ek Linux kullanıcı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede mevcut paketlerini güncelleştirmek için bir paket Yöneticisi'ni çalıştırın](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirin](cloudinit-update-vm-hostname.md) 
- [Bir uygulama paketi yükleme, yapılandırma dosyasını güncelleyin ve anahtarları ekleme](tutorial-automate-vm-deployment.md)
