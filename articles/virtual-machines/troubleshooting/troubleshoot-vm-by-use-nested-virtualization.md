---
title: Azure'da iç içe sanallaştırma'yı kullanarak Azure VM sorun gidermeye | Microsoft Docs
description: Azure'da iç içe sanallaştırma kullanarak bir Azure VM sorun giderme
services: virtual-machines-windows
documentationcenter: ''
author: glimoli
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: c84d015da907c8792f09d1d60e6bc8eddb7e2957
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60444366"
---
# <a name="troubleshoot-a-problem-azure-vm-by-using-nested-virtualization-in-azure"></a>Azure'da iç içe sanallaştırma kullanarak Azure VM ilgili bir sorun giderme

Bu makalede, sorun giderme amacıyla (Kurtarma VM) Hyper-V konağında sorunu VM disk bağlayabileceğiniz şekilde, Microsoft Azure'da iç içe sanallaştırma ortamı oluşturma işlemini gösterir.

## <a name="prerequisites"></a>Önkoşullar

VM sorun bağlamak için kurtarma VM aşağıdaki gereksinimleri karşılaması gerekir:

-   Kurtarma VM sorun VM ile aynı konumda olmalıdır.

-   Kurtarma VM sorun VM ile aynı kaynak grubunda olması gerekir.

-   Kurtarma VM, sorunlu VM aynı türde (standart veya Premium) depolama hesabı kullanmanız gerekir.

## <a name="step-1-create-a-rescue-vm-and-install-hyper-v-role"></a>1\. adım: Kurtarma sanal makine oluşturma ve Hyper-V rolünü yükleme

1.  Yeni bir kurtarma sanal makine oluşturun:

    -  İşletim Sistemi: Windows Server 2016 Datacenter

    -  Boyut: Herhangi bir V3 seri iç içe sanallaştırmayı destekleyen en az iki çekirdek. Daha fazla bilgi için [yeni Dv3 ve Ev3 VM boyutları ile tanışın](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/).

    -  Aynı konum, depolama hesabı ve kaynak grubunda VM sorun.

    -  Aynı depolama türüne, sorunlu VM (standart veya Premium) seçin.

2.  Sonra kurtarma VM oluşturulduğunda, Kurtarma VM'ye Uzak Masaüstü Bağlantısı.

3.  Sunucu Yöneticisi'nde **Yönet** > **rol ve Özellik Ekle**.

4.  İçinde **yükleme türünü** bölümünden **rol tabanlı veya özellik tabanlı yükleme**.

5.  İçinde **hedef sunucuyu seçin** bölümünde, Kurtarma VM'ın seçili olduğundan emin olun.

6.  Seçin **Hyper-V rolünü** > **Özellik Ekle**.

7.  Seçin **sonraki** üzerinde **özellikleri** bölümü.

8.  Bir sanal anahtar varsa, onu seçin. Aksi takdirde seçin **sonraki**.

9.  Üzerinde **geçiş** bölümünden **İleri**

10. Üzerinde **varsayılan depolar** bölümünden **sonraki**.

11. Gerekli olursa sunucuyu yeniden başlatmanız kutuyu işaretleyin.

12. **Yükle**’yi seçin.

13. Hyper-V rolünü yüklemek için sunucu izin verin. Bu işlem birkaç dakika sürer ve sunucu otomatik olarak yeniden başlatılır.

## <a name="step-2-create-the-problem-vm-on-the-rescue-vms-hyper-v-server"></a>2\. adım: Kurtarma sanal makinenin Hyper-V sunucusunda sorunu VM oluşturma

1.  Sorunu VM disk adını kaydetmek ve VM sorun silin. Tüm bağlı diskleri tutmak olduğundan emin olun. 

2.  Sorununuzu VM işletim sistemi diskini bir kurtarma VM veri diski olarak ekleyin.

    1.  Sorun VM silindikten sonra kurtarma sanal Makineye gidin.

    2.  Seçin **diskleri**, ardından **Ekle veri diski**.

    3.  Sorun VM'nin diskini seçin ve ardından **Kaydet**.

3.  Kurtarma sanal makinesine bağlanmış, Uzak Masaüstü başarıyla sonra diski var.

4.  Disk Yönetimi'ni (diskmgmt.msc) açın. Sorunun diskin VM olarak ayarlandığından emin olun **çevrimdışı**.

5.  Hyper-V Yöneticisi'ni açın: İçinde **Sunucu Yöneticisi'ni**seçin **Hyper-V rolünü**. Sunucuya sağ tıklayın ve ardından **Hyper-V Yöneticisi'ni**.

6.  Hyper-V Yöneticisi'nde, Kurtarma VM'ye sağ tıklayın ve ardından **yeni** > **sanal makine** > **sonraki**.

7.  VM için bir ad yazın ve ardından **sonraki**.

8.  Seçin **1. kuşak**.

9.  Başlangıç belleğini 1024 MB veya daha fazla ayarlayın.

10. Oluşturulan Hyper-V ağ anahtarı (varsa) seçin. Aksi takdirde sonraki sayfaya taşıyın.

11. Seçin **daha sonra bir sanal sabit disk ekleme**.

    ![bir sonraki Sanal Sabit Disk seçeneği ekleme hakkında görüntü](media/troubleshoot-vm-by-use-nested-virtualization/attach-disk-later.png)

12. Seçin **son** VM oluşturulduğunda.

13. Sağ tıklayın, oluşturduğunuz ve ardından VM **ayarları**.

14. Seçin **IDE Denetleyicisi 0**seçin **sabit sürücüsü**ve ardından **Ekle**.

    ![Yeni sabit sürücü görüntüsü hakkında ekler](media/troubleshoot-vm-by-use-nested-virtualization/create-new-drive.png)    

15. İçinde **fiziksel sabit Disk**, sorunun, Azure VM'ye VM diski seçin. Listelenen tüm diskleri görmüyorsanız, diskin Disk Yönetimi'ni kullanarak çevrimdışı ayarlanırsa denetleyin.

    ![görüntünün hakkında diskini bağlar](media/troubleshoot-vm-by-use-nested-virtualization/mount-disk.png)  


17. Seçin **Uygula**ve ardından **Tamam**.

18. VM'de çift tıklayın ve sonra başlatın.

19. Artık şirket içi VM olarak VM üzerinde çalışabilirsiniz. Gereksinim duyduğunuz herhangi bir sorun giderme adımları izleyebilirsiniz.

## <a name="step-3-re-create-your-azure-vm-in-azure"></a>3\. adım: Azure'da Azure VM yeniden oluşturma

1.  VM çevrimiçine aldıktan sonra sanal Makineye Hyper-V Yöneticisi'ni kapatın.

2.  Git [Azure portalında](https://portal.azure.com) ve kurtarma VM'yi seçin > diskler, diskin adını kopyalayın. Sonraki adımda ad kullanır. Kurtarma VM'den sabit diski çıkarın.

3.  Git **tüm kaynakları**, disk adını arayın ve ardından diski seçin.

     ![disk görüntüsü hakkında arar](media/troubleshoot-vm-by-use-nested-virtualization/search-disk.png)     

4. Tıklayın **VM oluşturma**.

     ![görüntünün hakkında diskten vm oluşturur.](media/troubleshoot-vm-by-use-nested-virtualization/create-vm-from-vhd.png) 

Diskten VM oluşturmak için Azure PowerShell'i de kullanabilirsiniz. Daha fazla bilgi için [PowerShell kullanarak var olan bir diskten yeni bir VM oluşturma](../windows/create-vm-specialized.md#create-the-new-vm). 

## <a name="next-steps"></a>Sonraki adımlar

Sanal makinenizde bağlanma sorunu yaşıyorsanız bkz [Azure VM'ye RDP sorunlarını giderme bağlantıları](troubleshoot-rdp-connection.md). Sanal makinenizde çalışan uygulamalara erişim sorunları için bkz: [bir Windows sanal makinesinde uygulama bağlantı sorunlarını giderme](troubleshoot-app-connection.md).
