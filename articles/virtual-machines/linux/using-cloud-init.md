---
title: Azure'da Linux sanal makineleri için cloud-init desteğine genel bakış | Microsoft Docs
description: Microsoft azure'da cloud-init özelliklerine genel bakış
services: virtual-machines-linux
documentationcenter: ''
author: rickstercdn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: 6dd1dd0ce2395e2b06d80385ffd299835a280526
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60614036"
---
# <a name="cloud-init-support-for-virtual-machines-in-azure"></a>Azure'da sanal makineler için cloud-init desteği
Bu makale için mevcut destek açıklar [cloud-init](https://cloudinit.readthedocs.io) bir sanal makine (VM) ya da sanal makineyi yapılandırmak için ölçek kümeleri (VMSS zaman azure'da sağlama sırasında). Kaynakları Azure tarafından sağlanan sonra ilk önyüklemede bu cloud-init betikleri çalıştırın.  

## <a name="cloud-init-overview"></a>Cloud-init genel bakış
[Cloud-init](https://cloudinit.readthedocs.io), Linux VM’sini ilk kez önyüklendiğinde özelleştirmeyi sağlayan, sık kullanılan bir yaklaşımdır. cloud-init’i paket yükleme, dosyalara yazma ve kullanıcılar ile güvenliği yapılandırma işlemleri için kullanabilirsiniz. Cloud-init önyükleme işlemi sırasında çağrıldığı için ek adımlar veya yapılandırmanıza uygulayabileceğiniz gerekli aracı yoktur.  Düzgün bir şekilde biçimlendirme hakkında daha fazla bilgi için `#cloud-config` dosyaları görmek [cloud-init belgeler sitesinde](https://cloudinit.readthedocs.io/en/latest/topics/format.html#cloud-config-data).  `#cloud-config` dosyaları, base64 ile kodlanmış metin dosyalarıdır.

Cloud-init, dağıtımlar arasında da çalışır. Örneğin, bir paket yüklemek için **apt-get install** veya **yum install** kullanmazsınız. Bunun yerine, yüklenecek paketlerin listesini tanımlayabilirsiniz. Cloud-init, seçtiğiniz dağıtım için yerel paket yönetim aracını otomatik olarak kullanır.

 Etkin olarak desteklenen Linux distro ortaklarımızla birlikte kullanılabilir cloud-init etkinleştirilmiş görüntüleri Azure Market'te sahip olmak için çalışıyoruz. Bu görüntüler, cloud-init dağıtımlarınızı yapar ve yapılandırmaları Vm'leri ve VM ölçek kümeleri (VMSS) ile sorunsuz bir şekilde çalışır. Aşağıdaki tabloda, Azure platformunda geçerli cloud-init etkinleştirilmiş görüntüleri kullanılabilirliği açıklanmaktadır:

| Yayımcı | Sunduğu | SKU | Sürüm | cloud-init hazır |
|:--- |:--- |:--- |:--- |:--- |
|Canonical |UbuntuServer |18.04-LTS |en son |evet | 
|Canonical |UbuntuServer |17.10 |en son |evet | 
|Canonical |UbuntuServer |16.04-LTS |en son |evet | 
|Canonical |UbuntuServer |14.04.5-LTS |en son |evet |
|CoreOS |CoreOS |Dengeli |en son |evet |
|OpenLogic |CentOS |7-CI |en son |önizleme |
|RedHat |RHEL |7-RAW-CI |en son |önizleme |

Şu anda Azure Stack RHEL 7.4 ve CentOS cloud-init kullanarak 7.4 sağlama desteklemez.

## <a name="what-is-the-difference-between-cloud-init-and-the-linux-agent-wala"></a>Cloud-init ve Linux Aracısı'nı (WALA) arasındaki fark nedir?
WALA sağlamak ve Vm'leri yapılandırma ve Azure uzantılarını işlemek için kullanılan bir Azure platforma özgü aracısıdır. Mevcut cloud-init müşteriler kendi geçerli cloud-init betikleri kullanmaya izin vermek üzere yerine Linux Aracısı cloud-init kullanmak için Vm'leri yapılandırma görevini güçlendirerek.  Linux sistemleri yapılandırma için cloud-init betiklerdeki mevcut Yatırımlar varsa, vardır **gerekli hiçbir ek ayar** bunları etkinleştirmek için. 

Azure CLI'yı eklemezseniz `--custom-data` zaman WALA sağlama sırasında anahtar en az VM VM sağlama ve dağıtım varsayılanları ile tamamlamak için gereken parametreleri sağlama alır.  Cloud-init başvuruyorsa `--custom-data` değiştirmek, her özel verilerinizi (ayrı ayrı ayarlar veya tam betik) içinde yer alan WALA varsayılan ayarları geçersiz kılar. 

VM'lerin WALA yapılandırmaları zaman sağlama maksimum VM içinde çalışacak şekilde zaman kısıtlı.  Cloud-init yapılandırma Vm'lere uygulanan zaman kısıtlamalarına sahip değildir ve bir dağıtım tarafından zaman aşımından başarısız olmasına neden olmaz. 

## <a name="deploying-a-cloud-init-enabled-virtual-machine"></a>Sanal makine etkin bir cloud-init dağıtma
Dağıtım sırasında bir cloud-init etkinleştirilmiş dağıtım başvuru olarak, cloud-init etkinleştirilmiş sanal makine dağıtımına kadar kolaydır.  Linux dağıtım maintainers etkinleştirin ve kullanıcıların temel Azure yayımlanan görüntülere cloud-init tümleştirmek seçmeniz gerekir. Dağıtmak istediğiniz görüntüyü cloud-init etkinleştirilmiş olduğunu doğruladıktan sonra görüntüyü dağıtmak için Azure CLI'yı kullanabilirsiniz. 

Bu görüntü dağıtımı ilk adımı, bir kaynak grubu oluşturmaktır [az grubu oluşturma](/cli/azure/group) komutu. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
Adlı geçerli kabuğunuzun içinde bir dosya oluşturmak için sonraki adımdır *cloud-init.txt* oluşturup aşağıdaki yapılandırmayı yapıştırın. Bu örnekte, dosyayı yerel makinenizde değil Cloud shell'de oluşturun. İstediğiniz düzenleyiciyi kullanabilirsiniz. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor cloud-init.txt` adını girin. Kullanılacak #1 seçin **nano** Düzenleyici. Başta birinci satır olmak üzere cloud-init dosyasının tamamının doğru bir şekilde kopyalandığından emin olun:

```yaml
#cloud-config
package_upgrade: true
packages:
  - httpd
```
Basın `ctrl-X` dosya çıkmak için şunu yazın `y` tuşuna basın ve dosyayı kaydetmek için `enter` çıkış dosya adını doğrulamak için.

Bir VM oluşturmak için son adımdır [az vm oluşturma](/cli/azure/vm) komutu. 

Aşağıdaki örnekte adlı bir VM oluşturur *centos74* ve bunlar zaten varsayılan anahtar konumunda yoksa SSH anahtarları oluşturulur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.  `--custom-data` parametresini kullanarak cloud-init yapılandırma dosyanızı geçirin. Dosyayı mevcut çalışma dizininizin dışına kaydettiyseniz *cloud-init.txt* yapılandırmasının tam yolunu belirtin. Aşağıdaki örnekte adlı bir VM oluşturur *centos74*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud-init.txt \
  --generate-ssh-keys 
```

VM oluşturulduğunda Azure CLI bilgi dağıtımınıza özgü gösterir. `publicIpAddress` değerini not edin. Bu adres, VM’ye erişmek için kullanılır.  Sanal Makinenin oluşturulması paketlerin yüklenmesi ve uygulamanın başlatılması için biraz zaman alabilir. Azure CLI sizi isteme geri döndürdükten sonra çalışmaya devam eden arka plan görevleri vardır. VM'ye SSH olabilir ve cloud-init günlükleri görüntülemek için sorun giderme bölümünde açıklanan adımları kullanın. 

## <a name="troubleshooting-cloud-init"></a>Cloud-init sorunlarını giderme
VM sağlandıktan, cloud-init ile tüm modüllerin çalışır ve betik tanımlanan sonra `--custom-data` VM'yi yapılandırmak için.  Herhangi bir hata veya eksikliklerden yapılandırmasından sorun gidermeniz gerekiyorsa, modül adı için arama yapmanız (`disk_setup` veya `runcmd` gibi) cloud-init günlüğünde - bulunan **/var/log/cloud-init.log**.

> [!NOTE]
> Her modülü hatası sonuçları içinde önemli bir cloud-init genel yapılandırma hatası. Örneğin, kullanarak `runcmd` modülü, komut başarısız olursa, cloud-init yine de rapor sağlama başarılı runcmd modülü yürütülen olduğundan.

Cloud-init günlüğü daha fazla ayrıntı için başvurmak [cloud-init belgeleri](https://cloudinit.readthedocs.io/en/latest/topics/logging.html) 

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişiklikleri cloud-init örnekleri için aşağıdaki belgelere bakın:
 
- [Bir VM'ye ek Linux kullanıcı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede mevcut paketlerini güncelleştirmek için bir paket Yöneticisi'ni çalıştırın](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirin](cloudinit-update-vm-hostname.md) 
- [Bir uygulama paketi yükleme, yapılandırma dosyasını güncelleyin ve anahtarları ekleme](tutorial-automate-vm-deployment.md)
