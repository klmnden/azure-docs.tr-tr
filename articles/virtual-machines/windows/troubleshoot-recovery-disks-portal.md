---
title: Bir Windows Azure portalında VM sorun giderme kullanın | Microsoft Docs
description: Kurtarma için Azure portalını kullanarak VM işletim sistemi diski bağlanarak azure'da Windows sanal makine sorunlarını giderme hakkında bilgi edinin
services: virtual-machines-windows
documentationCenter: ''
authors: genlin
manager: jeconnoc
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: 523bf4055b4181715efd2c2d67e4dde086e7540e
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a>Kurtarma için Azure portalını kullanarak VM işletim sistemi diski ekleyerek Windows VM sorun giderme
Azure, Windows sanal makine (VM) önyükleme veya disk bir hatayla karşılaştığında, sanal sabit diskin kendisinde sorun giderme adımları gerçekleştirmeniz gerekebilir. Yaygın bir örnek VM başarıyla önyükleme engeller başarısız uygulama güncelleştirmesi olacaktır. Bu makalede Azure portal, sanal sabit diski başka bir Windows hataları düzeltin, sonra özgün VM'yi yeniden oluşturmak için VM'e bağlanmak için nasıl kullanılacağını ayrıntılarını verir.

## <a name="recovery-process-overview"></a>Kurtarma işlemine genel bakış
Sorun giderme işlemi aşağıdaki gibidir:

1. Sorunlarla, sanal sabit diskleri tutmak VM silin.
2. Ekleme ve sorun giderme amacıyla başka bir Windows VM için sanal sabit diski bağlama.
3. Sorun giderme işlemlerini yapacağınız VM'ye bağlanın. Dosyaları düzenleyin veya özgün sanal sabit diskte sorunlarını gidermek için herhangi bir aracı çalıştırın.
4. Sorun giderme işlemlerini yaptığınız VM’den sanal sabit diski çıkarın.
5. Özgün sanal sabit disk kullanan bir VM oluşturun.


## <a name="determine-boot-issues"></a>Önyükleme sorunlarını belirleme
Neden VM'yi doğru önyükleme mümkün değil belirlemek için önyükleme tanılaması VM ekran inceleyin. Başarısız uygulama güncelleştirme ya da bir temel sanal sabit silindiğinde veya taşındığında disk yaygın bir örnek olacaktır.

Portalda, VM seçin ve sonra aşağı kaydırın **destek + sorun giderme** bölümü. Tıklatın **önyükleme tanılama** ekran görüntülemek için. Herhangi bir özel hata iletileri veya hata kodları VM bir sorunla karşılaşan neden belirlemenize yardımcı olması için unutmayın. Aşağıdaki örnek, hizmetleri durdurma bekleniyor VM gösterir:

![Önyükleme tanılaması konsol günlükleri VM görüntüleme](./media/troubleshoot-recovery-disks-portal/screenshot-error.png)

Tıklatarak **ekran** VM ekran yakalama karşıdan yüklemek için.


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

1. Uzak Masaüstü Bağlantısı, VM açın. Portal ve tıklatın, VM seçin **Bağlan**. Karşıdan yükle ve RDP bağlantı dosyasını açın. VM'nize gibi oturum açmak için kimlik bilgilerinizi girin:

    ![VM'nize Uzak Masaüstü kullanarak oturum açın](./media/troubleshoot-recovery-disks-portal/open-remote-desktop.png)

2. Açık **Sunucu Yöneticisi'ni**seçeneğini belirleyip **dosya ve depolama hizmetleri**. 

    ![Dosya ve depolama hizmetleri Sunucu Yöneticisi içinde seçin](./media/troubleshoot-recovery-disks-portal/server-manager-select-storage.png)

3. Veri diski otomatik olarak algılanır ve bağlı. Bağlı disklerin listesini görmek için seçin **diskleri**. Sürücü harfi birim bilgilerini görüntülemek için veri diski seçebilirsiniz. Aşağıdaki örnek, iliştirilmiş ve kullanarak veri diski gösterir **F:**:

    ![Bağlı disk ve birim bilgileri Sunucu Yöneticisi'nde](./media/troubleshoot-recovery-disks-portal/server-manager-disk-attached.png)


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Özgün sanal sabit diskinde ilgili sorunları giderme
Varolan bir sanal sabit takılı diski ile tüm bakım ve sorun giderme adımları gereken şekilde artık gerçekleştirebilirsiniz. Sorunları giderdikten sonra aşağıdaki adımlarla devam edin.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Çıkarın ve özgün sanal sabit disk ayırma
Hatalarınızı çözüldükten sonra sorun giderme VM varolan sanal sabit diski kullanımdan çıkarın. Sorun giderme VM için sanal sabit disk ekleme kira serbest kadar herhangi bir VM ile sanal sabit disk kullanamazsınız.

1. VM için RDP oturumundan açmak **Sunucu Yöneticisi'ni**seçeneğini belirleyip **dosya ve depolama hizmetleri**:

    ![Sunucu Yöneticisi'nde dosya ve depolama hizmetleri seçin](./media/troubleshoot-recovery-disks-portal/server-manager-select-storage.png)

2. Seçin **diskleri** ve veri diskinizi seçin. Veri diskinizi sağ tıklatın ve seçin **Çevrimdışına Al**:

    ![Sunucu Yöneticisi'nde çevrimdışı olarak veri diski ayarlayın](./media/troubleshoot-recovery-disks-portal/server-manager-set-disk-offline.png)

3. Şimdi VM sanal sabit diski kullanımdan çıkarın. Azure portalında, VM seçin ve tıklatın **diskleri**. Varolan bir sanal sabit diski seçin ve ardından **ayırma**:

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

## <a name="next-steps"></a>Sonraki adımlar
VM'nize bağlantı sorunları yaşıyorsanız, bkz: [bir Azure VM için RDP sorun giderme bağlantılarını](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). VM üzerinde çalışan uygulamalara erişim sorunları için bkz: [Windows VM üzerinde uygulama bağlantı sorunlarını giderme](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Kaynak Yöneticisi'ni kullanma hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
