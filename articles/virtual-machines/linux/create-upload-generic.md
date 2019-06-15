---
title: Azure'da bir Linux VHD'si oluşturma ve yükleme
description: Oluşturma ve bir Azure sanal Linux işletim sistemi içeren sabit disk (VHD) yükleme öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: szarkos
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: d351396c-95a0-4092-b7bf-c6aae0bbd112
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: szark
ms.openlocfilehash: 1ef273b65bb3a8b8536d27c70e8ba05e74faa39b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64702478"
---
# <a name="information-for-non-endorsed-distributions"></a>Dağıtımlarla için bilgi
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Azure platformu, yalnızca bir Linux işletim sistemi çalıştıran sanal makineler için SLA'sı geçerlidir, [destekli dağıtımlar](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kullanılır. Bu desteklenen dağıtımlar için önceden yapılandırılmış Linux görüntüleri Azure Market'te sağlanır.

* [-Azure'da Linux destekli dağıtımlar](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Microsoft azure'da Linux görüntüleri için destek](https://support.microsoft.com/kb/2941892)

Azure üzerinde çalışan tüm dağıtımları birkaç önkoşul vardır. Bu makalede, her dağıtım farklı olduğu gibi kapsamlı olamaz. Aşağıdaki tüm ölçütleri karşılayan olsa bile, önemli ölçüde düzgün çalışması için Linux sisteminiz için ince gerekebilir.

Biri ile başlamanızı öneririz [Azure destekli dağıtımlarda Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Aşağıdaki makaleler Azure üzerinde desteklenen çeşitli desteklenen Linux dağıtımı hazırlama işlemini gösterir:

* **[CentOS tabanlı dağıtımlar](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

Bu makalede, Azure'da Linux dağıtımınız çalıştırmaya yönelik genel kılavuz odaklanır.

## <a name="general-linux-installation-notes"></a>Genel Linux yükleme notları
* Hyper-V sanal sabit disk (VHDX) biçimi yalnızca Azure'da desteklenmiyor *VHD'yi sabit*.  Disk Hyper-V Yöneticisi'ni kullanarak VHD biçimine dönüştürebilir veya [Convert-VHD](https://docs.microsoft.com/powershell/module/hyper-v/convert-vhd) cmdlet'i. VirtualBox kullanıyorsanız seçin **boyutu sabit** (dinamik olarak ayrılan) varsayılan yerine bir disk oluştururken.
* Azure, yalnızca 1. kuşak sanal makineleri destekler. 1\. nesil sanal makine VHD dosya biçimine VHDX ve sabit boyutlu diske dinamik olarak genişletilen dönüştürebilirsiniz. Bir sanal makinenin oluşturulması değiştiremezsiniz. Daha fazla bilgi için [Hyper-V'de 1 veya 2. kuşak sanal makine oluşturmalısınız?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)
* VHD için izin verilen boyut 1,023 GB'dir.
* Linux sistemini yüklerken mantıksal birim Yöneticisi (çoğu yüklemeleri varsayılan olan LVM) yerine standart bölümleri kullanmanızı öneririz. Standart bölümleri kullanarak, özellikle bir işletim sistemi diski hiç olmadığı kadar başka bir özdeş sanal makineye sorun giderme için ekli ise kopyalanan sanal makineler ile LVM ad çakışmalarını önlemek. [LVM'yi](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veri diskleri üzerinde kullanılabilir.
* UDF dosya sistemleri bağlamak için çekirdek desteği gereklidir. Azure'da ilk önyüklemede sağlama yapılandırması Konuk bağlı UDF biçimli medyasını kullanarak Linux VM geçirilir. Azure Linux Aracısı yapılandırmasını okuma ve VM sağlama UDF dosya sistemini bağlamalarına gerekir.
* Linux çekirdek sürümleri daha önce 2.6.37 NUMA Hyper-V üzerinde daha büyük VM boyutlarıyla desteklemez. Yukarı Akış kullanan eski dağıtımlar Bu sorun öncelikle etkileri Red Hat 2.6.32 çekirdek ve Red Hat Enterprise Linux (RHEL) 6.6 (çekirdek 2.6.32 504) olarak düzeltildi. 2\.6.32-504 önyükleme parametresi ayarlanmalıdır daha özel çekirdekler 2.6.37 eski veya RHEL tabanlı çekirdekler eski çalıştıran sistemlere `numa=off` grub.conf çekirdek komut satırında. Daha fazla bilgi için [Red Hat KB 436883](https://access.redhat.com/solutions/436883).
* İşletim sistemi diski üzerinde takas bölümü yapılandırmayın. Linux aracısı aşağıdaki adımlarda açıklandığı gibi geçici kaynak diski üzerinde takas dosyası oluşturmak için yapılandırılabilir.
* Tüm VHD'leri azure'da bir sanal Boyut 1 MB ile uyumlu olması gerekir. Ham bir diskten VHD'ye dönüştürme yaparken aşağıdaki adımlarda anlatıldığı gibi ham disk boyutu 1 MB, dönüştürmeden önce bir çok olduğunu sağlamalısınız.

### <a name="installing-kernel-modules-without-hyper-v"></a>Hyper-v'siz çekirdek modülleri yükleme
Linux Azure'da çalıştırmak için belirli çekirdek modülleri gerektirir. Bu nedenle azure Hyper-V hiper yönetici üzerinde çalışır. Hyper-V dışında oluşturulmuş bir VM'niz varsa, sanal Makineyi bir Hyper-V ortamında çalıştığından emin algılamazsa Linux yükleyicileri sürücüleri Hyper-V için ilk ramdisk (initrd veya initramfs) içermeyebilir. Linux görüntünüzü hazırlamak için bir farklı sanallaştırma sistemi (örneğin, Virtualbox, KVM ve benzeri) kullanırken, böylece initrd yeniden gerekebilir, en az hv_vmbus ve hv_storvsc çekirdek modülleri ilk ramdisk üzerinde kullanılabilir.  Yukarı Akış Red Hat dağıtım ve diğerleri üzerinde büyük olasılıkla tabanlı sistemler için bu bilinen bir sorun var.

İnitrd veya initramfs görüntü yeniden oluşturma mekanizması dağıtıma bağlı olarak değişiklik gösterebilir. Dağıtımınıza ait belgeler veya destek için uygun yordamı başvurun.  İnitrd kullanarak yeniden oluşturma için bir örnek aşağıda verilmiştir `mkinitrd` yardımcı programı:

1. Mevcut initrd görüntüsü yedekleyin:

    ```
    cd /boot
    sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak
    ```

2. İnitrd hv_vmbus ve hv_storvsc çekirdek modülleri ile yeniden oluşturun:

    ```
    sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`
    ```

### <a name="resizing-vhds"></a>VHD yeniden boyutlandırma
VHD görüntüleri azure'da bir sanal Boyut 1 MB ile hizalı olmalıdır.  Genellikle, Hyper-V kullanılarak oluşturulan VHD'lerin düzgün şekilde hizalanmış.  VHD doğru bir şekilde hizalı değil ise, bir VHD'den görüntü oluşturmaya çalıştığınızda, aşağıdakine benzer bir hata iletisi alabilirsiniz.

* VHD http:\//\<mystorageaccount >.blob.core.windows.net/vhds/MyLinuxVM.vhd 21475270656 bayt desteklenmeyen sanal boyutuna sahiptir. Boyut (MB) cinsinden bir tam sayı olmalıdır.

Bu durumda, Hyper-V Yöneticisi konsolunu kullanarak VM'yi yeniden boyutlandırın veya [boyutlandırma VHD](https://technet.microsoft.com/library/hh848535.aspx) PowerShell cmdlet'i.  Bir Windows ortamında çalışıyorsa olmayan kullanmanızı öneririz `qemu-img` (gerekirse) dönüştürme ve VHD'yi yeniden boyutlandırmak için.

> [!NOTE]
> Var olan bir [qemu img bilinen hata](https://bugs.launchpad.net/qemu/+bug/1490611) sürümleri > düzgün şekilde biçimlendirilmemiş bir VHD sonuçları 2.2.1 =. Sorun QEMU 2.6 içinde düzeltilmiştir. Kullanmanızı öneririz `qemu-img` 2.2.0 veya alt, veya 2.6 ya da daha yüksek.
> 

1. Doğrudan gibi araçları kullanarak VHD'yi yeniden boyutlandırma `qemu-img` veya `vbox-manage` yapılamamasına bir VHD neden olabilir.  Öncelikle VHD için bir ham disk görüntüsü dönüştürmenizi öneririz.  Ardından sanal makine görüntüsünü ham disk görüntü (örneğin, KVM bazı hiper yöneticilerin için varsayılan) olarak oluşturulmuş olsa bile, bu adımı atlayabilirsiniz.
 
    ```
    qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw
    ```

1. Sanal Boyut 1 MB ile hizalanır. böylece gerekli disk görüntü boyutu hesaplayın.  Aşağıdaki bash Kabuk betiği kullanır `qemu-img info` sanal disk görüntüsünün boyutunu belirlemek için ve sonraki 1 MB boyutuna hesaplar.

    ```bash
    rawdisk="MyLinuxVM.raw"
    vhddisk="MyLinuxVM.vhd"

    MB=$((1024*1024))
    size=$(qemu-img info -f raw --output json "$rawdisk" | \
    gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

    rounded_size=$((($size/$MB + 1)*$MB))
    
    echo "Rounded Size = $rounded_size"
    ```

3. Kullanarak ham disk yeniden boyutlandırma `$rounded_size` ayarlanan yukarıdaki.

    ```bash
    qemu-img resize MyLinuxVM.raw $rounded_size
    ```

4. Şimdi, ham disk geri sabit boyutlu VHD'ye dönüştürür.

    ```bash
    qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd
    ```

   Veya 2.6 + sürümü qemu ile dahil `force_size` seçeneği.

    ```bash
    qemu-img convert -f raw -o subformat=fixed,force_size -O vpc MyLinuxVM.raw MyLinuxVM.vhd
    ```

## <a name="linux-kernel-requirements"></a>Linux çekirdeğinin gereksinimleri

Hyper-V ve Azure Linux Integration Services (LIS) sürücülerini Yukarı Akış Linux çekirdeğinin doğrudan katkıda. (Örneğin, 3.x) yeni bir Linux çekirdek sürümü dahil birçok dağıtımları bu sürücüleri kullanılabilir zaten var veya aksi halde bu sürücüleri backported sürümleri ile kendi defterleri sağlayın.  Mümkün olduğunda çalışmasını öneririz. Bu nedenle bu sürücüleri sürekli yeni düzeltmeler ve özellikler, Yukarı Akış çekirdek güncelleştirilen bir [dağıtım onaylı](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Bu düzeltmeler ve güncelleştirmeler içerir.

Bir değişken 6.0 için 6.3 Red Hat Enterprise Linux sürümleri çalıştırıyorsanız, ardından yüklemeniz gerekir, [Hyper-V için son LIS sürücüleri](https://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). RHEL 6.4 + (ve türevleri) ile başlayan LIS sürücüleri ile çekirdek zaten dahildir ve bu nedenle hiçbir ek yükleme paketleri gereklidir.

Özel bir çekirdek gerekiyorsa, yeni bir çekirdek sürümü (örneğin 3.8 +) öneririz. Dağıtımları veya kendi çekirdek bakımını yapan satıcılar için backport için düzenli olarak LIS sürücüleri Yukarı Akış çekirdekten, özel çekirdeğe ihtiyacınız olacak.  Nispeten yeni bir çekirdek sürümü zaten çalıştırmakta olduğunuz olsa bile, Yukarı Akış hiçbirini izlememektedir LIS sürücüleri ve backport bunları gerektiği şekilde giderir önerilir. İçinde belirtilen konumlar LIS sürücü kaynak dosyalarının [MAINTAINERS](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) Linux çekirdek kaynak ağacının dosyasında:
```
    F:    arch/x86/include/asm/mshyperv.h
    F:    arch/x86/include/uapi/asm/hyperv.h
    F:    arch/x86/kernel/cpu/mshyperv.c
    F:    drivers/hid/hid-hyperv.c
    F:    drivers/hv/
    F:    drivers/input/serio/hyperv-keyboard.c
    F:    drivers/net/hyperv/
    F:    drivers/scsi/storvsc_drv.c
    F:    drivers/video/fbdev/hyperv_fb.c
    F:    include/linux/hyperv.h
    F:    tools/hv/
```
Aşağıdaki düzeltme ekleri çekirdek eklenmesi gerekir. Bu liste tüm dağıtımları için tam olamaz.

* [ata_piix: varsayılan olarak Hyper-V sürücüleri disklere ertele](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
* [storvsc: Hesabı için aktarım sırasında paketlerinde SIFIRLAMA yolu](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
* [storvsc: WRITE_SAME kullanımını kaçının](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
* [storvsc: RAID ve sanal ana bilgisayar bağdaştırıcısı sürücüleri yazma aynı devre dışı](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
* [storvsc: NULL işaretçiye başvurma düzeltme](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
* [storvsc: halka arabelleği hataları g/ç dondurma neden olabilir](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
* [scsi_sysfs: __scsi_remove_device çift yürütülmesini karşı koruma](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)

## <a name="the-azure-linux-agent"></a>Azure Linux Aracısı
[Azure Linux Aracısı](../extensions/agent-linux.md) `waagent` Azure'da bir Linux sanal makine sağlar. Dosya sorunları, en son sürümünü alın veya çekme istekleri gönderme [Linux Aracısı GitHub deposunu](https://github.com/Azure/WALinuxAgent).

* Linux Aracısı Apache 2.0 lisansı altında yayınlanır. Çoğu dağıtımda zaten RPM veya deb paketleri için aracı sağlar ve bu paketleri kolayca yüklü ve güncelleştirilmiş.
* Python v2.6 + Azure Linux Aracısı gerektirir.
* Aracı ayrıca, python pyasn1 modül gerektirir. Çoğu dağıtımları yüklenmesi için ayrı bir paket bu modülü sağlar.
* Bazı durumlarda, Azure Linux Aracısı NetworkManager ile uyumlu olmayabilir. Çok sayıda dağıtımları tarafından sağlanan RPM/Deb paketi NetworkManager waagent paketine çakışma olarak yapılandırın. Bu gibi durumlarda Linux Aracısı paketi yüklediğinizde NetworkManager kaldırılır.
* Azure Linux aracısı veya bu düzeyin üstünde olmalıdır [desteklenen minimum sürüm](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support).

## <a name="general-linux-system-requirements"></a>Genel Linux sistem gereksinimleri

1. Tüm konsol iletileri ilk seri bağlantı noktasına gönderilen kernel önyükleme satırına GRUB veya GRUB2 aşağıdaki parametreleri içerecek şekilde değiştirin. Bu iletiler Azure yardımcı olabilecek sorunları hata ayıklama desteği.
    ```  
    console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300
    ```
    Ayrıca öneririz *kaldırma* varsa aşağıdaki parametreleri.
    ```  
    rhgb quiet crashkernel=auto
    ```
    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilen tüm günlükler istediğimiz bir bulut ortamında kullanışlı değildir. `crashkernel` Seçenek olabilir sol gerekirse yapılandırılmış, ancak Not Bu parametre sanal makinede en az 128 daha küçük VM boyutları için sorunlu olabilecek MB, kullanılabilir bellek miktarını azaltır.

1. Azure Linux aracısını yükleyin.
  
    Azure Linux Aracısı, azure'da bir Linux görüntüsü sağlamak için gereklidir.  Birçok Dağıtım Aracısı'nı (paket genellikle WALinuxAgent veya walinuxagent olarak adlandırılır) bir RPM veya Deb paketini sağlayın.  Aracı da el ile adımları izleyerek yüklenebilir [Linux Aracısı Kılavuzu](../extensions/agent-linux.md).

1. SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış emin olun.  Bu yapılandırma, genellikle varsayılan değerdir.

1. İşletim sistemi diski üzerinde takas alanı oluşturmayın.
  
    Azure Linux Aracısı, Azure üzerinde sağladıktan sonra VM'ye bağlı yerel kaynak disk kullanılan takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak disk bir *geçici* disk ve sanal Makinenin sağlaması kaldırıldığında boşaltılabilir. Azure Linux Aracısı'nı (yukarıdaki adım 2) yükledikten sonra aşağıdaki parametrelerle /etc/waagent.conf gerektiği gibi değiştirin.
    ```  
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: Set this to your desired size.
    ```
1. Sanal makinenin sağlamasını kaldırmak için aşağıdaki komutları çalıştırın.
  
     ```
     sudo waagent -force -deprovision
     export HISTSIZE=0
     logout
     ```  
   > [!NOTE]
   > Üzerinde VirtualBox çalıştırdıktan sonra aşağıdaki hatayı görebilirsiniz `waagent -force -deprovision` bildiren `[Errno 5] Input/output error`. Bu hata iletisini kritik değil ve yok sayılabilir.

* Sanal makineyi kapatır ve Azure'a VHD yükleme.

