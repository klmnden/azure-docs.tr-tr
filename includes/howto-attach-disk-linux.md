---
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 11/25/2018
ms.author: cynthn
ms.openlocfilehash: da2605e7d6dc8d46aa5c5cb89ec3e6f46076ffd1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61476292"
---
Diskler hakkında daha fazla bilgi için bkz. [Sanal Makineler için Diskler ve VHD’ler hakkında](../articles/virtual-machines/linux/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

<a id="attachempty"></a>

## <a name="attach-an-empty-disk"></a>Boş disk ekleme
1. Azure Klasik CLI'yı açın ve [Azure aboneliğinize bağlanma](/cli/azure/authenticate-azure-cli). Azure Hizmet Yönetimi modunda olduğunuzdan emin olun (`azure config mode asm`).
2. `azure vm disk attach-new` yazarak aşağıdaki örnekte gösterildiği gibi yeni bir disk oluşturup ekleyin. *myVM* değerini Linux Sanal Makinenizin adıyla değiştirin ve diskin GB cinsinden boyutunu belirtin (bu örnekte *100GB*):

    ```azurecli
    azure vm disk attach-new myVM 100
    ```

3. Veri diski oluşturulup eklendikten sonra aşağıdaki örnekte gösterildiği gibi `azure vm disk list <virtual-machine-name>` çıktısında listelenir:
   
    ```azurecli
    azure vm disk list TestVM
    ```

    Çıktı aşağıdaki örneğe benzer:

    ```bash
    info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        myVM-2645b8030676c8f8.vhd  Linux
     data:    0    100       myVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

<a id="attachexisting"></a>

## <a name="attach-an-existing-disk"></a>Var olan bir diski ekleme
Var olan bir diskin eklenmesi için depolama hesabında bir .vhd olmalıdır.

1. Azure Klasik CLI'yı açın ve [Azure aboneliğinize bağlanma](/cli/azure/authenticate-azure-cli). Azure Hizmet Yönetimi modunda olduğunuzdan emin olun (`azure config mode asm`).
2. Eklemek istediğiniz VHD’nin Azure aboneliğinize daha önce yüklenip yüklenmediğini denetleyin:
   
    ```azurecli
    azure vm disk list
    ```

    Çıktı aşağıdaki örneğe benzer:

    ```azurecli
     info:    Executing command vm disk list
   
   * Fetching disk images
     data:    Name                                          OS
     data:    --------------------------------------------  -----
     data:    myTestVhd                                     Linux
     data:    TestVM-ubuntuVMasm-0-201508060029150744  Linux
     data:    TestVM-ubuntuVMasm-0-201508060040530369
     info:    vm disk list command OK
    ```

3. Kullanmak istediğiniz diski bulamazsanız `azure vm disk create` veya `azure vm disk upload` kullanarak aboneliğinize yerel bir VHD yükleyebilirsiniz. `disk create` örneği aşağıdaki gibi olacaktır:
   
    ```azurecli
    azure vm disk create myVhd .\TempDisk\test.VHD -l "East US" -o Linux
    ```

    Çıktı aşağıdaki örneğe benzer:

    ```azurecli
    info:    Executing command vm disk create
    + Retrieving storage accounts
    info:    VHD size : 10 GB
    info:    Uploading 10485760.5 KB
    Requested:100.0% Completed:100.0% Running:   0 Time:   25s Speed:    82 KB/s
    info:    Finishing computing MD5 hash, 16% is complete.
    info:    https://mystorageaccount.blob.core.windows.net/disks/myVHD.vhd was
    uploaded successfully
    info:    vm disk create command OK
    ```
   
   Ayrıca `azure vm disk upload` kullanarak belirli bir depolama hesabına VHD yükleyebilirsiniz. Azure sanal makine veri disklerinizi yönetme komutları hakkında daha fazla bilgi için [buraya bakın](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

4. Şimdi istediğiniz VHD’yi sanal makinenize ekleyin:
   
    ```azurecli
    azure vm disk attach myVM myVhd
    ```
   
   *myVM* değerini sanal makinenizin adıyla, *myVHD* değerini istediğiniz VHD ile değiştirdiğinizden emin olun.

5. `azure vm disk list <virtual-machine-name>` ile diskin sanal makineye eklendiğini doğrulayabilirsiniz:
   
    ```azurecli
    azure vm disk list myVM
    ```

    Çıktı aşağıdaki örneğe benzer:

    ```azurecli
     info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        TestVM-2645b8030676c8f8.vhd  Linux
     data:    1    10        test.VHD
     data:    0    100        TestVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK
    ```

> [!NOTE]
> Bir veri diski ekledikten sonra sanal makinenin diski depolama için kullanabilmesi için sanal makinede oturum açmanız ve diski başlatmanız gerekir (diski başlatma hakkında daha fazla bilgi için aşağıdaki adımlara bakın).
> 
> 

