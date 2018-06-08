---
title: Bilgi yerleşik güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri Azure Automation nasıl
description: Nasıl için yerleşik bir Azure sanal makine Azure Otomasyonu parçası olan güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri ile bilgi edinin
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: conceptual
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 0174e2a3c0b14c52b5750e343932a5df39d18976
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34833403"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions"></a>Yerleşik güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri

Azure Otomasyonu bilgisayarlarınızda yüklü stok işletim sistemi güvenlik güncelleştirmeleri yönetmek ve değişiklikleri izlemek için çözüm sağlar. Yerleşik makinelere birden çok yolu vardır, çözüm gerçekleştirebilir [bir sanal makineden](automation-onboard-solutions-from-vm.md), [birden fazla makine gözatma gelen](automation-onboard-solutions-from-browse.md), Otomasyon hesabınızdan veya göre [runbook](automation-onboard-solutions.md). Bu makalede onboarding Automation hesabınız bu çözümlerden kapsar.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure’da oturum açın

## <a name="enable-solutions"></a>Çözümleri etkinleştirme

Otomasyon hesabınıza gidin ve seçin **stok** veya **değişiklik izleme** altında **yapılandırma yönetimi**.

Otomasyon hesabı ve günlük analizi çalışma alanı seçin ve tıklatın **etkinleştirmek** çözümü etkinleştirilemiyor. Çözümün etkinleştirilmesi 15 dakika sürer.

![Yerleşik stok çözümü](media/automation-onboard-solutions-from-automation-account/onboardsolutions.png)

Değişiklik İzleme ve Sayım çözümü, sanal makinelerinizde [değişiklikleri izleme](automation-vm-change-tracking.md) ve [sayım](automation-vm-inventory.md) olanağı sağlar. Bu adımda çözümü bir sanal makine üzerinde etkinleştirirsiniz.

Değişiklik izleme ve sayım çözümü ekleme bildirimi tamamlandığında **YAPILANDIRMA YÖNETİMİ** bölümünde **Güncelleştirme Yönetimi**’ne tıklayın.

Güncelleştirme Yönetimi çözümü, Azure Windows VM’leriniz için güncelleştirmeleri ve yamaları yönetmenizi sağlar. Kullanılabilir güncelleştirmelerin durumunu değerlendirebilir, gerekli güncelleştirmelerin yüklemesini zamanlayabilir ve güncelleştirmelerin VM’ye başarıyla uygulandığını doğrulamak için dağıtım sonuçlarını gözden geçirebilirsiniz. Bu eylem çözümü, VM için etkin.

Seçin **güncelleştirme yönetimi** altında **güncelleştirme yönetimi**. Seçilen Log Analytics çalışma alanı, önceki adımda kullanılan çalışma alanıdır. Güncelleştirme yönetimi çözümünü eklemek için **Etkinleştir**’e tıklayın. Çözümün etkinleştirilmesi 15 dakika sürer.

![Yerleşik güncelleştirme çözümü](media/automation-onboard-solutions-from-automation-account/onboardsolutions2.png)

## <a name="scope-configuration"></a>Kapsam Yapılandırması

Her çözüm çözümünü edinme bilgisayarları hedeflemek için çalışma alanı içindeki bir kapsam yapılandırması kullanır. Kapsam yapılandırması belirli bilgisayarlara çözüm kapsamını sınırlamak için kullanılan bir veya daha fazla kayıtlı aramaları grubudur. Kapsam yapılandırmalarında Otomasyon hesabınızın altında erişmek için **ilgili kaynaklar**seçin **çalışma**. Ardından altında çalışma alanında **çalışma veri kaynakları**seçin **kapsam yapılandırmaları**.

Seçilen çalışma alanı yoksa, güncelleştirme yönetimi veya değişiklik izleme çözümü henüz, aşağıdaki kapsam yapılandırmaları oluşturulur:

* **MicrosoftDefaultScopeConfig ChangeTracking**

* **MicrosoftDefaultScopeConfig güncelleştirmeleri**

Seçilen çalışma alanı zaten çözümü vardır. Çözümü yeniden dağıtılmadığı ve kapsam yapılandırması için eklenmez.

## <a name="saved-searches"></a>Kayıtlı aramalar

Bir bilgisayarı güncelleştirme yönetimi ya da değişiklik izleme ve stok çözümleri eklendiğinde, iki Kaydedilmiş aramaları çalışma alanınızdaki birine eklenir. Kaydedilen aramalar için bu çözümleri hedeflenen bilgisayarları içeren sorgular var.

Otomasyon hesabınıza gidin ve seçin **kayıtlı aramalar** altında **genel**. Aşağıdaki tabloda bu çözümler tarafından kullanılan iki Kaydedilmiş aramaları görülebilir:

|Ad     |Kategori  |Diğer ad  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Güncelleştirmeler        | Updates__MicrosoftDefaultComputerGroup         |

Grup doldurmak için kullanılan sorguyu görüntülemek için kaydedilmiş ya da Ara'yı seçin. Aşağıdaki resimde, sorgu ve sonuçları gösterilmektedir:

![Kayıtlı aramalar](media/automation-onboard-solutions-from-automation-account/savedsearch.png)

## <a name="onboard-azure-vms"></a>Yerleşik Azure VM'ler

Otomasyon hesabı seçin **stok** veya **değişiklik izleme** altında **yapılandırma yönetimi**, veya **güncelleştirme yönetimi** altında **güncelleştirme yönetimi**.

Tıklatın **+ Azure VM'ler eklemek**, listeden bir veya daha fazla sanal makineleri seçin. Etkinleştirilemez sanal makineler, out ve seçilmesi için gri. Üzerinde **güncelleştirme yönetimini etkinleştirme** sayfasında, **etkinleştirmek**. Bu çözüm için arama kayıtlı bilgisayar grubu için seçili sanal makineleri ekler.

![Azure sanal makineleri etkinleştirme](media/automation-onboard-solutions-from-automation-account/enable-azure-vms.png)

## <a name="onboard-a-non-azure-machine"></a>Azure olmayan bir makine ekleme

Makinelerin Azure içinde değil, el ile eklenmesi gerekir. Otomasyon hesabı seçin **stok** veya **değişiklik izleme** altında **yapılandırma yönetimi**, veya **güncelleştirme yönetimi** altında **güncelleştirme yönetimi**.

Tıklatın **Ekle Azure olmayan makine**. Yeni bir tarayıcı penceresi ile yukarı açılır [yüklemek ve Microsoft Monitoring Agent makinede yapılandırmak yönergeler](../log-analytics/log-analytics-concept-hybrid.md) makine çözüme raporlama başlayabilmesi için. Onboarding şu anda yönetilen bir makine System Center Operations Manager tarafından varsa, yeni bir aracı gerekli değildir, çalışma alanı bilgisi var olan aracıyı girilir.

## <a name="onboard-machines-in-the-workspace"></a>Çalışma alanındaki yerleşik makineler

El ile makine ya da çalışma gereksiniminizi etkinleştirilmesi, çözüm için Azure Automation'için eklenmesi için zaten raporlayan makineler yüklü. Otomasyon hesabı seçin **stok** veya **değişiklik izleme** altında **yapılandırma yönetimi**, veya **güncelleştirme yönetimi** altında **güncelleştirme yönetimi**.

Seçin **yönetmek makineler**. Bu açılır **yönetmek makineler** sayfası. Bu sayfa, çözüm makineler, tüm kullanılabilir makineler select kümesi üzerinde veya tüm geçerli makineler için çözüm etkinleştirmek ve gelecekteki tüm makinelerde etkinleştirebilirsiniz olanak sağlar.

![Kayıtlı aramalar](media/automation-onboard-solutions-from-automation-account/managemachines.png)

### <a name="all-available-machines"></a>Tüm kullanılabilir makineler

Çözüm için tüm kullanılabilir makineler etkinleştirmek için seçin **kullanılabilir tüm makinelerde etkinleştir**. Bu makineleri tek tek eklemek için denetim devre dışı bırakır. Bu görev arama sorgusu kayıtlı bilgisayar grubu için çalışma alanına raporlama makinelerin tüm adları ekler.

### <a name="all-available-and-future-machines"></a>Tüm kullanılabilir ve gelecekteki makineler

Tüm kullanılabilir makineler ve gelecekteki tüm makineler için çözüm etkinleştirmek için seçin **tüm kullanılabilir ve gelecekteki makinelerde etkinleştir**. Bu seçenek Kaydedilmiş aramaları ve kapsam yapılandırmaları çalışma alanından siler. Bu, tüm Azure ve çalışma alanına raporlama Azure olmayan makinelere çözüme açar.

### <a name="selected-machines"></a>Seçilen makineler

Bir veya daha fazla makine için çözüm etkinleştirmek için seçin **seçili makinelerde etkinleştir** tıklatıp **ekleme** çözüme eklemek istediğiniz her bir makine yanındaki. Bu görev çözüm için arama sorgusu kayıtlı bilgisayar grubu seçili makine adları ekler.

## <a name="next-steps"></a>Sonraki adımlar

Bunların nasıl kullanılacağını öğrenmek için çözümler hakkında öğreticileri devam edin.

* [Öğretici - VM için güncelleştirmeleri yönetebilirsiniz.](automation-tutorial-update-management.md)

* [Öğretici - VM yazılımı tanımlayın](automation-tutorial-installed-software.md)

* [Öğretici - VM üzerindeki değişiklikler sorun giderme](automation-tutorial-troubleshoot-changes.md)