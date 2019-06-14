---
title: Yazılım RAID Linux çalıştıran bir sanal makinede yapılandırma | Microsoft Docs
description: Mdadm Azure'da Linux üzerinde RAID yapılandırmak için kullanmayı öğrenin.
services: virtual-machines-linux
documentationcenter: na
author: rickstercdn
manager: jeconnoc
editor: tysonn
tag: azure-service-management,azure-resource-manager
ms.assetid: f3cb2786-bda6-4d2c-9aaf-2db80f490feb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: rclaus
ms.subservice: disks
ms.openlocfilehash: e773fdcb031f0f8f896ea40d76231fd54a603dc4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60328808"
---
# <a name="configure-software-raid-on-linux"></a>Linux’ta Yazılım RAID yapılandırma
Birden fazla bağlı veri diskleri tek bir RAID cihaz sunmak için Azure'da Linux sanal makinelerinde RAID yazılım kullanmak yaygın bir senaryodur. Genellikle bu performansı artırmak ve yalnızca tek bir diske kullanmaya kıyasla iyi aktarım hızı için izin vermek için kullanılabilir.

## <a name="attaching-data-disks"></a>Veri diski ekleme
İki veya daha fazla boş veri diskleri, bir RAID cihaz yapılandırmak için gereklidir.  Disk g/ç performansını artırmak için bir RAID cihaz oluşturmak için birincil nedeni olmasıdır.  GÇ gereksinimlerinize bağlı olarak, 500'e kadar GÇ/ps her disk veya Premium depolama disk başına en fazla 5000 GÇ/ps ile ile standart depolama içinde depolanan diski seçebilirsiniz. Bu makalede, sağlamak ve bir Linux sanal makinesine veri diski konusunda ayrıntıya geçmez.  Microsoft Azure makaleye göz atın [bir diski](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) azure'da Linux sanal makinesi için bir boş veri diski ekleme konusunda ayrıntılı yönergeler için.

## <a name="install-the-mdadm-utility"></a>Mdadm yardımcı programını yükleyin
* **Ubuntu**
  ```bash
  sudo apt-get update
  sudo apt-get install mdadm
  ```

* **CentOS & Oracle Linux**
  ```bash
  sudo yum install mdadm
  ```

* **SLES ve openSUSE**
  ```bash  
  zypper install mdadm
  ```

## <a name="create-the-disk-partitions"></a>Disk bölümleri oluşturma
Bu örnekte, /dev/sdc üzerinde tek bir diskin oluştururuz. Yeni disk bölümü /dev/sdc1 çağrılmaz.

1. Başlangıç `fdisk` bölümleri oluşturmaya başlamak için

    ```bash
    sudo fdisk /dev/sdc
    Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
    Building a new DOS disklabel with disk identifier 0xa34cb70c.
    Changes will remain in memory only, until you decide to write them.
    After that, of course, the previous content won't be recoverable.

    WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
                    switch off the mode (command 'c') and change display units to
                    sectors (command 'u').
    ```

1. Tuşuna 'N' sayıda oluşturmak için komut istemine bir **n**eni bölüm:

    ```bash
    Command (m for help): n
    ```

1. Ardından, oluşturmak için ' p tuşlarına basın. bir **p**birincil bölüm:

    ```bash 
    Command action
            e   extended
            p   primary partition (1-4)
    ```

1. Bölüm numarası 1'i seçmek için '1' e basın:

    ```bash
    Partition number (1-4): 1
    ```

1. Başlangıç noktası basın veya yeni bir bölüm seçin `<enter>` sürücüdeki boş alan başına bölüm yerleştirmek için Varsayılanı kabul edin:

    ```bash   
    First cylinder (1-1305, default 1):
    Using default value 1
    ```

1. Bölümün boyutunu seçin, örneğin bir 10 gigabayt bölüm oluşturmak için ' +10G' yazın. Veya basın `<enter>` sürücünün tamamını kapsayan tek bir bölüm oluşturun:

    ```bash   
    Last cylinder, +cylinders or +size{K,M,G} (1-1305, default 1305): 
    Using default value 1305
    ```

1. Ardından, Kimliğini değiştirme ve **t**türü Bölümü '83' varsayılan kimliği (Linux) kimliği 'fd' (Linux RAID otomatik):

    ```bash  
    Command (m for help): t
    Selected partition 1
    Hex code (type L to list codes): fd
    ```

1. Son olarak, bölümleme tablosu yazmaya ve fdisk çıkın:

    ```bash   
    Command (m for help): w
    The partition table has been altered!
    ```

## <a name="create-the-raid-array"></a>RAID dizi oluşturma
1. Aşağıdaki örnek olacak "stripe üç ayrı veri disklerinde (sdc1, sdd1, sde1) üç bölüm yer alan" (RAID düzeyi 0).  Adlı yeni bir RAID cihaz bu komutu çalıştırdıktan sonra **/dev/md127** oluşturulur. Ayrıca bu veri diskleri, biz daha önce başka bir işlevsiz RAID dizi parçası bunu eklemek gerekli olabileceğini unutmayın `--force` parametresi `mdadm` komutu:

    ```bash  
    sudo mdadm --create /dev/md127 --level 0 --raid-devices 3 \
        /dev/sdc1 /dev/sdd1 /dev/sde1
    ```

1. Yeni bir RAID cihaz üzerinde dosya sistemi oluşturun
   
    a. **CentOS, Oracle Linux, SLES 12, openSUSE ve Ubuntu**

    ```bash   
    sudo mkfs -t ext4 /dev/md127
    ```
   
    b. **SLES 11**

    ```bash
    sudo mkfs -t ext3 /dev/md127
    ```
   
    c. **SLES 11** - boot.md etkinleştirmek ve mdadm.conf oluşturma

    ```bash
    sudo -i chkconfig --add boot.md
    sudo echo 'DEVICE /dev/sd*[0-9]' >> /etc/mdadm.conf
    ```
   
   > [!NOTE]
   > SUSE sistemlerinde bu değişiklikleri yaptıktan sonra bir yeniden başlatma gerekli olabilir. Bu adım *değil* SLES 12 gerekli.
   > 
   > 

## <a name="add-the-new-file-system-to-etcfstab"></a>Yeni dosya sistemi için /etc/fstab Ekle
> [!IMPORTANT]
> Yanlış /etc/fstab dosyayı düzenlemeye yapılamamasına bir sistemde neden olabilir. Emin değilseniz, düzgün bir şekilde bu dosya düzenleme hakkında daha fazla bilgi için ait dağıtım belgelerine bakın. Ayrıca düzenlemeden önce /etc/fstab dosyasının yedek bir kopyası oluşturulur önerilir.

1. Örneğin, yeni bir dosya sistemi için istenen bağlama noktası oluşturun:

    ```bash
    sudo mkdir /data
    ```
1. /Etc/fstab, düzenlerken **UUID** cihaz adı yerine dosya sistemine başvurmak için kullanılmalıdır.  Kullanım `blkid` yeni dosya sistemi UUID'si belirlemek için yardımcı program:

    ```bash   
    sudo /sbin/blkid
    ...........
    /dev/md127: UUID="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" TYPE="ext4"
    ```

1. /Etc/fstab bir metin düzenleyicisinde açın ve örneğin yeni bir dosya sistemi için bir giriş ekleyin:

    ```bash   
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults  0  2
    ```
   
    Veya **SLES 11**:

    ```bash
    /dev/disk/by-uuid/aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext3  defaults  0  2
    ```
   
    Ardından, kaydedin ve /etc/fstab kapatın.

1. / Etc/fstab girişin doğru olduğunu sınayın:

    ```bash  
    sudo mount -a
    ```

    Bu komut bir hata iletisi olursa, lütfen /etc/fstab dosyasındaki sözdizimini denetleyin.
   
    Sonraki çalıştırma `mount` dosya sistemi monte emin olmak için komut:

    ```bash   
    mount
    .................
    /dev/md127 on /data type ext4 (rw)
    ```

1. (İsteğe bağlı) Hatasız önyükleme parametreleri
   
    **fstab yapılandırma**
   
    Çoğu dağıtımda ya da dahil `nobootwait` veya `nofail` bağlama parametreleri/etc/fstab dosyasına eklenebilir. Bu parametreleri hataları için belirli bir dosya sistemi bağlarken ve düzgün şekilde RAID dosya sistemi monte etmesini yüklenemiyor olsa bile önyüklenecek şekilde devam etmek Linux sistem verin. Bu parametreler hakkında daha fazla bilgi için dağıtımınıza ait belgelere bakın.
   
    Örnek (Ubuntu):

    ```bash  
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,nobootwait  0  2
    ```   

    **Linux önyükleme parametreleri**
   
    Yukarıdaki parametreleri, çekirdek parametresi yanı sıra "`bootdegraded=true`" bile sanal makineden bir veri sürücüsü yanlışlıkla kaldırdıysanız RAID zarar görmüş veya düşürülmüş için örnek olarak algılanan önyükleme sisteme izin verebilirsiniz. Varsayılan olarak bu önyüklenebilir olmayan sistem sonuçlanabilir.
   
    Çekirdek parametrelerini düzgün şekilde düzenlemek nasıl dağıtımınıza ait belgelere bakın. Örneğin, çoğu dağıtımda (CentOS, Oracle Linux, SLES 11) Bu parametreleri el ile geçirmek eklenebilir "`/boot/grub/menu.lst`" dosya.  Ubuntu üzerinde bu parametre için eklenebilir `GRUB_CMDLINE_LINUX_DEFAULT` değişken üzerinde "/ varsayılan/etc/grub".


## <a name="trimunmap-support"></a>TRIM/UNMAP desteği
Bazı Linux çekirdeklerinin diskte kullanılmayan blokları atmak TRIM/UNMAP işlemleri destekler. Bu işlemler sayfaları silinmiş Azure artık geçerli değil ve atılabilir bilgilendirmek için standart depolama alanında birincil yararlıdır. Büyük dosyaları oluşturup ardından bunları silerseniz sayfaları atılıyor maliyetinden tasarruf ettirebilir.

> [!NOTE]
> (512 KB) varsayılan öbek boyutu dizi ayarlarsanız RAID atma komutları gönderemezsiniz. Konaktaki unmap ayrıntı da 512 KB olmasıdır. Dizinin öbek boyutu mdadm'ın aracılığıyla değiştirilen `--chunk=` parametresi ve KIRPMA/unmap istekleri çekirdek tarafından dikkate.

TRIM etkinleştirmek için iki şekilde destek Linux VM'nize vardır. Her zamanki şekilde dağıtımınız için önerilen yaklaşım bakın:

- Kullanım `discard` bağlama seçeneği `/etc/fstab`, örneğin:

    ```bash
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,discard  0  2
    ```

- Bazı durumlarda `discard` seçeneği performans etkileri olabilir. Alternatif olarak, çalıştırabileceğiniz `fstrim` komutunu el ile komut satırından veya düzenli olarak çalıştırmak için crontab ekleyin:

    **Ubuntu**

    ```bash
    # sudo apt-get install util-linux
    # sudo fstrim /data
    ```

    **RHEL/CentOS**
    ```bash
    # sudo yum install util-linux
    # sudo fstrim /data
    ```
