---
title: "Linux sanal makineleri azure'daki bulut init desteği'ne genel bakış | Microsoft Docs"
description: "Microsoft Azure bulut init özelliklerine genel bakış"
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: 88133aff36aaef544d555cb121e23ff23fcc3367
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2018
---
# <a name="cloud-init-support-for-virtual-machines-in-azure"></a>Azure'da sanal makineler için bulut başlatma desteği
Bu makale için mevcut destek açıklanır [bulut init](https://cloudinit.readthedocs.io) bir sanal makine (VM) ya da sanal makineyi yapılandırmak için ölçek (VMSS) Azure zamanında sağlama sırasında ayarlar. Kaynakları Azure tarafından sağlanan sonra bu bulut başlatma komut dosyaları ilk önyükleme çalıştırın.  

## <a name="cloud-init-overview"></a>Cloud-init genel bakış
[Bulut init](https://cloudinit.readthedocs.io) ilk kez önyükleme gibi bir Linux VM özelleştirmek için yaygın olarak kullanılan bir yaklaşımdır. Bulut init paketleri yüklemek ve dosyaları yazma veya kullanıcılar ve güvenlik yapılandırmak için kullanabilirsiniz. Bulut init ilk önyükleme işlemi sırasında çağrıldığı için ek adımlar veya yapılandırmanızı uygulamak için gerekli aracıların yok.  Doğru biçim hakkında daha fazla bilgi için `#cloud-config` dosyaları görmek [bulut init belgeleri sitesi](http://cloudinit.readthedocs.io/en/latest/topics/format.html#cloud-config-data).  `#cloud-config`dosyaları base64 ile kodlanmış metin dosyalarıdır.

Bulut init dağıtımları üzerinde de çalışır. Örneğin, kullanmadığınız **get apt yükleme** veya **yum yükleme** bir paketi yüklemek için. Bunun yerine, yüklemek için paketlerin listesini tanımlayabilirsiniz. Bulut init otomatik olarak seçtiğiniz distro için yerel paket Yönetim Aracı'nı kullanır.

 Etkin olarak ile doğrulanan Linux distro ortaklarımızın Azure marketi'ndeki bulut init etkin görüntüleri kullanılabilir olması için çalışıyoruz. Bu görüntüler, bulut init dağıtımlarınızın yapar ve yapılandırmaları VM'ler ve VM ölçek kümeleri (VMSS) ile sorunsuz bir şekilde çalışabilirsiniz. Aşağıdaki tabloda Azure platformu geçerli bulut init etkin görüntüleri kullanılabilirliğine özetlenmektedir:

| Yayımcı | Sunduğu | SKU | Sürüm | Bulut init hazır
|:--- |:--- |:--- |:--- |:--- |:--- |
|Canonical |UbuntuServer |16.04 LTS |en son |evet | 
|Canonical |UbuntuServer |14.04.5-LTS |en son |evet |
|CoreOS |CoreOS |Dengeli |en son |evet |
|OpenLogic |CentOS |7 CI |en son |önizleme |
|RedHat |RHEL |7 HAM-CI |en son |önizleme |

## <a name="what-is-the-difference-between-cloud-init-and-the-linux-agent-wala"></a>Bulut Init ve Linux Aracısı'nı (WALA) arasındaki fark nedir?
WALA sağlamak ve sanal makineleri yapılandırmak ve Azure uzantılarını işlemek için kullanılan bir Azure platforma özgü aracısıdır. Biz, bulut init geçerli kendi bulut başlatma komut dosyaları kullanmak mevcut bulut init müşterileri izin vermek üzere yerine Linux Aracısı'nı kullanmak için sanal makineleri yapılandırma görevini geliştirme.  Linux sistemleri yapılandırmak için bulut init komut dosyalarında Yatırımlar varsa vardır **ek ayar gerekmiyor** bunları etkinleştirmek için. 

Azure CLI eklemezseniz `--custom-data` zaman, WALA sağlama sırasında anahtar minimum VM VM sağlamak ve Varsayılanları dağıtımla tamamlamak için gereken parametreleri sağlama alır.  Bulut Init başvuruyorsa `--custom-data` değiştirmek, her özel verilerinizi (ayrı ayrı ayarlar ya da tam komut dosyası) içerdiği WALA varsayılan ayarları geçersiz kılar. 

Sanal makineleri WALA yapılandırmalarını zaman sağlama maksimum VM içinden çalışması için zaman kısıtlı.  Bulut init yapılandırmaları Vm'lere uygulanan zaman kısıtlamaları yoksa ve bir dağıtım zaman aşımına uğramadan tarafından başarısız olmasına neden olmaz. 

## <a name="deploying-a-cloud-init-enabled-virtual-machine"></a>Sanal makine etkin bulut init dağıtma
Bir bulut init etkin sanal makine dağıtımı, dağıtımı sırasında bir bulut init etkin dağıtım başvuran olarak kadar basittir.  Linux dağıtım maintainers etkinleştirmek ve bunların temel Azure yayımlanan görüntülere bulut init tümleştirmek seçmeniz gerekir. Dağıtmak istediğiniz görüntünün bulut init etkin olduğunu doğruladıktan sonra görüntüyü dağıtmak için Azure CLI kullanabilirsiniz. 

Bu görüntü dağıtımı ilk adımı sahip bir kaynak grubu oluşturmaktır [az grubu oluşturma](/cli/azure/group#create) komutu. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
Sonraki adım, geçerli kabuğunda adlı bir dosya oluşturmaktır *bulut init.txt* ve aşağıdaki yapılandırma yapıştırın. Bu örnekte, yerel makinenizde olmayan bulut kabuğunda dosyası oluşturun. İstediğiniz herhangi bir düzenleyicisini kullanabilirsiniz. Girin `sensible-editor cloud-init.txt` dosyası oluşturun ve kullanılabilir düzenleyicileri listesini görmek için. # 1'ı kullanmayı seçin **nano** Düzenleyici. Tüm bulut init dosyanın doğru şekilde kopyalandığından emin olun özellikle ilk satırı:

```yaml
#cloud-config
package_upgrade: true
packages:
  -httpd
```
Basın `ctrl-X` dosya çıkmak için şunu yazın `y` basın ve dosyayı kaydetmek için `enter` çıkış dosyasının adına onaylamak için.

Son adım, bir VM oluşturmaktır [az vm oluşturma](/cli/azure/vm#az_vm_create) komutu. 

Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *centos74* ve zaten bir varsayılan anahtar konumda yoksa, SSH anahtarları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.  Kullanım `--custom-data` bulut init yapılandırma dosyanızda geçirmek için parametre. Tam yolunu belirtmeniz *bulut init.txt* mevcut çalışma dizininizi dışında dosyasını kaydettiyseniz yapılandırma. Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *centos74*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud-init.txt \
  --generate-ssh-keys 
```

Azure CLI VM oluşturduğunuz sırada bilgileri dağıtımınıza özgü gösterir. `publicIpAddress` değerini not edin. Bu adres, VM’ye erişmek için kullanılır.  Oluşturulacak VM, yüklemek için paketleri ve uygulamayı başlatmak için biraz zaman alabilir. Azure CLI sorusu döndükten sonra çalışmaya devam arka plan görevleri vardır. VM SSH olabilir ve bulut init günlükleri görüntülemek için sorun giderme bölümünde açıklanan adımları kullanın. 

## <a name="troubleshooting-cloud-init"></a>Bulut başlatma sorunlarını giderme
VM sağlandıktan sonra bulut init tüm modüllerin çalışacak ve tanımlı betik `--custom-data` VM yapılandırmak için.  Herhangi bir hata veya eksikliklerden yapılandırmasından gidermeniz gerekiyorsa, modül adı aramak gerekir (`disk_setup` veya `runcmd` örneğin) bulut init günlüğüne - bulunan **/var/log/cloud-init.log**.

> [!NOTE]
> Her modülü hatası önemli bir bulut init sonuçları genel yapılandırma hatası. Örneğin, kullanarak `runcmd` modülü, komut dosyası başarısız olursa, bulut init hala bildirir sağlama başarılı runcmd modülü yürütülen olduğundan.

Bulut init günlüğü daha fazla ayrıntı için başvurmak [bulut init belgeleri](http://cloudinit.readthedocs.io/en/latest/topics/logging.html) 

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişiklikleri bulut init örnekleri için aşağıdaki belgelere bakın:
 
- [Bir VM için ek Linux kullanıcı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede mevcut paketleri güncelleştirmek için bir paket Yöneticisi'ni çalıştırın](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirme](cloudinit-update-vm-hostname.md) 
- [Bir uygulama paketi yükleme, yapılandırma dosyalarını güncelleştirmek ve anahtarları Ekle](tutorial-automate-vm-deployment.md)
