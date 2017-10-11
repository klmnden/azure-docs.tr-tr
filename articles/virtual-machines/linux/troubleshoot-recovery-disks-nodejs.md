---
title: Bir Linux VM Azure CLI 1.0 ile sorun giderme kullanma | Microsoft Docs
description: "Kurtarma için Azure CLI 1.0 kullanarak VM işletim sistemi diski bağlanarak Linux VM sorunlarını giderme hakkında bilgi edinin"
services: virtual-machines-linux
documentationCenter: 
authors: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: d817358211f123c96d899c5cff88cc47aeb5c9c1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-cli-10"></a>Bir Linux VM kurtarma Azure CLI 1.0 kullanarak VM için işletim sistemi diski ekleyerek sorun giderme
Linux sanal makine (VM) önyükleme veya disk bir hatayla karşılaştığında, sanal sabit diskin kendisinde sorun giderme adımları gerçekleştirmeniz gerekebilir. Geçersiz bir giriş yaygın bir örnek olacaktır `/etc/fstab` engelleyen VM başarıyla önyükleme yapamamasına. Bu makalede Azure CLI 1.0, sanal sabit diski başka bir Linux hataları düzeltin, sonra özgün VM'yi yeniden oluşturmak için VM'e bağlanmak için nasıl kullanılacağını ayrıntılarını verir.


## <a name="cli-versions-to-complete-the-task"></a>Görevi tamamlamak için kullanılacak CLI sürümleri
Görevi aşağıdaki CLI sürümlerinden birini kullanarak tamamlayabilirsiniz:

- [Azure CLI 1.0](#recovery-process-overview) – bizim CLI Klasik ve kaynak yönetimi dağıtım modeline (Bu makalede)
- [Azure CLI 2.0](../windows/troubleshoot-recovery-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): Kaynak yönetimi dağıtım modeline yönelik yeni nesil CLI'mız


## <a name="recovery-process-overview"></a>Kurtarma işlemine genel bakış
Sorun giderme işlemi aşağıdaki gibidir:

1. Sorunlarla, sanal sabit diskleri tutmak VM silin.
2. Ekleme ve sorun giderme amacıyla başka bir Linux VM sanal sabit diski bağlayın.
3. Sorun giderme işlemlerini yapacağınız VM'ye bağlanın. Dosyaları düzenleyin veya özgün sanal sabit diskte sorunlarını gidermek için herhangi bir aracı çalıştırın.
4. Sorun giderme işlemlerini yaptığınız VM’den sanal sabit diski çıkarın.
5. Özgün sanal sabit disk kullanan bir VM oluşturun.

Bilgisayarınızda yüklü olduğundan emin olun [en son Azure CLI 1.0](../../cli-install-nodejs.md) yüklü ve oturum açmış ve Resource Manager modunu kullanma:

```azurecli
azure config mode arm
```

Aşağıdaki örneklerde, parametre adları kendi değerlerinizle değiştirin. Örnek parametre adlarında `myResourceGroup`, `mystorageaccount`, ve `myVM`.


## <a name="determine-boot-issues"></a>Önyükleme sorunlarını belirleme
Neden VM'yi doğru önyükleme mümkün değil belirlemek için seri çıktıyı inceleyin. Yaygın bir örnek, geçersiz bir giriş olduğunu `/etc/fstab`, ya da temel sanal silindiğinde veya taşındığında disk.

Aşağıdaki örnek adlı VM'den seri çıktısını alır `myVM` kaynak grubunda adlı `myResourceGroup`:

```azurecli
azure vm get-serial-output --resource-group myResourceGroup --name myVM
```

VM önyükleme neden başarısız olduğunu belirlemek için seri çıkış gözden geçirin. Seri çıkış herhangi bir gösterge sağlayan değil, günlük dosyalarını inceleyin gerekebilir `/var/log` sorun giderme bir VM öğesine bağlı sanal sabit diske sahip olduğunda.


## <a name="view-existing-virtual-hard-disk-details"></a>Var olan sanal sabit disk ayrıntılarını görüntüleyin
Sanal sabit disk için başka bir VM iliştirebilirsiniz önce sanal sabit disk (VHD) adını tanımlamak gerekir. 

Aşağıdaki örnek adlı VM için bilgilerini alır `myVM` kaynak grubunda adlı `myResourceGroup`:

```azurecli
azure vm show --resource-group myResourceGroup --name myVM
```

Ara `Vhd URI` yukarıdaki komut çıktısı olarak. Aşağıdaki örnek çıktısında kesilmiş `Vhd URI` son satırında:

```azurecli
info:    Executing command vm show
+ Looking up the VM "myVM"
+ Looking up the NIC "myNic"
+ Looking up the public ip "myPublicIP"
...
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :myVM
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/myVM201610292712.vhd
```


## <a name="delete-existing-vm"></a>Var olan VM silme
Sanal sabit diskler ve sanal makineler Azure'da iki ayrı kaynaktır. Bir sanal sabit disk, işletim sisteminin kendisi, uygulamalar ve yapılandırmalar depolandığı ' dir. VM boyutunu veya konumunu tanımlar ve bir sanal sabit disk veya sanal ağ arabirim kartı (NIC) gibi kaynakları başvuran meta verilerdir. Her sanal sabit disk için bir VM eklendiğinde atanmış bir kira var. Veri diskleri VM çalışırken bile eklenip çıkarılabilir, ancak VM kaynağı silinmedikçe işletim sistemi diski çıkarılamaz. Bu VM durduruldu ve deallocated durumda olduğunda bile işletim sistemi diski bir VM ile ilişkilendirilecek kira sürdürür.

VM kurtarmak için ilk adım, VM kaynak silmektir. VM’yi sildiğinizde sanal sabit diskler depolama hesabınızda kalır. VM silindikten sonra sanal sabit diski ve hataları gidermek için başka bir VM'e ekleyin.

Aşağıdaki örnek adlı VM siler `myVM` kaynak grubundan adlı `myResourceGroup`:

```azurecli
azure vm delete --resource-group myResourceGroup --name myVM 
```

VM için başka bir VM sanal sabit disk eklemeden önce silme tamamlanana kadar bekleyin. VM ile ilişkilendirir sanal sabit disk üzerindeki kira süresini sanal sabit disk için başka bir VM eklemeden önce yayımlanması gerekir.


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Varolan bir sanal sabit diski başka bir VM'e ekleyin
Sonraki birkaç adımda için sorun giderme amacıyla başka bir VM kullanın. Varolan bir sanal sabit diski bulun ve diskin içeriği düzenlemek için sorun giderme bu VM'e ekleyin. Bu işlem, yapılandırma hataları düzeltin ya da ek uygulama veya sistem günlük dosyalarını, örneğin gözden olanak sağlar. Seçin veya sorun giderme amacıyla kullanmak için başka bir VM oluşturun.

Varolan bir sanal sabit diski taktığınızda, önceki elde disk URL'yi belirtmek `azure vm show` komutu. Aşağıdaki örnek sorun giderme VM adlı varolan bir sanal sabit diski iliştirir `myVMRecovery` kaynak grubunda adlı `myResourceGroup`:

```azurecli
azure vm disk attach --resource-group myResourceGroup --name myVMRecovery \
    --vhd-url https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
```


## <a name="mount-the-attached-data-disk"></a>Bağlı veri diski bağlama

> [!NOTE]
> Aşağıdaki örnekler bir Ubuntu VM gerekli adımları ayrıntılı olarak açıklanmaktadır. Red Hat Enterprise Linux veya SUSE, gibi farklı Linux distro kullanıyorsanız, günlük dosyası konumları ve `mount` komutları biraz farklı olabilir. Komutları uygun değişiklikleri, belirli distro belgelerine bakın.

1. SSH uygun kimlik bilgilerini kullanarak, sorun giderme VM. Bu disk, sorun giderme VM'ye ekli ilk veri diski olduğundan, diskin büyük olasılıkla bağlı `/dev/sdc`. Kullanım `dmseg` bağlı disklerde görüntülemek için:

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

    Önceki örnekte, işletim sistemi diski altındadır `/dev/sda` ve her bir VM altındadır için sağlanan geçici disk `/dev/sdb`. Birden fazla veri diski varsa, bunlar adresindeki olmalıdır `/dev/sdd`, `/dev/sde`ve benzeri.

2. Varolan bir sanal sabit diski bağlamak için bir dizin oluşturun. Aşağıdaki örnek adlı bir dizin oluşturur `troubleshootingdisk`:

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. Var olan sanal sabit diskin birden çok bölüm varsa, gerekli bir bölüm bağlayın. Aşağıdaki örnekte, ilk birincil bölüm bağlar `/dev/sdc1`:

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > Veri diskleri sanal sabit disk evrensel benzersiz tanımlayıcı (UUID) kullanarak Azure vm'lerinde bağlamak en iyi uygulamadır. Sanal sabit disk UUID'si kullanılarak bağlanması özelliği bu kısa sorun giderme senaryo için gerekli değildir. Bununla birlikte, normal kullanım altında düzenleme `/etc/fstab` sanal sabit diskler bağlayın UUID yerine aygıt adını kullanarak VM önyükleme başarısız olmasına neden olabilir.


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Özgün sanal sabit diskinde ilgili sorunları giderme
Varolan bir sanal sabit takılı diski ile tüm bakım ve sorun giderme adımları gereken şekilde artık gerçekleştirebilirsiniz. Sorunları giderdikten sonra aşağıdaki adımlarla devam edin.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Çıkarın ve özgün sanal sabit disk ayırma
Hatalarınızı çözüldükten sonra çıkarın ve varolan bir sanal sabit diski, sorun giderme sanal makineden ayırın. Sorun giderme VM için sanal sabit disk ekleme kira serbest kadar herhangi bir VM ile sanal sabit disk kullanamazsınız.

1. Sorun giderme VM için SSH oturumundan varolan bir sanal sabit diski çıkarın. Bağlama noktası üst dizini dışında ilk değiştirin:

    ```bash
    cd /
    ```

    Şimdi varolan bir sanal sabit diski çıkarın. Aşağıdaki örnek konumunda aygıttan çıkarır `/dev/sdc1`:

    ```bash
    sudo umount /dev/sdc1
    ```

2. Şimdi VM sanal sabit diski kullanımdan çıkarın. SSH oturumu sorun giderme VM'nize çıkın. Azure CLI ilk sorun giderme VM eklenen veri disklerini listeleyin. Aşağıdaki örnek adlı VM'ye bağlı veri disklerinden listeler `myVMRecovery` kaynak grubunda adlı `myResourceGroup`:

    ```azurecli
    azure vm disk list --resource-group myResourceGroup --vm-name myVMRecovery
    ```

    Not `Lun` , varolan bir sanal sabit disk için değer. Aşağıdaki örnek komut çıktısı bağlı LUN 0 varolan bir sanal disk gösterilmektedir:

    ```azurecli
    info:    Executing command vm disk list
    + Looking up the VM "myVMRecovery"
    data:    Name              Lun  DiskSizeGB  Caching  URI
    data:    ------            ---  ----------  -------  ------------------------------------------------------------------------
    data:    myVM              0                None     https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
    info:    vm disk list command OK
    ```

    Geçerli kullanarak VM veri diski kullanımdan çıkarın `Lun` değeri:

    ```azurecli
    azure vm disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --lun 0
    ```


## <a name="create-vm-from-original-hard-disk"></a>Özgün sabit diskten VM oluşturma
Özgün sanal sabit diskten bir VM oluşturmak için kullanmak [bu Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd). Gerçek JSON şablon aşağıdaki bağlantıda şöyledir:

- https://RAW.githubusercontent.com/Azure/Azure-QuickStart-Templates/master/201-VM-Specialized-VHD/azuredeploy.JSON

Önceki komutu VHD URL'yi kullanarak mevcut sanal ağda, bir VM şablonu dağıtır. Aşağıdaki örnek adlı kaynak grubunu şablon dağıtır `myResourceGroup`:

```azurecli
azure group deployment create --resource-group myResourceGroup --name myDeployment \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json
```

VM adı gibi şablonu için istemleri yanıt (`myDeployedVM` aşağıdaki örnek), işletim sistemi türü (`Linux`) ve VM boyutu (`Standard_DS1_v2`). `osDiskVhdUri` Daha önce kullanıldığında aynı varolan bir sanal sabit diski sorun giderme VM'ye ekleniyor. Komut çıktısı ve istemleri örneği aşağıdaki gibidir:

```azurecli
info:    Executing command group deployment create
info:    Supply values for the following parameters
vmName:  myDeployedVM
osType:  Linux
osDiskVhdUri:  https://mystorageaccount.blob.core.windows.net/vhds/myVM201610292712.vhd
vmSize:  Standard_DS1_v2
existingVirtualNetworkName:  myVnet
existingVirtualNetworkResourceGroup:  myResourceGroup
subnetName:  mySubnet
dnsNameForPublicIP:  mypublicipdeployed
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "mydeployment"
+ Waiting for deployment to complete
+
```


## <a name="re-enable-boot-diagnostics"></a>Önyükleme tanılaması yeniden etkinleştirin

Var olan sanal sabit diskten VM'nizi oluşturduğunuzda, önyükleme tanılaması otomatik olarak etkinleştirilmemiş olabilir. Aşağıdaki örnek adlı VM'de tanılama uzantısını etkinleştirir `myDeployedVM` kaynak grubunda adlı `myResourceGroup`:

```azurecli
azure vm enable-diag --resource-group myResourceGroup --name myDeployedVM
```

## <a name="next-steps"></a>Sonraki adımlar
VM'nize bağlantı sorunları yaşıyorsanız, bkz: [sorun giderme SSH bağlantıları bir Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). VM üzerinde çalışan uygulamalara erişim sorunları için bkz: [bir Linux VM üzerinde uygulama bağlantı sorunlarını giderme](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).