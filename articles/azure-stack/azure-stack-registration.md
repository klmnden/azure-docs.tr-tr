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
ms.date: 12/04/2018
ms.author: jeffgilb
ms.reviewer: brbartle
ms.openlocfilehash: 58dfb3f02b338d62fcfb10e4d8c1bc492cdacbda
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2018
ms.locfileid: "52890560"
---
# <a name="register-azure-stack-with-azure"></a>Azure Stack Azure ile kaydedin

Azure Stack Azure ile kaydetme, Azure Market öğelerini indirme ve ticaret verileri Microsoft'a raporlamaya ayarlamak için sağlar. Azure Stack kaydettikten sonra kullanım için Azure ticaret bildirilir ve kayıt için kullanılan abonelik altında görebilirsiniz.

Bu makaledeki bilgiler, Azure ile kayıt Azure Stack tümleşik sistemleri açıklanmaktadır. Azure ile ASDK kaydetme hakkında daha fazla bilgi için bkz: [Azure Stack kayıt](https://docs.microsoft.com/azure/azure-stack/asdk/asdk-register) ASDK belgelerinde.

> [!IMPORTANT]  
> Kayıt teklifi Market'te öğeleri dahil olmak üzere tam Azure Stack işlevleri desteklemek için gereklidir. Ayrıca, Azure Stack-,-Kullandıkça Ödeme modeli kullanılırken kaydedilmezse lisanslama koşulları ihlal olacaktır. Azure Stack lisanslama modelleri hakkında daha fazla bilgi için bkz. [sayfa satın alma](https://azure.microsoft.com/overview/azure-stack/how-to-buy/).

## <a name="prerequisites"></a>Önkoşullar

Kaydetmeden önce aşağıdakiler yerinde gerekir:

 - Kimlik bilgilerinizi doğrulayın
 - PowerShell dil modunu ayarlayın
 - Azure Stack için PowerShell'i yükleme
 - Azure Stack araçları indirin
 - Kayıt senaryonuz belirleme

### <a name="verify-your-credentials"></a>Kimlik bilgilerinizi doğrulayın

Azure Stack Azure ile kaydetmeden önce şunlara sahip olmalısınız:

- Abonelik kimliği için bir Azure aboneliği. Yalnızca EA, CSP veya CSP paylaşılan hizmetler abonelikleri kaydı için desteklenir. CSP'ler gerek karar vermek kullanılıp kullanılmayacağını [CSP veya APSS bir abonelik kullanın](azure-stack-add-manage-billing-as-a-csp.md#create-a-csp-or-apss-subscription).<br><br>Kimliğini almak için Azure'da oturum açın, sırayla **tüm hizmetleri**. Ardından, altında **genel** kategorisi seçin **abonelikleri**, kullanmak istediğiniz aboneliğe tıklayın ve altında **Essentials** abonelik kimliği bulabilirsiniz

  > [!Note]  
  > Almanya bulut abonelikleri şu anda desteklenmemektedir.

- Kullanıcı adı ve parola için aboneliğin sahibi olan bir hesap.

- Kullanıcı hesabı, Azure aboneliğine erişiminiz olması ve bu abonelikle dizinde kimlik uygulama ve hizmet sorumluları oluşturma izniniz gerekiyor.

- Azure Stack kaynak sağlayıcısına kayıtlı (Ayrıntılar için aşağıdaki Azure Stack kaynak sağlayıcısını kaydetme bölümüne bakın).

Kayıt sonrasında Azure Active Directory genel yönetici izni gerekli değildir. Ancak, bazı işlemler, genel yönetici kimlik bilgileri gerektirebilir. Örneğin, bir kaynak sağlayıcısı yükleyicisi betiği veya izin verilecek gerektiren yeni bir özelliktir. Geçici olarak hesap genel yönetici izinleri yeniden devreye sokmanız veya sahiplerinden biri olan ayrı bir genel yönetici hesabı kullanın *varsayılan sağlayıcı aboneliği*.

Bu gereksinimleri karşıladığını Azure aboneliğiniz yoksa, şunları yapabilirsiniz [buradan ücretsiz bir Azure hesabı oluşturun](https://azure.microsoft.com/free/?b=17.06). Azure aboneliğinize ücret ödemeden Azure Stack kaydetme artmasına neden olur.

### <a name="powershell-language-mode"></a>PowerShell dil modu

Azure Stack başarıyla kaydetmek için PowerShell dil modunu ayarlanmalıdır **FullLanguageMode**.  Geçerli dil modunu tam, yükseltilmiş bir PowerShell penceresi açın ve aşağıdaki PowerShell cmdlet'lerini çalıştırmak için ayarlandığını doğrulamak için:

```PowerShell  
$ExecutionContext.SessionState.LanguageMode
```

Çıktıyı döndürür olun **FullLanguageMode**. Herhangi bir dil modu döndürülen kayıt başka bir makinede çalıştırılması gerekir ya da dil modunu ayarlanması gerekir **FullLanguageMode** devam etmeden önce.

### <a name="install-powershell-for-azure-stack"></a>Azure Stack için PowerShell'i yükleme

Azure ile kaydetmek için Azure Stack için en son PowerShell kullanmanız gerekir.

En son sürümü zaten yüklü değilse bkz [Azure Stack için PowerShell yükleme](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).

### <a name="download-the-azure-stack-tools"></a>Azure Stack araçları indirin

Azure Stack araçları GitHub deposunu Azure Stack işlevselliği destekleyen PowerShell modüllerini içerir. Kayıt işlevleri dahil olmak üzere. İçeri aktarma ve kullanmak gereken kayıt işlemi sırasında **RegisterWithAzure.psm1** PowerShell modülü, Azure Stack örneğinizin Azure ile kaydetmek için Azure Stack araçları deposunda bulunamadı.

En son sürümü kullandığınızdan emin olmak için Azure Stack araçları var olan tüm sürümlerini silmeniz gerekir ve [Github'dan en son sürümünü indirin](azure-stack-powershell-download.md) Azure'la kaydetmeden önce.

### <a name="determine-your-registration-scenario"></a>Kayıt senaryonuz belirleme

Azure Stack dağıtımınıza olabilir *bağlı* veya *bağlantısı kesildi*.

 - **Bağlı**  
 İnternet'e ve azure'a bağlanabilmesi için Azure Stack dağıttığınız anlamına gelir bağlı. Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS), kimlik deposu ya da sahip. Bağlı bir dağıtım ile iki faturalandırma modelimiz seçebilirsiniz:-,-kullandıkça veya kapasite tabanlı.
    - [Azure'ı kullanarak bağlı bir Azure Stack kayıt **-,-kullandıkça** faturalandırma modeli](#register-connected-with-pay-as-you-go-billing)
    - [Azure'ı kullanarak bağlı bir Azure Stack kayıt **kapasite** faturalandırma modeli](#register-connected-with-capacity-billing)

 - **Bağlantısı kesildi**  
 Bağlantısı kesilmiş olan Azure dağıtım seçeneği, dağıtma ve Azure Stack internet bağlantısı olmadan kullanın. Ancak, bağlantısı kesilmiş bir dağıtım ile bir AD FS kimlik deposunu ve kapasite tabanlı faturalandırma modeli için sınırlı olursunuz.
    - [Bağlantısı kesilmiş bir Azure Stack kullanarak kaydolmanız **kapasite** faturalandırma modeli ](#register-disconnected-with-capacity-billing)

### <a name="determine-a-unique-registration-name-to-use"></a>Kullanmak için benzersiz kayıt adını belirleme 
Azure Stack Azure ile kaydettiğinizde, benzersiz kayıt adı sağlamanız gerekir. Azure Stack aboneliğinizi bir Azure kaydı ile ilişkilendirmek için kolay bir yolu, Azure Stack kullanmaktır **bulut kimliği**. 

> [!NOTE]
> Kapasite tabanlı bir faturalandırma modelini kullanarak azure Stack kayıtlarını sürece bu yıllık abonelik süresi dolduktan sonra yeniden kaydederken benzersiz adını değiştirmek gerekir, [süresi dolmuş kayıt silme](azure-stack-registration.md#change-the-subscription-you-use) ve yeniden kaydedin Azure.

Azure Stack dağıtımınıza bulut kimliği belirlemek için bir bilgisayarda yönetici ayrıcalıklı uç noktasına erişebildiğinden daha çalıştırırken aşağıdaki komutlar, PowerShell'i açın ve kaydı **Cloudıd** değeri: 

```powershell
Run: Enter-PSSession -ComputerName <privileged endpoint computer name> -ConfigurationName PrivilegedEndpoint
Run: get-azurestackstampinformation 
```

## <a name="register-connected-with-pay-as-you-go-billing"></a>Kullandıkça Öde faturalandırma ile bağlı kaydetme

Azure Stack-,-Kullandıkça Ödeme modelini kullanarak Azure ile kaydetmek için aşağıdaki adımları kullanın.

> [!Note]  
> Bu adımların tümünü (CESARETLENDİRİCİ) ayrıcalıklı uç noktasına erişimi olan bir bilgisayardan çalıştırmanız gerekir. CESARETLENDİRİCİ hakkında daha fazla ayrıntı için bkz: [Azure Stack'te ayrıcalıklı uç noktayı kullanarak](azure-stack-privileged-endpoint.md).

Azure ve internet bağlantılı ortamlar erişebilirsiniz. Bu ortamlar için Azure ile Azure Stack kaynak sağlayıcısını kaydedin ve ardından faturalama modelinizi yapılandırmanız gerekir.

1. Azure ile Azure Stack kaynak sağlayıcısını kaydetmek için PowerShell ISE yönetici olarak başlatın ve aşağıdaki PowerShell cmdlet'leriyle **EnvironmentName** parametre uygun Azure aboneliğini türüne (bkz: parametreleri aşağıdaki).

2. Azure Stack kaydetmek için kullandığınız bir Azure hesabı ekleyin. Hesap eklemek için şunu çalıştırın **Add-AzureRmAccount** cmdlet'i. Azure hesabı kimlik bilgilerinizi girmeniz istenir ve hesabınızın yapılandırmasına bağlı olarak 2 faktörlü kimlik doğrulaması kullanmak zorunda kalabilirsiniz.

   ```PowerShell
      Add-AzureRmAccount -EnvironmentName "<environment name>"
   ```

   | Parametre | Açıklama |  
   |-----|-----|
   | EnvironmentName | Azure bulut aboneliği ortam adı. Desteklenen ortam adları **AzureCloud**, **AzureUSGovernment**, veya Çin Azure aboneliği kullanıyorsanız **AzureChinaCloud**.  |

3. Birden fazla aboneliğiniz varsa, kullanmak istediğiniz seçmek için aşağıdaki komutu çalıştırın:  

   ```PowerShell  
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Azure aboneliğinizdeki Azure Stack kaynak sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın:

   ```PowerShell  
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. PowerShell ISE yönetici olarak başlatın ve gidin **kayıt** klasöründe **AzureStack araçları ana** dizini oluşturuldu, [Azure Stack Araçlarıindirildi](#bkmk_tools). İçeri aktarma **RegisterWithAzure.psm1** kullanarak PowerShell Modülü:

   ```PowerShell  
   Import-Module .\RegisterWithAzure.psm1
   ```

6. Ardından, aynı PowerShell oturumunda, doğru Azure PowerShell bağlamına oturum emin olun. Yukarıdaki Azure Stack kaynak sağlayıcısını kaydetmek için kullanılan Azure hesap budur. Çalıştırılacak Powershell:

   ```PowerShell  
      Add-AzureRmAccount -EnvironmentName "<environment name>"
   ```

   | Parametre | Açıklama |  
   |-----|-----|
   | EnvironmentName | Azure bulut aboneliği ortam adı. Desteklenen ortam adları **AzureCloud**, **AzureUSGovernment**, veya Çin Azure aboneliği kullanıyorsanız **AzureChinaCloud**.  |

7. Aynı PowerShell oturumunda çalıştırın **kümesi AzsRegistration** cmdlet'i. Çalıştırılacak PowerShell:  

   ```PowerShell  
   $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
   $RegistrationName = "<unique-registration-name>"
   Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -BillingModel PayAsYouUse `
      -RegistrationName $RegistrationName
   ```
   Set-AzsRegistration cmdlet'i hakkında daha fazla bilgi için bkz. [kayıt başvuru](#registration-reference).

  İşlem, 10 ile 15 dakika arasında sürer. Komut tamamlandığında bir ileti görürsünüz **"ortamınızı şimdi kaydedilir ve sağlanan parametreleri kullanarak etkinleştirildi."**

## <a name="register-connected-with-capacity-billing"></a>Kapasite faturalandırmayla bağlı kaydetme

Azure Stack-,-Kullandıkça Ödeme modelini kullanarak Azure ile kaydetmek için aşağıdaki adımları kullanın.

> [!Note]  
> Bu adımların tümünü (CESARETLENDİRİCİ) ayrıcalıklı uç noktasına erişimi olan bir bilgisayardan çalıştırmanız gerekir. CESARETLENDİRİCİ hakkında daha fazla ayrıntı için bkz: [Azure Stack'te ayrıcalıklı uç noktayı kullanarak](azure-stack-privileged-endpoint.md).

Azure ve internet bağlantılı ortamlar erişebilirsiniz. Bu ortamlar için Azure ile Azure Stack kaynak sağlayıcısını kaydedin ve ardından faturalama modelinizi yapılandırmanız gerekir.

1. Azure ile Azure Stack kaynak sağlayıcısını kaydetmek için PowerShell ISE yönetici olarak başlatın ve aşağıdaki PowerShell cmdlet'leriyle **EnvironmentName** parametre uygun Azure aboneliğini türüne (bkz: parametreleri aşağıdaki).

2. Azure Stack kaydetmek için kullandığınız bir Azure hesabı ekleyin. Hesap eklemek için şunu çalıştırın **Add-AzureRmAccount** cmdlet'i. Azure hesabı kimlik bilgilerinizi girmeniz istenir ve hesabınızın yapılandırmasına bağlı olarak 2 faktörlü kimlik doğrulaması kullanmak zorunda kalabilirsiniz.

   ```PowerShell  
      Add-AzureRmAccount -EnvironmentName "<environment name>"
   ```

   | Parametre | Açıklama |  
   |-----|-----|
   | EnvironmentName | Azure bulut aboneliği ortam adı. Desteklenen ortam adları **AzureCloud**, **AzureUSGovernment**, veya Çin Azure aboneliği kullanıyorsanız **AzureChinaCloud**.  |

3. Birden fazla aboneliğiniz varsa, kullanmak istediğiniz seçmek için aşağıdaki komutu çalıştırın:  

   ```PowerShell  
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Azure aboneliğinizdeki Azure Stack kaynak sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın:

   ```PowerShell  
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. PowerShell ISE yönetici olarak başlatın ve gidin **kayıt** klasöründe **AzureStack araçları ana** dizini oluşturuldu, [Azure Stack Araçlarıindirildi](#bkmk_tools). İçeri aktarma **RegisterWithAzure.psm1** kullanarak PowerShell Modülü:

  ```PowerShell  
  $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
  $RegistrationName = "<unique-registration-name>"
  Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -AgreementNumber <EA agreement number> `
      -BillingModel Capacity `
      -RegistrationName $RegistrationName
  ```
   > [!Note]  
   > Kullanım için UsageReportingEnabled parametreli raporlama devre dışı bırakabilirsiniz **kümesi AzsRegistration** cmdlet parametresini false olarak ayarlayarak. 
   
  Set-AzsRegistration cmdlet'i hakkında daha fazla bilgi için bkz. [kayıt başvuru](#registration-reference).

## <a name="register-disconnected-with-capacity-billing"></a>Kayıt kapasite faturalandırmayla bağlantısı kesildi

(İnternet bağlantısı olmayan) bağlantısı kesilmiş bir ortamda Azure Stack kaydediyorsanız, Azure Stack ortamından bir kayıt belirtecinizi almak ve sonra bu belirteci Azure'a bağlanabilir ve bir bilgisayara ihtiyacınız [PowerShell yüklü Azure Stack için](#bkmk_powershell).  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Azure Stack ortamından bir kayıt belirtecinizi almak

1. PowerShell ISE yönetici olarak başlatın ve gidin **kayıt** klasöründe **AzureStack araçları ana** dizini oluşturuldu, [Azure Stack Araçlarıindirildi](#bkmk_tools). İçeri aktarma **RegisterWithAzure.psm1** Modülü:  

   ```PowerShell  
   Import-Module .\RegisterWithAzure.psm1
   ```

2. Kayıt belirtecinizi almak için aşağıdaki PowerShell cmdlet'lerini çalıştırın:  

   ```Powershell
   $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential $YourCloudAdminCredential -UsageReportingEnabled:$False -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
   ```
   Get-AzsRegistrationToken cmdlet'i hakkında daha fazla bilgi için bkz. [kayıt başvuru](#registration-reference).

   > [!Tip]  
   > Kayıt belirteç için belirtilen dosya kaydedilir *$FilePathForRegistrationToken*. Dosya yolu veya dosya adı, günlüklerinizin bekletilme değiştirebilirsiniz.

3. Azure üzerinde kullanım için bu kayıt belirtecinizi kaydetme makine bağlı. Dosya veya metin $FilePathForRegistrationToken kopyalayabilirsiniz.

### <a name="connect-to-azure-and-register"></a>Azure ve kayıt bağlanma

Internet'e bağlı bilgisayarda RegisterWithAzure.psm1 modülünü içeri aktarın ve oturum açmak için doğru Azure Powershell bağlamı için aynı adımları gerçekleştirin. Ardından kayıt AzsEnvironment çağırın. Azure ile kaydetmek için kayıt belirtecini belirtin. Azure Stack aynı Azure abonelik Kimliğini kullanarak birden fazla örneğini kaydediyorsanız, benzersiz kayıt adı belirtin. Aşağıdaki cmdlet'i çalıştırın:

  ```PowerShell  
  $registrationToken = "<Your Registration Token>"
  $RegistrationName = "<unique-registration-name>"
  Register-AzsEnvironment -RegistrationToken $registrationToken  -RegistrationName $RegistrationName
  ```

İsteğe bağlı olarak, kayıt belirtecinizi içeren bir dosyaya işaret edecek şekilde Get-Content cmdlet'i kullanabilirsiniz:

  ```PowerShell  
  $registrationToken = Get-Content -Path '<Path>\<Registration Token File>'
  Register-AzsEnvironment -RegistrationToken $registrationToken -RegistrationName $RegistrationName
  ```

  > [!Note]  
  > Kayıt kaynak adı ve gelecekte başvurmak için kayıt belirtecini kaydedin.

### <a name="retrieve-an-activation-key-from-azure-registration-resource"></a>Azure kaydı kaynaktan etkinleştirme anahtarı alma

Ardından, Azure'da Register-AzsEnvironment sırasında oluşturulan kayıt kaynağıyla etkinleştirme anahtarı almak gerekir.

Etkinleştirme anahtarı almak için aşağıdaki PowerShell cmdlet'lerini çalıştırın:  

  ```Powershell
  $RegistrationResourceName = "<unique-registration-name>"
  $KeyOutputFilePath = "$env:SystemDrive\ActivationKey.txt"
  $ActivationKey = Get-AzsActivationKey -RegistrationName $RegistrationResourceName -KeyOutputFilePath $KeyOutputFilePath
  ```

  > [!Tip]  
  > Etkinleştirme anahtarı için belirtilen dosyada kaydedilmiş *$KeyOutputFilePath*. Dosya yolu veya dosya adı, günlüklerinizin bekletilme değiştirebilirsiniz.

### <a name="create-an-activation-resource-in-azure-stack"></a>Azure Stack'te bir etkinleştirme kaynağını oluşturma

Azure Stack ortamına, Get-AzsActivationKey oluşturulan etkinleştirme anahtarından dosya ya da metin döndürür. Ardından bu etkinleştirme anahtarı kullanarak Azure Stack'te bir etkinleştirme kaynağını oluşturacaksınız. Bir etkinleştirme kaynağını oluşturmak için aşağıdaki PowerShell cmdlet'lerini çalıştırın:  

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

1. Azure Stack oturum [Yönetim Portalı](https://docs.microsoft.com/azure/azure-stack/azure-stack-manage-portals#access-the-administrator-portal): https&#58;/ / adminportal. *&lt;bölge >. &lt;fqdn >*.
2. Seçin **tüm hizmetleri**ve ardından altındaki **Yönetim** kategorisi, select **Market Yönetim** > **azure'danEkle**.

(Örneğin, WordPress) azure'dan kullanılabilir öğeleri listesini görürseniz, etkinleştirme başarılı oldu. Ancak, bağlantısı kesilmiş ortamlarda, Azure Stack marketini Azure Market öğeleri tarafından görülmez.

> [!Note]  
> Kayıt tamamlandıktan sonra değil kaydetmek için etkin uyarı görünmeyecek.

## <a name="renew-or-change-registration"></a>Yenileme veya kaydı değiştirme

### <a name="renew-or-change-registration-in-connected-environments"></a>Yenileme veya bağlı ortamlarda kaydı değiştirme

Güncelleştirebilir veya aşağıdaki durumlarda kaydınızı yenilemeniz gerekir:

- Sonra kapasite tabanlı yıllık aboneliğinizi yenileyin.
- Faturalandırma modelinizin değiştirdiğinizde.
- Ne zaman kapasite kullanıma dayalı faturalandırma için değişiklikleri (düğüm ekleme/kaldırma) ölçeklendirin.

#### <a name="change-the-subscription-you-use"></a>Kullandığınız aboneliği Değiştir

Aboneliğinizi değiştirmek istiyorsanız, kullanırsanız, önce çalıştırmalısınız **Remove-AzsRegistration** cmdlet'ini doğru Azure PowerShell bağlamına oturum açtığınızdan emin olun ve son olarak çalıştırın **AzsRegistration kümesi**  değiştirilen dahil olmak üzere herhangi bir parametre ile \<faturalandırma modeli\>:

  ```PowerShell  
  Remove-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel <billing model> -RegistrationName $RegistrationName
  ```

#### <a name="change-the-billing-model-or-how-to-offer-features"></a>Faturalandırma modeli veya nasıl sunacağınızı özelliklerini değiştirme

Faturalandırma modeli veya nasıl sunacağınızı yüklemenizin özelliklerini değiştirmek istiyorsanız, yeni değerleri ayarlamak için kayıt işlevi çağırabilir. Geçerli kaydı silmeniz gerekmez:

  ```PowerShell  
  Set-AzsRegistration -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel <billing model> -RegistrationName $RegistrationName
  ```

### <a name="renew-or-change-registration-in-disconnected-environments"></a>Yenileme veya bağlantısı kesilmiş ortamlarda kaydı değiştirme

Güncelleştirebilir veya aşağıdaki durumlarda kaydınızı yenilemeniz gerekir:

- Sonra kapasite tabanlı yıllık aboneliğinizi yenileyin.
- Faturalandırma modelinizin değiştirdiğinizde.
- Ne zaman kapasite kullanıma dayalı faturalandırma için değişiklikleri (düğüm ekleme/kaldırma) ölçeklendirin.

#### <a name="remove-the-activation-resource-from-azure-stack"></a>Azure yığını etkinleştirme kaynağını kaldırın

İlk Azure Stack ve Azure kayıt kaynakta etkinleştirme kaynak kaldırmak gerekir.  

Azure Stack'te etkinleştirme kaynak kaldırmak için Azure Stack ortamınıza aşağıdaki PowerShell cmdlet'lerini çalıştırın:  

  ```Powershell
  Remove-AzsActivationResource -PrivilegedEndpointCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  ```

Ardından, Azure'da kayıt kaynağı kaldırmak için bağlı bir Azure üzerinde olduğundan emin olun, bilgisayar için doğru Azure PowerShell bağlamı oturum açın ve aşağıda açıklandığı gibi uygun bir PowerShell cmdlet'lerini çalıştırın.

Kaynak oluşturmak için kullanılan kayıt belirtecinizi kullanabilirsiniz:  

  ```Powershell
  $registrationToken = "<registration token>"
  Unregister-AzsEnvironment -RegistrationToken $registrationToken
  ```

Veya, kayıt adı kullanabilirsiniz:

  ```Powershell
  $registrationName = "AzureStack-<unique-registration-name>"
  Unregister-AzsEnvironment -RegistrationName $registrationName
  ```

### <a name="re-register-using-disconnected-steps"></a>Bağlantısı kesilmiş adımları kullanarak yeniden kaydolun

Artık tamamen bağlantısı kesilmiş bir senaryoda kaydı ve bir Azure Stack ortamına bağlı olmayan bir senaryoda kaydetmek için adımları yinelemeniz gerekir.

### <a name="disable-or-enable-usage-reporting"></a>Kullanım raporlama etkinleştirmek veya devre dışı

Reporting ile kullanım kapasite faturalandırma modeli kullandığınız Azure Stack ortamlarda, kapatmak **UsageReportingEnabled** parametresi kullanarak **kümesi AzsRegistration** veya  **Get-AzsRegistrationToken** cmdlet'leri. Azure Stack, varsayılan olarak kullanım ölçümleri raporları. Kapasite kullanır veya bağlantısı kesilmiş bir ortam destekleyen işleçlerle kullanım bildirimini devre dışı bırak gerekecektir.

#### <a name="with-a-connected-azure-stack"></a>Bağlı bir Azure Stack ile

   ```PowerShell  
   $CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials> -Message "Enter the cloud domain credentials to access the privileged endpoint."
   $RegistrationName = "<unique-registration-name>"
   Set-AzsRegistration `
      -PrivilegedEndpointCredential $CloudAdminCred `
      -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
      -BillingModel Capacity
      -RegistrationName $RegistrationName
   ```

#### <a name="with-a-disconnected-azure-stack"></a>Bağlantısı kesilmiş bir Azure Stack ile

1. Kayıt belirtecini değiştirmek için aşağıdaki PowerShell cmdlet'lerini çalıştırın:  

   ```Powershell
   $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
   $RegistrationToken = Get-AzsRegistrationToken -PrivilegedEndpointCredential -UsageReportingEnabled:$False
   $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<EA agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
   ```

   > [!Tip]  
   > Kayıt belirteç için belirtilen dosya kaydedilir *$FilePathForRegistrationToken*. Dosya yolu veya dosya adı, günlüklerinizin bekletilme değiştirebilirsiniz.

2. Azure üzerinde kullanım için bu kayıt belirtecinizi kaydetme makine bağlı. Dosya veya metin $FilePathForRegistrationToken kopyalayabilirsiniz.

## <a name="move-a-registration-resource"></a>Kayıt kaynak taşıma
Kayıt kaynak aynı abonelik altında kaynak grupları arasında taşıma **olduğu** tüm ortamlar için desteklenir. Her iki aboneliğin aynı iş ortağı kimliğine çözümlediğinizde ancak kayıt kaynak abonelikler arasında taşıma yalnızca CSP'ler için desteklenir Kaynakları yeni kaynak grubuna taşıma hakkında daha fazla bilgi için bkz. [kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).

## <a name="registration-reference"></a>Kayıt başvurusu

### <a name="set-azsregistration"></a>Set-AzsRegistration

Azure Stack Azure ile kaydedin ve etkinleştirme veya teklif Market ve kullanım raporlama öğelerinin devre dışı bırakmak için Set-AzsRegistration kullanabilirsiniz.

Cmdlet'i çalıştırmak için ihtiyacınız vardır:
- Herhangi bir türde genel bir Azure aboneliği.
- Ayrıca Azure PowerShell için sahip veya bu abonelik için katkıda bulunanı olan bir hesapla oturum açmanız gerekir.

```PowerShell
    Set-AzsRegistration [-PrivilegedEndpointCredential] <PSCredential> [-PrivilegedEndpoint] <String> [[-AzureContext]
    <PSObject>] [[-ResourceGroupName] <String>] [[-ResourceGroupLocation] <String>] [[-BillingModel] <String>]
    [-MarketplaceSyndicationEnabled] [-UsageReportingEnabled] [[-AgreementNumber] <String>] [[-RegistrationName]
    <String>] [<CommonParameters>]
   ```

| Parametre | Tür | Açıklama |
|-------------------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| PrivilegedEndpointCredential | PSCredential | İçin kullanılan kimlik bilgileri [ayrıcalıklı uç noktasına erişmek](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). Kullanıcı adı şu biçimdedir **AzureStackDomain\CloudAdmin**. |
| PrivilegedEndpoint | Dize | Dağıtım görevleri ile günlük toplama ve diğer posta gibi özellikler sağlayan bir önceden yapılandırılmış Uzaktan PowerShell Konsolu. Daha fazla bilgi edinmek için bkz [ayrıcalıklı uç noktayı kullanarak](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) makalesi. |
| AzureContext | PSObject |  |
| ResourceGroupName | Dize |  |
| ResourceGroupLocation | Dize |  |
| BillingModel | Dize | Faturalandırma modeli, aboneliğinizin kullanır. İzin verilen bu parametre için değerler: kapasite PayAsYouUse ve geliştirme. |
| MarketplaceSyndicationEnabled | True/False | Portalda Market yönetim özelliği kullanılabilir olup olmadığını belirler. İnternet bağlantısı ile kaydetme gerekiyorsa true olarak ayarlayın. Bağlantısı kesilmiş ortamlarda kaydediliyor false olarak ayarlayın. Bağlantısı kesilmiş kayıtları için [çevrimdışı dağıtım aracı](azure-stack-download-azure-marketplace-item.md#disconnected-or-a-partially-connected-scenario) Market öğelerini indirme için kullanılabilir. |
| UsageReportingEnabled | True/False | Azure Stack, varsayılan olarak kullanım ölçümleri raporları. Kapasite kullanır veya bağlantısı kesilmiş bir ortam destekleyen işleçlerle kullanım bildirimini devre dışı bırak gerekecektir. İzin verilen bu parametre için değerler: True, False. |
| AgreementNumber değeri | Dize |  |
| registrationName | Dize | Kayıt betiği Azure Stack birden fazla örneğinde aynı Azure abonelik kimliği kullanarak çalıştırıyorsanız kayıt için benzersiz bir ad ayarlayın. Parametrenin varsayılan değeri **AzureStackRegistration**. Azure Stack birden fazla örneğinde aynı adı kullanın, ancak komut başarısız olur. |

### <a name="get-azsregistrationtoken"></a>Get-AzsRegistrationToken

Get-AzsRegistrationToken giriş parametrelerini kayıt belirtecinizi oluşturur.

```PowerShell  
    Get-AzsRegistrationToken [-PrivilegedEndpointCredential] <PSCredential> [-PrivilegedEndpoint] <String>
    [-BillingModel] <String> [[-TokenOutputFilePath] <String>] [-UsageReportingEnabled] [[-AgreementNumber] <String>]
    [<CommonParameters>]
```
| Parametre | Tür | Açıklama |
|-------------------------------|--------------|-------------|
| PrivilegedEndpointCredential | PSCredential | İçin kullanılan kimlik bilgileri [ayrıcalıklı uç noktasına erişmek](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). Kullanıcı adı şu biçimdedir **AzureStackDomain\CloudAdmin**. |
| PrivilegedEndpoint | Dize |  Dağıtım görevleri ile günlük toplama ve diğer posta gibi özellikler sağlayan bir önceden yapılandırılmış Uzaktan PowerShell Konsolu. Daha fazla bilgi edinmek için bkz [ayrıcalıklı uç noktayı kullanarak](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint) makalesi. |
| AzureContext | PSObject |  |
| ResourceGroupName | Dize |  |
| ResourceGroupLocation | Dize |  |
| BillingModel | Dize | Faturalandırma modeli, aboneliğinizin kullanır. İzin verilen bu parametre için değerler: kapasite PayAsYouUse ve geliştirme. |
| MarketplaceSyndicationEnabled | True/False |  |
| UsageReportingEnabled | True/False | Azure Stack, varsayılan olarak kullanım ölçümleri raporları. Kapasite kullanır veya bağlantısı kesilmiş bir ortam destekleyen işleçlerle kullanım bildirimini devre dışı bırak gerekecektir. İzin verilen bu parametre için değerler: True, False. |
| AgreementNumber değeri | Dize |  |


## <a name="next-steps"></a>Sonraki adımlar

[Azure Market öğelerini indirme](azure-stack-download-azure-marketplace-item.md)
