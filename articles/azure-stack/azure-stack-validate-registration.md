---
title: Azure kaydı doğrulamak için Azure yığınına | Microsoft Docs
description: Azure kaydı doğrulamak için Azure yığın hazırlık Denetleyicisi'ni kullanın.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 84eb1c08cc3f9ef104e2eb0b96ed397315c3f374
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="validate-azure-registration"></a>Azure kaydı doğrula 
Azure aboneliğinizde Azure yığın ile kullanıma hazır olduğunu doğrulamak için Azure yığın hazırlık Denetleyicisi aracını (AzsReadinessChecker) kullanın. Bir Azure yığın dağıtımına başlamadan önce kayıt doğrulayın. Hazırlık denetleyicisi doğrular:
- Kullandığınız Azure aboneliği desteklenen bir tür değil. Abonelikler, bir bulut hizmeti sağlayıcısı (CSP) veya Kurumsal Anlaşma (Kurumsal Sözleşme) olması gerekir. 
- Aboneliğiniz Azure ile kaydetmek için kullandığınız hesap Azure'a oturum açabilir ve abonelik sahibi. 

Azure yığın kayıt hakkında daha fazla bilgi için bkz: [kaydetmek Azure yığın Azure ile](azure-stack-registration.md). 

## <a name="get-the-readiness-checker-tool"></a>Hazırlık Denetleyicisi Aracı'nı edinin
Azure yığın hazırlık Denetleyicisi aracını (AzsReadinessChecker) en son sürümünü edinilebilir indirme [PSGallery](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Önkoşullar
Aşağıdaki önkoşulların yerinde olması gerekir.

**Aracın çalıştığı bilgisayar:**
 - Windows 10 veya Windows Server 2016 Internet bağlantısına sahip.
 - PowerShell 5.1 veya üstü. Sürümünüzü denetlemek için aşağıdaki PowerShell cmd çalıştırın ve daha sonra gözden *ana* sürümü ve *küçük* sürümleri:  

    >`$PSVersionTable.PSVersion` 
 - Yapılandırma [Azure yığını için PowerShell](azure-stack-powershell-install.md). 
 - En son sürümünü indirme [Microsoft Azure yığın hazırlık denetleyicisi](https://aka.ms/AzsReadinessChecker) aracı.  

**Azure Active Directory ortamında:**
 - Kullanıcı adı ve parolası, Azure yığın ile kullanacağınız Azure abonelik sahibi olan bir hesap tanımlayın.  
 - Kullanacağınız Azure aboneliği için abonelik Kimliğini tanımlayın. 
 - Kullanacağınız AzureEnvironment tanımlayın: *AzureCloud*, *AzureGermanCloud*, veya *AzureChinaCloud*.

## <a name="validate-azure-registration"></a>Azure kaydı doğrula
1. Önkoşulları karşılayan bir bilgisayar üzerinde yönetici bir PowerShell komut istemini açın ve AzsReadinessChecker yüklemek için aşağıdaki komutu çalıştırın.
    > `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

2. Ayarlamak için aşağıdaki PowerShell isteminden çalıştırıp *$registrationCredential* abonelik sahibi olan hesap olarak.   Değiştir *subscriptionowner@contoso.onmicrosoft.com* hesabı ve Kiracı. 
    > `$registrationCredential = Get-Credential subscriptionowner@contoso.onmicrosoft.com -Message "Enter Credentials for Subscription Owner"`

3. Ayarlamak için aşağıdaki PowerShell isteminden çalıştırıp *$subscriptionID* olarak kullanacağınız Azure aboneliği. Değiştir *xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx* kendi aboneliği ID'ye sahip  
     > `$subscriptionID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"` 

4. Aboneliğinizin doğrulamayı başlatmak için aşağıdaki PowerShell isteminde çalıştırın 
   - Değer AzureEnvironment belirtin *AzureCloud*, *AzureGermanCloud*, veya *AzureChinaCloud*.  
   - Azure Active Directory yöneticiniz ve Azure Active Directory Kiracı adınızı sağlayın. 

   > `Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -AzureEnvironment AzureCloud -RegistrationSubscriptionID $subscriptionID`

5. Aracı çalıştıktan sonra çıktıyı gözden geçirin. Oturum açma ve kayıt gereksinimleri için durumunun Tamam olduğunu doğrulayın. Başarılı bir doğrulama aşağıdaki görüntü gibi görünür:  
![Çalışma doğrulama](./media/azure-stack-validate-registration/registration-validation.png)


## <a name="report-and-log-file"></a>Rapor ve günlük dosyası
Her zaman doğrulama çalıştırır, sonuçları günlüklerini **AzsReadinessChecker.log** ve **AzsReadinessCheckerReport.json**. Bu dosyaların konumunu PowerShell'de doğrulama sonuçlarını görüntüler. 

Bu dosyalar yardımcı olabilecek Azure yığın dağıtmak veya doğrulama sorunları araştırmak için önce doğrulama durumu paylaşın. Her iki dosya her sonraki doğrulama denetiminin sonuçlarını kalıcı olmasını sağlar. Rapor dağıtım takım onayınız kimlik yapılandırmasının sağlar. Günlük dosyası, doğrulama sorunları araştırmak dağıtım veya destek ekibiniz yardımcı olabilir. 

Varsayılan olarak, her iki dosya yazılan *C:\Users\<kullanıcı adı > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
 - Kullanım **- OutputPath** ***&lt;yolu&gt;*** farklı rapor konumunu belirtmek için çalışma komut satırı sonunda parametresi.   
 - Kullanım **- CleanReport** parametre bilgilerini temizlemek için run komutunun sonuna *AzsReadinessCheckerReport.json*.  Önceki hakkında aracını çalıştırır. Daha fazla bilgi için [Azure yığın doğrulama raporu](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Doğrulama hataları
Bir doğrulama denetimi başarısız olursa, hata hakkındaki ayrıntılar PowerShell penceresinde görüntüler. Araç ayrıca AzsReadinessChecker.log bilgileri günlüğe kaydeder.

Aşağıdaki örnekler, ortak doğrulama hataları hakkında rehberlik sağlar.

### <a name="user-must-be-an-owner-of-the-subscription"></a>Kullanıcı aboneliğin sahibi olmanız gerekir   
![Abonelik sahibi](./media/azure-stack-validate-registration/subscription-owner.png)
**neden** -hesap Azure Abonelik Yöneticisi değil.   

**Çözümleme** -Azure yığın dağıtımından kullanım için faturalandırılır Azure Abonelik Yöneticisi olan bir hesap kullanın.


### <a name="expired-or-temporary-password"></a>Süresi dolmuş ya da geçici parola 
![süresi dolmuş parola](./media/azure-stack-validate-registration/expired-password.png)
**neden** -hesap parolasını ya da süresi dolmuş veya geçici olduğundan oturum açamaz.     

**Çözümleme** - PowerShell'de çalıştırın ve parola sıfırlama için istemleri izleyin. 
  > `Login-AzureRMAccount` 

Alternatif olarak, oturum https://portal.azure.com hesabı ve kullanıcı parolayı değiştirmek için zorlanır gibi.


### <a name="microsoft-accounts-are-not-supported-for-registration"></a>Microsoft hesapları kaydı için desteklenmez.  
![Desteklenmeyen hesabı](./media/azure-stack-validate-registration/unsupported-account.png)
**neden** -(Outlook.com veya Hotmail.com gibi) bir Microsoft hesabı belirtildi.  Bu hesapları desteklenmez.

**Çözümleme** -bir hesap ve abonelik bir bulut hizmeti sağlayıcısı (CSP) veya Kurumsal Anlaşma (Kurumsal Sözleşme) kullanın. 


### <a name="unknown-user-type"></a>Bilinmeyen kullanıcı türü  
![Bilinmeyen kullanıcıya](./media/azure-stack-validate-registration/unknown-user.png)
**neden** -hesap belirtilen Azure Active Directory ortamına oturum açamıyor. Bu örnekte, *AzureChinaCloud* olarak belirtilen *AzureEnvironment*.  

**Çözümleme** -hesabın belirtilen Azure ortamı için geçerli olduğunu doğrulayın. PowerShell'de hesabının belirli bir ortam için geçerli olduğunu doğrulamak için aşağıdaki komutu çalıştırın.     
  > `Login-AzureRmAccount -EnvironmentName AzureChinaCloud`


## <a name="next-steps"></a>Sonraki Adımlar
[Azure kimlik doğrulama](azure-stack-validate-identity.md)
[hazırlık raporu görüntülemek](azure-stack-validation-report.md)
[genel Azure yığın tümleştirme konuları](azure-stack-datacenter-integration.md)

