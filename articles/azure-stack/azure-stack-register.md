---
title: "Azure yığın kaydetme | Microsoft Docs"
description: "Azure yığını, Azure aboneliğiniz ile kaydedin."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/5/2018
ms.author: jeffgilb
ms.openlocfilehash: c2edafbf483692d5a11771268a1755c11b74521f
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="register-azure-stack-with-your-azure-subscription"></a>Azure yığını, Azure aboneliğiniz ile kaydetme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Kayıt yaptırabilirsiniz [Azure yığın](azure-stack-poc.md) Azure Market öğesi Azure'dan karşıdan yüklemek ve ticari veri geri Microsoft'a raporlama ayarlamak için ile.

> [!NOTE]
>Market dağıtım ve kullanım raporlama gibi önemli Azure yığın işlevselliğini sınamak için sağladığından kayıt önerilir. Azure yığın kaydettikten sonra kullanım için Azure ticaret bildirilir. Kayıt için kullanılan abonelik altında görebilirsiniz. Azure yığın Geliştirme Seti kullanıcıların bunlar rapor herhangi kullanım için ücretlendirilmezsiniz.


## <a name="get-azure-subscription"></a>Azure aboneliği edinme

Azure ile Azure yığın kaydetmeden önce şunlara sahip olmalısınız:

- Bir Azure aboneliği için abonelik kimliği. Kimliği almak için Azure oturumu açın, **daha fazla hizmet** > **abonelikleri**, kullanmak istediğiniz aboneliği tıklatın ve altında **Essentials** bulabilirsiniz **Abonelik kimliği**. Çin, Almanya ve US government abonelikler şu anda desteklenmiyor.
- Kullanıcı adı ve parola abonelik sahibi olan bir hesap için (MSA/2FA hesapları desteklenir).
- *Azure yığın 1712 güncelleştirme sürümünü (1.0.180103.2) ile başlayan gerekli değil:* Azure Active Directory Azure aboneliği için. Azure portalının sağ üst köşedeki adresindeki, avatar üzerine gelerek, bu dizin Azure içinde bulabilirsiniz.

Bu gereksinimleri karşılayan bir Azure aboneliğiniz yoksa, şunları yapabilirsiniz [buradan ücretsiz bir Azure hesabı oluşturma](https://azure.microsoft.com/en-us/free/?b=17.06). Azure yığın kaydetme, Azure aboneliğinizin üzerinde hiçbir ücret doğurur.

## <a name="register-azure-stack-with-azure"></a>Azure ile Azure yığın kaydedin  
> [!NOTE]
> Tüm adımları ayrıcalıklı uç noktasına erişimi olan bir makineden çalıştırmanız gerekir. Azure yığın Geliştirme Seti için ana bilgisayar olacaktır. Tümleşik bir sistem kullanıyorsanız, Azure yığın operatörünüze başvurun.
>

1. Bir yönetici olarak bir PowerShell konsolu açın ve [PowerShell için Azure Yığın Yükle](azure-stack-powershell-install.md).  

2. Azure yığın kaydetmek için kullandığınız Azure hesabı ekleyin. Hesap eklemek için çalıştırın `Add-AzureRmAccount` EnvironmentName parametre kümesi cmdlet'iyle **AzureCloud**. Azure hesabı kimlik bilgilerinizi girmeniz istenir ve hesabınızın yapılandırmasına bağlı olarak 2 öğeli kimlik doğrulama kullanmak zorunda kalabilirsiniz.

   ```Powershell
   Add-AzureRmAccount -EnvironmentName "AzureCloud"
   ```

3. Birden çok aboneliğiniz varsa, kullanmak istediğiniz birini seçmek için aşağıdaki komutu çalıştırın:  

   ```powershell
      Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   ```

4. Azure aboneliğinizde Azure yığın kaynak sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın:

   ```Powershell
   Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AzureStack
   ```

5. Varolan kayda karşılık gelen PowerShell modülleri sürümlerini silecek ve [Github'dan en son sürümünü indirme](azure-stack-powershell-download.md).  

6. Önceki adımda oluşturduğunuz "AzureStack araçları yönetici" dizininden "Kayıt" klasörüne gidin ve ".\RegisterWithAzure.psm1" modülünü içeri aktarın:  

   ```powershell
   Import-Module .\RegisterWithAzure.psm1
   ```

7. Aynı PowerShell oturumunda, aşağıdaki komut dosyasını çalıştırın: kimlik bilgileri istendiğinde belirtin `azurestack\cloudadmin` kullanıcıyı ve parolayı yerel yönetici için dağıtım sırasında kullandığınız ile aynıdır.  

   ```powershell
   $AzureContext = Get-AzureRmContext
   $CloudAdminCred = Get-Credential -UserName AZURESTACK\CloudAdmin -Message "Enter the cloud domain credentials to access the privileged endpoint"
   Set-AzsRegistration `
       -CloudAdminCredential $CloudAdminCred `
       -PrivilegedEndpoint AzS-ERCS01 `
       -BillingModel Development
   ```

   | Parametre | Açıklama |  
   |--------|-------------|
   | CloudAdminCredential | İçin kullanılan bulut etki alanı kimlik bilgileri [ayrıcalıklı uç noktasına erişmek](azure-stack-privileged-endpoint.md#access-the-privileged-endpoint). Kullanıcı adı biçimindedir  **\<Azure yığın etki alanı\>\cloudadmin**. Geliştirme Seti için kullanıcı adı ayarlamak **azurestack\cloudadmin**. Tümleşik bir sistem kullanıyorsanız, bu değer almak için Azure yığın operatörünüze başvurun.|  
   | PrivilegedEndpoint | Dağıtım görevleri günlük toplama ve diğer posta gibi özellikleriyle sağlayan bir önceden yapılandırılmış Uzaktan PowerShell Konsolu. Geliştirme Seti için ayrıcalıklı endpoint "AzS-ERCS01" sanal makine üzerinde barındırılır. Tümleşik bir sistem kullanıyorsanız, bu değer almak için Azure yığın operatörünüze başvurun. Daha fazla bilgi edinmek için bkz [kullanan ayrıcalıklı uç noktasını](azure-stack-privileged-endpoint.md) makalesi.|  
   | BillingModel | Aboneliğinizi kullanan faturalama modeli. İzin verilen değerler için bu parametreyi are - **kapasite**, **PayAsYouUse**, ve **geliştirme**. Geliştirme Seti için bu değeri ayarlamak **geliştirme**. Tümleşik bir sistem kullanıyorsanız, bu değer almak için Azure yığın operatörünüze başvurun. |

8. Komut tamamlandığında bir ileti görür "etkinleştirme Azure yığın (Bu adımı tamamlamak için 10 dakika sürebilir)."




## <a name="verify-the-registration"></a>Kayıt doğrulayın  

1. Yönetici portalı'na (https://adminportal.local.azurestack.external) oturum açın.

2. Tıklatın **daha fazla hizmet** > **Market Yönetim** > **Azure'dan eklemek**.

3. Bir öğe (örneğin, WordPress) azure'dan kullanılabilir listesi görürseniz, etkinleştirme başarılı oldu.

> [!NOTE]
> Kayıt tamamlandıktan sonra değil kaydettirmek için etkin uyarı artık görünür.


## <a name="modify-the-registration"></a>Kayıt değiştirme

### <a name="change-the-subscription-you-use"></a>Kullandığınız abonelik değiştirme
Kullandığınız abonelik değiştirmek istiyorsanız, gerekir ilk Kaldır AzsRegistration çalıştırmak, doğru Azure PowerShell bağlamına oturum emin olun ve ardından kümesi AzsRegistration değiştirilen parametreleri ile çalıştırın.

  ```Powershell   
  Remove-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint
  Set-AzureRmContext -SubscriptionId $NewSubscriptionId
  Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```
### <a name="change-the-billing-model-or-syndication-features"></a>Faturalama modeli ya da dağıtım özelliklerini değiştirme
Faturalama modeli veya yüklemeniz için dağıtım özellikleri değiştirmek istiyorsanız, yeni değerleri ayarlamak için kayıt işlevi çağırabilirsiniz. Önce geçerli kaydı kaldırmak gerekmez.  

  ```Powershell
     Set-AzsRegistration -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel PayAsYouUse
  ```


## <a name="disconnected-registration"></a>Bağlantısı kesilmiş kayıt
*Bu bölümdeki bilgiler, Azure yığın 1712 güncelleştirme sürümünü (1.0.180103.2) ile başlayan uygular ve daha önceki sürümlerde desteklenmiyor.*

Bağlantısı kesilmiş bir ortamda Azure yığın kaydediyorsanız Azure yığın ortamından bir kayıt belirtecinizi almak ve kayıt için Azure'a bağlanabilmesi için bir makinede belirtecini kullanmak gerekir.  

### <a name="get-a-registration-token-from-the-azure-stack-environment"></a>Bir kayıt Azure yığın ortamından belirteci alma
  1. Kayıt belirtecinizi almak için şu komutu çalıştırın:  

     ```Powershell
        $FilePathForRegistrationToken = $env:SystemDrive\RegistrationToken.txt
        $RegistrationToken = Get-AzsRegistrationToken -CloudAdminCredential $YourCloudAdminCredential -PrivilegedEndpoint $YourPrivilegedEndpoint -BillingModel Capacity -AgreementNumber '<your agreement number>' -TokenOutputFilePath $FilePathForRegistrationToken
      ```
      > [!TIP]  
      > Kayıt belirtecinizi için belirtilen dosyasında kaydedilen *$env:SystemDrive\RegistrationToken.txt*.

  2. Bu kayıt belirtecinizi Azure üzerinde kullanılmak üzere kaydetme makine bağlı.


### <a name="connect-to-azure-and-register"></a>Azure ve kayıt bağlanmak
Bu yordam için adımları Azure'a bağlanabilmesi için bir makinede çalıştırın.

  1. [Karşılık gelen son PowerShell modülleri için kayıt Github'dan indirdiğinizde](azure-stack-powershell-download.md).  

  2. Önceki adımda oluşturduğunuz "AzureStack araçları yönetici" dizininden "Kayıt" klasörüne gidin ve ".\RegisterWithAzure.psm1" modülünü içeri aktarın:  

     ```powershell
     Import-Module .\RegisterWithAzure.psm1
     ```

  3. Kopya [RegisterWithAzure.psm1](https://go.microsoft.com/fwlink/?linkid=842959) bir klasöre gibi *C:\Temp*.

  4. PowerShell ISE yönetici olarak başlatın ve RegisterWithAzure modülü içe aktarın.  

  5. Azure yığın ortamınızı kaydetmek için kullanmak istediğiniz doğru Azure PowerShell bağlamına oturum açtığınızdan emin olun:  

     ```Powershell
        Set-AzureRmContext -SubscriptionId $YourAzureSubscriptionId   
     ```
  6. Azure ile kaydetmek için kayıt belirtecini belirtin:

     ```Powershell  
       $registrationToken = "*Your Registration Token*"
       Register-AzsEnvironment -RegistrationToken $registrationToken  
     ```
    İsteğe bağlı olarak, kayıt belirtecinizi içeren bir dosyaya işaret için Get-Content cmdlet'i kullanabilirsiniz:

    ```Powershell  
       $registrationToken = Get-Content -Path 'C:\Temp\<Registration Token File>'
       Register-AzsEnvironment -RegistrationToken $registrationToken  
    ```
> [!NOTE]  
> Kayıt kaynak adı ya da daha sonra başvurmak için kayıt belirteci kaydedin.

### <a name="remove-a-registered-resource"></a>Kayıtlı bir kaynağı kaldırırsanız
Ardından, kayıt kaynağı kaldırmak istiyorsanız, UnRegister-AzsEnvironment kullanın ve kaydı kaynak adı veya Register-AzsEnvironment için kullanılan kayıt belirtecini geçirin gerekir.
- **Kayıt kaynak adı:**

  ```Powershell    
     UnRegister-AzsEnvironment -RegistrationName "*Name of the registration resource*"
  ```
- **Kayıt belirtecinizi:**    

  ```Powershell
     $registrationToken = "*Your copied registration token*"
     UnRegister-AzsEnvironment -RegistrationToken $registrationToken
   ```


## <a name="next-steps"></a>Sonraki adımlar
[Azure Stack’e bağlanma](azure-stack-connect-azure-stack.md)
