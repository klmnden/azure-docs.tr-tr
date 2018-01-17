---
title: "Azure yığınının Azure kaydı tümleşik sistemleri | Microsoft Docs"
description: "Azure yığın Azure bağlı çok düğümlü dağıtımlar için Azure kayıt işlemini açıklar."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: 03a6ce77eed16cbc5d2fe46094b0b6ac7fbc022e
ms.sourcegitcommit: 5108f637c457a276fffcf2b8b332a67774b05981
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="register-azure-stack-with-azure"></a>Azure ile Azure yığın kaydedin
Market öğesi Azure'dan karşıdan yüklemek ve ticari veri geri Microsoft'a raporlama ayarlamak için Azure ile Azure yığın kaydedebilirsiniz. Azure yığın kaydettikten sonra kullanım için Azure ticaret bildirilir. Kayıt için kullanılan abonelik altında görebilirsiniz.

> [!IMPORTANT]
> Kayıt, ödeme olarak,-kullanımlı fatura modelini seçerseniz zorunludur. Aksi durumda, kullanım aksi raporlanmayacak gibi Azure yığın dağıtımına lisans koşulları ihlal olur.

## <a name="before-you-register-azure-stack-with-azure"></a>Önce Azure yığın Azure ile kaydedin
Azure ile Azure yığın kaydetmeden önce şunlara sahip olmalısınız:

- Bir Azure aboneliği için abonelik kimliği. Kimliği almak için Azure oturumu açın, **daha fazla hizmet** > **abonelikleri**, kullanmak istediğiniz aboneliği tıklatın ve altında **Essentials** bulabilirsiniz Abonelik kimliği 

  > [!NOTE]
  > Çin, Almanya ve US government abonelikler şu anda desteklenmiyor. 

- Kullanıcı adı ve parola abonelik sahibi olan bir hesap için (MSA/2FA hesapları desteklenir)
- *Azure yığın 1712 güncelleştirme sürümünü (180106.1) ile başlayan gerekli değil*: Azure aboneliği için Azure AD. Azure portalının sağ üst köşedeki adresindeki, avatar üzerine gelerek, bu dizin Azure içinde bulabilirsiniz. 
- Azure yığın kaynak sağlayıcısı kayıtlı (Ayrıntılar için aşağıdaki Azure yığın kaynak sağlayıcısını Kaydet bölümüne bakın)

Bu gereksinimleri karşılayan bir Azure aboneliğiniz yoksa, şunları yapabilirsiniz [buradan ücretsiz bir Azure hesabı oluşturma](https://azure.microsoft.com/free/?b=17.06). Azure yığın kaydetme, Azure aboneliğinizin üzerinde hiçbir ücret doğurur.

### <a name="bkmk_powershell"></a>PowerShell için Azure yığın yükle
Sistem Azure ile kaydetmek için Azure yığınının son PowerShell kullanmanız gerekir.

Zaten yüklü değilse, [PowerShell için Azure Yığın Yükle](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install). 

### <a name="bkmk_tools"></a>Azure yığın araçları yükleyin
Azure yığın araçları GitHub deposunu Azure yığın işlevselliğini destekleyen PowerShell modülleri içerir; Kayıt işlevleri dahil olmak üzere. Kayıt sırasında aktarıp RegisterWithAzure.psm1 PowerShell modülü kullanabilirsiniz gerekecek işlemi Azure yığın örneğinizi Azure ile kaydetmek için Azure yığın araçları deposundaki buldu. 

```powershell
# Change directory to the root directory. 
cd \

# Download the tools archive.
  invoke-webrequest `
  https://github.com/Azure/AzureStack-Tools/archive/master.zip `
  -OutFile master.zip

# Expand the downloaded files.
  expand-archive master.zip `
  -DestinationPath . `
  -Force

# Change to the tools directory.
  cd AzureStack-Tools-master
```

## <a name="register-azure-stack-in-connected-environments"></a>Azure yığın bağlı ortamlarda kaydetme
Bağlantılı ortamlar, internet'e ve Azure erişebilir. Bu ortamlar için Azure yığın kaynak sağlayıcısı Azure ile kaydedin ve faturalama modelinizi yapılandırmanız gerekir.

### <a name="register-the-azure-stack-resource-provider"></a>Azure yığın kaynak sağlayıcısını Kaydet
Azure ile Azure yığın kaynak sağlayıcısını kaydetmek için Powershell ISE yönetici olarak başlatın ve aşağıdaki PowerShell komutlarını kullanın. Bu komutlar aşağıdakileri yapar:
- Kullanılan ve ayarlamak için bir Azure aboneliğin sahibi oturum ister `EnvironmentName` parametresi **AzureCloud**.
- Azure kaynak sağlayıcısı kaydetme **Microsoft.AzureStack**.

Çalıştırılacak PowerShell:

```powershell
Login-AzureRmAccount -EnvironmentName "AzureCloud"
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack 
```

### <a name="register-azure-stack-with-azure-using-the-pay-as-you-use-billing-model"></a>Azure yığın ödeme olarak,-kullanımlı fatura modelini kullanarak Azure ile kaydedin
Azure yığın ödeme olarak,-kullanımlı fatura modelini kullanarak Azure ile kaydetmek için aşağıdaki adımları kullanın.

PowerShell ISE yönetici olarak başlatın ve gidin **kayıt** klasöründe **AzureStack araçları ana** dizin oluşturulur, [Azure yığın Araçlarıindirilen](#bkmk_tools). İçeri aktarma **RegisterWithAzure.psm1** PowerShell kullanarak Modülü: 

Çalıştırılacak PowerShell:

```powershell
Import-Module .\RegisterWithAzure.psm1
```
Ardından, aynı PowerShell oturumunda çalıştırın **kümesi AzsRegistration** cmdlet'i. Kimlik bilgileri istendiğinde, Azure abonelik sahibi belirtin.  

Çalıştırılacak PowerShell:

```powershell
$AzureContext = Get-AzureRmContext
$CloudAdminCred = Get-Credential -UserName <Azure subscription owner>  -Message "Enter the cloud domain credentials to access the privileged endpoint"
Set-AzsRegistration `
    -CloudAdminCredential $CloudAdminCred `
    -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
    -BillingModel PayAsYouUse
```

|Parametre|Açıklama|
|-----|-----|
|CloudAdminCredential|Azure aboneliğinin sahibi kimlik bilgilerini (kullanıcı adı ve parola) içeren PowerShell nesnesi.|
|PrivilegedEndpoint|Dağıtım görevleri günlük toplama ve diğer posta gibi özellikleriyle sağlayan bir önceden yapılandırılmış Uzaktan PowerShell Konsolu. Daha fazla bilgi edinmek için bkz [kullanan ayrıcalıklı uç noktasını](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint#access-the-privileged-endpoint) makalesi.|
|BillingModel|Aboneliğinizi kullanan faturalama modeli. Bu parametre için değer izin verilen: kapasite, PayAsYouUse ve geliştirme.|

### <a name="register-azure-stack-with-azure-using-the-capacity-billing-model"></a>Azure yığın kapasite fatura modelini kullanarak Azure ile kaydedin
Ödeme olarak,-kullanımlı fatura modelini kullanarak kayıt için kullanılan aynı yönergeleri izleyin, ancak altında kapasite satın anlaşma numarası ekleyin ve değiştirme `BillingModel` parametresi **kapasite**. Diğer tüm parametreleri değiştirilmez.

Çalıştırılacak PowerShell:
```powershell
$AzureContext = Get-AzureRmContext
$CloudAdminCred = Get-Credential -UserName <Azure subscription owner>  -Message "Enter the cloud domain credentials to access the privileged endpoint"
Set-AzsRegistration `
    -CloudAdminCredential $CloudAdminCred `
    -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
    -AgreementNumber <EA agreement number> `
    -BillingModel Capacity
```

## <a name="register-azure-stack-in-disconnected-environments"></a>Bağlantısı kesilmiş ortamlarda Azure yığın kaydetme 
*Bu bölümdeki bilgiler, Azure yığın 1712 güncelleştirme sürümünü (180106.1) ile başlayan uygular ve daha önceki sürümlerde desteklenmiyor.*

(İnternet bağlantısı olmayan) bağlantısı kesilmiş bir ortamda Azure yığın kaydediyorsanız, Azure yığın ortamından bir kayıt belirtecinizi almak ve ardından bu belirtece Azure'a bağlanabilir ve olan bir bilgisayarda gereken [PowerShell Azure yüklü yığınının](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Bir kayıt Azure yığın ortamından belirteci alma
  1. Kayıt belirtecinizi almak için aşağıdaki PowerShell komutlarını çalıştırın:  

     ```Powershell
        $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
        $RegistrationToken = Get-AzsRegistrationToken -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<your agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
      ```
      > [!TIP]  
      > Kayıt belirtecinizi için belirtilen dosyasında kaydedilen *$env:SystemDrive\RegistrationToken.txt*.

  2. Bu kayıt belirtecinizi Azure üzerinde kullanılmak üzere kaydetme makine bağlı.


### <a name="connect-to-azure-and-register"></a>Azure ve kayıt bağlanmak
PowerShell ISE yönetici olarak başlatın ve gidin **kayıt** klasöründe **AzureStack araçları ana** dizin oluşturulur, [Azure yığın Araçlarıindirilen](#bkmk_tools). İçeri aktarma **RegisterWithAzure.psm1** Modülü: 

Çalıştırılacak PowerShell:
```powershell
Import-Module .\RegisterWithAzure.psm1
```
Ardından, aynı PowerShell oturumunda Azure ile kaydetmek için kayıt belirtecini belirtin:

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
> Kayıt kaynak adı ya da daha sonra başvurmak için kayıt belirteci kaydedin.

## <a name="verify-azure-stack-registration"></a>Azure yığın kayıt doğrulayın
Azure yığın Azure ile başarılı bir şekilde kaydettirildi doğrulamak için aşağıdaki adımları kullanın.
1. Azure yığına oturum [Yönetici portalı](https://docs.microsoft.com/azure/azure-stack/azure-stack-manage-portals#access-the-administrator-portal): https &#58; / / adminportal. *&lt;bölge >. &lt;fqdn >*.
2. Tıklatın **daha fazla hizmet** > **Market Yönetim** > **Azure'dan eklemek**.

Bir öğe (örneğin, WordPress) azure'dan kullanılabilir listesi görürseniz, etkinleştirme başarılı oldu.

> [!NOTE]
> Kayıt tamamlandıktan sonra değil kaydettirmek için etkin uyarı artık görünür.

## <a name="renew-or-change-registration"></a>Yenileme veya kayıt değiştirme
Güncelleştirme veya kaydınızı aşağıdaki durumlarda Yenile gerekir:
- Sonra kapasite tabanlı yıllık aboneliğinizi yenileyin.
- Faturalama modelinizi değiştirdiğinizde.
- Ne zaman kapasite tabanlı faturalama için değişiklikleri (Ekle/Kaldır düğümler) ölçeklendirin.

### <a name="change-the-subscription-you-use"></a>Kullandığınız abonelik değiştirme
Aboneliğinizi değiştirmek isterseniz kullanırsanız, ilk önce çalışmalıdır **Kaldır AzsRegistration** cmdlet, ardından doğru Azure PowerShell bağlamına oturum açtığınızdan emin olun ve son olarak çalıştırın **kümesi AzsRegistration**  tüm parametreleri değişti:

```powershell
Remove-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
Set-AzureRmContext -SubscriptionId $NewSubscriptionId
Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
```

### <a name="change-the-billing-model-or-syndication-features"></a>Faturalama modeli ya da dağıtım özelliklerini değiştirme
Faturalama modeli veya yüklemeniz için dağıtım özellikleri değiştirmek istiyorsanız, yeni değerleri ayarlamak için kayıt işlevi çağırabilirsiniz. Geçerli kayıt kaldırın gerekmez: 

```powershell
Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
```

## <a name="remove-a-registered-resource"></a>Kayıtlı bir kaynağı kaldırırsanız
Bir kayıt kaldırmak istediğiniz sonra kullanmalısınız **UnRegister-AzsEnvironment** cmdlet'i ve kayıt kaynağı adı veya kayıt geçişinde simge, kullanılan **Register-AzsEnvironment**.

Bir kaynak adı kullanarak bir kaydını kaldırmak için:

```Powershell    
UnRegister-AzsEnvironment -RegistrationName "*Name of the registration resource*"
```
Bir kayıt belirteci kullanarak bir kaydını kaldırmak için:

```Powershell
$registrationToken = "*Your copied registration token*"
UnRegister-AzsEnvironment -RegistrationToken $registrationToken
```

