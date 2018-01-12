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
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/11/2017
ms.author: eamono
ms.custom: mvc
ms.openlocfilehash: e277aa44dfe625780d72a78010f0638c73a6b182
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
---
# <a name="onboard-update-and-change-tracking-solutions-to-azure-automation"></a>Yerleşik güncelleştirme ve değişiklik çözümleri için Azure Otomasyonu izleme

Bu öğreticide, bilgi Azure Otomasyonu VM'ler için otomatik olarak yerleşik güncelleştirme, değişiklik izleme ve stok çözümleri nasıl:

> [!div class="checklist"]
> * Yerleşik bir Azure VM
> * Çözümlerle
> * Yükleme ve modülleri güncelleştirme
> * Onboarding runbook'u İçeri Aktar
> * Runbook'u Başlat

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gereklidir:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da [ücretsiz hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) için kaydolabilirsiniz.
* [Otomasyon hesabı](automation-offering-get-started.md) makineleri yönetmek için.
* Sisteme eklenecek bir [sanal makine](../virtual-machines/windows/quick-create-portal.md).

## <a name="onboard-an-azure-vm"></a>Yerleşik bir Azure VM

Yerleşik Azure sanal makineleri için değişiklik izleme veya güncelleştirme yönetimi çözümü ile edildi otomatik olarak mevcut bir VM'yi olmalıdır. Bu adımda, yerleşik bir sanal makineyle güncelleştirme yönetimi ve değişiklik izleme.

### <a name="enable-change-tracking-and-inventory"></a>Değişiklik izleme ve stok etkinleştir

Değişiklik izleme ve stok çözüm olanağı sağlar [Değişiklikleri İzle](automation-vm-change-tracking.md) ve [stok](automation-vm-inventory.md) sanal makinelerinizin üzerinde. Bu adımda, bir sanal makinede çözüm etkinleştirin.

1. Sol menüden seçin **Automation hesapları**ve ardından listede Otomasyon hesabınızı seçin.
1. Seçin **stok** altında **yapılandırma yönetimi**.
1. Varolan bir günlük analizi çalışma alanını seçin veya yeni oluşturun. Tıklatın **etkinleştirmek** düğmesi.

![Yerleşik güncelleştirme çözümü](media/automation-onboard-solutions/inventory-onboard.png)

Değişiklik izleme ve envanter çözüm ekleme bildirimi tamamlandığında tıklayın **güncelleştirme yönetimi** altında **yapılandırma yönetimi**.

### <a name="enable-update-management"></a>Güncelleştirme yönetimi etkinleştir

Güncelleştirme yönetimi çözümü, güncelleştirme ve düzeltme eklerini Azure Windows Vm'leriniz için yönetmenizi sağlar. Kullanılabilir güncelleştirmeleri, gerekli güncelleştirmelerin zamanlama yükleme durumunu değerlendirmek ve güncelleştirmeleri doğrulamak için dağıtım sonuçları gözden geçirin VM başarıyla uygulandı. Bu adımda, VM için çözüm sağlar.

1. Otomasyon hesabınızdan seçin **güncelleştirme yönetimi** altında **güncelleştirme yönetimi**.
1. Seçilen günlük analizi çalışma alanı önceki adımda kullanılan aynı çalışma alanıdır. Tıklatın **etkinleştirmek** onboarding için güncelleştirme yönetimi çözümü.

![Yerleşik güncelleştirme çözümü](media/automation-onboard-solutions/update-onboard.png)

Güncelleştirme yönetimi çözümü yüklenirken, mavi bir başlık gösterilir. Çözüm etkinleştirildiğinde seçin **değişiklik izleme** altında **yapılandırma yönetimi** sonraki adıma gidin.

### <a name="select-azure-vm-to-be-managed"></a>Yönetilecek Azure VM seçin

Çözümlerin etkinleştirilir, bu çözümler giriş için bir Azure VM ekleyebilirsiniz.

1. Otomasyon hesabınızdan üzerinde **değişiklik izleme** sayfasında, **+ Azure VM eklemek** , sanal makinenize eklemek için.

1. Liste ve seçin, VM seçin **etkinleştirmek**. Bu eylem değişiklik tacking ve sanal makine için stok çözümü sağlar.

   ![Vm için Güncelleştirme çözümü etkinleştir](media/automation-onboard-solutions/enable-change-tracking.png)

1. VM ekleme bildirim tamamlandığında, Otomasyon hesabı seçin **güncelleştirme yönetimi** altında **güncelleştirme yönetimi**.

1. Seçin **+ Azure VM eklemek** , sanal makinenize eklemek için.

1. Liste ve seçin, VM seçin **etkinleştirmek**. Bu eylem, sanal makine için güncelleştirme yönetimi çözümü sağlar.

   ![Vm için Güncelleştirme çözümü etkinleştir](media/automation-onboard-solutions/enable-update.png)

> [!NOTE]
> Diğer çözüm, sonraki çözüm etkinleştirilirken tamamlanmasını bekleme belirten bir ileti alırsınız: *başka bir çözüm yüklemesi devam ediyor bu veya başka bir sanal makine. Yükleme tamamlandığında etkinleştir düğmesi etkin ve çözüm bu sanal makine üzerinde yüklemesini isteyebilir.*

## <a name="install-and-update-modules"></a>Yükleme ve modülleri güncelleştirme

En son Azure modüllerle güncelleştirmek ve içeri aktarmak için gerekli olan `AzureRM.OperationalInsights` başarıyla çözüm ekleme otomatik hale getirmek için.

## <a name="update-azure-modules"></a>Azure modülleri güncelleştir

Otomasyon hesabınızdan seçin **modülleri** altında **paylaşılan kaynakları**. Seçin **güncelleştirme Azure modülleri** Azure modülleri en son sürüme güncelleştirmek için. Seçin **Evet** tüm mevcut Azure modülleri en son sürüme güncelleştirmek için komut çubuğunda.

![Güncelleştirme modülleri](media/automation-onboard-solutions/update-modules.png)

### <a name="install-azurermoperationalinsights-module"></a>AzureRM.OperationalInsights modülünü yükleme

Gelen **modülleri** sayfasında, **Gözat galeri** modülü Galerisi'ni açın. AzureRM.OperationalInsights için arama yapın ve bu modül Otomasyon dikkate alın.

![OperationalInsights modülünü içeri aktarın](media/automation-onboard-solutions/import-operational-insights-module.png)

## <a name="import-the-onboarding-runbook"></a>Onboarding runbook'u İçeri Aktar

1. Otomasyon hesabınızdan seçin **Runbook'lar** altında **işlem OTOMASYONU**.
1. Seçin **Gözat galeri**.
1. Arama **güncelleştirin ve değişiklik izleme**, runbook tıklatın ve seçin **alma** üzerinde **kaynağı görüntüle** sayfası. Seçin **Tamam** runbook Otomasyon dikkate alınacak.

  ![Onboarding runbook'u İçeri Aktar](media/automation-onboard-solutions/import-from-gallery.png)

1. Üzerinde **Runbook** sayfasında, **Düzenle**seçeneğini belirleyip **Yayımla**. Üzerinde **yayımlama Runbook** iletişim kutusunda **Evet** runbook'u yayımlamak için.

## <a name="start-the-runbook"></a>Runbook'u Başlat

Değişiklik izleme veya çözümleri bu runbook'u başlatmak için bir Azure VM güncelleştirmesi edildi olması gerekir. Var olan sanal makine ve çözüm edildi ile kaynak grubu için parametreler gerektirir.

1. Enable-MultipleSolution runbook'u açın.

   ![Birden çok çözüm runbook'ları](media/automation-onboard-solutions/runbook-overview.png)

1. Başlat düğmesine tıklayın ve parametreler için aşağıdaki değerleri girin.

   * **VMNAME** -boş bırakın. Güncelleştirme veya değişiklik izleme çözümü eklemek için var olan bir VM adı. Bu değer boş bırakarak, kaynak grubundaki tüm sanal makineleri edildi var.
   * **VMRESOURCEGROUP** -edildi olmasını VM'ler için kaynak grubunun adı.
   * **SUBSCRİPTİONID** -boş bırakın. Yeni VM edildi olması için abonelik kimliği. Boş bırakılırsa, çalışma alanının abonelik kullanılır. Farklı bir abonelik kimliği verildiğinde bu Otomasyon hesabı için RunAs hesabı olarak katkıda bulunan bu abonelik için de eklenmelidir.
   * **ALREADYONBOARDEDVM** -el ile güncelleştirmeleri ya da ChangeTracking edildi olan VM adını çözümü.
   * **ALREADYONBOARDEDVMRESOURCEGROUP** -VM bir üyesi olan kaynak grubunun adı.
   * **SOLUTIONTYPE** -girin **güncelleştirmeleri** veya **ChangeTracking**

   ![Enable-MultipleSolution runbook parametreleri](media/automation-onboard-solutions/runbook-parameters.png)

1. Seçin **Tamam** runbook işi başlatmak için.
1. İlerlemeyi izleme ve runbook işi sayfadaki hataları.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Yerleşik bir Azure sanal makinesi el ile.
> * Yükleyin ve gerekli Azure modüllerini güncelleştirin.
> * Bir runbook bu onboards Azure sanal makineleri içeri aktarın.
> * Runbook bu onboards Azure sanal makineleri otomatik olarak başlar.

Runbook'ları zamanlama hakkında daha fazla bilgi için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Runbook'ları zamanlama](automation-schedules.md).
