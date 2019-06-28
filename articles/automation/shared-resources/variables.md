---
title: Azure Otomasyonu değişken varlıkları
description: Değişken varlıkları, tüm runbook'ları ve Azure Automation DSC yapılandırmaları için kullanılabilir değerlerdir.  Bu makalede, değişkenlerin ve hem metin hem de grafik yazma ile çalışma konusunda ayrıntılar açıklanmaktadır.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 05/14/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 622b4ab41162a7858097f717a103878f05917cd3
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342154"
---
# <a name="variable-assets-in-azure-automation"></a>Azure Otomasyonu değişken varlıkları

Değişken varlıkları, tüm runbook'ları ve Otomasyon hesabı DSC yapılandırmaları için kullanılabilir değerlerdir. Azure portalı, PowerShell runbook'tan veya DSC yapılandırmasından içinde yönetilebilir. Otomasyon değişkenleri aşağıdaki senaryolarda kullanışlıdır:

- Birden çok runbook veya DSC yapılandırmaları arasında bir değer paylaşın.

- Bir değeri aynı runbook'tan veya DSC yapılandırmasından birden çok iş arasında paylaştırma.

- Portaldan veya PowerShell komut satırından runbook'lar ya da bir dizi ortak yapılandırma öğeleri belirli listesi gibi bir sanal makine adı, belirli bir kaynak grubu, bir AD etki alanı adı ve daha fazlası gibi DSC yapılandırmaları tarafından kullanılan bir değeri yönetme.  

Otomasyon değişkenleri kalıcı yaptığınızdan runbook'tan veya DSC yapılandırmasından başarısız olsa bile kullanılabilirler. Bu davranış, aynı runbook veya DSC yapılandırması çalıştırıldığında bir sonraki zamandan sonra bir başkası tarafından kullanılır ve kullanılan bir runbook tarafından ayarlamak için bir değer sağlar.

Bir değişken oluşturulduğunda, depolanan olduğunu belirtebilirsiniz şifrelenmiş. Şifrelenmiş değişkenler Azure Automation'da güvenli bir şekilde depolanır ve değeri alınamaz [Get-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Get-AzureRmAutomationVariable) Azure PowerShell modülünün bir parçası olarak gönderilen cmdlet'i. Şifrelenmiş bir değeri almanın tek yolu **Get-AutomationVariable** runbook'tan veya DSC yapılandırmasından etkinlik. Şifrelenmiş bir değişkene şifrelenmemiş için değiştirmek istiyorsanız, size gereken silebilir ve şifrelenmemiş olarak değişkeni yeniden oluşturun.

>[!NOTE]
>Azure automation'da güvenli varlıkların kimlik bilgileri, sertifikalar, bağlantılar ve şifrelenmiş değişkenler içerir. Bu varlıklar şifrelenir ve her Otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanarak Azure automation'da depolanır. Bu anahtar depolanan bir sistem anahtar kasası yönetilen. Güvenli bir varlık depolamadan önce anahtarı Key Vault'tan yüklenir ve sonra varlık şifrelemek için kullanılır. Bu işlem, Azure Otomasyonu tarafından yönetilir.

## <a name="variable-types"></a>Değişken türleri

Azure portalı ile bir değişkeni oluşturduğunuzda, portal değişken değeri girmek için uygun denetimi görüntüleyebilmesi aşağı açılan listeden bir veri türü belirtmeniz gerekir. Değişkeni, bu veri türü için sınırlı değildir. Farklı türde bir değer belirtmek istiyorsanız Windows PowerShell kullanarak değişkenini ayarlamalıdır. Belirtirseniz **tanımlanmamış**, değişkenin değerini ayarlar **$null**, ve bir değerle ayarlamanız gerekir [Set-AzureRMAutomationVariable](/powershell/module/AzureRM.Automation/Set-AzureRmAutomationVariable) cmdlet'ini veya **Set-AutomationVariable** etkinlik. Oluşturma veya değiştirme Portalı'nda bir karmaşık değişken türü için değer, ancak Windows PowerShell kullanarak herhangi bir türde bir değer sağlayabilirsiniz. Karmaşık türler olarak döndürülür bir [PSCustomObject](/dotnet/api/system.management.automation.pscustomobject).

Bir dizi ya da karma tablosu oluşturuluyor ve değişkenine kaydederek birden çok değeri tek bir değişkende depolayabilirsiniz.

Otomasyon'da kullanılabilir değişken türlerinin bir listesi aşağıda verilmiştir:

* String
* Integer
* DateTime
* Boolean
* Null

## <a name="azurerm-powershell-cmdlets"></a>AzureRM PowerShell cmdlet'leri

AzureRM için aşağıdaki tabloda yer alan cmdlet'ler Windows PowerShell ile Otomasyon kimlik bilgisi varlıkları oluşturmak ve yönetmek için kullanılır. Bunlar parçası olarak gönderilen [AzureRM.Automation Modülü](/powershell/azure/overview), olduğu Automation runbook'ları ve DSC yapılandırmaları için kullanılabilir.

| Cmdlet'ler | Açıklama |
|:---|:---|
|[Get-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Get-AzureRmAutomationVariable)|Mevcut bir değişkenin değerini alır.|
|[New-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/New-AzureRmAutomationVariable)|Yeni bir değişken oluşturur ve değerini ayarlar.|
|[Remove-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Remove-AzureRmAutomationVariable)|Mevcut bir değişken kaldırır.|
|[Set-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Set-AzureRmAutomationVariable)|Mevcut bir değişken için değeri ayarlar.|

## <a name="activities"></a>Etkinlikler

Aşağıdaki tablodaki etkinlikler bir runbook ve DSC yapılandırmaları kimlik bilgilerini erişmek için kullanılır.

| Etkinlikler | Açıklama |
|:---|:---|
|Get-AutomationVariable|Mevcut bir değişkenin değerini alır.|
|Set-AutomationVariable|Mevcut bir değişken için değeri ayarlar.|

> [!NOTE]
> Değişkenleri kullanmaktan kaçınmanız gerekir – Name parametresinde **Get-AutomationVariable** runbook veya runbook veya DSC yapılandırma ve Otomasyon arasındaki bağımlılıkları getirebileceğinden DSC yapılandırması Tasarım zamanında değişkenleri.

İşlevleri aşağıdaki tabloda, erişim ve Python2 runbook değişkenlerinde almak için kullanılır.

|Python2 işlevleri|Açıklama|
|:---|:---|
|automationassets.get_automation_variable|Mevcut bir değişkenin değerini alır. |
|automationassets.set_automation_variable|Mevcut bir değişken için değeri ayarlar. |

> [!NOTE]
> Varlık işlevlerine erişmek için Python runbook'unuzu üst kısmında "automationassets" modülü içeri aktarmalısınız.

## <a name="creating-a-new-automation-variable"></a>Yeni Otomasyon değişkeni oluşturma

### <a name="to-create-a-new-variable-with-the-azure-portal"></a>Azure portalı ile yeni bir değişken oluşturmak için

1. Otomasyon hesabınızdan tıklayın **varlıklar** kutucuğuna ve ardından **varlıklar** dikey penceresinde **değişkenleri**.
2. Üzerinde **değişkenleri** kutucuk seçin **değişken Ekle**.
3. Seçenekleri doldurun **yeni değişken** dikey de **Oluştur** yeni değişkeni kaydetmek.

### <a name="to-create-a-new-variable-with-windows-powershell"></a>Windows PowerShell ile yeni bir değişken oluşturmak için

[New-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/New-AzureRmAutomationVariable) cmdlet'i yeni bir değişken oluşturur ve bunun başlangıçtaki değerini ayarlar. Değerini kullanarak alabilirsiniz [Get-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Get-AzureRmAutomationVariable). Ardından bir basit tür değeri ise bu türdeki döndürülür. Karmaşık bir tür ise bir **PSCustomObject** döndürülür.

Aşağıdaki örnek komutlarda dize türünde bir değişken oluşturun ve ardından değerini döndürmek gösterilmektedir.

```powershell
New-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" 
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
–Encrypted $false –Value 'My String'
$string = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value
```

Aşağıdaki örnek komutlar bir karmaşık türü ile bir değişken oluşturun ve ardından özelliklerini döndürmek gösterilmektedir. Bu durumda, bir sanal makine nesne öğesinden **Get-AzureRmVm** kullanılır.

```powershell
$vm = Get-AzureRmVm -ResourceGroupName "ResourceGroup01" –Name "VM01"
New-AzureRmAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm

$vmValue = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
–AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
$vmName = $vmValue.Name
$vmIpAddress = $vmValue.IpAddress
```

## <a name="using-a-variable-in-a-runbook-or-dsc-configuration"></a>Bir runbook'tan veya DSC yapılandırmasından bir değişkende kullanma

Kullanım **Set-AutomationVariable** bir PowerShell runbook veya DSC yapılandırması, bir Otomasyon değişkenin değerini ayarlamak için etkinlik ve **Get-AutomationVariable** onu almak. Kullanmamalısınız **Set-AzureRMAutomationVariable** veya **Get-AzureRMAutomationVariable** runbook'tan veya DSC yapılandırması iş akışı etkinlikleri az verimli olduğundan cmdlet'leri. Ayrıca güvenli değişkenlerle değeri alınamıyor **Get-AzureRMAutomationVariable**. Bir runbook'tan veya DSC yapılandırmasından içinde yeni bir değişken oluşturmak için tek yolu kullanmaktır [New-AzureRMAutomationVariable](/powershell/module/AzureRM.Automation/New-AzureRmAutomationVariable) cmdlet'i.

### <a name="textual-runbook-samples"></a>Metinsel runbook örnekleri

#### <a name="setting-and-retrieving-a-simple-value-from-a-variable"></a>Bir değişkenden gelen basit bir değer alma ve ayarlama

Aşağıdaki örnek komutlar, ayarlamak ve metinsel bir runbook'taki bir değişkeni almak gösterilmektedir. Bu örnek, Tamsayı türünde değişkenler adlandırdığınız görünür duruma varsayılır *Numberofıterations* ve *NumberOfRunnings* adlı dize türündeki değişkenlerin ve *SampleMessage* sahip oluşturulmuş.

```powershell
$NumberOfIterations = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
$NumberOfRunnings = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
$SampleMessage = Get-AutomationVariable -Name 'SampleMessage'

Write-Output "Runbook has been run $NumberOfRunnings times."

for ($i = 1; $i -le $NumberOfIterations; $i++) {
    Write-Output "$i`: $SampleMessage"
}
Set-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)
```

#### <a name="setting-and-retrieving-a-variable-in-python2"></a>Bir değişkende Python2 alma ve ayarlama

Aşağıdaki örnek kod, bir değişken, bir değişken ayarlamak ve mevcut olmayan değişkende Python2 runbook için bir özel durum işleme gösterilmektedir.

```python
import automationassets
from automationassets import AutomationAssetNotFound

# get a variable
value = automationassets.get_automation_variable("test-variable")
print value

# set a variable (value can be int/bool/string)
automationassets.set_automation_variable("test-variable", True)
automationassets.set_automation_variable("test-variable", 4)
automationassets.set_automation_variable("test-variable", "test-string")

# handle a non-existent variable exception
try:
    value = automationassets.get_automation_variable("non-existing variable")
except AutomationAssetNotFound:
    print "variable not found"
```

### <a name="graphical-runbook-samples"></a>Grafik runbook örnekleri

Bir grafik runbook eklediğiniz **Get-AutomationVariable** veya **Set-AutomationVariable** sağ tıklayarak grafik düzenleyicisini ve etkinlik seçerek Kitaplık bölmesinde değişkeni istersiniz.

![Değişken tuvale Ekle](../media/variables/runbook-variable-add-canvas.png)

#### <a name="setting-values-in-a-variable"></a>Bir değişken değerlerini ayarlama

Aşağıdaki görüntüde basit bir grafik runbook değere sahip bir değişken güncelleştirilecek örnek etkinlikleri gösterir. Bu örnekte **Get-AzureRmVM** alır tek bir Azure sanal makine ve bilgisayar adı, dize türünde bir Otomasyon değişkenine kaydeder. Farketmez olup olmadığını [bağlantıdır bir ardışık düzen veya dizisi](../automation-graphical-authoring-intro.md#links-and-workflow) yalnızca tek bir nesne çıktısında beklediğiniz olduğundan.

![Basit değişken Ayarla](../media/variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>Sonraki Adımlar

- Grafik yazma etkinlikleri birbirine bağlama hakkında daha fazla bilgi için bkz: [grafik yazma içindeki bağlantılar](../automation-graphical-authoring-intro.md#links-and-workflow)
- Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](../automation-first-runbook-graphical.md)
