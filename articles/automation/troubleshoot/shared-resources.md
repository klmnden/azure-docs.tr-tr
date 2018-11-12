---
title: Azure Otomasyonu Paylaşılan kaynaklarla hatalarını giderme
description: Azure Otomasyonu paylaşılan kaynakları ile ilgili sorunları giderme hakkında bilgi edinin
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 11/05/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 385d2969e65647ab0b5c5e21c07b127104587e7e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51263564"
---
# <a name="troubleshoot-errors-with-shared-resources"></a>Paylaşılan kaynaklar hatalarla ilgili sorunları giderme

Bu makalede, paylaşılan kaynaklar Azure Automation'da kullanırken çalışabilir genelinde sorunları çözmek için çözümleri açıklanmaktadır.

## <a name="modules"></a>Modüller

### <a name="module-stuck-importing"></a>Senaryo: Bir modülü içeri aktarma takıldı

#### <a name="issue"></a>Sorun

İçeri aktarma veya modüllerinizi Azure Otomasyonu'nda güncelleştirme olarak takılı bir modül Bul **alma** durumu.

#### <a name="error"></a>Hata

PowerShell modüllerini içeri aktarma karmaşık çok adımlı bir işlemdir. Bu işlem, doğru şekilde içeri değil bir modül olasılığını tanıtır. Bu meydana gelirse, geçici bir durumda aldığınız modülü takılabilir. Bu işlem hakkında daha fazla bilgi için bkz. [PowerShell modülünü içeri aktararak]( /powershell/developer/module/importing-a-powershell-module#the-importing-process).

#### <a name="resolution"></a>Çözüm

Bu sorunu çözmek için takılmış modülü kaldırmak **alma** kullanarak durum [Remove-Azurermautomationmodule'a](/powershell/module/azurerm.automation/remove-azurermautomationmodule) cmdlet'i. Modülün içeri aktarılması daha sonra yeniden deneyebilir.

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name ModuleName -ResourceGroupName ExampleResourceGroup -AutomationAccountName ExampleAutomationAccount -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediniz veya sorununuzu çözmenize yüklenemiyor, daha fazla destek için aşağıdaki kanalları birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.