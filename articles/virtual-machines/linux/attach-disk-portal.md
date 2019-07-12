---
title: Bir Linux VM'ye veri diski ekleme | Microsoft Docs
description: Bir Linux VM için yeni veya var olan veri diski portalını kullanın.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 5e1c6212-976c-4962-a297-177942f90907
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2018
ms.author: cynthn
ms.subservice: disks
ms.openlocfilehash: 162857ed1b22edf67b44cb4648607103cf733c7d
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67671832"
---
# <a name="use-the-portal-to-attach-a-data-disk-to-a-linux-vm"></a>Bir Linux VM'ye veri diski için portalı kullanma 
Bu makalede Azure portalı üzerinden bir Linux sanal makinesi için yeni ve var olan diskleri ekleme gösterilmektedir. Ayrıca [Azure portalında bir Windows sanal makinesine veri diski](../windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Sanal makinenizde diski önce bu ipuçlarını gözden geçirin:

* Sanal makinenin boyutunu, iliştirebilirsiniz kaç veri diskinin denetler. Ayrıntılar için bkz [sanal makine boyutları](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Sanal makinelere bağlanan diskler Azure'da depolanan gerçekten .vhd dosyalarıdır. Ayrıntılar için bkz. bizim [yönetilen disklere giriş](managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Disk ekledikten sonra yapmanız [yeni disk bağlanacak Linux VM'ye bağlanmak](#connect-to-the-linux-vm-to-mount-the-new-disk).


## <a name="find-the-virtual-machine"></a>Sanal makine bulunamadı
1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Sol menüsünde **sanal makineler**.
3. Listeden sanal makineyi seçin.
4. İçin sanal makine sayfasında **Essentials**, tıklayın **diskleri**.
   
    ![Disk ayarlarını Aç](./media/attach-disk-portal/find-disk-settings.png)


## <a name="attach-a-new-disk"></a>Yeni bir disk ekleme

1. Üzerinde **diskleri** bölmesinde tıklayın **+ veri diski Ekle**.
2. Aşağı açılan menüsüne tıklayın **adı** seçip **Oluştur disk**:

    ![Azure oluşturma yönetilen disk](./media/attach-disk-portal/create-new-md.png)

3. Yönetilen disk için bir ad girin. Varsayılan ayarları gözden geçirin, gerektiği şekilde güncelleştirin ve ardından **Oluştur**.
   
   ![Disk ayarları gözden geçirin](./media/attach-disk-portal/create-new-md-settings.png)

4. Tıklayın **Kaydet** yönetilen disk oluşturun ve VM yapılandırmasını güncelleştirmek için:

   ![Yeni Azure yönetilen diski Kaydet](./media/attach-disk-portal/confirm-create-new-md.png)

5. Azure disk oluşturur ve sanal makineye iliştirir sonra yeni disk sanal makinenin disk ayarları altında listelenir **veri diskleri**. Yönetilen diskler, üst düzey bir kaynakla tam olarak disk kaynak grubunu kökünde görüntülenir:

   ![Azure yönetilen Disk kaynak grubunda](./media/attach-disk-portal/view-md-resource-group.png)

## <a name="attach-an-existing-disk"></a>Var olan bir diski ekleme
1. Üzerinde **diskleri** bölmesinde tıklayın **+ veri diski Ekle**.
2. Aşağı açılan menüsüne tıklayın **adı** mevcut yönetilen disklere Azure aboneliğinize erişilebilir bir listesini görüntülemek için. Yönetilen disk eklemek için seçin:

   ![Var olan Azure yönetilen Disk ekleme](./media/attach-disk-portal/select-existing-md.png)

3. Tıklayın **Kaydet** mevcut yönetilen diski ve VM yapılandırmasını güncelleştirmek için:
   
   ![Azure yönetilen diski güncelleştirmeleri Kaydet](./media/attach-disk-portal/confirm-attach-existing-md.png)

4. Azure disk sanal makineye ekler. sonra sanal makinenin disk ayarları altında listelenen **veri diskleri**.

## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Yeni disk bağlanacak Linux VM'ye bağlanma
İçin bölüm, biçimlendirmek ve Linux VM'nize, VM'ye SSH kullanabilmesi için yeni diski bağlayın. Daha fazla bilgi için bkz. [Azure’da Linux ile SSH kullanma](mac-create-ssh-keys.md). Aşağıdaki örnek, Genel DNS girişi ile bir VM bağlandığı *mypublicdns.westus.cloudapp.azure.com* kullanıcı *azureuser*: 

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

VM'nize bağlandıktan sonra bir disk eklemek hazırsınız. İlk olarak, disk kullanarak bulma `dmesg` (yeni diskinizi bulmak için kullandığınız yöntem farklılık gösterebilir). Aşağıdaki örnek dmesg filtrelemek için kullanır. *SCSI* diskler:

```bash
dmesg | grep SCSI
```

Çıktı aşağıdaki örneğe benzer:

```bash
[    0.294784] SCSI subsystem initialized
[    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
[    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
[    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
[ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
```

Burada, *sdc* istiyoruz disktir. 

### <a name="partition-a-new-disk"></a>Yeni bir disk bölümü
Verileri içeren varolan bir diski kullanıyorsanız, disk takılamadı için atlayın. Yeni bir disk bağlıyorsanız, disk bölümleme gerekir.

`fdisk` ile diski bölümlendirin. Disk boyutu 2 tebibytes (TiB) veya daha büyük daha sonra ise GPT kullanmalısınız kullanabileceğiniz bölümleme, `parted` GPT bölümleme gerçekleştirilecek. Disk boyutu 2TiB altında ise, MBR veya GPT bölümleme kullanabilirsiniz. Birincil disk 1 bölüme kolaylaştırır ve diğer Varsayılanları kabul edin. Aşağıdaki örnek başlatır `fdisk` üzerinde işlem */dev/sdc*:

```bash
sudo fdisk /dev/sdc
```

Kullanım `n` yeni bir bölüm eklemek için komutu. Bu örnekte, biz de tercih `p` için birincil bölüm ve varsayılan değerleri kabul edin. Çıktı aşağıdaki örneğe benzer olacaktır:

```bash
Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
Building a new DOS disklabel with disk identifier 0x2a59b123.
Changes will remain in memory only, until you decide to write them.
After that, of course, the previous content won't be recoverable.

Warning: invalid flag 0x0000 of partition table 4 will be corrected by w(rite)

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-10485759, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-10485759, default 10485759):
Using default value 10485759
```

Bölüm tablosu yazarak yazdırma `p` ve ardından `w` diske yükleyin ve çıkın tablo yazmak. Çıktı aşağıdaki örneğe benzer olmalıdır:

```bash
Command (m for help): p

Disk /dev/sdc: 5368 MB, 5368709120 bytes
255 heads, 63 sectors/track, 652 cylinders, total 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x2a59b123

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048    10485759     5241856   83  Linux

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```

Artık, bir dosya sistemi bölümü yazma `mkfs` komutu. Dosya sistemi türünüz ve cihaz adı belirtin. Aşağıdaki örnek, oluşturur bir *ext4* dosya sisteminde */dev/sdc1* , önceki adımlarda oluşturulan bölümüne:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Çıktı aşağıdaki örneğe benzer:

```bash
mke2fs 1.42.9 (4-Feb-2014)
Discarding device blocks: done
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
327680 inodes, 1310464 blocks
65523 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=1342177280
40 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
    32768, 98304, 163840, 229376, 294912, 819200, 884736
Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```
### <a name="mount-the-disk"></a>Diski bağlayın
Kullanarak dosya sistemini bağlamak için bir dizin oluşturma `mkdir`. Aşağıdaki örnek, bir dizin oluşturur. */datadrive*:

```bash
sudo mkdir /datadrive
```

Kullanım `mount` sonra dosya sistemi bağlamak için. Aşağıdaki örnek bağlar */dev/sdc1* için bölüm */datadrive* bağlama noktası:

```bash
sudo mount /dev/sdc1 /datadrive
```

Sürücüyü otomatik olarak yeniden başlatma sonrası yeniden emin olmak için onu eklenmelidir */etc/fstab* dosya. UUID (evrensel benzersiz tanımlayıcı) kullanıldığını da önemle tavsiye edilir */etc/fstab* yalnızca cihaz adı yerine sürücü başvurmak için (gibi */dev/sdc1*). İşletim sistemi önyüklemesi sırasında disk hata algılarsa, UUID kullanarak belirli bir konuma bağlı hatalı disk önler. Veri diskleri kalan sonra bu cihaz kimlikleri aynı atanır. Sürücünün UUID'sini, yeni bulmak için kullanın `blkid` yardımcı programı:

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

Ardından, açık */etc/fstab* gibi bir metin düzenleyicisinde dosya:

```bash
sudo vi /etc/fstab
```

Bu örnekte, UUID değeri kullanın */dev/sdc1* oluşturulduğu önceki adımları ve takma noktası, cihaz */datadrive*. Sonuna aşağıdaki satırı ekleyin */etc/fstab* dosyası:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

> [!NOTE]
> Daha sonra fstab düzenleme olmadan veri diski kaldırma VM önyükleme başarısız olmasına neden olabilir. Çoğu dağıtımları ya da sağlamak *nofail* ve/veya *nobootwait* fstab seçenekleri. Bu seçenekler, önyükleme sırasında bağlamak disk başarısız olsa bile önyüklemek bir sistem sağlar. Bu parametreler hakkında daha fazla bilgi için dağıtım 's belgelerine başvurun.
> 
> *Nofail* seçeneği, sanal makine bile dosya sistemi bozuk ya da diskin önyükleme sırasında yok başlatılmadan sağlar. Bu seçenek olmadan davranışı açıklandığı karşılaşabilirsiniz [olamaz SSH için Linux VM'si FSTAB hataları nedeniyle](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)

### <a name="trimunmap-support-for-linux-in-azure"></a>Azure'da Linux TRIM/UNMAP desteği
Bazı Linux çekirdeklerinin diskte kullanılmayan blokları atmak TRIM/UNMAP işlemleri destekler. Bu özellik sayfaları silinmiş Azure artık geçerli değil ve iptal edilecek ve büyük dosyaları oluşturmak ve bunları silin, para tasarrufu yapabileceğiniz bilgilendirmek için standart depolama alanında özellikle yararlıdır.

TRIM etkinleştirmek için iki şekilde destek Linux VM'nize vardır. Her zamanki şekilde dağıtımınız için önerilen yaklaşım bakın:

* Kullanım `discard` bağlama seçeneği */etc/fstab*, örneğin:

    ```bash
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```
* Bazı durumlarda, `discard` seçeneği performans etkileri olabilir. Alternatif olarak, çalıştırabileceğiniz `fstrim` komutunu el ile komut satırından veya düzenli olarak çalıştırmak için crontab ekleyin:
  
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

## <a name="next-steps"></a>Sonraki adımlar
Ayrıca [veri diski](add-disk.md) Azure CLI kullanarak.
