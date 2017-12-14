---
title: "Oluşturma veya bir Azure Otomasyonu runbook'u içeri aktarma"
description: "Bu makalede, Azure Automation'da yeni bir runbook oluşturmak veya bir dosyadan içe açıklar."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 24414362-b690-4474-8ca7-df18e30fc31d
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/29/2017
ms.author: magoedte;bwren
ms.openlocfilehash: f2e34e6a4d3d2f29fe6320d805e38e6fccbb74de
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="creating-or-importing-a-runbook-in-azure-automation"></a>Oluşturma veya bir Azure Otomasyonu runbook'u içeri aktarma
Bir runbook'un Azure Otomasyon ya da ekleyebilirsiniz [yenisini oluşturmadan](#creating-a-new-runbook) veya bir dosya ya da mevcut bir runbook'u alarak [Runbook Galerisi](automation-runbook-gallery.md). Bu makalede, oluşturma ve runbook'ları bir dosyadan içeri aktarma hakkında bilgiler sağlar.  Tüm topluluk runbook'ları ve modülleri erişme hakkında bilgi almak [Azure Otomasyonu Runbook ve modül galerileri](automation-runbook-gallery.md).

## <a name="creating-a-new-runbook"></a>Yeni bir runbook oluşturma
Azure portalı veya Windows PowerShell kullanarak Azure Otomasyonu'nda yeni bir runbook oluşturabilirsiniz. Runbook oluşturulduktan sonra bu bilgileri kullanarak düzenleyebilirsiniz [öğrenme PowerShell iş akışı](automation-powershell-workflow.md) ve [Azure Automation'da grafik yazma](automation-graphical-authoring-intro.md).

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-classic-portal"></a>Azure Klasik portalı ile yeni bir Azure Otomasyonu runbook oluşturmak için
Yalnızca çalışabilirsiniz [PowerShell iş akışı runbook'ları](automation-runbook-types.md#powershell-workflow-runbooks) Azure portalında.

1. Azure Klasik portalında tıklayın, **yeni**, **uygulama hizmetleri**, **Otomasyon**, **Runbook**, **hızlı Oluştur**.
2. Gerekli bilgileri girin ve ardından **oluşturma**. Runbook adı bir harf ile başlamalı ve harf, rakam, alt çizgi ve çizgi olabilir.
3. Runbook'u hemen düzenlemek istiyorsanız, i **Runbook'u Düzenle**. Aksi takdirde tıklatın **Tamam**.
4. Yeni runbook'unuz kasasındaki **Runbook'lar** sekmesi.

### <a name="to-create-a-new-azure-automation-runbook-with-the-azure-portal"></a>Azure portalı ile yeni bir Azure Otomasyonu runbook oluşturmak için
1. Azure portalında, Otomasyon hesabınızı açın.
2. Hub'ından seçin **Runbook'lar** listesini açmak için.
3. Tıklayın **runbook Ekle** düğmesine ve ardından **yeni bir runbook oluşturmak**.
4. Tür a **adı** seçin ve runbook için kendi [türü](automation-runbook-types.md). Runbook adı bir harf ile başlamalı ve harf, rakam, alt çizgi ve çizgi olabilir.
5. Tıklatın **oluşturma** runbook oluşturma ve Düzenleyicisi'ni açın.

### <a name="to-create-a-new-azure-automation-runbook-with-windows-powershell"></a>Windows PowerShell ile yeni bir Azure Otomasyonu runbook oluşturmak için
Kullanabileceğiniz [yeni AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt619376.aspx) boş bir oluşturmak için cmdlet'i [PowerShell iş akışı runbook'u](automation-runbook-types.md#powershell-workflow-runbooks). Ya da belirtebilirsiniz **adı** daha sonra düzenleyebilirsiniz veya belirtebilirsiniz boş bir runbook oluşturmak için parametre **yolu** runbook dosyası içeri aktarmak için parametre. **Türü** parametresi de dört runbook türlerinden birini belirtmek için dahil olmalıdır.

Aşağıdaki örnek komutlarda yeni bir boş runbook'unun nasıl oluşturulacağını gösterir.

    New-AzureRmAutomationRunbook -AutomationAccountName MyAccount `
    -Name NewRunbook -ResourceGroupName MyResourceGroup -Type PowerShell

## <a name="importing-a-runbook-from-a-file-into-azure-automation"></a>Bir runbook, Azure Automation'a bir dosyadan içeri aktarma
Azure Otomasyonu'nda bir PowerShell komut dosyası veya PowerShell iş akışı (.ps1 uzantılı), dışarı aktarılan bir grafik runbook (.graphrunbook) veya bir Python 2 komut dosyası (.py uzantılı) içeri aktararak, yeni bir runbook oluşturabilirsiniz.  Belirtmeniz gerekir [runbook türü](automation-runbook-types.md) aşağıdaki noktaları dikkate alarak, içeri aktarma sırasında oluşturulur.

* .Graphrunbook dosya yalnızca yeni bir alınabilir [grafik runbook](automation-runbook-types.md#graphical-runbooks), ve grafik runbook'lar .graphrunbook dosyasından yalnızca oluşturulabilir.
* Bir PowerShell iş akışı içeren bir .ps1 dosyası yalnızca içine aktarılabilen bir [PowerShell iş akışı runbook'u](automation-runbook-types.md#powershell-workflow-runbooks).  Dosyada birden çok PowerShell iş akışı varsa, içeri aktarma başarısız olur. Her bir iş akışı kendi dosyaya kaydedin ve her ayrı ayrı alın.
* Bir iş akışı içermeyen bir .ps1 dosyası da aktarılabilen bir [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) veya [PowerShell iş akışı runbook'u](automation-runbook-types.md#powershell-workflow-runbooks).  PowerShell iş akışı runbook'a içe aktardıysanız, daha sonra bir iş akışına dönüştürülür ve açıklamaları yapılan değişiklikler belirtme runbook'ta içindedir.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-classic-portal"></a>Klasik Azure portalı ile dosyasından bir runbook'u içeri aktarma
Bir komut dosyası Azure Automation'a içeri aktarmak için aşağıdaki yordamı kullanabilirsiniz.  Bu portalı kullanarak bir PowerShell iş akışı runbook'u yalnızca bir .ps1 dosyasına aktarabilirsiniz unutmayın.  Diğer türleri için Azure Portalı'nı kullanmanız gerekir.

1. Klasik Azure portalında seçin **Otomasyon** ve ardından bir Otomasyon hesabı seçin.
2. **İçeri Aktar**’a tıklayın.
3. Tıklatın **dosyasına göz atın** ve almak için komut dosyasını bulun.
4. Runbook'u hemen düzenlemek istiyorsanız, i **Runbook'u Düzenle**. Aksi takdirde, Tamam'ı tıklatın.
5. Yeni runbook kasasındaki **Runbook'lar** Otomasyon hesabının sekmesi.
6. Yapmanız gerekenler [runbook yayımlama](#publishing-a-runbook) çalıştırabilmeniz için önce.

### <a name="to-import-a-runbook-from-a-file-with-the-azure-portal"></a>Azure portal ile bir dosyasından bir runbook'u içeri aktarma
Bir komut dosyası Azure Automation'a içeri aktarmak için aşağıdaki yordamı kullanabilirsiniz.  

> [!NOTE]
> Portalı kullanarak bir PowerShell iş akışı runbook'a yalnızca bir .ps1 dosyasına aktarabilirsiniz unutmayın.
> 
> 

1. Azure portalında, Otomasyon hesabınızı açın.
2. Hub'ından seçin **Runbook'lar** listesini açmak için.
3. Tıklayın **runbook Ekle** düğmesine ve ardından **alma**.
4. Tıklatın **Runbook dosyası** alınacak dosyayı seçmek için
5. Varsa **adı** alanı etkinse ve sonra değiştirmek için seçeneğiniz vardır.  Runbook adı bir harf ile başlamalı ve harf, rakam, alt çizgi ve çizgi olabilir.
6. [Runbook türü](automation-runbook-types.md) otomatik olarak seçilir, ancak geçerli kısıtlamaları göz önüne alındıktan sonra türünü değiştirebilirsiniz. 
7. Yeni runbook Otomasyon hesabı için runbook'ları listesinde görüntülenir.
8. Yapmanız gerekenler [runbook yayımlama](#publishing-a-runbook) çalıştırabilmeniz için önce.

> [!NOTE]
> Bir grafik runbook veya bir grafik PowerShell iş akışı runbook içeri aktardıktan sonra diğer tür istediyseniz dönüştürmek için seçeneğiniz vardır. Bir metinsel runbook'a dönüştürülemiyor.
>  
> 

### <a name="to-import-a-runbook-from-a-script-file-with-windows-powershell"></a>Windows PowerShell ile bir betik dosyasından bir runbook'u içeri aktarma
Kullanabileceğiniz [alma AzureRMAutomationRunbook](https://msdn.microsoft.com/library/mt603735.aspx) cmdlet'ini taslak olarak PowerShell iş akışı runbook'u bir komut dosyası. Kullanmadığınız sürece runbook zaten varsa, içeri aktarma başarısız *-Force* parametresi. 

Aşağıdaki örnek komutlar bir komut dosyası bir runbook'a içeri aktarma göstermektedir.

    $automationAccountName =  "AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $scriptPath = "C:\Runbooks\Sample_TestRunbook.ps1"
    $RGName = "ResourceGroup"

    Import-AzureRMAutomationRunbook -Name $runbookName -Path $scriptPath `
    -ResourceGroupName $RGName -AutomationAccountName $automationAccountName `
    -Type PowerShellWorkflow 


## <a name="publishing-a-runbook"></a>Runbook yayımlama
Oluşturduğunuzda ya da yeni bir runbook'u içeri çalıştırabilmeniz için önce onu yayımlamanız gerekir.  Otomasyon içindeki her runbook'un bir taslak ve bir yayımlanmış sürümü vardır. Yalnızca yayımlanan sürüm çalıştırılabilir ve yalnızca taslak sürüm düzenlenebilir. Yayımlanan sürüm taslak sürümdeki herhangi bir değişiklikten etkilenmez. Taslak sürümü kullanılabilir hale getirmek istediğinizde, size, yayımlanan sürümü taslak sürümle değiştirebilirsiniz yayımlayın.

## <a name="to-publish-a-runbook-using-the-azure-classic-portal"></a>Klasik Azure portalını kullanarak bir runbook'u yayımlamak için
1. Azure Klasik Portalı'nda runbook'u açın.
2. Ekranın üstündeki **Yazar**.
3. Ekranın alt kısmındaki tıklatın **Yayımla** ve ardından **Evet** doğrulama iletisi.

## <a name="to-publish-a-runbook-using-the-azure-portal"></a>Azure Portalı'nı kullanarak bir runbook'u yayımlamak için
1. Azure portalında runbook'u açın.
2. Tıklatın **Düzenle** düğmesi.
3. Tıklatın **Yayımla** düğmesine ve ardından **Evet** doğrulama iletisi.

## <a name="to-publish-a-runbook-using-windows-powershell"></a>Windows PowerShell kullanarak bir runbook'u yayımlamak için
Kullanabileceğiniz [Yayımla AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603705.aspx) Windows PowerShell ile bir runbook'u yayımlamak için cmdlet. Aşağıdaki örnek komutlar bir örnek runbook'un nasıl yayımlanacağı göstermektedir.

    $automationAccountName =  AutomationAccount"
    $runbookName = "Sample_TestRunbook"
    $RGName = "ResourceGroup"

    Publish-AzureRmAutomationRunbook -AutomationAccountName $automationAccountName `
    -Name $runbookName -ResourceGroupName $RGName


## <a name="next-steps"></a>Sonraki Adımlar
* Nasıl size Runbook ve PowerShell modülü Galerisi sağlayacağı hakkında bilgi edinmek için [Azure Otomasyonu Runbook ve modül galerileri](automation-runbook-gallery.md)
* Bir metin düzenleyicisiyle PowerShell ve PowerShell iş akışı runbook'ları düzenleme hakkında daha fazla bilgi için bkz: [Azure Otomasyonu'nda metinsel runbook'lar düzenleme](automation-edit-textual-runbook.md)
* Grafik runbook yazma hakkında daha fazla bilgi için bkz: [Azure Automation'da grafik yazma](automation-graphical-authoring-intro.md)

