---
title: Öğretici - Azure CLI ile Azure disklerini yönetme | Microsoft Docs
description: Bu öğreticide, Azure CLI kullanarak sanal makineler için Azure diskleri oluşturup yönetmeyi öğrenirsiniz
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/30/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 04fad24b17d7f74211deae53c0d044f2049660f2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46978327"
---
# <a name="tutorial---manage-azure-disks-with-the-azure-cli"></a>Öğretici - Azure CLI ile Azure disklerini yönetme

Azure sanal makineleri (VM) işletim sistemini, uygulamalarını ve verilerini depolamak için diskleri kullanır. VM oluştururken, beklenen iş yüküne uygun disk boyutu ve yapılandırmasını seçmek önemlidir. Bu öğreticide, VM disklerini dağıtma ve yönetme işlemleri gösterilir. Şunları öğreneceksiniz:

> [!div class="checklist"]
> * İşletim sistemi diskleri ve geçici diskler
> * Veri diskleri
> * Standart ve Premium diskler
> * Disk performansı
> * Veri disklerini ekleme ve hazırlama
> * Diskleri yeniden boyutlandırma
> * Disk anlık görüntüleri

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici için Azure CLI 2.0.30 veya sonraki bir sürümünü çalıştırmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](/cli/azure/install-azure-cli).

## <a name="default-azure-disks"></a>Varsayılan Azure diskleri

Azure sanal makinesi oluşturulduğunda, sanal makineye otomatik olarak iki disk eklenir.

**İşletim sistemi diski** - İşletim sistemi diskleri 2 TB'a kadar boyutlandırılabilir ve VM'nin işletim sistemini barındırır. İşletim sistemi diski varsayılan olarak */dev/sda* etiketine sahiptir. İşletim sistemi diskinin yapılandırmasını önbelleğe alan disk, işletim sistemi performansı için iyileştirilir. Bu yapılandırma nedeniyle işletim sistemi diski uygulamalar veya veriler için **kullanılmamalıdır**. Uygulamalar ve veriler için, bu öğreticide daha sonra ayrıntılı olarak açıklanan veri disklerini kullanın.

**Geçici disk** - Geçici diskler, VM ile aynı Azure konağında bulunan bir katı hal sürücüsünü kullanır. Geçici diskler yüksek performansa sahiptir ve geçici veri işleme gibi işlemler için kullanılabilir. Ancak VM yeni bir konağa taşındığında, geçici diskte depolanan tüm veriler kaldırılır. Geçici diskin boyutu, VM boyutu tarafından belirlenir. Geçici diskler */dev/sdb* etiketine ve */mnt* bağlama noktasına sahiptir.

### <a name="temporary-disk-sizes"></a>Geçici disk boyutları

| Tür | Ortak boyutlar | En yüksek geçici disk boyutu (GiB) |
|----|----|----|
| [Genel amaçlı](sizes-general.md) | A, B ve D serisi | 1600 |
| [İşlem için iyileştirilmiş](sizes-compute.md) | F serisi | 576 |
| [Bellek için iyileştirilmiş](sizes-memory.md) | D, E, G ve M serisi | 6144 |
| [Depolama için iyileştirilmiş](sizes-storage.md) | L serisi | 5630 |
| [GPU](sizes-gpu.md) | N serisi | 1440 |
| [Yüksek performans](sizes-hpc.md) | A ve H serisi | 2000 |

## <a name="azure-data-disks"></a>Azure veri diskleri

Uygulamaları yüklemek ve verileri depolamak için başka veri diskleri eklenebilir. Dayanıklı ve duyarlı veri depolama gerektiren her koşulda veri diskleri kullanılmalıdır. Her veri diski maksimum 4 TB kapasiteye sahiptir. Sanal makinenin boyutu, bir VM’ye kaç veri diskinin eklenebileceğini belirler. Her VM vCPU için iki veri diski eklenebilir.

### <a name="max-data-disks-per-vm"></a>VM başına en fazla veri diski

| Tür | VM Boyutu | VM başına en fazla veri diski |
|----|----|----|
| [Genel amaçlı](sizes-general.md) | A, B ve D serisi | 64 |
| [İşlem için iyileştirilmiş](sizes-compute.md) | F serisi | 64 |
| [Bellek için iyileştirilmiş](../virtual-machines-windows-sizes-memory.md) | D, E ve G serileri | 64 |
| [Depolama için iyileştirilmiş](../virtual-machines-windows-sizes-storage.md) | L serisi | 64 |
| [GPU](sizes-gpu.md) | N serisi | 64 |
| [Yüksek performans](sizes-hpc.md) | A ve H serisi | 64 |

## <a name="vm-disk-types"></a>VM disk türleri

Azure iki disk türü sunar.

### <a name="standard-disk"></a>Standart disk

Standart Depolama, HDD’ler ile desteklenir ve yüksek performans sunarken uygun maliyetli depolama sağlar. Standart diskler, uygun maliyetli bir geliştirme ve iş yükü testi için idealdir.

### <a name="premium-disk"></a>Premium disk

Premium diskler SSD tabanlı, yüksek performanslı ve düşük gecikme süreli disk ile desteklenir. Üretim iş yükü çalıştıran VM'ler için son derece uygundur. Premium Depolama, DS serisi, DSv2 serisi, GS serisi ve FS serisi VM'lerini destekler. Disk boyutu seçilirken boyutun değeri sonraki türe yuvarlanır. Örneğin disk boyutu 128 GB’den azsa disk türü P10’dur. Disk boyutu 129 GB ile 512 GB arasında ise boyut P20’dir. 512 GB’ın üstündeki diskler P30 boyutundadır.

### <a name="premium-disk-performance"></a>Premium disk performansı

|Premium depolama diski türü | P4 | P6 | P10 | P20 | P30 | P40 | P50 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Disk boyutu (yuvarlanmış değer) | 32 GB | 64 GB | 128 GB | 512 GB | 1.024 GB (1 TB) | 2.048 GB (2 TB) | 4.095 GB (4 TB) |
| Disk başına en fazla IOPS | 120 | 240 | 500 | 2.300 | 5.000 | 7.500 | 7.500 |
Disk başına aktarım hızı | 25 MB/sn | 50 MB/sn | 100 MB/s | 150 MB/s | 200 MB/sn | 250 MB/sn | 250 MB/sn |

Yukarıdaki tabloda, disk başına maksimum IOPS tanımlanmış olsa da birden çok veri diski bölümlenerek daha yüksek performansa ulaşılabilir. Örneğin bir Standard_GS5 VM’si en fazla 80.000 IOPS’ye ulaşabilir. VM başına IOPS üst sınırı hakkında ayrıntılı bilgi için bkz. [Linux VM türleri](sizes.md).

## <a name="create-and-attach-disks"></a>Disk oluşturma ve ekleme

Veri diskleri oluşturulabilir ve VM oluşturulduğunda veya varolan bir VM’ye eklenebilir.

### <a name="attach-disk-at-vm-creation"></a>VM oluşturulurken disk ekleme

[az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun.

```azurecli-interactive
az group create --name myResourceGroupDisk --location eastus
```

[az vm create](/cli/azure/vm#az-vm-create) komutuyla bir VM oluşturun. Aşağıdaki örnek, *myVM* adlı bir VM oluşturur, *azureuser* adlı bir kullanıcı hesabı ekler ve yoksa SSH anahtarlarını oluşturur. `--datadisk-sizes-gb` bağımsız değişkeni, ek bir disk oluşturulması ve sanal makineye eklenmesi gerektiğini belirtmek için kullanılır. Birden fazla disk oluşturmak ve eklemek için disk boyutu değerlerinin boşlukla ayrılmış bir listesini kullanın. Aşağıdaki örnekte her iki veri diskinin boyutu 128 GB olan bir VM oluşturulur. Disk boyutları 128 GB olduğundan her disk de P10 (disk başına en fazla 500 IOPS sağlar) olarak yapılandırılabilir.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroupDisk \
  --name myVM \
  --image UbuntuLTS \
  --size Standard_DS2_v2 \
  --admin-username azureuser \
  --generate-ssh-keys \
  --data-disk-sizes-gb 128 128
```

### <a name="attach-disk-to-existing-vm"></a>Varolan VM’ye disk ekleme

Yeni bir veri diski oluşturmak ve bunu varolan bir sanal makineye eklemek için [az vm disk attach](/cli/azure/vm/disk#az-vm-disk-attach) komutunu kullanın. Aşağıdaki örnek 128 gigabayt boyutunda bir premium disk oluşturur ve bunu son adımda oluşturulan VM’ye ekler.

```azurecli-interactive
az vm disk attach \
    --resource-group myResourceGroupDisk \
    --vm-name myVM \
    --disk myDataDisk \
    --size-gb 128 \
    --sku Premium_LRS \
    --new
```

## <a name="prepare-data-disks"></a>Veri disklerini hazırlama

Bir disk sanal makineye eklendikten sonra, diskin kullanılması için işletim sisteminin yapılandırılması gerekir. Aşağıdaki örnekte bir diskin el ile nasıl yapılandırılacağı gösterilmektedir. Bu işlem aynı zamanda cloud-init kullanılarak otomatikleştirilebilir. Bu konu [sonraki öğreticide](./tutorial-automate-vm-deployment.md) açıklanacak.

### <a name="manual-configuration"></a>El ile yapılandırma

Sanal makine ile bir SSH bağlantısı oluşturun. Örnek IP adresini, sanal makinenin genel IP adresiyle değiştirin.

```azurecli-interactive
ssh azureuser@52.174.34.95
```

`fdisk` ile diski bölümlendirin.

```bash
(echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
```

`mkfs` komutu kullanarak dosya sistemini bölüme yazın.

```bash
sudo mkfs -t ext4 /dev/sdc1
```

İşletim sisteminde erişilebilir olması için yeni diski bağlayın.

```bash
sudo mkdir /datadrive && sudo mount /dev/sdc1 /datadrive
```

Disk artık *datadrive* bağlama noktası üzerinden erişilebilir, `df -h` komutunu çalıştırarak bunu doğrulayabilirsiniz.

```bash
df -h
```

Çıktı, */datadrive* konumuna bağlanan yeni sürücüyü gösterir.

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        30G  1.4G   28G   5% /
/dev/sdb1       6.8G   16M  6.4G   1% /mnt
/dev/sdc1        50G   52M   47G   1% /datadrive
```

Bir yeniden başlatma işleminden sonra sürücünün yeniden bağlanmasını sağlamak için sürücü, */etc/fstab* dosyasına eklenmelidir. Bunu yapmak için `blkid` yardımcı programıyla diskin UUID’sini alın.

```bash
sudo -i blkid
```

Bu durumda çıktı sürücünün UUID’sini, `/dev/sdc1`, görüntüler.

```bash
/dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
```

*/etc/fstab* dosyasına aşağıdakine benzer bir satır ekleyin.

```bash
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive  ext4    defaults,nofail   1  2
```

Artık disk yapılandırıldığında göre SSH oturumunu kapatabilirsiniz.

```bash
exit
```

## <a name="resize-vm-disk"></a>VM diskini yeniden boyutlandırma

VM dağıtıldıktan sonra işletim sistemi diskinin veya eklenmiş herhangi bir diskin boyutu artırabilir. Daha fazla alana veya daha yüksek düzeyde performansa (P10, P20 veya P30 gibi) ihtiyaç duyduğunuzda diskin boyunu artırmak yararlı olur. Disk boyutunu azaltılamaz.

Disk boyutunu artırmadan önce diskin kimliği veya adı gerekir. Kaynak gruptaki tüm diskleri döndürmek için [az disk list](/cli/azure/disk#az-disk-list) komutunu kullanın. Yeniden boyutlandırmak istediğiniz diskin adını not alın.

```azurecli-interactive
az disk list \
    --resource-group myResourceGroupDisk \
    --query '[*].{Name:name,Gb:diskSizeGb,Tier:accountType}' \
    --output table
```

VM serbest bırakılmalıdır. VM’yi durdurup serbest bırakmak için [az vm deallocate](/cli/azure/vm#az-vm-deallocate) komutunu kullanın.

```azurecli-interactive
az vm deallocate --resource-group myResourceGroupDisk --name myVM
```

Diski yeniden boyutlandırmak için [az disk update](/cli/azure/vm/disk#az-vm-disk-update) komutunu kullanın. Bu örnek *myDataDisk* adındaki bir diski 1 terabayt olarak yeniden boyutlandırır.

```azurecli-interactive
az disk update --name myDataDisk --resource-group myResourceGroupDisk --size-gb 1023
```

Yeniden boyutlandırma tamamlandıktan sonra VM’yi başlatın.

```azurecli-interactive
az vm start --resource-group myResourceGroupDisk --name myVM
```

İşletim sistemi diskini yeniden boyutlandırırsanız disk bölümü otomatik olarak genişletilir. Veri diskini yeniden boyutlandırırsanız, geçerli bölümlerin tümünün VM’lerin işletim sisteminde genişletilmesi gerekir.

## <a name="snapshot-azure-disks"></a>Azure disklerinin anlık görüntülerini alma

Bir disk anlık görüntüsü aldığınızda, Azure diskin belirli bir noktadaki salt okunur kopyasını oluşturur. Azure VM anlık görüntüleri, yapılandırma değişiklikleri yapmadan önce VM’nin durumunu hızla kaydetmenize yardımcı olur. Yapılandırma değişiklikleri istenmeyen sonuçlar ortaya çıkardığında, VM durumu anlık görüntü kullanılarak geri yüklenebilir. VM birden fazla disk içeriyorsa her bir disk için diğerlerinden bağımsız olarak bir anlık görüntü alınır. Uygulamayla tutarlı yedekler almak için disk anlık görüntülerini almadan önce VM’yi durdurmayı göz önünde bulundurun. Bunun yerine VM çalışırken otomatik olarak yedeklemeyi sağlayan [Azure Backup hizmetini](/azure/backup/) kullanabilirsiniz.

### <a name="create-snapshot"></a>Anlık görüntü oluşturma

Sanal makine diski anlık görüntüsü oluşturmadan önce diskin kimliği veya adı gerekir. Disk kimliğini döndürmek için [az vm show](/cli/azure/vm#az-vm-show) komutunu kullanın. Bu örnekte sonraki adımda kullanılabilmesi için disk kimliği bir değişken içinde saklanır.

```azurecli-interactive
osdiskid=$(az vm show -g myResourceGroupDisk -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

Artık sanal makinenin kimliğine sahip olduğunuza göre aşağıdaki komut, diskin anlık görüntüsünü oluşturabilir.

```azurcli
az snapshot create \
    --resource-group myResourceGroupDisk \
    --source "$osdiskid" \
    --name osDisk-backup
```

### <a name="create-disk-from-snapshot"></a>Anlık görüntüden disk oluşturma

Bu anlık görüntü daha sonra, sanal makineyi yeniden oluşturmak için kullanılabilen bir disk e dönüştürülebilir.

```azurecli-interactive
az disk create --resource-group myResourceGroupDisk --name mySnapshotDisk --source osDisk-backup
```

### <a name="restore-virtual-machine-from-snapshot"></a>Sanal makineyi anlık görüntüden geri yükleme

Sanal makineyi kurtarma işlemini gösterebilmemiz için varolan sanal makineyi silin.

```azurecli-interactive
az vm delete --resource-group myResourceGroupDisk --name myVM
```

Anlık görüntü diskinden yeni bir sanal makine oluşturun.

```azurecli-interactive
az vm create \
    --resource-group myResourceGroupDisk \
    --name myVM \
    --attach-os-disk mySnapshotDisk \
    --os-type linux
```

### <a name="reattach-data-disk"></a>Veri diskini yeniden ekleme

Tüm veri disklerinin sanal makineye yeniden eklenmesi gerekir.

Öncelikle [az disk list](/cli/azure/disk#az-disk-list) komutunu kullanarak veri diski adını bulun. Bu örnek, diskin adını *datadisk* adlı bir değişkene yerleştirir ve bu değişkeni sonraki adımda kullanabilirsiniz.

```azurecli-interactive
datadisk=$(az disk list -g myResourceGroupDisk --query "[?contains(name,'myVM')].[name]" -o tsv)
```

Diski eklemek için [az vm disk attach](/cli/azure/vm/disk#az-vm-disk-attach) komutunu kullanın.

```azurecli-interactive
az vm disk attach –g myResourceGroupDisk –-vm-name myVM –-disk $datadisk
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki VM disk konularını öğrendiniz:

> [!div class="checklist"]
> * İşletim sistemi diskleri ve geçici diskler
> * Veri diskleri
> * Standart ve Premium diskler
> * Disk performansı
> * Veri disklerini ekleme ve hazırlama
> * Diskleri yeniden boyutlandırma
> * Disk anlık görüntüleri

VM yapılandırmasını otomatikleştirme hakkında bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [VM yapılandırmasını otomatikleştirme](./tutorial-automate-vm-deployment.md)
