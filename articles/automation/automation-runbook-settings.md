---
title: Azure Automation Runbook ayarları
description: Azure Otomasyonu ve Azure portalı ve Windows PowerShell kullanarak nasıl değiştirileceğini bir runbook için yapılandırma ayarları açıklanmaktadır.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/19/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 064fdc6b50bf21b78d122c650ae22f8b4f70c608
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="runbook-settings"></a>Runbook ayarları
Azure automation'da her runbook, tanımlanmasına ve günlüğe kaydetme davranışını değiştirmeye yardımcı birden çok ayarlarına sahiptir. Bu ayarların her biri aşağıda yordamlar tarafından ve ardından bunları değiştirme konusunda açıklanmıştır.

## <a name="settings"></a>Ayarlar
### <a name="name-and-description"></a>Ad ve açıklama
Oluşturulduktan sonra bir runbook adı değiştirilemiyor. Açıklama isteğe bağlıdır ve en çok 512 karakter olabilir.

### <a name="tags"></a>Etiketler
Etiketleri ayrı sözcükleri ve bir runbook'u tanımlamaya yardımcı olmak için tümcecikleri atamanızı sağlar. Örneğin, ne zaman gönderdiğiniz bir runbook'a [PowerShell Galerisi](https://www.powershellgallery.com/), runbook listelenen kategorilerini tanımlamak üzere özel etiketler belirtin. Bir runbook için birden çok etiket virgül ile ayırarak belirtebilirsiniz.

### <a name="logging"></a>Günlüğe kaydetme
Varsayılan olarak, ayrıntılı ve ilerleme durumu kayıtlarını iş geçmişine yazılmaz. Bu kayıtlarını günlüğe kaydetmek için belirli bir runbook ayarlarını değiştirebilirsiniz. Bu kayıtlar hakkında daha fazla bilgi için bkz: [Runbook çıkışı ve iletileri](automation-runbook-output-and-messages.md).

## <a name="changing-runbook-settings"></a>Runbook ayarlarını değiştirme

### <a name="changing-runbook-settings-with-the-azure-portal"></a>Azure portalı ile runbook ayarlarını değiştirme
Azure Portalı'nda bir runbook ayarlarını değiştirebilirsiniz **ayarları** dikey runbook.

1. Azure portalında seçin **Otomasyon** ve ardından bir Otomasyon hesabı adına tıklayın.
2. Seçin **Runbook'lar** sekmesi.
3. Bir runbook'un adına tıklayın ve runbook için ayarları dikey penceresine yönlendirilirsiniz. Buradan belirtin veya etiketleri, runbook açıklaması değiştirebilir, günlüğe kaydetme ve izleme ayarlarını yapılandırmak ve sorunlarını gidermenize yardımcı olması için destek araçları erişim.     

### <a name="changing-runbook-settings-with-windows-powershell"></a>Windows PowerShell ile runbook ayarlarını değiştirme
Kullanabileceğiniz [kümesi AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) bir runbook'un ayarlarını değiştirmek için cmdlet. Birden çok etiket belirtmek istiyorsanız, virgülle ayrılmış değerler etiketleri parametresine sahip bir dizi veya tek bir dize ya da sağlayabilir. Geçerli etiketler alabilirsiniz [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).

Aşağıdaki örnek komutlar bir runbook için özelliklerin nasıl ayarlanacağını gösterir. Bu örnek üç etiketleri varolan etiketleri ekler ve ayrıntılı kayıtların günlüğe kaydedilmesi gerektiğini belirtir.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="next-steps"></a>Sonraki adımlar
* Oluşturmak ve runbook'lardan çıkış ve hata iletileri almak öğrenmek için bkz: [Runbook çıkışı ve iletileri](automation-runbook-output-and-messages.md) 
* Zaten topluluk veya başka bir kaynaktan tarafından geliştirilen runbook Ekle ya da kendi runbook bakın oluşturmak için nasıl anlamak için [oluşturma veya bir Runbook'u içeri aktarma](automation-creating-importing-runbook.md) 

