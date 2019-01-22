---
title: Oluşturma veya Azure automation'da bir runbook içeri aktarma
description: Bu makalede, Azure Automation'da yeni bir runbook oluşturmak veya bir dosyadan içe açıklar.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 08/06/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: fdc064ab2b74424ce1e4e163c8843bfebc28bcf4
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54435575"
---
# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>Oluşturma veya Azure automation'da bir runbook içeri aktarma

Bir runbook için Azure Otomasyonu tarafından ekleyebileceğiniz [yeni bir tane oluşturmak](#creating-a-new-runbook) veya bir dosyadaki veya mevcut bir runbook'u içeri aktarma [Runbook Galerisi](automation-runbook-gallery.md). Bu makalede, oluşturma ve runbook'ları bir dosyadan içeri aktarma hakkında bilgiler sağlar.  Tüm topluluk runbook'ları ve modülleri erişim ayrıntıları alabilirsiniz [Azure Otomasyonu Runbook ve modül galerileri](automation-runbook-gallery.md).

## <a name="creating-a-new-runbook"></a>Yeni bir runbook oluşturma

Azure portalı veya Windows PowerShell kullanarak Azure Automation'da yeni bir runbook oluşturabilirsiniz. Runbook oluşturulduktan sonra bu bilgileri kullanarak düzenleyebilirsiniz [PowerShell iş akışını öğrenme](automation-powershell-workflow.md) ve [Azure Otomasyonu'nda grafik yazma](automation-graphical-authoring-intro.md).

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-portal"></a>Azure portalı ile yeni bir Azure Otomasyonu runbook oluşturmak için

1. Azure portalında, Otomasyon hesabınızı açın.
1. Hub'ından seçin **runbook'ları** runbook'ların listesini açmak için.
1. Tıklayarak **runbook Ekle** düğmesine ve ardından **yeni bir runbook oluşturmak**.
1. Tür a **adı** seçin ve runbook için kendi [türü](automation-runbook-types.md). Runbook adı bir harfle başlamalıdır ve harfler, rakamlar, alt çizgiler ve kısa çizgiler içerebilir.
1. Tıklayın **Oluştur** runbook oluşturma ve Düzenleyicisi'ni açın.

### <a name="to-create-a-new-azure-automation-runbook-with-windows-powershell"></a>Windows PowerShell ile yeni bir Azure Otomasyonu runbook oluşturmak için
Kullanabileceğiniz [New-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/new-azurermautomationrunbook) boş oluşturmak için cmdlet'i [PowerShell iş akışı runbook'u](automation-runbook-types.md#powershell-workflow-runbooks). **Türü** parametresi de dört runbook türlerinden birini belirtmek için dahil edilen olmalıdır.

Aşağıdaki örnek komutlarda yeni bir boş runbook'unun nasıl oluşturulacağını gösterir.

```azurepowershell-interactive
New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
-Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell
```

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>Bir runbook'u dosyadan Azure Automation'a içeri aktarabilirsiniz.

Azure Otomasyonu'nda bir PowerShell komut dosyası veya PowerShell iş akışı (.ps1 uzantısı), dışarı aktarılan bir grafik runbook (.graphrunbook) veya bir Python 2 komut dosyası (.py uzantısı) içeri aktararak, yeni bir runbook oluşturabilirsiniz.  Belirtmelisiniz [runbook türünü](automation-runbook-types.md) aşağıdaki konuları dikkate alarak, içeri aktarma sırasında oluşturulur.

* .Graphrunbook dosya yalnızca yeni bir alınabilir [grafik runbook](automation-runbook-types.md#graphical-runbooks), ve grafik runbook'ları yalnızca .graphrunbook dosyasından oluşturulabilir.
* Bir PowerShell iş akışı içeren bir .ps1 dosyası yalnızca içine aktarılabilen bir [PowerShell iş akışı runbook'u](automation-runbook-types.md#powershell-workflow-runbooks).  Birden çok PowerShell iş akışı dosyası içeriyorsa, içe aktarma başarısız olur. Her bir iş akışı kendi dosyasına kaydedin ve her ayrı olarak içeri aktarmak gerekir.
* Bir iş akışı içermeyen bir .ps1 dosyası ya da aktarılabilen bir [PowerShell runbook'u](automation-runbook-types.md#powershell-runbooks) veya [PowerShell iş akışı runbook'u](automation-runbook-types.md#powershell-workflow-runbooks).  Bir PowerShell iş akışı runbook alınırsa, daha sonra bir iş akışına dönüştürülür ve açıklamaları yapılan değişiklikler belirtme runbook'ta yer alır.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-portal"></a>Bir dosyadan Azure portalıyla bir runbook'u içeri aktarmak için

Bir betik dosyası Azure Automation'a içeri aktarmak için aşağıdaki yordamı kullanabilirsiniz.  

> [!NOTE]
> Portalı kullanarak bir PowerShell iş akışı runbook'a yalnızca bir .ps1 dosyasına aktarabilirsiniz unutmayın.

1. Azure portalında, Otomasyon hesabınızı açın.
2. Hub'ından seçin **runbook'ları** runbook'ların listesini açmak için.
3. Tıklayarak **runbook Ekle** düğmesine ve ardından **alma**.
4. Tıklayın **Runbook dosyası** Alınacak dosya seçin
5. Varsa **adı** alanı etkinse, sonra değiştirmek için seçeneğiniz vardır.  Runbook adı bir harfle başlamalıdır ve harfler, rakamlar, alt çizgiler ve kısa çizgiler içerebilir.
6. [Runbook türü](automation-runbook-types.md) otomatik olarak seçilir, ancak geçerli kısıtlamalarını hesaba kattıktan sonra türünü değiştirebilirsiniz. 
7. Yeni runbook Otomasyon hesabı için runbook'ları listesinde görünür.
8. Yapmanız gerekenler [runbook'u yayımlayamadı](#publishing-a-runbook) önce çalıştırabilirsiniz.

> [!NOTE]
> Grafik runbook'u veya grafik PowerShell iş akışı runbook'u içeri aktardıktan sonra isterseniz başka bir türe dönüştürmek için seçeneğiniz vardır. Metinsel runbook'a dönüştürülemiyor.
>  
> 

### <a name="to-import-a-runbook-from-a-script-file-with-windows-powershell"></a>Windows PowerShell ile bir betik dosyasından bir runbook'u içeri aktarmak için
Kullanabileceğiniz [Import-AzureRMAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/import-azurermautomationrunbook) PowerShell iş akışı runbook taslağı olarak bir betik dosyasını almak için cmdlet'i. Kullanmadığınız sürece runbook zaten varsa, içeri aktarma başarısız *-Force* parametresi. 

Aşağıdaki örnek komutlarda bir betik dosyasının bir runbook'a nasıl aktarılacağı gösterilmektedir.

```azurepowershell-interactive
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
$RGName = "ResourceGroup"

Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
-ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
-Type PowerShellWorkflow
```

## <a name="publishing-a-runbook"></a>Runbook yayımlama

Oluştururken veya yeni bir runbook'u içeri çalışabilmesi için önce onu yayımlamanız gerekir.  Otomasyon içindeki her runbook'un bir taslak ve bir yayımlanmış sürümü vardır. Yalnızca Yayımlanan sürüm çalıştırılabilir ve yalnızca Taslak sürüm düzenlenebilir. Yayımlanan sürüm Taslak sürümdeki herhangi bir değişiklikten etkilenmez. Taslak sürümü kullanılabilir olması, size, yayımlanan sürümü taslak sürümle değiştirebilirsiniz yayımlamalısınız.

## <a name="to-publish-a-runbook-using-the-azure-portal"></a>Azure portalını kullanarak bir runbook'u yayımlamak için

1. Runbook, Azure portalında açın.
1. Tıklayın **Düzenle** düğmesi.
1. Tıklayın **Yayımla** düğmesine ve ardından **Evet** doğrulama iletisi.

## <a name="to-publish-a-runbook-using-windows-powershell"></a>Windows PowerShell kullanarak bir runbook'u yayımlamak için
Kullanabileceğiniz [Yayımla-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/publish-azurermautomationrunbook) Windows PowerShell ile bir runbook'u yayımlamak için cmdlet'i. Aşağıdaki örnek komutlar bir örnek runbook'un nasıl yayımlanacağı gösterir.

```azurepowershell-interactive
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$RGName = "ResourceGroup"

Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
-Name $runbookName -ResourceGroupName $RGName
```

## <a name="next-steps"></a>Sonraki Adımlar

* Runbook ve modül Galerisi PowerShell kazanabileceğinizi hakkında bilgi edinmek için [Azure Otomasyonu Runbook ve modül galerileri](automation-runbook-gallery.md)
* PowerShell ve PowerShell iş akışı runbook'ları ile bir metin düzenleyicisini düzenleme hakkında daha fazla bilgi edinmek için [Azure Otomasyonu, metinsel runbook'ları düzenleme](automation-edit-textual-runbook.md)
* Grafik runbook yazma hakkında daha fazla bilgi edinmek için [Azure Otomasyonu'nda grafik yazma](automation-graphical-authoring-intro.md)
