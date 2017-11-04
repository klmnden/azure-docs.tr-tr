---
title: "Linux çalıştıran bir sanal makinede LVM yapılandırma | Microsoft Docs"
description: "Linux Azure üzerinde LVM yapılandırmayı öğrenin."
services: virtual-machines-linux
documentationcenter: na
author: szarkos
manager: timlt
editor: tysonn
tag: azure-service-management,azure-resource-manager
ms.assetid: 7f533725-1484-479d-9472-6b3098d0aecc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 7926627aaa3f0da935131f491d927ab5cb4b35c9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-lvm-on-a-linux-vm-in-azure"></a>Azure'da bir Linux VM LVM yapılandırın
Bu belge, Azure sanal makinenizde mantıksal Birimi Yöneticisi (LVM) yapılandırmak nasıl ele alınacaktır. Sanal makineye bağlı bir diskte LVM yapılandırmak için uygun olsa da, varsayılan olarak işletim sistemi disk üzerinde yapılandırılmış LVM çoğu bulut görüntüleri sahip olmaz. Bu işletim sistemi diski şimdiye kadar aynı dağıtım ve türü, başka bir VM yani sırasında kurtarma senaryosunda bağlıysa, yinelenen birim grupları ile sorunları önlemek için yapılır. Bu nedenle yalnızca veri disklerde LVM kullanmak için önerilir.

## <a name="linear-vs-striped-logical-volumes"></a>Doğrusal şeritli mantıksal birimler karşılaştırması
LVM fiziksel disk sayısını bir tek bir depolama birimine yerleştirilebilmesi birleştirmek için kullanılabilir. Varsayılan olarak LVM genellikle doğrusal mantıksal birimler, fiziksel depolama alanı birlikte birleştirilmiş yani oluşturur. Bu durumda okuma/yazma işlemleri genellikle yalnızca tek bir diske gönderilir. Buna karşılık, biz de okuma ve yazma işlemleri (yani RADI0 için benzer) birim grubu içinde yer alan birden fazla diske dağıtıldığı şeritli mantıksal birimler oluşturabilirsiniz. Olası performans nedenleriyle böylece okuma ve yazma işlemleri tüm eklenen veri disklerini kullanan, mantıksal birimleri şeritler isteyeceksiniz.

Bu belge, tek bir birimde grubuna birkaç veri diski birleştirmek ve şeritli mantıksal birim oluşturmak nasıl anlatmaktadır. Çoğu dağıtımları ile çalışmak için aşağıdaki adımları biraz genelleştirilmiş. Çoğu durumda, Azure üzerinde LVM yönetmek için iş akışları ve yardımcı programları diğer ortamlara temelde farklı değildir. Her zamanki gibi ayrıca Linux satıcınıza için lütfen belgeleri ve LVM belirli dağıtımınız ile kullanmak için en iyi uygulamalar danışın.

## <a name="attaching-data-disks"></a>Veri diskleri ekleme
Bir genellikle iki veya daha fazla boş veri disklerle LVM kullanırken başlamak isteyeceksiniz. G/ç gereksinimlerinize bağlı olarak, en fazla 500 GÇ/ps disk veya bizim Premium storage başına disk başına en fazla 5000 GÇ/ps ile bizim standart depolamada depolanan diskleri ekleme seçebilirsiniz. Bu makalede ayrıntıya sağlamak ve veri diskleri için Linux sanal makine ekleme konusunda geçer değil. Lütfen Microsoft Azure makalesine bakın [bir diski kullanıma açın](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Azure Linux sanal makinede boş veri diski ekleme konusunda ayrıntılı yönergeler için.

## <a name="install-the-lvm-utilities"></a>LVM yardımcı programlarını yükleyin
* **Ubuntu**

    ```bash  
    sudo apt-get update
    sudo apt-get install lvm2
    ```

* **RHEL, CentOS ve Oracle Linux**

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

    Ayrıca düzenlemelisiniz üzerinde SLES11 `/etc/sysconfig/lvm` ve `LVM_ACTIVATED_ON_DISCOVERED` "etkinleştirmek için":

    ```sh   
    LVM_ACTIVATED_ON_DISCOVERED="enable" 
    ```

## <a name="configure-lvm"></a>LVM'yi yapılandırma
Bu kılavuzda olarak adlandırılan üç veri diskleri ekli varsayacağız `/dev/sdc`, `/dev/sdd` ve `/dev/sde`. Bunlar her zaman aynı yol adları, VM'deki olabileceğini unutmayın. Çalıştırabilirsiniz '`sudo fdisk -l`' ya da kullanılabilir disklerinizi listelemek için benzer komutu.

1. Fiziksel birimler hazırlayın:

    ```bash    
    sudo pvcreate /dev/sd[cde]
    Physical volume "/dev/sdc" successfully created
    Physical volume "/dev/sdd" successfully created
    Physical volume "/dev/sde" successfully created
    ```

2. Bir birim grubu oluşturun. Bu örnekte, biz birim grubu aradığınız `data-vg01`:

    ```bash    
    sudo vgcreate data-vg01 /dev/sd[cde]
    Volume group "data-vg01" successfully created
    ```

3. Mantıksal birim oluşturun. Biz aşağıdaki komutunu adlı tek bir mantıksal birim oluşturacak `data-lv01` tüm birim grubu span, ancak bunu da birim grubunda birden çok mantıksal birim oluşturmak için uygun olduğunu unutmayın.

    ```bash   
    sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
    Logical volume "data-lv01" created.
    ```

4. Mantıksal birimi biçimlendirme

    ```bash  
    sudo mkfs -t ext4 /dev/data-vg01/data-lv01
    ```
   
   > [!NOTE]
   > SLES11 kullanımıyla `-t ext3` ext4 yerine. SLES11 yalnızca ext4 bağlanan dosya sistemlerinin salt okunur erişimi de destekler.

## <a name="add-the-new-file-system-to-etcfstab"></a>Yeni dosya sistemi için /etc/fstab Ekle
> [!IMPORTANT]
> Yanlış bir şekilde düzenleyerek `/etc/fstab` dosya önyüklenemez bir sisteme neden. Emin değilseniz, Lütfen doğru bu dosyayı düzenlemek hakkında bilgi için dağıtım 's belgelerine bakın. Ayrıca bir yedeğini önerilen `/etc/fstab` dosyasını düzenlemeden önce oluşturulur.

1. Örneğin, yeni bir dosya sistemi için istenen bağlama noktası oluşturun:

    ```bash  
    sudo mkdir /data
    ```

2. Mantıksal birim yolunu bulun

    ```bash    
    lvdisplay
    --- Logical volume ---
    LV Path                /dev/data-vg01/data-lv01
    ....
    ```

3. Açık `/etc/fstab` bir metin düzenleyicisinde ve örneğin yeni dosya sistemi için bir giriş ekleyin:

    ```bash    
    /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2
    ```   
    Ardından, Kaydet ve Kapat `/etc/fstab`.

4. Test `/etc/fstab` giriştir doğru:

    ```bash    
    sudo mount -a
    ```

    Bu komutu bir hata iletisi sonuçlanırsa Lütfen sözdizimi iade `/etc/fstab` dosya.
   
    Sonraki çalıştırma `mount` komutu dosya sistemi takılı emin olun:

    ```bash    
    mount
    ......
    /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)
    ```

5. (İsteğe bağlı) Hatasız önyükleme parametrelerinde`/etc/fstab`
   
    Çoğu dağıtımda ya da dahil `nobootwait` veya `nofail` bağlama eklenebilir parametreleri `/etc/fstab` dosya. Bu parametreler belirli dosya sistemi bağlanması gerektiğinde hataları için izin ve Linux sistemin düzgün RAID dosya sistemi bağlama alamıyor olsa bile önyüklemeye devam etmesini izin ver. Lütfen bu parametreleri hakkında daha fazla bilgi için dağıtım 's belgelerine bakın.
   
    Örnek (Ubuntu):

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
    ```

## <a name="trimunmap-support"></a>KIRPMA/UNMAP desteği
Bazı Linux tekrar disk üzerindeki kullanılmayan blokları atmak için KIRPMA/UNMAP işlemleri desteklemez. Bu işlemler sayfaları silinmiş Azure artık geçerli değil ve iptal edilecek bildirmek için standart depolama öncelikle faydalıdır. Büyük dosyaları oluşturmak ve bunları silerseniz sayfaları atılıyor maliyet kaydedebilirsiniz.

KIRPMA etkinleştirmenin iki yolu desteği, Linux VM'NİZDE vardır. Her zamanki gibi dağıtımınız için önerilen yaklaşım bakın:

- Kullanım `discard` bağlama seçeneği `/etc/fstab`, örneğin:

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,discard  0  2
    ```

- Bazı durumlarda `discard` seçeneği performans etkileri olabilir. Alternatif olarak, çalıştırabilirsiniz `fstrim` komutunu el ile komut satırından veya düzenli olarak çalışacak şekilde crontab için ekleyin:

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
