---
title: "Azure Otomasyonu değişken varlıkları"
description: "Değişken varlıkları tüm runbook'lar ve Azure Otomasyonu DSC yapılandırmalarında kullanılabilen değerlerdir.  Bu makalede, değişkenlerin ve bunlarla metinsel ve grafik yazma çalışma konusunda ayrıntılar açıklanmaktadır."
services: automation
ms.service: automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 7c36fce380712da6572e9512a05af9c23c4152a2
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
---
# <a name="variable-assets-in-azure-automation"></a>Azure Otomasyonu değişken varlıkları

Değişken varlıkları tüm runbook'lar ve Otomasyon hesabınızda DSC yapılandırmaları kullanılabilen değerlerdir. Bunlar oluşturulan, değiştirilebilir ve Windows PowerShell, Azure portalından ve içinden alınan bir runbook veya DSC yapılandırması. Otomasyon değişkenleri aşağıdaki senaryolarda kullanışlıdır:

- Birden çok runbook'ları veya DSC yapılandırması arasında bir değer paylaşır.

- Bir değeri aynı runbook veya DSC yapılandırması birden çok iş arasında paylaştırma.

- Bir değeri portalından veya runbook'lar veya bir dizi ortak yapılandırma öğelerinin belirli listesi gibi VM adları, belirli bir kaynak grubunun, bir AD etki alanı adı, vb. gibi DSC yapılandırmaları tarafından kullanılan Windows PowerShell komut satırından yönetme.  

Böylece runbook veya DSC yapılandırma başarısız olsa bile kullanılabilir olmaya devam Otomasyon değişkenleri kalıcıdır. Bu DSC yapılandırması bu İleri çalıştırıldığında ve aynı runbook tarafından kullanılan veya bir başkası tarafından sonra kullanılan bir runbook tarafından ayarlamak için bir değer de sağlar.

Bir değişken oluşturulduğunda, depolanan olduğunu belirtebilirsiniz şifrelenmiş. Bir değişken şifrelendiğinde, Azure Automation'da güvenli bir şekilde depolanır ve değeri alınamaz [Get-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Get-AzureRmAutomationVariable) Azure PowerShell modülünün bir parçası olarak gönderilen cmdlet'i. Şifrelenmiş değer alınabilir tek yolu **Get-AutomationVariable** etkinliğini runbook veya DSC yapılandırması.

>[!NOTE]
>Azure Automation güvenli varlıkların kimlik bilgileri, sertifikalar, bağlantıları ve şifrelenmiş değişkenler içerir. Bu varlıklar şifrelenir ve her Otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanarak Azure automation'da depolanır. Bu anahtar, anahtar kasasında depolanır. Güvenli bir varlık depolamadan önce anahtarı anahtar Kasası'nı yüklenir ve varlık şifrelemek için kullanılır.

## <a name="variable-types"></a>Değişken türleri

Azure portal ile bir değişkeni oluşturduğunuzda, portal değişken değeri girmek için uygun denetimi görüntüleyebilmesi aşağı açılan listeden bir veri türü belirtmeniz gerekir. Değişkeni bu veri türü için sınırlı değildir, ancak farklı türde bir değer belirtmek istiyorsanız Windows PowerShell kullanarak değişkenini ayarlamalıdır. Belirtirseniz **tanımlanmamış**, değişkenin değerini ayarlamak sonra **$null**, ve değerle ayarlamanız gerekir [kümesi AzureRMAutomationVariable](/powershell/module/AzureRM.Automation/Set-AzureRmAutomationVariable) cmdlet'ini veya **Set-AutomationVariable** etkinlik. Oluşturma veya karmaşık değişken türü portalında değerini değiştirin, ancak Windows PowerShell kullanarak herhangi bir türde bir değer sağlayabilir. Karmaşık türler olarak döndürülür bir [PSCustomObject](/dotnet/api/system.management.automation.pscustomobject).

Bir dizi ya da karma tablosu oluşturarak ve değişkenine kaydetme birden çok değeri tek bir değişkende depolayabilirsiniz.

Değişken türleri Otomasyon kullanılabilir bir listesi verilmiştir:

* Dize
* Tamsayı
* DateTime
* Boole
* Null

## <a name="azurerm-powershell-cmdlets"></a>AzureRM PowerShell cmdlet'leri
AzureRM için aşağıdaki tablodaki cmdlet'ler oluşturmak ve Windows PowerShell ile automation kimlik bilgisi varlıkları yönetmek için kullanılır. Bir parçası olarak sevk [AzureRM.Automation Modülü](/powershell/azure/overview) olduğu Automation runbook'ları ve DSC yapılandırmaları için kullanılabilir.

| Cmdlet'leri | Açıklama |
|:---|:---|
|[Get-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Get-AzureRmAutomationVariable)|Mevcut bir değişken değerini alır.|
|[New-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/New-AzureRmAutomationVariable)|Yeni bir değişken oluşturur ve değerini ayarlar.|
|[Remove-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Remove-AzureRmAutomationVariable)|Mevcut bir değişken kaldırır.|
|[Set-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Set-AzureRmAutomationVariable)|Mevcut bir değişken için değeri ayarlar.|

## <a name="activities"></a>Etkinlikler
Aşağıdaki tablodaki etkinlikler bir runbook ve DSC yapılandırmaları kimlik bilgilerini erişmek için kullanılır.

| Etkinlikler | Açıklama |
|:---|:---|
|Get-AutomationVariable|Mevcut bir değişken değerini alır.|
|Set-AutomationVariable|Mevcut bir değişken için değeri ayarlar.|

> [!NOTE] 
> Yapmaktan kaçınmalısınız – Name parametresinde **Get-AutomationVariable** runbook veya bu runbook'ları veya DSC yapılandırması ve Otomasyon değişkenleri arasındaki bağımlılıkları tasarım zamanında getirebileceğinden DSC yapılandırması.

Aşağıdaki tabloda işlevleri erişmek ve bir Python2 runbook'ta değişkenlere almak için kullanılır. 

|Python2 işlevleri|Açıklama|
|:---|:---|
|automationassets.get_automation_variable|Mevcut bir değişken değerini alır. |
|automationassets.set_automation_variable|Mevcut bir değişken için değeri ayarlar. |

> [!NOTE] 
> Varlık işlevleri erişmek için Python runbook'unuz üstünde "automationassets" modülü içeri aktarmalısınız.

## <a name="creating-a-new-automation-variable"></a>Yeni Otomasyon değişkeni oluşturma

### <a name="to-create-a-new-variable-with-the-azure-portal"></a>Azure portalı ile yeni bir değişken oluşturmak için

1. Otomasyon hesabınızdan tıklatın **varlıklar** döşeme ve daha sonra **varlıklar** dikey penceresinde, select **değişkenleri**.
2. Üzerinde **değişkenleri** kutucuğu, select **değişken Ekle**.
3. Seçenekleri tamamlayın **yeni değişken** tıklayın ve dikey **oluşturma** yeni değişkeni kaydetmek.

### <a name="to-create-a-new-variable-with-windows-powershell"></a>Windows PowerShell ile yeni bir değişken oluşturmak için

[Yeni AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/New-AzureRmAutomationVariable) cmdlet'i yeni bir değişken oluşturur ve ilk değerini ayarlar. Değer kullanarak alabilirsiniz [Get-AzureRmAutomationVariable](/powershell/module/AzureRM.Automation/Get-AzureRmAutomationVariable). Değer basit bir tür ise, bu türdeki döndürülür. Karmaşık bir tür ise sonra bir **PSCustomObject** döndürülür.

Aşağıdaki örnek komutlar dize türünde bir değişken oluşturma ve değerini döndür göstermektedir.

    New-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" 
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable' `
    –Encrypted $false –Value 'My String'
    $string = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name 'MyStringVariable').Value

Aşağıdaki örnek komutlar bir karmaşık türü ile bir değişken oluşturun ve özelliklerini dönmek nasıl gösterir. Bir sanal makine bu durumda, nesne **Get-AzureRmVm** kullanılır.

    $vm = Get-AzureRmVm -ResourceGroupName "ResourceGroup01" –Name "VM01"
    New-AzureRmAutomationVariable –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable" –Encrypted $false –Value $vm
    
    $vmValue = (Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName "MyAutomationAccount" –Name "MyComplexVariable").Value
    $vmName = $vmValue.Name
    $vmIpAddress = $vmValue.IpAddress



## <a name="using-a-variable-in-a-runbook-or-dsc-configuration"></a>Bir runbook veya DSC yapılandırması bir değişken kullanma

Kullanım **Set-AutomationVariable** bir PowerShell runbook veya DSC yapılandırması, bir Otomasyon değişkenin değerini ayarlamak için etkinlik ve **Get-AutomationVariable** bunu almak için. Kullanmamanız **kümesi AzureRMAutomationVariable** veya **Get-AzureRMAutomationVariable** cmdlet'leri runbook veya iş akışı etkinlikleri az verimli olduğundan DSC yapılandırması. Güvenli değişkenlerle değeri alınamıyor **Get-AzureRMAutomationVariable**. Bir runbook veya DSC yapılandırması içinde yeni bir değişken oluşturmak için tek yolu kullanmaktır [yeni AzureRMAutomationVariable](/powershell/module/AzureRM.Automation/New-AzureRmAutomationVariable) cmdlet'i.


### <a name="textual-runbook-samples"></a>Metin biçiminde runbook örnekleri

#### <a name="setting-and-retrieving-a-simple-value-from-a-variable"></a>Basit bir değer bir değişkeninden alma ve ayarlama

Aşağıdaki örnek komutlar ayarlamak ve metin biçiminde runbook bir değişkende almak nasıl gösterir. Bu örnekte, Tamsayı türünde değişkenleri adlı görünür duruma varsayılır *Numberofıterations* ve *NumberOfRunnings* ve adlı dize türünde bir değişken *SampleMessage* zaten oluşturulmuş.

    $NumberOfIterations = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfIterations'
    $NumberOfRunnings = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" -Name 'NumberOfRunnings'
    $SampleMessage = Get-AutomationVariable -Name 'SampleMessage'
    
    Write-Output "Runbook has been run $NumberOfRunnings times."
    
    for ($i = 1; $i -le $NumberOfIterations; $i++) {
       Write-Output "$i`: $SampleMessage"
    }
    Set-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" –AutomationAccountName "MyAutomationAccount" –Name NumberOfRunnings –Value ($NumberOfRunnings += 1)

#### <a name="setting-and-retrieving-a-complex-object-in-a-variable"></a>Karmaşık bir nesne, bir değişkende alma ve ayarlama

Aşağıdaki örnek kod, metin biçiminde runbook karmaşık bir değere sahip bir değişken güncelleştirme gösterilmiştir. Bu örnekte, bir Azure sanal makinesi ile alınan **Get-AzureVM** ve varolan bir Otomasyon değişkeni kaydedildi.  İçinde anlatıldığı gibi [değişken türleri](#variable-types), bu PSCustomObject depolanır.

    $vm = Get-AzureVM -ServiceName "MyVM" -Name "MyVM"
    Set-AutomationVariable -Name "MyComplexVariable" -Value $vm

Aşağıdaki kodda değeri değişkeninden alınır ve sanal makineyi başlatmak için kullanılır.

    $vmObject = Get-AutomationVariable -Name "MyComplexVariable"
    if ($vmObject.PowerState -eq 'Stopped') {
       Start-AzureVM -ServiceName $vmObject.ServiceName -Name $vmObject.Name
    }


#### <a name="setting-and-retrieving-a-collection-in-a-variable"></a>Bir değişken koleksiyonunda alma ve ayarlama

Aşağıdaki örnek kod bir metinsel runbook'ta karmaşık değerler koleksiyonu ile bir değişken kullanmayı gösterir. Bu örnekte, birden fazla Azure sanal makineler ile alınan **Get-AzureVM** ve varolan bir Otomasyon değişkeni kaydedildi. İçinde anlatıldığı gibi [değişken türleri](#variable-types), bu PSCustomObjects koleksiyonu olarak depolanır.

    $vms = Get-AzureVM | Where -FilterScript {$_.Name -match "my"}     
    Set-AutomationVariable -Name 'MyComplexVariable' -Value $vms

Aşağıdaki kodda, koleksiyon değişkeninden alınır ve her sanal makineyi başlatmak için kullanılır.

    $vmValues = Get-AutomationVariable -Name "MyComplexVariable"
    ForEach ($vmValue in $vmValues)
    {
       if ($vmValue.PowerState -eq 'Stopped') {
          Start-AzureVM -ServiceName $vmValue.ServiceName -Name $vmValue.Name
       }
    }
    
#### <a name="setting-and-retrieving-a-variable-in-python2"></a>Bir değişkende Python2 alma ve ayarlama
Aşağıdaki örnek kod, bir değişken kullanmak için bir değişken ayarlamak ve Python2 runbook'taki mevcut olmayan değişken için bir özel durum işleme gösterilmektedir.

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


### <a name="graphical-runbook-samples"></a>Grafik runbook örnekleri

Bir grafik runbook'ta eklediğiniz **Get-AutomationVariable** veya **Set-AutomationVariable** grafik düzenleyicisini Kitaplık bölmesinde değişkeni sağ tıklatın ve istediğiniz etkinliği seçerek.

![Tuvale değişken Ekle](media/automation-variables/runbook-variable-add-canvas.png)

#### <a name="setting-values-in-a-variable"></a>Bir değişkende değerleri ayarlama
Aşağıdaki resimde bir grafik runbook basit bir değere sahip bir değişken güncelleştirmek için örnek etkinliklerini gösterir. Bu örnekte, tek bir Azure sanal makine ile alınan **Get-AzureRmVM** ve varolan bir Otomasyon değişkeni dize türünde bir bilgisayar adı kaydedilir. Önemli değildir olup olmadığını [bağlantıdır bir ardışık düzen veya dizisi](automation-graphical-authoring-intro.md#links-and-workflow) yalnızca tek bir nesne çıktıda beklediğiniz beri.

![Basit değişken Ayarla](media/automation-variables/runbook-set-simple-variable.png)

## <a name="next-steps"></a>Sonraki Adımlar

* Grafik yazma etkinlikleri birbirine bağlama hakkında daha fazla bilgi için bkz: [grafik yazma içindeki bağlantılar](automation-graphical-authoring-intro.md#links-and-workflow)
* Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](automation-first-runbook-graphical.md) 
