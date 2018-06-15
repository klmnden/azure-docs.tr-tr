---
title: Bir Linux VM Azure portalında sorun giderme kullanma | Microsoft Docs
description: Kurtarma için Azure portalını kullanarak VM işletim sistemi diski bağlanarak Linux sanal makine sorunlarını giderme hakkında bilgi edinin
services: virtual-machines-linux
documentationCenter: ''
authors: iainfoulds
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/14/2016
ms.author: iainfou
ms.openlocfilehash: 89c4c5c986375177918f14417c6b5a9a24925908
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34271751"
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a>Kurtarma için Azure portalını kullanarak VM işletim sistemi diski ekleyerek bir Linux VM sorun giderme
Linux sanal makine (VM) önyükleme veya disk bir hatayla karşılaştığında, sanal sabit diskin kendisinde sorun giderme adımları gerçekleştirmeniz gerekebilir. Geçersiz bir giriş yaygın bir örnek olacaktır `/etc/fstab` engelleyen VM başarıyla önyükleme yapamamasına. Bu makalede Azure portal, sanal sabit diski başka bir Linux hataları düzeltin, sonra özgün VM'yi yeniden oluşturmak için VM'e bağlanmak için nasıl kullanılacağını ayrıntılarını verir.

## <a name="recovery-process-overview"></a>Kurtarma işlemine genel bakış
Sorun giderme işlemi aşağıdaki gibidir:

1. Sorunlarla, sanal sabit diskleri tutmak VM silin.
2. Ekleme ve sorun giderme amacıyla başka bir Linux VM sanal sabit diski bağlayın.
3. Sorun giderme işlemlerini yapacağınız VM'ye bağlanın. Dosyaları düzenleyin veya özgün sanal sabit diskte sorunlarını gidermek için herhangi bir aracı çalıştırın.
4. Sorun giderme işlemlerini yaptığınız VM’den sanal sabit diski çıkarın.
5. Özgün sanal sabit disk kullanan bir VM oluşturun.

Yönetilen disk kullanan VM için bkz: [yeni bir işletim sistemi diski ekleyerek bir yönetilen Disk VM sorun giderme](#troubleshoot-a-managed-disk-vm-by-attaching-a-new-os-disk).

## <a name="determine-boot-issues"></a>Önyükleme sorunlarını belirleme
Önyükleme tanılaması ve neden, VM doğru önyükleme mümkün değil belirlemek için VM ekran inceleyin. Geçersiz bir giriş yaygın bir örnek olacaktır `/etc/fstab`, veya bir temel sanal sabit silindiğinde veya taşındığında disk.

Portalda, VM seçin ve sonra aşağı kaydırın **destek + sorun giderme** bölümü. Tıklatın **önyükleme tanılama** , VM'den akışı konsol iletilerini görüntülemek için. VM bir sorunla karşılaşan neden belirleyebilirsiniz varsa görmek için konsol günlüklerini gözden geçirin. Aşağıdaki örnek, bir VM el ile etkileşimi gerektiren bakım moduna takılmış gösterir:

![Önyükleme tanılaması konsol günlükleri VM görüntüleme](./media/troubleshoot-recovery-disks-portal/boot-diagnostics-error.png)

Tıklatarak **ekran** sayfanın üst kısmında bir VM'nin ekran yakalama indirmek için önyükleme tanılama günlüğü.


## <a name="view-existing-virtual-hard-disk-details"></a>Var olan sanal sabit disk ayrıntılarını görüntüleyin
Sanal sabit disk için başka bir VM iliştirebilirsiniz önce sanal sabit disk (VHD) adını tanımlamak gerekir. 

Portal, kaynak grubunu seçin, sonra depolama hesabınızı seçin. Tıklatın **BLOB'lar**, aşağıdaki örnekte olduğu gibi:

![Depolama BLOB'ları seçin](./media/troubleshoot-recovery-disks-portal/storage-account-overview.png)

Adlı bir kapsayıcıya sahip genellikle **VHD'ler** sanal sabit disklerinizi depolar. Sanal sabit disklerin listesini görüntülemek için bir kapsayıcı seçin. (Önek genellikle VM adıdır), VHD adını not edin:

![Depolama kapsayıcısı VHD tanımlayın](./media/troubleshoot-recovery-disks-portal/storage-container.png)

Listeden, varolan bir sanal sabit diski seçin ve aşağıdaki adımlarda kullanılmak URL'yi kopyalayın:

![Var olan sanal sabit disk URL'sini Kopyala](./media/troubleshoot-recovery-disks-portal/copy-vhd-url.png)


## <a name="delete-existing-vm"></a>Var olan VM silme
Sanal sabit diskler ve sanal makineler Azure'da iki ayrı kaynaktır. Bir sanal sabit disk, işletim sisteminin kendisi, uygulamalar ve yapılandırmalar depolandığı ' dir. VM boyutunu veya konumunu tanımlar ve bir sanal sabit disk veya sanal ağ arabirim kartı (NIC) gibi kaynakları başvuran meta verilerdir. Her sanal sabit disk için bir VM eklendiğinde atanmış bir kira var. Veri diskleri VM çalışırken bile eklenip çıkarılabilir, ancak VM kaynağı silinmedikçe işletim sistemi diski çıkarılamaz. Bu VM durduruldu ve deallocated durumda olduğunda bile işletim sistemi diski bir VM ile ilişkilendirilecek kira sürdürür.

VM kurtarmak için ilk adım, VM kaynak silmektir. VM’yi sildiğinizde sanal sabit diskler depolama hesabınızda kalır. VM silindikten sonra sanal sabit diski ve hataları gidermek için başka bir VM'e ekleyin.

Portalda VM seçin ve sonra tıklatın **silmek**:

![VM önyükleme tanılama önyükleme hatası gösteren ekran görüntüsü](./media/troubleshoot-recovery-disks-portal/stop-delete-vm.png)

VM için başka bir VM sanal sabit disk eklemeden önce silme tamamlanana kadar bekleyin. VM ile ilişkilendirir sanal sabit disk üzerindeki kira süresini sanal sabit disk için başka bir VM eklemeden önce yayımlanması gerekir.


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Varolan bir sanal sabit diski başka bir VM'e ekleyin
Sonraki birkaç adımda için sorun giderme amacıyla başka bir VM kullanın. Varolan bir sanal sabit diski bulun ve diskin içeriği düzenlemek için sorun giderme bu VM'e ekleyin. Bu işlem, yapılandırma hataları düzeltin ya da ek uygulama veya sistem günlük dosyalarını, örneğin gözden olanak sağlar. Seçin veya sorun giderme amacıyla kullanmak için başka bir VM oluşturun.

1. Portaldan, kaynak grubunu seçin, sonra sorun giderme VM'yi seçin. Seçin **diskleri** ve ardından **Attach varolan**:

    ![Portalı'nda mevcut diski kullanıma açın](./media/troubleshoot-recovery-disks-portal/attach-existing-disk.png)

2. Mevcut sanal sabit diskinizi seçmek için **VHD Dosyası**’na tıklayın:

    ![Mevcut bir VHD'ye göz atma](./media/troubleshoot-recovery-disks-portal/select-vhd-location.png)

3. Depolama hesabı ve kapsayıcı seçip, varolan bir VHD'Yİ'ı tıklatın. Tıklatın **seçin** düğmesi Seçiminizi onaylayın:

    ![Mevcut VHD’nizi seçme](./media/troubleshoot-recovery-disks-portal/select-vhd.png)

4. Artık seçili, VHD ile tıklatın **Tamam** varolan bir sanal sabit diski eklemek için:

    ![Varolan bir sanal sabit diski ekleme onaylayın](./media/troubleshoot-recovery-disks-portal/attach-disk-confirm.png)

5. Birkaç saniye sonra **diskleri** bölmesi, VM için varolan bir sanal sabit bir veri diski bağlı disk listeler:

    ![Veri diski olarak bağlı olan sanal sabit disk](./media/troubleshoot-recovery-disks-portal/attached-disk.png)


## <a name="mount-the-attached-data-disk"></a>Bağlı veri diski bağlama

> [!NOTE]
> Aşağıdaki örnekler bir Ubuntu VM gerekli adımları ayrıntılı olarak açıklanmaktadır. Red Hat Enterprise Linux veya SUSE, gibi farklı Linux distro kullanıyorsanız, günlük dosyası konumları ve `mount` komutları biraz farklı olabilir. Komutları uygun değişiklikleri, belirli distro belgelerine bakın.

1. SSH uygun kimlik bilgilerini kullanarak, sorun giderme VM. Bu disk, sorun giderme VM'ye ekli ilk veri diski ise, büyük olasılıkla bağlı `/dev/sdc`. Kullanım `dmseg` bağlı disklerde listelemek için:

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
Hatalarınızı çözüldükten sonra sorun giderme VM varolan sanal sabit diski kullanımdan çıkarın. Sorun giderme VM için sanal sabit disk ekleme kira serbest kadar herhangi bir VM ile sanal sabit disk kullanamazsınız.

1. Sorun giderme VM için SSH oturumundan varolan bir sanal sabit diski çıkarın. Bağlama noktası üst dizini dışında ilk değiştirin:

    ```bash
    cd /
    ```

    Şimdi varolan bir sanal sabit diski çıkarın. Aşağıdaki örnek konumunda aygıttan çıkarır `/dev/sdc1`:

    ```bash
    sudo umount /dev/sdc1
    ```

2. Şimdi VM sanal sabit diski kullanımdan çıkarın. Portal ve tıklatın, VM seçin **diskleri**. Varolan bir sanal sabit diski seçin ve ardından **ayırma**:

    ![Varolan bir sanal sabit disk ayırma](./media/troubleshoot-recovery-disks-portal/detach-disk.png)

    VM başarıyla veri diski devam etmeden önce ayrılmış kadar bekleyin.

## <a name="create-vm-from-original-hard-disk"></a>Özgün sabit diskten VM oluşturma
Özgün sanal sabit diskten bir VM oluşturmak için kullanmak [bu Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet). Önceki komutu VHD URL'yi kullanarak mevcut sanal ağda, bir VM şablonu dağıtır. Tıklatın **Azure'a Dağıt** gibi düğmesi:

![Github'dan şablondan VM dağıtma](./media/troubleshoot-recovery-disks-portal/deploy-template-from-github.png)

Şablon dağıtımı için Azure portal içine yüklenir. Yeni VM ve mevcut Azure kaynakları için adlar girin ve, varolan bir sanal sabit diski URL'sini yapıştırın. Dağıtımı başlatmak için tıklatın **satın alma**:

![VM şablonu dağıtma](./media/troubleshoot-recovery-disks-portal/deploy-from-image.png)


## <a name="re-enable-boot-diagnostics"></a>Önyükleme tanılaması yeniden etkinleştirin
Var olan sanal sabit diskten VM'nizi oluşturduğunuzda, önyükleme tanılaması otomatik olarak etkinleştirilmemiş olabilir. Önyükleme tanılaması durumunu denetleyin ve gerekirse etkinleştirmek için portalda, VM'yi seçin. Altında **izleme**, tıklatın **tanılama ayarları**. Durum olduğundan emin olun **üzerinde**ve yanındaki onay işareti **önyükleme tanılama** seçilir. Herhangi bir değişiklik yaparsanız, tıklatın **kaydetmek**:

![Önyükleme tanılaması ayarları güncelleştirme](./media/troubleshoot-recovery-disks-portal/reenable-boot-diagnostics.png)

## <a name="troubleshoot-a-managed-disk-vm-by-attaching-a-new-os-disk"></a>Yeni bir işletim sistemi diski ekleyerek bir yönetilen Disk VM sorun giderme
1. Etkilenen yönetilen Disk Windows VM durdurun.
2. [Yönetilen disk anlık görüntü](../windows/snapshot-copy-managed-disk.md) yönetilen Disk VM işletim sistemi disk.
3. [Yönetilen bir disk anlık görüntüden oluşturma](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md).
4. [Yönetilen diski VM bir veri diski Ekle](../windows/attach-disk-ps.md).
5. [Veri diski 4. adımdan işletim sistemi diski değiştirmek](../windows/os-disk-swap.md).

## <a name="next-steps"></a>Sonraki adımlar
VM'nize bağlantı sorunları yaşıyorsanız, bkz: [sorun giderme SSH bağlantıları bir Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). VM üzerinde çalışan uygulamalara erişim sorunları için bkz: [bir Linux VM üzerinde uygulama bağlantı sorunlarını giderme](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Kaynak Yöneticisi'ni kullanma hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
