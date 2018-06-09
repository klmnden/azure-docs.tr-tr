---
title: Azure yığınının Azure kaydı tümleşik sistemleri | Microsoft Docs
description: Azure yığın Azure bağlı çok düğümlü dağıtımlar için Azure kayıt işlemini açıklar.
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
ms.date: 06/07/2018
ms.author: jeffgilb
ms.reviewer: avishwan
ms.openlocfilehash: 7d14b246220264641a3bb726d5505c25dc25bbbd
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35248151"
---
# <a name="register-azure-stack-with-azure"></a>Azure ile Azure yığın kaydedin

Kaydetme [Azure yığın](azure-stack-poc.md) Azure ile Azure Market öğesi indirmek ve ticaret veri geri Microsoft'a raporlama ayarlamak için sağlar. Azure yığın kaydettikten sonra kullanım için Azure ticaret bildirilir ve kayıt için kullanılan abonelik altında görebilirsiniz.

> [!IMPORTANT]
> Kayıt Market dağıtım dahil olmak üzere, tam Azure yığın işlevleri desteklemek için gereklidir. Ayrıca, Azure ödeme olarak,-kullanımlı faturalama modeli kullanılırken kaydettirmezseniz koşullarını lisans yığınının ihlali olacaktır. Azure yığını lisanslama modelleri hakkında daha fazla bilgi için lütfen bkz [sayfa satın alma](https://azure.microsoft.com/overview/azure-stack/how-to-buy/).

## <a name="prerequisites"></a>Önkoşullar

Azure ile Azure yığın kaydetmeden önce şunlara sahip olmalısınız:

- Bir Azure aboneliği için abonelik kimliği. Kimliği almak için Azure oturumu açın, **daha fazla hizmet** > **abonelikleri**, kullanmak istediğiniz aboneliği tıklatın ve altında **Essentials** bulabilirsiniz Abonelik kimliği

  > [!NOTE]
  > Almanya ve ABD devlet kurumları abonelikler şu anda desteklenmemektedir.

- Kullanıcı adı ve parola abonelik sahibi olan bir hesap için (MSA/2FA hesapları desteklenir).
- Azure yığın kaynak sağlayıcısı kayıtlı (Ayrıntılar için aşağıdaki Azure yığın kaynak sağlayıcısını Kaydet bölümüne bakın).

Bu gereksinimleri karşılayan bir Azure aboneliğiniz yoksa, şunları yapabilirsiniz [buradan ücretsiz bir Azure hesabı oluşturma](https://azure.microsoft.com/free/?b=17.06). Azure yığın kaydetme, Azure aboneliğinizin üzerinde hiçbir ücret doğurur.

### <a name="powershell-language-mode"></a>PowerShell dil modu

Başarılı bir şekilde Azure yığın kaydettirmek için PowerShell dil modu ayarlanmalıdır **FullLanguageMode**.  Geçerli dil modu tam, yükseltilmiş bir PowerShell penceresi açın ve aşağıdaki PowerShell komutlarını çalıştırmak için ayarlandığını doğrulamak için:

```powershell
$ExecutionContext.SessionState.LanguageMode
```

Çıktıyı döndürür olun **FullLanguageMode**. Başka bir dil modu döndürülen kayıt başka bir makinede çalıştırılması gerekir ya da dil modu ayarlanması gereken **FullLanguageMode** devam etmeden önce.

### <a name="bkmk_powershell"></a>PowerShell için Azure yığın yükle

Azure ile kaydetmek için Azure yığınının son PowerShell kullanmanız gerekir.

Zaten yüklü değilse, [PowerShell için Azure Yığın Yükle](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).

### <a name="bkmk_tools"></a>Azure yığın araçları yükleyin

Azure yığın araçları GitHub deposunu Azure yığın işlevselliğini destekleyen PowerShell modülleri içerir; Kayıt işlevleri dahil olmak üzere. İçeri aktarma ve RegisterWithAzure.psm1 PowerShell modülü kullanması gereken kayıt işlemi sırasında Azure yığın örneğinizi Azure ile kaydetmek için Azure yığın araçları deposu bulunamadı.

En son sürümünü kullandığınızdan emin olmak için mevcut Azure yığın araçları sürümlerini silmeniz gerekir ve [Github'dan en son sürümü karşıdan](azure-stack-powershell-download.md) Azure ile kaydetmeden önce.

## <a name="register-azure-stack-in-connected-environments"></a>Azure yığın bağlı ortamlarda kaydetme

Bağlantılı ortamlar, internet'e ve Azure erişebilir. Bu ortamlar için Azure yığın kaynak sağlayıcısı Azure ile kaydedin ve faturalama modelinizi yapılandırmanız gerekir.

> [!NOTE]
> Tüm adımları ayrıcalıklı uç noktasına erişimi olan bir bilgisayardan çalıştırmanız gerekir.

### <a name="register-the-azure-stack-resource-provider"></a>Azure yığın kaynak sağlayıcısını Kaydet

Azure ile Azure yığın kaynak sağlayıcısını kaydetmek için PowerShell ISE yönetici olarak başlatın ve aşağıdaki PowerShell komutları ile kullanmak **EnvironmentName** parametre uygun Azure aboneliğini türü (bkz. Aşağıdaki parametreler).

1. Azure yığın kaydetmek için kullandığınız Azure hesabı ekleyin. Hesap eklemek için çalıştırın **Add-AzureRmAccount** cmdlet'i. Azure genel yönetici hesabı kimlik bilgilerinizi girmeniz istenir ve hesabınızın yapılandırmasına bağlı olarak 2 öğeli kimlik doğrulama kullanmak zorunda kalabilirsiniz.

   ```PowerShell
      Add-AzureRmAccount -EnvironmentName "<Either AzureCloud or AzureChinaCloud>"
   ```

   | Parametre | Açıklama |  
   |-----|-----|
   | EnvironmentName | Azure bulut aboneliği ortam adı. Desteklenen ortam adları **AzureCloud** veya Çin Azure aboneliği kullanıyorsanız **AzureChinaCloud**.  |
   |  |  |

2. Birden çok aboneliğiniz varsa, kullanmak istediğiniz birini seçmek için aşağıdaki komutu çalıştırın:  

   ```PowerShell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

3. Azure aboneliğinizde Azure yığın kaynak sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın:

   ```PowerShell
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

### <a name="register-azure-stack-with-azure-using-the-pay-as-you-use-billing-model"></a>Azure yığın ödeme olarak,-kullanımlı fatura modelini kullanarak Azure ile kaydedin

Azure yığın ödeme olarak,-kullanımlı fatura modelini kullanarak Azure ile kaydetmek için aşağıdaki adımları kullanın.

1. PowerShell ISE yönetici olarak başlatın ve gidin **kayıt** klasöründe **AzureStack araçları ana** dizin oluşturulur, [Azure yığın Araçlarıindirilen](#bkmk_tools). İçeri aktarma **RegisterWithAzure.psm1** PowerShell kullanarak Modülü:

   ```powershell
   Import-Module .\RegisterWithAzure.psm1
   ```

2. Ardından, aynı PowerShell oturumunda, doğru Azure PowerShell bağlamına oturum emin olun. Yukarıdaki Azure yığın kaynak sağlayıcısını kaydetmek için kullanılan azure hesap budur. Çalıştırılacak Powershell:

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
  |PrivilegedEndpointCredential|İçin kullanılan kimlik bilgileri [ayrıcalıklı uç noktasına erişmek](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). Kullanıcı adı biçimindedir **AzureStackDomain\CloudAdmin**.|
  |PrivilegedEndpoint|Dağıtım görevleri günlük toplama ve diğer posta gibi özellikleriyle sağlayan bir önceden yapılandırılmış Uzaktan PowerShell Konsolu. Daha fazla bilgi edinmek için bkz [kullanan ayrıcalıklı uç noktasını](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) makalesi.|
  |BillingModel|Aboneliğinizi kullanan faturalama modeli. Bu parametre için değer izin verilen: kapasite, PayAsYouUse ve geliştirme.|
  |  |  |

  İşlem arasında 10-15 dakika sürer. Komut tamamlandığında iletisini görürsünüz **"ortamınızı artık kayıtlı ve sağlanan parametreler kullanılarak etkinleştirilmesi."**

### <a name="register-azure-stack-with-azure-using-the-capacity-billing-model"></a>Azure yığın kapasite fatura modelini kullanarak Azure ile kaydedin

Ödeme olarak,-kullanımlı fatura modelini kullanarak kayıt için kullanılan aynı yönergeleri izleyin, ancak altında kapasite satın anlaşma numarası ekleyin ve değiştirme **BillingModel** parametresi **kapasite**. Diğer tüm parametreleri değiştirilmez.

Çalıştırılacak PowerShell:

```powershell
$CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
Set-AzsRegistration `
    -PrivilegedEndpointCredential $CloudAdminCred `
    -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
    -AgreementNumber <EA agreement number> `
    -BillingModel Capacity
```

## <a name="register-azure-stack-in-disconnected-environments"></a>Bağlantısı kesilmiş ortamlarda Azure yığın kaydetme

*Bu bölümdeki bilgiler, Azure yığın 1712 güncelleştirme sürümünü (180106.1) ile başlayan uygular ve daha önceki sürümlerde desteklenmiyor.*

(İnternet bağlantısı olmayan) bağlantısı kesilmiş bir ortamda Azure yığın kaydediyorsanız, Azure yığın ortamından bir kayıt belirtecinizi almak ve ardından bu belirtece Azure'a bağlanabilir ve olan bir bilgisayarda gereken [PowerShell Azure yüklü yığınının](#bkmk_powershell).  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Bir kayıt Azure yığın ortamından belirteci alma

1. PowerShell ISE yönetici olarak başlatın ve gidin **kayıt** klasöründe **AzureStack araçları ana** dizin oluşturulur, [Azure yığın Araçlarıindirilen](#bkmk_tools). İçeri aktarma **RegisterWithAzure.psm1** Modülü:  

   ```powershell
   Import-Module .\RegisterWithAzure.psm1
   ```

2. Kayıt belirtecinizi almak için aşağıdaki PowerShell komutlarını çalıştırın:  

   ```Powershell
   $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
   ```

   > [!TIP]  
   > Kayıt belirtecinizi için belirtilen dosyasında kaydedilen *$FilePathForRegistrationToken*. Filepath ya da dosya kümeleri değiştirebilirsiniz.

3. Bu kayıt belirtecinizi Azure üzerinde kullanılmak üzere kaydetme makine bağlı. Dosya veya metin $FilePathForRegistrationToken kopyalayabilirsiniz.

### <a name="connect-to-azure-and-register"></a>Azure ve kayıt bağlanmak

Internet'e bağlı olan bilgisayarda oturum açma ve RegisterWithAzure.psm1 Modülü doğru Azure Powershell bağlamına içeri aktarmak için aynı adımları gerçekleştirin. Ardından kayıt AzsEnvironment çağırın ve Azure ile kaydetmek için kayıt belirtecini belirtin:

  ```Powershell  
  $registrationToken = "<Your Registration Token>"
  Register-AzsEnvironment -RegistrationToken $registrationToken  
  ```

İsteğe bağlı olarak, kayıt belirtecinizi içeren bir dosyaya işaret için Get-Content cmdlet'i kullanabilirsiniz:

  ```Powershell  
  $registrationToken = Get-Content -Path '<Path>\<Registration Token File>'
  Register-AzsEnvironment -RegistrationToken $registrationToken  
  ```

  > [!NOTE]  
  > Kayıt kaynak adı ve gelecekte başvurmak için kayıt belirteci kaydedin.

### <a name="retrieve-an-activation-key-from-azure-registration-resource"></a>Azure kaydı kaynaktan bir etkinleştirme anahtarı alma

Ardından, Azure'da Register-AzsEnvironment sırasında oluşturulan kayıt kaynağı etkinleştirme anahtarı almak gerekir.

Etkinleştirme anahtarı almak için aşağıdaki PowerShell komutlarını çalıştırın:  

  ```Powershell
  $RegistrationResourceName = "AzureStack-<Cloud Id for the Environment to register>"
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt"
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName -KeyOutputFilePath $KeyOutputFilePath
  ```

  > [!TIP]  
  > Etkinleştirme anahtarı için belirtilen dosyasında kaydedilen *$KeyOutputFilePath*. Filepath ya da dosya kümeleri değiştirebilirsiniz.

### <a name="create-an-activation-resource-in-azure-stack"></a>Azure yığınında bir etkinleştirme kaynağı oluşturma

Azure yığın ortama Get-AzsActivationKey oluşturulan etkinleştirme anahtarı dosya veya metin döndürür. Sonraki o etkinleştirme anahtarı kullanarak Azure yığınında etkinleştirme kaynak oluşturur. Kaynak bir etkinleştirme oluşturmak için aşağıdaki PowerShell komutlarını çalıştırın:  

  ```Powershell
  $ActivationKey = "<activation key>"
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey
  ```

İsteğe bağlı olarak, kayıt belirtecinizi içeren bir dosyaya işaret için Get-Content cmdlet'i kullanabilirsiniz:

  ```Powershell
  $ActivationKey = Get-Content -Path '<Path>\<Activation Key File>'
  New-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -ActivationKey $ActivationKey
  ```

## <a name="verify-azure-stack-registration"></a>Azure yığın kayıt doğrulayın

Azure yığını, Azure ile başarılı bir şekilde kaydedildiğini doğrulamak için aşağıdaki adımları kullanın.

1. Azure yığına oturum [Yönetici portalı](https://docs.microsoft.com/azure/azure-stack/azure-stack-manage-portals#access-the-administrator-portal): https&#58;/ / adminportal. *&lt;bölge >. &lt;fqdn >*.
2. Seçin **daha fazla hizmet** > **Market Yönetim** > **Azure'dan eklemek**.

Bir öğe (örneğin, WordPress) azure'dan kullanılabilir listesi görürseniz, etkinleştirme başarılı oldu. Ancak, bağlantısı kesilmiş ortamlarda, Azure yığın Market'te Azure Market öğesi görmez.

> [!NOTE]
> Kayıt tamamlandıktan sonra değil kaydettirmek için etkin uyarı artık görünür.

## <a name="renew-or-change-registration"></a>Yenileme veya kayıt değiştirme

### <a name="renew-or-change-registration-in-connected-environments"></a>Yenileme veya bağlı ortamlarda kayıt değiştirme

Güncelleştirme veya kaydınızı aşağıdaki durumlarda Yenile gerekir:

- Sonra kapasite tabanlı yıllık aboneliğinizi yenileyin.
- Faturalama modelinizi değiştirdiğinizde.
- Ne zaman kapasite tabanlı faturalama için değişiklikleri (Ekle/Kaldır düğümler) ölçeklendirin.

#### <a name="change-the-subscription-you-use"></a>Kullandığınız abonelik değiştirme

Aboneliğinizi değiştirmek isterseniz kullanırsanız, ilk önce çalışmalıdır **Kaldır AzsRegistration** cmdlet, ardından doğru Azure PowerShell bağlamına oturum açtığınızdan emin olun ve son olarak çalıştırın **kümesi AzsRegistration**  tüm parametreleri değişti:

  ```powershell
  Remove-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```

#### <a name="change-the-billing-model-or-syndication-features"></a>Faturalama modeli ya da dağıtım özelliklerini değiştirme

Faturalama modeli veya yüklemeniz için dağıtım özellikleri değiştirmek istiyorsanız, yeni değerleri ayarlamak için kayıt işlevi çağırabilirsiniz. Geçerli kayıt kaldırın gerekmez:

  ```powershell
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```

### <a name="renew-or-change-registration-in-disconnected-environments"></a>Yenileme veya bağlantısı kesilmiş ortamlarda kayıt değiştirme

Güncelleştirme veya kaydınızı aşağıdaki durumlarda Yenile gerekir:

- Sonra kapasite tabanlı yıllık aboneliğinizi yenileyin.
- Faturalama modelinizi değiştirdiğinizde.
- Ne zaman kapasite tabanlı faturalama için değişiklikleri (Ekle/Kaldır düğümler) ölçeklendirin.

#### <a name="remove-the-activation-resource-from-azure-stack"></a>Azure yığınından etkinleştirme kaynak Kaldır

İlk Azure yığını ve Azure kayıt kaynağında etkinleştirme kaynak kaldırmak gerekir.  

Azure yığınında etkinleştirme kaynak kaldırmak için Azure yığın ortamınızda aşağıdaki PowerShell komutlarını çalıştırın:  

  ```Powershell
  Remove-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  ```

Ardından, Azure'da kayıt kaynağı kaldırmak için bağlı bir Azure emin olun bilgisayar, doğru Azure PowerShell bağlamına oturum açın ve aşağıda açıklandığı gibi uygun PowerShell komutlarını çalıştırın.

Kaynak oluşturmak için kullanılan kayıt belirteci kullanabilirsiniz:  

  ```Powershell
  $registrationToken = "<registration token>"
  Unregister-AzsEnvironment -RegistrationToken $registrationToken
  ```

Veya kayıt adı kullanabilirsiniz:

  ```Powershell
  $registrationName = "AzureStack-<Cloud Id of Azure Stack Environment>"
  Unregister-AzsEnvironment -RegistrationName $registrationName
  ```

### <a name="re-register-using-disconnected-steps"></a>Bağlantısı kesilmiş adımları kullanarak yeniden kaydolun

Şimdi tamamen bağlantısı kesilmiş bir senaryoda kaydı ve Azure yığın Ortam bağlantısı kesilmiş bir senaryoda kaydetmek için adımları yinelemeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[Market öğesi Azure'dan indirin](azure-stack-download-azure-marketplace-item.md)
