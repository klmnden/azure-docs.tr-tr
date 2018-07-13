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
ms.date: 03/12/2018
ms.author: szark
ms.openlocfilehash: 17b4df83b141d5365a8d6244c4ab73b0eba5ed73
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972358"
---
# <a name="information-for-non-endorsed-distributions"></a>Desteklenmeyen Dağıtımlarla ilgili bilgiler
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Azure platformu, yalnızca bir Linux işletim sistemi çalıştıran sanal makineler için SLA'sı geçerlidir, [destekli dağıtımlar](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kullanılır. Bu desteklenen dağıtımlar için gerekli yapılandırmayla Azure Market'teki Linux görüntüleri sağlanır.

* [-Azure'da Linux destekli dağıtımlar](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Microsoft azure'da Linux görüntüleri için destek](https://support.microsoft.com/kb/2941892)

Azure üzerinde çalışan tüm dağıtımları düzgün bir platform üzerinde çalıştırmasını olanağı sağlamak için önkoşulları karşılaması gerekir.  Her dağıtım farklı olarak bu makalede olmadığı göre en kapsamlı; ve aşağıdaki tüm ölçütleri karşılayan olsa bile bu platformda düzgün çalıştığından emin olmak için Linux sisteminizi önemli ölçüde ince gerektiğini mümkündür.

İle başlamanız önerilir, bu nedenle olduğu bir [Azure destekli dağıtımlarda Linux](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mümkün olduğunda. Aşağıdaki makalede Azure'da desteklenen çeşitli desteklenen Linux dağıtımı hazırlama konusunda size kılavuzluk eder:

* **[CentOS tabanlı dağıtımlar](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

Bu makalenin geri kalanında, Linux dağıtımınız Azure'da çalıştırmaya yönelik genel kılavuz odaklanır.

## <a name="general-linux-installation-notes"></a>Genel Linux yükleme notları
* VHDX biçimi, Azure'da yalnızca desteklenmiyor **VHD'yi sabit**.  Disk Hyper-V Yöneticisi'ni veya convert-vhd cmdlet'ini kullanarak VHD biçimine dönüştürebilirsiniz. VirtualBox kullanıyorsanız seçerek yani **boyutu sabit** diski oluştururken dinamik olarak ayrılan varsayılan aksine.
* Azure, yalnızca 1. kuşak sanal makineleri destekler. 1. nesil sanal makine VHD dosya biçimine VHDX ve sabit boyutlu diske dinamik olarak genişletilen dönüştürebilirsiniz. Ancak, bir sanal makinenin oluşturulması değiştiremezsiniz. Daha fazla bilgi için [Hyper-V'de 1 veya 2. kuşak sanal makine oluşturmalısınız?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)
* VHD için izin verilen boyut 1,023 GB'dir.
* Linux sistemini yüklerken olduğu *önerilen* LVM (genellikle birçok yüklemeleri için varsayılan) yerine standart bölümleri kullanın. Özellikle bir işletim sistemi diski hiç sorun giderme için başka bir özdeş sanal makineye eklenmesi gerekiyorsa bu kopyalanan sanal makineler ile ad çakışmalarını LVM uğraşmasına gerek kalmaz. [LVM'yi](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veri diskleri üzerinde kullanılabilir.
* UDF dosya sistemleri bağlamak için çekirdek desteği gereklidir. Azure'da ilk önyüklemede sağlama yapılandırması Linux VM Konuk bağlı UDF biçimli ortam aracılığıyla geçirilir. Azure Linux Aracısı yapılandırmasını okuma ve VM sağlama UDF dosya sistemi monte etmesini mümkün olması gerekir.
* Linux çekirdek sürümleri 2.6.37 NUMA büyük VM boyutları ile Hyper-V üzerinde desteklemez. Yukarı Akış kullanan eski dağıtımlar Bu sorun öncelikle etkileri Red Hat 2.6.32 çekirdek ve RHEL 6.6 (çekirdek 2.6.32 504) düzeltildi. 2.6.32-504 önyükleme parametresi ayarlanmalıdır daha özel çekirdekler 2.6.37 eski veya RHEL tabanlı çekirdekler eski çalıştıran sistemlere `numa=off` çekirdeğini grub.conf içinde komut satırı. Daha fazla bilgi için bkz: Red Hat [KB 436883](https://access.redhat.com/solutions/436883).
* İşletim sistemi diski üzerinde takas bölümü yapılandırmayın. Linux Aracısı, geçici kaynak diski üzerinde takas dosyası oluşturmak için yapılandırılabilir.  Aşağıdaki adımlarda bunu hakkında daha fazla bilgi bulunabilir.
* Tüm VHD'leri azure'da bir sanal Boyut 1 MB ile uyumlu olması gerekir. Ham bir diskten VHD'ye dönüştürme yaparken ham disk boyutu 1 MB dönüştürmeden önce bir çok olduğundan emin olmalısınız. Aşağıdaki adımlarda daha fazla bilgi bulunabilir.

### <a name="installing-kernel-modules-without-hyper-v"></a>Hyper-v'siz çekirdek modülleri yükleme
Linux belirli çekirdek modülleri Azure'da çalıştırmak için yüklü olmasını gerektirir. Bu nedenle azure Hyper-V hiper yönetici üzerinde çalışır. Hyper-V dışında oluşturulmuş bir VM'niz varsa, bir Hyper-V ortamında çalıştığından emin algılamazsa Linux yükleyicileri sürücüleri Hyper-V için ilk ramdisk (initrd veya initramfs) içermeyebilir. Farklı sanallaştırma sistemi (diğer bir deyişle, Virtualbox, KVM, vb.), Linux görüntüsünü hazırlamak için kullanırken initrd emin olmak için yeniden oluşturma gerekebilir en az `hv_vmbus` ve `hv_storvsc` çekirdek modülleri ilk ramdisk üzerinde kullanılabilir.  Bu bilinen bir sorun en az bir Yukarı Akış Red Hat dağıtım noktasında tabanlı sistemler açıktır.

İnitrd veya initramfs görüntü yeniden oluşturma mekanizması dağıtıma bağlı olarak değişiklik gösterebilir. Dağıtımınıza ait belgeler veya destek için uygun yordamı başvurun.  İnitrd kullanarak yeniden oluşturmak nasıl bir örnek aşağıda verilmiştir `mkinitrd` yardımcı programı:

İlk olarak, var olan initrd görüntüsü yedekleyin:

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

Ardından, initrd ile yeniden `hv_vmbus` ve `hv_storvsc` çekirdek modülleri:

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### <a name="resizing-vhds"></a>VHD yeniden boyutlandırma
VHD görüntüleri azure'da bir sanal Boyut 1 MB ile hizalı olmalıdır.  Genellikle, Hyper-V kullanılarak oluşturulan VHD'lerin zaten doğru hizalanmalıdır.  VHD düzgün hizalanmış değil, oluşturmaya çalıştığınızda şuna benzer bir hata iletisi alabilirsiniz bir *görüntü* , VHD'den:

    "The VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. The size must be a whole number (in MBs).”

Bu davranışı düzeltmek için Hyper-V Yöneticisi konsolunu kullanarak VM'yi yeniden boyutlandırma veya [boyutlandırma VHD](http://technet.microsoft.com/library/hh848535.aspx) Powershell cmdlet'i.  Bir Windows ortamında çalıştırmıyorsanız qemu img (gerekirse) dönüştürmek için kullanılması önerilir ve VHD'yi yeniden boyutlandırın.

> [!NOTE]
> Qemu img sürümlerinde bilinen bir hata olduğunu > düzgün şekilde biçimlendirilmemiş bir VHD sonuçları 2.2.1 =. Sorun QEMU 2.6 içinde düzeltilmiştir. Qemu-img 2.2.0 ya da alt kullanın veya 2.6 veya sonraki bir sürüme güncelleştirmek için önerilir. Başvuru: https://bugs.launchpad.net/qemu/+bug/1490611.
> 
> 

1. Doğrudan gibi araçları kullanarak VHD'yi yeniden boyutlandırma `qemu-img` veya `vbox-manage` yapılamamasına bir VHD neden olabilir.  Bu nedenle, bir ham disk görüntüsünü VHD'ye ilk Dönüştür önerilir.  Ardından ham disk görüntüsü (örneğin, KVM bazı hiper yöneticilerin için varsayılan) olarak VM görüntü zaten oluşturulmuş olsa bile bu adımı atlayabilirsiniz:
   
       # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

2. Gerekli sanal Boyut 1 MB ile uyumlu olduğunu emin olmak için disk görüntü boyutu hesaplayın.  Aşağıdaki bash Kabuk betiği bu konuda size yardımcı olabilir.  Betik kullanır "`qemu-img info`" sanal disk görüntüsünün boyutunu belirlemek için ve sonraki 1 MB boyutuna hesaplar:
   
       rawdisk="MyLinuxVM.raw"
       vhddisk="MyLinuxVM.vhd"
   
       MB=$((1024*1024))
       size=$(qemu-img info -f raw --output json "$rawdisk" | \
              gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
   
       rounded_size=$((($size/$MB + 1)*$MB))
       echo "Rounded Size = $rounded_size"

3. Yukarıdaki komut kümesi olarak $rounded_size kullanarak ham diski yeniden boyutlandırma:
   
       # qemu-img resize MyLinuxVM.raw $rounded_size

4. Şimdi, ham disk geri sabit boyutlu VHD'ye dönüştürür:
   
       # qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd

   Veya qemu sürümüyle **2.6 +** dahil `force_size` seçeneği:

       # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc MyLinuxVM.raw MyLinuxVM.vhd

## <a name="linux-kernel-requirements"></a>Linux çekirdeğinin gereksinimleri
Hyper-V ve Azure Linux Integration Services (LIS) sürücülerini Yukarı Akış Linux çekirdeğinin doğrudan katkıda. Yeni bir Linux çekirdek sürümü (yani 3.x) dahil birçok dağıtımları bu sürücüleri kullanılabilir zaten var veya aksi takdirde bu sürücüleri backported sürümleri ile kendi defterleri sağlayın.  Mümkün olduğunda çalıştırmak için önerilir bu nedenle bu sürücüleri sürekli yeni düzeltmeler ve özellikler, Yukarı Akış çekirdek güncelleştirilen bir [dağıtım onaylı](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Bu düzeltmeler ve güncelleştirmeler içerir.

Bir değişken Red Hat Enterprise Linux sürümleri çalıştırıyorsanız **6.0 6.3**, sonra da Hyper-V için LIS en son sürücüleri yüklemeniz gerekir. Sürücüleri bulunabilir [bu konumdaki](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). RHEL itibarıyla **6.4 +** (ve türevleri) LIS sürücüleri ile çekirdek zaten dahildir ve bu nedenle hiçbir ek yükleme paketleri gereklidir bu sistemlerin Azure'da çalışmak üzere.

Özel bir çekirdek gerekiyorsa, daha yeni bir çekirdek sürümünü kullanmanız önerilir (yani **3.8 +**). Bu dağıtımları veya kendi çekirdek bakımını yapan satıcılar için bazı çaba düzenli olarak backport LIS gereklidir, özel çekirdek Yukarı Akış çekirdekten sürücüleri.  Nispeten yeni bir çekirdek sürümü çalışmakta olan olsa bile, Yukarı Akış düzeltmelerle LIS sürücüleri ve backport bunları gerektiği şekilde izlemek için önemle tavsiye edilir. LIS sürücü kaynak dosyalarının konumunu kullanılabilir [MAINTAINERS](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) Linux çekirdek kaynak ağacının dosyasında:

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

En azından aşağıdaki düzeltme ekleri olmaması sorunlara neden Azure'da bilinen ve Çekirdekte bu şekilde eklenmelidir. Bu liste, kapsamlı veya tüm dağıtımlar için tam olmadığı göre yok:

* [ata_piix: varsayılan olarak Hyper-V sürücüleri disklere ertele](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
* [storvsc: hesap SIFIRLAMA yolunda aktarım sırasında paketler için](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
* [storvsc: WRITE_SAME kullanımını kaçının](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
* [storvsc: devre dışı yazma aynı RAID ve sanal ana bilgisayar bağdaştırıcısı sürücüleri](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
* [storvsc: NULL işaretçiye düzeltme](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
* [storvsc: halka arabelleği hataları g/ç dondurma neden olabilir](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
* [scsi_sysfs: __scsi_remove_device çift yürütülmesini karşı koruma](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)

## <a name="the-azure-linux-agent"></a>Azure Linux Aracısı
[Azure Linux Aracısı](../extensions/agent-linux.md) (waagent), Azure'da bir Linux sanal makine düzgün şekilde sağlamak için gereklidir. Dosya sorunları, en son sürümünü alın veya çekme istekleri gönderme [Linux Aracısı GitHub deposunu](https://github.com/Azure/WALinuxAgent).

* Linux Aracısı Apache 2.0 lisansı altında yayınlanır. Çoğu dağıtımda zaten RPM veya deb paketleri için aracıyı sağlayın ve bu nedenle bazı durumlarda, bu sunucuya yüklenebilir ve çok az çabayla güncelleştirildi.
* Python v2.6 + Azure Linux Aracısı gerektirir.
* Aracı ayrıca, python pyasn1 modül gerektirir. Çoğu dağıtımların bu yüklenebilmesi için ayrı bir paket sağlayın.
* Bazı durumlarda, Azure Linux Aracısı NetworkManager ile uyumlu olmayabilir. Çok sayıda dağıtımları tarafından sağlanan RPM/Deb paketi waagent paketine çakışma olarak NetworkManager yapılandırın ve Linux aracı paketi yüklediğinizde, bu nedenle NetworkManager kaldırılır.
* Azure Linux Aracısı olması gereken en düşük desteklenen sürüm için bu makaleye bakın [ayrıntıları](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support).

## <a name="general-linux-system-requirements"></a>Genel Linux sistem gereksinimleri

* Kernel önyükleme satırına GRUB veya GRUB2 aşağıdaki parametreleri içerecek şekilde değiştirin. Bu ayrıca, Azure size yardımcı olabilir ilk seri bağlantı noktası için tüm konsol iletileri gönderilmesini sağlar sorunları hata ayıklama desteği:
  
        console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300
  
    Bu ayrıca, Azure size yardımcı olabilir ilk seri bağlantı noktası için tüm konsol iletileri gönderilmesini sağlar sorunları hata ayıklama desteği.
  
    Yukarıdakine ek olarak, ancak önerilir *Kaldır* varsa aşağıdaki parametreleri:
  
        rhgb quiet crashkernel=auto
  
    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilmesi için tüm günlükleri istediğimiz bir bulut ortamında kullanışlı değildir. `crashkernel` Seçenek olabilir sol isterseniz yapılandırılmış, ancak Not Bu parametre daha küçük VM boyutlarına sorunlu olabilecek VM tarafından 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır.

* Azure Linux aracısını yükleme
  
    Azure Linux Aracısı, azure'da bir Linux görüntüsü sağlamak için gereklidir.  Birçok Dağıtım Aracısı'nı (paket genellikle 'WALinuxAgent' veya 'walinuxagent' olarak adlandırılır) bir RPM veya Deb paketini sağlayın.  Aracı da el ile adımları izleyerek yüklenebilir [Linux Aracısı Kılavuzu](../extensions/agent-linux.md).

* SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış emin olun.  Bu, genellikle varsayılan değerdir.

* İşletim sistemi diski üzerinde takas alanı oluşturma
  
    Azure Linux Aracısı, Azure üzerinde sağladıktan sonra VM'ye bağlı yerel kaynak disk kullanılan takas alanı otomatik olarak yapılandırabilirsiniz. Yerel kaynak disk bir *geçici* disk ve sanal Makinenin sağlaması kaldırıldığında boşaltılabilir. Azure Linux Aracısı'nı yükledikten sonra (önceki adıma bakın), aşağıdaki parametrelerle /etc/waagent.conf uygun şekilde değiştirin:
  
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

* Son adım olarak, sanal makinenin sağlamasını kaldırmak için aşağıdaki komutları çalıştırın:
  
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
  
  > [!NOTE]
  > Üzerinde VirtualBox çalıştırdıktan sonra aşağıdaki hatayı görebilirsiniz ' waagent-force - sağlamayı kaldırma ': `[Errno 5] Input/output error`. Bu hata iletisini kritik değil ve yok sayılabilir.
  > 
  > 

* Sanal makineyi kapatır ve Azure'a VHD yükleme.

