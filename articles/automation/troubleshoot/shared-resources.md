---
title: Azure Otomasyonu Paylaşılan kaynaklarla hatalarını giderme
description: Azure Otomasyonu paylaşılan kaynakları ile ilgili sorunları giderme hakkında bilgi edinin
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 12/3/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: ce78c86cdae9a06100fd17d00e0229805e42983b
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52848468"
---
# <a name="troubleshoot-errors-with-shared-resources"></a>Paylaşılan kaynaklar hatalarla ilgili sorunları giderme

Bu makalede, paylaşılan kaynaklar Azure Automation'da kullanırken çalışabilir genelinde sorunları çözmek için çözümleri açıklanmaktadır.

## <a name="modules"></a>Modüller

### <a name="module-stuck-importing"></a>Senaryo: Bir modülü içeri aktarma takıldı

#### <a name="issue"></a>Sorun

Bir modül olarak takılmış **alma** aktardığınızda veya modüllerinizi Azure Otomasyonu'nda güncelleştirme durumu.

#### <a name="cause"></a>Nedeni

PowerShell modüllerini içeri aktarma karmaşık çok adımlı bir işlemdir. Bu işlem, doğru şekilde içeri değil bir modül olasılığını tanıtır. Bu gerçekleşirse, geçici bir durumda aldığınız modülü takılabilir. Bu işlem hakkında daha fazla bilgi için bkz. [PowerShell modülünü içeri aktararak]( /powershell/developer/module/importing-a-powershell-module#the-importing-process).

#### <a name="resolution"></a>Çözüm

Bu sorunu çözmek için takılmış modülü kaldırmak **alma** kullanarak durum [Remove-Azurermautomationmodule'a](/powershell/module/azurerm.automation/remove-azurermautomationmodule) cmdlet'i. Modülün içeri aktarılması daha sonra yeniden deneyebilir.

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name ModuleName -ResourceGroupName ExampleResourceGroup -AutomationAccountName ExampleAutomationAccount -Force
```

## <a name="run-as-accounts"></a>Farklı Çalıştır hesapları

### <a name="unable-create-update"></a>. Senaryo: Oluşturulacak veya güncelleştirilecek bir farklı çalıştır hesabı oluşturulamıyor

#### <a name="issue"></a>Sorun

Oluşturulacak veya güncelleştirilecek bir farklı çalıştır hesabı denediğinizde aşağıdaki hata iletisine benzer bir hata iletisi:

```error
You do not have permissions to create…
```

#### <a name="cause"></a>Nedeni

Bir kaynak grubu düzeyinde kaynak kilitli veya oluşturmak veya farklı çalıştır hesabını güncelleştirmek gereken izinlere sahip değilsiniz.

#### <a name="resolution"></a>Çözüm

Oluşturun veya bir farklı çalıştır hesabını güncelleştirmek için farklı çalıştır hesabı kullanılan çeşitli kaynakları için uygun izinleri olmalıdır. Oluşturmak veya bir farklı çalıştır hesabını güncelleştirmek için gereken izinleri hakkında bilgi edinmek için [farklı çalıştır hesabı izinleri](../manage-runas-account.md#permissions).

Bir kilit nedeniyle sorun ise kilit kilitli kaynağına gidin ve kaldırmak için Tamam olduğunu doğrulayın, kilit sağ tıklayın ve seçin **Sil** kilidini kaldırmak için.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediniz veya sorununuzu çözmenize yüklenemiyor, daha fazla destek için aşağıdaki kanalları birini ziyaret edin:

* [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
* [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
* Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.