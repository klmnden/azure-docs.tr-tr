---
title: Bir Azure VM yerleşik güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri
description: Bilgi nasıl için yerleşik bir Azure sanal makinesi güncelleştirme yönetimi, değişiklik izleme ve stok çözümleriyle, parçası olan Azure Otomasyonu.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/06/2018
ms.topic: conceptual
ms.service: automation
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: f270b2ccea51e83bc6475051b8667bf73d7fd717
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36221521"
---
# <a name="onboard-update-management-change-tracking-and-inventory-solutions-from-an-azure-virtual-machine"></a>Bir Azure sanal makinesi yerleşik güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri

Azure Otomasyonu işletim sistemi güvenlik güncelleştirmelerini yönetmenize, değişiklikleri izlemek ve bilgisayarlarınızda yüklü stok yardımcı olmak için çözüm sağlar. Yerleşik makinelere birden çok yolu vardır. Bir sanal makine çözümden gerçekleştirebilir [Otomasyon hesabınızdan](automation-onboard-solutions-from-automation-account.md), [birden fazla makine gözatma gelen](automation-onboard-solutions-from-browse.md), kullanarak veya bir [runbook](automation-onboard-solutions.md). Bu makale bir Azure sanal makinesi bu çözümlerden ekleme kapsar.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="enable-the-solutions"></a>Çözümlerle

Varolan bir sanal makineye gidin. Altında **OPERATIONS**seçin **güncelleştirme yönetimi**, **stok**, veya **değişiklik izleme**.

Yalnızca VM için çözüm etkinleştirmek için emin **bu VM için etkinleştirme** seçilir. Yerleşik çözüm için birden fazla makine seçin **VM'ler için bu abonelikte etkinleştirmek**ve ardından **etkinleştirmek için makineleri seçin**. Nasıl için yerleşik birden fazla makine aynı anda öğrenmek için [yerleşik güncelleştirme yönetimi, değişiklik izleme ve stok çözümleri](automation-onboard-solutions-from-automation-account.md).

Otomasyon hesabı ve Azure günlük analizi çalışma alanı seçin ve ardından **etkinleştirmek** çözümü etkinleştirilemiyor. Çözümün etkinleştirilmesi 15 dakika sürer.

![Yerleşik güncelleştirme yönetimi çözümü](media/automation-onboard-solutions-from-vm/onboard-solution.png)

Diğer çözümleri gidin ve ardından **etkinleştirmek**. Bu çözümleri daha önce etkinleştirilmiş bir çözüm olarak aynı çalışma ve Automation hesabı kullandığından günlük analizi ve Otomasyon hesabı açılan listeleri devre dışı bırakılır.

> [!NOTE]
> **Değişiklik izleme** ve **stok** aynı çözümü kullanın. Bu çözümlerden birini etkinleştirildiğinde, diğeri de etkinleştirilir.

## <a name="scope-configuration"></a>Kapsam yapılandırması

Her çözüm çözümünü edinme bilgisayarları hedeflemek için çalışma alanında bir kapsam yapılandırması kullanır. Kapsam yapılandırması, belirli bilgisayarlara çözüm kapsamını sınırlamak için kullanılan bir veya daha fazla kayıtlı aramaları grubudur. Kapsam yapılandırmalarında Otomasyon hesabınızın altında erişmek için **ilgili kaynaklar**seçin **çalışma**. Çalışma alanında, altında **çalışma veri kaynakları**seçin **kapsam yapılandırmaları**.

Seçilen çalışma alanı güncelleştirme yönetimi veya değişiklik izleme çözümü zaten yoksa, aşağıdaki kapsam yapılandırmaları oluşturulur:

* **MicrosoftDefaultScopeConfig ChangeTracking**

* **MicrosoftDefaultScopeConfig güncelleştirmeleri**

Seçilen çalışma çözümü zaten varsa, çözüme imzalanmasını değil ve kapsam yapılandırması eklenmez.

Elipsleri seçin (**...** ) herhangi bir yapılandırmaları ve ardından **Düzenle**. İçinde **Düzen kapsam yapılandırması** bölmesinde, **bilgisayar grupları Seç**. **Bilgisayar grupları** bölmesi kapsam yapılandırması oluşturmak için kullanılan Kaydedilmiş aramaları gösterir.

## <a name="saved-searches"></a>Kayıtlı aramalar

Bir bilgisayarı güncelleştirme yönetimi, değişiklik izleme veya envanter çözümleri eklendiğinde, bilgisayar bir çalışma alanınızda iki Kaydedilmiş aramaları eklenir. Kaydedilen aramalar için bu çözümleri hedeflenen bilgisayarları içeren sorguları gerçekleşir.

Çalışma alanınıza gidin. Altında **genel**seçin **kayıtlı aramalar**. Bu çözümler tarafından kullanılan iki Kaydedilmiş aramaları aşağıdaki tabloda gösterilmiştir:

|Ad     |Kategori  |Diğer ad  |
|---------|---------|---------|
|MicrosoftDefaultComputerGroup     |  ChangeTracking       | ChangeTracking__MicrosoftDefaultComputerGroup        |
|MicrosoftDefaultComputerGroup     | Güncelleştirmeler        | Updates__MicrosoftDefaultComputerGroup         |

Grup doldurmak için kullanılan sorguyu görüntülemek için Kaydedilmiş aramaları birini seçin. Aşağıdaki resimde, sorgu ve sonuçları gösterilmektedir:

![Kayıtlı aramalar](media/automation-onboard-solutions-from-vm/logsearch.png)

## <a name="next-steps"></a>Sonraki adımlar

Bunların nasıl kullanılacağını öğrenmek çözümler için öğreticileri için devam edin:

* [Öğretici - VM için güncelleştirmeleri yönetebilirsiniz.](automation-tutorial-update-management.md)
* [Öğretici - VM yazılımı tanımlayın](automation-tutorial-installed-software.md)
* [Öğretici - VM üzerindeki değişiklikler sorun giderme](automation-tutorial-troubleshoot-changes.md)
