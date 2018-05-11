---
title: Azure Otomasyonu metinsel runbook'larda düzenleme
description: Bu makalede farklı yordamlar PowerShell ve PowerShell iş akışı runbook'ları Azure Automation ile çalışmak için metin düzenleyicisini kullanarak sağlar.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/02/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: b5c12507ccf1ba290c4063f4e4e0e0704eec4b9b
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="editing-textual-runbooks-in-azure-automation"></a>Azure Otomasyonu metinsel runbook'larda düzenleme

Azure Otomasyonu metin düzenleyicide düzenlemek için kullanılan [PowerShell runbook'ları](automation-runbook-types.md#powershell-runbooks) ve [PowerShell iş akışı runbook'ları](automation-runbook-types.md#powershell-workflow-runbooks). Bu runbook'lar için ortak kaynaklarına erişim de yardımcı olmak için diğer kod düzenleyicileri IntelliSense ve ek özel özelliklerle renk kodlama gibi tipik özelliklerini sahiptir. Bu makalede, bu düzenleyicisiyle farklı işlevleri gerçekleştirmek için ayrıntılı adımlar sağlanmaktadır.

Metin Düzenleyicisi bir runbook'a cmdlet'leri, varlıkları ve alt runbook'lar için kod ekleme özelliğini içerir. Kodu kendiniz yazmak yerine, kullanılabilir kaynaklar listesinden seçin ve uygun kodu runbook'a.

Azure Otomasyonu içindeki her runbook'un taslak ve yayımlanan olmak üzere iki sürümü vardır. Runbook'un taslak sürümünü düzenler ve yürütülmek üzere yayımlarsınız. Yayımlanan sürüm düzenlenemez. Bkz: [runbook yayımlama](automation-creating-importing-runbook.md#publishing-a-runbook) daha fazla bilgi için.

Çalışmak için [grafik Runbook'lar](automation-runbook-types.md#graphical-runbooks), bkz: [Azure Automation'da grafik yazma](automation-graphical-authoring-intro.md).

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Azure portal ile bir runbook'u düzenlemek için

Metin düzenleyicide düzenlemek için bir runbook açmak için aşağıdaki yordamı kullanın.

1. Azure Portal'da, automation hesabınızı seçin.
2. Altında **işlem OTOMASYONU**seçin **Runbook'lar** listesini açmak için.
3. Düzenle ve ardından istediğiniz runbook'u seçin **Düzenle** düğmesi.
4. Gerekli düzenlemeleri yapın.
5. Tıklatın **kaydetmek** düzenlemeleriniz olduğunda tamamlandı.
6. Tıklatın **Yayımla** yayımlanmasını runbook'un en son taslak sürümünü istiyorsanız.

### <a name="to-insert-a-cmdlet-into-a-runbook"></a>Bir cmdlet bir runbook'a ekleme

1. Metin düzenleyicisini tuvalde cmdlet eklemek istediğiniz yeri imleç Konumlandır.
2. Genişletme **cmdlet'leri** kitaplığı denetimi düğümünde.
3. Kullanmak istediğiniz cmdlet içeren modülü genişletin.
4. Ekle ve seçmek için cmdlet sağ tıklayın **tuvale Ekle**. Cmdlet birden fazla parametre kümesi varsa, varsayılan kümesine eklenir. Farklı parametre kümesi seçmek için cmdlet genişletebilirsiniz.
5. Cmdlet'i için kodu, tüm parametrelerin listesi ile eklenir.
6. Köşeli ayraçlar <> gerekli parametreleri için tarafından veri türü yerine uygun değeri girin. Gerekli olmayan tüm parametreleri kaldırın.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Alt runbook için kodu runbook'a eklemek için

1. Metin düzenleyicisini tuvalde imleci kodunu yerleştirmek istediğiniz yere getirin [alt runbook](automation-child-runbooks.md).
2. Genişletme **Runbook'lar** kitaplığı denetimi düğümünde.
3. Runbook Ekle ve seçmek için sağ tıklatın **tuvale Ekle**.
4. Alt runbook için kodu ile runbook parametreleri için herhangi bir yer tutucu eklenir.
5. Yer tutucuları her parametre için uygun değerlerle değiştirin.

### <a name="to-insert-an-asset-into-a-runbook"></a>Bir runbook'a bir varlık eklemek için

1. Metin düzenleyicisini tuvalde alt runbook için kod eklemek istediğiniz yeri imleç Konumlandır.
2. Genişletme **varlıklar** kitaplığı denetimi düğümünde.
3. İstediğiniz varlık türü için düğümü genişletin.
4. Varlık Ekle ve seçmek için sağ tıklatın **tuvale Ekle**. İçin [değişken varlıkları](automation-variables.md), şunlardan birini seçin **tuvale "değişken Al" Ekle** veya **tuvale "değişken Ayarla" Ekle** almak veya değişkeni ayarlamak istediğinize bağlı olarak.
5. Varlık için kod runbook'a eklenir.

## <a name="to-edit-an-azure-automation-runbook-using-windows-powershell"></a>Windows PowerShell kullanarak bir Azure Otomasyonu runbook'u düzenlemek için

Windows PowerShell ile bir runbook'u düzenlemek için tercih ettiğiniz Düzenleyicisi'ni kullanın ve bir .ps1 dosyasına kaydedin. Kullanabileceğiniz [Get-AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/getazurerunbookdefinition) runbook'un içeriğini almak üzere ve ardından [kümesi AzureAutomationRunbookDefinition](http://aka.ms/runbookauthor/cmdlet/setazurerunbookdefinition) varolan değiştirmek için cmdlet taslak runbook ile değiştirilmiş bir.

### <a name="to-retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>Windows PowerShell kullanarak bir Runbook'un içeriğini almak için

Aşağıdaki örnek komutlar bir runbook için komut almak ve bir komut dosyasına kaydetmek nasıl gösterir. Bu örnekte Taslak sürümü alınmaktadır. Bu sürüm değiştirilemez rağmen runbook'un yayımlanan sürümünü almak mümkündür.

```powershell-interactive
$automationAccountName = "MyAutomationAccount"
$runbookName = "Sample-TestRunbook"
$scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

$runbookDefinition = Get-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Slot Draft
$runbookContent = $runbookDefinition.Content

Out-File -InputObject $runbookContent -FilePath $scriptPath
```

### <a name="to-change-the-contents-of-a-runbook-using-windows-powershell"></a>Windows PowerShell kullanarak bir Runbook'un içeriğini değiştirmek için

Aşağıdaki örnek komutlar bir runbook'un mevcut içeriğinin bir betik dosyasının içeriğiyle değiştirme göstermektedir. Bu aynı örnek yordamda olarak olduğuna dikkat edin [Windows PowerShell ile bir betik dosyasından bir runbook'u içeri aktarma](automation-creating-importing-runbook.md).

```powershell-interactive
$automationAccountName = "MyAutomationAccount"
$runbookName = "Sample-TestRunbook"
$scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

Set-AzureAutomationRunbookDefinition -AutomationAccountName $automationAccountName -Name $runbookName -Path $scriptPath -Overwrite
Publish-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName
```

## <a name="related-articles"></a>İlgili makaleler

* [Oluşturma veya bir Azure Otomasyonu runbook'u içeri aktarma](automation-creating-importing-runbook.md)
* [PowerShell iş akışı öğrenme](automation-powershell-workflow.md)
* [Grafik Azure Otomasyonu'nda yazma](automation-graphical-authoring-intro.md)
* [Sertifikalar](automation-certificates.md)
* [Bağlantılar](automation-connections.md)
* [Kimlik Bilgileri](automation-credentials.md)
* [Zamanlamalar](automation-schedules.md)
* [Değişkenler](automation-variables.md)
