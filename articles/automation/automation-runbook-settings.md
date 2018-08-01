---
title: Azure Otomasyonu'nda Runbook ayarları
description: Azure Otomasyonu ve Azure portalı ve Windows PowerShell kullanarak bunları değiştirmek nasıl bir runbook için yapılandırma ayarları açıklanmaktadır.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 2174135aaf2e16907f16f38c1df1ec002b3083fd
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39391444"
---
# <a name="runbook-settings"></a>Runbook ayarları
Azure automation'da her runbook, tanımlanmasına ve günlüğe kaydetme davranışını değiştirmeye yardımcı birden çok ayara sahiptir. Bu ayarların her biri aşağıda yordamları tarafından ve ardından bunları değiştirme konusunda açıklanmıştır.

## <a name="settings"></a>Ayarlar
### <a name="name-and-description"></a>Ad ve açıklama
Oluşturulduktan sonra bir runbook adını değiştiremezsiniz. Açıklama isteğe bağlıdır ve en çok 512 karakter olabilir.

### <a name="tags"></a>Etiketler
Etiketleri olan ayrı sözcükleri ve tümceleri bir runbook'u tanımlamaya yardımcı olmak için atamanızı sağlar. Örneğin, ne zaman gönderdiğiniz bir runbook'a [PowerShell Galerisi](https://www.powershellgallery.com/), kategorilerini runbook listelenen belirlemek için belirli etiketleri belirtin. Bir runbook için birden çok etiketi virgülle ayırarak belirtebilirsiniz.

### <a name="logging"></a>Günlüğe kaydetme
Varsayılan olarak, ayrıntılı ve ilerleme durumu kayıtları, iş geçmişine yazılmaz. Bu kayıtlarını günlüğe kaydetmek için belirli bir runbook ayarlarını değiştirebilirsiniz. Bu kayıtlar hakkında daha fazla bilgi için bkz. [Runbook çıkışı ve iletileri](automation-runbook-output-and-messages.md).

## <a name="changing-runbook-settings"></a>Runbook ayarlarını değiştirme

### <a name="changing-runbook-settings-with-the-azure-portal"></a>Azure portalı ile runbook ayarlarını değiştirme
Azure Portalı'nda bir runbook ayarlarını değiştirebilirsiniz **ayarları** runbook dikey.

1. Azure portalında **Otomasyon** ve ardından bir Otomasyon hesabının adına tıklayın.
2. Seçin **runbook'ları** sekmesi.
3. Bir runbook'un adına tıklayın ve runbook için ayarları dikey penceresine yönlendirilirsiniz. Buradan belirtin veya etiketleri, runbook açıklaması değiştirebilir, günlüğe kaydetme ve izleme ayarlarını yapılandırmak ve sorunlarını gidermenize yardımcı olmak için destek araçlarına erişin.     

### <a name="changing-runbook-settings-with-windows-powershell"></a>Windows PowerShell ile runbook ayarlarını değiştirme
Kullanabileceğiniz [Set-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) cmdlet'i bir runbook ayarlarını değiştirmek için. Birden çok etiket belirtmek istiyorsanız, virgülle ayrılmış değerlerle etiketleri parametresine bir dizi veya tek bir dize ya da sağlayabilir. Geçerli etiketler alabilirsiniz [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).

Aşağıdaki örnek komutlar bir runbook'un özelliklerin nasıl ayarlanacağını gösterir. Bu örnek, üç etiketleri için varolan etiketleri ekler ve ayrıntılı kayıtların günlüğe kaydedilmesi gerektiğini belirtir.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="next-steps"></a>Sonraki adımlar
* Oluşturmak ve runbook'lardan çıktı ve hata iletileri almak nasıl öğrenmek için bkz. [Runbook çıkışı ve iletileri](automation-runbook-output-and-messages.md) 
* Zaten topluluk veya diğer kaynak tarafından geliştirilmiş bir runbook eklemek veya oluşturmak için kendi runbook bkz anlamak için [oluşturma veya bir Runbook'u içeri aktarma](automation-creating-importing-runbook.md) 

