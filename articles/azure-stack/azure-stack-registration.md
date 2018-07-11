---
title: Azure kaydı için Azure Stack tümleşik sistemleri | Microsoft Docs
description: Azure Stack Azure bağlı çok düğümlü dağıtımlar için Azure kayıt işlemi açıklanmaktadır.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2018
ms.author: jeffgilb
ms.reviewer: brbartle
ms.openlocfilehash: 65525ffe33ddc100dd3066e7c2b52ef8a856fbc3
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37934678"
---
# <a name="register-azure-stack-with-azure"></a>Azure Stack Azure ile kaydedin

Kaydetme [Azure Stack](azure-stack-poc.md) ile Azure, Azure Market öğelerini indirme ve ticaret verileri Microsoft'a raporlamaya ayarlamak için sağlar. Azure Stack kaydettikten sonra kullanım için Azure ticaret bildirilir ve kayıt için kullanılan abonelik altında görebilirsiniz.

> [!IMPORTANT]  
> Kayıt, Pazar dağıtımı da dahil olmak üzere tam Azure Stack işlevleri desteklemek için gereklidir. Ayrıca, Azure Stack-,-Kullandıkça Ödeme modeli kullanılırken kaydedilmezse lisanslama koşulları ihlal olacaktır. Azure Stack lisanslama modelleri hakkında daha fazla bilgi için bkz. [sayfa satın alma](https://azure.microsoft.com/overview/azure-stack/how-to-buy/).

## <a name="prerequisites"></a>Önkoşullar

Azure Stack Azure ile kaydetmeden önce şunlara sahip olmalısınız:

- Abonelik kimliği için bir Azure aboneliği. Kimliğini almak için Azure'da oturum açın, sırayla **diğer hizmetler** > **abonelikleri**, kullanmak istediğiniz aboneliğe tıklayın ve altında **Essentials** bulabilirsiniz Abonelik kimliği

  > [!NOTE]
  > Almanya ve ABD kamu bulut abonelikleri şu anda desteklenmiyor.

- Kullanıcı adı ve parola için aboneliğin sahibi olan bir hesabı (MSA/2FA hesapları desteklenir).
- Azure Stack kaynak sağlayıcısına kayıtlı (Ayrıntılar için aşağıdaki Azure Stack kaynak sağlayıcısını kaydetme bölümüne bakın).

Bu gereksinimleri karşıladığını Azure aboneliğiniz yoksa, şunları yapabilirsiniz [buradan ücretsiz bir Azure hesabı oluşturun](https://azure.microsoft.com/free/?b=17.06). Azure aboneliğinize ücret ödemeden Azure Stack kaydetme artmasına neden olur.

### <a name="powershell-language-mode"></a>PowerShell dil modu

Azure Stack başarıyla kaydetmek için PowerShell dil modunu ayarlanmalıdır **FullLanguageMode**.  Geçerli dil modunu tam, yükseltilmiş bir PowerShell penceresi açın ve aşağıdaki PowerShell komutlarını çalıştırmak için ayarlandığını doğrulamak için:

```powershell
$ExecutionContext.SessionState.LanguageMode
```

Çıktıyı döndürür olun **FullLanguageMode**. Herhangi bir dil modu döndürülen kayıt başka bir makinede çalıştırılması gerekir ya da dil modunu ayarlanması gerekir **FullLanguageMode** devam etmeden önce.

### <a name="bkmk_powershell"></a>Azure Stack için PowerShell'i yükleme

Azure ile kaydetmek için Azure Stack için en son PowerShell kullanmanız gerekir.

Henüz yüklü değilse, [Azure Stack için PowerShell yükleme](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).

### <a name="bkmk_tools"></a>Azure Stack araçları indirin

Azure Stack araçları GitHub deposunu Azure Stack işlevselliği destekleyen PowerShell modüllerini içerir. Kayıt işlevleri dahil olmak üzere. İçeri aktarma ve RegisterWithAzure.psm1 PowerShell modülü kullanması gereken kayıt işlemi sırasında Azure Stack örneğinizin Azure ile kaydetmek için Azure Stack araçları deposunda bulunamadı.

En son sürümü kullandığınızdan emin olmak için Azure Stack araçları var olan tüm sürümlerini silmeniz gerekir ve [Github'dan en son sürümünü indirin](azure-stack-powershell-download.md) Azure'la kaydetmeden önce.

## <a name="register-azure-stack-in-connected-environments"></a>Azure Stack bağlı ortamlarda kaydetme

Azure ve internet bağlantılı ortamlar erişebilirsiniz. Bu ortamlar için Azure ile Azure Stack kaynak sağlayıcısını kaydedin ve ardından faturalama modelinizi yapılandırmanız gerekir.

> [!NOTE]
> Bu adımların tümünü ayrıcalıklı uç noktasına erişimi olan bir bilgisayardan çalıştırmanız gerekir.

### <a name="register-the-azure-stack-resource-provider"></a>Azure Stack kaynak sağlayıcısını kaydetme

Azure ile Azure Stack kaynak sağlayıcısını kaydetmek için PowerShell ISE yönetici olarak başlatın ve aşağıdaki PowerShell komutları ile kullanın **EnvironmentName** parametre uygun Azure aboneliğini türüne (bkz: parametreleri aşağıdaki).

1. Azure Stack kaydetmek için kullandığınız bir Azure hesabı ekleyin. Hesap eklemek için şunu çalıştırın **Add-AzureRmAccount** cmdlet'i. Azure genel yönetici hesabı kimlik bilgilerinizi girmeniz istenir ve hesabınızın yapılandırmasına bağlı olarak 2 faktörlü kimlik doğrulaması kullanmak zorunda kalabilirsiniz.

   ```PowerShell
      Add-AzureRmAccount -EnvironmentName "<Either AzureCloud or AzureChinaCloud>"
   ```

   | Parametre | Açıklama |  
   |-----|-----|
   | EnvironmentName | Azure bulut aboneliği ortam adı. Desteklenen ortam adları **AzureCloud** veya Çin Azure aboneliği kullanıyorsanız **AzureChinaCloud**.  |
   |  |  |

2. Birden fazla aboneliğiniz varsa, kullanmak istediğiniz seçmek için aşağıdaki komutu çalıştırın:  

   ```PowerShell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

3. Azure aboneliğinizdeki Azure Stack kaynak sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın:

   ```PowerShell
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

### <a name="register-azure-stack-with-azure-using-the-pay-as-you-use-billing-model"></a>Azure Stack-,-Kullandıkça Ödeme modelini kullanarak Azure ile kaydedin

Azure Stack-,-Kullandıkça Ödeme modelini kullanarak Azure ile kaydetmek için aşağıdaki adımları kullanın.

1. PowerShell ISE yönetici olarak başlatın ve gidin **kayıt** klasöründe **AzureStack araçları ana** dizini oluşturuldu, [Azure Stack Araçlarıindirildi](#bkmk_tools). İçeri aktarma **RegisterWithAzure.psm1** kullanarak PowerShell Modülü:

   ```powershell
   Import-Module .\RegisterWithAzure.psm1
   ```

2. Ardından, aynı PowerShell oturumunda, doğru Azure PowerShell bağlamına oturum emin olun. Yukarıdaki Azure Stack kaynak sağlayıcısını kaydetmek için kullanılan azure hesap budur. Çalıştırılacak Powershell:

   ```powershell
   Add-AzureRmAccount -Environment "<Either AzureCloud or AzureChinaCloud>"
   ```

3. Aynı PowerShell oturumunda çalıştırın **kümesi AzsRegistration** cmdlet'i. Çalıştırılacak PowerShell:  

   ```powershell
   $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
   Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -BillingModel PayAsYouUse
   ```

  |Parametre|Açıklama|
  |-----|-----|
  |PrivilegedEndpointCredential|İçin kullanılan kimlik bilgileri [ayrıcalıklı uç noktasına erişmek](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). Kullanıcı adı şu biçimdedir **AzureStackDomain\CloudAdmin**.|
  |PrivilegedEndpoint|Dağıtım görevleri ile günlük toplama ve diğer posta gibi özellikler sağlayan bir önceden yapılandırılmış Uzaktan PowerShell Konsolu. Daha fazla bilgi edinmek için bkz [ayrıcalıklı uç noktayı kullanarak](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) makalesi.|
  |BillingModel|Faturalandırma modeli, aboneliğinizin kullanır. İzin verilen bu parametre için değerler: kapasite PayAsYouUse ve geliştirme.|
  |  |  |

  İşlem, 10 ile 15 dakika arasında sürer. Komut tamamlandığında bir ileti görürsünüz **"ortamınızı şimdi kaydedilir ve sağlanan parametreleri kullanarak etkinleştirildi."**

### <a name="register-azure-stack-with-azure-using-the-capacity-billing-model"></a>Azure Stack kapasite faturalandırma modeli kullanarak Azure ile kaydedin

,-Kullandıkça Ödeme modeli kullanarak kaydetmek için kullanılan aynı yönergeleri izleyin ancak altında kapasite satın anlaşma numarası ekleme ve değiştirme **BillingModel** parametresi **Kapasite**. Diğer tüm parametreler değiştirilmez.

Çalıştırılacak PowerShell:

```powershell
$CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
Set-AzsRegistration `
    -PrivilegedEndpointCredential $CloudAdminCred `
    -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
    -AgreementNumber <EA agreement number> `
    -BillingModel Capacity
```

## <a name="register-azure-stack-in-disconnected-environments"></a>Azure Stack'i bağlantısız ortamlarda kaydetme
(İnternet bağlantısı olmayan) bağlantısı kesilmiş bir ortamda Azure Stack kaydediyorsanız, Azure Stack ortamından bir kayıt belirtecinizi almak ve sonra bu belirteci Azure'a bağlanabilir ve bir bilgisayara ihtiyacınız [PowerShell yüklü Azure Stack için](#bkmk_powershell).  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Azure Stack ortamından bir kayıt belirtecinizi almak

1. PowerShell ISE yönetici olarak başlatın ve gidin **kayıt** klasöründe **AzureStack araçları ana** dizini oluşturuldu, [Azure Stack Araçlarıindirildi](#bkmk_tools). İçeri aktarma **RegisterWithAzure.psm1** Modülü:  

   ```powershell
   Import-Module .\RegisterWithAzure.psm1
   ```

2. Kayıt belirtecinizi almak için aşağıdaki PowerShell komutlarını çalıştırın:  

   ```Powershell
   $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
   ```

   > [!TIP]  
   > Kayıt belirteç için belirtilen dosya kaydedilir *$FilePathForRegistrationToken*. Dosya yolu veya dosya adı, günlüklerinizin bekletilme değiştirebilirsiniz.

3. Azure üzerinde kullanım için bu kayıt belirtecinizi kaydetme makine bağlı. Dosya veya metin $FilePathForRegistrationToken kopyalayabilirsiniz.

### <a name="connect-to-azure-and-register"></a>Azure ve kayıt bağlanma

İnternet'e bağlı olan bilgisayara RegisterWithAzure.psm1 modülü ve oturum açma için doğru Azure Powershell bağlamı içeri aktarmak için aynı adımları gerçekleştirin. Ardından kayıt AzsEnvironment arayın ve Azure ile kaydetmek için kayıt belirtecini belirtin:

  ```Powershell  
  $registrationToken = "<Your Registration Token>"
  Register-AzsEnvironment -RegistrationToken $registrationToken  
  ```

İsteğe bağlı olarak, kayıt belirtecinizi içeren bir dosyaya işaret edecek şekilde Get-Content cmdlet'i kullanabilirsiniz:

  ```Powershell  
  $registrationToken = Get-Content -Path '<Path>\<Registration Token File>'
  Register-AzsEnvironment -RegistrationToken $registrationToken  
  ```

  > [!NOTE]  
  > Kayıt kaynak adı ve gelecekte başvurmak için kayıt belirtecini kaydedin.

### <a name="retrieve-an-activation-key-from-azure-registration-resource"></a>Azure kaydı kaynaktan etkinleştirme anahtarı alma

Ardından, Azure'da Register-AzsEnvironment sırasında oluşturulan kayıt kaynağıyla etkinleştirme anahtarı almak gerekir.

Etkinleştirme anahtarı almak için aşağıdaki PowerShell komutlarını çalıştırın:  

  ```Powershell
  $RegistrationResourceName = "AzureStack-<Cloud Id for the Environment to register>"
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt"
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName -KeyOutputFilePath $KeyOutputFilePath
  ```

  > [!TIP]  
  > Etkinleştirme anahtarı için belirtilen dosyada kaydedilmiş *$KeyOutputFilePath*. Dosya yolu veya dosya adı, günlüklerinizin bekletilme değiştirebilirsiniz.

### <a name="create-an-activation-resource-in-azure-stack"></a>Azure Stack'te bir etkinleştirme kaynağını oluşturma

Azure Stack ortamına, Get-AzsActivationKey oluşturulan etkinleştirme anahtarından dosya ya da metin döndürür. Ardından bu etkinleştirme anahtarı kullanarak Azure Stack'te bir etkinleştirme kaynağını oluşturacaksınız. Kaynak bir etkinleştirme oluşturmak için aşağıdaki PowerShell komutlarını çalıştırın:  

  ```Powershell
  $ActivationKey = "<activation key>"
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey
  ```

İsteğe bağlı olarak, kayıt belirtecinizi içeren bir dosyaya işaret edecek şekilde Get-Content cmdlet'i kullanabilirsiniz:

  ```Powershell
  $ActivationKey = Get-Content -Path '<Path>\<Activation Key File>'
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey
  ```

## <a name="verify-azure-stack-registration"></a>Azure Stack kayıt doğrulayın

Azure Stack, Azure ile başarılı bir şekilde kaydedildiğini doğrulamak için aşağıdaki adımları kullanın.

1. Azure Stack oturum [Yönetici portalı](https://docs.microsoft.com/azure/azure-stack/azure-stack-manage-portals#access-the-administrator-portal): https&#58;/ / adminportal. *&lt;bölge >. &lt;fqdn >*.
2. Seçin **diğer hizmetler** > **Market Yönetim** > **Azure'dan ekleme**.

(Örneğin, WordPress) azure'dan kullanılabilir öğeleri listesini görürseniz, etkinleştirme başarılı oldu. Ancak, bağlantısı kesilmiş ortamlarda, Azure Stack marketini Azure Market öğeleri tarafından görülmez.

> [!NOTE]
> Kayıt tamamlandıktan sonra değil kaydetmek için etkin uyarı görünmeyecek.

## <a name="renew-or-change-registration"></a>Yenileme veya kaydı değiştirme

### <a name="renew-or-change-registration-in-connected-environments"></a>Yenileme veya bağlı ortamlarda kaydı değiştirme

Güncelleştirebilir veya aşağıdaki durumlarda kaydınızı yenilemeniz gerekir:

- Sonra kapasite tabanlı yıllık aboneliğinizi yenileyin.
- Faturalandırma modelinizin değiştirdiğinizde.
- Ne zaman kapasite kullanıma dayalı faturalandırma için değişiklikleri (düğüm ekleme/kaldırma) ölçeklendirin.

#### <a name="change-the-subscription-you-use"></a>Kullandığınız aboneliği Değiştir

Aboneliğinizi değiştirmek istiyorsanız, kullanırsanız, önce çalıştırmalısınız **Remove-AzsRegistration** cmdlet'ini doğru Azure PowerShell bağlamına oturum açtığınızdan emin olun ve son olarak çalıştırın **AzsRegistration kümesi**  tüm parametreleri değiştirildi:

  ```powershell
  Remove-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```

#### <a name="change-the-billing-model-or-syndication-features"></a>Faturalama modeli ya da dağıtım özelliklerini değiştirme

Faturalandırma modeli veya yüklemenizin dağıtım özelliklerini değiştirmek istiyorsanız, yeni değerleri ayarlamak için kayıt işlevi çağırabilir. Geçerli kaydı silmeniz gerekmez:

  ```powershell
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```

### <a name="renew-or-change-registration-in-disconnected-environments"></a>Yenileme veya bağlantısı kesilmiş ortamlarda kaydı değiştirme

Güncelleştirebilir veya aşağıdaki durumlarda kaydınızı yenilemeniz gerekir:

- Sonra kapasite tabanlı yıllık aboneliğinizi yenileyin.
- Faturalandırma modelinizin değiştirdiğinizde.
- Ne zaman kapasite kullanıma dayalı faturalandırma için değişiklikleri (düğüm ekleme/kaldırma) ölçeklendirin.

#### <a name="remove-the-activation-resource-from-azure-stack"></a>Azure yığını etkinleştirme kaynağını kaldırın

İlk Azure Stack ve Azure kayıt kaynakta etkinleştirme kaynak kaldırmak gerekir.  

Azure Stack'te etkinleştirme kaynak kaldırmak için Azure Stack ortamınıza aşağıdaki PowerShell komutlarını çalıştırın:  

  ```Powershell
  Remove-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  ```

Ardından, Azure'da kayıt kaynağı kaldırmak için bağlı bir Azure üzerinde olduğundan emin olun, bilgisayar için doğru Azure PowerShell bağlamı oturum açın ve aşağıda açıklandığı gibi uygun bir PowerShell komutlarını çalıştırın.

Kaynak oluşturmak için kullanılan kayıt belirtecinizi kullanabilirsiniz:  

  ```Powershell
  $registrationToken = "<registration token>"
  Unregister-AzsEnvironment -RegistrationToken $registrationToken
  ```

Veya, kayıt adı kullanabilirsiniz:

  ```Powershell
  $registrationName = "AzureStack-<Cloud Id of Azure Stack Environment>"
  Unregister-AzsEnvironment -RegistrationName $registrationName
  ```

### <a name="re-register-using-disconnected-steps"></a>Bağlantısı kesilmiş adımları kullanarak yeniden kaydolun

Artık tamamen bağlantısı kesilmiş bir senaryoda kaydı ve bir Azure Stack ortamına bağlı olmayan bir senaryoda kaydetmek için adımları yinelemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Market öğelerini indirme](azure-stack-download-azure-marketplace-item.md)
