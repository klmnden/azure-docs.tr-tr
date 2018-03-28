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
ms.date: 03/26/2018
ms.author: jeffgilb
ms.reviewer: avishwan
ms.openlocfilehash: 1dc3d9a96b9b27927cc8cc66b5e80987fba4f8ea
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="register-azure-stack-with-azure"></a>Azure ile Azure yığın kaydedin
Kaydetme [Azure yığın](azure-stack-poc.md) Azure ile Azure Market öğesi indirmek ve ticaret veri geri Microsoft'a raporlama ayarlamak için sağlar. Azure yığın kaydettikten sonra kullanım için Azure ticaret bildirilir ve kayıt için kullanılan abonelik altında görebilirsiniz. 

> [!IMPORTANT]
> Kayıt, ödeme olarak,-kullanımlı fatura modelini seçerseniz zorunludur. Aksi durumda, kullanım aksi raporlanmayacak gibi Azure yığın dağıtımına lisans koşulları ihlal olur.

## <a name="prerequisites"></a>Önkoşullar
Azure ile Azure yığın kaydetmeden önce şunlara sahip olmalısınız:

- Bir Azure aboneliği için abonelik kimliği. Kimliği almak için Azure oturumu açın, **daha fazla hizmet** > **abonelikleri**, kullanmak istediğiniz aboneliği tıklatın ve altında **Essentials** bulabilirsiniz Abonelik kimliği 

  > [!NOTE]
  > Çin, Almanya ve ABD devlet kurumları abonelikler şu anda desteklenmemektedir. 

- Kullanıcı adı ve parola abonelik sahibi olan bir hesap için (MSA/2FA hesapları desteklenir)
- Azure yığın kaynak sağlayıcısı kayıtlı (Ayrıntılar için aşağıdaki Azure yığın kaynak sağlayıcısını Kaydet bölümüne bakın).

Bu gereksinimleri karşılayan bir Azure aboneliğiniz yoksa, şunları yapabilirsiniz [buradan ücretsiz bir Azure hesabı oluşturma](https://azure.microsoft.com/free/?b=17.06). Azure yığın kaydetme, Azure aboneliğinizin üzerinde hiçbir ücret doğurur.

### <a name="bkmk_powershell"></a>PowerShell için Azure yığın yükle
Azure ile kaydetmek için Azure yığınının son PowerShell kullanmanız gerekir.

Zaten yüklü değilse, [PowerShell için Azure Yığın Yükle](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install). 

### <a name="bkmk_tools"></a>Azure yığın araçları yükleyin
Azure yığın araçları GitHub deposunu Azure yığın işlevselliğini destekleyen PowerShell modülleri içerir; Kayıt işlevleri dahil olmak üzere. Kayıt sırasında aktarıp RegisterWithAzure.psm1 PowerShell modülü kullanabilirsiniz gerekecek işlemi Azure yığın örneğinizi Azure ile kaydetmek için Azure yığın araçları deposundaki buldu. 

En son sürümünü kullandığınızdan emin olmak için mevcut Azure yığın araçları sürümlerini silmeniz gerekir ve [Github'dan en son sürümü karşıdan](azure-stack-powershell-download.md) Azure ile kaydetmeden önce.

## <a name="register-azure-stack-in-connected-environments"></a>Azure yığın bağlı ortamlarda kaydetme
Bağlantılı ortamlar, internet'e ve Azure erişebilir. Bu ortamlar için Azure yığın kaynak sağlayıcısı Azure ile kaydedin ve faturalama modelinizi yapılandırmanız gerekir.

> [!NOTE]
> Tüm adımları ayrıcalıklı uç noktasına erişimi olan bir bilgisayardan çalıştırmanız gerekir. 

### <a name="register-the-azure-stack-resource-provider"></a>Azure yığın kaynak sağlayıcısını Kaydet
Azure ile Azure yığın kaynak sağlayıcısını kaydetmek için Powershell ISE yönetici olarak başlatın ve aşağıdaki PowerShell komutlarını kullanın. Bu komutlar aşağıdakileri yapar:
- Kullanılan ve ayarlamak için bir Azure aboneliğin sahibi oturum ister **EnvironmentName** parametresi **AzureCloud**.
- Azure kaynak sağlayıcısı kaydetme **Microsoft.AzureStack**.

1. Azure yığın kaydetmek için kullandığınız Azure hesabı ekleyin. Hesap eklemek için çalıştırın **Add-AzureRmAccount** cmdlet'i. Azure genel yönetici hesabı kimlik bilgilerinizi girmeniz istenir ve hesabınızın yapılandırmasına bağlı olarak 2 öğeli kimlik doğrulama kullanmak zorunda kalabilirsiniz.

   ```Powershell
      Add-AzureRmAccount -EnvironmentName AzureCloud
   ```

2. Birden çok aboneliğiniz varsa, kullanmak istediğiniz birini seçmek için aşağıdaki komutu çalıştırın:  

   ```powershell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

3. Azure aboneliğinizde Azure yığın kaynak sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın:

   ```Powershell
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
$CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials>  -Message "Enter the credentials to access the privileged endpoint"
Set-AzsRegistration `
    -CloudAdminCredential $CloudAdminCred `
    -PrivilegedEndpoint <PrivilegedEndPoint computer name> `
    -BillingModel PayAsYouUse
```

|Parametre|Açıklama|
|-----|-----|
|CloudAdminCredential|PowerShell nesnesi ayrıcalıklı uç noktasına erişmek için kullanılan kimlik bilgileri (kullanıcı adı ve parola) içeriyor.|
|PrivilegedEndpoint|Dağıtım görevleri günlük toplama ve diğer posta gibi özellikleriyle sağlayan bir önceden yapılandırılmış Uzaktan PowerShell Konsolu. Daha fazla bilgi edinmek için bkz [kullanan ayrıcalıklı uç noktasını](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint#access-the-privileged-endpoint) makalesi.|
|BillingModel|Aboneliğinizi kullanan faturalama modeli. Bu parametre için değer izin verilen: kapasite, PayAsYouUse ve geliştirme.|

### <a name="register-azure-stack-with-azure-using-the-capacity-billing-model"></a>Azure yığın kapasite fatura modelini kullanarak Azure ile kaydedin
Ödeme olarak,-kullanımlı fatura modelini kullanarak kayıt için kullanılan aynı yönergeleri izleyin, ancak altında kapasite satın anlaşma numarası ekleyin ve değiştirme `BillingModel` parametresi **kapasite**. Diğer tüm parametreleri değiştirilmez.

Çalıştırılacak PowerShell:
```powershell
$AzureContext = Get-AzureRmContext
$CloudAdminCred = Get-Credential -UserName <Privileged endpoint credentials>  -Message "Enter the credentials to access the privileged endpoint"
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
1. Azure yığına oturum [Yönetici portalı](https://docs.microsoft.com/azure/azure-stack/azure-stack-manage-portals#access-the-administrator-portal): https&#58;/ / adminportal. *&lt;bölge >. &lt;fqdn >*.
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
## <a name="next-steps"></a>Sonraki adımlar

[Market öğesi Azure'dan indirin](azure-stack-download-azure-marketplace-item.md)