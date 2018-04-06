---
title: Azure'da bir Linux VM bir disk ekleme | Microsoft Docs
description: Klasik dağıtım modeli kullanarak bir Linux VM için bir veri diski Ekle öğrenin ve kullanıma hazır olacak şekilde diski başlatın
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 4901384d-2a6f-4f46-bba0-337a348b7f87
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: e7f587f6126f60f18bb4c6f184ec58cf7efc1a81
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="how-to-attach-a-data-disk-to-a-linux-virtual-machine"></a>Nasıl bir Linux sanal makineye bir veri diski Ekle
> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Bkz: nasıl yapılır [Resource Manager dağıtım modelini kullanarak bir veri diskini](../add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Boş disk ve Azure Vm'leriniz için veri içeren diskleri ekleyebilirsiniz. Her iki disk türleri için bir Azure depolama hesabında bulunan .vhd dosyalarıdır. Disk ekledikten sonra olarak Linux makine için herhangi bir disk ekleyip, başlatmak ve kullanıma hazır olacak şekilde biçimlendirmeniz gerekir. Bu makale ayrıntıları boş diskleri ve zaten sonra başlatmak ve yeni bir diski biçimlendirmek nasıl yanı sıra Vm'leriniz için verileri içeren diskleri ekleme.

> [!NOTE]
> Bir sanal makine verilerini depolamak için bir veya daha fazla ayrı disk kullanmak iyi bir uygulamadır. Bir Azure sanal makine oluşturduğunuzda, bir işletim sistemi diski ve geçici bir disk vardır. **Geçici disk kalıcı veri depolamak için kullanmayın.** Adından da anlaşılacağı gibi yalnızca geçici depolama sağlar. Azure depolama alanında bulunan değil çünkü hiçbir artıklık veya yedekleme sunar.
> Geçici disk genellikle Azure Linux aracısı tarafından yönetilir ve otomatik olarak bağlanan **/mnt/kaynak** (veya **/mnt** Ubuntu görüntülerinde). Öte yandan, bir veri diski Linux çekirdek tarafından şöyle adlandırılabilir `/dev/sdc`, ve bölüm, biçimlendirmek ve bu kaynak bağlamanız gerekir. Bkz: [Azure Linux Aracısı Kullanıcı Kılavuzu] [ Agent] Ayrıntılar için.
> 
> 

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a>Linux içindeki yeni bir veri diski başlatın
1. SSH, VM. Daha fazla bilgi için bkz: [Linux çalıştıran bir sanal makine için oturum açma][Logon].
2. Sonraki başlatmak için veri diski için cihaz tanımlayıcısı bulmanız gerekir. Bunu yapmak için iki yolu vardır:
   
    bir) Grep günlükleri, aşağıdaki komutu olduğu gibi SCSI aygıtlar için:
   
    ```bash
    sudo grep SCSI /var/log/messages
    ```
   
    Yeni Ubuntu dağıtımları için kullanmanız gerekebilir `sudo grep SCSI /var/log/syslog` oturum çünkü `/var/log/messages` varsayılan olarak devre dışı olabilir.
   
    Görüntülenen iletileri eklendi son veri diski tanıtıcısı bulabilirsiniz.
   
    ![Disk iletileri alma](./media/attach-disk/scsidisklog.png)
   
    OR
   
    b) kullanım `lsscsi` cihaz kimliği bulmak için komutu. `lsscsi` ya da yüklenebilir `yum install lsscsi` (Red Hat üzerinde dağıtımları bağlı olarak) veya `apt-get install lsscsi` (Debian üzerinde dağıtımları bağlı olarak). Tarafından aradığınız disk bulabilirsiniz kendi *lun* veya **mantıksal birim numarası**. Örneğin, *lun* bağlı diskler gelen kolayca görülebilir için `azure vm disk list <virtual-machine>` olarak:

    ```azurecli
    azure vm disk list myVM
    ```

    Çıkış aşağıdakine benzer:

    ```azurecli
    info:    Executing command vm disk list
    + Fetching disk images
    + Getting virtual machines
    + Getting VM disks
    data:    Lun  Size(GB)  Blob-Name                         OS
    data:    ---  --------  --------------------------------  -----
    data:         30        myVM-2645b8030676c8f8.vhd  Linux
    data:    0    100       myVM-76f7ee1ef0f6dddc.vhd
    info:    vm disk list command OK
    ```
   
    Bu veri çıkışı ile karşılaştırmak `lsscsi` aynı sanal makine örneği için:
   
    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```
   
    Her satır düzeninde son sayısıdır *lun*. Bkz: `man lsscsi` daha fazla bilgi için.
3. İsteminde Cihazınızı oluşturmak için aşağıdaki komutu yazın:
   
    ```bash
    sudo fdisk /dev/sdc
    ```

4. İstendiğinde yazın **n** bir bölüm oluşturmak için.

    ![Cihaz oluşturma](./media/attach-disk/fdisknewpartition.png)

5. İstendiğinde yazın **p** bölümü birincil bölüm duruma getirmek için. Tür **1** ilk bölüm ve ardından yazın silindir için varsayılan değeri kabul etmek için enter yapma. Bazı sistemlerinde ilk ve son kesimler, silindir yerine varsayılan değerlerini gösterebilir. Bu varsayılan değerleri kabul etmeyi tercih edebilirsiniz.

    ![Bölümü oluşturma](./media/attach-disk/fdisknewpartdetails.png)


6. Tür **p** bölümlenme şekli disk ayrıntılarını görmek için.

    ![Liste disk bilgileri](./media/attach-disk/fdiskpartitiondetails.png)


7. Tür **w** disk ayarlarını yazmak için.

    ![Disk değişiklikleri yazma](./media/attach-disk/fdiskwritedisk.png)

8. Artık yeni bölüme dosya sistemi oluşturabilirsiniz. Cihaz kimliği bölüm numarası ekleme (aşağıdaki örnekte `/dev/sdc1`). Aşağıdaki örnek, bir ext4 bölüm üzerinde /dev/sdc1 oluşturur:
   
    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
   
    ![Dosya sistemi oluştur](./media/attach-disk/mkfsext4.png)
   
   > [!NOTE]
   > SuSE Linux Enterprise 11 sistemleri yalnızca ext4 dosya sistemleri için salt okunur erişimi de destekler. Bu sistemler için yeni dosya sistemi ext4 yerine ext3 olarak biçimlendirmek için önerilir.

9. Yeni dosya sistemi gibi bağlamak için bir dizin oluşturun:
   
    ```bash
    sudo mkdir /datadrive
    ```

10. Son olarak sürücünün şu şekilde bağlayabilirsiniz:
   
    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```
   
    Veri diski olarak kullanılmak üzere hazırdır **/datadrive**.
   
    ![Dizin oluşturun ve disk bağlama](./media/attach-disk/mkdirandmount.png)

11. Yeni sürücü için /etc/fstab ekleyin:
   
    Sürücünün bir yeniden başlatmadan sonra otomatik olarak yeniden sağlamak için/etc/fstab dosyasına eklenmelidir. Ayrıca, yalnızca cihaz adını (yani /dev/sdc1) yerine sürücü başvurmak için UUID (evrensel benzersiz tanıtıcı) içinde /etc/fstab kullanılır önerilir. İşletim sistemi önyükleme ve ardından bu cihaz kimliklerini atanmasını kalan olan veri disklerinin sırasında disk hatası algılarsa, belirtilen bir konuma bağlı hatalı disk UUID'si kullanılarak önler. Yeni sürücü UUID'si bulmak için kullanabileceğiniz **blkid** yardımcı programı:
   
    ```bash
    sudo -i blkid
    ```
   
    Çıktı aşağıdaki örneğe benzer:
   
    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

    > [!NOTE]
    > Yanlış bir şekilde düzenleyerek **/etc/fstab** dosya önyüklenemez bir sisteme neden. Emin değilseniz, düzgün şekilde bu dosyayı düzenlemek hakkında bilgi için dağıtım 's belgelerine bakın. Ayrıca, düzenlemeye başlamadan önce /etc/fstab dosyanızın bir yedeğini oluşturduğunuz önerilir.

    Ardından, açık **/etc/fstab** dosyasını bir metin düzenleyicisinde:

    ```bash
    sudo vi /etc/fstab
    ```

    Bu örnekte, UUID değeri için yeni kullanırız **/dev/sdc1** oluşturulduğu önceki adımları ve mountpoint aygıt **/datadrive**. Sonuna aşağıdaki satırı ekleyin **/etc/fstab** dosyası:

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
    ```

    Ya da SuSE Linux tabanlı sistemler üzerinde biraz farklı bir biçim kullanmanız gerekebilir:

    ```sh
    /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    ```

    > [!NOTE]
    > `nofail` Seçeneği sağlar VM dosya sistemi bozuk ya da önyükleme sırasında diski yok olsa bile başlatır. Bu seçenek olmadan, davranış açıklandığı gibi karşılaşabilirsiniz [olamaz SSH Linux VM FSTAB hatalar nedeniyle](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).

    Dosya sistemi takılı artık test edebilirsiniz düzgün çıkarma ve dosya sistemi çıkarmadan, yani örneği kullanarak bağlama noktası `/datadrive` önceki adımlarda oluşturduğunuz:

    ```bash
    sudo umount /datadrive
    sudo mount /datadrive
    ```

    Varsa `mount` komutu, bir hata üretir, doğru sözdizimi için/etc/fstab dosyasını denetleyin. Ek veri sürücüleri veya bölümleri oluşturulduğu varsa, bunları/etc/fstab de ayrı olarak girin.

    Sürücü yazılabilir bu komutu kullanarak yapabilirsiniz:

    ```bash
    sudo chmod go+w /datadrive
    ```

    > [!NOTE]
    > Sonradan bir veri diski fstab düzenlemeden kaldırma VM önyükleme başarısız olmasına neden olabilir. Bu sık karşılaşılan bir durumdur, çoğu dağıtımları ya da sağlamanız `nofail` ve/veya `nobootwait` adresindeki bağlamak disk başarısız olsa bile önyükleme sistemi izin fstab seçenekleri önyükleme saati. Bu parametreler hakkında daha fazla bilgi için dağıtım ait belgelere bakın.

### <a name="trimunmap-support-for-linux-in-azure"></a>KIRPMA/UNMAP Azure Linux desteği
Bazı Linux tekrar disk üzerindeki kullanılmayan blokları atmak için KIRPMA/UNMAP işlemleri desteklemez. Bu işlemler sayfaları silinmiş Azure artık geçerli değil ve iptal edilecek bildirmek için standart depolama öncelikle faydalıdır. Büyük dosyaları oluşturmak ve bunları silerseniz sayfaları atılıyor maliyet kaydedebilirsiniz.

KIRPMA etkinleştirmenin iki yolu desteği, Linux VM'NİZDE vardır. Her zamanki gibi dağıtımınız için önerilen yaklaşım bakın:

* Kullanım `discard` bağlama seçeneği `/etc/fstab`, örneğin:

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```

* Bazı durumlarda `discard` seçeneği performans etkileri olabilir. Alternatif olarak, çalıştırabilirsiniz `fstrim` komutunu el ile komut satırından veya düzenli olarak çalışacak şekilde crontab için ekleyin:
  
    **Ubuntu**
  
    ```bash
    sudo apt-get install util-linux
    sudo fstrim /datadrive
    ```
  
    **RHEL/CentOS**
  
    ```bash
    sudo yum install util-linux
    sudo fstrim /datadrive
    ```

## <a name="troubleshooting"></a>Sorun giderme
[!INCLUDE [virtual-machines-linux-lunzero](../../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Sonraki Adımlar
Daha fazla bilgiyi aşağıdaki makalelerde, Linux VM'NİZDE kullanma hakkında:

* [Linux çalıştıran bir sanal makine için oturum açma][Logon]
* [Nasıl bir Linux sanal makinede bir diski kullanımdan çıkarın](detach-disk-classic.md)
* [Klasik dağıtım modeli ile Azure CLI kullanma](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [Azure'da bir Linux VM üzerinde RAID yapılandırın](../configure-raid.md)
* [Azure'da bir Linux VM LVM yapılandırın](../configure-lvm.md)

<!--Link references-->
[Agent]:../agent-user-guide.md
[Logon]:../mac-create-ssh-keys.md
