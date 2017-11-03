---
title: "Azure yedekleme ile bir VM dosyaları geri | Microsoft Docs"
description: "Yedekleme ve kurtarma Hizmetleri ile Azure VM'de dosya düzeyinde geri yüklemeler gerçekleştirmeyi öğrenin."
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
ms.date: 09/29/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: e1bbae56b70c50fcf691db47efd9dc587686e7da
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="restore-files-to-a-virtual-machine-in-azure"></a>Azure'da sanal makine için dosyaları geri yükle
Azure yedekleme coğrafi olarak yedekli kurtarma kasalarında depolanan kurtarma noktaları oluşturur. Bir kurtarma noktasından geri yüklediğinizde, tüm VM veya tek tek dosyaları geri yükleyebilirsiniz. Bu makalede tek tek dosyaları geri yükleme ayrıntılı olarak açıklanmaktadır. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Liste ve select kurtarma noktaları
> * Bir kurtarma noktası bir VM'ye bağlanın
> * Dosyaları bir kurtarma noktasından geri yükle

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, Bu öğretici, Azure CLI Sürüm 2.0.18 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli). 


## <a name="prerequisites"></a>Ön koşullar
Bu öğretici, korunan bir Linux VM Azure yedekleme ile gerektirir. Yanlışlıkla dosya silme ve kurtarma işlemi benzetimini yapmak için bir web sunucusundan bir sayfayı silin. Bir Web sunucusu çalıştıran ve korunan bir Linux VM Azure yedekleme ile gerekirse bkz [CLI ile azure'da bir sanal makine yedekleme](quick-backup-vm-cli.md).


## <a name="backup-overview"></a>Backup’a genel bakış
Azure yedekleme başlattığında, yedekleme uzantısını VM üzerinde bir zaman içinde nokta anlık görüntüsünü alır. İlk yedek istendiğinde yedekleme uzantısını VM yüklenir. Yedekleme gerçekleştiğinde VM çalışmıyorsa, azure yedekleme anlık görüntü temel alınan depolama da alabilir.

Varsayılan olarak, Azure Backup dosya sisteminin tutarlı bir yedeklemenin alır. Azure yedekleme anlık görüntüsünü alır sonra veri kurtarma Hizmetleri Kasası'na aktarılır. Verimliliği en üst düzeye çıkarmak için Azure Backup tanımlar ve yalnızca son yedeklemeden sonra değiştirilen veri blokları aktarır.

Veri aktarımı tamamlandığında, anlık görüntü kaldırılır ve bir kurtarma noktası oluşturulur.


## <a name="delete-a-file-from-a-vm"></a>Bir sanal makineden dosya silme
Yanlışlıkla silme veya bir dosyaya değişiklik, tek tek dosyaların bir kurtarma noktasından geri yükleyebilirsiniz. Bu işlem, bir kurtarma noktası olarak yedeklenen dosyalara gözatın ve yalnızca gereksinim duyduğunuz dosyalarını geri olanak sağlar. Bu örnekte, biz dosya düzeyinde kurtarma işlemini göstermek için bir web sunucusundan bir dosyayı silin.

1. VM'nize bağlanmak için VM ile IP adresi elde [az vm Göster](/cli/azure/vm?view=azure-cli-latest#az_vm_show):

     ```azurecli-interactive
     az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
     ```

2. Web siteniz şu anda çalıştığını doğrulamak için VM genel IP adresi için bir web tarayıcısı açın. Web tarayıcı penceresini açık bırakın.

    ![Varsayılan NGINX web sayfası](./media/tutorial-restore-files/nginx-working.png)

3. SSH ile VM bağlayın. Değiştir *Publicıpaddress* önceki komutta edinilen ortak IP adresine sahip:

    ```bash
    ssh publicIpAddress
    ```

4. Varsayılan sayfa web sunucusunda silin */var/www/html/index.nginx-debian.html* gibi:

    ```bash
    sudo rm /var/www/html/index.nginx-debian.html
    ```

5. Web tarayıcınızda web sayfasını yenileyin. Web sitesi sayfasında, aşağıdaki örnekte gösterildiği gibi artık yükler:

    ![Varsayılan sayfa artık NGINX web sitesini yükler](./media/tutorial-restore-files/nginx-broken.png)

6. Aşağıdaki gibi VM SSH oturumu kapatın:

    ```bash
    exit
    ```


## <a name="generate-file-recovery-script"></a>Dosya Kurtarma komut dosyası oluştur
Dosyalarınızı geri yüklemek için Azure yedekleme, kurtarma noktası bir yerel sürücü olarak bağlanan VM üzerinde çalıştırılacak bir komut dosyası sağlar. Bu yerel diske göz atın, VM için dosyaları geri ardından Kurtarma noktası bağlantısını kesin. Azure yedekleme zamanlaması ve bekletme için atanan ilkesini temel alarak, verilerinizi yedeklemek devam eder.

1. VM, kullanım için liste kurtarma noktalarına [az yedekleme recoverypoint listesi](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az_backup_recoverypoint_list). Bu örnekte, biz en son kurtarma noktası adlı VM için seçin *myVM* içinde korumalı *myRecoveryServicesVault*:

    ```azurecli-interactive
    az backup recoverypoint list \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --query [0].name \
        --output tsv
    ```

2. Bağlanan veya bağlar, kurtarma noktası, VM için komut dosyası elde etmesini [az yedekleme geri yükleme dosyaları bağlama rp](https://docs.microsoft.com/cli/azure/backup/restore/files?view=azure-cli-latest#az_backup_restore_files_mount_rp). Aşağıdaki örnek komut dosyası adlı VM için alır *myVM* içinde korumalı *myRecoveryServicesVault*.

    Değiştir *myRecoveryPointName* önceki komutta elde edilen kurtarma noktası adı:

    ```azurecli-interactive
    az backup restore files mount-rp \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --rp-name myRecoveryPointName
    ```

    Komut dosyası indirilir ve parola, aşağıdaki örnekte olduğu gibi görüntülenir:

    ```
    File downloaded: myVM_we_1571974050985163527.sh. Use password c068a041ce12465
    ```

3. VM'nize komut dosyasını aktarmak için güvenli kopyalama (SCP) kullanın. İndirilen komut adını sağlayın ve değiştirme *Publicıpaddress* vm'nizin ortak IP adresine sahip. Sondaki eklediğinizden emin olun `:` SCP sonunda komut aşağıdaki gibi:

    ```bash
    scp myVM_we_1571974050985163527.sh 52.174.241.110:
    ```


## <a name="restore-file-to-your-vm"></a>VM'nize dosya geri yükleme
Şimdi, VM'ye kopyalanan kurtarma betiği ile kurtarma noktası bağlanabilir ve dosyaları geri yükle.

1. SSH ile VM bağlayın. Değiştir *Publicıpaddress* vm'nizin aşağıdaki gibi ortak IP adresine sahip:

    ```bash
    ssh publicIpAddress
    ```

2. Ekle düzgün çalışması komut dosyanızı izin vermek için yürütme izinleri ile **chmod**. Kendi komut dosyasının adını girin:

    ```bash
    chmod +x myVM_we_1571974050985163527.sh
    ```

3. Kurtarma noktası bağlamak için komut dosyasını çalıştırın. Kendi komut dosyasının adını girin:

    ```bash
    ./myVM_we_1571974050985163527.sh
    ```

    Komut dosyasını çalıştırır gibi kurtarma noktası erişmek için bir parola girmeniz istenir. Önceki çıktıda gösterilen parolayı girmeniz [az yedekleme geri yükleme dosyaları bağlama rp](https://docs.microsoft.com/cli/azure/backup/restore/files?view=azure-cli-latest#az_backup_restore_files_mount_rp) kurtarma betiği oluşturulan komutu.

    Komut dosyası çıktısı için kurtarma noktası yol sağlar. Aşağıdaki örnek çıkış kurtarma noktası adresindeki bağlandığını gösterir */home/azureuser/myVM-20170919213536/Volume1*:

    ```
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
    Please enter the password as shown on the portal to securely connect to the recovery point. : c068a041ce12465
    
    Connecting to recovery point using ISCSI service...
    
    Connection succeeded!
    
    Please wait while we attach volumes of the recovery point to this machine...
    
    ************ Volumes of the recovery point and their mount paths on this machine ************
    
    Sr.No.  |  Disk  |  Volume  |  MountPath
    
    1)  | /dev/sdc  |  /dev/sdc1  |  /home/azureuser/myVM-20170919213536/Volume1
    
    ************ Open File Explorer to browse for files. ************
    ```

4. Kullanım **cp** takılı kurtarma noktasından NGINX varsayılan web sayfası kopyalamak için özgün dosya konumuna yedekleyin. Değiştir */home/azureuser/myVM-20170919213536/Volume1* bağlama noktası kendi konumu ile:

    ```bash
    sudo cp /home/azureuser/myVM-20170919213536/Volume1/var/www/html/index.nginx-debian.html /var/www/html/
    ```

6. Web tarayıcınızda web sayfasını yenileyin. Web sitesi artık doğru şekilde yeniden, aşağıdaki örnekte gösterildiği gibi yükler:

    ![NGINX web sitesi artık doğru yükler](./media/tutorial-restore-files/nginx-restored.png)

7. Aşağıdaki gibi VM SSH oturumu kapatın:

    ```bash
    exit
    ```

8. Kurtarma noktası ile bir VM'den çıkarın [az yedekleme geri yükleme dosyaları çıkarın rp](https://docs.microsoft.com/cli/azure/backup/restore/files?view=azure-cli-latest#az_backup_restore_files_unmount_rp). Aşağıdaki örnek kurtarma noktası adlı VM'den çıkarır *myVM* içinde *myRecoveryServicesVault*.

    Değiştir *myRecoveryPointName* önceki komutlar elde kurtarma noktanızın adını:
    
    ```azurecli-interactive
    az backup restore files unmount-rp \
        --resource-group myResourceGroup \
        --vault-name myRecoveryServicesVault \
        --container-name myVM \
        --item-name myVM \
        --rp-name myRecoveryPointName
    ```

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir kurtarma noktası bir VM'ye bağlı ve bir web sunucusu için dosyalar geri. Şunları öğrendiniz:

> [!div class="checklist"]
> * Liste ve select kurtarma noktaları
> * Bir kurtarma noktası bir VM'ye bağlanın
> * Dosyaları bir kurtarma noktasından geri yükle

Azure için Windows Server'ı Yedekle konusunda bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Azure için Windows Server'ı Yedekle](tutorial-backup-windows-server-to-azure.md)

