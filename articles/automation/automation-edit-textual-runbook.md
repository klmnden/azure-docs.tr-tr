---
title: Azure Otomasyonu, metinsel runbook'ları düzenleme
description: Bu makalede farklı yordamlar Azure Automation PowerShell ve PowerShell iş akışı runbook'ları ile çalışmak için metin düzenleyicisini kullanarak sağlar.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 08/01/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9c684e9d1bf6cec12024cedfb5360d10e400e139
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61071102"
---
# <a name="editing-textual-runbooks-in-azure-automation"></a>Azure Otomasyonu, metinsel runbook'ları düzenleme

Azure Otomasyonu metin düzenleyicisinde düzenlemek için kullanılan [PowerShell runbook'ları](automation-runbook-types.md#powershell-runbooks) ve [PowerShell iş akışı runbook'ları](automation-runbook-types.md#powershell-workflow-runbooks). Bu runbook'ları için ortak kaynaklara erişirken yardımcı olması için IntelliSense ve renk kodlaması ile ek özel özellikler gibi diğer kod düzenleyicilerinden, tipik özellikleri sunar. Bu makalede, bu düzenleyicisiyle farklı işlevleri gerçekleştirmek için ayrıntılı adımlar verilmektedir.

Metin Düzenleyicisi bir runbook'a cmdlet'leri, varlıkları ve alt runbook'lar için kod ekleme özelliğini içerir. Kodu kendiniz yazmak yerine, kullanılabilir kaynakları listesinden seçin ve uygun kodu runbook'a eklenir.

Azure Otomasyonu içindeki her runbook'un taslak ve yayımlanan olmak üzere iki sürümü vardır. Runbook'un taslak sürümünü düzenler ve yürütülmek üzere yayımlarsınız. Yayımlanan sürüm düzenlenemez. Daha fazla bilgi için [runbook yayımlama](manage-runbooks.md#publish-a-runbook).

Çalışmak için [grafik runbook'ları](automation-runbook-types.md#graphical-runbooks), bkz: [Azure Otomasyonu'nda grafik yazma](automation-graphical-authoring-intro.md).

## <a name="to-edit-a-runbook-with-the-azure-portal"></a>Azure portalıyla bir runbook'u düzenlemek için

Metin düzenleyicisinde düzenlemek için bir runbook açmak için aşağıdaki yordamı kullanın.

1. Azure portalında Otomasyon hesabınızı seçin.
2. Altında **süreç OTOMASYONU**seçin **runbook'ları** runbook'ların listesini açmak için.
3. Düzenleyin ve ardından istediğiniz runbook'u seçmek **Düzenle** düğmesi.
4. Runbook'u düzenleyin.
5. Düzenlemeleriniz tamamlandığında **Kaydet** 'e tıklayın.
6. Runbook'un en son taslak sürümünün yayımlanmasını istiyorsanız **Yayımla** 'ya tıklayın.

### <a name="to-insert-a-cmdlet-into-a-runbook"></a>Bir cmdlet bir runbook'a ekleme

1. Metin düzenleyicisini tuvalinde cmdlet yerleştirmek istediğiniz imleci getirin.
2. Genişletin **cmdlet'leri** kitaplık denetiminde düğümü.
3. Kullanmak istediğiniz bir cmdlet'i içeren modül genişletin.
4. Ekle ve seçmek için cmdlet sağ **tuvale Ekle**. Cmdlet kümesi birden fazla parametre varsa, varsayılan eklenir. Ayrıca, farklı parametre kümesi seçin için cmdlet'i genişletebilirsiniz.
5. Cmdlet'i için kod, tüm parametrelerinin listesi ile eklenir.
6. Gerekli parametreleri için Küme ayraçları <> çevrelenen veri türü yerine uygun değeri belirtin. İhtiyacınız olmayan herhangi bir parametre kaldırın.

### <a name="to-insert-code-for-a-child-runbook-into-a-runbook"></a>Bir alt runbook için kodu runbook'a eklemek için

1. Metin düzenleyicisini tuvalinde kodunu yerleştirmek istediğiniz imleci konumlandırma [alt runbook](automation-child-runbooks.md).
2. Genişletin **runbook'ları** kitaplık denetiminde düğümü.
3. Runbook Ekle ve seçmek için sağ **tuvale Ekle**.
4. Alt runbook için kodu, runbook parametreleri için herhangi bir yer tutucu ile eklenir.
5. Yer tutucuları her parametre için uygun değerlerle değiştirin.

### <a name="to-insert-an-asset-into-a-runbook"></a>Runbook'a bir varlık eklemek için

1. Metin düzenleyicisini tuvalin alt runbook için kodu yerleştirmek istediğiniz imleci getirin.
2. Genişletin **varlıklar** kitaplık denetiminde düğümü.
3. İstediğiniz varlık türü düğümünü genişletin.
4. Varlık Ekle ve seçmek için sağ **tuvale Ekle**. İçin [değişken varlıkları](automation-variables.md), şunlardan birini seçin **tuvale "değişkenine Al"** veya **tuvale "değişkenini Ayarla" Ekle** almak veya değişkeni ayarlamak istediğinize bağlı olarak.
5. Varlık için kod runbook'a eklenir.

## <a name="to-edit-an-azure-automation-runbook-using-windows-powershell"></a>Windows PowerShell kullanarak bir Azure Otomasyonu runbook'u düzenlemek için

Windows PowerShell ile bir runbook'u düzenlemek için tercih ettiğiniz düzenleyiciyi kullanın ve kaydetmesi bir `.ps1` dosya. Kullanabileceğiniz [dışarı aktarma-AzureRmAutomationRunbook](/powershell/module/AzureRM.Automation/Export-AzureRmAutomationRunbook) runbook'un içeriğini almak için cmdlet ve ardından [Import-AzureRmAutomationRunbook](/powershell/module/AzureRM.Automation/import-azurermautomationrunbook) taslak runbook ile varolan bir değişiklik.

### <a name="to-retrieve-the-contents-of-a-runbook-using-windows-powershell"></a>Windows PowerShell kullanarak bir Runbook'un içeriğini almak için

Aşağıdaki örnek komutlarda bir runbook için betiğin nasıl alınacağı ve bir betik dosyasına kaydedileceği gösterilmektedir. Bu örnekte Taslak sürümü alınmaktadır. Bu sürüm değiştirilemez olsa da runbook'un yayımlanan sürümünü almak mümkündür.

```powershell-interactive
$resourceGroupName = "MyResourceGroup"
$automationAccountName = "MyAutomatonAccount"
$runbookName = "Hello-World"
$scriptFolder = "c:\runbooks"

Export-AzureRmAutomationRunbook -Name $runbookName -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName -OutputFolder $scriptFolder -Slot Draft
```

### <a name="to-change-the-contents-of-a-runbook-using-windows-powershell"></a>Windows PowerShell kullanarak bir Runbook'un içeriğini değiştirmek için

Aşağıdaki örnek komutlar bir runbook'un mevcut içeriğinin bir betik dosyasının içeriğiyle nasıl değiştirileceğini gösterir. Aynı örnek yordamda olarak budur [Windows PowerShell ile bir betik dosyasından bir runbook'u içeri aktarmak için](manage-runbooks.md#import-a-runbook).

```powershell-interactive
$resourceGroupName = "MyResourceGroup"
$automationAccountName = "MyAutomatonAccount"
$runbookName = "Hello-World"
$scriptFolder = "c:\runbooks"

Import-AzureRmAutomationRunbook -Path "$scriptfolder\Hello-World.ps1" -Name $runbookName -Type PowerShell -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName -Force
Publish-AzureRmAutomationRunbook -Name $runbookName -AutomationAccountName $automationAccountName -ResourceGroupName $resourceGroupName
```

## <a name="related-articles"></a>İlgili makaleler

* [Azure Otomasyonu runbook'ları yönetme](manage-runbooks.md)
* [PowerShell iş akışını öğrenme](automation-powershell-workflow.md)
* [Azure Otomasyonu'nda yazma grafik](automation-graphical-authoring-intro.md)
* [Sertifikalar](automation-certificates.md)
* [Bağlantılar](automation-connections.md)
* [Kimlik Bilgileri](automation-credentials.md)
* [Zamanlamalar](automation-schedules.md)
* [Değişkenler](automation-variables.md)

