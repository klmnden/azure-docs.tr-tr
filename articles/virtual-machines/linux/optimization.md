---
title: Linux VM Azure ile ilgili en iyi duruma getirme | Microsoft Docs
description: Azure ile ilgili en iyi performans için Linux VM oluşturdunuz emin olmak için bazı en iyi duruma getirme ipuçlarını öğrenin
keywords: Linux sanal makine, sanal makine linux ubuntu sanal makine
services: virtual-machines-linux
documentationcenter: ''
author: rickstercdn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 8baa30c8-d40e-41ac-93d0-74e96fe18d4c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: rclaus
ms.openlocfilehash: 75bba953a7a5737f0388e53a9f6f38dd8324eb83
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="optimize-your-linux-vm-on-azure"></a>Azure’da Linux VM’nizi iyileştirme
Linux sanal makine (VM) oluşturma komut satırından veya portalından yapmak kolaydır. Bu öğreticide, Microsoft Azure platformu üzerinde performansı iyileştirmek için ayarladığınız emin olmak nasıl gösterir. Bu konuda bir Ubuntu Server VM kullanır, ancak, Linux kullanarak sanal makine oluşturabilirsiniz [kendi görüntü şablonları olarak](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

## <a name="prerequisites"></a>Önkoşullar
Bu konu çalışan bir Azure aboneliği zaten sahip varsayar ([ücretsiz deneme sürenizde](https://azure.microsoft.com/pricing/free-trial/)) ve bir VM Azure aboneliğinizde zaten sağlanmış. En son sahip olduğunuzdan emin olun [Azure CLI 2.0](/cli/azure/install-az-cli2) yüklü ve Azure aboneliğinizle oturum açmış [az oturum açma](/cli/azure/reference-index#az_login) , önce [bir VM oluşturma](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="azure-os-disk"></a>Azure işletim sistemi diski
Azure'da bir Linux VM oluşturduktan sonra kendisiyle ilişkilendirilmiş iki disk var. **/ dev/sda** , işletim sistemi disk **/dev/sdb** geçici disktir.  Ana işletim sistemi diski kullanmayın (**/dev/sda**) olarak işletim sistemi dışındaki her şey için optimize edilen için hızlı VM önyükleme süresi ve iş yükleriniz için iyi bir performans sağlamaz. Kalıcı almak için VM için bir veya daha fazla disk eklemek için kullanmak istediğiniz ve depolama verileriniz için en iyi duruma getirilmiş. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Disk boyutu ve performans hedefleri için ekleme
VM boyutuna göre en fazla 16 ek A Series, D-serisi 32 disklerde disklerde iliştirebilirsiniz ve G serisi 64 disklerde makine - boyutu 1 TB'ye kadar her. Ek disk alanı ve IOPS gereksinimlerini gerektiği gibi ekleyin. Her diskin 500 IOPS performans hedefinin Premium Storage için standart depolama ve disk başına 5000 Iops'yi kadar vardır.  Premium depolama diskler hakkında daha fazla bilgi için bkz: [Premium Storage: Azure VM'ler için yüksek performanslı depolama](../windows/premium-storage.md)

Burada kendi önbellek ayarlarını ayarlanmış olarak Premium depolama disklerde yüksek IOPS elde etmek için **ReadOnly** veya **hiçbiri**, devre dışı bırakmalısınız **engelleri** bağlama sırasında Linux dosya sisteminde. Premium yedeklenen depolama diskleri yazma işlemleri için bu önbellek ayarlarını dayanıklı olduğundan engelleri gerekmez.

* Kullanırsanız **reiserFS**, bağlama seçeneği kullanılarak devre dışı bırak engelleri `barrier=none` (engelleri etkinleştirmek için kullanın `barrier=flush`)
* Kullanırsanız **ext3/ext4**, bağlama seçeneği kullanılarak devre dışı bırak engelleri `barrier=0` (engelleri etkinleştirmek için kullanın `barrier=1`)
* Kullanırsanız **XFS**, bağlama seçeneği kullanılarak devre dışı bırak engelleri `nobarrier` (engelleri etkinleştirmek için seçeneği kullanın `barrier`)

## <a name="unmanaged-storage-account-considerations"></a>Yönetilmeyen depolama hesabında dikkate alınacak noktalar
Bir VM ile Azure CLI 2.0 oluşturduğunuzda, varsayılan eylem Azure yönetilen diskleri kullanmaktır.  Bu diskleri Azure platformu tarafından işlenir ve hazırlık veya konum depolamaya gerektirmez.  Yönetilmeyen diskler bir depolama hesabı gerektirir ve bazı ek performans konuları vardır.  Yönetilen diskler hakkında daha fazla bilgi için bkz. [Azure Yönetilen Disklere genel bakış](../windows/managed-disks-overview.md).  Yalnızca yönetilmeyen diskleri kullandığınızda aşağıdaki bölümde performans özetlemektedir.  Yeniden, varsayılan ve önerilen bir depolama çözümü, yönetilen diskleri kullanmaktır.

Yönetilmeyen disklerle VM oluşturursanız, yakınında olun ve ağ gecikmesini en aza indirmek için VM ile aynı bölgede bulunan depolama hesaplarından diskleri bağladığınızdan emin olun.  Her standart depolama hesabının en fazla 20 sahip k IOPS ve 500 TB boyutu kapasitesi.  İşletim sistemi diski ve oluşturduğunuz herhangi bir veri diski dahil olmak üzere 40 yoğun olarak kullanılan diskler için bu sınır üretir. Premium depolama hesapları için maksimum IOPS sınırı yoktur ancak 32 TB boyut sınırı yoktur. 

Yüksek IOPS iş yükleri ve postalarla seçtiniz, standart depolama diskleriniz için standart depolama hesapları için 20.000 IOPS sınırına ulaşıp değil emin olmak için birden çok depolama hesapları arasında diskleri bölme gerekebilir. VM'yi farklı depolama hesapları ve en iyi yapılandırmanızı elde etmek için depolama hesabı türleri arasında disklerden bir karışımını içerebilir.
 

## <a name="your-vm-temporary-drive"></a>VM geçici sürücünüze
Bir VM oluşturduğunuzda varsayılan olarak Azure, bir işletim sistemi diski ile sağlar (**/dev/sda**) ve geçici bir disk (**/dev/sdb**).  Eklediğiniz Göster yukarı olarak tüm ek diskleri **/dev/sdc**, **/dev/sdd**, **/dev/sde** ve benzeri. Geçici diskteki tüm verileri (**/dev/sdb**) sağlam değildir ve Bakım VM'yi yeniden başlatılmasını zorlar veya belirli olaylar gibi VM yeniden boyutlandırma, yeniden dağıtım, kaybolmuş olabilir.  Geçici disk türü ve boyutu ilgili dağıtım sırasında seçtiğiniz VM boyutu. Premium tümünün boyutu en fazla 48 k ek performans için yerel bir SSD tarafından desteklenen geçici sürücü VM'ler (DS, G ve DS_V2 serisi) IOPS. 

## <a name="linux-swap-file"></a>Linux takas dosyası
Azure VM Ubuntu veya CoreOS görüntüden ise, bir bulut-config bulut başlatma göndermek için CustomData kullanabilirsiniz. Varsa, [özel Linux görüntü karşıya](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bulut init kullanan, aynı zamanda bulut init kullanarak takas bölümleri yapılandırın.

Ubuntu bulut görüntülerinde takas bölümü yapılandırmak için bulut init kullanmanız gerekir. Daha fazla bilgi için bkz: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

Bulut init desteği olmadan görüntüler için Azure Marketi'nden dağıtılan VM görüntüleri bir VM Linux işletim sistemiyle tümleştirilmiş aracısına sahip. Bu aracı VM'nin çeşitli Azure Hizmetleri ile etkileşimde bulunmasına izin veren. Azure Marketi'nden standart bir görüntü dağıttığınız varsayıldığında, doğru Linux takas dosyası ayarlarınızı yapılandırmak için aşağıdakileri yapmanız gerekir:

Bulun ve iki giriş değiştirme **/etc/waagent.conf** dosya. Bunlar, ayrılmış takas dosyası varlığını ve takas dosyası boyutunu denetler. Değiştirilecek arayan parametreleri `ResourceDisk.EnableSwap=N` ve `ResourceDisk.SwapSizeMB=0` 

Parametreleri aşağıdaki ayarları değiştirin:

* ResourceDisk.EnableSwap=Y
* ResourceDisk.SwapSizeMB={size gereksinimlerinizi karşılayacak şekilde MB} 

Değişikliği yaptıktan sonra waagent yeniden başlatın veya bu değişiklikleri yansıtacak şekilde, Linux VM yeniden başlatma gerekir.  Değişiklikler uygulanmamış olabilir ve kullandığınız takas dosyası oluşturulan bildiğiniz `free` boş alan görüntülemek için komutu. Aşağıdaki örnek değiştirme sonucu olarak oluşturulan 512 MB takas dosyası sahip **waagent.conf** dosyası:

```bash
azuseruser@myVM:~$ free
            total       used       free     shared    buffers     cached
Mem:       3525156     804168    2720988        408       8428     633192
-/+ buffers/cache:     162548    3362608
Swap:       524284          0     524284
```

## <a name="io-scheduling-algorithm-for-premium-storage"></a>Premium depolama g/ç zamanlama algoritması
2.6.18 ile Linux çekirdek, algoritma zamanlama g/ç CFQ (tamamen Orta Sıralama algoritması) son değiştirildiği varsayılan. Rasgele erişim g/ç modelleri için CFQ ve son tarih arasındaki performans farklar düşünülerek fark yoktur.  SSD tabanlı diskler disk g/ç düzeni daha sıralı olduğu için geri SEKMEYİ veya son algoritmasına geçme daha iyi g/ç performansı elde edebilirsiniz.

### <a name="view-the-current-io-scheduler"></a>Geçerli g/ç Zamanlayıcı görüntüleyin
Aşağıdaki komutu kullanın:  

```bash
cat /sys/block/sda/queue/scheduler
```

Geçerli Zamanlayıcı gösteren çıktı bakın.  

```bash
noop [deadline] cfq
```

### <a name="change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Algoritma zamanlama g/ç geçerli cihaz (/ dev/sda) değiştirme
Aşağıdaki komutları kullanın:  

```bash
azureuser@myVM:~$ sudo su -
root@myVM:~# echo "noop" >/sys/block/sda/queue/scheduler
root@myVM:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
root@myVM:~# update-grub
```

> [!NOTE]
> Bu ayar için uygulama **/dev/sda** tek başına kullanışlı değildir. Tüm veri disklerde burada sıralı g/ç g/ç düzeni dominates ayarlayın.  

Gösteren aşağıdaki çıktı görmeniz gerekir **grub.cfg** başarıyla yeniden oluşturuldu ve varsayılan Zamanlayıcı SEKMEYİ için güncelleştirilmiştir.  

```bash
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-3.13.0-34-generic
Found initrd image: /boot/initrd.img-3.13.0-34-generic
Found linux image: /boot/vmlinuz-3.13.0-32-generic
Found initrd image: /boot/initrd.img-3.13.0-32-generic
Found memtest86+ image: /memtest86+.elf
Found memtest86+ image: /memtest86+.bin
done
```

Redhat dağıtım ailesi için aşağıdaki komut yalnızca gerekir:   

```bash
echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local
```

## <a name="using-software-raid-to-achieve-higher-iops"></a>Yazılım RAID elde etmek için daha yüksek g kullanarak / Ops
İş yüklerinizi tek bir disk sunabileceğinden daha fazla IOPS gerektiriyorsa, birden çok disk, yazılım RAID yapılandırmasını kullanmak gerekir. Azure disk dayanıklılık yerel doku katmanında gerçekleştirdiğinden, yüksek düzeyde bir RAID-0 şeritleme yapılandırmadan performans elde edersiniz.  Sağlamak ve Azure ortamında diskleri oluşturun ve bunları bölümlendirme, biçimlendirme ve sürücüleri takma önce Linux VM'ye ekleyin.  Yazılım RAID kurulumu, Linux VM'de Azure yapılandırma hakkında daha fazla ayrıntı bulunabilir **[yapılandırma yazılım RAID Linux'ta](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)** belge.

## <a name="next-steps"></a>Sonraki Adımlar
Önce ve değişiklik olan etkisini ölçmek için her bir değişiklikten sonra testleri gerçekleştirmek gereken tüm en iyi duruma getirme tartışmalarını unutmayın.  En iyi duruma getirme, ortamınızdaki farklı makinelerde farklı sonuçlar olan bir adım adım işlemidir.  Bir yapılandırma ile ilgili ne çalışacağı başkaları için çalışmayabilir.

Ek kaynaklar için bazı yararlı bağlantılar: 

* [Premium Depolama: Azure Sanal Makine İş Yükleri için Yüksek Performanslı Depolama](premium-storage.md)
* [Azure Linux Aracısı Kullanıcı Kılavuzu](../extensions/agent-linux.md)
* [Azure Linux VM'ler MySQL performansı en iyi duruma getirme](classic/optimize-mysql.md)
* [Yazılım RAID Linux üzerinde yapılandırma](configure-raid.md)
