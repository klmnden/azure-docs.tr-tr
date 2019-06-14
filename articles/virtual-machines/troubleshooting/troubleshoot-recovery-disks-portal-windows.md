---
title: Bir Windows VM Azure Portalı'nda sorun giderme kullanın | Microsoft Docs
description: İşletim sistemi diskini bir kurtarma sanal Makinesine Azure portalını kullanarak bağlanarak azure'da Windows sanal makine sorunlarını gidermeyi öğrenin
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: jeconnoc
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/13/2018
ms.author: genli
ms.openlocfilehash: ec2da7d9f659f32c40f7a2685ab08be4eec27ed5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60320932"
---
# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-the-azure-portal"></a>İşletim sistemi diskini bir kurtarma Azure portalını kullanarak sanal Makinesine ekleyerek bir Windows sanal makinesinin sorunlarını giderme
Azure'da Windows sanal makinesi (VM), önyükleme veya disk bir hatasıyla karşılaşırsa, sanal sabit diskin kendisinde sorun giderme adımları gerçekleştirmeniz gerekebilir. Yaygın olarak karşılaşılan örneklerden VM başarıyla önyükleme engelleyen bir uygulamanın güncelleştirme olacaktır. Bu makalede, sanal sabit diskinizi başka bir Windows varsa hataları düzeltin ve ardından orijinal VM'yi yeniden oluşturmak için VM'ye bağlanmak için Azure portalını kullanma işlemi açıklanmaktadır.

## <a name="recovery-process-overview"></a>Kurtarma işlemine genel bakış
Sorun giderme işlemi aşağıdaki gibidir:

1. Sanal sabit diskleri tutmak, sorun yaşayan VM'yi silin.
2. Ekleme ve sorun giderme amacıyla başka bir Windows VM için sanal sabit diski bağlayın.
3. Sorun giderme işlemlerini yapacağınız VM'ye bağlanın. Dosyaları düzenleyin veya özgün sanal sabit diskte sorunları gidermek için herhangi bir aracı çalıştırın.
4. Sorun giderme işlemlerini yaptığınız VM’den sanal sabit diski çıkarın.
5. Orijinal sanal sabit diski kullanarak bir VM oluşturun.

Bu kullanan yönetilen diskin VM için artık Azure PowerShell bir VM için işletim sistemi diskini değiştirmek için kullanabiliriz. Artık VM silip ihtiyacımız var. Daha fazla bilgi için [işletim sistemi diskini bir kurtarma için Azure PowerShell kullanarak VM ekleyerek bir Windows sanal makinesinin sorunlarını giderme](troubleshoot-recovery-disks-windows.md).

## <a name="determine-boot-issues"></a>Önyükleme sorunlarını belirleme
Neden sanal makinenizin doğru ön yükleyebileceğini değil belirlemek için önyükleme tanılama VM ekran inceleyin. Yaygın olarak karşılaşılan örneklerden başarısız bir uygulama güncelleştirmesinin ya da bir temel alınan sanal sabit silinmiş veya taşınmış disk gerekir.

Portalında VM'nizi seçin ve ardından aşağı kaydırarak **destek + sorun giderme** bölümü. Tıklayın **önyükleme tanılaması** ekran görüntülemek için. Herhangi bir özel hata iletileri veya hata kodları VM bir sorunla karşılaşan neden belirlemeye yardımcı olması için dikkat edin. 

![Konsol günlükleri görüntüleme VM önyükleme tanılaması](./media/troubleshoot-recovery-disks-portal-windows/screenshot-error.png)

Ayrıca **indirme ekran** bir VM ekran görüntüsü yakalama indirmek için.

## <a name="view-existing-virtual-hard-disk-details"></a>Mevcut sanal sabit disk ayrıntıları görüntüleyin
Sanal sabit diskinizi başka bir sanal makineye iliştirebilmek için önce sanal sabit disk (VHD) adını tanımlamak gerekir. 

Portalda kaynak grubunuzu seçin, ardından depolama hesabınızı seçin. Tıklayın **Blobları**, aşağıdaki örnekte olduğu gibi:

![Depolama BLOB'ları seçin](./media/troubleshoot-recovery-disks-portal-windows/storage-account-overview.png)

Genellikle, adlı bir kapsayıcıya sahip **VHD'ler** , sanal sabit disklerinizin depolar. Sanal sabit disklerin listesini görüntülemek üzere kapsayıcıyı seçin. (Önek genellikle sanal makinenizin adıdır) VHD'nizi adını not edin:

![Depolama kapsayıcısı VHD tanımlayın](./media/troubleshoot-recovery-disks-portal-windows/storage-container.png)

Listeden var olan sanal sabit diskinizi seçin ve aşağıdaki adımlarda kullanılmak URL'yi kopyalayın:

![Mevcut sanal sabit disk URL'sini Kopyala](./media/troubleshoot-recovery-disks-portal-windows/copy-vhd-url.png)


## <a name="delete-existing-vm"></a>Mevcut VM'yi silin
Sanal sabit diskler ve sanal makineler Azure'da iki ayrı kaynaktır. Bir sanal sabit disk, işletim sisteminin kendisi, uygulamalar ve yapılandırmalar depolandığı yerdir. VM boyutunu veya konumunu tanımlar ve bir sanal sabit disk veya sanal ağ arabirim kartı (NIC) gibi kaynaklara başvurur meta verilerdir. Her sanal sabit disk bir VM'ye atanan bir kira var. Veri diskleri VM çalışırken bile eklenip çıkarılabilir, ancak VM kaynağı silinmedikçe işletim sistemi diski çıkarılamaz. Kira, VM durdurulmuş ve serbest bırakılmış durumda olsa bile işletim sistemi diski ile bir VM ile ilişkisini sürdürür.

VM'nizi kurtarmanın ilk adımı, VM kaynağını silmektir. VM’yi sildiğinizde sanal sabit diskler depolama hesabınızda kalır. VM silindikten sonra sanal sabit diski ve hataları gidermek için başka bir sanal makineye ekleyin.

Portalında VM'nizi seçin ve ardından tıklayın **Sil**:

![VM önyükleme tanılama önyükleme hatası gösteren ekran görüntüsü](./media/troubleshoot-recovery-disks-portal-windows/stop-delete-vm.png)

VM sanal sabit diski başka bir sanal makineye eklemeden önce silme işlemlerinin tamamlanmasını bekleyin. Kira VM ile ilişkilendiren sanal sabit diski sanal sabit diski başka bir sanal makineye iliştirebilmek için önce yayımlanması gerekir.


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Mevcut sanal sabit diski başka bir VM'ye
Sonraki birkaç adımı için sorun giderme amacıyla başka bir VM kullanın. Varolan bir sanal sabit diski bulun ve diskin içeriği düzenlemek için bu sorun giderme sanal makinesine ekleyebilir. Bu işlem, yapılandırma hataları düzeltin veya ek uygulama veya sistem günlük dosyalarını, örneğin gözden geçirmek sağlar. Seçin veya sorun giderme amacıyla kullanmak üzere başka bir VM oluşturun.

1. Portalda kaynak grubunuzu seçin, sonra sorun giderme VM'nizi seçin. Seçin **diskleri** ve ardından **iliştirme varolan**:

    ![Portalda mevcut diski ekleme](./media/troubleshoot-recovery-disks-portal-windows/attach-existing-disk.png)

2. Mevcut sanal sabit diskinizi seçmek için **VHD Dosyası**’na tıklayın:

    ![Mevcut bir VHD'ye göz atma](./media/troubleshoot-recovery-disks-portal-windows/select-vhd-location.png)

3. Depolama hesabı ve kapsayıcı seçip mevcut VHD'NİZİ'ı tıklatın. Tıklayın **seçin** düğmesini Seçiminizi onaylayın:

    ![Mevcut VHD’nizi seçme](./media/troubleshoot-recovery-disks-portal-windows/select-vhd.png)

4. Artık VHD'niz seçiliyken, tıklayın **Tamam** mevcut sanal sabit diski eklemek için:

    ![Mevcut sanal sabit disk ekleme onaylayın](./media/troubleshoot-recovery-disks-portal-windows/attach-disk-confirm.png)

5. Birkaç saniye sonra **diskleri** VM'niz için bölmesi, mevcut sanal sabit veri diski olarak bağlı disk listeler:

    ![Veri diski olarak bağlı olan sanal sabit disk](./media/troubleshoot-recovery-disks-portal-windows/attached-disk.png)


## <a name="mount-the-attached-data-disk"></a>Bağlı veri diski takma

1. Sanal makinenize Uzak Masaüstü Bağlantısı açın. Portal ve tıklatın VM'nizi seçin **Connect**. İndirin ve RDP bağlantı dosyasını açın. Sanal makinenizde şu şekilde oturum açmak için kimlik bilgilerinizi girin:

    ![Sanal makinenize Uzak Masaüstü kullanarak oturum açın](./media/troubleshoot-recovery-disks-portal-windows/open-remote-desktop.png)

2. Açık **Sunucu Yöneticisi'ni**, ardından **dosya ve depolama hizmetleri**. 

    ![Dosya ve depolama hizmetleri Sunucu Yöneticisi içinde seçin](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

3. Veri diski otomatik olarak algılandı ve bağlı. Bağlı disklerin listesini görmek için seçin **diskleri**. Sürücü harfini birim bilgilerini görüntülemek için veri diski seçebilirsiniz. Aşağıdaki örnek, iliştirilmiş ve kullanarak veri diski gösterir **F:** :

    ![Bağlı disk ve birim bilgileri Sunucu Yöneticisi'nde](./media/troubleshoot-recovery-disks-portal-windows/server-manager-disk-attached.png)


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Özgün sanal sabit diskteki sorunları düzeltme
Mevcut sanal sabit bağlı disk ile artık tüm bakım ve sorun giderme adımlarını gereken şekilde gerçekleştirebilirsiniz. Sorunları giderdikten sonra aşağıdaki adımlarla devam edin.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Çıkarın ve özgün sanal sabit disk detach
Mevcut sanal sabit diski sorun giderme, hataları çözümlendikten sonra çıkarın. Sorun giderme sanal makinesine sanal sabit disk ekleme kira ilişkisini sonlandırana kadar sanal sabit diskinizi başka bir VM ile kullanamazsınız.

1. Sanal makinenize yönelik RDP oturumundan açın **Sunucu Yöneticisi'ni**, ardından **dosya ve depolama hizmetleri**:

    ![Sunucu Yöneticisi'nde dosya ve depolama hizmetleri seçin](./media/troubleshoot-recovery-disks-portal-windows/server-manager-select-storage.png)

2. Seçin **diskleri** ve ardından veri diskinizi seçin. Veri diskinizi sağ tıklayıp **çevrimdışına**:

    ![Veri diski, Sunucu Yöneticisi'nde çevrimdışı olarak ayarlayın](./media/troubleshoot-recovery-disks-portal-windows/server-manager-set-disk-offline.png)

3. Artık VM'den sanal sabit diski çıkarın. Azure portalında VM'nizi seçin ve tıklayın **diskleri**. Mevcut sanal sabit diskinizi seçin ve ardından **ayırma**:

    ![Mevcut sanal sabit diski ayırma](./media/troubleshoot-recovery-disks-portal-windows/detach-disk.png)

    VM başarıyla veri diski devam etmeden önce ayrılmış kadar bekleyin.

## <a name="create-vm-from-original-hard-disk"></a>Orijinal sabit diskten VM oluşturma
Özgün sanal sabit diskten bir VM oluşturmak için kullanın [bu Azure Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-new-or-existing-vnet). Şablonun, önceki komuttan VHD URL'sini kullanarak mevcut veya yeni sanal ağına, bir VM dağıtır. Tıklayın **azure'a Dağıt** düğmesi gibi:

![Github'dan şablondan VM dağıtma](./media/troubleshoot-recovery-disks-portal-windows/deploy-template-from-github.png)

Şablon dağıtımı için Azure portalında yüklenir. Yeni VM ve mevcut Azure kaynaklarınıza adlarını girin ve mevcut sanal sabit diskinizi URL'sini yapıştırın. Dağıtımı başlatmak için tıklatın **satın alma**:

![Şablondan VM dağıtma](./media/troubleshoot-recovery-disks-portal-windows/deploy-from-image.png)


## <a name="re-enable-boot-diagnostics"></a>Önyükleme tanılaması yeniden etkinleştirin
Mevcut sanal sabit diskten VM oluşturma, önyükleme tanılamaları otomatik olarak etkinleştirilmemiş olabilir. Önyükleme tanılaması durumunu denetleyin ve gerekirse etkinleştirmek için portalda VM'nizi seçin. Altında **izleme**, tıklayın **tanılama ayarları**. Durum olduğundan emin olun **üzerinde**ve yanındaki onay işaretini **önyükleme tanılaması** seçilir. Herhangi bir değişiklik yaparsanız, tıklayın **Kaydet**:

![Önyükleme tanılama ayarları güncelleştir](./media/troubleshoot-recovery-disks-portal-windows/reenable-boot-diagnostics.png)

## <a name="next-steps"></a>Sonraki adımlar
Sanal makinenizde bağlanma sorunu yaşıyorsanız bkz [Azure VM'ye RDP sorunlarını giderme bağlantıları](troubleshoot-rdp-connection.md). Sanal makinenizde çalışan uygulamalara erişim sorunları için bkz: [bir Windows sanal makinesinde uygulama bağlantı sorunlarını giderme](troubleshoot-app-connection.md).

Resource Manager kullanma hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md).

