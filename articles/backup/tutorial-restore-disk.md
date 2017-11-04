---
title: Azure yedekleme ile VM diski geri | Microsoft Docs
description: "Bir disk geri yüklemek ve bir kurtarma, yedekleme ve kurtarma Hizmetleri ile azure'da bir VM oluşturma hakkında bilgi edinin."
services: backup, virtual-machines
documentationcenter: virtual-machines
author: markgalioto
manager: carmonm
editor: 
tags: azure-resource-manager, virtual-machine-backup
ms.assetid: 
ms.service: backup, virtual-machines
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/28/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 9bc6da13786eb9eb6186ceadf0432b3a3ec2c941
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="restore-a-disk-and-create-a-recovered-vm-in-azure"></a>Bir disk geri yüklemek ve kurtarılan bir VM oluşturma
Azure yedekleme coğrafi olarak yedekli kurtarma kasalarında depolanan kurtarma noktaları oluşturur. Bir kurtarma noktasından geri yüklediğinizde, tüm VM veya tek tek dosyaları geri yükleyebilirsiniz. Bu makalede, tam bir VM geri açıklanmaktadır. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Liste ve select kurtarma noktaları
> * Bir disk kurtarma noktasından geri yükleme
> * Geri yüklenen diskten bir VM oluşturma

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.18 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 


## <a name="prerequisites"></a>Ön koşullar
Bu öğretici, korunan bir Linux VM Azure yedekleme ile gerektirir. Yanlışlıkla VM silme ve kurtarma işlemi benzetimini yapmak için bir kurtarma noktasındaki bir diskten VM oluşturun. Korunan bir Linux VM Azure yedekleme ile gerekirse bkz [CLI ile azure'da bir sanal makine yedekleme](quick-backup-vm-cli.md).


## <a name="backup-overview"></a>Backup’a genel bakış
Azure yedekleme başlattığında, yedekleme uzantısını VM üzerinde bir zaman içinde nokta anlık görüntüsünü alır. İlk yedek istendiğinde yedekleme uzantısını VM yüklenir. Yedekleme gerçekleştiğinde VM çalışmıyorsa, azure yedekleme anlık görüntü temel alınan depolama da alabilir.

Varsayılan olarak, Azure Backup dosya sisteminin tutarlı bir yedeklemenin alır. Azure yedekleme anlık görüntüsünü alır sonra veri kurtarma Hizmetleri Kasası'na aktarılır. Verimliliği en üst düzeye çıkarmak için Azure Backup tanımlar ve yalnızca son yedeklemeden sonra değiştirilen veri blokları aktarır.

Veri aktarımı tamamlandığında, anlık görüntü kaldırılır ve bir kurtarma noktası oluşturulur.


## <a name="list-available-recovery-points"></a>Liste kullanılabilir kurtarma noktaları
Bir disk geri yüklemek için Kurtarma veri kaynağı olarak bir kurtarma noktası seçin. Varsayılan ilke her gün bir kurtarma noktası oluşturur ve bunları 30 gün boyunca korur gibi belirli bir noktaya kurtarma zamanında seçmenize olanak tanır kurtarma noktaları birtakım kullanmaya devam edebilir. 

Kullanılabilir kurtarma noktalarının bir listesini görmek için [az yedekleme recoverypoint listesi](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az_backup_recoverypoint_list). Kurtarma noktası **adı** diskleri kurtarmak için kullanılır. Bu öğreticide, en son kurtarma noktası istiyoruz. `--query [0].name` Parametresi en son kurtarma noktası adı şu şekilde seçer:

```azurecli-interactive
az backup recoverypoint list \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --container-name myVM \
    --item-name myVM \
    --query [0].name \
    --output tsv
```


## <a name="restore-a-vm-disk"></a>Bir VM disk geri yükleme
Diskinizin kurtarma noktasından geri yüklemek için ilk Azure storage hesabı oluşturun. Bu depolama hesabı, geri yüklenen disk depolamak için kullanılır. Ek adımlarda, geri yüklenen diski, bir VM oluşturmak için kullanılır.

1. Bir depolama hesabı oluşturmak için kullanmak [az depolama hesabı oluşturma](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_create). Depolama hesabı adı tamamen küçük harfli olması ve genel olarak benzersiz olması gerekir. Değiştir *mystorageaccount* kendi benzersiz bir ad ile:

    ```azurecli-interactive
    az storage account create \
        --resource-group myResourceGroup \
        --name mystorageaccount \
        --sku Standard_LRS
    ```

2. Disk kurtarma noktası ile geri [az yedeklemeyi geri yükleme diskleri geri](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az_backup_restore_restore_disks). Değiştir *mystorageaccount* önceki komutta oluşturulan depolama hesabı adı. Değiştir *myRecoveryPointName* aldığınız önceki çıktıda kurtarma noktası adına sahip [az yedekleme recoverypoint listesi](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az_backup_recoverypoint_list) komutu:

    ```azurecli-interactive
    az backup restore restore-disks \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --storage-account mystorageaccount \
        --rp-name myRecoveryPointName
    ```


## <a name="monitor-the-restore-job"></a>Geri yükleme işi İzleyicisi
Geri yükleme işi durumunu izlemek için kullanmak [az yedekleme işi listesi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az_backup_job_list):

```azurecli-interactive 
az backup job list \
    --resource-group myResourceGroup \
    --vault-name myRecoveryServicesVault \
    --output table
```

Geri yükleme işi gösteren bir aşağıdaki örneğe benzer bir çıkış *devam ediyor*:

```
Name      Operation        Status      Item Name    Start Time UTC       Duration
--------  ---------------  ----------  -----------  -------------------  --------------
7f2ad916  Restore          InProgress  myvm         2017-09-19T19:39:52  0:00:34.520850
a0a8e5e6  Backup           Completed   myvm         2017-09-19T03:09:21  0:15:26.155212
fe5d0414  ConfigureBackup  Completed   myvm         2017-09-19T03:03:57  0:00:31.191807
```

Zaman *durum* geri yükleme işi raporları *tamamlandı*, disk depolama hesabına geri yüklendi.


## <a name="convert-the-restored-disk-to-a-managed-disk"></a>Geri yüklenen disk yönetilen bir diske Dönüştür
Geri yükleme işi yönetilmeyen bir disk oluşturur. Diskten bir VM oluşturmak için öncelikle yönetilen bir diske dönüştürülmesi gerekir.

1. Depolama hesabıyla ilgili bağlantı bilgilerini elde [az depolama hesabı Göster bağlantı dizesi](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_show_connection_string). Değiştir *mystorageaccount* depolama alanınızın adıyla hesap gibi:
    
    ```azurecli-interactive
    export AZURE_STORAGE_CONNECTION_STRING=$( az storage account show-connection-string \
        --resource-group myResourceGroup \
        --output tsv \
        --name mystorageaccount )
    ```

2. Yönetilmeyen diskinizin depolama hesabında güvenli hale getirilir. Aşağıdaki komutlar, yönetilmeyen disk hakkında bilgi almak ve adlı bir değişken oluşturun *URI* kullanılan sonraki adımda, yönetilen Disk oluşturduğunuzda.

    ```azurecli-interactive
    container=$(az storage container list --query [0].name -o tsv)
    blob=$(az storage blob list --container-name $container --query [0].name -o tsv)
    uri=$(az storage blob url --container-name $container --name $blob -o tsv)
    ```

3. Kurtarılan diskinizden ile yönetilen bir Disk oluşturmak üzere, şimdi [az disketi](https://docs.microsoft.com/cli/azure/disk?view=azure-cli-latest#az_disk_create). *URI* önceki adımı değişkeninden yönetilen diskiniz için kaynak olarak kullanılır.

    ```azurecli-interactive
    az disk create \
        --resource-group myResourceGroup \
        --name myRestoredDisk \
        --source $uri
    ```

4. Geri yüklenen diskinizden şimdi yönetilen bir diski olması gibi yönetilmeyen disk ve depolama hesabıyla Temizleme [az depolama hesabını silme](/cli/azure/storage/account?view=azure-cli-latest#az_storage_account_delete). Değiştir *mystorageaccount* depolama alanınızın adıyla hesap gibi:

    ```azurecli-interactive
    az storage account delete \
        --resource-group myResourceGroup \
        --name mystorageaccount
    ```


## <a name="create-a-vm-from-the-restored-disk"></a>Geri yüklenen diskten bir VM oluşturma
Son adım, yönetilen diskten bir VM oluşturmaktır.

1. İle yönetilen diskten bir VM oluşturma [az vm oluşturma](/cli/azure/vm?view=azure-cli-latest#az_vm_create) gibi:

    ```azurecli-interactive
    az vm create \
        --resource-group myResourceGroup \
        --name myRestoredVM \
        --attach-os-disk myRestoredDisk \
        --os-type linux
    ```

2. VM kurtarılan diskinizden oluşturulduğunu doğrulamak için kaynak grubunuzun içindeki Vm'leri listelemek [az vm listesi](/cli/azure/vm?view=azure-cli-latest#az_vm_list) gibi:

    ```azurecli-interactive
    az vm list --resource-group myResourceGroup --output table
    ```


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir disk kurtarma noktasından geri ve ardından diskten VM oluşturdunuz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Liste ve select kurtarma noktaları
> * Bir disk kurtarma noktasından geri yükleme
> * Geri yüklenen diskten bir VM oluşturma

Tek tek dosyaların bir kurtarma noktasından geri yükleme hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Azure'da sanal makine için dosyaları geri yükle](tutorial-restore-files.md)

