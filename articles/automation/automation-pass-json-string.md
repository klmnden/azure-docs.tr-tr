---
title: Azure Otomasyonu runbook’una bir JSON nesnesi geçirme
description: Nasıl bir JSON nesnesi olarak bir runbook için parametreler
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
keywords: PowerShell, runbook, json, azure Otomasyonu
ms.openlocfilehash: 1bdeef02621924bbb7af1e676d2b275229761081
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/11/2018
ms.locfileid: "42061482"
---
# <a name="pass-a-json-object-to-an-azure-automation-runbook"></a>Azure Otomasyonu runbook’una bir JSON nesnesi geçirme

Bir runbook'ta bir JSON dosyası betiğine geçirmek istediğiniz verileri depolamak için yararlı olabilir.
Örneğin, tüm bir runbook'a geçirmek istediğiniz parametreler içeren bir JSON dosyası oluşturabilirsiniz.
Bunu yapmak için JSON bir dizeye Dönüştür ve içeriğini runbook'a geçirmeden önce bu dize bir PowerShell nesnesine dönüştürmek sahip.

Bu örnekte, çağıran bir PowerShell komut dosyası oluşturacağız [Start-AzureRmAutomationRunbook](https://docs.microsoft.com/powershell/module/azurerm.automation/start-azurermautomationrunbook) JSON içeriği runbook'a geçirerek bir PowerShell runbook'u başlatın.
PowerShell runbook parametreleri geçirilen JSON VM için almaya bir Azure VM'yi yeniden başlatır.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Azure aboneliği. Henüz bir aboneliğiniz yoksa [MSDN abone avantajlarınızı etkinleştirebilir](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ya da <a href="/pricing/free-account/" target="_blank">[ücretsiz hesap için kaydolabilirsiniz](https://azure.microsoft.com/free/).
* Runbook’u tutacak ve Azure kaynaklarında kimlik doğrulamasını yapacak bir [Automation hesabı](automation-sec-configure-azure-runas-account.md).  Bu hesabın sanal makineyi başlatma ve durdurma izni olmalıdır.
* Azure sanal makinesi. Bu makineyi durdurup başlatacağımız için makinenin üretime yönelik bir VM olmaması gerekir.
* Azure Powershell, yerel bir makinede yüklü. Bkz: [yüklemek ve Azure Powershell yapılandırma](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.1.0) Azure PowerShell edinme hakkında bilgi için.

## <a name="create-the-json-file"></a>JSON dosyası oluşturun

Bir metin dosyasına şu test yazın ve kaydedileceği `test.json` yerel bilgisayarınızda bir yerde.

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

## <a name="create-the-runbook"></a>Runbook oluşturma

"Test-Json" Azure automation'da adlı yeni bir PowerShell runbook oluşturun.
Yeni bir PowerShell runbook'u oluşturma konusunda bilgi almak için bkz: [ilk PowerShell runbook'um](automation-first-runbook-textual-powershell.md).

JSON verilerini kabul etmek için runbook giriş parametresi olarak bir nesne gerçekleştirmeniz gerekir.

Runbook daha sonra JSON biçiminde tanımlanan özellikleri kullanabilirsiniz.

```powershell
Param(
     [parameter(Mandatory=$true)]
     [object]$json
)

# Connect to Azure account   
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Convert object to actual JSON
$json = $json | ConvertFrom-Json

# Use the values from the JSON object as the parameters for your command
Start-AzureRmVM -Name $json.VMName -ResourceGroupName $json.ResourceGroup
 ```

 Kaydedin ve bu runbook Otomasyon hesabınızda yayımlayın.

## <a name="call-the-runbook-from-powershell"></a>Powershell'den runbook'u çağırma

Artık Azure PowerShell kullanarak yerel makinenizde runbook çağırabilirsiniz.
Aşağıdaki PowerShell komutlarını çalıştırın:

1. Azure'da oturum açma:
   ```powershell
   Connect-AzureRmAccount
   ```
    Azure kimlik bilgilerinizi girmeniz istenir.

   > [!IMPORTANT]
   > **Add-AzureRmAccount** için bir diğer ad sunulmuştur **Connect-AzureRMAccount**. Ne zaman kitaplığınızı arama öğe görmüyorsanız, **Connect-AzureRMAccount**, kullanabileceğiniz **Add-AzureRmAccount**, veya Otomasyon hesabınızda modüllerinizi güncelleştirebilirsiniz.

1. JSON dosyasının içeriği almak ve bir dizeye dönüştürün:
    ```powershell
    $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
    ```
    `JsonPath` JSON dosyasını kaydettiğiniz yoludur.
1. Dize içeriklerini dönüştürme `$json` bir PowerShell nesnesi için:
   ```powershell
   $JsonParams = @{"json"=$json}
   ```
1. Parametre için bir karma tablo oluşturma `Start-AzureRmAutomationRunbook`:
   ```powershell
   $RBParams = @{
        AutomationAccountName = 'AATest'
        ResourceGroupName = 'RGTest'
        Name = 'Test-Json'
        Parameters = $JsonParams
   }
   ```
   Değerini ayarlayarak fark `Parameters` JSON dosyasından değerleri içeren bir PowerShell nesnesine. 
1. Runbook’u başlatma
   ```powershell
   $job = Start-AzureRmAutomationRunbook @RBParams
   ```

Runbook VM başlatmak için JSON dosyasından değerleri kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* PowerShell ve PowerShell iş akışı runbook'ları ile bir metin düzenleyicisini düzenleme hakkında daha fazla bilgi edinmek için [Azure Otomasyonu, metinsel runbook'ları düzenleme](automation-edit-textual-runbook.md) 
* Daha fazla hakkında oluşturma ve runbook'ları içeri aktarma bilgi edinmek için [oluşturma veya Azure automation'da bir runbook içeri aktarma](automation-creating-importing-runbook.md)


