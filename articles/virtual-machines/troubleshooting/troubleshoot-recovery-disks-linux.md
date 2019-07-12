---
title: Azure CLI ile VM sorunlarını gidermek için bir Linux kullanma | Microsoft Docs
description: İşletim sistemi diskini bir kurtarma için Azure CLI kullanarak VM bağlanarak Linux VM sorunlarını giderme hakkında bilgi edinin
services: virtual-machines-linux
documentationCenter: ''
author: genlin
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/16/2017
ms.author: genli
ms.openlocfilehash: e1e91ec4393072a7da78c0de800cab26608c74d6
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709329"
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-with-the-azure-cli"></a>İşletim sistemi diskini bir kurtarma VM'si Azure CLI ile ekleyerek bir Linux VM sorunlarını giderme
Linux sanal makinenize (VM), önyükleme veya disk bir hatasıyla karşılaşırsa, sanal sabit diskin kendisinde sorun giderme adımları gerçekleştirmeniz gerekebilir. Geçersiz bir giriş, yaygın olarak karşılaşılan örneklerden olacaktır `/etc/fstab` engelleyen sanal makine başarıyla önyükleme airdrop. Bu makalede, sanal sabit diskinizi başka bir Linux tüm hataları düzeltin ve ardından orijinal VM'yi yeniden oluşturmak için sanal Makineye bağlanmak için Azure CLI kullanma işlemi açıklanmaktadır. 


## <a name="recovery-process-overview"></a>Kurtarma işlemine genel bakış
Sorun giderme işlemi aşağıdaki gibidir:

1. Sanal sabit diskleri tutmak, sorun yaşayan VM'yi silin.
2. Ekleme ve sorun giderme amacıyla başka bir Linux VM için sanal sabit diski bağlayın.
3. Sorun giderme işlemlerini yapacağınız VM'ye bağlanın. Dosyaları düzenleyin veya özgün sanal sabit diskte sorunları gidermek için herhangi bir aracı çalıştırın.
4. Sorun giderme işlemlerini yaptığınız VM’den sanal sabit diski çıkarın.
5. Orijinal sanal sabit diski kullanarak bir VM oluşturun.

Yönetilen disk kullanan bir VM için bkz: [yeni bir işletim sistemi diskini ekleyerek bir yönetilen Disk sanal makinesinin sorunlarını giderme](#troubleshoot-a-managed-disk-vm-by-attaching-a-new-os-disk).

Bu sorun giderme adımlarını gerçekleştirmek için en son gerekir. [Azure CLI](/cli/azure/install-az-cli2) yüklü ve bir Azure hesabı kullanarak oturum açmış [az login](/cli/azure/reference-index).

Aşağıdaki örneklerde, parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında `myResourceGroup`, `mystorageaccount`, ve `myVM`.


## <a name="determine-boot-issues"></a>Önyükleme sorunlarını belirleme
Neden sanal makinenizin doğru ön yükleyebileceğini değil belirlemek için seri çıktıyı inceleyin. Yaygın olarak karşılaşılan örneklerden geçersiz bir giriştir `/etc/fstab`, ya da temel alınan sanal silinmiş veya taşınmış disk.

İle önyükleme günlüklerini alma [az vm önyükleme tanılaması get-boot-log](/cli/azure/vm/boot-diagnostics). Aşağıdaki örnekte adlı VM'den seri çıktısını alır `myVM` adlı kaynak grubunda `myResourceGroup`:

```azurecli
az vm boot-diagnostics get-boot-log --resource-group myResourceGroup --name myVM
```

Sanal Makineyi önyüklemek neden başarısız olduğunu belirlemek için seri çıkışını gözden geçirin. Herhangi bir göstergesi seri çıkış sağlayarak değil, günlük dosyalarını gözden geçirmek gerekebilir `/var/log` bir sorun giderme sanal makinesine bağlı sanal sabit diski oluşturduktan sonra.


## <a name="view-existing-virtual-hard-disk-details"></a>Mevcut sanal sabit disk ayrıntıları görüntüleyin
Başka bir sanal makineye sanal sabit disk (VHD) iliştirebilmek için önce işletim sistemi diskinin URI tanımlamak gerekir. 

VM'nizi hakkındaki bilgileri görüntüleyin [az vm show](/cli/azure/vm). Kullanım `--query` işletim sistemi diski için URI ayıklamak için bayrak. Aşağıdaki örnekte adlı VM için disk bilgileri alır `myVM` adlı kaynak grubunda `myResourceGroup`:

```azurecli
az vm show --resource-group myResourceGroup --name myVM \
    --query [storageProfile.osDisk.vhd.uri] --output tsv
```

URI benzer **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd** .

## <a name="delete-existing-vm"></a>Mevcut VM'yi silin
Sanal sabit diskler ve sanal makineler Azure'da iki ayrı kaynaktır. Bir sanal sabit disk, işletim sisteminin kendisi, uygulamalar ve yapılandırmalar depolandığı yerdir. VM boyutunu veya konumunu tanımlar ve bir sanal sabit disk veya sanal ağ arabirim kartı (NIC) gibi kaynaklara başvurur meta verilerdir. Her sanal sabit disk bir VM'ye atanan bir kira var. Veri diskleri VM çalışırken bile eklenip çıkarılabilir, ancak VM kaynağı silinmedikçe işletim sistemi diski çıkarılamaz. Kira, VM durdurulmuş ve serbest bırakılmış durumda olsa bile işletim sistemi diski ile bir VM ile ilişkisini sürdürür.

VM'nizi kurtarmanın ilk adımı, VM kaynağını silmektir. VM’yi sildiğinizde sanal sabit diskler depolama hesabınızda kalır. VM silindikten sonra sanal sabit diski ve hataları gidermek için başka bir sanal makineye ekleyin.

VM silme [az vm delete](/cli/azure/vm). Aşağıdaki örnekte adlı sanal makine silinir `myVM` kaynak grubundan adlı `myResourceGroup`:

```azurecli
az vm delete --resource-group myResourceGroup --name myVM 
```

VM sanal sabit diski başka bir sanal makineye eklemeden önce silme işlemlerinin tamamlanmasını bekleyin. Kira VM ile ilişkilendiren sanal sabit diski sanal sabit diski başka bir sanal makineye iliştirebilmek için önce yayımlanması gerekir.


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Mevcut sanal sabit diski başka bir VM'ye
Sonraki birkaç adımı için sorun giderme amacıyla başka bir VM kullanın. Varolan bir sanal sabit diski bulun ve diskin içeriği düzenlemek için bu sorun giderme sanal makinesine ekleyebilir. Bu işlem, yapılandırma hataları düzeltin veya ek uygulama veya sistem günlük dosyalarını, örneğin gözden geçirmek sağlar. Seçin veya sorun giderme amacıyla kullanmak üzere başka bir VM oluşturun.

İle mevcut sanal sabit disk takma [az vm unmanaged-diski](/cli/azure/vm/unmanaged-disk). Mevcut sanal sabit disk eklediğinizde, önceki elde disk URI'si belirtin `az vm show` komutu. Aşağıdaki örnekte mevcut bir sanal sabit disk adlı sorun giderme vm'siyle `myVMRecovery` adlı kaynak grubunda `myResourceGroup`:

```azurecli
az vm unmanaged-disk attach --resource-group myResourceGroup --vm-name myVMRecovery \
    --vhd-uri https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
```


## <a name="mount-the-attached-data-disk"></a>Bağlı veri diski takma

> [!NOTE]
> Aşağıdaki örnekler bir Ubuntu VM üzerinde gerekli adımları ayrıntılı olarak açıklanmaktadır. Red Hat Enterprise Linux veya SUSE gibi farklı bir Linux distro kullanıyorsanız günlük dosyası konumları ve `mount` komutları biraz farklı olabilir. Komutları uygun değişiklikleri için belirli, distro belgelerine bakın.

1. Uygun kimlik bilgilerini kullanarak sorun giderme sanal makinenize yönelik SSH. Bu diski sorun giderme, VM'ye bağlı ilk veri diski varsa, disk için büyük olasılıkla bağlandı `/dev/sdc`. Kullanım `dmseg` kullanıma açılan diskler görüntülemek için:

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

    Önceki örnekte, işletim sistemi diski altındadır `/dev/sda` ve geçici disk, her VM için sağlanan `/dev/sdb`. Birden çok veri diski varsa, bunlar konumunda olmalıdır `/dev/sdd`, `/dev/sde`ve benzeri.

2. Mevcut sanal sabit diskinizi bağlamak için bir dizin oluşturun. Aşağıdaki örnekte adlı bir dizin oluşturur `troubleshootingdisk`:

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. Mevcut sanal sabit diskin birden çok bölüm varsa, gerekli bölümü bağlayın. Aşağıdaki örnek, ilk birincil bölüm bağlar `/dev/sdc1`:

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > Sanal sabit diski evrensel benzersiz tanımlayıcı (UUID) kullanarak azure'daki sanal veri diski takma en iyi uygulamadır. UUID kullanarak sanal sabit disk oluşturma özelliği bu kısa sorun giderme senaryo için gerekli değildir. Ancak, normal kullanım altında düzenleme `/etc/fstab` sanal sabit diskler bağlayın UUID yerine cihaz adını kullanarak VM'yi önyüklemek başarısız olmasına neden olabilir.


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Özgün sanal sabit diskteki sorunları düzeltme
Mevcut sanal sabit bağlı disk ile artık tüm bakım ve sorun giderme adımlarını gereken şekilde gerçekleştirebilirsiniz. Sorunları giderdikten sonra aşağıdaki adımlarla devam edin.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Çıkarın ve özgün sanal sabit disk detach
Hataları çözümlendikten sonra çıkarın ve mevcut sanal sabit diski, sorun giderme sanal makinesinden ayrılamadı. Sorun giderme sanal makinesine sanal sabit disk ekleme kira ilişkisini sonlandırana kadar sanal sabit diskinizi başka bir VM ile kullanamazsınız.

1. Sorun giderme sanal makinenize yönelik SSH oturumundan, varolan bir sanal sabit diski çıkarın. İlk dışında bağlama noktası üst dizinini değiştirin:

    ```bash
    cd /
    ```

    Artık mevcut sanal sabit diski çıkarın. Aşağıdaki örnek, cihazda çıkarır `/dev/sdc1`:

    ```bash
    sudo umount /dev/sdc1
    ```

2. Artık VM'den sanal sabit diski çıkarın. Sorun giderme sanal Makinenize SSH oturumundan çıkın. Sorun giderme sanal makinenize bağlı veri diskleri Listele [az vm unmanaged-disk listesi](/cli/azure/vm/unmanaged-disk). Aşağıdaki örnekte adlı VM'ye veri diskleri listelenmiştir `myVMRecovery` adlı kaynak grubunda `myResourceGroup`:

    ```azurecli
    azure vm unmanaged-disk list --resource-group myResourceGroup --vm-name myVMRecovery \
        --query '[].{Disk:vhd.uri}' --output table
    ```

    Mevcut sanal sabit diski için bu adı not edin. Örneğin, bir diskin adını URI'si ile **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd** olduğu **myVHD**. 

    VM'den veri diski çıkarma [az vm unmanaged-diskini](/cli/azure/vm/unmanaged-disk). Aşağıdaki örnekte adlı disk ayırır `myVHD` adlı VM'den `myVMRecovery` içinde `myResourceGroup` kaynak grubu:

    ```azurecli
    az vm unmanaged-disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --name myVHD
    ```


## <a name="create-vm-from-original-hard-disk"></a>Orijinal sabit diskten VM oluşturma
Özgün sanal sabit diskten bir VM oluşturmak için kullanın [bu Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd). Gerçek JSON şablonunu aşağıdaki bağlantıda verilmiştir:

- https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-specialized-vhd-new-or-existing-vnet/azuredeploy.json

Şablonun, önceki komuttan VHD URİ'Sİ'ı kullanarak bir VM dağıtır. Şablon ile dağıtım [az grubu dağıtım oluşturma](/cli/azure/group/deployment). Özgün VHD'nizi URI'yı sağlayın ve ardından işletim sistemi türü, VM boyutu ve VM adını aşağıdaki gibi belirtin:

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeployment \
  --parameters '{"osDiskVhdUri": {"value": "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"},
    "osType": {"value": "Linux"},
    "vmSize": {"value": "Standard_DS1_v2"},
    "vmName": {"value": "myDeployedVM"}}' \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json
```

## <a name="re-enable-boot-diagnostics"></a>Önyükleme tanılaması yeniden etkinleştirin
Mevcut sanal sabit diskten VM oluşturma, önyükleme tanılamaları otomatik olarak etkinleştirilmemiş olabilir. Önyükleme tanılamasını etkinleştirin [az vm boot-diagnostics disable](/cli/azure/vm/boot-diagnostics). Aşağıdaki örnekte adlı VM'de tanılama uzantısını etkinleştirir `myDeployedVM` adlı kaynak grubunda `myResourceGroup`:

```azurecli
az vm boot-diagnostics enable --resource-group myResourceGroup --name myDeployedVM
```

## <a name="troubleshoot-a-managed-disk-vm-by-attaching-a-new-os-disk"></a>Yeni bir işletim sistemi diskini ekleyerek bir yönetilen Disk sanal makinesinin sorunlarını giderme
1. Etkilenen sanal Makineyi durdurun.
2. [Yönetilen disk anlık görüntü oluşturma](../linux/snapshot-copy-managed-disk.md) yönetilen Disk sanal makinenin işletim sistemi diskinin.
3. [Anlık görüntüden yönetilen disk oluşturma](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md).
4. [VM veri diski olarak yönetilen diski](../windows/attach-disk-ps.md).
5. [Veri diski 4. adımdan işletim sistemi diskini değiştirmek](../windows/os-disk-swap.md).

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinenizde bağlanma sorunu yaşıyorsanız bkz [sorun giderme SSH bağlantıları için bir Azure VM](troubleshoot-ssh-connection.md). Sanal makinenizde çalışan uygulamalara erişim sorunları için bkz: [bir Linux sanal makinesinde uygulama bağlantı sorunlarını giderme](troubleshoot-app-connection.md).

