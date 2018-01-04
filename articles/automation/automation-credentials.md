---
title: "Kimlik bilgisi varlıkları Azure Automation | Microsoft Docs"
description: "Azure Otomasyonu kimlik bilgisi varlıkları runbook veya DSC yapılandırması tarafından erişilen kaynaklar için kimlik doğrulaması için kullanılan güvenlik kimlik bilgilerini içerir. Bu makalede kimlik bilgisi varlıkları oluşturma ve bir runbook veya DSC yapılandırması kullanın."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: 3209bf73-c208-425e-82b6-df49860546dd
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/14/2017
ms.author: bwren
ms.openlocfilehash: 516f0ddcc50b3e6d744f70063b2112090d2e411d
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="credential-assets-in-azure-automation"></a>Azure Otomasyonu kimlik bilgisi varlıkları
Bir Otomasyon kimlik bilgisi varlığı tutan bir [PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential) bir kullanıcı adı ve parola gibi güvenlik kimlik bilgileri içeren bir nesne. Kullanıcı adı ve parola için bazı uygulama veya hizmet kimlik doğrulaması gerektiren sağlamak için PSCredential nesnesinin ayıklamak veya runbook'ları ve DSC yapılandırmaları kimlik doğrulaması için bir PSCredential nesnesi kabul cmdlet'leri kullanabilir. Kimlik bilgileri özellikleri Azure Otomasyonu'nda güvenli bir şekilde depolanır ve runbook veya DSC yapılandırması ile erişilen [Get-AutomationPSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential.aspx) etkinlik.

> [!NOTE]
> Azure Automation güvenli varlıkların kimlik bilgileri, sertifikalar, bağlantıları ve şifrelenmiş değişkenler içerir. Bu varlıklar şifrelenir ve her Otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanarak Azure Automation depolanır. Bu anahtarı bir sertifika tarafından şifrelenir ve Azure Otomasyonu'nda depolanır. Güvenli bir varlık depolamak önce anahtar Otomasyon hesabı için sertifika aracılığıyla çözülür ve varlık şifrelemek için kullanılan.  

## <a name="azure-classic-powershell-cmdlets"></a>Azure Klasik PowerShell cmdlet'leri
Aşağıdaki tabloda yer alan cmdlet'ler oluşturmak ve Windows PowerShell ile automation kimlik bilgisi varlıkları yönetmek için kullanılır.  Bir parçası olarak sevk [Azure PowerShell Modülü](/powershell/azure/overview) olduğu Automation runbook'ları ve DSC yapılandırmaları için kullanılabilir.

| Cmdlet'leri | Açıklama |
|:--- |:--- |
| [Get-AzureAutomationCredential](/powershell/module/azure/get-azureautomationcredential?view=azuresmps-3.7.0) |Bir kimlik bilgisi varlığı ilgili bilgileri alır. Kimlik yalnızca alabilir gelen **Get-AutomationPSCredential** etkinlik. |
| [AzureAutomationCredential yeni](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Yeni bir Otomasyon kimlik bilgisi oluşturur. |
| [Remove - AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Otomasyon kimlik bilgileri kaldırır. |
| [Set - AzureAutomationCredential](/powershell/module/azure/new-azureautomationcredential?view=azuresmps-3.7.0) |Var olan Otomasyon kimlik bilgileri özelliklerini ayarlar. |

## <a name="azurerm-powershell-cmdlets"></a>AzureRM PowerShell cmdlet'leri
AzureRM için aşağıdaki tablodaki cmdlet'ler oluşturmak ve Windows PowerShell ile automation kimlik bilgisi varlıkları yönetmek için kullanılır.  Bir parçası olarak sevk [AzureRM.Automation Modülü](/powershell/azure/overview) olduğu Automation runbook'ları ve DSC yapılandırmaları için kullanılabilir.

| Cmdlet'leri | Açıklama |
|:--- |:--- |
| [Get-AzureRmAutomationCredential](/powershell/module/azurerm.automation/get-azurermautomationcredential?view=azurermps-4.4.0) |Bir kimlik bilgisi varlığı ilgili bilgileri alır.  |
| [AzureRmAutomationCredential yeni](/powershell/module/azurerm.automation/new-azurermautomationcredential?view=azurermps-4.4.0) |Yeni bir Otomasyon kimlik bilgisi oluşturur. |
| [Remove-AzureRmAutomationCredential](/powershell/module/azurerm.automation/remove-azurermautomationcredential?view=azurermps-4.4.0) |Otomasyon kimlik bilgileri kaldırır. |
| [Set-AzureRmAutomationCredential](/powershell/module/azurerm.automation/set-azurermautomationcredential?view=azurermps-4.4.0) |Var olan Otomasyon kimlik bilgileri özelliklerini ayarlar. |

## <a name="runbook-activities"></a>Runbook etkinlikleri
Aşağıdaki tablodaki etkinlikler bir runbook ve DSC yapılandırmaları kimlik bilgilerini erişmek için kullanılır.

| Etkinlikler | Açıklama |
|:--- |:--- |
| Get-AutomationPSCredential |Bir runbook veya DSC yapılandırması kullanmak için bir kimlik bilgisi alır. Döndürür bir [System.Management.Automation.PSCredential](http://msdn.microsoft.com/library/system.management.automation.pscredential) nesnesi. |

> [!NOTE]
> Yapmaktan kaçınmalısınız – Name parametresinde Get-AutomationPSCredential bu runbook'lar ya da DSC yapılandırmaları arasındaki bağımlılıkları karmaşık hale ve kimlik bilgisi varlıkları tasarım zamanında olduğundan.

## <a name="python2-functions"></a>Python2 işlevleri
Aşağıdaki tabloda işlevi bir Python2 runbook'ta kimlik bilgilerine erişmek için kullanılır.

| İşlev | Açıklama |
|:---|:---|
| automationassets.get_automation_credential | Bir kimlik bilgisi varlığı ilgili bilgileri alır. |

> [!NOTE]
> Varlık işlevleri erişmek için Python runbook'unuz üstünde "automationassets" modülü içeri aktarmalısınız.

## <a name="creating-a-new-credential-asset"></a>Yeni bir kimlik bilgisi varlığı oluşturma

### <a name="to-create-a-new-credential-asset-with-the-azure-portal"></a>Azure portalı ile yeni kimlik bilgileri varlığı oluşturmak için
1. Otomasyon hesabınızdan tıklatın **varlıklar** açmak için bölümü **varlıklar** dikey.
2. Tıklatın **kimlik bilgileri** açmak için bölümü **kimlik bilgileri** dikey.
3. Tıklatın **bir kimlik bilgisi Ekle** dikey pencerenin üstündeki.
4. Formu tamamlayıp tıklatın **oluşturma** yeni kimlik bilgilerini kaydetmek için.

### <a name="to-create-a-new-credential-asset-with-windows-powershell"></a>Windows PowerShell ile yeni kimlik bilgileri varlığı oluşturmak için
Aşağıdaki örnek komutlarda yeni bir Otomasyon kimlik bilgisi oluşturulacağını gösterir. Bir PSCredential nesnesi ilk adı ve parola ile oluşturulur ve kimlik bilgileri varlığı oluşturmak için kullanılır. Alternatif olarak, kullanabileceğinizi **Get-Credential** cmdlet'ini bir adı ve parola yazın istenir.

    $user = "MyDomain\MyUser"
    $pw = ConvertTo-SecureString "PassWord!" -AsPlainText -Force
    $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $user, $pw
    New-AzureAutomationCredential -AutomationAccountName "MyAutomationAccount" -Name "MyCredential" -Value $cred

### <a name="to-create-a-new-credential-asset-with-the-azure-classic-portal"></a>Azure Klasik portalı ile yeni bir kimlik bilgisi varlığı oluşturmak için
1. Otomasyon hesabınızdan tıklatın **varlıklar** pencerenin üstündeki.
2. Pencerenin alt kısmındaki tıklatın **ayar Ekle**.
3. Tıklatın **kimlik bilgisi Ekle**.
4. İçinde **kimlik bilgisi türü** açılan listesinde, select **PowerShell kimlik bilgisi**.
5. Sihirbazı tamamlamak ve yeni kimlik bilgilerini kaydetmek için onay kutusunu işaretleyin.

## <a name="using-a-powershell-credential"></a>PowerShell kimlik bilgisi kullanma
Bir runbook ya da DSC yapılandırması bir kimlik bilgisi varlığı almak **Get-AutomationPSCredential** etkinlik. Bu döndürür bir [PSCredential nesnesinin](http://msdn.microsoft.com/library/system.management.automation.pscredential.aspx) bir etkinlik veya PSCredential parametresi gerektiriyor cmdlet'ini kullanabilirsiniz. Tek tek kullanmak için kimlik bilgisi nesnesinin özellikleri de alabilirsiniz. Nesnesi bir kullanıcı adı ve güvenli parola özelliğine sahiptir veya kullanabilirsiniz **GetNetworkCredential** döndürülecek yöntemi bir [NetworkCredential](http://msdn.microsoft.com/library/system.net.networkcredential.aspx) güvenli olmayan bir sürümünü sağlayacak nesnesi parola.

### <a name="textual-runbook-sample"></a>Metin biçiminde runbook örneği
Aşağıdaki örnek komutlar bir runbook'ta PowerShell kimlik bilgisi kullanmayı gösterir. Bu örnekte, kimlik bilgisi alınır ve kendi kullanıcı adı ve parola değişkenleri atanmış.

    $myCredential = Get-AutomationPSCredential -Name 'MyCredential'
    $userName = $myCredential.UserName
    $securePassword = $myCredential.Password
    $password = $myCredential.GetNetworkCredential().Password


### <a name="graphical-runbook-sample"></a>Grafik runbook örneği
Eklediğiniz bir **Get-AutomationPSCredential** sağ tıklayarak grafik düzenleyicisini Kitaplık bölmesinde kimlik bilgisi ve seçerek bir grafik runbook etkinliği **tuvale Ekle**.

![Kimlik bilgisi tuvale Ekle](media/automation-credentials/credential-add-canvas.png)

Aşağıdaki resimde, grafik bir runbook'ta kimlik bilgilerini kullanarak bir örnek gösterilmektedir.  Bu durumda, onu açıklandığı gibi Azure kaynakları için bir runbook için kimlik doğrulaması sağlamak için kullanılan [Azure AD kullanıcı hesabı kimlik doğrulaması Runbook'larla](automation-create-aduser-account.md).  İlk etkinlik Azure aboneliği erişimi kimlik bilgisi alır.  **Add-AzureAccount** etkinlik sonra bu kimlik bilgisi bundan sonra gelen tüm etkinlikler için kimlik doğrulaması sağlamak için kullanır.  A [ardışık düzen bağlantısına](automation-graphical-authoring-intro.md#links-and-workflow) bu yana işte **Get-AutomationPSCredential** tek bir nesne bekleniyor.  

![Kimlik bilgisi tuvale Ekle](media/automation-credentials/get-credential.png)

## <a name="using-a-powershell-credential-in-dsc"></a>DSC içinde bir PowerShell kimlik bilgisi kullanma
Azure Otomasyonu DSC yapılandırmalarında kullanarak kimlik bilgisi varlıkları başvurabilir sırada **Get-AutomationPSCredential**, kimlik bilgisi varlıkları de geçirilebilir içinde parametreleri aracılığıyla isterseniz. Daha fazla bilgi için bkz: [Azure Otomasyonu DSC yapılandırmalarında derleme](automation-dsc-compile.md#credential-assets).

## <a name="using-credentials-in-python2"></a>İçinde Python2 kimlik bilgilerini kullanarak
Aşağıdaki örnek Python2 runbook'lardaki kimlik bilgilerine erişirken bir örnek gösterilmektedir.

    import automationassets
    from automationassets import AutomationAssetNotFound

    # get a credential
    cred = automationassets.get_automation_credential("credtest")
    print cred["username"]
    print cred["password"]

## <a name="next-steps"></a>Sonraki Adımlar
* Grafik yazma içinde bağlantılar hakkında daha fazla bilgi için bkz: [grafik yazma içindeki bağlantılar](automation-graphical-authoring-intro.md#links-and-workflow)
* Otomasyon farklı kimlik doğrulama yöntemleriyle anlamak için bkz: [Azure Automation güvenliği](automation-security-overview.md)
* Grafik runbook'ları kullanmaya başlamak için bkz. [İlk grafik runbook uygulamam](automation-first-runbook-graphical.md)
* PowerShell iş akışı runbook'larını kullanmaya başlamak için bkz. [İlk PowerShell iş akışı runbook uygulamam](automation-first-runbook-textual.md) 
* Python2 runbook'ları kullanmaya başlamak için bkz: [ilk Python2 runbook Uygulamam](automation-first-runbook-textual-python2.md) 

