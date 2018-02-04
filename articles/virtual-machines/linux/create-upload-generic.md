---
title: "Oluşturma ve azure'da bir Linux VHD yükleme"
description: "Oluşturma ve bir Azure sanal sabit Linux işletim sistemi içeren disk (VHD) yükleme hakkında bilgi edinme."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: d351396c-95a0-4092-b7bf-c6aae0bbd112
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 771b3ffa0ece10e7373011536a12ed4cb1a1dd6d
ms.sourcegitcommit: e19742f674fcce0fd1b732e70679e444c7dfa729
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="information-for-non-endorsed-distributions"></a>Desteklenmeyen Dağıtımlarla ilgili bilgiler
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Azure platformu SLA yalnızca bir zaman Linux işletim sistemi çalıştıran sanal makineler için geçerlidir, [destekli dağıtımlar](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kullanılır. Azure resmi galeride sağlanmayan tüm Linux dağıtımları gerekli yapılandırma ile doğrulanan dağıtımları ' dir.

* [Azure - Linux'ta destekli dağıtımlar](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Microsoft Azure Linux görüntüleri için destek](https://support.microsoft.com/kb/2941892)

Azure üzerinde çalışan tüm dağıtımları platformda düzgün çalışması için bir fırsat için önkoşulları karşılaması gerekir.  Her dağıtım farklı olarak bu makalede halinde kapsamlı; ve aşağıdaki ölçütlere uyan olsa bile, yine önemli ölçüde bu platformda düzgün çalışmasını sağlamak için Linux sisteminizi ince ayar gerektiğini oldukça mümkündür.

Biri ile başlamanızı öneririz bu nedenle için bizim [Azure destekli dağıtımlar Linux'ta](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) mümkün olduğunda. Aşağıdaki makaleler, Azure üzerinde desteklenen çeşitli doğrulanan Linux dağıtımları hazırlamak nasıl yardımcı olur:

* **[CentOS tabanlı dağıtımlar](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

Bu makalenin geri kalanında Linux dağıtımınızı Azure üzerinde çalışan için genel rehberlik özelliklere odaklanır.

## <a name="general-linux-installation-notes"></a>Genel Linux yükleme notları
* VHDX biçimi, Azure'da yalnızca desteklenmiyor **VHD sabit**.  Disk Hyper-V Yöneticisi'ni veya convert-vhd cmdlet'ini kullanarak VHD biçimine dönüştürebilirsiniz. VirtualBox kullanıyorsanız bu seçerek gelir **boyutu sabit** disk oluşturulurken dinamik olarak ayrılan varsayılan aksine.
* Azure yalnızca 1. nesil sanal makineleri desteklemektedir. VHD dosya biçimine VHDX ve dinamik olarak genişletilen bir sabit boyutlu disk için 1. nesil bir sanal makine dönüştürebilirsiniz. Ancak, bir sanal makinenin oluşturma değiştiremezsiniz. Daha fazla bilgi için bkz: [Hyper-V'de 1 veya 2. nesil sanal makine oluşturmalısınız?](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v)
* VHD için izin verilen maksimum boyutu 1,023 GB'dir.
* Linux sistemini yüklerken olan *önerilen* LVM (genellikle birçok yüklemeleri için varsayılan) yerine standart bölümleri kullanın. Özellikle bir işletim sistemi diski şimdiye kadar aynı başka bir VM için sorun giderme için eklenmesi gerekiyorsa, bu kopyalanan VMs LVM ad çakışmalarını önler. [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veri diskler üzerinde kullanılabilir.
* UDF dosya sistemleri bağlanması için çekirdek desteği gereklidir. Azure üzerinde ilk önyükleme sırasında sağlama yapılandırma Linux VM konağına bağlı UDF biçimli medya üzerinden geçirilir. Azure Linux Aracısı'nı yapılandırmasını okuma ve VM sağlamak için UDF dosya sistemi bağlama kurabilmesi gerekir.
* Linux çekirdek sürümleri 2.6.37 aşağıda NUMA ile büyük VM boyutları üzerinde Hyper-V desteklemez. Yukarı Akış kullanarak eski dağıtımları Bu sorun öncelikle etkileri Red Hat 2.6.32 çekirdek ve RHEL 6.6 (çekirdek 2.6.32 504) giderilmiştir. 2.6.32-504 önyükleme parametresinin ayarlamalısınız daha özel tekrar 2.6.37 eski veya tekrar eski RHEL tabanlı çalıştıran sistemlerde `numa=off` grub.conf içinde komut satırı çekirdeğini. Red Hat daha fazla bilgi için bkz: [KB 436883](https://access.redhat.com/solutions/436883).
* Bir takas bölüm işletim sistemi disk üzerinde yapılandırmayın. Linux Aracısı geçici kaynak disk üzerinde bir takas dosyası oluşturmak için yapılandırılabilir.  Aşağıdaki adımlarda bu hakkında daha fazla bilgi bulunabilir.
* Tüm VHD'leri boyutları 1 MB'ün katları olmalıdır.

### <a name="installing-kernel-modules-without-hyper-v"></a>Hyper-V olmadan çekirdek modülleri yükleme
Azure Hyper-V hiper yönetici üzerinde çalışır ve böylece Linux Azure üzerinde çalıştırmak için belirli çekirdek modülleri yüklü olmasını gerektirir. Hyper-V dışında oluşturulmuş bir VM'niz varsa, çalışıp çalışmadığını algılar sürece Linux yükleyicileri sürücüleri Hyper-V için ilk ramdisk (initrd veya initramfs) içeremeyen bir Hyper-V ortamı. Farklı sanallaştırma sistem (yani Virtualbox, KVM, vb.), Linux görüntüsünü hazırlamak için kullanırken, emin olmak için initrd yeniden oluşturma gerekebilir en az `hv_vmbus` ve `hv_storvsc` çekirdek modülleri ilk ramdisk üzerinde kullanılabilir.  Bilinen bir sorun en az Yukarı Akış Red Hat dağıtım tabanlı sistemlerde budur.

İnitrd veya initramfs görüntüsünü yeniden oluşturma için mekanizma dağıtım bağlı olarak değişebilir. Lütfen dağıtım ait belgelere bakın veya için uygun yordamı destekler.  İşte initrd kullanarak yeniden ilişkin bir örnek `mkinitrd` yardımcı programı:

İlk olarak, var olan initrd görüntüyü oluşturan yedekleyin:

    # cd /boot
    # sudo cp initrd-`uname -r`.img  initrd-`uname -r`.img.bak

Ardından, initrd ile yeniden `hv_vmbus` ve `hv_storvsc` çekirdek modülleri:

    # sudo mkinitrd --preload=hv_storvsc --preload=hv_vmbus -v -f initrd-`uname -r`.img `uname -r`


### <a name="resizing-vhds"></a>VHD'ler yeniden boyutlandırma
VHD görüntüleri Azure üzerinde bir sanal boyutu 1 MB ile hizalı olması gerekir.  Genellikle, Hyper-V kullanılarak oluşturulan VHD zaten doğru hizalanmalıdır.  VHD düzgün hizalanmış değil sonra oluşturma girişiminde bulunduğunuzda aşağıdakine benzer bir hata iletisi alabilirsiniz bir *görüntü* , VHD'den:

    "The VHD http://<mystorageaccount>.blob.core.windows.net/vhds/MyLinuxVM.vhd has an unsupported virtual size of 21475270656 bytes. The size must be a whole number (in MBs).”

Bu sorunu gidermek için Hyper-V Yöneticisi konsolunu kullanarak VM boyutlandırabilirsiniz veya [yeniden boyutlandırma VHD](http://technet.microsoft.com/library/hh848535.aspx) Powershell cmdlet'i.  Bir Windows ortamında çalıştırmıyorsanız (gerekirse) dönüştürmek için qemu img kullanın ve VHD'yi yeniden boyutlandırmak için önerilir.

> [!NOTE]
> Qemu img sürümlerinde bilinen bir hata varsa > düzgün biçimlendirilmemiş bir VHD sonuçları 2.2.1 =. Sorun QEMU 2.6 düzeltilmiştir. Qemu img 2.2.0 veya alt kullanın veya 2.6 veya daha yüksek güncelleştirmek için önerilir. Reference: https://bugs.launchpad.net/qemu/+bug/1490611.
> 
> 

1. Doğrudan gibi araçları kullanılarak VHD yeniden boyutlandırma `qemu-img` veya `vbox-manage` önyüklenemez bir VHD neden olabilir.  Bu nedenle ilk dönüştürme ham disk görüntüsü VHD'ye önerilir.  VM görüntüsü zaten ham disk görüntüsü (KVM gibi bazı hiper yöneticilerin için varsayılan) oluşturulduysa bu adımı atlayın:
   
       # qemu-img convert -f vpc -O raw MyLinuxVM.vhd MyLinuxVM.raw

2. Sanal Boyut 1 MB ile hizalanır emin olmak için disk görüntüsü gerekli boyutunu hesaplayabilirsiniz.  Aşağıdaki bash Kabuk betiği bu ile yardımcı olabilir.  Komut dosyası kullanır "`qemu-img info`" sanal disk görüntü boyutunu belirlemek için ve ardından İleri 1 MB boyutuna hesaplar:
   
       rawdisk="MyLinuxVM.raw"
       vhddisk="MyLinuxVM.vhd"
   
       MB=$((1024*1024))
       size=$(qemu-img info -f raw --output json "$rawdisk" | \
              gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')
   
       rounded_size=$((($size/$MB + 1)*$MB))
       echo "Rounded Size = $rounded_size"

3. Yukarıdaki komut dosyasını kümesinde olarak $rounded_size kullanarak ham disk yeniden boyutlandırma:
   
       # qemu-img resize MyLinuxVM.raw $rounded_size

4. Şimdi, ham diski bir sabit boyutlu VHD Geri Dönüştür:
   
       # qemu-img convert -f raw -o subformat=fixed -O vpc MyLinuxVM.raw MyLinuxVM.vhd

   Veya qemu sürümüyle **2.6 +** dahil `force_size` seçeneği:

       # qemu-img convert -f raw -o subformat=fixed,force_size -O vpc MyLinuxVM.raw MyLinuxVM.vhd

## <a name="linux-kernel-requirements"></a>Linux çekirdek gereksinimleri
Hyper-V ve Microsoft Azure Linux Tümleştirme hizmetleri (LIS) sürücülerini Yukarı Akış Linux çekirdek doğrudan katkısı. Yeni bir Linux çekirdek sürümü (yani 3.x) dahil birçok dağıtımları kullanılabilir bu sürücüleri zaten var veya aksi halde bu sürücüleri backported sürümleri ile bunların çekirdek sağlar.  Mümkün olduğunda çalıştırmak için önerilir böylece bu sürücüleri sürekli yeni düzeltmeler ve özellikler, Yukarı Akış çekirdek güncelleştirilen bir [dağıtım destekli](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Bu düzeltmeler ve güncelleştirmeler içerecektir.

Bir değişken Red Hat Enterprise Linux sürümleri çalıştırıyorsanız **6.0 6.3**, Hyper-V için en son LIS sürücüleri yüklemeniz gerekir. Sürücüleri bulunabilir [bu konumda](http://go.microsoft.com/fwlink/p/?LinkID=254263&clcid=0x409). RHEL itibariyle **6.4 +** (ve türevleri) LIS sürücüleri zaten çekirdek ile dahil edilir ve bu nedenle hiçbir ek yükleme paketleri gerekiyor bu sistemleri Azure üzerinde çalıştırmak için.

Özel bir çekirdek gerekiyorsa, daha yeni bir çekirdek sürümü kullanmak için önerilir (yani **3.8 +**). Bu dağıtımları veya kendi çekirdek korumak satıcıları için bazı çaba olacak özel çekirdek için Yukarı Akış çekirdekten LIS sürücüleri düzenli olarak backport için gereklidir.  Nispeten yeni bir çekirdek sürümü çalışmakta olan olsa bile, Yukarı Akış düzeltmelerle LIS sürücüleri ve backport olanlar gerektiğinde izlemek için önerilir. LIS sürücü kaynak dosyalarının konumunu kullanılabilir [MAINTAINERS](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/tree/MAINTAINERS) Linux çekirdek kaynak ağaç dosyasında:

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

Bir çok düşük, en aşağıdaki düzeltme ekleri yokluğu bilinen Azure üzerinde sorunlara neden ve Çekirdeği'nde bu nedenle eklenmelidir. Bu liste halinde kapsamlı veya tüm dağıtımlar için tam verilmiştir:

* [ata_piix: varsayılan olarak Hyper-V sürücüleri disklere erteleme](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/ata/ata_piix.c?id=cd006086fa5d91414d8ff9ff2b78fbb593878e3c)
* [storvsc: hesap SIFIRLAMA yolundaki aktarım sırasında paketler için](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/drivers/scsi/storvsc_drv.c?id=5c1b10ab7f93d24f29b5630286e323d1c5802d5c)
* [storvsc: WRITE_SAME kullanımını kaçının](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=3e8f4f4065901c8dfc51407e1984495e1748c090)
* [storvsc: devre dışı yazma aynı RAID ve sanal ana bilgisayar bağdaştırıcısı sürücüleri](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=54b2b50c20a61b51199bedb6e5d2f8ec2568fb43)
* [storvsc: NULL işaretçiye düzeltme](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=b12bb60d6c350b348a4e1460cd68f97ccae9822e)
* [storvsc: halka arabelleği hataları g/ç dondurma neden olabilir](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/storvsc_drv.c?id=e86fb5e8ab95f10ec5f2e9430119d5d35020c951)
* [scsi_sysfs: __scsi_remove_device çift yürütme karşı koruma](https://git.kernel.org/cgit/linux/kernel/git/next/linux-next.git/commit/drivers/scsi/scsi_sysfs.c?id=be821fd8e62765de43cc4f0e2db363d0e30a7e9b)

## <a name="the-azure-linux-agent"></a>Azure Linux Aracısı
[Azure Linux Aracısı](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (waagent) düzgün azure'da bir Linux sanal makine sağlamak için gereklidir. Dosya sorunları, en son sürümünü alın veya çekme istekleri gönderme [Linux Aracısı GitHub deposuna](https://github.com/Azure/WALinuxAgent).

* Linux Aracısı'nı Apache 2.0 lisansı altında yayımlanır. Çoğu dağıtımda zaten RPM veya deb paketleri aracı için sağlayın ve bu nedenle bazı durumlarda bu yüklenebilir ve çok az çaba ile güncelleştirilmiştir.
* Python v2.6 + Azure Linux Aracısı'nı gerektirir.
* Aracı ayrıca python pyasn1 modülü gerektirir. Çoğu dağıtımları bu yüklenebilmesi için ayrı bir paket sağlayın.
* Bazı durumlarda Azure Linux Aracısı'nı NetworkManager ile uyumlu olmayabilir. Birçok dağıtımları tarafından sağlanan RPM/Deb paketleri NetworkManager waagent pakete çakışma olarak yapılandırın ve Linux Aracı paketini yüklediğinizde, bu nedenle NetworkManager kaldırır.
* Azure Linux Aracısı'nı olmalıdır desteklenen minimum sürümü üzerinde bu makale için bkz: [ayrıntıları](https://support.microsoft.com/en-us/help/4049215/extensions-and-virtual-machine-agent-minimum-version-support).

## <a name="general-linux-system-requirements"></a>Genel Linux sistem gereksinimleri

* Çekirdek önyükleme satırı kaz veya GRUB2 aşağıdaki parametreleri içerecek şekilde değiştirin. Bu tüm konsol iletileri hangi Azure yardımcı olabilecek ilk seri bağlantı noktasına gönderilen sağlayacak sorunları ayıklama desteği:
  
        console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300
  
    Bu tüm konsol iletileri hangi Azure yardımcı olabilecek ilk seri bağlantı noktasına gönderilen sağlayacak sorunları ayıklama desteği.
  
    Yukarıdakilerin yanı sıra için önerilir *kaldırmak* varsa aşağıdaki parametreleri:
  
        rhgb quiet crashkernel=auto
  
    Grafik ve sessiz önyükleme seri bağlantı noktasına gönderilecek tüm günlükleri burada istiyoruz bulut ortamında bulunan kullanışlı değildir. `crashkernel` Seçeneği olabilir sol isterseniz yapılandırılmış, ancak Not küçük VM boyutlarını sorunlu olabilecek bu parametre VM tarafından 128 MB veya daha fazla kullanılabilir bellek miktarını azaltır.

* Azure Linux Aracısı'nı yükleme
  
    Azure Linux Aracısı, Azure Linux görüntüde sağlamak için gereklidir.  Çoğu dağıtımda Aracı (paket genellikle 'WALinuxAgent' veya 'walinuxagent' olarak adlandırılır) bir RPM ya da Deb paketi girin.  Aracı da el ile adımları izleyerek yüklenebilir [Linux Aracısı Kılavuzu](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

* SSH sunucusu yüklü ve önyükleme sırasında başlatılacak şekilde yapılandırılmış olduğundan emin olun.  Bu genellikle varsayılan seçenektir.

* Takas alanı işletim sistemi diski oluşturma
  
    Azure Linux Aracısı'nı otomatik olarak takas alanı Azure üzerinde sağladıktan sonra VM'ye bağlı yerel kaynak diski kullanarak yapılandırabilirsiniz. Yerel kaynak disk Not bir *geçici* disk ve VM sağlaması kaldırılıyor. sağlaması zaman boşaltılabilir. Azure Linux Aracısı'nı yükledikten sonra (önceki adıma bakın), /etc/waagent.conf aşağıdaki parametrelerinde uygun şekilde değiştirin:
  
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.

* Son adım olarak, sanal makine yetkisini kaldırma için aşağıdaki komutları çalıştırın:
  
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
  
  > [!NOTE]
  > Üzerinde VirtualBox çalıştırdıktan sonra aşağıdaki hatayı görebilirsiniz ' waagent-force - deprovision': `[Errno 5] Input/output error`. Bu hata iletisini kritik değildir ve göz ardı edilebilir.
  > 
  > 

* Ardından, sanal makineyi kapatın ve VHD Azure'a yüklemeniz gerekir.

