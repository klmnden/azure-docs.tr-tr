---
title: Azure'da Linux VM'nizi iyileştirme | Microsoft Docs
description: Linux VM'niz Azure ile ilgili en iyi performans için ayarlamış olduğunuz emin olmak için bazı en iyi duruma getirme ipuçlarını öğrenin
keywords: Linux sanal makinesi, sanal makine linux ubuntu sanal makinesi
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
ms.subservice: disks
ms.openlocfilehash: 30d153863a20dcdddc702ee5a37c34a2938d7446
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61473918"
---
# <a name="optimize-your-linux-vm-on-azure"></a>Azure’da Linux VM’nizi iyileştirme
Bir Linux sanal makinesini (VM) oluşturma, komut satırından veya portalından yapmak kolaydır. Bu öğreticide, Microsoft Azure platformunda performansı iyileştirmek için ayarlamış olduğunuz emin olmak nasıl gösterir. Bu konuda bir Ubuntu Server VM kullanır, ancak Linux kullanarak sanal makine oluşturabilirsiniz [şablonları olarak kendi görüntülerinizi](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

## <a name="prerequisites"></a>Önkoşullar
Bu konu, çalışan bir Azure aboneliği zaten sahip olduğunuzu varsayar ([ücretsiz denemeye kaydolmayla](https://azure.microsoft.com/pricing/free-trial/)) ve Azure Aboneliğinize bir VM zaten sağladınız. En son sahip olduğunuzdan emin olun [Azure CLI](/cli/azure/install-az-cli2) yüklü ve Azure aboneliğinizde oturum [az login](/cli/azure/reference-index) , önce [VM oluşturma](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="azure-os-disk"></a>Azure işletim sistemi diski
Azure'da bir Linux VM oluşturduktan sonra onunla ilişkili iki disk var. **/ dev/sda** , işletim sistemi diski **/dev/sdb** , geçici disk.  Ana işletim sistemi diski kullanmayın ( **/dev/sda**) olarak işletim sistemi dışındaki her şey için optimize için hızlı VM önyükleme saati ve iş yükleriniz için iyi bir performans sağlamaz. VM'nize kalıcı almak için bir veya daha fazla disk eklemek istediğiniz ve depolama için verilerinizi en iyi duruma getirilmiş. 

## <a name="adding-disks-for-size-and-performance-targets"></a>Boyut ve performans hedefleri için disk ekleme
VM boyutuna göre A serisi, D serisi 32 disklerde en fazla 16 ek disklerde ekleyebilirsiniz ve G serisi 64 disklerde makine - boyutu 1 TB'ye kadar her. Ek disk IOPS gereksinimleri ve alan gerektiği gibi ekleyin. Her disk için Premium depolama standart depolama ve disk başına 5000 Iops'yi kadar 500 IOPS performans hedefinin sahiptir.

Burada kendi önbellek ayarlarını ayarlanan olarak Premium depolama diskleri üzerinde en yüksek IOPS elde etmek için **salt okunur** veya **hiçbiri**, devre dışı bırakmanız gerekir **engelleri** bağlama sırasında Linux'ta dosya sistemi. Desteklenen Premium depolama diskleri yazma işlemleri için bu önbellek ayarlarını dayanıklı olduğundan engelleri gerekmez.

* Kullanırsanız **reiserFS**, bağlama seçeneği kullanılarak devre dışı bırakma engelleri `barrier=none` (engellerini etkinleştirmek için kullanın `barrier=flush`)
* Kullanırsanız **ext3/ext4**, bağlama seçeneği kullanılarak devre dışı bırakma engelleri `barrier=0` (engellerini etkinleştirmek için kullanın `barrier=1`)
* Kullanırsanız **XFS**, bağlama seçeneği kullanılarak devre dışı bırakma engelleri `nobarrier` (engellerini etkinleştirmek için seçeneği kullanın `barrier`)

## <a name="unmanaged-storage-account-considerations"></a>Yönetilmeyen depolama hesabında dikkate alınacak noktalar
Azure CLI ile bir VM oluşturduğunuzda, varsayılan eylem Azure yönetilen diskler kullanmaktır.  Bu diskler Azure platformu tarafından işlenir ve depolanması için hazırlık veya konum gerektirmez.  Yönetilmeyen diskler bir depolama hesabı gerektirir ve bazı ek performansla ilgili önemli noktalar vardır.  Yönetilen diskler hakkında daha fazla bilgi için bkz. [Azure Yönetilen Disklere genel bakış](../windows/managed-disks-overview.md).  Aşağıdaki bölümde, yalnızca yönetilmeyen diskler kullandığınızda performansla ilgili önemli noktalar özetlenmektedir.  Yine, varsayılan ve önerilen depolama çözümü olan yönetilen diskleri kullanma.

Yönetilmeyen disklerle bir VM oluşturursanız, yakınında olun ve ağ gecikmesini en aza indirmek için VM ile aynı bölgede bulunan depolama hesaplarından diskleri bağladığınızdan emin olun.  Her standart depolama hesabı, en fazla 20 sahiptir IOPS k ve 500 TB boyut kapasite.  Bu sınır hem işletim sistemi diski hem de oluşturduğunuz herhangi bir veri diskleri dahil olmak üzere yaklaşık 40 yoğun olarak kullanılan diskler için üretir. Premium depolama hesapları için maksimum IOPS sınırı yoktur ancak 32 TB boyut sınırı yoktur. 

Diskleriniz için standart depolama yüksek IOPS kullanan iş yüklerinin ve uğraşmanızı seçtiniz zaman, disklerin 20.000 IOPS sınırı standart depolama hesapları için isabet değil emin olmak için birden çok depolama hesapları arasında bölmek gerekebilir. Sanal makinenize farklı depolama hesapları ve en iyi yapılandırmanızı elde etmek için depolama hesabı türleri arasında disklerden bir karışımını içerebilir.
 

## <a name="your-vm-temporary-drive"></a>Sanal makine geçici drive'ınızdaki
Bir VM oluşturduğunuzda varsayılan olarak Azure, bir işletim sistemi diski ile sağlar ( **/dev/sda**) ve geçici bir diskle ( **/dev/sdb**).  Tüm ek diskler, ekleme Göster olarak **/dev/sdc**, **/dev/sdd**, **/dev/sde** ve benzeri. Geçici diskteki tüm verilerin ( **/dev/sdb**) kalıcı değildir ve Bakım, sanal Makinenizin yeniden başlatılmasını zorlar veya belirli olayları gibi VM yeniden boyutlandırma, yeniden dağıtım, kayıp olabilir.  Geçici, disk türü ve boyutu, dağıtım sırasında seçtiğiniz VM boyutuna ilgilidir. Geçici sürücü en fazla 48 k ek performans için bir yerel SSD destekli Vm'leri (DS, G ve DS_V2 serisi) tüm premium boyut IOPS. 

## <a name="linux-swap-file"></a>Linux takas dosyası
Azure VM, Ubuntu veya CoreOS görüntüden ise, bir bulut yapılandırma için cloud-init göndermek için CustomData kullanabilirsiniz. Varsa, [özel bir Linux görüntüsü karşıya](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) cloud-init kullanan, cloud-init kullanarak takas bölümleri de yapılandırın.

Ubuntu görüntülerinde Cloud takas bölümü yapılandırmak için cloud-init kullanmanız gerekir. Daha fazla bilgi için [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

Cloud-init desteği olmadan görüntüler için VM görüntüleri Azure Marketi'nden dağıtılan işletim sistemiyle tümleştirilmiş bir VM Linux Aracısı var. Bu aracı, çeşitli Azure Hizmetleri ile etkileşim kurmak VM sağlar. Standart bir Azure Market görüntüsünden dağıttığınız varsayıldığında, doğru bir şekilde Linux takas dosyası ayarlarınızı yapılandırmak için aşağıdakileri yapmanız gerekir:

Bulun ve iki giriş değiştirme **/etc/waagent.conf** dosya. Bunlar, bir adanmış takas dosyanın varlığını ve takas dosyası boyutunu denetler. Değiştirilecek aradığınız parametreler `ResourceDisk.EnableSwap=N` ve `ResourceDisk.SwapSizeMB=0` 

Parametreler için aşağıdaki ayarları değiştirin:

* ResourceDisk.EnableSwap=Y
* İhtiyaçlarınızı karşılamak için MB cinsinden ResourceDisk.SwapSizeMB={size} 

Değişikliği yaptıktan sonra waagent yeniden başlatabilir veya bu değişiklikleri yansıtacak şekilde Linux VM yeniden başlatma gerekir.  Değişiklikleri uygulanmıştır ve kullandığınızda takas dosyası oluşturuldu bildiğiniz `free` boş alan görmek için komutu. Aşağıdaki örnek değiştirme sonucu olarak oluşturulan 512 MB takas dosyası sahip **waagent.conf** dosyası:

```bash
azuseruser@myVM:~$ free
            total       used       free     shared    buffers     cached
Mem:       3525156     804168    2720988        408       8428     633192
-/+ buffers/cache:     162548    3362608
Swap:       524284          0     524284
```

## <a name="io-scheduling-algorithm-for-premium-storage"></a>Premium depolama g/ç zamanlama algoritması
2\.6.18 ile Linux çekirdek algoritması zamanlama g/ç son değiştirildiği CFQ (tamamen adil Sıralama algoritması) için varsayılan. Rastgele erişim g/ç modelleri için göz ardı edilebilir performans farklarını CFQ ve son tarih arasındaki farkı yoktur.  SSD tabanlı diskler disk g/ç deseni genellikle sıralı olduğu için geri NOOP veya son algoritmaya geçiş daha iyi g/ç performansı elde edebilirsiniz.

### <a name="view-the-current-io-scheduler"></a>Geçerli g/ç Zamanlayıcısını görüntüleyin
Aşağıdaki komutu kullanın:  

```bash
cat /sys/block/sda/queue/scheduler
```

Geçerli Zamanlayıcı gösteren çıktı görürsünüz.  

```bash
noop [deadline] cfq
```

### <a name="change-the-current-device-devsda-of-io-scheduling-algorithm"></a>Geçerli cihaz algoritması zamanlama g/ç (/ dev/sda) değiştirme
Aşağıdaki komutları kullanın:  

```bash
azureuser@myVM:~$ sudo su -
root@myVM:~# echo "noop" >/sys/block/sda/queue/scheduler
root@myVM:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
root@myVM:~# update-grub
```

> [!NOTE]
> Bu ayar için uygulama **/dev/sda** tek başına kullanışlı değildir. Tüm veri disklerinde, sıralı g/ç g/ç deseni burada kaplamaktadır ayarlayın.  

Gösteren aşağıdaki çıktıyı görmelisiniz **grub.cfg** başarıyla yeniden oluşturuldu ve varsayılan Zamanlayıcı için NOOP güncelleştirildi.  

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

Red Hat dağıtım ailesi için aşağıdaki komut yalnızca gerekir:   

```bash
echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local
```

## <a name="using-software-raid-to-achieve-higher-iops"></a>Yazılım RAID elde etmek için daha yüksek miyim kullanarak / Ops
İş yüklerinizi tek bir diske sunabileceğinden daha yüksek IOPS gerekiyorsa, bir yazılım RAID yapılandırma birden çok disk kullanmanız gerekir. Azure disk dayanıklılık yerel fabric katmanında gerçekleştirdiğinden, en üst düzey bir RAID-0 şeritleme yapılandırmasından performans elde edin.  Sağlama ve diskler Azure ortamında oluşturmak ve bunları bölümleme, biçimlendirme ve sürücüleri bağlama önce Linux VM'nize ekleyebilirsiniz.  Azure'da Linux vm'nize yazılım RAID Kurulumu yapılandırma hakkında ayrıntılı bilgi bulunabilir **[Linux'ta yazılım RAID yapılandırma](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)** belge.

## <a name="next-steps"></a>Sonraki Adımlar
Önce ve değişikliğin etkisini ölçmek için her değişiklikten sonra testleri gerçekleştirmek gereken tüm en iyi duruma getirme tartışmaların unutmayın.  En iyi duruma getirme farklı sonuçlar ortamınızdaki farklı makineler arasında olan bir adım adım işlemidir.  Bir yapılandırma için neyin işe yaradığını başkaları için çalışmayabilir.

Ek kaynaklar için yararlı olabilecek bağlantıları:

* [Azure Linux Aracısı Kullanım Kılavuzu](../extensions/agent-linux.md)
* [Azure Linux vm'lerde MySQL performansını iyileştirme](classic/optimize-mysql.md)
* [Linux'ta yazılım RAID yapılandırma](configure-raid.md)
