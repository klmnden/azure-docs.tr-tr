---
title: Azure automation'da bağlantı varlıkları
description: Bağlantı varlıkları Azure automation'da bir runbook veya DSC yapılandırmasından dış hizmete veya uygulamaya bağlanmak için gereken bilgileri içerir. Bu makalede, bağlantılar ve hem metin hem de grafik yazma ile çalışma ayrıntılarını açıklar.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 01/16/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: e7dccc4a396d4cf8af1062057c4c3ce6efe978ed
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61074273"
---
# <a name="connection-assets-in-azure-automation"></a>Azure automation'da bağlantı varlıkları

Bir Otomasyon bağlantı varlığı, bir runbook'tan veya DSC yapılandırmasından dış hizmete veya uygulamaya bağlanmak için gereken bilgileri içerir. Bu, bir kullanıcı adı ve parola gibi bir URL veya bir bağlantı noktası bağlantı bilgilerine ek olarak gibi kimlik doğrulaması için gerekli bilgileri içerebilir. Bir bağlantı değeri, birden fazla değişken oluşturma aksine bir varlık içinde belirli bir uygulamaya bağlanmak için tüm özellikler engelliyor. Kullanıcı tek bir yerde bağlantı değerlerini düzenleyebilir ve bir bağlantı adı runbook'tan veya DSC yapılandırmasından tek bir parametre olarak geçirebilirsiniz. Bir bağlantı için özellikleri runbook veya DSC yapılandırması ile erişilebilen **Get-AutomationConnection** etkinlik. 

Bir bağlantı oluşturduğunuzda belirtmeniz gerekir bir *bağlantı türü*. Bağlantı bir özellikler kümesini tanımlayan bir şablon türüdür. Bağlantı, bağlantı türü tanımlanmış her bir özellik için değerleri tanımlar. Bağlantı türleri için Azure Automation tümleştirme modülleri eklendi veya oluşturulan [Azure Automation API](/previous-versions/azure/reference/mt163818(v=azure.100)) tümleştirme modülü bağlantı türünü içerir ve Otomasyon hesabınızda içeri aktarılır. Aksi takdirde, bir Otomasyon bağlantı türü belirtmek için bir meta veri dosyası oluşturmanız gerekecektir.  Bu ilişkin daha fazla bilgi için bkz. [tümleştirme modülleri](automation-integration-modules.md).  

>[!NOTE]
>Azure automation'da güvenli varlıkların kimlik bilgileri, sertifikalar, bağlantılar ve şifrelenmiş değişkenler içerir. Bu varlıklar şifrelenir ve her Otomasyon hesabı için oluşturulan benzersiz bir anahtar kullanarak Azure automation'da depolanır. Bu anahtar depolanan bir sistem anahtar kasası yönetilen. Güvenli bir varlık depolamadan önce anahtarı Key Vault'tan yüklenir ve sonra varlık şifrelemek için kullanılır. Bu işlem, Azure Otomasyonu tarafından yönetilir.

## <a name="connection-types"></a>Bağlantı türleri

Azure Otomasyonu'nda yerleşik bağlantıları üç tür vardır:

* **Azure** -Bu bağlantı, Klasik kaynakları yönetmek için kullanılabilir.
* **AzureClassicCertificate** -Bu bağlantı tarafından kullanılan **AzureClassicRunAs** hesabı.
* **AzureServicePrincipal** -Bu bağlantı tarafından kullanılan **AzureRunAs** hesabı.

Çoğu durumda, oluşturduğunuzda oluşturulan bağlantı kaynağı oluşturma gerekmez bir [RunAs hesabı](manage-runas-account.md).

## <a name="windows-powershell-cmdlets"></a>Windows PowerShell Cmdlet'leri

Aşağıdaki tabloda yer alan cmdlet'ler Windows PowerShell ile Otomasyon bağlantıları oluşturma ve yönetme için kullanılır. Bunlar parçası olarak gönderilen [Azure PowerShell Modülü](/powershell/azure/overview) olduğu Automation runbook'ları ve DSC yapılandırmaları için kullanılabilir.

|Cmdlet|Açıklama|
|:---|:---|
|[Get-AzureRmAutomationConnection](/powershell/module/azurerm.automation/get-azurermautomationconnection)|Bir bağlantı alır. Bağlantının alanların değerlerini içeren bir karma tablo içerir.|
|[New-AzureRmAutomationConnection](/powershell/module/azurerm.automation/new-azurermautomationconnection)|Yeni bir bağlantı oluşturur.|
|[Remove-AzureRmAutomationConnection](/powershell/module/azurerm.automation/remove-azurermautomationconnection)|Mevcut bir bağlantıyı kaldırır.|
|[Set-AzureRmAutomationConnectionFieldValue](/powershell/module/azurerm.automation/set-azurermautomationconnectionfieldvalue)|Mevcut bir bağlantı için belirli bir alanın değerini ayarlar.|

## <a name="activities"></a>Etkinlikler

Aşağıdaki tablodaki etkinlikler bir runbook'tan veya DSC yapılandırmasından bağlantıları erişmek için kullanılır.

|Etkinlikler|Açıklama|
|---|---|
|[Get-AutomationConnection](/powershell/module/servicemanagement/azure/get-azureautomationconnection?view=azuresmps-3.7.0)|Kullanmak için bir bağlantı alır. Bağlantı özelliklerini içeren bir karma tablo döndürür.|

>[!NOTE] 
>Değişkenleri kullanmaktan kaçınmanız gerekir Name parametresinde ile **Get-AutomationConnection** bu tasarım zamanında runbook veya DSC yapılandırmaları ve bağlantı varlıkları arasındaki bağımlılıkları getirebileceğinden.

 
## <a name="python2-functions"></a>Python2 işlevleri 
Aşağıdaki tabloda işlevi bir Python2 runbook'ta bağlantılara erişmek için kullanılır. 

| İşlev | Açıklama | 
|:---|:---| 
| automationassets.get_automation_connection | Bir bağlantı alır. Bağlantı özelliklerini içeren bir sözlüğü döndürür. | 

> [!NOTE] 
> Varlık işlevlerine erişmek için Python runbook'unuzu üst kısmında "automationassets" modülü içeri aktarmalısınız.

## <a name="creating-a-new-connection"></a>Yeni bir bağlantı oluşturma

### <a name="to-create-a-new-connection-with-the-azure-portal"></a>Azure portalı ile yeni bir bağlantı oluşturmak için

1. Otomasyon hesabınızdan tıklayın **varlıklar** açmak için bölümü **varlıklar** dikey penceresi.
2. Tıklayın **bağlantıları** açmak için bölümü **bağlantıları** dikey penceresi.
3. Tıklayın **bağlantı ekleme** dikey penceresinin üstünde.
4. İçinde **türü** açılır listesinde, oluşturmak istediğiniz bağlantı türünü seçin. Form, belirli bir tür için özellikler sunar.
5. Formu tamamlayıp tıklayın **Oluştur** yeni bağlantıyı kaydetmek için.

### <a name="to-create-a-new-connection-with-windows-powershell"></a>Windows PowerShell ile yeni bir bağlantı oluşturmak için

Windows PowerShell ile yeni bir bağlantı oluşturmak [yeni AzureRmAutomationConnection](/powershell/module/azurerm.automation/new-azurermautomationconnection) cmdlet'i. Bu cmdlet adlı bir parametre kullanılmaktadır **ConnectionFieldValues** , bekliyor bir [karma tablo](https://technet.microsoft.com/library/hh847780.aspx) her bağlantı türü tarafından tanımlanan özellikleri için değerleri tanımlama.

Otomasyonla biliyorsanız [Run As hesabı](automation-sec-configure-azure-runas-account.md) hizmet sorumlusunu kullanarak runbook'ların kimliğini doğrulamak için PowerShell Betiği portaldan farklı çalıştır hesabı oluşturmaya alternatif olarak, yeni bir bağlantı oluşturur Aşağıdaki örnek komutlar kullanarak varlık.  

```powershell
$ConnectionAssetName = "AzureRunAsConnection"
$ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId}
New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues
```

Bağlantı varlığı, Otomasyon hesabı oluşturduğunuzda, bunu otomatik olarak birkaç genel modüller bağlantı türü ile birlikte varsayılan içerdiğinden oluşturmak için komut dosyası kullanabilir **AzureServicePrincipal** için oluşturma **AzureRunAsConnection** bağlantı varlığı.  Farklı kimlik doğrulama yöntemiyle bir hizmete veya uygulamaya bağlanmak için yeni bir bağlantı varlığı oluşturmaya çalışırsanız, bağlantı türü Otomasyon hesabınızda önceden tanımlı olmadığından, başarısız olacağı için bunu göz önünde tutmak önemlidir.  Kendi özel veya modülünden bağlantı türünün nasıl oluşturulacağı konusunda daha fazla bilgi [PowerShell Galerisi](https://www.powershellgallery.com), bkz: [tümleştirme modülleri](automation-integration-modules.md)
  
## <a name="using-a-connection-in-a-runbook-or-dsc-configuration"></a>Bir runbook'tan veya DSC yapılandırmasından bağlantı kullanma

Bir runbook'tan veya DSC yapılandırmasından ile bağlantıda almak **Get-AutomationConnection** cmdlet'i.  Kullanamazsınız [Get-AzureRmAutomationConnection](/powershell/module/azurerm.automation/get-azurermautomationconnection) etkinlik.  Bu etkinlik bağlantıdaki farklı alanların değerlerini alır ve bunları olarak döndüren bir [karma tablo](https://go.microsoft.com/fwlink/?LinkID=324844) sonra kullanılabilen runbook'tan veya DSC yapılandırmasından uygun komutları ile.

### <a name="textual-runbook-sample"></a>Metinsel runbook örneği

Aşağıdaki örnek komutlar runbook'unuzda Azure Resource Manager kaynaklarıyla kimlik doğrulaması için daha önce belirtilen farklı çalıştır hesabını kullanmayı gösterir.  Bağlantı varlığı temsil eden kimlik bilgilerini değil, sertifika tabanlı hizmet sorumlusu başvuran farklı çalıştır hesabı kullanır.  

```powershell
$Conn = Get-AutomationConnection -Name AzureRunAsConnection 
Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
```

> [!IMPORTANT]
> **Add-AzureRmAccount** için bir diğer ad sunulmuştur **Connect-AzureRMAccount**. Ne zaman kitaplığınızı arama öğe görmüyorsanız, **Connect-AzureRMAccount**, kullanabileceğiniz **Add-AzureRmAccount**, veya Otomasyon hesabınızda modüllerinizi güncelleştirebilirsiniz.

### <a name="graphical-runbook-samples"></a>Grafik runbook örnekleri

Eklediğiniz bir **Get-AutomationConnection** bağlantıda grafik Düzenleyicisi Kitaplık bölmesinde sağ tıklatıp seçerek grafiksel bir runbook etkinliğine **tuvale Ekle**.

![Tuvale Ekle](media/automation-connections/connection-add-canvas.png)

Aşağıdaki görüntüde, grafik bir runbook'ta bağlantı kullanma örneği gösterilmektedir.  Bu, metinsel bir runbook ile farklı çalıştır hesabını kullanarak kimlik doğrulaması için yukarıda gösterilen aynı örnektir.  Bu örnekte **sabit değer** veri kümesi için **farklı çalıştır bağlantısını Al** aktivitesi kimlik doğrulaması için bir bağlantı nesnesi kullanır.  A [ardışık düzen bağlantısına](automation-graphical-authoring-intro.md#links-and-workflow) ServicePrincipalCertificate parametre kümesi tek bir nesne bekliyor olduğundan burada kullanılır.

![bağlantıları](media/automation-connections/automation-get-connection-object.png)

### <a name="python2-runbook-sample"></a>Python2 runbook örneği
Aşağıdaki örnek, bir Python2 runbook farklı çalıştır bağlantısı kullanarak kimlik doğrulaması yapmayı gösterir.

```python
""" Tutorial to show how to authenticate against Azure resource manager resources """
import azure.mgmt.resource
import automationassets

def get_automation_runas_credential(runas_connection):
  """ Returns credentials to authenticate against Azure resoruce manager """
  from OpenSSL import crypto
  from msrestazure import azure_active_directory
  import adal

  # Get the Azure Automation Run As service principal certificate
  cert = automationassets.get_automation_certificate("AzureRunAsCertificate")
  pks12_cert = crypto.load_pkcs12(cert)
  pem_pkey = crypto.dump_privatekey(crypto.FILETYPE_PEM, pks12_cert.get_privatekey())

  # Get Run As connection information for the Azure Automation service principal
  application_id = runas_connection["ApplicationId"]
  thumbprint = runas_connection["CertificateThumbprint"]
  tenant_id = runas_connection["TenantId"]

  # Authenticate with service principal certificate
  resource = "https://management.core.windows.net/"
  authority_url = ("https://login.microsoftonline.com/" + tenant_id)
  context = adal.AuthenticationContext(authority_url)
  return azure_active_directory.AdalAuthentication(
    lambda: context.acquire_token_with_client_certificate(
      resource,
      application_id,
      pem_pkey,
      thumbprint)
  )

# Authenticate to Azure using the Azure Automation Run As service principal
runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")
azure_credential = get_automation_runas_credential(runas_connection)
```

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme [grafik yazma içinde bağlantıları](automation-graphical-authoring-intro.md#links-and-workflow) yönlendirmek ve runbook'larınızı mantık akışını denetlemek nasıl yapılacağını görmek için.  

- Azure automation'da tümleştirme modülü olarak çalışacak şekilde kendi PowerShell modüllerinizi oluştururken Azure Otomasyonu'nun kullanımını PowerShell modülleri ve en iyi uygulamalar hakkında daha fazla bilgi için bkz: [tümleştirme modülleri](automation-integration-modules.md).  

