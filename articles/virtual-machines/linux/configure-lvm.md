---
title: Linux çalıştıran bir sanal makinede LVM'yi yapılandırma | Microsoft Docs
description: Azure'da Linux üzerinde LVM'yi yapılandırma konusunda bilgi edinin.
services: virtual-machines-linux
documentationcenter: na
author: szarkos
manager: jeconnoc
editor: tysonn
tag: azure-service-management,azure-resource-manager
ms.assetid: 7f533725-1484-479d-9472-6b3098d0aecc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 09/27/2018
ms.author: szark
ms.subservice: disks
ms.openlocfilehash: 08f98775360b8c0a82f68f322053cb71f0e79af3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60739089"
---
# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>Azure'da Linux sanal makinesi üzerinde LVM'yi yapılandırma
Bu belge, Azure sanal makineler'de mantıksal birim Yöneticisi (LVM) yapılandırma işlemi ele alınmaktadır. LVM'yi işletim sistemi diski veya veri diskleri Azure sanal makinelerinde kullanılabilir, ancak varsayılan olarak çoğu bulut görüntü işletim sistemi diskinde yapılandırılmış LVM olmaz. Aşağıdaki adımlar, veri diskleri için LVM'yi yapılandırma üzerinde odaklanır.

## <a name="linear-vs-striped-logical-volumes"></a>Şeritli mantıksal birimler ve doğrusal
LVM'yi bir tek bir depolama birimine fiziksel disk sayısını birleştirmek için kullanılabilir. Varsayılan olarak LVM genellikle doğrusal mantıksal birimler, yani fiziksel depolama alanı birleştirilmesinden oluşturacaksınız. Bu durumda okuma/yazma işlemleri genellikle yalnızca tek bir diske gönderilir. Buna karşılık, okuma ve yazma işlemleri (RADI0 için benzer) birim grubu içindeki birden fazla diske dağıtıldığı şeritli mantıksal birimleri de oluşturabiliriz. Performansla ilgili nedenlerden dolayı böylece tüm bağlı veri diskleri okuma ve yazma işlemleri kullanmak, mantıksal birimleri stripe istediğiniz olasıdır.

Bu belge, bir tek birim grupta birkaç veri diskleri birleştiren ve Bölüştürülmüş bir mantıksal birim oluşturmak nasıl anlatmaktadır. Aşağıdaki adımlar, çoğu dağıtımları ile çalışmak için genelleştirilmiş. Çoğu durumda, azure'da LVM yönetmek için iş akışları ve yardımcı programlar diğer ortamlara tamamen farklı değildir. Her zamanki şekilde belgeleri ve LVM, belirli bir dağıtım ile kullanmak için en iyi uygulamalar için ayrıca Linux satıcınıza başvurun.

## <a name="attaching-data-disks"></a>Veri diski ekleme
Bir genellikle iki veya daha fazla boş veri diskleri ile LVM kullanırken başlamak isteyeceksiniz. GÇ gereksinimlerinize bağlı olarak, 500'e kadar GÇ/ps her disk veya Premium depolama disk başına en fazla 5000 GÇ/ps ile ile standart depolama içinde depolanan diski seçebilirsiniz. Bu makalede ayrıntıya sağlamak ve bir Linux sanal makinesine veri diski nasıl geçer değil. Microsoft Azure makaleye göz atın [bir diski](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) azure'da Linux sanal makinesi için bir boş veri diski ekleme konusunda ayrıntılı yönergeler için.

## <a name="install-the-lvm-utilities"></a>LVM'yi yardımcı programlarını yükleyin
* **Ubuntu**

    ```bash  
    sudo apt-get update
    sudo apt-get install lvm2
    ```

* **RHEL, CentOS & Oracle Linux**

    ```bash   
    sudo yum install lvm2
    ```

* **SLES 12 ve openSUSE**

    ```bash   
    sudo zypper install lvm2
    ```

* **SLES 11**

    ```bash   
    sudo zypper install lvm2
    ```

    Ayrıca düzenlemelisiniz SLES11 üzerinde `/etc/sysconfig/lvm` ayarlayıp `LVM_ACTIVATED_ON_DISCOVERED` "etkinleştirmek için":

    ```sh   
    LVM_ACTIVATED_ON_DISCOVERED="enable" 
    ```

## <a name="configure-lvm"></a>LVM'yi yapılandırma
Bu kılavuzda bağlı, biz olarak başvuracağınız üç veri diskleri varsayacağız `/dev/sdc`, `/dev/sdd` ve `/dev/sde`. Bu yollar sanal disk yolu adları eşleşmiyor olabilir. Çalıştırabileceğiniz '`sudo fdisk -l`' veya kullanılabilir disklerinizi listelemek için benzer komutu.

1. Fiziksel birimler hazırlayın:

    ```bash    
    sudo pvcreate /dev/sd[cde]
    Physical volume "/dev/sdc" successfully created
    Physical volume "/dev/sdd" successfully created
    Physical volume "/dev/sde" successfully created
    ```

2. Bir birim grubu oluşturun. Bu örnekte biz birim grubu aradığınız `data-vg01`:

    ```bash    
    sudo vgcreate data-vg01 /dev/sd[cde]
    Volume group "data-vg01" successfully created
    ```

3. Mantıksal birim oluşturun. Biz aşağıdaki komutta adlı tek bir mantıksal birim oluşturur `data-lv01` birimin tamamını grubu span, ancak bunu Ayrıca toplu grubunda birden çok mantıksal birim oluşturmak için uygun olduğunu unutmayın.

    ```bash   
    sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
    Logical volume "data-lv01" created.
    ```

4. Mantıksal birimi biçimlendirin

    ```bash  
    sudo mkfs -t ext4 /dev/data-vg01/data-lv01
    ```
   
   > [!NOTE]
   > SLES11 kullanımıyla `-t ext3` ext4 yerine. SLES11 yalnızca ext4 dosya sistemleri için yalnızca okuma erişimi destekler.

## <a name="add-the-new-file-system-to-etcfstab"></a>Yeni dosya sistemi için /etc/fstab Ekle
> [!IMPORTANT]
> Yanlış düzenleme `/etc/fstab` dosya yapılamamasına bir sistemde neden olabilir. Emin değilseniz, düzgün bir şekilde bu dosya düzenleme hakkında daha fazla bilgi için ait dağıtım belgelerine bakın. Ayrıca, önerilen bir yedeğini `/etc/fstab` dosyasını düzenlemeden önce oluşturulur.

1. Örneğin, yeni bir dosya sistemi için istenen bağlama noktası oluşturun:

    ```bash  
    sudo mkdir /data
    ```

2. Mantıksal birimi yolu bulun

    ```bash    
    lvdisplay
    --- Logical volume ---
    LV Path                /dev/data-vg01/data-lv01
    ....
    ```

3. Açık `/etc/fstab` bir metin düzenleyicisinde ve örneğin yeni bir dosya sistemi için bir giriş ekleyin:

    ```bash    
    /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2
    ```   
    Ardından, kaydedin ve kapatın `/etc/fstab`.

4. Test `/etc/fstab` girdidir doğru:

    ```bash    
    sudo mount -a
    ```

    Bu komut bir hata iletisi sonuçlanırsa sözdizimi iade `/etc/fstab` dosya.
   
    Sonraki çalıştırma `mount` dosya sistemi monte emin olmak için komut:

    ```bash    
    mount
    ......
    /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)
    ```

5. (İsteğe bağlı) Hatasız önyükleme parametreleri `/etc/fstab`
   
    Çoğu dağıtımda ya da dahil `nobootwait` veya `nofail` bağlama eklenebilir parametreleri `/etc/fstab` dosya. Bu parametreleri hataları için belirli bir dosya sistemi bağlarken ve düzgün şekilde RAID dosya sistemi monte etmesini yüklenemiyor olsa bile önyüklenecek şekilde devam etmek Linux sistem verin. Bu parametreler hakkında daha fazla bilgi için dağıtımınıza ait belgelere bakın.
   
    Örnek (Ubuntu):

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
    ```

## <a name="trimunmap-support"></a>TRIM/UNMAP desteği
Bazı Linux çekirdeklerinin diskte kullanılmayan blokları atmak TRIM/UNMAP işlemleri destekler. Bu işlemler sayfaları silinmiş Azure artık geçerli değil ve atılabilir bilgilendirmek için standart depolama alanında birincil yararlıdır. Büyük dosyaları oluşturup ardından bunları silerseniz sayfaları atılıyor maliyetinden tasarruf ettirebilir.

TRIM etkinleştirmek için iki şekilde destek Linux VM'nize vardır. Her zamanki şekilde dağıtımınız için önerilen yaklaşım bakın:

- Kullanım `discard` bağlama seçeneği `/etc/fstab`, örneğin:

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,discard  0  2
    ```

- Bazı durumlarda `discard` seçeneği performans etkileri olabilir. Alternatif olarak, çalıştırabileceğiniz `fstrim` komutunu el ile komut satırından veya düzenli olarak çalıştırmak için crontab ekleyin:

    **Ubuntu**

    ```bash 
    # sudo apt-get install util-linux
    # sudo fstrim /datadrive
    ```

    **RHEL/CentOS**

    ```bash 
    # sudo yum install util-linux
    # sudo fstrim /datadrive
    ```
