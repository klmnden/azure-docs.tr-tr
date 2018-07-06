---
title: Bilgi nasıl Azure Otomasyonu'nda güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri ekleme
description: Ekleme için bir Azure sanal makine nasıl Azure Otomasyonu parçası olan güncelleştirme yönetimi, değişiklik izleme ve stok çözümleriyle öğrenin
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: conceptual
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 8649b96c9cf95e4a25b24dedf447aef133ef299a
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37865412"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions"></a>Yerleşik güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri

Azure Otomasyonu, stok bilgisayarlarınızda yüklü işletim sistemi güvenlik güncelleştirmelerini yönetmek ve değişiklikleri izlemek için çözümler sağlar. Makine birden çok yolu vardır, bu çözüme ekleyebilir [bir sanal makineden](automation-onboard-solutions-from-vm.md), [birden çok makine gözatma gelen](automation-onboard-solutions-from-browse.md), Otomasyon hesabınızdan veya göre [runbook](automation-onboard-solutions.md). Bu makalede onboarding Bu çözümler Otomasyon hesabınızdan kapsar.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure’da oturum açın

## <a name="enable-solutions"></a>Çözümleri etkinleştirme

Otomasyon hesabınıza gidin ve şunlardan birini seçin **Envanter** veya **değişiklik izleme** altında **yapılandırma yönetimi**.

Log analytics çalışma alanı ve Otomasyon hesabı seçin ve tıklayın **etkinleştirme** çözümü etkinleştirmek için. Çözümün etkinleştirilmesi 15 dakika sürer.

![Stok çözümünü ekleme](media/automation-onboard-solutions-from-automation-account/onboardsolutions.png)

Değişiklik İzleme ve Sayım çözümü, sanal makinelerinizde [değişiklikleri izleme](automation-vm-change-tracking.md) ve [sayım](automation-vm-inventory.md) olanağı sağlar. Bu adımda çözümü bir sanal makine üzerinde etkinleştirirsiniz.

Değişiklik izleme ve sayım çözümü ekleme bildirimi tamamlandığında **YAPILANDIRMA YÖNETİMİ** bölümünde **Güncelleştirme Yönetimi**’ne tıklayın.

Güncelleştirme Yönetimi çözümü, Azure Windows VM’leriniz için güncelleştirmeleri ve yamaları yönetmenizi sağlar. Kullanılabilir güncelleştirmelerin durumunu değerlendirebilir, gerekli güncelleştirmelerin yüklemesini zamanlayabilir ve güncelleştirmelerin VM’ye başarıyla uygulandığını doğrulamak için dağıtım sonuçlarını gözden geçirebilirsiniz. Bu eylem çözümü VM'niz için etkin.

Seçin **güncelleştirme yönetimi** altında **güncelleştirme yönetimi**. Seçilen Log Analytics çalışma alanı, önceki adımda kullanılan çalışma alanıdır. Güncelleştirme yönetimi çözümünü eklemek için **Etkinleştir**’e tıklayın. Çözümün etkinleştirilmesi 15 dakika sürer.

![Güncelleştirme çözümü](media/automation-onboard-solutions-from-automation-account/onboardsolutions2.png)

## <a name="scope-configuration"></a>Kapsam Yapılandırması

Her bir çözümün kapsam yapılandırması çözümünü edinme bilgisayarları hedeflemek için bir çalışma alanı içinde kullanır. Kapsam yapılandırması belirli bilgisayarlara çözüm kapsamını sınırlamak için kullanılan bir veya daha fazla kayıtlı aramalar grubudur. Kapsam yapılandırmaları, Otomasyon hesabınız kapsamında erişmeye **ilgili kaynakları**seçin **çalışma**. Ardından çalışma alanı altındaki **çalışma alanı veri kaynakları**seçin **kapsam yapılandırmaları**.

Seçilen çalışma alanı yoksa, güncelleştirme yönetimi veya değişiklik izleme çözümlerini henüz aşağıdaki kapsam yapılandırmaları oluşturulur:

* **MicrosoftDefaultScopeConfig değişiklik izleme**

* **MicrosoftDefaultScopeConfig güncelleştirmeleri**

Seçilen çalışma alanında Çözüm zaten varsa. Çözümü yeniden dağıtılmadığı ve kapsam yapılandırması için eklenmez.

## <a name="saved-searches"></a>Kayıtlı aramalar

Bir bilgisayarı güncelleştirme yönetimi veya değişiklik izleme ve sayım çözümlerini eklendiğinde, çalışma alanınızdaki iki kayıtlı aramalar birine eklenir. Bu kayıtlı aramalar, bu çözümleri için hedeflenen bilgisayarları içeren sorgular kullanılmaktadır.

Otomasyon hesabınıza gidin ve seçin **kayıtlı aramalar** altında **genel**. Aşağıdaki tabloda bu çözüm tarafından kullanılan iki kayıtlı aramalar görülebilir:

|Ad     |Kategori  |Diğer ad  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  Değişiklik izleme       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Güncelleştirmeler        | Updates__MicrosoftDefaultComputerGroup         |

Grup doldurmak için kullanılan sorguyu görüntülemek için ya da kayıtlı bir aramayı seçin. Aşağıdaki resimde, sorgu ve sonuçları gösterilmektedir:

![Kayıtlı aramalar](media/automation-onboard-solutions-from-automation-account/savedsearch.png)

## <a name="onboard-azure-vms"></a>Azure VM ekleme

Otomasyon hesabı seçin **Envanter** veya **değişiklik izleme** altında **yapılandırma yönetimi**, veya **güncelleştirme yönetimi** altında **güncelleştirme yönetimi**.

Tıklayın **+ Azure VM ekleme**, listeden bir veya daha fazla sanal makine seçin. Sanal makinelerin etkinleştirilemez, out ve seçilmesi için gri. Üzerinde **güncelleştirme yönetimini etkinleştirme** sayfasında **etkinleştirme**. Bu, seçili VM'ler bilgisayar grubu kayıtlı araması çözümü ekler.

![Azure sanal makineleri etkinleştirme](media/automation-onboard-solutions-from-automation-account/enable-azure-vms.png)

## <a name="onboard-a-non-azure-machine"></a>Azure olmayan bir makine ekleme

Makineleri Azure el ile eklenmesi gerekir. Otomasyon hesabı seçin **Envanter** veya **değişiklik izleme** altında **yapılandırma yönetimi**, veya **güncelleştirme yönetimi** altında **güncelleştirme yönetimi**.

Tıklayın **Ekle Azure olmayan makine**. Bu yedekleme ile yeni bir tarayıcı penceresi açar [yüklemek ve makinede Microsoft Monitoring Agent'ı yapılandırmak yönergeler](../log-analytics/log-analytics-concept-hybrid.md) makine çözüme raporlama başlayabilmesi için. Ekleme şu anda yönetilen bir makine System Center Operations Manager tarafından varsa, yeni bir aracı gerekli değildir, çalışma alanı bilgileri var olan aracıyı girilir.

## <a name="onboard-machines-in-the-workspace"></a>Çalışma alanındaki yerleşik makineler

El ile makine ya da çalışma gereksiniminizi etkinleştirilmesi, çözüm için Azure Otomasyonu'için eklenmesi için önceden bildirimde bulunan makineler yüklü. Otomasyon hesabı seçin **Envanter** veya **değişiklik izleme** altında **yapılandırma yönetimi**, veya **güncelleştirme yönetimi** altında **güncelleştirme yönetimi**.

Seçin **yönetme makineler**. Bu açılır **makinelerini yönetme** sayfası. Bu sayfa, bir makine, tüm kullanılabilir makineleri Grup çözümü etkinleştirin veya tüm geçerli makineler için çözümü etkinleştirme ve tüm gelecek makinelerde etkinleştir sağlar.

![Kayıtlı aramalar](media/automation-onboard-solutions-from-automation-account/managemachines.png)

### <a name="all-available-machines"></a>Tüm kullanılabilir makineler

Tüm kullanılabilir makineleri çözümü etkinleştirmek için işaretleyin **kullanılabilir tüm makinelerde etkinleştir**. Bu, tek tek makineler eklemek için denetimi devre dışı bırakır. Arama sorgusu kayıtlı bilgisayar grubu için çalışma alanına bildirimde bulunan makineler tüm adları bu görev ekler.

### <a name="all-available-and-future-machines"></a>Tüm mevcut ve gelecekteki makinelerin

Tüm kullanılabilir makinelerde ve gelecekteki tüm makineler için çözümü etkinleştirmek için işaretleyin **tüm mevcut ve gelecekteki makinelerde etkinleştir**. Bu seçenek kayıtlı aramalar ve kapsam yapılandırmaları çalışma alanından siler. Bu, tüm Azure ve Azure olmayan makineler, çalışma alanına raporlama çözümü açar.

### <a name="selected-machines"></a>Seçilen makineler

Bir veya daha fazla makine için çözümü etkinleştirmek için seçin **seçili makinelerde etkinleştir** tıklatıp **ekleme** çözüme eklemek istediğiniz her makineyi yanında. Bu görev seçili makine adları kaydedilen arama sorgu çözümü bilgisayar grubuna ekler.

## <a name="next-steps"></a>Sonraki adımlar

Bunları nasıl kullanacağınızı öğrenmek için çözümler öğreticiler devam edin.

* [Öğretici - sanal Makineniz için güncelleştirmeleri yönetme](automation-tutorial-update-management.md)

* [Öğretici - VM üzerinde yazılım tanımlama](automation-tutorial-installed-software.md)

* [Öğretici - bir VM üzerindeki değişikliklerle ilgili sorunları giderme](automation-tutorial-troubleshoot-changes.md)