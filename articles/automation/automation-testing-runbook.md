---
title: Azure Otomasyonu runbook'u test etme | Microsoft Docs
description: "Azure Otomasyonu runbook'u yayımlamadan önce beklendiği gibi çalıştığından emin olmak için test edebilirsiniz.  Bu makalede, bir runbook'u test ve çıktısını görüntülemek açıklar."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 7f7db785-52c0-4613-aa12-b02fd32a5182
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/12/2016
ms.author: magoedte;bwren
ms.openlocfilehash: df675dbf915d0fe08905a83b7874fdd95deed42d
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="testing-a-runbook-in-azure-automation"></a>Azure Otomasyonu runbook'u test etme
Bir runbook'u test ettiğinizde [taslak sürüm](automation-creating-importing-runbook.md#publishing-a-runbook) yürütülür ve gerçekleştirdiği tüm işlemler tamamlanır. Hiçbir iş geçmişi oluşturulmaz, ancak [çıkış](automation-runbook-output-and-messages.md#output-stream) ve [uyarı ve hata](automation-runbook-output-and-messages.md#message-streams) akışları Test görüntülenen bölmesinde çıktı. İletileri için [ayrıntılı akış](automation-runbook-output-and-messages.md#message-streams) çıkış bölmesi eksikse görüntülenen [$VerbosePreference değişkeni](automation-runbook-output-and-messages.md#preference-variables) devam et ayarlanır.

Taslak sürüm çalıştırılır olsa bile, runbook hala akışını normal olarak yürütür ve işlemleri ortamdaki kaynakları kullanarak gerçekleştirir. Bu nedenle, yalnızca üretim dışı kaynaklar runbook'ları test etmeniz gerekir.

Her test için yordamı [runbook türü](automation-runbook-types.md) aynı ve orada metin düzenleyicisini ve grafik düzenleyicisini Azure portalında arasında sınamada fark olmasıdır.  

## <a name="to-test-a-runbook-in-the-azure-portal"></a>Azure portalında bir runbook'u test etme
Tüm iş [runbook türü](automation-runbook-types.md) Azure portalında.

1. Ya da runbook'un taslak sürümünü açın [metin düzenleyicisini](automation-edit-textual-runbook.md) veya [grafik Düzenleyicisi](automation-graphical-authoring-intro.md).
2. Tıklatın **Test** Test dikey penceresini açmak için düğmeye.
3. Runbook parametrelere sahipse, test için kullanılmak üzere değerleri nerede sağlayabilir sol bölmede listelenir.
4. Test çalışmasını istiyorsanız, bir [karma Runbook çalışanı](automation-hybrid-runbook-worker.md), sonra değiştirmek **çalıştırma ayarları** için **karma çalışanı** ve hedef grubun adını seçin.  Aksi takdirde, varsayılan tutmak **Azure** testi bulutta çalıştırmak için.
5. Tıklatın **Başlat** test başlamak için Başlat.
6. Runbook ise [PowerShell iş akışı](automation-runbook-types.md#powershell-workflow-runbooks) veya [grafik](automation-runbook-types.md#graphical-runbooks), durdurmak veya çıkış Bölmesi ' nin altındaki düğmelerle onu test devam ederken askıya alma. Runbook'u askıya aldığınızda, askıya alınmadan önce geçerli etkinliği tamamlar. Runbook askıya alındığında, durdurabilir veya yeniden başlatın.
7. Çıkış bölmesinde runbook çıkışı inceleyin.

## <a name="next-steps"></a>Sonraki Adımlar
* Oluşturma veya bir runbook'u içeri bilgi edinmek için [oluşturma veya bir Azure Otomasyonu runbook'u içeri aktarma](automation-creating-importing-runbook.md)
* Grafik Yazma hakkında daha fazla bilgi için bkz. [Azure Automation’da grafik yazma](automation-graphical-authoring-intro.md)
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md)
* Durum iletilerini ve hataları döndürülecek runboks yapılandırma hakkında daha fazla bilgi edinmek için de dahil olmak üzere önerilen uygulamalar, bkz: [Runbook çıkışı ve iletileri Azure Automation](automation-runbook-output-and-messages.md)

