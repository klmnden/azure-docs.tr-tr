---
title: Azure Automation modülleri yönetme
description: Bu makalede, Azure Automation modülleri yönetme işlemi açıklanır
services: automation
ms.service: automation
author: bobbytreed
ms.author: robreed
ms.date: 06/05/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: cd085164fc9804e0c1c822df1c72d3ef94093a07
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67672798"
---
# <a name="manage-modules-in-azure-automation"></a>Azure Automation modülleri yönetme

Azure Otomasyonu, Otomasyon hesabınızda PowerShell tabanlı runbook'ları tarafından kullanılmak üzere PowerShell modüllerini içeri aktarma olanağı sağlar. Bu modüller, özel modülleri PowerShell Galerisi'nden oluşturduğunuz veya Azure AzureRM ve Az modülleri olabilir. Bazı modüller, bir Otomasyon hesabı oluşturduğunuzda varsayılan olarak içeri aktarılır.

## <a name="import-modules"></a>Modülleri içeri aktarma

Otomasyon hesabınızda bir modülü içeri aktarabilirsiniz birden çok yolu vardır. Aşağıdaki bölümlerde, bir modülü içeri aktarmak için farklı yollar gösterilmiştir.

> [!NOTE]
> Bir dosyanın bir modül Azure Automation'da kullanılacak en fazla yol 140 karakterdir. 140 karakterden herhangi bir yolu PowerShell oturumuna içeri aktarılacak mümkün olmayacaktır `Import-Module`.

### <a name="powershell"></a>PowerShell

Kullanabileceğiniz [New-Azurermautomationmodule'a](/powershell/module/azurerm.automation/new-azurermautomationmodule) Otomasyon hesabınızda bir modülü içeri aktarmak için. Cmdlet'i bir url bir modül zip paketini alır.

```azurepowershell-interactive
New-AzureRmAutomationModule -Name <ModuleName> -ContentLinkUri <ModuleUri> -ResourceGroupName <ResourceGroupName> -AutomationAccountName <AutomationAccountName>
```

### <a name="azure-portal"></a>Azure portal

Azure portalında, Otomasyon hesabınıza gidin ve seçin **modülleri** altında **paylaşılan kaynakları**. Tıklayın **+ Modül Ekle**. Seçin bir **.zip** tıklatın ve modülü içeren dosya **Tamam** alma işlemi başlatmak için.

### <a name="powershell-gallery"></a>PowerShell Galerisi

PowerShell Galerisi modülleri öğesinden alınan olabilir [PowerShell Galerisi](https://www.powershellgallery.com) doğrudan ya da Otomasyon hesabınızdan.

PowerShell Galerisi'nden bir modülü içeri aktarmak için şu adrese gidin https://www.powershellgallery.com ve içeri aktarmak istediğiniz modülü arayın. Tıklayın **Azure Otomasyonu Dağıt** üzerinde **Azure Otomasyonu** sekmesinde altında **yükleme seçenekleri**. Bu eylem, Azure portalını açar. Üzerinde **alma** sayfasında Otomasyon hesabınızı seçin ve tıklayın **Tamam**.

![PowerShell Galerisi modülünü İçeri Aktar](../media/modules/powershell-gallery.png)

Ayrıca, modülleri PowerShell Galerisi'nden Otomasyon hesabınızdan doğrudan aktarabilirsiniz. Otomasyon hesabınızı seçin **modülleri** altında **paylaşılan kaynakları**. Modüller sayfasında tıklayın **Galeriye Gözat**. Bu açılır **Galeriye Gözat** sayfası. Bir modül için PowerShell Galerisi aramak için bu sayfayı kullanabilirsiniz. Tıklatıp içeri aktarmak istediğiniz modülü seçin **alma**. Üzerinde **alma** sayfasında **Tamam** içeri aktarma işlemini başlatmak için.

![Azure portalından PowerShell Galerisi içeri aktarma](../media/modules/gallery-azure-portal.png)

## <a name="delete-modules"></a>Modüller Sil

Bir modül ile ilgili sorunlar veya bir modülün önceki bir sürüme geri almak ihtiyacınız varsa, Otomasyon hesabınızdan silebilirsiniz. Orijinal sürümünü silinemiyor [varsayılan modülleri](#default-modules) bir Otomasyon hesabı oluşturduğunuzda aktarılır. Silmek istediğiniz modülü birini daha yeni bir sürümü olup olmadığını [varsayılan modülleri](#default-modules) yüklendiğinde, Otomasyon hesabınıza yüklediğiniz sürüme geri alma. Aksi takdirde, Otomasyon hesabınızdan silmek herhangi bir modül kaldırılır.

### <a name="azure-portal"></a>Azure portal

Azure portalında, Otomasyon hesabınıza gidin ve seçin **modülleri** altında **paylaşılan kaynakları**. Kaldırmak istediğiniz modülü seçin. Üzerinde **Modülü** sayfası, clcick **Sil**. Bu modül ise [varsayılan modülleri](#default-modules) Otomasyon hesabının oluşturulduğu zaman mevcut olan sürümüne geri alınacak.

### <a name="powershell"></a>PowerShell

PowerShell üzerinden modülü kaldırmak için aşağıdaki komutu çalıştırın:

```azurepowershell-interactive
Remove-AzureRmAutomationModule -Name <moduleName> -AutomationAccountName <automationAccountName> -ResourceGroupName <resourceGroupName>
```

## <a name="internal-cmdlets"></a>İç cmdlet'leri

İç cmdlet'leri listesi aşağıdadır `Orchestrator.AssetManagement.Cmdlets` her Otomasyon hesabına içeri aktarılan modül. Bu cmdlet'leri, runbook'ları ve DSC yapılandırmalarında erişebilir ve Otomasyon hesabınızda varlıklarınızı etkileşime olanak sağlar. Ayrıca, iç cmdlet'leri, gizli dizileri almak izin gelen şifrelenmiş **değişken** değerleri **kimlik bilgilerini**ve şifrelenmiş **bağlantı** alanları. Azure PowerShell cmdlet'leri bu gizli dizileri almak mümkün değildir. Bu cmdlet'ler, örtük olarak bunları kullanırken Azure'a bağlanmak gerektirmez. Bu, bir farklı çalıştır Azure'de kimlik doğrulamasını kullanması gereken hesabı gibi bir bağlantı olduğu senaryolar için yararlıdır.

|Ad|Açıklama|
|---|---|
|Get-AutomationCertificate|`Get-AutomationCertificate [-Name] <string> [<CommonParameters>]`|
|Get-AutomationConnection|`Get-AutomationConnection [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]` |
|Get-AutomationPSCredential|`Get-AutomationPSCredential [-Name] <string> [<CommonParameters>]` |
|Get-AutomationVariable|`Get-AutomationVariable [-Name] <string> [-DoNotDecrypt] [<CommonParameters>]`|
|Set-AutomationVariable|`Set-AutomationVariable [-Name] <string> -Value <Object> [<CommonParameters>]` |
|Başlangıç AutomationRunbook|`Start-AutomationRunbook [-Name] <string> [-Parameters <IDictionary>] [-RunOn <string>] [-JobId <guid>] [<CommonParameters>]`|
|Bekleme AutomationJob|`Wait-AutomationJob -Id <guid[]> [-TimeoutInMinutes <int>] [-DelayInSeconds <int>] [-OutputJobsTransitionedToRunning] [<CommonParameters>]`|

## <a name="add-a-connection-type-to-your-module"></a>Modülünüzün için bir bağlantı türü ekleme

Özel bir sağlayabilir [bağlantı türü](../automation-connections.md) modülünüzde için isteğe bağlı bir dosya ekleyerek Otomasyon hesabınızı kullanabilirsiniz. Otomasyon hesabınızda modülün cmdlet'leriyle kullanılacak bir Azure Otomasyonu bağlantı türünü belirten bir meta veri dosyası bu dosyadır. Bunu başarmak için nasıl bir PowerShell modülü yazma bilmeniz gerekir. Modülü yazma hakkında daha fazla bilgi için bkz: [nasıl yazılacağını PowerShell betik modülündeki](/powershell/developer/module/how-to-write-a-powershell-script-module).

![Azure portalında özel bir bağlantı kullanın](../media/modules/connection-create-new.png)

Bir Azure Otomasyonu bağlantı türü eklemek için modülünüzde adlı bir dosya içermelidir `<ModuleName>-Automation.json` bağlantı türü özelliklerini belirtir. Sıkıştırılmış .zip dosyanızın modül klasöründe içinde yerleştirilen bir json dosyası budur. Bu dosya, sistem veya modülü temsil eden hizmete bağlanmak için gerekli olan bağlantı alanlarını içerir. Bu yapılandırma, Azure Automation'da bağlantı türü oluşturma yukarı sona erer. Alan adları ayarlayabilirsiniz bu dosyayı kullanan türleri, ve alanların şifreli veya isteğe bağlı, modülün bağlantı türü olması gerekir. Aşağıdaki örnek, bir kullanıcı adı ve parola özelliği tanımlayan json dosyası biçiminde bir şablondur:

```json
{
   "ConnectionFields": [
   {
      "IsEncrypted":  false,
      "IsOptional":  true,
      "Name":  "Username",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  true,
      "IsOptional":  false,
      "Name":  "Password",
      "TypeName":  "System.String"
   }
   ],
   "ConnectionTypeName":  "MyModuleConnection",
   "IntegrationModuleName":  "MyModule"
}
```

## <a name="module-best-practices"></a>Modül en iyi uygulamalar

PowerShell modülleri, içerdikleri cmdlet'ler runbook'larda ve DSC kaynakları kullanıma DSC yapılandırmaları içinde kullanmak için kullanılabilir hale getirmek için Azure Otomasyonu içine aktarılabilir. Planda, Azure Otomasyonu bu modülleri depolar ve runbook işi ve DSC derleme işi yürütme süresinde, bunları burada runbook'ları çalıştırmak ve DSC yapılandırmaları derleme Azure Otomasyonu korumalı alanlarına yükler. Tüm modüllerdeki DSC kaynakları da otomatik olarak Automation DSC çekme sunucusuna da yerleştirilir. DSC yapılandırmaları geçerli olduğu durumlarda makineler tarafından çekilebilir.

Azure Otomasyonu kullanmak için bir PowerShell modülü yazarken, aşağıdakileri dikkate alın öneririz:

* Modüldeki her cmdlet için bir özet, açıklama ve yardım URI’sı vardır. PowerShell'de, kullanıcının **Get-Help** cmdlet'ini kullanarak cmdlet'ler hakkında yardım almasını sağlayacak bazı bilgiler tanımlayabilirsiniz. Aşağıdaki örnek, bir özeti tanımlayabilir ve Yardım için URI .psm1 modülü dosyasındaki gösterilmektedir:

  ```powershell
  <#
       .SYNOPSIS
        Gets a Contoso User account
  #>
  function Get-ContosoUser {
  [CmdletBinding](DefaultParameterSetName='UseConnectionObject', `
  HelpUri='https://www.contoso.com/docs/information')]
  [OutputType([String])]
  param(
     [Parameter(ParameterSetName='UserAccount', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [string]
     $UserName,

     [Parameter(ParameterSetName='UserAccount', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [string]
     $Password,

     [Parameter(ParameterSetName='ConnectionObject', Mandatory=true)]
     [ValidateNotNullOrEmpty()]
     [Hashtable]
     $Connection
  )

  switch ($PSCmdlet.ParameterSetName) {
     "UserAccount" {
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $UserName, $Password
        Connect-Contoso -Credential $cred
     }
     "ConnectionObject" {
        Connect-Contoso -Connection $Connection
    }
  }
  }
  ```

  Bu bilgiler verildiğinde gösterir kullanarak bu Yardım **Get-Help** PowerShell konsolundaki cmdlet'i. Bu açıklama, ayrıca Azure portalında görüntülenir.

  ![Tümleştirme Modülü Yardımı](../media/modules/module-activity-description.png)

* Modül bir dış hizmete bağlanır, içermesi gereken bir [bağlantı türü](#add-a-connection-type-to-your-module). Modüldeki her cmdlet, parametre olarak bir bağlantı nesnesi (ilgili bağlantı türünün bir örneği) alabilmelidir. Bu, kullanıcıların bir cmdlet'i her çağırdığında bağlantı varlığı parametrelerinin cmdlet'in ilgili parametreleriyle eşlenecek sağlar. Yukarıdaki runbook örneği temel alarak, ContosoConnection adlı bir örnek Contoso bağlantı varlığı Contoso kaynaklarına erişmek ve dış hizmetten veri döndürmek için kullanır.

  Aşağıdaki örnekte, alanları kullanıcı adı ve parola özelliklerine eşleştirilir bir `PSCredential` nesnesi ve ardından cmdlet'e geçirilir.

  ```powershell
  $contosoConnection = Get-AutomationConnection -Name 'ContosoConnection'

  $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $contosoConnection.UserName, $contosoConnection.Password
  Connect-Contoso -Credential $cred
  }
  ```

  Bu davranış yaklaşım daha kolay ve daha iyi bir yolu, doğrudan bağlantı nesnesi cmdlet'e geçiyor:

  ```powershell
  $contosoConnection = Get-AutomationConnection -Name 'ContosoConnection'

  Connect-Contoso -Connection $contosoConnection
  }
  ```

  Bağlantı nesnesini doğrudan parametrelerin bağlantı alanları yerine bir parametre olarak kabul etmelerine izin vererek, önceki örnekte olduğu gibi davranış cmdlet'lerinizi için etkinleştirebilirsiniz. Azure Automation kullanmayan kullanıcılar cmdlet'lerinizi bağlantı nesnesi gibi davranacak bir Hashtable yapılandırmadan çağırabilir, genellikle bir parametre kümesi her biri için kullanmanız gerekir. Parametre kümesi `UserAccount`, bağlantı alanı özelliklerini geçirmek için kullanılır. `ConnectionObject` bağlantı üzerinden düz geçirmenize olanak tanır.

* Modüldeki tüm cmdlet'ler için çıktı türünü tanımlayın. Cmdlet için bir çıktı türünün tanımlanması tasarım zamanında IntelliSense’in, cmdlet’in yazma sırasında kullanılan çıktı özelliklerini belirlemenize yardımcı olmasını sağlar. Otomasyon runbook grafik, tasarım zamanı bilgisinin bir kolayca kullanıcı deneyimi modülünüzde anahtarına olduğu yazma sırasında bu özellikle yararlı olur.

  Bu ekleyerek gerçekleştirilebilir `[OutputType([<MyOutputType>])]` MyOutputType geçerli bir tür olduğu. OutputType hakkında daha fazla bilgi için bkz: [hakkında işlevleri OutputTypeAttribute](/powershell/module/microsoft.powershell.core/about/about_functions_outputtypeattribute). Aşağıdaki kodu ekleyerek, bir örnektir `OutputType` bir cmdlet için:

  ```powershell
  function Get-ContosoUser {
  [OutputType([String])]
  param(
     [string]
     $Parameter1
  )
  # <script location here>
  }
  ```

  ![Grafik Runbook’u Çıktı Türü](../media/modules/runbook-graphical-module-output-type.png)

  Bu davranış çalıştırmak zorunda kalmadan PowerShell ıse'de cmdlet çıktısının "İleri tür" işlevine benzer.

  ![POSH IntelliSense](../media/modules/automation-posh-ise-intellisense.png)

* Modüldeki tüm cmdlet’leri durum bilgisiz hale getirin. Birden çok runbook işi aynı anda aynı AppDomain ve aynı işlemi ve korumalı alan içinde çalıştırabilirsiniz. Bu düzeyde paylaşılan herhangi bir durum varsa, her diğer işleri etkileyebilir. Bu davranış, aralıklı olarak ortaya çıkan ve sorunların tanılanması faydalanılan sabit neden olabilir.  İşte yapılmaması gereken bir örnek.

  ```powershell
  $globalNum = 0
  function Set-GlobalNum {
     param(
         [int] $num
     )

     $globalNum = $num
  }
  function Get-GlobalNumTimesTwo {
     $output = $globalNum * 2

     $output
  }
  ```

* Modül tamamen xcopy'ye bir pakette yer almalıdır. Runbook'ların yürütülmesi gerektiğinde azure Automation modülleri Automation korumalı alanlarına dağıtılır. Modüller ister çalışıyor üzerinde konak bağımsız olarak çalışması gerekir. Zip erişebiliyor olmalısınız ve modül paket taşıyın ve sahip başka bir ana bilgisayarın PowerShell ortamına içe aktarıldığında da normal çalışmasını işlev. Gerçekleşmesi için sırayla modülün modül klasörü dışında herhangi bir dosya bağlıdır olmamalıdır. Bu klasör modülü Azure Automation'a içeri aktarıldığında ayarlananlar klasördür. Bir ürün yüklü olduğunda, bu ayarları kümesi gibi modülü de herhangi bir konağa benzersiz kayıt defteri ayarlarını bağlı olmamalıdır. Modüldeki tüm dosyaları bir yol 140'den az karakter içermelidir. Tüm yollar 140 karakterden, runbook içeri aktarma sorunlarına neden olur. Bu en iyi yönteme uyulmazsa modül Azure Automation'da kullanılamaz.  

* Başvuru değilse [Az Azure Powershell modülleri](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) de başvuru olmayan, modülünüzde olun `AzureRM`. `Az` Modülü ile birlikte kullanılamaz `AzureRM` modüller. `Az` runbook'larında desteklenir, ancak varsayılan olarak içe aktarılmaz. Hakkında bilgi edinmek için `Az` modülleri ve hesaba katmanız gereken noktalar [Az Modül desteği, Azure automation'da](../az-modules.md).

## <a name="default-modules"></a>Varsayılan modülleri

Aşağıdaki tablo, bir Otomasyon hesabı oluşturduğunuzda varsayılan olarak aktarılan modülleri listeler. Aşağıda listelenen modülleri içeri bunların yeni sürümlerini olabilir, ancak bunları daha yeni bir sürümü silseniz bile özgün sürümle Otomasyon hesabınızdan kaldırılamıyor.

|Modül adı|Version|
|---|---|
| AuditPolicyDsc | 1.1.0.0 |
| Azure | 1.0.3 |
| Azure Depolama | 1.0.3 |
| AzureRM.Automation | 1.0.3 |
| AzureRM.Compute | 1.2.1 |
| AzureRM.Profile | 1.0.3 |
| AzureRM.Resources | 1.0.3 |
| AzureRM.Sql | 1.0.3 |
| AzureRM.Storage | 1.0.3 |
| ComputerManagementDsc | 5.0.0.0 |
| GPRegistryPolicyParser | 0.2 |
| Microsoft.PowerShell.Core | 0 |
| Microsoft.PowerShell.Diagnostics |  |
| Microsoft.PowerShell.Management |  |
| Microsoft.PowerShell.Security |  |
| Microsoft.PowerShell.Utility |  |
| Microsoft.WSMan.Management |  |
| Orchestrator.AssetManagement.Cmdlets | 1\. |
| PSDscResources | 2.9.0.0 |
| SecurityPolicyDsc | 2.1.0.0 |
| StateConfigCompositeResources | 1\. |
| xDSCDomainjoin | 1.1 |
| xPowerShellExecutionPolicy | 1.1.0.0 |
| xRemoteDesktopAdmin | 1.1.0.0 |

## <a name="next-steps"></a>Sonraki adımlar

* PowerShell Modülleri oluşturma hakkında daha fazla bilgi için bkz. [Windows PowerShell Modülü Yazma](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)