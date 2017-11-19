---
title: "Azure CLI ile Azure diskleri yönetme | Microsoft Docs"
description: "Öğretici - Azure CLI ile Azure diskleri yönetme"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 5a7a58d4c402bcaf639bd255bb7c8b111694e548
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="manage-azure-disks-with-the-azure-cli"></a>Azure CLI ile Azure diskleri yönetme

Azure sanal makineleri diskleri sanal makineleri işletim sistemi, uygulamaları ve verileri depolamak için kullanır. Bir VM oluşturulurken bir disk boyutu ve beklenen iş yükü için uygun yapılandırma seçmek önemlidir. Bu öğretici, dağıtma ve VM diskleri yönetme kapsar. Hakkında bilgi edinin:

> [!div class="checklist"]
> * İşletim sistemi ve geçici disklerle
> * Veri diskleri
> * Standart ve Premium diskleri
> * Disk performansı
> * Ekleme ve veri diskleri hazırlama
> * Diskleri yeniden boyutlandırma
> * Disk anlık görüntüler


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.4 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="default-azure-disks"></a>Azure diskleri varsayılan

Bir Azure sanal makine oluşturulduğunda, iki disk otomatik olarak sanal makineye bağlanmış. 

**İşletim sistemi diski** -işletim sistemi disklerinde en fazla 1 terabayttan küçük boyuta sahip ve VM'ler işletim sistemi barındırır. İşletim sistemi diski etiketli */dev/sda* varsayılan olarak. İşletim sistemi disk yapılandırması önbelleğe alma disk işletim sistemi performans için optimize edilmiştir. Bu yapılandırma, işletim sistemi diski nedeniyle **vermemelisiniz** konak uygulamalar veya veri. Uygulamalar ve veriler için bu makalenin sonraki bölümlerinde ayrıntılı veri diskleri kullanın. 

**Geçici disk** -geçici diskleri VM ile aynı Azure konakta bulunan bir katı hal sürücüsü kullanın. Temp disklerinin yüksek oranda kullanıcı durumda ve geçici veri işleme gibi işlemler için kullanılabilir. Ancak, VM yeni bir ana bilgisayara taşındığında, geçici bir diskte depolanan tüm verileri kaldırılır. Geçici disk boyutunu VM boyutu tarafından belirlenir. Geçici diskleri etiketli */dev/sdb* ve mountpoint, */mnt*.

### <a name="temporary-disk-sizes"></a>Geçici disk boyutları

| Tür | VM boyutu | En büyük geçici disk boyutu (GB) |
|----|----|----|
| [Genel amaçlı](sizes-general.md) | A ve D serisi | 800 |
| [İşlem için iyileştirilmiş](sizes-compute.md) | F serisi | 800 |
| [Bellek için iyileştirilmiş](../virtual-machines-windows-sizes-memory.md) | D ve G serisi | 6144 |
| [Depolama için iyileştirilmiş](../virtual-machines-windows-sizes-storage.md) | L serisi | 5630 |
| [GPU](sizes-gpu.md) | N serisi | 1440 |
| [Yüksek performans](sizes-hpc.md) | A ve H serisi | 2000 |

## <a name="azure-data-disks"></a>Azure veri diski

Uygulama yükleme ve verilerini depolamak için ek veri disklerinin eklenebilir. Veri diskleri sağlam ve esnek veri depolama burada istenen herhangi bir durumda kullanılmalıdır. Her veri diski 1 terabayttan küçük maksimum kapasitesine sahiptir. Kaç tane veri diskleri için bir VM eklenebilecek sanal makine boyutunu belirler. Her VM vCPU için iki veri diskleri eklenebilir. 

### <a name="max-data-disks-per-vm"></a>VM başına en fazla veri diski

| Tür | VM boyutu | VM başına en fazla veri diski |
|----|----|----|
| [Genel amaçlı](sizes-general.md) | A ve D serisi | 32 |
| [İşlem için iyileştirilmiş](sizes-compute.md) | F serisi | 32 |
| [Bellek için iyileştirilmiş](../virtual-machines-windows-sizes-memory.md) | D ve G serisi | 64 |
| [Depolama için iyileştirilmiş](../virtual-machines-windows-sizes-storage.md) | L serisi | 64 |
| [GPU](sizes-gpu.md) | N serisi | 48 |
| [Yüksek performans](sizes-hpc.md) | A ve H serisi | 32 |

## <a name="vm-disk-types"></a>VM disk türleri

Azure disk iki tür sağlar.

### <a name="standard-disk"></a>Standart disk

Standart Depolama, HDD’ler ile desteklenir ve yüksek performans sunarken uygun maliyetli depolama sağlar. Standart diskler için uygun maliyetli geliştirme idealdir ve iş yükü test.

### <a name="premium-disk"></a>Premium disk

Premium diskler, SSD tabanlı yüksek performanslı, düşük gecikme süreli disk tarafından desteklenir. Üretim iş yükü çalıştıran VM'ler için mükemmel. Premium depolama destekleyen DS serisi, DSv2 serisi, GS serisi ve FS-serisi VM'ler. Üç tür (P10, P20, P30) Premium diskleri gelir, disk türü disk boyutunu belirler. Disk boyutu değeri seçerken, sonraki türü yuvarlanır. Örneğin, disk boyutu 128 GB daha az ise, disk türünü P10 ' dir. Disk boyutu 129 GB ile 512 GB arasında ise, bir P20 boyutudur. Hiçbir şey 512 GB'den, bir P30 boyutudur.

### <a name="premium-disk-performance"></a>Premium disk performansı

|Premium depolama disk türü | P10 | P20 | P30 |
| --- | --- | --- | --- |
| Disk boyutu (yukarı yuvarlar) | 128 GB | 512 GB | 1.024 GB (1 TB) |
| Disk başına en fazla IOPS | 500 | 2,300 | 5,000 |
Disk başına aktarım hızı | 100 MB/s | 150 MB/s | 200 MB/sn |

Yukarıdaki tabloda, disk başına maksimum IOPS tanımlanmaktadır olsa da, daha yüksek düzeyde performans birden çok veri diskleri bölümlemesine tarafından elde edilebilir. Örneğin, bir Standard_GS5 VM en 80.000 IOPS elde edebilirsiniz. VM başına maksimum IOPS hakkında ayrıntılı bilgi için bkz: [Linux VM boyutları](sizes.md).

## <a name="create-and-attach-disks"></a>Oluşturma ve diskleri ekleme

Veri diskleri oluşturulur ve VM oluşturma zamanında veya mevcut bir VM'yi bağlı.

### <a name="attach-disk-at-vm-creation"></a>VM oluşturulurken diski kullanıma açın

[az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) komutuyla bir kaynak grubu oluşturun. 

```azurecli-interactive 
az group create --name myResourceGroupDisk --location eastus
```

Kullanarak bir VM oluşturun [az vm oluşturma]( /cli/azure/vm#create) komutu. `--datadisk-sizes-gb` Bağımsız değişkeni bir ek disk oluşturulabilir ve sanal makineye bağlı olduğunu belirtmek için kullanılır. Oluşturma ve birden fazla disk eklemek için disk boyutu değerleri boşlukla ayrılmış bir listesini kullanın. Aşağıdaki örnekte, iki veri disklerinde, her iki 128 GB VM oluşturulur. 128 GB disk boyutu olduğundan, bu diskleri hem de disk başına en fazla 500 IOPS sağlar P10s olarak yapılandırılır.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupDisk \
  --name myVM \
  --image UbuntuLTS \
  --size Standard_DS2_v2 \
  --data-disk-sizes-gb 128 128 \
  --generate-ssh-keys
```

### <a name="attach-disk-to-existing-vm"></a>Var olan VM disk ekleme

Oluşturabilir ve varolan bir sanal makineye yeni bir disk eklemek için kullanın [az vm diskini](/cli/azure/vm/disk#attach) komutu. Aşağıdaki örnek, bir premium disk boyutu, 128 gigabayt oluşturur ve son adımda oluşturulan VM ekler.

```azurecli-interactive 
az vm disk attach --vm-name myVM --resource-group myResourceGroupDisk --disk myDataDisk --size-gb 128 --sku Premium_LRS --new 
```

## <a name="prepare-data-disks"></a>Veri diskleri hazırlama

Bir disk sanal makineye bağlandıktan sonra işletim sistemi diski kullanacak şekilde yapılandırılması gerekir. Aşağıdaki örnek, el ile bir disk nasıl yapılandırılacağını gösterir. Bu işlem ayrıca içinde ele bulut-init, kullanarak otomatik olarak yapılabilir bir [sonraki öğretici](./tutorial-automate-vm-deployment.md).

### <a name="manual-configuration"></a>El ile yapılandırma

Sanal makine ile bir SSH bağlantısı oluşturun. Örnek IP adresinin, sanal makine genel IP ile değiştirin.

```azurecli-interactive 
ssh 52.174.34.95
```

Diskle bölüm `fdisk`.

```bash
(echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
```

Kullanarak bir dosya sistemi bölüm için yazma `mkfs` komutu.

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Böylece işletim sisteminde erişilebilir yeni diski bağlayın.

```bash
sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
```

Disk şimdi aracılığıyla erişilebilir *datadrive* çalıştırarak doğrulanabilir mountpoint `df -h` komutu. 

```bash
df -h
```

Çıktı üzerinde oluşturulmuş yeni bir sürücüye gösterir */datadrive*.

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        30G  1.4G   28G   5% /
/dev/sdb1       6.8G   16M  6.4G   1% /mnt
/dev/sdc1        50G   52M   47G   1% /datadrive
```

Yeniden başlatmanın ardından sürücüsünün yeniden emin olmak için onu eklenmeli */etc/fstab* dosya. Bunu yapmak için diski UUID'si almak `blkid` yardımcı programı.

```bash
sudo -i blkid
```

Sürücü UUID'si çıktısını görüntüler `/dev/sdc1` bu durumda.

```bash
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

Aşağıdakine benzer bir satır ekleyin */etc/fstab* dosya. Ayrıca engelleri yazma Not kullanarak devre dışı bırakılabilir *engel = 0*, bu yapılandırma disk performansını artırabilir. 

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive  ext4    defaults,nofail,barrier=0   1  2
```

Disk yapılandırılabilir, SSH oturumu kapatın.

```bash
exit
```

## <a name="resize-vm-disk"></a>VM disk yeniden boyutlandırma

Bir VM dağıtıldıktan sonra işletim sistemi diski veya hiçbir bağlı veri diski boyutu artırılabilir. Bir diskin boyutunu artırmayı daha fazla depolama alanı ya da daha yüksek düzeyde performans (P10, P20, P30) ihtiyacı olduğunda faydalıdır. Disk boyutu azaltılamaz unutmayın.

Disk boyutunu artırmayı önce kimliği veya disk adı gerekli. Kullanım [az disk listesi](/cli/azure/disk#az_disk_list) bir kaynak grubunda tüm diskleri döndürülecek komutu. Yeniden boyutlandırma istediğiniz disk adını not edin.

```azurecli-interactive 
az disk list -g myResourceGroupDisk --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' --output table
```

VM de serbest gerekir. Kullanım [az vm serbest bırakma]( /cli/azure/vm#deallocate) durdurun ve VM serbest bırakma için komutu.

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupDisk --name myVM
```

Kullanım [az disk güncelleştirme](/cli/azure/vm/disk#update) diski yeniden boyutlandırmak için komutu. Bu örnek adlı bir disk yeniden boyutlandırır *myDataDisk* 1 terabayttan küçük için.

```azurecli-interactive 
az disk update --name myDataDisk --resource-group myResourceGroupDisk --size-gb 1023
```

Yeniden boyutlandırma işlemi tamamlandıktan sonra VM'yi başlatın.

```azurecli-interactive 
az vm start --resource-group myResourceGroupDisk --name myVM
```

İşletim sistemi diski yeniden boyutlandırılabilir, bölüm otomatik olarak olması genişletilmiştir. Bir veri diski yeniden boyutlandırılabilir VM'ler işletim sisteminde genişletilecek geçerli bölümler gerekir.

## <a name="snapshot-azure-disks"></a>Azure diskleri anlık görüntüsünü alın

Bir disk anlık disk okuma yalnızca, zaman noktası kopyasını oluşturur. Azure VM anlık görüntüler, yapılandırma değişiklikleri yapmadan önce bir VM durumunu hızlı bir şekilde kaydetmek için kullanışlıdır. İstenmeyen olması için yapılandırma değişiklikleri kanıtlanmasına durumunda VM durumunu anlık görüntü kullanılarak geri yüklenebilir. Birden fazla disk bir VM'ye sahip olduğu anlık görüntü diğer bağımsız olarak her diskin alınır. Uygulama tutarlı yedek almak için disk anlık görüntüler gerçekleştirmeden önce VM durdurma göz önünde bulundurun. Alternatif olarak, kullanın [Azure Backup hizmeti](/azure/backup/), VM çalışırken otomatik yedeklemeler sağlar.

### <a name="create-snapshot"></a>Anlık görüntü oluşturma

Bir sanal makine disk anlık oluşturmadan önce kimliği veya disk adı gerekli. Kullanım [az vm Göster](https://docs.microsoft.com/en-us/cli/azure/vm#az_vm_show) disk kimliği döndürülecek komutu. Bu örnekte, böylece daha sonraki bir adımda kullanılabilir disk kimliği bir değişkende depolanır.

```azurecli-interactive 
osdiskid=$(az vm show -g myResourceGroupDisk -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

Sanal makine diskini kimliğini sahip olduğunuza göre aşağıdaki komut diskin anlık görüntüsünü oluşturur.

```azurcli
az snapshot create -g myResourceGroupDisk --source "$osdiskid" --name osDisk-backup
```

### <a name="create-disk-from-snapshot"></a>Disk anlık görüntüden oluşturma

Bu anlık görüntü sonra sanal makineyi yeniden oluşturmak için kullanılan bir diske dönüştürülebilir.

```azurecli-interactive 
az disk create --resource-group myResourceGroupDisk --name mySnapshotDisk --source osDisk-backup
```

### <a name="restore-virtual-machine-from-snapshot"></a>Sanal makine anlık görüntüden geri yükleme

Sanal makine kurtarması göstermek için var olan sanal makineyi silin. 

```azurecli-interactive 
az vm delete --resource-group myResourceGroupDisk --name myVM
```

Yeni bir sanal makine anlık görüntü diski oluşturun.

```azurecli-interactive 
az vm create --resource-group myResourceGroupDisk --name myVM --attach-os-disk mySnapshotDisk --os-type linux
```

### <a name="reattach-data-disk"></a>Veri diski yeniden bağlayın

Tüm veri diskleri sanal makineyi yeniden gerekir.

İlk disk adı kullanarak verileri bulma [az disk listesi](https://docs.microsoft.com/cli/azure/disk#az_disk_list) komutu. Bu örnek diskin adını adlı bir değişkende yerleştirir *datadisk*, sonraki adımda kullanılır.

```azurecli-interactive 
datadisk=$(az disk list -g myResourceGroupDisk --query "[?contains(name,'myVM')].[name]" -o tsv)
```

Kullanım [az vm diskini](https://docs.microsoft.com/cli/azure/vm/disk#az_vm_disk_attach) diski eklemek için komutu.

```azurecli-interactive 
az vm disk attach –g myResourceGroupDisk –-vm-name myVM –-disk $datadisk
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, VM diskleri konuları hakkında gibi öğrenilen:

> [!div class="checklist"]
> * İşletim sistemi ve geçici disklerle
> * Veri diskleri
> * Standart ve Premium diskleri
> * Disk performansı
> * Ekleme ve veri diskleri hazırlama
> * Diskleri yeniden boyutlandırma
> * Disk anlık görüntüler

VM yapılandırması otomatikleştirme hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [VM yapılandırmasını otomatikleştirme](./tutorial-automate-vm-deployment.md)
