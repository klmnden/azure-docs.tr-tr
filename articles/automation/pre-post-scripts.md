---
title: Yapılandırma güncelleştirme yönetimi dağıtımınız azure'da öncesi ve sonrası komut dosyaları
description: Bu makalede, öncesi yapılandırılıp yönetileceği açıklanır ve sonrası betikler için güncelleştirme dağıtımları
services: automation
ms.service: automation
ms.subservice: update-management
author: bobbytreed
ms.author: robreed
ms.date: 05/17/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 94ec7c54e8e49685ad0289102f092516bcb0acfc
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478251"
---
# <a name="manage-pre-and-post-scripts"></a>Yönetme öncesi ve sonrası betikleri

Otomasyon hesabınızda (öncesi görev) önce PowerShell runbook'ları çalıştırmak öncesi ve sonrası komut dosyaları sağlar ve sonra (sonrası görev) bir güncelleştirme dağıtımı. Öncesi ve sonrası komut dosyalarını Azure içerik ve yerel olarak çalıştırın. Güncelleştirme dağıtımının başında ön betiklerini çalıştırın. Sonunda dağıtımın ve sonrasında, yapılandırılmış herhangi bir yeniden başlatma sonrası betiklerini çalıştırın.

## <a name="runbook-requirements"></a>Runbook gereksinimleri

Önceki veya sonraki bir betik olarak kullanılacak runbook için runbook Otomasyon hesabınıza içe aktarılmaz ve yayımlanan gerekir. Bu işlem hakkında daha fazla bilgi için bkz. [runbook yayımlama](manage-runbooks.md#publish-a-runbook).

## <a name="using-a-prepost-script"></a>Ön/son betik kullanarak

Değiştirmek için bir öncesi kullanın ve post bir güncelleştirme dağıtımına komut dosyası, güncelleştirme dağıtımı oluşturarak başlayın. Seçin **ön betiklerini + sonrası betikler**. Bu eylem açar **seçin Ön betiklerini + sonrası betikler** sayfası.  

![Komut dosyaları seçin](./media/pre-post-scripts/select-scripts.png)

Bu örnekte, kullanmak istediğiniz betiği seçin, kullandığınız **UpdateManagement TurnOnVms** runbook. Runbook seçtiğinizde **yapılandırma betiği** sayfasında açılır **ön betik**. Tıklayın **Tamam** işiniz bittiğinde.

Bu işlem için yineleme **UpdateManagement TurnOffVms** betiği. Ancak seçerken **betik türü**, seçin **sonrası betik**.

**Seçili öğeleri** bölümü seçilen iki komut gösterdiğini ve öncesi betiği bulunur ve diğer sonrası betiği.

![Seçilen öğeler](./media/pre-post-scripts/selected-items.png)

Güncelleştirme dağıtımınızı yapılandırma tamamlayın.

Güncelleştirme dağıtımınıza tamamlandığında gidebilirsiniz **güncelleştirme dağıtımları** sonuçlarını görüntülemek için. Gördüğünüz gibi betik öncesi ve betik sonrası durumunu sağlanır.

![Güncelleştirme sonuçları](./media/pre-post-scripts/update-results.png)

Güncelleştirme dağıtımını çalıştırmak tıklayarak öncesi ve sonrası betikler için ek ayrıntılar sunulur. Çalışma zamanında betik kaynağı için bir bağlantı sağlanır.

![Dağıtım sonuçları çalıştırma](./media/pre-post-scripts/deployment-run.png)

## <a name="passing-parameters"></a>Parametreleri geçirme

Ne zaman önceden yapılandırdığınız ve sonrası betikler, runbook zamanlama gibi parametreleri geçirebilirsiniz. Parametreler, güncelleştirme dağıtımı oluşturma zamanında tanımlanır. Öncesi ve sonrası betikler aşağıdaki türlerini destekler:

* [char]
* [bayt]
* [int]
* [uzun]
* [ondalık]
* [tek]
* [çift]
* [DateTime]
* [string]

Başka bir nesne türü gerekiyorsa, bunu başka bir runbook'ta kendi mantığınız ile türüne atayabilirsiniz.

Standart runbook parametrelerini ek olarak, ek bir parametre olarak sağlanır. Bu parametre **SoftwareUpdateConfigurationRunContext**. Bu bir JSON dizesi parametresidir ve parametresi önceki veya sonraki betiğinizde tanımlarsanız, bu otomatik olarak güncelleştirme dağıtımından geçirilir. Parametre bir alt güncelleştirme dağıtımı hakkında bilgi içeren tarafından döndürülen bilgilerin [SoftwareUpdateconfigurations API](/rest/api/automation/softwareupdateconfigurations/getbyname#updateconfiguration) aşağıdaki tabloda değişkeninde sağlanan özelliklerini gösterir:

## <a name="stopping-a-deployment"></a>Bir dağıtım durduruluyor

Bir dağıtım öncesi komut dosyasına dayalı durdurmak istiyorsanız, şunları yapmalısınız [throw](automation-runbook-execution.md#throw) bir özel durum. Bir özel durum yoksa, dağıtım ve son betik çalışmaya devam edecektir. [Örnek runbook](https://gallery.technet.microsoft.com/Update-Management-Run-6949cc44?redir=0) galeride Bunu yapmak nasıl gösterir. Bu runbook'tan bir parçacığı aşağıda verilmiştir.

```powershell
#In this case, we want to terminate the patch job if any run fails.
#This logic might not hold for all cases - you might want to allow success as long as at least 1 run succeeds
foreach($summary in $finalStatus)
{
    if ($summary.Type -eq "Error")
    {
        #We must throw in order to fail the patch deployment.  
        throw $summary.Summary
    }
}
```

### <a name="softwareupdateconfigurationruncontext-properties"></a>SoftwareUpdateConfigurationRunContext properties

|Özellik  |Açıklama  |
|---------|---------|
|SoftwareUpdateConfigurationName     | Yazılım güncelleştirme Yapılandırması adı        |
|SoftwareUpdateConfigurationRunId     | Çalıştırma için benzersiz kimliği.        |
|SoftwareUpdateConfigurationSettings     | Yazılım güncelleştirme yapılandırması ile ilgili özellikler koleksiyonu         |
|SoftwareUpdateConfigurationSettings.operatingSystem     | Güncelleştirme dağıtımı için hedeflenen işletim sistemleri         |
|SoftwareUpdateConfigurationSettings.duration     | Güncelleştirme dağıtımının en uzun süresi Çalıştır `PT[n]H[n]M[n]S` ISO8601 göre de denir "bakım penceresi"          |
|SoftwareUpdateConfigurationSettings.Windows     | Windows bilgisayarlar için ilgili özellikler koleksiyonu         |
|SoftwareUpdateConfigurationSettings.Windows.excludedKbNumbers     | Güncelleştirme dağıtımı'ndan hariç tutulan KB'leri listesi        |
|SoftwareUpdateConfigurationSettings.Windows.includedUpdateClassifications     | Güncelleştirme dağıtım için seçtiğiniz güncelleştirme sınıflandırmaları        |
|SoftwareUpdateConfigurationSettings.Windows.rebootSetting     | Ayarları güncelleştirme dağıtımı için yeniden başlatma        |
|azureVirtualMachines     | Güncelleştirme dağıtımındaki Azure Vm'leri için Resourceıds listesi        |
|nonAzureComputerNames|Güncelleştirme dağıtımındaki FQDN'leri Azure dışı bilgisayarlar listesi|

Aşağıdaki örnek, bir JSON dizesi için geçirilen **SoftwareUpdateConfigurationRunContext** parametresi:

```json
"SoftwareUpdateConfigurationRunContext":{
      "SoftwareUpdateConfigurationName":"sampleConfiguration",
      "SoftwareUpdateConfigurationRunId":"00000000-0000-0000-0000-000000000000",
      "SoftwareUpdateConfigurationSettings":{
         "operatingSystem":"Windows",
         "duration":"PT2H0M",
         "windows":{
            "excludedKbNumbers":[
               "168934",
               "168973"
            ],
            "includedUpdateClassifications":"Critical",
            "rebootSetting":"IfRequired"
         },
         "azureVirtualMachines":[
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-01",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-02",
            "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresources/providers/Microsoft.Compute/virtualMachines/vm-03"
         ], 
         "nonAzureComputerNames":[
            "box1.contoso.com",
            "box2.contoso.com"
         ]
      }
   }
```

Tüm özellikleri ile tam bir örnek bulabilirsiniz: [Yazılım güncelleştirme yapılandırmaları - ada göre alma](/rest/api/automation/softwareupdateconfigurations/getbyname#examples)

> [!NOTE]
> `SoftwareUpdateConfigurationRunContext` Nesne makineler için yinelenen girdiler içerebilir. Bu, birden çok kez aynı makinede çalıştırılacak öncesi ve sonrası betikler neden olabilir. Geçici çözüm için bu davranış, kullanım `Sort-Object -Unique` betiğinizde yalnızca benzersiz bir sanal makine adı seçin.

## <a name="samples"></a>Örnekler

Öncesi ve sonrası betikleri bulunabilir için örnekleri [Komut Merkezi Galerisi](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&f%5B0%5D.Value=WindowsAzure&f%5B0%5D.Text=Windows%20Azure&f%5B1%5D.Type=SubCategory&f%5B1%5D.Value=WindowsAzure_automation&f%5B1%5D.Text=Automation&f%5B2%5D.Type=SearchText&f%5B2%5D.Value=update%20management&f%5B3%5D.Type=Tag&f%5B3%5D.Value=Patching&f%5B3%5D.Text=Patching&f%5B4%5D.Type=ProgrammingLanguage&f%5B4%5D.Value=PowerShell&f%5B4%5D.Text=PowerShell), veya Azure Portalı aracılığıyla içeri aktarılabilir. Otomasyon hesabınızda, portal üzerinden altında almak için **süreç otomasyonu**seçin **Runbook'lar Galerisi**. Kullanım **güncelleştirme yönetimi** Filtresi.

![Galeri listesi](./media/pre-post-scripts/runbook-gallery.png)

Veya, bunlar için komut dosyası adlarına göre aşağıda görüldüğü gibi arayabilirsiniz:

* Güncelleştirme yönetimi - Vm'leri etkinleştirin
* Güncelleştirme yönetimi - sanal makineleri Kapat
* Güncelleştirme yönetimi - betiği yerel olarak çalıştırma
* Güncelleştirme yönetimi - ön/son komut dosyaları için şablon
* Güncelleştirme yönetimi - betiği çalıştırma komutu çalıştırma

> [!IMPORTANT]
> Runbook'ları içeri aktardıktan sonra yapmanız gerekenler **Yayımla** kullanılabilmesi için önce bunları. Yapmak için Otomasyon hesabınızda, select runbook'u bulun **Düzenle**, tıklatıp **Yayımla**.

Örnekleri, aşağıdaki örnekte tanımlanan temel şablondaki tüm temel alır. Bu şablon, öncesi ve sonrası betikleri ile kullanmak için kendi runbook'unuzu oluşturmak için kullanılabilir. Azure ile kimlik doğrulama ve işleme için gerekli mantığı `SoftwareUpdateConfigurationRunContext` parametresi dahil edilir.

```powershell
<# 
.SYNOPSIS 
 Barebones script for Update Management Pre/Post 
 
.DESCRIPTION 
  This script is intended to be run as a part of Update Management Pre/Post scripts.  
  It requires a RunAs account. 
 
.PARAMETER SoftwareUpdateConfigurationRunContext 
  This is a system variable which is automatically passed in by Update Management during a deployment. 
#> 
 
param( 
    [string]$SoftwareUpdateConfigurationRunContext 
) 
#region BoilerplateAuthentication 
#This requires a RunAs account 
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection' 
 
Add-AzureRmAccount ` 
    -ServicePrincipal ` 
    -TenantId $ServicePrincipalConnection.TenantId ` 
    -ApplicationId $ServicePrincipalConnection.ApplicationId ` 
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint 
 
$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID 
#endregion BoilerplateAuthentication 
 
#If you wish to use the run context, it must be converted from JSON 
$context = ConvertFrom-Json  $SoftwareUpdateConfigurationRunContext 
#Access the properties of the SoftwareUpdateConfigurationRunContext 
$vmIds = $context.SoftwareUpdateConfigurationSettings.AzureVirtualMachines | Sort-Object -Unique
$runId = $context.SoftwareUpdateConfigurationRunId 
 
Write-Output $context 
 
#Example: How to create and write to a variable using the pre-script: 
<# 
#Create variable named after this run so it can be retrieved 
New-AzureRmAutomationVariable -ResourceGroupName $ResourceGroup –AutomationAccountName $AutomationAccount –Name $runId -Value "" –Encrypted $false 
#Set value of variable  
Set-AutomationVariable –Name $runId -Value $vmIds 
#> 
 
#Example: How to retrieve information from a variable set during the pre-script 
<# 
$variable = Get-AutomationVariable -Name $runId 
#>      
```

## <a name="interacting-with-machines"></a>Makineleri ile etkileşim kurma

Bir runbook Otomasyon hesabınızdaki ve doğrudan dağıtımınızdaki makinelere öncesi ve sonrası görevleri Çalıştır. Öncesi ve sonrası görevler, ayrıca Azure ait içerikte çalıştırmasına ve Azure olmayan makineler erişiminiz yok. Aşağıdaki bölümlerde bir Azure sanal makinesi veya Azure olmayan makine oldukları nasıl makinelerle doğrudan etkileşim kurabileceğine gösterilmektedir:

### <a name="interacting-with-azure-machines"></a>Azure makineleri ile etkileşim kurma

Öncesi ve sonrası görevleri runbook'lar olarak çalışması ve dağıtımınızda Azure Vm'leriniz üzerinde yerel olarak çalışmaz. Azure Vm'leriniz ile etkileşimde bulunmak üzere, aşağıdaki öğelere sahip olmanız gerekir:

* Bir farklı çalıştır hesabı
* Çalıştırmak istediğiniz runbook'u

Azure makineleri ile etkileşim kurmak için kullanmalısınız [Invoke-AzureRmVMRunCommand](/powershell/module/azurerm.compute/invoke-azurermvmruncommand) Azure Vm'leriniz ile etkileşim kurmak için cmdlet'i. Bunu yapmak nasıl bir örnek için runbook bkz [güncelleştirme yönetimi - betiğini Çalıştır komutu Çalıştır ile](https://gallery.technet.microsoft.com/Update-Management-Run-40f470dc).

### <a name="interacting-with-non-azure-machines"></a>Azure olmayan makineler ile etkileşim kurma

Öncesi ve sonrası görevleri Azure bağlamında çalışır ve Azure olmayan makineler erişiminiz yok. Azure olmayan makineler ile etkileşimde bulunmak üzere, aşağıdaki öğelere sahip olmanız gerekir:

* Bir farklı çalıştır hesabı
* Karma Runbook çalışanı makinede yüklü
* Yerel olarak çalıştırmak istediğiniz runbook'u
* Üst runbook

Azure olmayan makineler ile etkileşimde bulunmak üzere üst runbook Azure bağlamında çalıştırılır. Bu runbook'u ile bir alt runbook'u çağırır [Start-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook) cmdlet'i. Belirtmelisiniz `-RunOn` parametresi ve karma Runbook çalışanı üzerinde çalışacak bir betik için adını belirtin. Bunu yapmak nasıl bir örnek için runbook bkz [güncelleştirme yönetimi - betiği yerel olarak çalıştırma](https://gallery.technet.microsoft.com/Update-Management-Run-6949cc44).

## <a name="abort-patch-deployment"></a>Düzeltme eki dağıtımı Durdur

Öncesi betiğinizin hata verirse, dağıtımınızı durdurmak isteyebilirsiniz. Bunu yapmak için [throw](/powershell/module/microsoft.powershell.core/about/about_throw) komut dosyanızda bir hata oluşturur herhangi bir mantık için bir hata.

```powershell
if (<My custom error logic>)
{
    #Throw an error to fail the patch deployment.  
    throw "There was an error, abort deployment"
}
```

## <a name="known-issues"></a>Bilinen sorunlar

* Öncesi ve sonrası betikler kullanırken, parametreleri için bir Boole değeri, nesneler ve diziler geçiremezsiniz. Runbook başarısız olur. Desteklenen türler tam bir listesi için bkz. [parametreleri](#passing-parameters).

## <a name="next-steps"></a>Sonraki adımlar

Windows sanal makineleriniz için güncelleştirmeleri yönetme konusunda bilgi almak için öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Azure Windows Vm'leriniz için güncelleştirme ve yamaları yönetmenize](automation-tutorial-update-management.md)

