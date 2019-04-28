---
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 11/25/2018
ms.author: cynthn
ms.openlocfilehash: 94f662cea5f20485659a7b93549b758fdd7770f6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61476277"
---
Sanal makineye (VM) bağlı bir veri diskine ihtiyacınız olmadığında bunu kolayca ayırabilirsiniz. VM’den bir diski ayırdığınızda disk depolama biriminden kaldırılmaz. Disk üzerinde var olan verileri yeniden kullanmak isterseniz bu verileri aynı VM’ye veya başka bir VM’ye yeniden ekleyebilirsiniz.  

> [!NOTE]
> Azure'da VM’ler işletim sistemi diski, yerel geçici disk ve isteğe bağlı veri diskleri gibi farklı tür diskler kullanır. Ayrıntılar için bkz. [Sanal Makinelerde Diskler ve VHD’ler Hakkında](../articles/virtual-machines/linux/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). VM’yi silmediğiniz sürece işletim sistemi diskini ayıramazsınız.

## <a name="find-the-disk"></a>Diski bulma
Bir diski VM’den ayırmadan önce LUN numarasını bulmanız gerekir. Bu numara ayrılacak disk için bir tanımlayıcıdır. Bunu yapmak için şu adımları uygulayın:

1. Azure CLI’yi açın ve [Azure aboneliğinize bağlanın](/cli/azure/authenticate-azure-cli). Azure Hizmet Yönetimi modunda olduğunuzdan emin olun (`azure config mode asm`).
2. VM'nize hangi disklerin bağlı olduğunu bulun. Aşağıdaki örnekte `myVM` adlı VM’nin diskleri listelenmiştir:

    ```azurecli
    azure vm disk list myVM
    ```

    Çıktı aşağıdaki örneğe benzer:

    ```azurecli
    * Fetching disk images
    * Getting virtual machines
    * Getting VM disks
      data:    Lun  Size(GB)  Blob-Name                         OS
      data:    ---  --------  --------------------------------  -----
      data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
      data:    0    30        myDataDisk.vhd
      info:    vm disk list command OK
    ```

3. Ayırmak istediğiniz diskin LUN veya **mantıksal birim numarasına** dikkat edin.

## <a name="remove-operating-system-references-to-the-disk"></a>Diske işletim sistemi başvurularını kaldırın
Diski Linux konuğundan ayırmadan önce diskteki hiçbir bölümün kullanımda olmadığından emin olmanız gerekir. İşletim sisteminin yeniden başlatma sonrası yeniden takılmayı denemeyeceğinden emin olun. Bu adımlar diski [eklerken](../articles/virtual-machines/linux/classic/attach-disk-classic.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) muhtemelen oluşturduğunuz yapılandırmayı geri alır.

1. Disk tanımlayıcısını bulmak için `lsscsi` komutunu kullanın. `lsscsi`, `yum install lsscsi` (Red Hat üzerinde dağıtımlarda) veya `apt-get install lsscsi` (Debian üzerinde dağıtımlarda) aracılığıyla yüklenebilir. LUN numarasını kullanarak aradığınız disk tanımlayıcısını bulabilirsiniz. Her satırdaki tanımlama grubunda bulunan son sayı LUN’dur. Aşağıdaki `lsscsi` örneğinde, LUN 0 */dev/sdc* ile eşlenir.

    ```bash
    [1:0:0:0]    cd/dvd  Msft     Virtual CD/ROM   1.0   /dev/sr0
    [2:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sda
    [3:0:1:0]    disk    Msft     Virtual Disk     1.0   /dev/sdb
    [5:0:0:0]    disk    Msft     Virtual Disk     1.0   /dev/sdc
    ```

2. Ayrılacak diskle ilişkili bölümleri bulmak için `fdisk -l <disk>` kullanın. Aşağıdaki örnekte `/dev/sdc` için çıktı gösterilmiştir:

    ```bash
    Disk /dev/sdc: 1098.4 GB, 1098437885952 bytes, 2145386496 sectors
    Units = sectors of 1 * 512 = 512 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk label type: dos
    Disk identifier: 0x5a1d2a1a
    
        Device Boot      Start         End      Blocks   Id  System
    /dev/sdc1            2048  2145386495  1072692224   83  Linux
    ```

3. Disk için listelenen her bir bölümü ayırın. Aşağıdaki örnekte `/dev/sdc1` çıkarılır:

    ```bash
    sudo umount /dev/sdc1
    ```

4. Tüm bölümlerin UUID'lerini bulmak için `blkid` komutunu kullanın. Çıktı aşağıdaki örneğe benzer:

    ```bash
    /dev/sda1: UUID="11111111-1b1b-1c1c-1d1d-1e1e1e1e1e1e" TYPE="ext4"
    /dev/sdb1: UUID="22222222-2b2b-2c2c-2d2d-2e2e2e2e2e2e" TYPE="ext4"
    /dev/sdc1: UUID="33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e" TYPE="ext4"
    ```

5. Ayrılacak diskin tüm bölümlerinde **/etc/fstab** dosyasındaki cihaz yolları veya UUID’ler ile ilişkilendirilmiş girişleri kaldırın.  Bu örneğin girişleri şöyle olabilir:

    ```sh  
   UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults   1   2
   ```

    or
   
   ```sh   
   /dev/sdc1   /datadrive   ext4   defaults   1   2
   ```

## <a name="detach-the-disk"></a>Diski ayırma
Diskin LUN numarasını bulup işletim sistemi başvurularını kaldırdıktan sonra diski ayırmaya hazır olursunuz:

1. Seçili diski sanal makineden ayırmak için `azure vm disk detach
   <virtual-machine-name> <LUN>`.komutunu çalıştırın. Aşağıdaki örnekte `myVM` adlı VM'den `0` adlı LUN ayrılmaktadır:
   
    ```azurecli
    azure vm disk detach myVM 0
    ```

2. Diskin ayrılıp ayrılmadığını `azure vm disk list` seçeneğini tekrar kullanarak görebilirsiniz. Aşağıdaki örnekte `myVM` adlı VM denetlenmektedir:
   
    ```azurecli
    azure vm disk list myVM
    ```

    Çıktı, veri diskinin artık bağlı olmadığını gösteren aşağıdaki örneğe benzer:

    ```azurecli
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        ubuntuVM-2645b8030676c8f8.vhd  Linux
     info:    vm disk list command OK
    ```

Ayrılmış disk depolama alanında kalır, ancak artık bir sanal makineye bağlı değildir.

