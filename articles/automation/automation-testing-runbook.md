---
title: Azure Automation'da bir runbook test edilirken
description: Azure Otomasyonu'ndaki bir runbook'u yayımlamadan önce beklendiği gibi çalıştığından emin olmak için test edebilirsiniz.  Bu makalede, bir runbook'u test etme ve çıktısını görüntülemek açıklar.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 95e3f4426fab8ed3ff28877607dee8694962e79f
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54422482"
---
# <a name="testing-a-runbook-in-azure-automation"></a>Azure Automation'da bir runbook test edilirken
Bir runbook'u test ettiğinizde [Taslak sürümü](automation-creating-importing-runbook.md#publishing-a-runbook) yürütülür ve gerçekleştirdiği tüm işlemler tamamlanır. Hiçbir iş geçmişi oluşturulmaz, ancak [çıkış](automation-runbook-output-and-messages.md#output-stream) ve [uyarı ve hata](automation-runbook-output-and-messages.md#message-streams) akışları Test görüntülenen bölmesi çıktı. İletileri [Verbose Stream](automation-runbook-output-and-messages.md#message-streams) çıkış bölmesinde yalnızca görüntülenip [$VerbosePreference değişkeni](automation-runbook-output-and-messages.md#preference-variables) devam et ayarlanır.

Taslak sürüm çalıştırılır olsa da, runbook yine de iş akışını normal olarak yürütür ve ortamda kaynaklara karşı herhangi bir eylem gerçekleştirir. Bu nedenle, runbook'ları üretim dışı kaynakları yalnızca test etmeniz gerekir.

Her test için yordamı [runbook türünü](automation-runbook-types.md) aynı ve orada olduğu metin düzenleyicisini ve Azure portalında grafik düzenleyicisini arasında test farklılığı yok.  

## <a name="to-test-a-runbook-in-the-azure-portal"></a>Azure portalında bir runbook'u test etme
Çalışabilir [runbook türü](automation-runbook-types.md) Azure portalında.

1. Ya da runbook'un taslak sürümünü açın [metin düzenleyicisini](automation-edit-textual-runbook.md) veya [grafik düzenleyicisini](automation-graphical-authoring-intro.md).
2. Tıklayın **Test** Test dikey penceresini açmak için düğmeyi.
3. Runbook'un parametreleri varsa, burada, test için kullanılan değerler sağlayabilirsiniz sol bölmesinde listelenir.
4. Test çalıştırmak isterseniz bir [karma Runbook çalışanı](automation-hybrid-runbook-worker.md), ardından değiştirme **çalıştırma ayarları** için **karma çalışanı** ve hedef grubu adını seçin.  Aksi takdirde, varsayılan tutun **Azure** testi bulutta çalıştırmak için.
5. Tıklayın **Başlat** testi başlatmak için düğme.
6. Runbook ise [PowerShell iş akışı](automation-runbook-types.md#powershell-workflow-runbooks) veya [grafik](automation-runbook-types.md#graphical-runbooks), durdurmak veya çıkış Bölmesi'nin altındaki düğmelerle edildiğini sırasında askıya alma. Bir runbook'u askıya aldığınızda, askıya alınmadan önce geçerli etkinliği tamamlar. Runbook askıya alındığında, durdurabilir veya yeniden başlatabilirsiniz.
7. Çıkış bölmesinde runbook çıktısını inceleyin.

## <a name="next-steps"></a>Sonraki Adımlar
* Bir runbook'u İçeri Aktar veya oluşturma hakkında bilgi edinmek için bkz: [oluşturma veya Azure automation'da bir runbook içeri aktarma](automation-creating-importing-runbook.md)
* Grafik Yazma hakkında daha fazla bilgi için bkz. [Azure Automation’da grafik yazma](automation-graphical-authoring-intro.md)
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)
* Durum iletilerini ve hataları döndürmek için runbook'ları yapılandırma hakkında daha fazla bilgi edinmek için de dahil olmak üzere önerilen uygulamalar, bkz: [Runbook çıkışı ve iletileri Azure Otomasyonu](automation-runbook-output-and-messages.md)


