---
title: "Bilgi yerleşik güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri Azure Automation nasıl"
description: "Nasıl için yerleşik bir Azure sanal makine Azure Otomasyonu parçası olan güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri ile bilgi edinin"
services: automation
keywords: 
author: georgewallace
ms.author: gwallace
ms.date: 02/28/2018
ms.topic: article
ms.service: automation
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 670a0c2a11ebfe09406233ab4b454b2e9c2ba0e0
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions"></a>Yerleşik güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri

Azure Otomasyonu bilgisayarlarınızda yüklü stok işletim sistemi güvenlik güncelleştirmeleri yönetmek ve değişiklikleri izlemek için çözüm sağlar. Yerleşik makinelere birden çok yolu vardır, çözüm gerçekleştirebilir [bir sanal makineden](automation-onboard-solutions-from-vm.md), Otomasyon hesabınızdan veya göre [runbook](automation-onboard-solutions.md). Bu makalede onboarding Automation hesabınız bu çözümlerden kapsar.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure oturumu açın

## <a name="enable-solutions"></a>Çözümlerle

Otomasyon hesabınıza gidin ve seçin **stok** veya **değişiklik izleme** altında **yapılandırma yönetimi**.

Otomasyon hesabı ve günlük analizi çalışma alanı seçin ve tıklatın **etkinleştirmek** çözümü etkinleştirilemiyor. Çözüm etkinleştirmek için 15 dakika sürer.

![Yerleşik stok çözümü](media/automation-onboard-solutions-from-automation-account/onboardsolutions.png)

Değişiklik izleme ve stok çözüm olanağı sağlar [Değişiklikleri İzle](automation-vm-change-tracking.md) ve [stok](automation-vm-inventory.md) sanal makinelerinizin üzerinde. Bu adımda, bir sanal makinede çözüm etkinleştirin.

Değişiklik izleme ve envanter çözüm ekleme bildirimi tamamlandığında tıklayın **güncelleştirme yönetimi** altında **yapılandırma yönetimi**.

Güncelleştirme yönetimi çözümü, güncelleştirme ve düzeltme eklerini Azure Windows Vm'leriniz için yönetmenizi sağlar. Kullanılabilir güncelleştirmeleri, gerekli güncelleştirmelerin zamanlama yükleme durumunu değerlendirmek ve güncelleştirmeleri doğrulamak için dağıtım sonuçları gözden geçirin VM başarıyla uygulandı. Bu eylem çözümü, VM için etkin.

Seçin **güncelleştirme yönetimi** altında **güncelleştirme yönetimi**. Seçilen günlük analizi çalışma alanı önceki adımda kullanılan aynı çalışma alanıdır. Tıklatın **etkinleştirmek** onboarding için güncelleştirme yönetimi çözümü. Çözüm etkinleştirmek için 15 dakika sürer.

![Yerleşik güncelleştirme çözümü](media/automation-onboard-solutions-from-automation-account/onboardsolutions2.png)

## <a name="scope-configuration"></a>Kapsam yapılandırması

Her çözüm çözümünü edinme bilgisayarları hedeflemek için çalışma alanı içindeki bir kapsam yapılandırması kullanır. Kapsam yapılandırması belirli bilgisayarlara çözüm kapsamını sınırlamak için kullanılan bir veya daha fazla kayıtlı aramaları grubudur. Kapsam yapılandırmalarında Otomasyon hesabınızın altında erişmek için **ilgili kaynaklar**seçin **çalışma**. Ardından altında çalışma alanında **çalışma veri kaynakları**seçin **kapsam yapılandırmaları**.

Varsayılan olarak iki kapsam yapılandırması oluşturuldu **MicrosoftDefaultScopeConfig ChangeTracking** ve **MicrosoftDefaultScopeConfig güncelleştirmeleri**.

## <a name="saved-searches"></a>Kaydedilen aramalar

Bir bilgisayarı güncelleştirme yönetimi ya da değişiklik izleme ve stok çözümleri eklendiğinde, iki Kaydedilmiş aramaları çalışma alanınızdaki birine eklenir. Kaydedilen aramalar için bu çözümleri hedeflenen bilgisayarları içeren sorgular var.

Otomasyon hesabınıza gidin ve seçin **kayıtlı aramalar** altında **genel**. Aşağıdaki tabloda bu çözümler tarafından kullanılan iki Kaydedilmiş aramaları görülebilir:

|Ad     |Kategori  |Diğer ad  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Güncelleştirmeler        | Updates__MicrosoftDefaultComputerGroup         |

Grup doldurmak için kullanılan sorguyu görüntülemek için kaydedilmiş ya da Ara'yı seçin. Aşağıdaki resimde, sorgu ve sonuçları gösterilmektedir:

![Kaydedilen aramalar](media/automation-onboard-solutions-from-automation-account/savedsearch.png)

## <a name="onboard-an-azure-machine"></a>Yerleşik bir Azure machine

Otomasyon hesabı seçin **stok** veya **değişiklik izleme** altında **yapılandırma yönetimi**, veya **güncelleştirme yönetimi** altında **güncelleştirme yönetimi**.

Tıklatın **+ Azure VM eklemek**, VM listeden seçin. Üzerinde **güncelleştirme yönetimi** sayfasında, **etkinleştirmek**. Bu çözüm için arama kayıtlı bilgisayar grubu için geçerli VM ekler.

## <a name="onboard-a-non-azure-machine"></a>Yerleşik bir Azure olmayan makine

Otomasyon hesabı seçin **stok** veya **değişiklik izleme** altında **yapılandırma yönetimi**, veya **güncelleştirme yönetimi** altında **güncelleştirme yönetimi**.

Tıklatın **Ekle Azure olmayan makine**. Bu yükleme ve makine çözüme raporlama başlayabilmesi için makinede Microsoft İzleme Aracısı Yapılandırma yönergelerini içeren yeni bir tarayıcı penceresi açar. Onboarding şu anda yönetilen bir makine System Center Operations Manager tarafından varsa, yeni bir aracı gerekli değildir, çalışma alanı bilgisi var olan aracıyı girilir.

## <a name="onboard-machines-in-the-workspace"></a>Çalışma alanındaki yerleşik makineler

Otomasyon hesabı seçin **stok** veya **değişiklik izleme** altında **yapılandırma yönetimi**, veya **güncelleştirme yönetimi** altında **güncelleştirme yönetimi**.

Seçin **yönetmek makineler**. Bu açılır **yönetmek makineler** sayfası. Bu sayfa, çözüm makineler, tüm kullanılabilir makineler select kümesi üzerinde veya tüm geçerli makineler için çözüm etkinleştirmek ve gelecekteki tüm makinelerde etkinleştirebilirsiniz olanak sağlar.

![Kaydedilen aramalar](media/automation-onboard-solutions-from-automation-account/managemachines.png)

### <a name="selected-machines"></a>Seçilen makineler

Bir veya daha fazla makine için çözüm etkinleştirmek için seçin **seçili makinelerde etkinleştir** tıklatıp **ekleme** çözüme eklemek istediğiniz her bir makine yanındaki. Bu görev çözüm için arama sorgusu kayıtlı bilgisayar grubu seçili makine adları ekler.

### <a name="all-available-machines"></a>Tüm kullanılabilir makineler

Çözüm için tüm kullanılabilir makineler etkinleştirmek için seçin **kullanılabilir tüm makinelerde etkinleştir**. Bu makineleri tek tek eklemek için denetim devre dışı bırakır. Bu görev arama sorgusu kayıtlı bilgisayar grubu için çalışma alanına raporlama makinelerin tüm adları ekler.

### <a name="all-available-and-future-machines"></a>Tüm kullanılabilir ve gelecekteki makineler

Tüm kullanılabilir makineler ve gelecekteki tüm makineler için çözüm etkinleştirmek için seçin **tüm kullanılabilir ve gelecekteki makinelerde etkinleştir**. Bu seçenek Kaydedilmiş aramaları ve kapsam yapılandırmaları çalışma alanından siler. Bu, tüm Azure ve çalışma alanına raporlama Azure olmayan makinelere çözüme açar.

## <a name="next-steps"></a>Sonraki adımlar

Bunların nasıl kullanılacağını öğrenmek için çözümler hakkında öğreticileri devam edin.

* [Öğretici - VM için güncelleştirmeleri yönetebilirsiniz.](automation-tutorial-update-management.md)

* [Öğretici - VM yazılımı tanımlayın](automation-tutorial-installed-software.md)

* [Öğretici - VM üzerindeki değişiklikler sorun giderme](automation-tutorial-troubleshoot-changes.md)