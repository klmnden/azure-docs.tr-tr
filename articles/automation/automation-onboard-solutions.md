---
title: "Yerleşik güncelleştirme ve değişiklik çözümleri için Azure Otomasyonu izleme | Microsoft Docs"
description: "Bilgi yerleşik güncelleştirme ve değişiklik çözümleri için Azure Otomasyonu izleme."
services: automation
documentationcenter: 
author: eamonoreilly
manager: 
editor: 
ms.assetid: edae1156-2dc7-4dab-9e5c-bf253d3971d0
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2017
ms.author: eamono
ms.openlocfilehash: 9ae5e9ebcbcf3af61318eac98defc6b249417f41
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2017
---
# <a name="onboard-update-and-change-tracking-solutions-to-azure-automation"></a>Yerleşik güncelleştirme ve değişiklik çözümleri için Azure Otomasyonu izleme

Bu öğreticide, bilgi Azure Otomasyonu VM'ler için otomatik olarak yerleşik güncelleştirme, değişiklik izleme ve stok çözümleri nasıl:

> [!div class="checklist"]
> * Yerleşik bir Azure sanal makinesi el ile.
> * Yükleyin ve gerekli Azure modüllerini güncelleştirin.
> * Bir runbook bu onboards Azure sanal makineleri içeri aktarın.
> * Runbook bu onboards Azure sanal makineleri otomatik olarak başlar.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir.
+ Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
+ [Otomasyon hesabı](automation-offering-get-started.md) makineleri yönetmek için.

## <a name="onboard-an-azure-virtual-machine-manually"></a>Yerleşik bir Azure sanal makinesi el ile

1.  Automation hesabını açın.
2.  Stok sayfasında tıklayın.
![Yerleşik stok çözümü](media/automation-onboard-solutions/inventory-onboard.png)
3.  Varolan bir günlük analizi çalışma alanını seçin veya yeni oluşturun. Etkinleştir düğmesini tıklatın.
4.  Değişiklik izleme ve envanter çözüm ekleme bildirimi tamamlandığında, güncelleştirme Yönetimi sayfasında'yi tıklatın.
![Yerleşik güncelleştirme çözümü](media/automation-onboard-solutions/update-onboard.png)
4.  Bu onboards güncelleştirme çözümü Etkinleştir düğmesini tıklatın.
5.  Güncelleştirme çözüm ekleme bildirimi tamamlandığında, değişiklik izleme sayfasında'yi tıklatın.
![Yerleşik değişiklik izleme](media/automation-onboard-solutions/change-tracking-onboard-vm.png)
6.  Azure VM Ekle düğmesine tıklayın.
![Değişiklik izleme için VM seçin](media/automation-onboard-solutions/enable-change-tracking.png)
7.  Bir Azure VM seçin ve değişiklik izleme ve envanter çözümü giriş için Etkinleştir düğmesini tıklatın.
8.  VM ekleme bildirim tamamlandığında, güncelleştirme Yönetimi sayfasında'yi tıklatın.
![Güncelleştirme yönetimi için yerleşik VM](media/automation-onboard-solutions/update-onboard-vm.png)
9.  Azure VM Ekle düğmesine tıklayın.
![Güncelleştirme yönetimi için VM seçin](media/automation-onboard-solutions/enable-update.png)
10.  Bir Azure VM seçin ve güncelleştirme yönetimi çözümü giriş için Etkinleştir düğmesini tıklatın.

## <a name="install-and-update-required-azure-modules"></a>Yükleme ve gerekli Azure modüllerini güncelleştirme

En son Azure modüllerle güncelleştirmek ve başarılı bir şekilde çözüm ekleme otomatik hale getirmek için AzureRM.OperationalInsights almak için gereklidir.
1.  Modülleri sayfasında tıklayın.
![Güncelleştirme modülleri](media/automation-onboard-solutions/update-modules.png)
2.  En son sürüme güncelleştirir güncelleştirme Azure modülleri düğmesine tıklayın. Güncelleştirmesinin tamamlanmasını bekleyin.
3.  Modül Galerisi'ni açmak için Gözat galeri düğmesine tıklayın.
![OperationalInsights modülünü içeri aktarın](media/automation-onboard-solutions/import-operational-insights-module.png)
4.  AzureRM.OperationalInsights için arama yapın ve bu modül Otomasyon dikkate alın.

## <a name="import-a-runbook-that-onboards-solutions-to-azure-vms"></a>Bir runbook bu onboards çözümleri Azure Vm'lerine alın.

1.  "İşlem OTOMASYONU" kategorisi altında Runbook'lar sayfasında'i tıklatın.
2.  "Gözat Galerisi" düğmesine tıklayın.
3.  "Güncelleştirmek ve değişiklik izleme" arama ve runbook Otomasyon dikkate alın.
![Onboarding runbook'u İçeri Aktar](media/automation-onboard-solutions/import-from-gallery.png)
4.  Runbook kaynağı görüntüleyip "Yayımla" düğmesine tıklayın "Düzenle"'i tıklatın.
![Onboarding runbook'u İçeri Aktar](media/automation-onboard-solutions/publish-runbook.png)

## <a name="start-the-runbook-that-onboards-azure-vms-automatically"></a>Runbook bu onboards Azure sanal makineleri otomatik olarak Başlat

Değişiklik izleme veya çözümleri bu runbook'u başlatmak için bir Azure VM güncelleştirmesi edildi olması gerekir. Var olan sanal makine ve çözüm edildi ile kaynak grubu için parametreler gerektirir.
1.  Enable-MultipleSolution runbook'u açın.
![Birden çok çözüm runbook'ları](media/automation-onboard-solutions/runbook-overview.png)
2.  Başlat düğmesine tıklayın ve parametreler için aşağıdaki değerleri girin.
    *   VMNAME. Güncelleştirme veya değişiklik izleme çözümü eklemek için var olan bir VM adı. Boş bırakın ve kaynak grubundaki tüm sanal makineleri edildi markalarıdır.
    *   VMRESOURCEGROUP. VM üyesi olan kaynak grubunun adı.
    *   SUBSCRİPTİONID. Abonelik kimliği yeni VM yerleşik bulunur. Boş bırakın ve çalışma alanının abonelik kullanılır. Farklı bir abonelik kimliği verildiğinde bu Otomasyon hesabı için RunAs hesabı olarak katkıda bulunan bu abonelik için de eklenmelidir.
    *   ALREADYONBOARDEDVM. El ile güncelleştirmeleri ya da ChangeTracking edildi olan VM adını çözümü.
    *   ALREADYONBOARDEDVMRESOURCEGROUP. VM üyesi olan kaynak grubunun adı.
    *   SOLUTIONTYPE. "Güncelleştirmeler" veya "ChangeTracking" girin
    
    ![Birden çok çözüm runbook parametreleri](media/automation-onboard-solutions/runbook-parameters.png)
3.  Runbook işi başlatmak için Tamam'ı tıklatın.
4.  İlerlemeyi izleme ve runbook işi sayfadaki hataları.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz: [runbook'ları zamanlama](automation-schedules.md).