---
title: Bir Linux VM Azure Portalı'nda sorun giderme kullanma | Microsoft Docs
description: İşletim sistemi diskini bir kurtarma sanal Makinesine Azure portalını kullanarak bağlanarak Linux sanal makine sorunlarını gidermeyi öğrenin
services: virtual-machines-linux
documentationCenter: ''
author: genlin
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/14/2016
ms.author: genli
ms.openlocfilehash: 65187c3ef6debfa27c8c4fea62bcd31b857b4171
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60319947"
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a>İşletim sistemi diskini bir kurtarma Azure portalını kullanarak sanal Makinesine ekleyerek bir Linux VM sorunlarını giderme
Linux sanal makinenize (VM), önyükleme veya disk bir hatasıyla karşılaşırsa, sanal sabit diskin kendisinde sorun giderme adımları gerçekleştirmeniz gerekebilir. Geçersiz bir giriş, yaygın olarak karşılaşılan örneklerden olacaktır `/etc/fstab` engelleyen sanal makine başarıyla önyükleme airdrop. Bu makalede, sanal sabit diskinizi başka bir Linux tüm hataları düzeltin ve ardından orijinal VM'yi yeniden oluşturmak için VM'ye bağlanmak için Azure portalını kullanma işlemi açıklanmaktadır.

## <a name="recovery-process-overview"></a>Kurtarma işlemine genel bakış
Sorun giderme işlemi aşağıdaki gibidir:

1. Sanal sabit diskleri tutmak, sorun yaşayan VM'yi silin.
2. Ekleme ve sorun giderme amacıyla başka bir Linux VM için sanal sabit diski bağlayın.
3. Sorun giderme işlemlerini yapacağınız VM'ye bağlanın. Dosyaları düzenleyin veya özgün sanal sabit diskte sorunları gidermek için herhangi bir aracı çalıştırın.
4. Sorun giderme işlemlerini yaptığınız VM’den sanal sabit diski çıkarın.
5. Orijinal sanal sabit diski kullanarak bir VM oluşturun.

Yönetilen disk kullanan bir VM için bkz: [yeni bir işletim sistemi diskini ekleyerek bir yönetilen Disk sanal makinesinin sorunlarını giderme](#troubleshoot-a-managed-disk-vm-by-attaching-a-new-os-disk).

## <a name="determine-boot-issues"></a>Önyükleme sorunlarını belirleme
Neden sanal makinenizin doğru ön yükleyebileceğini değil belirlemek için VM ekran görüntüsü ve önyükleme tanılaması inceleyin. Geçersiz bir giriş, yaygın olarak karşılaşılan örneklerden olacaktır `/etc/fstab`, ya da bir temel alınan sanal sabit silinmiş veya taşınmış disk.

Portalında VM'nizi seçin ve ardından aşağı kaydırarak **destek + sorun giderme** bölümü. Tıklayın **önyükleme tanılaması** , VM'den akışa konsol iletileri görüntülemek için. Sanal Makinenin bir sorunla karşılaşan neden karar verirseniz görmek için konsol günlüklerini gözden geçirin. Aşağıdaki örnek, el ile etkileşimi gerektiren bakım modunda bir VM'ye takılı gösterir:

![Konsol günlükleri görüntüleme VM önyükleme tanılaması](./media/troubleshoot-recovery-disks-portal-linux/boot-diagnostics-error.png)

Ayrıca **ekran** üstte yer alan bir VM ekran görüntüsü yakalama indirmek için önyükleme tanılama günlüğü.


## <a name="view-existing-virtual-hard-disk-details"></a>Mevcut sanal sabit disk ayrıntıları görüntüleyin
Sanal sabit diskinizi başka bir sanal makineye iliştirebilmek için önce sanal sabit disk (VHD) adını tanımlamak gerekir. 

Portalda kaynak grubunuzu seçin, ardından depolama hesabınızı seçin. Tıklayın **Blobları**, aşağıdaki örnekte olduğu gibi:

![Depolama BLOB'ları seçin](./media/troubleshoot-recovery-disks-portal-linux/storage-account-overview.png)

Genellikle, adlı bir kapsayıcıya sahip **VHD'ler** , sanal sabit disklerinizin depolar. Sanal sabit disklerin listesini görüntülemek üzere kapsayıcıyı seçin. (Önek genellikle sanal makinenizin adıdır) VHD'nizi adını not edin:

![Depolama kapsayıcısı VHD tanımlayın](./media/troubleshoot-recovery-disks-portal-linux/storage-container.png)

Listeden var olan sanal sabit diskinizi seçin ve aşağıdaki adımlarda kullanılmak URL'yi kopyalayın:

![Mevcut sanal sabit disk URL'sini Kopyala](./media/troubleshoot-recovery-disks-portal-linux/copy-vhd-url.png)


## <a name="delete-existing-vm"></a>Mevcut VM'yi silin
Sanal sabit diskler ve sanal makineler Azure'da iki ayrı kaynaktır. Bir sanal sabit disk, işletim sisteminin kendisi, uygulamalar ve yapılandırmalar depolandığı yerdir. VM boyutunu veya konumunu tanımlar ve bir sanal sabit disk veya sanal ağ arabirim kartı (NIC) gibi kaynaklara başvurur meta verilerdir. Her sanal sabit disk bir VM'ye atanan bir kira var. Veri diskleri VM çalışırken bile eklenip çıkarılabilir, ancak VM kaynağı silinmedikçe işletim sistemi diski çıkarılamaz. Kira, VM durdurulmuş ve serbest bırakılmış durumda olsa bile işletim sistemi diski ile bir VM ile ilişkisini sürdürür.

VM'nizi kurtarmanın ilk adımı, VM kaynağını silmektir. VM’yi sildiğinizde sanal sabit diskler depolama hesabınızda kalır. VM silindikten sonra sanal sabit diski ve hataları gidermek için başka bir sanal makineye ekleyin.

Portalında VM'nizi seçin ve ardından tıklayın **Sil**:

![VM önyükleme tanılama önyükleme hatası gösteren ekran görüntüsü](./media/troubleshoot-recovery-disks-portal-linux/stop-delete-vm.png)

VM sanal sabit diski başka bir sanal makineye eklemeden önce silme işlemlerinin tamamlanmasını bekleyin. Kira VM ile ilişkilendiren sanal sabit diski sanal sabit diski başka bir sanal makineye iliştirebilmek için önce yayımlanması gerekir.


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Mevcut sanal sabit diski başka bir VM'ye
Sonraki birkaç adımı için sorun giderme amacıyla başka bir VM kullanın. Varolan bir sanal sabit diski bulun ve diskin içeriği düzenlemek için bu sorun giderme sanal makinesine ekleyebilir. Bu işlem, yapılandırma hataları düzeltin veya ek uygulama veya sistem günlük dosyalarını, örneğin gözden geçirmek sağlar. Seçin veya sorun giderme amacıyla kullanmak üzere başka bir VM oluşturun.

1. Portalda kaynak grubunuzu seçin, sonra sorun giderme VM'nizi seçin. Seçin **diskleri** ve ardından **iliştirme varolan**:

    ![Portalda mevcut diski ekleme](./media/troubleshoot-recovery-disks-portal-linux/attach-existing-disk.png)

2. Mevcut sanal sabit diskinizi seçmek için **VHD Dosyası**’na tıklayın:

    ![Mevcut bir VHD'ye göz atma](./media/troubleshoot-recovery-disks-portal-linux/select-vhd-location.png)

3. Depolama hesabı ve kapsayıcı seçip mevcut VHD'NİZİ'ı tıklatın. Tıklayın **seçin** düğmesini Seçiminizi onaylayın:

    ![Mevcut VHD’nizi seçme](./media/troubleshoot-recovery-disks-portal-linux/select-vhd.png)

4. Artık VHD'niz seçiliyken, tıklayın **Tamam** mevcut sanal sabit diski eklemek için:

    ![Mevcut sanal sabit disk ekleme onaylayın](./media/troubleshoot-recovery-disks-portal-linux/attach-disk-confirm.png)

5. Birkaç saniye sonra **diskleri** VM'niz için bölmesi, mevcut sanal sabit veri diski olarak bağlı disk listeler:

    ![Veri diski olarak bağlı olan sanal sabit disk](./media/troubleshoot-recovery-disks-portal-linux/attached-disk.png)


## <a name="mount-the-attached-data-disk"></a>Bağlı veri diski takma

> [!NOTE]
> Aşağıdaki örnekler bir Ubuntu VM üzerinde gerekli adımları ayrıntılı olarak açıklanmaktadır. Red Hat Enterprise Linux veya SUSE gibi farklı bir Linux distro kullanıyorsanız günlük dosyası konumları ve `mount` komutları biraz farklı olabilir. Komutları uygun değişiklikleri için belirli, distro belgelerine bakın.

1. Uygun kimlik bilgilerini kullanarak sorun giderme sanal makinenize yönelik SSH. Bu diski sorun giderme, VM'ye bağlı ilk veri diski varsa, büyük olasılıkla bağlı `/dev/sdc`. Kullanım `dmseg` eklenen diskleri listeleme için:

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
Mevcut sanal sabit diski sorun giderme, hataları çözümlendikten sonra çıkarın. Sorun giderme sanal makinesine sanal sabit disk ekleme kira ilişkisini sonlandırana kadar sanal sabit diskinizi başka bir VM ile kullanamazsınız.

1. Sorun giderme sanal makinenize yönelik SSH oturumundan, varolan bir sanal sabit diski çıkarın. İlk dışında bağlama noktası üst dizinini değiştirin:

    ```bash
    cd /
    ```

    Artık mevcut sanal sabit diski çıkarın. Aşağıdaki örnek, cihazda çıkarır `/dev/sdc1`:

    ```bash
    sudo umount /dev/sdc1
    ```

2. Artık VM'den sanal sabit diski çıkarın. Portal ve tıklatın VM'nizi seçin **diskleri**. Mevcut sanal sabit diskinizi seçin ve ardından **ayırma**:

    ![Mevcut sanal sabit diski ayırma](./media/troubleshoot-recovery-disks-portal-linux/detach-disk.png)

    VM başarıyla veri diski devam etmeden önce ayrılmış kadar bekleyin.

## <a name="create-vm-from-original-hard-disk"></a>Orijinal sabit diskten VM oluşturma
Özgün sanal sabit diskten bir VM oluşturmak için kullanın [bu Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet). Şablonun, önceki komuttan VHD URL'sini kullanarak mevcut bir sanal ağına bir VM dağıtır. Tıklayın **azure'a Dağıt** düğmesi gibi:

![Github'dan şablondan VM dağıtma](./media/troubleshoot-recovery-disks-portal-linux/deploy-template-from-github.png)

Şablon dağıtımı için Azure portalında yüklenir. Yeni VM ve mevcut Azure kaynaklarınıza adlarını girin ve mevcut sanal sabit diskinizi URL'sini yapıştırın. Dağıtımı başlatmak için tıklatın **satın alma**:

![Şablondan VM dağıtma](./media/troubleshoot-recovery-disks-portal-linux/deploy-from-image.png)


## <a name="re-enable-boot-diagnostics"></a>Önyükleme tanılaması yeniden etkinleştirin
Mevcut sanal sabit diskten VM oluşturma, önyükleme tanılamaları otomatik olarak etkinleştirilmemiş olabilir. Önyükleme tanılaması durumunu denetleyin ve gerekirse etkinleştirmek için portalda VM'nizi seçin. Altında **izleme**, tıklayın **tanılama ayarları**. Durum olduğundan emin olun **üzerinde**ve yanındaki onay işaretini **önyükleme tanılaması** seçilir. Herhangi bir değişiklik yaparsanız, tıklayın **Kaydet**:

![Önyükleme tanılama ayarları güncelleştir](./media/troubleshoot-recovery-disks-portal-linux/reenable-boot-diagnostics.png)

## <a name="troubleshoot-a-managed-disk-vm-by-attaching-a-new-os-disk"></a>Yeni bir işletim sistemi diskini ekleyerek bir yönetilen Disk sanal makinesinin sorunlarını giderme
1. Etkilenen sanal Makineyi durdurun.
2. [Yönetilen disk anlık görüntü oluşturma](../windows/snapshot-copy-managed-disk.md) yönetilen Disk sanal makinenin işletim sistemi diskinin.
3. [Anlık görüntüden yönetilen disk oluşturma](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md).
4. [VM veri diski olarak yönetilen diski](../windows/attach-disk-ps.md).
5. [Veri diski 4. adımdan işletim sistemi diskini değiştirmek](../windows/os-disk-swap.md).

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinenizde bağlanma sorunu yaşıyorsanız bkz [sorun giderme SSH bağlantıları için bir Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Sanal makinenizde çalışan uygulamalara erişim sorunları için bkz: [bir Linux sanal makinesinde uygulama bağlantı sorunlarını giderme](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Resource Manager kullanma hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
