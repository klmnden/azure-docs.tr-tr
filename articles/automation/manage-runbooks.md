---
title: Azure Otomasyonu runbook'ları yönetme
description: Bu makalede, Azure Otomasyonu runbook'ları yönetmek açıklar.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 02/14/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 5bb52e0547ed9bc18d67370ffb9db35942212aab
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61300286"
---
# <a name="manage-runbooks-in-azure-automation"></a>Azure Otomasyonu runbook'ları yönetme

Bir runbook için Azure Otomasyonu tarafından ekleyebileceğiniz [yeni bir tane oluşturmak](#create-a-runbook) veya mevcut bir runbook'u dosyadan içeri aktarabilirsiniz veya [Runbook Galerisi](automation-runbook-gallery.md). Bu makalede, oluşturma ve runbook'ları bir dosyadan içeri aktarma hakkında bilgiler sağlar.  Tüm topluluk runbook'ları ve modülleri erişim ayrıntıları alabilirsiniz [Azure Otomasyonu Runbook ve modül galerileri](automation-runbook-gallery.md).

## <a name="create-a-runbook"></a>Runbook oluşturma

Azure portalı veya Windows PowerShell kullanarak Azure Automation'da yeni bir runbook oluşturabilirsiniz. Runbook oluşturulduktan sonra bu bilgileri kullanarak düzenleyebilirsiniz [PowerShell iş akışını öğrenme](automation-powershell-workflow.md) ve [Azure Otomasyonu'nda grafik yazma](automation-graphical-authoring-intro.md).

### <a name="create-a-runbook-in-the-azure-portal"></a>Azure Portalı'nda bir runbook oluşturma

1. Azure portalında, Otomasyon hesabınızı açın.
2. Hub'ından seçin **runbook'ları** runbook'ların listesini açmak için.
3. Tıklayarak **runbook Ekle** düğmesine ve ardından **yeni bir runbook oluşturmak**.
4. Tür a **adı** seçin ve runbook için kendi [türü](automation-runbook-types.md). Runbook adı bir harfle başlamalıdır ve harfler, rakamlar, alt çizgiler ve kısa çizgiler içerebilir.
5. Tıklayın **Oluştur** runbook oluşturma ve Düzenleyicisi'ni açın.

### <a name="create-a-runbook-with-powershell"></a>PowerShell ile bir runbook oluşturma

Kullanabileceğiniz [New-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/new-azurermautomationrunbook) boş oluşturmak için cmdlet'i [PowerShell iş akışı runbook'u](automation-runbook-types.md#powershell-workflow-runbooks). Kullanım **türü** dört runbook türlerinden birini belirtmek için parametre.

Aşağıdaki örnek komutlarda yeni bir boş runbook'unun nasıl oluşturulacağını gösterir.

```azurepowershell-interactive
New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
-Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell
```

## <a name="import-a-runbook"></a>Bir runbook'u İçeri Aktar

Azure Otomasyonu'nda bir PowerShell komut dosyası veya PowerShell iş akışı (.ps1 uzantısı), dışarı aktarılan bir grafik runbook (.graphrunbook) veya bir Python 2 komut dosyası (.py uzantısı) içeri aktararak, yeni bir runbook oluşturabilirsiniz.  Belirtmelisiniz [runbook türünü](automation-runbook-types.md) aşağıdaki konuları dikkate alarak, içeri aktarma sırasında oluşturulur.

* A `.graphrunbook` dosya yalnızca içeri aktarılacak bir yeni [grafik runbook](automation-runbook-types.md#graphical-runbooks), ve grafik runbook'ları yalnızca oluşturulabilir gelen bir `.graphrunbook` dosya.
* A `.ps1` içeren PowerShell iş akışı dosyası yalnızca aktarılabilir bir [PowerShell iş akışı runbook'u](automation-runbook-types.md#powershell-workflow-runbooks).  Birden çok PowerShell iş akışı dosyası içeriyorsa, içe aktarma başarısız olur. Her bir iş akışı kendi dosyasına kaydedin ve her ayrı olarak içeri aktarmak gerekir.
* A `.ps1` bir iş akışı içermeyen bir dosya ya da aktarılabilir bir [PowerShell runbook'u](automation-runbook-types.md#powershell-runbooks) veya [PowerShell iş akışı runbook'u](automation-runbook-types.md#powershell-workflow-runbooks).  Bir PowerShell iş akışı runbook alınırsa, daha sonra bir iş akışına dönüştürülür ve açıklamaları yapılan değişiklikler belirtme runbook'ta yer alır.

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
8. Yapmanız gerekenler [runbook'u yayımlayamadı](#publish-a-runbook) önce çalıştırabilirsiniz.

> [!NOTE]
> Grafik runbook'u veya grafik PowerShell iş akışı runbook'u içeri aktardıktan sonra isterseniz başka bir türe dönüştürmek için seçeneğiniz vardır. Metinsel runbook'a dönüştürülemiyor.

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

## <a name="test-a-runbook"></a>Bir runbook'u test etme

Bir runbook'u test ettiğinizde [Taslak sürümü](#publish-a-runbook) yürütülür ve gerçekleştirdiği tüm işlemler tamamlanır. Hiçbir iş geçmişi oluşturulmaz, ancak [çıkış](automation-runbook-output-and-messages.md#output-stream) ve [uyarı ve hata](automation-runbook-output-and-messages.md#message-streams) akışları Test görüntülenen bölmesi çıktı. İletileri [Verbose Stream](automation-runbook-output-and-messages.md#message-streams) çıkış bölmesinde yalnızca görüntülenip [$VerbosePreference değişkeni](automation-runbook-output-and-messages.md#preference-variables) devam et ayarlanır.

Taslak sürüm çalıştırılır olsa da, runbook yine de normal olarak yürütür ve ortamda kaynaklara karşı herhangi bir eylem gerçekleştirir. Bu nedenle, runbook'ları üretim dışı kaynaklar üzerinde yalnızca test etmeniz gerekir.

Her test için yordamı [runbook türünü](automation-runbook-types.md) kullanıcının test metin düzenleyicisini Azure portalında grafik düzenleyicisini arasındaki fark aynı ve orada olduğu.  

1. Ya da runbook'un taslak sürümünü açın [metin düzenleyicisini](automation-edit-textual-runbook.md) veya [grafik düzenleyicisini](automation-graphical-authoring-intro.md).
1. Tıklayın **Test** düğmesini Test sayfasını açın.
1. Runbook'un parametreleri varsa, burada, test için kullanılan değerler sağlayabilirsiniz sol bölmesinde listelenir.
1. Test çalıştırmak isterseniz bir [karma Runbook çalışanı](automation-hybrid-runbook-worker.md), ardından değiştirme **çalıştırma ayarları** için **karma çalışanı** ve hedef grubu adını seçin.  Aksi takdirde, varsayılan tutun **Azure** testi bulutta çalıştırmak için.
1. Tıklayın **Başlat** testi başlatmak için düğme.
1. Runbook ise [PowerShell iş akışı](automation-runbook-types.md#powershell-workflow-runbooks) veya [grafik](automation-runbook-types.md#graphical-runbooks), durdurmak veya çıkış Bölmesi'nin altındaki düğmelerle edildiğini sırasında askıya alma. Bir runbook'u askıya aldığınızda, askıya alınmadan önce geçerli etkinliği tamamlar. Runbook askıya alındığında, durdurabilir veya yeniden başlatabilirsiniz.
1. Çıkış bölmesinde runbook çıktısını inceleyin.

## <a name="publish-a-runbook"></a>Runbook yayımlama

Oluştururken veya yeni bir runbook'u içeri çalışabilmesi için önce onu yayımlamanız gerekir.  Otomasyon içindeki her runbook'un bir taslak ve bir yayımlanmış sürümü vardır. Yalnızca Yayımlanan sürüm çalıştırılabilir ve yalnızca Taslak sürüm düzenlenebilir. Yayımlanan sürüm Taslak sürümdeki herhangi bir değişiklikten etkilenmez. Taslak sürümü kullanılabilir olması, size, yayımlanan sürümü taslak sürümle değiştirebilirsiniz yayımlamalısınız.

### <a name="azure-portal"></a>Azure portal

1. Runbook, Azure portalında açın.
2. Tıklayın **Düzenle** düğmesi.
3. Tıklayın **Yayımla** düğmesine ve ardından **Evet** doğrulama iletisi.

### <a name="powershell"></a>PowerShell

Kullanabileceğiniz [Yayımla-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/publish-azurermautomationrunbook) Windows PowerShell ile bir runbook'u yayımlamak için cmdlet'i. Aşağıdaki örnek komutlar bir örnek runbook'un nasıl yayımlanacağı gösterir.

```azurepowershell-interactive
$automationAccountName =  "AutomationAccount"
$runbookName = "Sample_TestRunbook"
$RGName = "ResourceGroup"

Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
-Name $runbookName -ResourceGroupName $RGName
```

## <a name="next-steps"></a>Sonraki adımlar

* Runbook ve modül Galerisi PowerShell kazanabileceğinizi hakkında bilgi edinmek için [Azure Otomasyonu Runbook ve modül galerileri](automation-runbook-gallery.md)
* PowerShell ve PowerShell iş akışı runbook'ları ile bir metin düzenleyicisini düzenleme hakkında daha fazla bilgi edinmek için [Azure Otomasyonu, metinsel runbook'ları düzenleme](automation-edit-textual-runbook.md)
* Grafik runbook yazma hakkında daha fazla bilgi edinmek için [Azure Otomasyonu'nda grafik yazma](automation-graphical-authoring-intro.md)
