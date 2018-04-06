---
title: Azure CLI kullanarak Linux VM için bir disk ekleyin | Microsoft Docs
description: Azure CLI 1.0 ve 2. 0, Linux VM kalıcı bir disk eklemek öğrenin.
keywords: Linux sanal makine, kaynak disk ekleme
services: virtual-machines-linux
documentationcenter: ''
author: rickstercdn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 3005a066-7a84-4dc5-bdaa-574c75e6e411
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 02/02/2017
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c3d3e3468b491f366473899f5d073704ea9a95ea
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="add-a-disk-to-a-linux-vm"></a>Linux VM'ye disk ekleme
Bu makalede, verilerinizi - koruyabilmeniz için VM'yi yeniden sağlaması yapılana Bakım veya yeniden boyutlandırma nedeniyle olsa bile kalıcı bir disk VM'nize nasıl ekleneceği gösterilmektedir. 


## <a name="use-managed-disks"></a>Yönetilen diskleri kullanın
Azure Yönetilen Diskler, VM diskleriyle ilişkili depolama hesaplarını yöneterek Azure VM’leri için disk yönetimini basitleştirir. Sizin yalnızca gereksinim duyduğunuz disk türünü (Premium veya Standart) ve boyutunu belirtmeniz yeterlidir; disk Azure tarafından sizin adınıza oluşturulur ve yönetilir. Daha fazla bilgi için bkz: [yönetilen diskleri genel bakış](managed-disks-overview.md).


### <a name="attach-a-new-disk-to-a-vm"></a>Yeni bir disk bir VM'e ekleyin
Yeni bir disk üzerinde VM yeterlidir kullanırsanız [az vm diskini](/cli/azure/vm/disk?view=azure-cli-latest#az_vm_disk_attach) komutunu `--new` parametresi. VM'yi bir kullanılabilirlik dilimindeyse disk VM ile aynı bölgedeki otomatik olarak oluşturulur. Daha fazla bilgi için bkz: [kullanılabilirlik bölgeleri genel bakış](../../availability-zones/az-overview.md). Aşağıdaki örnek adlı bir disk oluşturur *myDataDisk* diğer bir deyişle *50*Gb boyutunda:

```azurecli
az vm disk attach -g myResourceGroup --vm-name myVM --disk myDataDisk \
  --new --size-gb 50
```

### <a name="attach-an-existing-disk"></a>Var olan bir diski ekleme 
Çoğu durumda. önceden oluşturulmuş diskleri bağlayın. Varolan bir disk eklemek için disk Kimliğini bulun ve Kimliğine geçirin [az vm diskini](/cli/azure/vm/disk?view=azure-cli-latest#az_vm_disk_attach) komutu. Aşağıdaki örnek sorgularda adlı bir disk için *myDataDisk* içinde *myResourceGroup*, adlı VM'ye iliştirir *myVM*:

```azurecli
# find the disk id
diskId=$(az disk show -g myResourceGroup -n myDataDisk --query 'id' -o tsv)
az vm disk attach -g myResourceGroup --vm-name myVM --disk $diskId
```

Çıktı aşağıdakine benzer görünüyor (kullanabileceğiniz `-o table` çıktıda biçimlendirmek için herhangi bir komuttan seçeneğine):

```json
{
  "accountType": "Standard_LRS",
  "creationData": {
    "createOption": "Empty",
    "imageReference": null,
    "sourceResourceId": null,
    "sourceUri": null,
    "storageAccountId": null
  },
  "diskSizeGb": 50,
  "encryptionSettings": null,
  "id": "/subscriptions/<guid>/resourceGroups/rasquill-script/providers/Microsoft.Compute/disks/myDataDisk",
  "location": "westus",
  "name": "myDataDisk",
  "osType": null,
  "ownerId": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "timeCreated": "2017-02-02T23:35:47.708082+00:00",
  "type": "Microsoft.Compute/disks"
}
```


## <a name="use-unmanaged-disks"></a>Yönetilmeyen diskleri kullanın
Yönetilmeyen diskler temel alınan depolama hesaplarını oluşturmak ve yönetmek için ek yükü gerektirir. Yönetilmeyen diskleri aynı depolama hesabı işletim sistemi diski olarak oluşturulur. Oluşturmak ve yönetilmeyen bir disk eklemek için kullanın [az vm yönetilmeyen disk ekleme](/cli/azure/vm/unmanaged-disk?view=azure-cli-latest#az_vm_unmanaged_disk_attach) komutu. Aşağıdaki örnek iliştirir bir *50*GB yönetilmeyen disk adlı VM'ye *myVM* kaynak grubunda adlı *myResourceGroup*:

```azurecli
az vm unmanaged-disk attach -g myResourceGroup -n myUnmanagedDisk --vm-name myVM \
  --new --size-gb 50
```


## <a name="connect-to-the-linux-vm-to-mount-the-new-disk"></a>Yeni diski Linux VM'ye bağlanın
Bölüm, biçimlendirme ve Linux VM, Azure VM içine SSH kullanabilmesi için yeni disk bağlamak için. Daha fazla bilgi için bkz. [Azure’da Linux ile SSH kullanma](mac-create-ssh-keys.md). Aşağıdaki örnek genel DNS girişi ile VM bağlanır *mypublicdns.westus.cloudapp.azure.com* kullanıcı *azureuser*: 

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

VM'nize bağlandıktan sonra disk eklemek hazırsınız. İlk olarak, disk kullanarak Bul `dmesg` (yeni diskinizin bulmak için kullandığınız yöntem farklı olabilir). Aşağıdaki örnek, filtre uygulamak için dmesg kullanır. *SCSI* diskler:

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

Burada, *sdc* istiyoruz disktir. Diskle bölüm `fdisk`, birincil disk bölüm 1 yapın ve diğer Varsayılanları kabul edin. Aşağıdaki örnek başlatır `fdisk` üzerinde işlem */dev/sdc*:

```bash
sudo fdisk /dev/sdc
```

Çıktı aşağıdaki örneğe benzer:

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

Bölüm yazarak oluşturmak `p` şekilde isteminde:

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

Şimdi, bir dosya sistemi olan bölüm yazma `mkfs` komutu. Dosya sistemi türünüz ve cihaz adı belirtin. Aşağıdaki örnekte bir *ext4* dosya sistemi üzerinde */dev/sdc1* , önceki adımlarda oluşturulan bölümüne:

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

Şimdi, dosya sistemini kullanarak bağlamak için bir dizin oluşturun `mkdir`. Aşağıdaki örnek dizini */datadrive*:

```bash
sudo mkdir /datadrive
```

Kullanım `mount` filesystem bağlamak için. Aşağıdaki örnek bağlar */dev/sdc1* için bölüm */datadrive* bağlama noktası:

```bash
sudo mount /dev/sdc1 /datadrive
```

Sürücünün bir yeniden başlatmadan sonra otomatik olarak yeniden emin olmak için onu eklenmeli */etc/fstab* dosya. Ayrıca (evrensel benzersiz tanıtıcı) UUID kullanılıyor önerilir */etc/fstab* yalnızca aygıt adı yerine sürücü başvurmak için (gibi */dev/sdc1*). İşletim Sisteminin önyükleme sırasında disk hatası algılarsa, UUID kullanarak belirli bir konuma bağlı hatalı disk önler. Veri diskleri kalan sonra bu aynı aygıt kimlikleri atanması. Yeni sürücü UUID'si bulmak için `blkid` yardımcı programı:

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

Ardından, açık */etc/fstab* gibi bir metin düzenleyicisinde dosya:

```bash
sudo vi /etc/fstab
```

Bu örnekte, UUID değeri kullanırız */dev/sdc1* oluşturulduğu önceki adımları ve mountpoint, aygıt */datadrive*. Sonuna aşağıdaki satırı ekleyin */etc/fstab* dosyası:

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
```

> [!NOTE]
> Daha sonra bir veri diski fstab düzenlemeden kaldırma VM önyükleme başarısız olmasına neden olabilir. Çoğu dağıtımları ya da sağlamak *nofail* ve/veya *nobootwait* fstab seçenekleri. Bu seçenekler, önyükleme sırasında bağlamak disk başarısız olsa bile önyükleme sistemi sağlar. Bu parametreler hakkında daha fazla bilgi için dağıtım ait belgelere bakın.
> 
> *Nofail* seçeneği sağlar VM dosya sistemi bozuk ya da önyükleme sırasında diski yok olsa bile başlatır. Bu seçenek olmadan, davranış açıklandığı gibi karşılaşabilirsiniz [olamaz SSH FSTAB hatalar nedeniyle Linux VM](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/)

### <a name="trimunmap-support-for-linux-in-azure"></a>KIRPMA/UNMAP Azure Linux desteği
Bazı Linux tekrar disk üzerindeki kullanılmayan blokları atmak için KIRPMA/UNMAP işlemleri desteklemez. Bu özellik sayfaları silinmiş Azure artık geçerli değil ve iptal ve büyük dosyaları oluşturmak ve bunları silerseniz paradan tasarruf edebilirsiniz bilgilendirmek için standart depolama özellikle yararlıdır.

KIRPMA etkinleştirmenin iki yolu desteği, Linux VM'NİZDE vardır. Her zamanki gibi dağıtımınız için önerilen yaklaşım bakın:

* Kullanım `discard` bağlama seçeneği */etc/fstab*, örneğin:

    ```bash
    UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,discard   1   2
    ```
* Bazı durumlarda, `discard` seçeneği performans etkileri olabilir. Alternatif olarak, çalıştırabilirsiniz `fstrim` komutunu el ile komut satırından veya düzenli olarak çalışacak şekilde crontab için ekleyin:
  
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
[!INCLUDE [virtual-machines-linux-lunzero](../../../includes/virtual-machines-linux-lunzero.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Unutmayın, bu bilgileri yazma sürece bunu yeniden başlatılırsa, yeni disk VM kullanılabilir olmadığını, [fstab](http://en.wikipedia.org/wiki/Fstab) dosya.
* Linux VM doğru şekilde yapılandırıldığından emin olmak için gözden [Linux makine performansı en iyi duruma](optimization.md) öneriler.
* İlave diskler eklenerek, depolama kapasitesi ve [RAID yapılandırma](configure-raid.md) ek performans.

