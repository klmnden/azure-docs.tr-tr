---
title: Azure ile ASDK kaydetme | Microsoft Docs
description: Azure Stack Market dağıtım ve kullanım raporlama etkinleştirmek için Azure ile kaydetmek açıklar.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2019
ms.author: jeffgilb
ms.reviewer: misainat
ms.lastreviewed: 01/16/2019
ms.openlocfilehash: 4dc4c9d4d936bbcf626884c5c90e16f640f268a0
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58487779"
---
# <a name="azure-stack-registration"></a>Azure Stack kaydı
Azure Market öğelerini indirme ve ticaret verileri Microsoft'a raporlamaya ayarlamak için Azure ile Azure Stack geliştirme Seti'ni (ASDK) yüklemenizi kaydedebilirsiniz. Kayıt, Pazar dağıtımı da dahil olmak üzere tam Azure Stack işlevleri desteklemek için gereklidir. Kayıt, Market dağıtım ve kullanım raporlama gibi önemli Azure Stack işlevselliğini test etmek etkinleştirmek için gereklidir. Azure Stack kaydettikten sonra kullanım için Azure ticaret bildirilir. Kayıt için kullanılan abonelik altında görebilirsiniz. Ancak ASDK kullanıcılar bunlar rapor tüm kullanımlar için ücretlendirilmezsiniz.

ASDK kaydedilmezse görebileceğiniz bir **etkinleştirme gerekli** , Azure Stack geliştirme Seti'ni kaydedilecek öneren uyarı bildirimi. Bu beklenen bir davranıştır.

## <a name="prerequisites"></a>Önkoşullar
Azure Stack PowerShell yüklü ve açıklandığı gibi Azure Stack araçları indirilen olun, Azure ile ASDK kaydetmek için bu yönergeleri kullanmadan önce [dağıtım sonrası yapılandırma](asdk-post-deploy.md) makalesi.

Ayrıca, PowerShell dil modunu ayarlamanız gerekir **FullLanguageMode** ASDK Azure ile kaydetmek için kullanılan bilgisayarda. Geçerli dil modunu tam, yükseltilmiş bir PowerShell penceresi açın ve aşağıdaki PowerShell komutlarını çalıştırmak için ayarlandığını doğrulamak için:

```powershell  
$ExecutionContext.SessionState.LanguageMode
```

Çıktıyı döndürür olun **FullLanguageMode**. Diğer bir dil modu döndürülen kayıt başka bir bilgisayarda çalıştırılması gerekir ya da dil modunu ayarlanması gerekir **FullLanguageMode** devam etmeden önce.

Azure AD hesabını kayıt ihtiyaçları için kullanılan Azure aboneliğine erişiminiz olması ve bu abonelikle dizinde kimlik uygulama ve hizmet sorumluları oluşturma izniniz yok. Azure Stack Azure ile kaydedin en az ayrıcalıklı yönetim tarafından kullanılması önerilir [kayıt için kullanılacak bir hizmet hesabı oluşturmanızı](../azure-stack-registration-role.md) genel yönetici kimlik bilgilerini kullanmak yerine.

## <a name="register-azure-stack-with-azure"></a>Azure Stack Azure ile kaydedin
Azure ile ASDK kaydetmek için aşağıdaki adımları izleyin.

> [!NOTE]
> Bu adımların tümünü ayrıcalıklı uç noktasına erişimi olan bir bilgisayardan çalıştırmanız gerekir. ASDK için Geliştirme Seti ana bilgisayar olmasıdır.

1. Yönetici olarak bir PowerShell konsolu açın.  

2. Azure ile ASDK yüklemenizi kaydetmek için aşağıdaki PowerShell komutlarını çalıştırın. Hem Azure faturalandırma abonelik Kimliğinizi hem de yerel ASDK yükleme için oturum açmanız. Azure faturalandırma bir abonelik kimliği yok henüz yapabilecekleriniz [buradan ücretsiz bir Azure hesabı oluşturun](https://azure.microsoft.com/free/?b=17.06). Azure aboneliğinize ücret ödemeden Azure Stack kaydetme artmasına neden olur.<br><br>Programını çalıştırdığınızda, kayıt için benzersiz bir ad ayarlayın **kümesi AzsRegistration** cmdlet'i. **RegistrationName** parametresinin varsayılan değeri **AzureStackRegistration**. Azure Stack birden fazla örneğinde aynı adı kullanın, ancak komut başarısız olur.

    ```powershell  
    # Add the Azure cloud subscription environment name. 
    # Supported environment names are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
    Add-AzureRmAccount -EnvironmentName "<environment name>"

    # Register the Azure Stack resource provider in your Azure subscription
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

    # Import the registration module that was downloaded with the GitHub tools
    Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

    # If you have multiple subscriptions, run the following command to select the one you want to use:
    # Get-AzureRmSubscription -SubscriptionID "<subscription ID>" | Select-AzureRmSubscription
    
    # Register Azure Stack
    $AzureContext = Get-AzureRmContext
    $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
    $RegistrationName = "<unique-registration-name>"
    Set-AzsRegistration `
    -PrivilegedEndpointCredential $CloudAdminCred `
    -PrivilegedEndpoint AzS-ERCS01 `
    -BillingModel Development `
    -RegistrationName $RegistrationName `
    -UsageReportingEnabled:$true
    ```
3. Betik tamamlandığında, bu iletiyi görmeniz gerekir: **Ortamınızı şimdi kaydedilir ve sağlanan parametreleri kullanarak etkinleştirildi.**

    ![Ortamınızı artık kayıtlı](media/asdk-register/1.PNG)


## <a name="register-in-disconnected-environments"></a>Bağlantısı kesilmiş ortamlarda kaydetme
Azure Stack bağlantısı kesilmiş bir ortamda (internet bağlantısı olmayan) kaydediyorsanız Azure Stack ortamından bir kayıt belirtecinizi almak ve sonra bu belirteci kaydedin ve bir etkinleştirme oluşturmak için Azure bağlanıp bir bilgisayarda gerekir Kaynak ASDK ortamınız için.
 
 > [!IMPORTANT]
 > Azure Stack kaydetmek için bu yönergeleri kullanmadan önce Azure Stack için PowerShell yüklü ve Azure Stack araçları açıklandığı indirilen emin [dağıtım sonrası yapılandırma](asdk-post-deploy.md) ASDK ana bilgisayarda makale bilgisayar ve bilgisayarın internet erişimi için Azure ve kayıt bağlanmak için kullanılan.

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Azure Stack ortamından bir kayıt belirtecinizi almak
ASDK konak bilgisayarda, yönetici olarak PowerShell'i başlatın ve gidin **kayıt** klasöründe **AzureStack araçları ana** Azure Stack araçları yüklendiğinde oluşturulan dizin. İçeri aktarmak için aşağıdaki PowerShell komutlarını kullanın **RegisterWithAzure.psm1** modülü ve ardından **Get-AzsRegistrationToken** kayıt belirtecinizi almak için cmdlet:  

   ```powershell  
   # Import the registration module that was downloaded with the GitHub tools
   Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

   # Create registration token
   $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
   # File path to save the token. This example saves the file as C:\RegistrationToken.txt.
   $FilePathForRegistrationToken = "$env:SystemDrive\RegistrationToken.txt"
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $CloudAdminCred `
   -UsageReportingEnabled:$false `
   -PrivilegedEndpoint AzS-ERCS01 `
   -BillingModel Development `
   -MarketplaceSyndicationEnabled:$false `
   -TokenOutputFilePath $FilePathForRegistrationToken
   ```

Bu kayıt belirtecinizi kullanmak için İnternet'e bağlı bir bilgisayara kaydedin. Dosyadan $FilePathForRegistrationToken parametresi tarafından oluşturulan dosya veya metni kopyalayın.

### <a name="connect-to-azure-and-register"></a>Azure ve kayıt bağlanma
Internet'e bağlı bilgisayara kullanmak aşağıdaki PowerShell komutlarını içeri aktarmak için **RegisterWithAzure.psm1** modülü ve ardından **Register-AzsEnvironment** cmdlet'i ile kullanarak Azure kaydetmek için Yeni oluşturduğunuz kaydı belirtecini ve benzersiz kayıt adı:  

  ```powershell  
  # Add the Azure cloud subscription environment name. 
  # Supported environment names are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
  Add-AzureRmAccount -EnvironmentName "<environment name>"

  # If you have multiple subscriptions, run the following command to select the one you want to use:
  # Get-AzureRmSubscription -SubscriptionID "<subscription ID>" | Select-AzureRmSubscription

  # Register the Azure Stack resource provider in your Azure subscription
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

  # Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  # Register with Azure
  # This example uses the C:\RegistrationToken.txt file.
  $registrationToken = Get-Content -Path "$env:SystemDrive\RegistrationToken.txt"
  $RegistrationName = "<unique-registration-name>"
  Register-AzsEnvironment -RegistrationToken $registrationToken `
  -RegistrationName $RegistrationName
  ```

Alternatif olarak, **Get-Content** kayıt belirtecinizi içeren bir dosyaya işaret edecek şekilde cmdlet:

  ```powershell  
  # Add the Azure cloud subscription environment name. 
  # Supported environment names are AzureCloud, AzureChinaCloud or AzureUSGovernment depending which Azure subscription you are using.
  Add-AzureRmAccount -EnvironmentName "<environment name>"

  # If you have multiple subscriptions, run the following command to select the one you want to use:
  # Get-AzureRmSubscription -SubscriptionID "<subscription ID>" | Select-AzureRmSubscription

  # Register the Azure Stack resource provider in your Azure subscription
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack

  # Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  # Register with Azure 
  # This example uses the C:\RegistrationToken.txt file.
  $registrationToken = Get-Content -Path "$env:SystemDrive\RegistrationToken.txt"
  Register-AzsEnvironment -RegistrationToken $registrationToken `
  -RegistrationName $RegistrationName
  ```

Kayıt tamamlandığında, benzer bir ileti görürsünüz **bilgisayarınızı Azure Stack ortamına artık Azure ile kayıtlı.**

> [!IMPORTANT]
> PowerShell penceresini kapatmayın. 

Gelecek başvurular için kaynak adı kayıt ve kayıt belirtecinizi kaydedin.

### <a name="retrieve-an-activation-key-from-the-azure-registration-resource"></a>Etkinleştirme anahtarı Azure kaydı kaynak alma

Hala İnternet'e bağlı bir bilgisayar kullanılarak **ve aynı PowerShell konsol penceresi**, oluşturulan Azure Active Directory'ye kayıt kaynaktan etkinleştirme anahtarı alınamıyor.

Etkinleştirme anahtarı almak için aşağıdaki PowerShell komutlarını çalıştırın, önceki adımda Azure ile kaydolurken sağladığınız benzersiz kayıt adı değeri kullanın:  

  ```Powershell
  $RegistrationResourceName = "<unique-registration-name>"
  # File path to save the activation key. This example saves the file as C:\ActivationKey.txt.
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt"
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName `
  -KeyOutputFilePath $KeyOutputFilePath
  ```
### <a name="create-an-activation-resource-in-azure-stack"></a>Azure Stack'te bir etkinleştirme kaynağını oluşturma

Azure Stack ortamına dosyasıyla döndürür veya metinden etkinleştirme anahtarı oluşturuldu **Get-AzsActivationKey**. Bu etkinleştirme anahtarı kullanarak Azure Stack'te bir etkinleştirme kaynağını oluşturmak için aşağıdaki PowerShell komutlarını çalıştırın:   

  ```Powershell
  # Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1
  
  $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
  $ActivationKey = "<activation key>"
  New-AzsActivationResource -PrivilegedEndpointCredential $CloudAdminCred `
  -PrivilegedEndpoint AzS-ERCS01 `
  -ActivationKey $ActivationKey
  ```

Alternatif olarak, **Get-Content** kayıt belirtecinizi içeren bir dosyaya işaret edecek şekilde cmdlet:

  ```Powershell
  # Import the registration module that was downloaded with the GitHub tools
  Import-Module C:\AzureStack-Tools-master\Registration\RegisterWithAzure.psm1

  $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the credentials to access the privileged endpoint."
  # This example uses the C:\ActivationKey.txt file.
  $ActivationKey = Get-Content -Path "$env:SystemDrive\Activationkey.txt"
  New-AzsActivationResource -PrivilegedEndpointCredential $CloudAdminCred `
  -PrivilegedEndpoint AzS-ERCS01 `
  -ActivationKey $ActivationKey
  ```

Etkinleştirme tamamlandıktan sonra benzer bir ileti görürsünüz **ortamınızı kayıt ve etkinleştirme işlemi tamamlandı.**

## <a name="verify-the-registration-was-successful"></a>Kaydın başarılı olduğunu doğrulayın

Kullanabileceğiniz **bölge Yönetimi** Azure Stack kaydın başarılı olduğunu doğrulamak için bir kutucuk. Bu kutucuk, Yönetici portalı'nda varsayılan pano üzerinde kullanılabilir.

1. Oturum [Azure Stack Yönetim Portalı](https://adminportal.local.azurestack.external).

2. Panoda **bölge Yönetimi**.

    [![Bölge Yönetimi kutucuğu](media/asdk-register/admin1sm.png "bölge Yönetimi kutucuğu")](media/asdk-register/admin1.png#lightbox)

3. Seçin **özellikleri**. Bu dikey pencere, ortamınızın ayrıntılarını ve durumunu gösterir. Durum olabilir **kayıtlı** veya **kayıtlı**. Kaydettiyseniz, Azure Stack, adını ve kayıt kaynak grubunun yanı sıra kaydolmak için kullandığınız ayrıca Azure abonelik Kimliğini gösterir.

## <a name="move-a-registration-resource"></a>Kayıt kaynak taşıma
Kayıt kaynak aynı abonelik altında kaynak grupları arasında taşıma **olduğu** desteklenir. Kaynakları yeni kaynak grubuna taşıma hakkında daha fazla bilgi için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).


## <a name="next-steps"></a>Sonraki adımlar

- [Bir Azure Stack Market öğesi Ekle](../azure-stack-marketplace.md)
