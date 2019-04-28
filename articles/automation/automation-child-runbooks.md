---
title: Azure automation'da alt runbook'lar
description: Başka bir runbook'tan Azure Automation'da bir runbook başlatma ve bunlar arasında bilgi paylaşımı için farklı yöntemler açıklanır.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 01/17/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 84f17b76f03c01d0b1441a50b9bcbddc1dfe2ef3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61081587"
---
# <a name="child-runbooks-in-azure-automation"></a>Azure automation'da alt runbook'lar

Azure Automation'ın diğer runbook'lar tarafından olan ayrı bir işleve sahip yeniden kullanılabilir, modüler runbook'lar yazmak için önerilen bir yöntemdir. Üst runbook genellikle gerekli işlevselliği gerçekleştirmek için bir veya daha fazla alt runbook'u çağırır. Bir alt runbook'u çağırmanın iki yolu vardır ve her anlamanız gereken belirli farklara sahiptir, böylece farklı senaryolarınız için en iyi olduğu belirleyebilirsiniz.

## <a name="invoking-a-child-runbook-using-inline-execution"></a>Satır içi yürütme kullanarak alt runbook'u çağırma

Başka bir runbook'tan bir runbook'u satır içi olarak çağırmak için, bir etkinlik veya cmdlet'i kullanırken yaptığınız gibi runbook'un adını kullanır ve parametreleri için değerler belirtirsiniz.  Aynı Otomasyon hesaptaki tüm runbook'lar tarafından bu şekilde kullanılacak. Üst runbook bir sonraki satıra geçmeden önce alt runbook'un tamamlanmasını bekler ve herhangi bir çıkış doğrudan üst öğeye döndürülür.

Bir runbook'u satır içi olarak çağırdığınızda üst runbook ile aynı işi çalıştırır. Çalıştırdığı alt runbook'un iş geçmişinde hiçbir belirti yoktur. Tüm özel durumlar ve akış çıkışları alt runbook'tan üst öğesi ile ilişkili. Bu davranış, daha az iş çalıştırılır ve bunları izlemek ve alt runbook tarafından karşılaşılan özel durumlar bu yana sorun giderme için daha kolay hale getirir ve kendi akış çıkışları üst işle ilişkili olan.

Bir runbook yayımlandığında, çağıran alt runbook'ları zaten yayımlanması gerekir. Bu durum, bir runbook derlendiğinde, Azure Otomasyonu alt runbook'larla bir ilişkilendirme oluşturur çünkü. Değilseniz, üst runbook doğru şekilde yayımlamak için görünür ancak başlatıldığında bir özel durum oluşturur. Bu durumda, alt runbook'ları doğru bir şekilde başvurmak için üst runbook'u yeniden yayımlayabilirsiniz. İlişki zaten oluşturulmuş olduğundan tüm alt runbook'lardan biri değiştirilirse üst runbook'u yeniden yayımlamanız gerekmez.

Satır içi olarak adlandırılan bir alt runbook'un parametreleri karmaşık nesneler de dahil olmak üzere herhangi bir veri türü olabilir. Yok hiçbir [JSON serileştirme](start-runbooks.md#runbook-parameters) , Azure portalını kullanarak bir runbook'u başlattığınızda veya Start-AzureRmAutomationRunbook cmdlet'iyle olduğundan.

### <a name="runbook-types"></a>Runbook türleri

Hangi türlerin birbirine çağırabilirsiniz:

* A [PowerShell runbook'u](automation-runbook-types.md#powershell-runbooks) ve [grafik runbook'ları](automation-runbook-types.md#graphical-runbooks) (her ikisi de olan PowerShell tabanlı) her bir satır çağırabilirsiniz.
* A [PowerShell iş akışı runbook'u](automation-runbook-types.md#powershell-workflow-runbooks) ve grafik PowerShell iş akışı runbook'ları (her ikisi de olan PowerShell iş akışı tabanlı) her bir satır çağırabilir
* PowerShell türleri ve PowerShell iş akışı türlerini birbirine satır içi olarak çağırdığınızda olamaz ve Start-AzureRmAutomationRunbook kullanması gerekir.

Zaman sipariş sağlasa da yayımlama:

* Runbook'ları Yayımla sırasını yalnızca PowerShell iş akışı ve grafik PowerShell iş akışı runbook'ları için önemlidir.

Satır içi yürütme kullanarak bir grafik veya PowerShell iş akışı alt runbook'u çağırmanın, runbook'un adını kullanın.  Bir PowerShell alt runbook'u çağırmanın, adıyla başlamalıdır *.\\*  betiği yerel dizinde bulunduğunu belirtmek için.

### <a name="example"></a>Örnek

Aşağıdaki örnek, üç parametre, karmaşık bir nesne, bir tamsayı ve bir Boole değeri kabul eden bir test alt runbook başlatır. Alt runbook'un çıkışı bir değişkene atanır.  Bu durumda, alt runbook'u bir PowerShell iş akışı runbook ' dir.

```azurepowershell-interactive
$vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
$output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true
```

Alt öğesi olarak bir PowerShell runbook kullanarak aynı örneği verilmiştir.

```azurepowershell-interactive
$vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
$output = .\PS-ChildRunbook.ps1 –VM $vm –RepeatCount 2 –Restart $true
```

## <a name="starting-a-child-runbook-using-cmdlet"></a>Cmdlet'ini kullanarak bir alt runbook'u başlatma

> [!IMPORTANT]
> Bir alt runbook ile çağırdığınız varsa `Start-AzureRmAutomationRunbook` cmdlet'iyle `-Wait` anahtarını ve alt runbook sonuçlarının bir nesne ise, hatalarla karşılaşabilirsiniz. Hatayı çözmek için bkz: [nesne çıkış alt runbook'larla](troubleshoot/runbooks.md#child-runbook-object) sonuçlar için yoklama ve kullanmak için mantığı uygulaması hakkında bilgi edinmek için [Get-AzureRmAutomationJobOutputRecord](/powershell/module/azurerm.automation/get-azurermautomationjoboutputrecord)

Kullanabileceğiniz [Start-AzureRmAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) açıklanan şekilde bir runbook başlatmak için cmdlet [Windows PowerShell ile bir runbook başlatmak için](start-runbooks.md#start-a-runbook-with-powershell). Bu cmdlet'in kullanımı iki mod vardır.  Alt runbook için alt proje oluşturulduğunda bir modda cmdlet iş kimliğini döndürür.  Belirterek etkinleştirmek diğer bütün modunda **-bekleyin** parametresini cmdlet'e alt iş tamamlanır ve alt runbook'tan çıkış döndürür kadar bekler.

Cmdlet ile başlatılan bir alt runbook işinden üst runbook'tan ayrı bir işlemde çalıştırır. Bu davranış, runbook'u satır içi başlangıç değerinden daha fazla iş sonuçlanır ve izlemek daha zor hale getirir. Üst her birinin tamamlanmasını beklemeden birden fazla alt runbook zaman uyumsuz olarak başlatabilirsiniz. Alt runbook'ların satır içi olarak çağrıldığı paralel yürütme türü için, üst runbook'un [parallel anahtar kelimesini](automation-powershell-workflow.md#parallel-processing)kullanması gerekir.

Alt runbook'ları çıktısını olmayan döndürülen üst runbook için güvenilir bir şekilde zamanlama nedeniyle. Ayrıca bazı değişkenler $VerbosePreference, $WarningPreference, ister ve diğer alt runbook'larına dağıtılmasını değil. Bu sorunlarla karşılaşmamak için alt runbook'ları kullanarak ayrı Otomasyon iş olarak başlayabilirsiniz `Start-AzureRmAutomationRunbook` cmdlet'iyle `-Wait` geçin. Üst runbook, alt runbook tamamlanana kadar bu engeller.

Üst runbook bekliyor engellenmesi istemiyorsanız, alt runbook kullanmaya başlayabilirsiniz `Start-AzureRmAutomationRunbook` olmadan cmdlet'i `-Wait` geçin. Daha sonra kullanmanız gerekir `Get-AzureRmAutomationJob` işin tamamlanmasını beklemek ve `Get-AzureRmAutomationJobOutput` ve `Get-AzureRmAutomationJobOutputRecord` sonuçlarını almak için.

Cmdlet ile başlatılan bir alt runbook için parametreler [Runbook Parametreleri](start-runbooks.md#runbook-parameters)bölümünde açıklandığı gibi karma tablosu olarak sağlanır. Yalnızca basit veri türleri kullanılabilir. Runbook karmaşık veri türü içeren bir parametreye sahipse satır içi olarak çağrılmalıdır.

Alt runbook'lar ayrı işler başlatılırken, abonelik bağlamına kaybolmuş olabilir. Belirli bir Azure aboneliği karşı Azure RM cmdlet'lerini çalıştırmak alt runbook için sırada, üst runbook bağımsız olarak bu abonelik için alt runbook'un doğrulaması gerekir.

Bir işlemde bir aboneliğin seçilmesi, işleri aynı Otomasyon hesabı içindeki birden fazla abonelik ile çalışıyorsanız, diğer işleri şu an seçili abonelik bağlamını değişebilir. Bu sorunu önlemek için `Disable-AzureRmContextAutosave –Scope Processsave` her runbook başına. Bu eylem, yalnızca söz konusu runbook yürütmesi bağlamını kaydeder.

### <a name="example"></a>Örnek

Aşağıdaki örnek, parametrelerle bir alt runbook başlatır ve Start-AzureRmAutomationRunbook kullanarak tamamlanmasını bekler-parametre bekleyin. Tamamlandığında, alt runbook'un çıktısını toplanır. Kullanılacak `Start-AzureRmAutomationRunbook`, Azure aboneliğiniz için kimlik doğrulaması gerekir.

```azurepowershell-interactive
# Ensures you do not inherit an AzureRMContext in your runbook
Disable-AzureRmContextAutosave –Scope Process

# Connect to Azure with RunAs account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true}

Start-AzureRmAutomationRunbook `
    –AutomationAccountName 'MyAutomationAccount' `
    –Name 'Test-ChildRunbook' `
    -ResourceGroupName 'LabRG' `
    -AzureRMContext $AzureContext `
    –Parameters $params –wait
```

## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Bir alt runbook çağırma yöntemlerinin karşılaştırılması

Aşağıdaki tabloda bir runbook'tan başka bir runbook çağırmanın iki yöntemi arasındaki farklar özetlenmiştir.

|  | Satır içi | Cmdlet |
|:--- |:--- |:--- |
| İş |Alt runbook'lar üst runbook'la aynı işte çalışır. |Alt runbook için ayrı bir iş oluşturulur. |
| Yürütme |Üst runbook devam etmeden önce alt runbook'un tamamlanmasını bekler. |Üst runbook alt runbook başlatıldıktan hemen devam *veya* üst runbook alt işi tamamlamak bekler. |
| Çıktı |Üst runbook doğrudan alt runbook'tan çıkış alabilir. |Üst runbook alt runbook işinden çıkış almak gerekir *veya* üst runbook alt runbook'tan çıkış doğrudan alabilirsiniz. |
| Parametreler |Alt runbook parametre değerleri ayrı ayrı belirtilir ve herhangi bir veri türünü kullanabilir. |Alt runbook parametreleri için değerler tek bir karma tablosunda birleştirilmesi gerekir. Yalnızca bu hashtable basit dahil, dizi ve nesne JSON serileştirmesi kullanan veri türleri. |
| Otomasyon Hesabı |Üst runbook, alt runbook yalnızca aynı Otomasyon hesabında kullanabilirsiniz. |Üst runbook alt runbook'tan bir bağlantınız herhangi bir Otomasyon hesabı aynı Azure aboneliği ve farklı bir aboneliği kullanabilirsiniz. |
| Yayımlama |Üst runbook yayımlanmadan önce alt runbook yayımlanmalıdır. |Önce üst runbook başlatıldığında her alt runbook yayımlanmalıdır. |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Automation'da bir runbook başlatma](start-runbooks.md)
* [Runbook çıkışı ve iletileri Azure Otomasyonu](automation-runbook-output-and-messages.md)

