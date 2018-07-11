---
title: Azure'da bir Linux VM'ye disk ekleme | Microsoft Docs
description: Kullanıma hazır olması için diski başlatın ve klasik dağıtım modelini kullanarak bir Linux VM'ye veri diski takılacağını öğrenin
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
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
ms.author: cynthn
ms.openlocfilehash: b5bb3a9353f83cb569988a068f3ca02da85f739c
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929160"
---
# <a name="how-to-attach-a-data-disk-to-a-linux-virtual-machine"></a>Bir Linux sanal makinesine veri diski ekleme
> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Bkz. nasıl [Resource Manager dağıtım modelini kullanarak bir veri diski ekleme](../add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Boş diskler hem de içeren Azure vm'lerinize veri diskleri ekleyebilirsiniz. Her iki disk türü, bir Azure depolama hesabında yer alan .vhd dosyalardır. Disk ekledikten sonra gibi bir Linux makineye herhangi bir disk eklemeye, başlatmak ve kullanıma hazır şekilde biçimlendirmek gerekir. Bu makalede hem boş diskler ve ardından başlatmak ve yeni bir diski biçimlendirmek nasıl yanı sıra, sanal makinelerinizin verileri içeren ekleme açıklanmaktadır.

> [!NOTE]
> Bir sanal makinenin verilerini depolamak için bir veya daha fazla ayrı diskler kullanmak için en iyi bir uygulamadır. Bir Azure sanal makine oluşturduğunuzda, işletim sistemi diski ve geçici bir diskle sahiptir. **Geçici disk, kalıcı verileri depolamak için kullanmayın.** Adından da anlaşılacağı gibi yalnızca geçici depolama sağlar. Azure depolama alanında bulunan değil çünkü yedeklilik ya da yedekleme sunar.
> Geçici disk genellikle Azure Linux aracısı tarafından yönetilir ve otomatik olarak bağlanan **/mnt/kaynak** (veya **/mnt** Ubuntu görüntülerinde). Öte yandan, bir veri diski tarafından Linux çekirdeğinin gibi adlandırılmış olabilir `/dev/sdc`, bölüm, biçimlendirme ve bu kaynak bağlamak gerekir. Bkz: [Azure Linux Aracısı Kullanım Kılavuzu] [ Agent] Ayrıntılar için.
> 
> 

[!INCLUDE [howto-attach-disk-windows-linux](../../../../includes/howto-attach-disk-linux.md)]

## <a name="initialize-a-new-data-disk-in-linux"></a>Linux'ta yeni bir veri diski başlatın
1. Sanal makinenize yönelik SSH. Daha fazla bilgi için [Linux çalıştıran bir sanal makine için oturum açma][Logon].
2. Sonraki cihaz tanımlayıcısı başlatmak veri diski bulmanız gerekir. Bunu yapmanın iki yolu vardır:
   
    (a) Grep günlüklerinde aşağıdaki komutu gibi SCSI cihazları için:
   
    ```bash
    sudo grep SCSI /var/log/messages
    ```
   
    Yeni Ubuntu dağıtımları için kullanmanız gerekebilir `sudo grep SCSI /var/log/syslog` oturum olduğundan `/var/log/messages` varsayılan olarak devre dışı bırakılabilir.
   
    Görüntülenen iletileri eklenen son veri disk tanımlayıcısını bulabilirsiniz.
   
    ![Disk iletileri alma](./media/attach-disk/scsidisklog.png)
   
    OR
   
    (b) kullanım `lsscsi` cihaz kimliğini bulmak için komutu. `lsscsi` ya da yüklenebilir `yum install lsscsi` (Red Hat üzerinde dağıtımlarda) veya `apt-get install lsscsi` (Debian üzerinde dağıtımlarda). Tarafından aradığınız disk bulabilirsiniz, *lun* veya **mantıksal birim numarası**. Örneğin, *lun* bağlı diskler gelen kolayca görülebilir için `azure vm disk list <virtual-machine>` olarak:

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
   
    Bu veri çıkışı ile karşılaştırma `lsscsi` aynı sanal makine örneği için:
   
    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```
   
    Her satırdaki tanımlama grubunda son sayı *lun*. Bkz: `man lsscsi` daha fazla bilgi için.
3. İstemde, Cihazınızı oluşturmak için aşağıdaki komutu yazın:
   
    ```bash
    sudo fdisk /dev/sdc
    ```

4. İstendiğinde, yazın **n** bir bölüm oluşturmak için.

    ![Cihaz oluşturma](./media/attach-disk/fdisknewpartition.png)

5. İstendiğinde, yazın **p** birincil bölüm bölüm yapma. Tür **1** ilk bölüm ve ardından yazın silindir için varsayılan değeri kabul etmek için enter yapma. Bazı sistemlerde, ilk ve son kesimler, silindir yerine varsayılan değerlerini gösterebilir. Bu varsayılan değerleri kabul etmeyi tercih edebilirsiniz.

    ![Bölüm oluşturma](./media/attach-disk/fdisknewpartdetails.png)


6. Tür **p** bölümlenmiş disk ayrıntılarını görmek için.

    ![Liste disk bilgileri](./media/attach-disk/fdiskpartitiondetails.png)


7. Tür **w** disk ayarlarını yazılacak.

    ![Disk değişiklikleri yazma](./media/attach-disk/fdiskwritedisk.png)

8. Artık yeni bölüme dosya sistemi oluşturabilirsiniz. Cihaz kimliği için bölüm numarasını ekleyin (aşağıdaki örnekte `/dev/sdc1`). Aşağıdaki örnek bir ext4 bölüm üzerinde /dev/sdc1 oluşturur:
   
    ```bash
    sudo mkfs -t ext4 /dev/sdc1
    ```
   
    ![Dosya sistemi oluşturun](./media/attach-disk/mkfsext4.png)
   
   > [!NOTE]
   > SuSE Linux Enterprise 11 sistemleri, yalnızca ext4 dosya sistemleri için yalnızca okuma erişimi destekler. Bu sistemler için yeni bir dosya sistemi ext4 yerine ext3 olarak biçimlendirmek için önerilir.

9. Yeni bir dosya sistemi gibi bağlamak için bir dizin olun:
   
    ```bash
    sudo mkdir /datadrive
    ```

10. Son olarak, sürücü şu şekilde bağlayabilirsiniz:
   
    ```bash
    sudo mount /dev/sdc1 /datadrive
    ```
   
    Veri diski olarak kullanılmak üzere hazırdır **/datadrive**.
   
    ![Dizin oluşturun ve diski bağlayın](./media/attach-disk/mkdirandmount.png)

11. Yeni sürücü için /etc/fstab ekleyin:
   
    Sürücüyü otomatik olarak yeniden başlatma sonrası yeniden sağlamak için/etc/fstab dosyasına eklenmelidir. Ayrıca, yalnızca cihaz adı (yani /dev/sdc1) yerine sürücü başvurmak için (evrensel benzersiz tanımlayıcı) UUID /etc/fstab içinde kullanılır önemle tavsiye edilir. UUID kullanarak işletim sistemi, önyükleme ve ardından bu cihaz kimliklerini atanmasını kalan varsa veri diskleri sırasında disk hata algılarsa, belirli bir konuma bağlı hatalı disk önler. Sürücünün UUID'sini, yeni bulmak için kullanabileceğiniz **blkid** yardımcı programı:
   
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
    > Yanlış düzenleme **/etc/fstab** dosya yapılamamasına bir sistemde neden olabilir. Emin değilseniz, düzgün bir şekilde bu dosya düzenleme hakkında daha fazla bilgi için ait dağıtım belgelerine bakın. Ayrıca düzenlemeden önce /etc/fstab dosyasının yedek bir kopyası oluşturulur önerilir.

    Ardından, açık **/etc/fstab** dosyasını bir metin düzenleyicisinde:

    ```bash
    sudo vi /etc/fstab
    ```

    Bu örnekte, UUID değeri yeni için kullandığımız **/dev/sdc1** oluşturulduğu önceki adımları ve takma noktası cihaz **/datadrive**. Sonuna aşağıdaki satırı ekleyin **/etc/fstab** dosyası:

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
    ```

    Ya da SuSE Linux tabanlı sistemlerde biraz farklı bir biçim kullanmak gerekebilir:

    ```sh
    /dev/disk/by-uuid/33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext3   defaults,nofail   1   2
    ```

    > [!NOTE]
    > `nofail` Seçeneği, sanal makine bile dosya sistemi bozuk ya da diskin önyükleme sırasında yok başlatılmadan sağlar. Bu seçenek olmadan davranışı açıklandığı karşılaşabilirsiniz [olamaz SSH için Linux VM'si FSTAB hataları nedeniyle](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/).

    Dosya sistemine bağlandığını artık test edebilirsiniz düzgün çıkarma ve sonra dosya sistemini kaldırmadan, yani örneği kullanarak bağlama noktası `/datadrive` önceki adımlarda oluşturulan:

    ```bash
    sudo umount /datadrive
    sudo mount /datadrive
    ```

    Varsa `mount` komutu, bir hata üretir, doğru sözdizimi için/etc/fstab dosyasını kontrol edin. Ek veri sürücüleri veya bölümler oluşturulan varsa, bunları/etc/fstab de ayrı olarak girin.

    Bu komutu kullanarak sürücü yazılabilir hale getirmek:

    ```bash
    sudo chmod go+w /datadrive
    ```

    > [!NOTE]
    > Daha sonra fstab düzenleme olmadan veri diski kaldırma VM önyükleme başarısız olmasına neden olabilir. Bu sık karşılaşılan bir durumdur, çoğu dağıtım ya da sağlamanız `nofail` ve/veya `nobootwait` adresindeki bağlamak disk başarısız olsa bile önyükleme için bir sisteme izin fstab seçenekleri önyükleme saati. Bu parametreler hakkında daha fazla bilgi için dağıtım 's belgelerine başvurun.

### <a name="trimunmap-support-for-linux-in-azure"></a>Azure'da Linux TRIM/UNMAP desteği
Bazı Linux çekirdeklerinin diskte kullanılmayan blokları atmak TRIM/UNMAP işlemleri destekler. Bu işlemler sayfaları silinmiş Azure artık geçerli değil ve atılabilir bilgilendirmek için standart depolama alanında birincil yararlıdır. Büyük dosyaları oluşturup ardından bunları silerseniz sayfaları atılıyor maliyetinden tasarruf ettirebilir.

TRIM etkinleştirmek için iki şekilde destek Linux VM'nize vardır. Her zamanki şekilde dağıtımınız için önerilen yaklaşım bakın:

* Kullanım `discard` bağlama seçeneği `/etc/fstab`, örneğin:

    ```sh
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```

* Bazı durumlarda `discard` seçeneği performans etkileri olabilir. Alternatif olarak, çalıştırabileceğiniz `fstrim` komutunu el ile komut satırından veya düzenli olarak çalıştırmak için crontab ekleyin:
  
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
Daha fazla Linux VM'nizi Aşağıdaki makaleler de kullanma hakkında:

* [Linux çalıştıran bir sanal makine için oturum açma][Logon]
* [Bir Linux sanal makinesinden bir diski ayırma](detach-disk-classic.md)
* [Azure CLI ile klasik dağıtım modelini kullanarak](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2)
* [Azure'da bir Linux VM'de RAID yapılandırma](../configure-raid.md)
* [Azure'da Linux sanal makinesi üzerinde LVM'yi yapılandırma](../configure-lvm.md)

<!--Link references-->
[Agent]:../../extensions/agent-linux.md
[Logon]:../mac-create-ssh-keys.md
