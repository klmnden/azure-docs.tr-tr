---
title: Azure automation'da alt runbook'lar
description: Başka bir runbook'tan Azure Automation'da bir runbook başlatma ve bunlar arasında bilgi paylaşımı için farklı yöntemler açıklanır.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 08/14/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 2060239b27ef05c34ea6f5b388b4c4086a44a826
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/15/2018
ms.locfileid: "42057128"
---
# <a name="child-runbooks-in-azure-automation"></a>Azure automation'da alt runbook'lar

Diğer runbook'lar tarafından kullanılabilen ayrı işleve sahip yeniden kullanılabilir, modüler runbook'lar yazmak için Azure automation'da en iyi bir yöntemdir. Üst runbook genellikle gerekli işlevselliği gerçekleştirmek için bir veya daha fazla alt runbook'u çağırır. Bir alt runbook'u çağırmanın iki yolu vardır ve her anlamanız gereken belirli farklara sahiptir ve böylece farklı senaryolarınız için en iyi olan belirleyebilirsiniz.

## <a name="invoking-a-child-runbook-using-inline-execution"></a>Satır içi yürütme kullanarak alt runbook'u çağırma

Başka bir runbook'tan bir runbook'u satır içi çağrılacak runbook adını kullanın ve tam olarak, bir etkinlik veya cmdlet'i kullanırken yaptığınız gibi parametreleri için değerler sağlayın.  Aynı Otomasyon hesaptaki tüm runbook'lar tarafından bu şekilde kullanılacak. Üst runbook bir sonraki satıra geçmeden önce alt runbook'un bekler ve herhangi bir çıkış doğrudan üst öğeye döndürülür.

Bir runbook'u satır içi çağırdığınızda üst runbook ile aynı işi çalıştırır. Çalıştırdığı alt runbook'un iş geçmişinde hiçbir belirti olmaz. Tüm özel durumlar ve akış çıkışları alt runbook'tan üst öğesi ile ilişkilendirilecektir. Bu daha az iş çalıştırılır ve bunları izlemek ve alt runbook tarafından karşılaşılan özel durumlar bu yana sorun giderme için daha kolay hale getirir ve kendi akış çıkışları üst işle ilişkili olan.

Bir runbook yayımlandığında, çağıran alt runbook'ları zaten yayımlanması gerekir. Bu durum, bir runbook derlendiğinde, Azure Otomasyonu alt runbook'larla bir ilişkilendirme oluşturur çünkü. Değilseniz, üst runbook doğru şekilde yayımlamak için görünür ancak başlatıldığında bir özel durum oluşturur. Böyle bir durumda, üst runbook alt runbook'ları doğru bir şekilde başvurmak için yayımlayabilirsiniz. İlişki zaten oluşturulmuş olduğundan tüm alt runbook'lardan biri değiştirilirse üst runbook'u yeniden yayımlamanız gerekmez.

Satır içi olarak adlandırılan bir alt runbook'un parametreleri karmaşık nesneler de dahil olmak üzere herhangi bir veri türü olabilir ve yoktur hiçbir [JSON serileştirme](automation-starting-a-runbook.md#runbook-parameters) , Azure portalını kullanarak bir runbook'u başlattığınızda veya sahip olduğu Start-AzureRmAutomationRunbook cmdlet'i.

### <a name="runbook-types"></a>Runbook türleri

Hangi türlerin birbirine çağırabilirsiniz:

* A [PowerShell runbook'u](automation-runbook-types.md#powershell-runbooks) ve [grafik runbook'ları](automation-runbook-types.md#graphical-runbooks) (her ikisi de olan PowerShell tabanlı) her bir satır çağırabilirsiniz.
* A [PowerShell iş akışı runbook'u](automation-runbook-types.md#powershell-workflow-runbooks) ve grafik PowerShell iş akışı runbook'ları (her ikisi de olan PowerShell iş akışı tabanlı) her bir satır çağırabilir
* PowerShell türleri ve PowerShell iş akışı türlerini birbirine satır içi olarak çağırdığınızda olamaz ve Start-AzureRmAutomationRunbook kullanması gerekir.

Zaman sipariş sağlasa da yayımlama:

* Runbook'ları Yayımla sırasını yalnızca PowerShell iş akışı ve grafik PowerShell iş akışı runbook'ları için önemlidir.

Satır içi yürütme kullanarak bir grafik veya PowerShell iş akışı alt runbook'u çağırmanın, yalnızca runbook'un adını kullanın.  Bir PowerShell alt runbook'u çağırmanın, adıyla başlamalıdır *.\\*  betiği yerel dizinde bulunduğunu belirtmek için.

### <a name="example"></a>Örnek

Aşağıdaki örnek, üç parametre, karmaşık bir nesne, bir tamsayı ve bir Boole değeri kabul eden bir test alt runbook'u çağırır. Alt runbook'un çıkışı bir değişkene atanır.  Bu durumda, alt runbook'u bir PowerShell iş akışı runbook ' dir.

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

Kullanabileceğiniz [Start-AzureRmAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook) açıklanan şekilde bir runbook başlatmak için cmdlet [Windows PowerShell ile bir runbook başlatmak için](automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell). Bu cmdlet'in kullanımı iki mod vardır.  Alt runbook için alt iş oluşturulduktan hemen sonra bir modda cmdlet iş kimliğini döndürür.  Belirterek etkinleştirmek diğer bütün modunda **-bekleyin** parametresini cmdlet'e alt kadar bekleyecek işi tamamlandığında ve alt runbook'tan çıkışı döndürür.

Cmdlet ile başlatılan bir alt runbook işinden üst runbook'tan ayrı bir işlemde çalışır. Bu runbook'u satır içi çağırma değerinden daha fazla iş sonuçlanır ve izlemek daha zor hale getirir. Üst her birinin tamamlanmasını beklemeden birden çok alt runbook zaman uyumsuz olarak başlatabilirsiniz. Alt runbook'ların satır içi olarak çağrıldığı Paralel yürütme türü için üst runbook kullanması gerekir [parallel anahtar kelimesini](automation-powershell-workflow.md#parallel-processing).

Alt runbook'ları çıktısını alınmadı güvenilir bir şekilde zamanlama nedeniyle üst runbook. Ayrıca bazı değişkenler $VerbosePreference, $WarningPreference, ister ve diğer alt runbook'larına dağıtılmasını değil. Bu sorunlarla karşılaşmamak için alt runbook'ları kullanarak ayrı Otomasyon iş olarak çağırabilirsiniz `Start-AzureRmAutomationRunbook` cmdlet'iyle `-Wait` geçin. Üst runbook, alt runbook tamamlanana kadar bu engeller.

Üst runbook bekliyor engellenmesi istemiyorsanız, alt runbook kullanarak çağırabileceğiniz `Start-AzureRmAutomationRunbook` olmadan cmdlet'i `-Wait` geçin. Daha sonra kullanmanız gerekir `Get-AzureRmAutomationJob` işin tamamlanmasını beklemek ve `Get-AzureRmAutomationJobOutput` ve `Get-AzureRmAutomationJobOutputRecord` sonuçlarını almak için.

Cmdlet ile başlatılan bir alt runbook için parametreler açıklandığı gibi karma tablosu olarak sağlanır [Runbook parametreleri](automation-starting-a-runbook.md#runbook-parameters). Yalnızca basit veri türleri kullanılabilir. Ardından runbook karmaşık veri türüne sahip bir parametreye sahipse, satır içi çağrılmalıdır.

Birden çok aboneliği ile çalışıyorsanız, abonelik bağlamına alt runbook'ları çağrılırken kaybolmuş olabilir. Abonelik bağlamına alt runbook'larına geçirilir emin olmak için ekleme `DefaultProfile` cmdlet'i ve ona geçiş bağlam parametresi.

### <a name="example"></a>Örnek

Aşağıdaki örnek, parametrelerle bir alt runbook başlatır ve Start-AzureRmAutomationRunbook kullanarak tamamlanmasını bekler-parametre bekleyin. Tamamlandığında, alt runbook'un çıktısını toplanır. Kullanılacak `Start-AzureRmAutomationRunbook`, Azure aboneliğiniz için kimlik doğrulaması gerekir.

```azurepowershell-interactive
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
    -DefaultProfile $AzureContext `
    –Parameters $params –wait
```

## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Bir alt runbook çağırma yöntemlerinin karşılaştırılması

Aşağıdaki tabloda, başka bir runbook'tan bir runbook'u çağırmak için iki yöntem arasındaki farklar özetlenmektedir.

|  | Satır İçi | Cmdlet |
|:--- |:--- |:--- |
| İş |Alt runbook'lar üst ile aynı işi çalıştırın. |Alt runbook için ayrı bir iş oluşturulur. |
| Yürütme |Üst runbook alt runbook devam etmeden önce tamamlanmasını bekler. |Üst runbook alt runbook başlatıldıktan hemen devam *veya* üst runbook alt işi tamamlamak bekler. |
| Çıktı |Üst runbook doğrudan alt runbook'tan çıkış elde edebilirsiniz. |Üst runbook alt runbook işinden çıkış almak gerekir *veya* üst runbook alt runbook'tan çıkış doğrudan alabilirsiniz. |
| Parametreler |Alt runbook parametre değerlerini ayrı ayrı belirtilir ve herhangi bir veri türü kullanabilirsiniz. |JSON serileştirmesi kullanan nesne veri türlerini ve değerleri dizisi, alt runbook parametreleri tek bir karma tablosunda birleştirilmelidir ve yalnızca basit içerebilir. |
| Otomasyon Hesabı |Üst runbook, alt runbook yalnızca aynı Otomasyon hesabında kullanabilirsiniz. |Bir bağlantı varsa, üst runbook alt runbook'tan herhangi bir Otomasyon hesabı aynı Azure aboneliği ve farklı bir aboneliği kullanabilirsiniz. |
| Yayımlama |Üst runbook yayımlanmadan önce alt runbook yayımlanmalıdır. |Üst runbook başlatılmadan önce istediğiniz zaman alt runbook yayımlanmalıdır. |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Automation'da bir runbook başlatma](automation-starting-a-runbook.md)
* [Runbook çıkışı ve iletileri Azure Otomasyonu](automation-runbook-output-and-messages.md)
