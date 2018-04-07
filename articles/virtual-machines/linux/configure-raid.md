---
title: Yazılım RAID Linux çalıştıran bir sanal makinede yapılandırmak | Microsoft Docs
description: Mdadm Azure'da RAID Linux'ta yapılandırmak için nasıl kullanılacağını öğrenin.
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
ms.openlocfilehash: d6e831692da37645e264c6674f1ba54bb16d25d4
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="configure-software-raid-on-linux"></a>Linux’ta Yazılım RAID yapılandırma
Yazılım RAID Linux sanal makinelerde tek bir RAID aygıt olarak birden çok eklenen veri disklerini sunmak için Azure içinde kullanmak için ortak bir senaryodur. Genellikle bu performansı artırmak ve yalnızca tek bir disk kullanmaya kıyasla geliştirilmiş işleme için izin vermek için kullanılabilir.

## <a name="attaching-data-disks"></a>Veri diskleri ekleme
İki veya daha fazla boş veri diskler, RAID aygıtı yapılandırmak için gereklidir.  Disk GÇ performansı artırmak için bir RAID aygıtı oluşturmak için birincil nedeni olmasıdır.  G/ç gereksinimlerinize bağlı olarak, en fazla 500 GÇ/ps disk veya bizim Premium storage başına disk başına en fazla 5000 GÇ/ps ile bizim standart depolamada depolanan diskleri ekleme seçebilirsiniz. Bu makalede, sağlamak ve veri diskleri için Linux sanal makine ekleme konusunda ayrıntıya geçmez.  Microsoft Azure makalesine bakın [bir diski kullanıma açın](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Azure Linux sanal makinede boş veri diski ekleme konusunda ayrıntılı yönergeler için.

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
Bu örnekte, /dev/sdc üzerinde tek disk bölümü oluşturuyoruz. Yeni disk bölümü /dev/sdc1 çağrılır.

1. Başlat `fdisk` bölümleri oluşturmaya başlamak için

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

2. Tuşuna 'n' oluşturmak için komut istemine bir **n**eni bölüm:

    ```bash
    Command (m for help): n
    ```

3. Ardından, oluşturmak için ' p' tuşuna basın. bir **p**birincil bölüm:

    ```bash 
    Command action
            e   extended
            p   primary partition (1-4)
    ```

4. Bölüm numarası 1 seçmek için '1' tuşuna basın:

    ```bash
    Partition number (1-4): 1
    ```

5. Yeni bölüm veya tuşuna başlangıç noktasını seçin `<enter>` sürücüdeki boş alan başına bölüm yerleştirmek için Varsayılanı kabul etmek için:

    ```bash   
    First cylinder (1-1305, default 1):
    Using default value 1
    ```

6. Bölümün boyutunu seçin, örneğin bir 10 gigabayt bölüm oluşturmak için ' +10G' yazın. Veya basın `<enter>` sürücünün tamamını kapsayan tek bir bölüm oluşturun:

    ```bash   
    Last cylinder, +cylinders or +size{K,M,G} (1-1305, default 1305): 
    Using default value 1305
    ```

7. Ardından, Tanıtıcıyı değiştirin ve **t**türü bölümün varsayılan Kimliği '83' nden (Linux) kimliği 'fd' (Linux RAID otomatik):

    ```bash  
    Command (m for help): t
    Selected partition 1
    Hex code (type L to list codes): fd
    ```

8. Son olarak, bölümleme tablosu diske yazma ve fdisk Çık:

    ```bash   
    Command (m for help): w
    The partition table has been altered!
    ```

## <a name="create-the-raid-array"></a>RAID dizisi oluşturma
1. Aşağıdaki örnek "stripe üç bölüm üç ayrı veri disk üzerinde (sdc1, sdd1, sde1) bulunan" (RAID Düzey 0).  Adlı yeni bir RAID cihaz bu komutu çalıştırdıktan sonra **/dev/md127** oluşturulur. Ayrıca bu veri diskleri, biz daha önce başka bir geçersiz RAID dizisi parçası onu eklemek için gereken olabileceğine dikkat edin `--force` parametresi `mdadm` komutu:

    ```bash  
    sudo mdadm --create /dev/md127 --level 0 --raid-devices 3 \
        /dev/sdc1 /dev/sdd1 /dev/sde1
    ```

2. Dosya sistemi yeni RAID cihazda oluştur
   
    a. **CentOS, Oracle Linux SLES 12, openSUSE ve Ubuntu**

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
   > SUSE sistemlerde bu değişiklikleri yaptıktan sonra bir yeniden başlatma gerekli olabilir. Bu adım *değil* SLES 12 gerekli.
   > 
   > 

## <a name="add-the-new-file-system-to-etcfstab"></a>Yeni dosya sistemi için /etc/fstab Ekle
> [!IMPORTANT]
> Yanlış /etc/fstab dosyasını düzenleyerek önyüklenemez bir sisteme neden olabilir. Emin değilseniz, düzgün şekilde bu dosyayı düzenlemek hakkında bilgi için dağıtım 's belgelerine bakın. Ayrıca, düzenlemeye başlamadan önce /etc/fstab dosyanızın bir yedeğini oluşturduğunuz önerilir.

1. Örneğin, yeni bir dosya sistemi için istenen bağlama noktası oluşturun:

    ```bash
    sudo mkdir /data
    ```
2. /Etc/fstab, düzenlerken **UUID** aygıt adı yerine dosya sistemine başvurmak için kullanılmalıdır.  Kullanım `blkid` yeni dosya sistemi için UUID belirlemeye yardımcı programı:

    ```bash   
    sudo /sbin/blkid
    ...........
    /dev/md127: UUID="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" TYPE="ext4"
    ```

3. /Etc/fstab bir metin düzenleyicisinde açın ve yeni dosya sistemi için bir giriş örneğin ekleyin:

    ```bash   
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults  0  2
    ```
   
    Veya **SLES 11**:

    ```bash
    /dev/disk/by-uuid/aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext3  defaults  0  2
    ```
   
    Ardından, kaydedin ve /etc/fstab kapatın.

4. / Etc/fstab girişin doğru olduğunu sınayın:

    ```bash  
    sudo mount -a
    ```

    Bu komutu bir hata iletisi sonuçlanırsa, lütfen /etc/fstab dosyasında sözdizimini denetleyin.
   
    Sonraki çalıştırma `mount` komutu dosya sistemi takılı emin olun:

    ```bash   
    mount
    .................
    /dev/md127 on /data type ext4 (rw)
    ```

5. (İsteğe bağlı) Hatasız önyükleme parametreleri
   
    **fstab yapılandırma**
   
    Çoğu dağıtımda ya da dahil `nobootwait` veya `nofail` bağlama/etc/fstab dosyasına eklenen parametreleri. Bu parametreler belirli dosya sistemi bağlanması gerektiğinde hataları için izin ve Linux sistemin düzgün RAID dosya sistemi bağlama alamıyor olsa bile önyüklemeye devam etmesini izin ver. Bu parametreler hakkında daha fazla bilgi için dağıtım 's belgelerine bakın.
   
    Örnek (Ubuntu):

    ```bash  
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,nobootwait  0  2
    ```   

    **Linux önyükleme parametreleri**
   
    Yukarıdaki parametreler, çekirdek parametresi ek olarak "`bootdegraded=true`" sanal makineden bir veri sürücüsünü yanlışlıkla kaldırdıysanız RAID zarar görmüş veya düşürülmüş için örnek olarak algılanan olsa bile önyükleme sisteme izin verebilirsiniz. Varsayılan olarak bu önyüklenebilir olmayan bir sistemi sonuçlanabilir.
   
    Lütfen düzgün çekirdek parametrelerini düzenlemek nasıl dağıtım 's belgelerine bakın. Örneğin, çoğu dağıtımda (CentOS, Oracle Linux, SLES 11) Bu parametreleri el ile çok eklenebilir "`/boot/grub/menu.lst`" dosya.  Ubuntu üzerinde bu parametreyi eklenebilir `GRUB_CMDLINE_LINUX_DEFAULT` değişkeninin "/ etc/varsayılan/kaz".


## <a name="trimunmap-support"></a>KIRPMA/UNMAP desteği
Bazı Linux tekrar disk üzerindeki kullanılmayan blokları atmak için KIRPMA/UNMAP işlemleri desteklemez. Bu işlemler sayfaları silinmiş Azure artık geçerli değil ve iptal edilecek bildirmek için standart depolama öncelikle faydalıdır. Büyük dosyaları oluşturmak ve bunları silerseniz sayfaları atılıyor maliyet kaydedebilirsiniz.

> [!NOTE]
> Dizi öbek boyutu (512 KB) varsayılan değerinden ayarlanırsa RAID atma komutları verin değil. Ana bilgisayarda unmap ayrıntı düzeyi de 512 KB olmasıdır. Dizinin öbek boyutu mdadm'ın aracılığıyla değiştirilmiş varsa `--chunk=` parametresi sonra KIRPMA ve eşlemesini istekleri çekirdekten dikkate.

KIRPMA etkinleştirmenin iki yolu desteği, Linux VM'NİZDE vardır. Her zamanki gibi dağıtımınız için önerilen yaklaşım bakın:

- Kullanım `discard` bağlama seçeneği `/etc/fstab`, örneğin:

    ```bash
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,discard  0  2
    ```

- Bazı durumlarda `discard` seçeneği performans etkileri olabilir. Alternatif olarak, çalıştırabilirsiniz `fstrim` komutunu el ile komut satırından veya düzenli olarak çalışacak şekilde crontab için ekleyin:

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
