---
title: Azure Stack için Azure kaydı doğrulamak | Microsoft Docs
description: Azure Stack hazırlık denetleyicisi Azure kaydı doğrulamak için kullanın.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/08/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 51753a5324bbbcbf4e951628a42dd3bf425354af
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49957591"
---
# <a name="validate-azure-registration"></a>Azure kaydı doğrula 
Azure Stack hazırlık Denetleyicisi Aracı (AzsReadinessChecker), Azure aboneliğinizin Azure Stack ile kullanıma hazır olduğunu doğrulamak için kullanın. Azure Stack dağıtıma başlamadan önce kayıt doğrulayın. Hazırlık denetleyicisi doğrular:
- Kullandığınız Azure aboneliği desteklenen bir türdür. Abonelikler, bir bulut hizmeti sağlayıcısı (CSP) veya Kurumsal Anlaşma (EA) olmalıdır. 
- Aboneliğinizin Azure ile kaydolmak için kullandığınız hesap Azure'da oturum açabilir ve bir abonelik sahibi. 

Azure Stack kayıt hakkında daha fazla bilgi için bkz: [kaydetme Azure Stack Azure ile](azure-stack-registration.md). 

## <a name="get-the-readiness-checker-tool"></a>Hazırlık Denetleyicisi Aracı'nı edinin
Azure Stack hazırlık Denetleyicisi Aracı (AzsReadinessChecker) en son sürümü üzerinde kullanılabilir yükleme [PSGallery](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Önkoşullar
Aşağıdaki önkoşulların karşılanması gerekir.

**Aracın çalıştığı bilgisayarda:**
 - Windows 10 veya Windows Server 2016, internet bağlantısı.
 - PowerShell 5.1 veya üzeri. Sürümünüzü denetlemek için aşağıdaki PowerShell cmd'yi çalıştırın ve daha sonra gözden *ana* sürümü ve *küçük* sürümleri:  

    >`$PSVersionTable.PSVersion` 
 - Yapılandırma [Azure Stack için PowerShell](azure-stack-powershell-install.md). 
 - En son sürümünü indirin [Microsoft Azure Stack hazırlık denetleyicisi](https://aka.ms/AzsReadinessChecker) aracı.  

**Azure Active Directory ortamı:**
 - Kullanıcı adı ve parola, Azure Stack ile kullandığınız Azure aboneliği sahibi olan bir hesap için bu seçeneği belirleyin.  
 - Abonelik kimliği için kullanacağınız Azure aboneliği tanımlayın. 
 - Kullanacağınız AzureEnvironment tanımlayın: *AzureCloud*, *AzureGermanCloud*, veya *AzureChinaCloud*.

## <a name="validate-azure-registration"></a>Azure kaydı doğrula
1. Önkoşulları karşılayan bir bilgisayar, bir yönetici bir PowerShell istemi açın ve ardından AzsReadinessChecker yüklemek için aşağıdaki komutu çalıştırın.
    > `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

2. PowerShell isteminden ayarlamak için aşağıdaki komutu çalıştırın *$registrationCredential* abonelik sahibi olan hesabının olarak.   Değiştirin *subscriptionowner@contoso.onmicrosoft.com* hesabı ve Kiracı. 
    > `$registrationCredential = Get-Credential subscriptionowner@contoso.onmicrosoft.com -Message "Enter Credentials for Subscription Owner"`

3. PowerShell isteminden ayarlamak için aşağıdaki komutu çalıştırın *$subscriptionID* olarak kullanacağınız bir Azure aboneliği. Değiştirin *xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx* kendi abonelik kimliğinizle  
     > `$subscriptionID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"` 

4. PowerShell isteminden aboneliğinizin doğrulamayı başlatmak için aşağıdaki komutu çalıştırın. 
   - Değer AzureEnvironment belirtin *AzureCloud*, *AzureGermanCloud*, veya *AzureChinaCloud*.  
   - Azure Active Directory yöneticiniz ve Azure Active Directory Kiracı adınızı sağlayın. 

   > `Invoke-AzsRegistrationValidation -RegistrationAccount $registrationCredential -AzureEnvironment AzureCloud -RegistrationSubscriptionID $subscriptionID`

5. Aracı çalıştırıldıktan sonra çıkışını gözden geçirin. Oturum açma ve kayıt gereksinimleri için Tamam durumu olduğunu onaylayın. Başarılı bir doğrulama aşağıdaki gibi görünür:  
````PowerShell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: OK

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
````


## <a name="report-and-log-file"></a>Rapor ve günlük dosyası
Her zaman doğrulama çalışır ve sonuçları günlükleri **AzsReadinessChecker.log** ve **AzsReadinessCheckerReport.json**. Doğrulama sonuçları PowerShell'de bu dosyalarının konumunu görüntüler. 

Bu dosyalar yardımcı olabilecek Azure Stack dağıtma veya doğrulama sorunları araştırmak için önce doğrulama durumu paylaşın. Her iki dosya her sonraki doğrulama denetimi sonuçlarını kalıcı hale getirin. Rapor dağıtım takım onayınız kimlik yapılandırması sağlar. Günlük dosyası dağıtım veya destek takım doğrulama sorunları araştırmanıza yardımcı olabilir. 

Varsayılan olarak, her iki dosya için yazılan *C:\Users\<kullanıcıadı > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
 - Kullanım **- OutputPath** ***&lt;yolu&gt;*** sonunda, farklı rapor konumunu belirtmek için çalışma komut satırı parametresi.   
 - Kullanım **- CleanReport** parametre bilgilerini temizlemek için bir run komutu sonunda *AzsReadinessCheckerReport.json*.  Önceki hakkında aracını çalıştırır. Daha fazla bilgi için [Azure Stack doğrulama raporu](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Doğrulama hataları
Doğrulama denetimi başarısız olursa, hata hakkındaki ayrıntılar PowerShell penceresinde görüntüler. Araç ayrıca AzsReadinessChecker.log için bilgileri günlüğe kaydeder.

Aşağıdaki örnekler, yaygın doğrulama hataları hakkında rehberlik sağlar.

### <a name="user-must-be-an-owner-of-the-subscription"></a>Kullanıcı, aboneliğin sahibi olmanız gerekir   
````PowerShell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail 
Error Details for registration account admin@contoso.onmicrosoft.com:
The user admin@contoso.onmicrosoft.com is role(s) Reader for subscription 3f961d1c-d1fb-40c3-99ba-44524b56df2d. User must be an owner of the subscription to be used for registration.
Additional help URL https://aka.ms/AzsRemediateRegistration

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
````
**Neden** -hesap Azure aboneliğinin bir Yöneticisi değildir.   

**Çözüm** -Azure Stack dağıtım kullanım için faturalandırılır Azure aboneliğinde yönetici olan bir hesap kullanın.


### <a name="expired-or-temporary-password"></a>Süresi dolmuş veya geçici parola 
````PowerShell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail 
Error Details for registration account admin@contoso.onmicrosoft.com:
Checking Registration failed with: Retrieving TenantId for subscription 3f961d1c-d1fb-40c3-99ba-44524b56df2d using account admin@contoso.onmicrosoft.com failed with AADSTS50055: Force Change P
assword.
Trace ID: 48fe06f5-a5b4-4961-ad45-a86964689900
Correlation ID: 3dd1c9b2-72fb-46a0-819d-058f7562cb1f
Timestamp: 2018-10-22 11:16:56Z: The remote server returned an error: (401) Unauthorized.

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
````
**Neden** -hesap parolası ya da süresi dolmuş veya geçici olduğundan oturum açamaz.     

**Çözüm** - PowerShell çalıştırın ve parolayı sıfırlamak için istemleri izleyin. 
  > `Login-AzureRMAccount` 

Alternatif olarak, oturum https://portal.azure.com gibi hesabı ve kullanıcı parolasını değiştirmeye zorlanır.


### <a name="unknown-user-type"></a>Bilinmeyen kullanıcı türü  
````PowerShell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail 
Error Details for registration account admin@contoso.onmicrosoft.com:
Checking Registration failed with: Retrieving TenantId for subscription 3f961d1c-d1fb-40c3-99ba-44524b56df2d using account admin@contoso.onmicrosoft.com failed with unknown_user_type: Unknown Us
er Type

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
````
**Neden** -hesabı için belirtilen Azure Active Directory ortamını oturum açamaz. Bu örnekte, *AzureChinaCloud* olarak belirtilen *AzureEnvironment*.  

**Çözüm** -hesap belirtilen Azure ortam için geçerli olduğunu doğrulayın. PowerShell'de hesabının belirli bir ortam için geçerli olup olmadığını doğrulamak için aşağıdaki komutu çalıştırın.     
  > `Login-AzureRmAccount -EnvironmentName AzureChinaCloud`


## <a name="next-steps"></a>Sonraki Adımlar
[Azure kimlik doğrulama](azure-stack-validate-identity.md)
[Hazırlık raporunu görüntülemek](azure-stack-validation-report.md)
[genel Azure Stack tümleştirme konuları](azure-stack-datacenter-integration.md)

