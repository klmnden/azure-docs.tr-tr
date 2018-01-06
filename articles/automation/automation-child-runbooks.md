---
title: "Alt runbook'ları Azure Automation | Microsoft Docs"
description: "Azure Otomasyonu'nda başka bir runbook'tan runbook başlatma ve bunlar arasında bilgi paylaşımı için farklı yöntemleri açıklar."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 919887b9-43e2-4c16-883c-f81807fe37db
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 5c18444b5a2767ccdd9a61a3bc9218fa4c0aac04
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2018
---
# <a name="child-runbooks-in-azure-automation"></a>Azure Otomasyonu'nda alt runbook'lar
Azure Automation'ın diğer runbook'lar tarafından kullanılabilen ayrı işleve sahip yeniden kullanılabilir, modüler runbook'lar yazmak için en iyi bir uygulamadır. Üst runbook genellikle gerekli işlevleri gerçekleştirmek için bir veya daha fazla alt runbook'u çağırır. Bir alt runbook'u çağırmanın iki yolu vardır ve her anlamanız gereken belirli farklara sahiptir, farklı senaryolarınız için en iyi olacağı belirleyebilmesi.

## <a name="invoking-a-child-runbook-using-inline-execution"></a>Satır içi yürütme kullanarak bir alt runbook çağırma
Başka bir runbook'tan bir runbook'u satır içi olarak çağırmak için runbook adı kullanın ve tam olarak, bir etkinlik veya cmdlet'i kullanırken yaptığınız gibi parametreleri için değerler sağlayın.  Aynı Otomasyon hesaptaki tüm runbook'lar bu şekilde kullanılacak diğerleri tarafından kullanılabilir. Üst runbook alt runbook'un bir sonraki satıra geçmeden önce tamamlanmasını bekler ve herhangi bir çıkış doğrudan üst öğeye döndürülür.

Bir runbook'u satır içi çağırdığınızda üst runbook ile aynı işi çalıştırır. Çalıştırdığı alt runbook'un iş geçmişinde hiçbir belirti olmaz. Tüm özel durumlar ve alt runbook'tan çıkış akışı üst öğesi ile ilişkilendirilir. Bu daha az iş çalıştırılır ve izlemek için ve alt runbook'un ve tüm ve akış çıkışı tarafından karşılaşılan özel durumlar üst işle ilişkili olduğundan gidermek için daha kolay hale getirir.

Bir runbook yayımlandığında, bu runbook'un çağırdığı tüm alt runbook zaten yayımlanması gerekir. Azure Otomasyonu runbook derlendiğinde alt runbook'larla bir ilişkilendirme derlemeler olmasıdır. Yoksa, üst runbook doğru şekilde yayımlamak için görünür ancak başlatıldığında bir özel durum oluşturur. Bu durumda, üst runbook alt runbook'ları doğru şekilde başvurmak için yayımlayabilirsiniz. İlişki zaten oluşturulmuş olduğundan herhangi bir alt runbook'lardan biri değiştirilirse üst runbook'u yeniden yayımlamanız gerekmez.

Satır içi olarak adlandırılan bir alt runbook'un parametreleri karmaşık nesneler de dahil olmak üzere herhangi bir veri türü olabilir ve olmadığından hiçbir [JSON serileştirmesi](automation-starting-a-runbook.md#runbook-parameters) Azure portal'ı kullanarak runbook'u başlattığınızda veya sahip olduğu Start-AzureRmAutomationRunbook cmdlet'ini kullanın.

### <a name="runbook-types"></a>Runbook türleri
Hangi tür birbirine çağırabilirsiniz:

* A [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) ve [grafik runbook'lar](automation-runbook-types.md#graphical-runbooks) (her ikisi de olan temel PowerShell) her bir satır çağırabilirsiniz.
* A [PowerShell iş akışı runbook'u](automation-runbook-types.md#powershell-workflow-runbooks) ve grafik PowerShell iş akışı runbook'ları (her ikisi de olan temel PowerShell iş akışı) her bir satır çağırın
* PowerShell türleri ve PowerShell iş akışı türleri birbirine satır içi çağrılamıyor ve başlangıç AzureRmAutomationRunbook kullanmanız gerekir.

Zaman sipariş sağlasa da yayımlayın:

* Runbook'ları Yayımla sırasını yalnızca PowerShell iş akışı ve grafik PowerShell iş akışı runbook'ları için önemlidir.

Satır içi yürütme kullanarak bir grafik veya PowerShell iş akışı alt runbook'u çağırdığınızda, yalnızca runbook'un adını kullanabilirsiniz.  PowerShell alt runbook çağırdığınızda, gerekir öncesinde adıyla *.\\*  komut dosyası yerel dizinde bulunur belirtmek için. 

### <a name="example"></a>Örnek
Aşağıdaki örnekte, üç parametre, karmaşık bir nesne, bir tamsayı ve bir Boole değeri kabul eden bir test alt runbook'u çağırır. Alt runbook'un çıkışı bir değişkene atanır.  Bu durumda, bir PowerShell iş akışı runbook alt runbook.

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true

Aşağıda bir PowerShell runbook alt öğesi olarak kullanarak aynı örnek verilmiştir.

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = .\PS-ChildRunbook.ps1 –VM $vm –RepeatCount 2 –Restart $true


## <a name="starting-a-child-runbook-using-cmdlet"></a>Cmdlet'ini kullanarak bir alt runbook'u başlatma
Kullanabileceğiniz [başlangıç AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) açıklandığı gibi bir runbook'u başlatmak için cmdlet [Windows PowerShell ile bir runbook'u başlatmak için](automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell). Bu cmdlet kullanım iki modu mevcuttur.  Alt iş alt runbook için oluşturulan hemen bir modda cmdlet iş kimliğini döndürür.  Belirterek etkinleştirmek diğer bütün modunda **-bekleyin** parametresi, cmdlet alt kadar bekler işi bittiğinde ve alt runbook'tan çıkış döndürür.

Bir alt runbook'tan bir cmdlet'le başlatılan iş, üst runbook'tan ayrı bir iş çalıştırılır. Bu runbook'u satır içi çağırma'den daha fazla iş sonuçlanır ve izlemek daha zor hale getirir. Üst her birinin tamamlanmasını beklemeden birden çok alt runbook zaman uyumsuz olarak başlatabilirsiniz. Alt runbook'ların satır içi olarak çağrıldığı Paralel yürütme türü için üst runbook kullanması gerekir [parallel anahtar kelimesini](automation-powershell-workflow.md#parallel-processing).

Bir cmdlet'le başlatılan bir alt runbook'un parametreleri açıklandığı gibi karma tablosu olarak sağlanır [Runbook parametreleri](automation-starting-a-runbook.md#runbook-parameters). Yalnızca basit veri türleri kullanılabilir. Ardından runbook karmaşık veri türü olan bir parametreye sahipse satır içi çağrılmalıdır.

### <a name="example"></a>Örnek
Aşağıdaki örnek parametrelere sahip bir alt runbook başlatır ve başlangıç AzureRmAutomationRunbook kullanarak tamamlanmasını bekler-parametre bekleyin. Tamamlandığında, çıktısını alt runbook'tan toplanır.

    $params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true} 
    $joboutput = Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-ChildRunbook" -ResourceGroupName "LabRG" –Parameters $params –wait


## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Alt runbook çağırma yöntemlerinin karşılaştırılması
Aşağıdaki tabloda, başka bir runbook'tan bir runbook'u çağırmak için iki yöntem arasındaki farklar özetlenmektedir.

|  | Satır içi | Cmdlet |
|:--- |:--- |:--- |
| İş |Alt runbook'lar üst ile aynı işi çalıştırın. |Alt runbook için ayrı bir iş oluşturulur. |
| Yürütme |Üst runbook alt runbook'un devam etmeden önce tamamlanmasını bekler. |Üst runbook alt runbook başlatıldıktan hemen sonra devam *veya* üst runbook alt iş tamamlamak bekler. |
| Çıktı |Üst runbook doğrudan alt runbook'tan çıkış alabilir. |Üst runbook alt runbook işinden çıkış almak gerekir *veya* üst runbook alt runbook'tan çıkış doğrudan alabilirsiniz. |
| Parametreler |Alt runbook parametre değerlerini ayrı ayrı belirtilir ve herhangi bir veri türü kullanabilirsiniz. |JSON serileştirmesi kullanan nesne veri türlerini ve alt runbook parametreleri tek bir karma tablosunda birleştirilmelidir ve yalnızca basit, içerebilir değerleri dizisi. |
| Otomasyon Hesabı |Üst runbook, yalnızca aynı automation hesabında alt runbook kullanabilirsiniz. |Bir bağlantı varsa, üst runbook alt runbook'tan herhangi bir Otomasyon hesabı aynı Azure aboneliği ve hatta farklı bir abonelik kullanabilirsiniz. |
| Yayımlama |Üst runbook yayımlanmadan önce alt runbook yayımlanmalıdır. |Üst runbook başlatılmadan önce dilediğiniz zaman alt runbook yayımlanmalıdır. |

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Otomasyonu runbook başlatma](automation-starting-a-runbook.md)
* [Runbook çıkışı ve iletileri Azure Automation](automation-runbook-output-and-messages.md)

