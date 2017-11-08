---
title: "Azure'da iç içe geçmiş sanallaştırma kullanarak Azure VM ilgili bir sorun giderme | Microsoft Docs"
description: "Azure içinde iç içe geçmiş sanallaştırma kullanarak Azure VM ilgili bir sorun giderme"
services: virtual-machines-windows
documentationcenter: 
author: glimoli
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/06/2017
ms.author: genli
ms.openlocfilehash: 35f52af5fbf0c945a766f5e5431c885d91df546a
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
# <a name="troubleshoot-a-problem-azure-vm-by-using-nested-virtualization-in-azure"></a>Azure'da iç içe geçmiş sanallaştırma kullanarak Azure VM ilgili bir sorun giderme

Bu makalede, sorun giderme amacıyla Hyper-V Konağı (Kurtarma VM) VM sorununun disk bağlayabilir şekilde Microsoft Azure'da bir iç içe geçmiş sanallaştırma ortamı oluşturmak gösterilmiştir.

## <a name="prerequisite"></a>Önkoşul

VM sorun bağlamak için kurtarma VM aşağıdaki önkoşulları karşılaması gerekir:

-   Kurtarma VM sorun VM ile aynı konumda olması gerekir.

-   Kurtarma VM sorun VM ile aynı kaynak grubunda olması gerekir.

-   Kurtarma VM VM sorunu aynı türde depolama hesabı (standart veya Premium) kullanmanız gerekir.

## <a name="step-1-create-a-recovery-vm-and-install-hyper-v-role"></a>1. adım: Kurtarma VM oluşturma ve Hyper-V rolünü yükleyin

1.  Yeni bir kurtarma VM oluşturun:

    -  İşletim sistemi: Windows Server 2016 Datacenter

    -  Boyut: İç içe geçmiş destek sanallaştırma V3 serisiyle en az iki çekirdek. Daha fazla bilgi için bkz: [yeni Dv3 ve Ev3 VM boyutları Tanıtımı](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/).

    -  Aynı konum, depolama hesabı ve kaynak grubu olarak VM sorun.

    -  Aynı depolama türünü (standart veya Premium) VM sorunu seçin.

2.  Sonra kurtarma VM oluşturulduğunda, Kurtarma VM, Uzak Masaüstü'nü.

3.  Sunucu Yöneticisi'nde seçin **Yönet** > **rol ve Özellik Ekle**.

4.  İçinde **yükleme türü** bölümünde, select **rol tabanlı veya özellik tabanlı yükleme**.

5.  İçinde **Select hedef sunucu** bölümünde, Kurtarma VM'ın seçili olduğundan emin olun.

6.  Seçin **Hyper-V rolünü** > **Özellik Ekle**.

7.  Seçin **sonraki** üzerinde **özellikleri** bölümü.

8.  Bir sanal anahtar varsa, onu seçin. Aksi takdirde seçin **sonraki**.

9.  Üzerinde **geçiş** bölümünde, select **sonraki**

10. Üzerinde **varsayılan depolar** bölümünde, select **sonraki**.

11. Sunucu otomatik olarak yeniden başlat için onay kutusunu işaretleyin.

12. Seçin **yükleme**.

13. Hyper-V rolünü yüklemek için sunucu izin verin. Bu işlem birkaç dakika sürer ve sunucu otomatik olarak yeniden başlatılır.

## <a name="step-2-create-the-problem-vm-on-the-recovery-vms-hyper-v-server"></a>2. adım: Kurtarma VM'ın Hyper-V sunucusunda sorun VM oluşturma

1.  Disk adı sorun VM kaydetmek ve VM sorun silin. Tüm bağlı disklerde tutmak emin olun. 

2.  İşletim sistemi diski, sorunun VM kurtarma VM veri diski olarak ekleyin.

    1.  Sonra sorun VM silinir, Kurtarma VM gidin.

    2.  Seçin **diskleri**ve ardından **Ekle veri diski**.

    3.  Sorun VM'in disk seçin ve ardından **kaydetmek**.

3.  Sonra diski başarıyla bağlanmış, Uzak Masaüstü kurtarma VM sahiptir.

4.  Disk Yönetimi'ni (diskmgmt.msc) açın. Sorun disk VM olarak ayarlandığından emin olun **çevrimdışı**.

5.  Hyper-V Yöneticisi'ni açın: İçinde **Sunucu Yöneticisi'ni**seçin **Hyper-V rolünü**. Sunucuya sağ tıklayın ve ardından **Hyper-V Yöneticisi'ni**.

6.  Hyper-V Yöneticisi'nde kurtarma VM'ye sağ tıklayın ve ardından **yeni** > **sanal makine** > **sonraki**.

7.  VM için bir ad yazın ve ardından **sonraki**.

8.  Seçin **1. nesil**.

9.  Başlangıç belleği 1024 MB veya daha fazla ayarlayın.

10. Uygunsa oluşturulduğu Hyper-V ağ anahtarı seçin. Else sonraki sayfaya gidin.

11. Seçin **bir sanal sabit diski sonra Ekle**.

    ![görüntünün bir sonraki Sanal Sabit Disk seçeneği ekleme hakkında](./media/troubleshoot-vm-by-use-nested-virtualization/attach-disk-later.png)

12. Seçin **son** VM oluşturulduğunda.

13. Oluşturulan ve ardından VM'ye sağ tıklayın **ayarları**.

14. Seçin **IDE Denetleyicisi 0**seçin **sabit sürücü**ve ardından **Ekle**.

    ![Yeni sabit sürücü görüntü hakkında ekler](./media/troubleshoot-vm-by-use-nested-virtualization/create-new-drive.png)    

15. İçinde **fiziksel sabit Disk**, Azure VM'ye bağlı VM sorununun diski seçin. Listelenen tüm diskleri görmüyorsanız, diskin Disk Yönetimi'ni kullanarak çevrimdışı ayarlanırsa denetleyin.

    ![disk görüntüsü hakkında bağlar](./media/troubleshoot-vm-by-use-nested-virtualization/mount-disk.png)  


17. Seçin **Uygula**ve ardından **Tamam**.

18. VM çift tıklatın ve sonra başlatın.

19. Şimdi şirket içi VM olarak VM üzerinde çalışabilir. Gereksinim duyduğunuz herhangi bir sorun giderme adımları izleyebilirsiniz.

## <a name="step-3-re-create-your-azure-vm-in-azure"></a>3. adım: Azure VM'nizi azure'da yeniden oluşturma

1.  VM çevrimiçine aldıktan sonra Hyper-V Yöneticisi'ni VM'yi kapatın.

2.  Git [Azure portal](https://portal.azure.com) ve kurtarma VM seçin > diskler, disk adı kopyalayın. Sonraki adımda adını kullanacaksınız. VM kurtarma sabit diski kullanımdan çıkarın.

3.  Git **tüm kaynakları**aramak için disk adı ve disk seçin.

     ![disk görüntüsü hakkında arar](./media/troubleshoot-vm-by-use-nested-virtualization/search-disk.png)     

4. Tıklatın **VM oluşturma**.

     ![görüntünün hakkında vm diskten oluşturur.](./media/troubleshoot-vm-by-use-nested-virtualization/create-vm-from-vhd.png) 

Diskten VM oluşturmak için Azure PowerShell'i de kullanabilirsiniz. Daha fazla bilgi için bkz: [PowerShell kullanarak var olan bir diskten yeni VM oluşturma](create-vm-specialized.md#create-the-new-vm). 

## <a name="next-steps"></a>Sonraki adımlar

VM'nize bağlantı sorunları yaşıyorsanız, bkz: [bir Azure VM için RDP sorun giderme bağlantılarını](troubleshoot-rdp-connection.md). VM üzerinde çalışan uygulamalara erişim sorunları için bkz: [Windows VM üzerinde uygulama bağlantı sorunlarını giderme](troubleshoot-app-connection.md).
