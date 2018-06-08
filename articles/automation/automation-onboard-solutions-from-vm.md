---
title: Bilgi bir Azure sanal makinenin nasıl yerleşik güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri için
description: Nasıl için yerleşik bir Azure sanal makine Azure Otomasyonu parçası olan güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri ile bilgi edinin
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: conceptual
ms.service: automation
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 675aebf35a6bee6e4cc4fd884204edb5bae4b848
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830570"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions-from-an-azure-virtual-machine"></a>Yerleşik güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri bir Azure sanal makinenin

Azure Otomasyonu bilgisayarlarınızda yüklü stok işletim sistemi güvenlik güncelleştirmeleri yönetmek ve değişiklikleri izlemek için çözüm sağlar. Birden çok yolla yerleşik makinelere, bir sanal makine çözümden gerçekleştirebilir [Otomasyon hesabınızdan](automation-onboard-solutions-from-automation-account.md), [birden fazla makine gözatma gelen](automation-onboard-solutions-from-browse.md), ya da [runbook](automation-onboard-solutions.md). Bu makalede, bu çözümlerin bir Azure sanal makinenin ekleme yer almaktadır.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure’da oturum açın

## <a name="enable-the-solutions"></a>Çözümlerle

Varolan bir sanal makineye gidin ve seçin **güncelleştirme yönetimi**, **stok**, veya **değişiklik izleme** altında **işlemleri**.

Yalnızca VM için çözüm olun etkinleştirmek **bu VM için etkinleştirme** radyo düğmesi seçildiğinde, yerleşik çözüme birden fazla makine seçin **VM'ler için bu abonelikte etkinleştirmek** tıklatıp**Etkinleştirmek için makineleri seçin**. Bakın, [yerleşik güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri](automation-onboard-solutions-from-automation-account.md) aynı anda birden çok makineleri adımları nasıl yerleşik görüntüleyin.

Çözümü etkinleştirmek için Log Analytics çalışma alanını ve otomasyon hesabını seçip **Etkinleştir**’e tıklayın. Çözümün etkinleştirilmesi 15 dakika sürer.

![Güncelleştirme çözümü ekleme](media/automation-onboard-solutions-from-vm/onboard-solution.png)

Diğer çözümleri gidin ve tıklayın **etkinleştirmek**, günlük analizi ve Automation hesabını daha önce etkinleştirilmiş bir çözüm olarak aynı çalışma ve Otomasyon hesabı kullandıkları açılan kutusu devre dışı.

> [!NOTE]
> **Değişiklik izleme** ve **stok** bir etkinleştirildiğinde aynı çözümü kullanan diğer de etkinleştirilir.

## <a name="scope-configuration"></a>Kapsam Yapılandırması

Her çözüm çözümünü edinme bilgisayarları hedeflemek için çalışma alanı içindeki bir kapsam yapılandırması kullanır. Kapsam yapılandırması belirli bilgisayarlara çözüm kapsamını sınırlamak için kullanılan bir veya daha fazla kayıtlı aramaları grubudur. Kapsam yapılandırmalarında Otomasyon hesabınızın altında erişmek için **ilgili kaynaklar**seçin **çalışma** altında çalışma alanında sonra **çalışma veri KAYNAKLARINI**, seçin **kapsam yapılandırmaları**.

Seçilen çalışma alanı yoksa, güncelleştirme yönetimi veya değişiklik izleme çözümü henüz, aşağıdaki kapsam yapılandırmaları oluşturulur:

* **MicrosoftDefaultScopeConfig ChangeTracking**

* **MicrosoftDefaultScopeConfig güncelleştirmeleri**

Seçilen çalışma alanı zaten çözümü vardır. Çözümü yeniden dağıtılmadığı ve kapsam yapılandırması için eklenmez.

Tüm yapılandırmaları seçin ve üç nokta (...) tıklatın **Düzenle**. Üzerinde **Düzen kapsam yapılandırması** sayfasında, **bilgisayar grupları Seç** açmak için **bilgisayar grupları** sayfası. Bu sayfa, kapsam yapılandırması oluşturmak için kullanılan Kaydedilmiş aramaları gösterir.

## <a name="saved-searches"></a>Kayıtlı aramalar

Bir bilgisayarı güncelleştirme yönetimi ya da değişiklik izleme ve stok çözümleri eklendiğinde, iki Kaydedilmiş aramaları çalışma alanınızdaki birine eklenir. Kaydedilen aramalar için bu çözümleri hedeflenen bilgisayarları içeren sorgular var.

Seçin ve çalışma alanına gidin **kayıtlı aramalar** altında **genel**. Aşağıdaki tabloda bu çözümler tarafından kullanılan iki Kaydedilmiş aramaları görülebilir:

|Ad     |Kategori  |Diğer ad  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Güncelleştirmeler        | Updates__MicrosoftDefaultComputerGroup         |

Grup doldurmak için kullanılan sorguyu görüntülemek için kaydedilmiş ya da Ara'yı seçin. Aşağıdaki resimde, sorgu ve sonuçları gösterilmektedir.

![Kayıtlı aramalar](media/automation-onboard-solutions-from-vm/logsearch.png)

## <a name="next-steps"></a>Sonraki adımlar

Bunların nasıl kullanılacağını öğrenmek için çözümler hakkında öğreticileri devam edin.

* [Öğretici - VM için güncelleştirmeleri yönetebilirsiniz.](automation-tutorial-update-management.md)

* [Öğretici - VM yazılımı tanımlayın](automation-tutorial-installed-software.md)

* [Öğretici - VM üzerindeki değişiklikler sorun giderme](automation-tutorial-troubleshoot-changes.md)
