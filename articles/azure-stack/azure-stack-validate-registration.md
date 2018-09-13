---
title: Azure Stack için Azure kaydı doğrulamak | Microsoft Docs
description: Azure Stack hazırlık denetleyicisi Azure kaydı doğrulamak için kullanın.
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
ms.date: 06/08/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 57869de8a99c65810da0c75f81c75d93eac88412
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35998582"
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

   > `Start-AzsReadinessChecker -RegistrationAccount $registrationCredential -AzureEnvironment AzureCloud -RegistrationSubscriptionID $subscriptionID`

5. Aracı çalıştırıldıktan sonra çıkışını gözden geçirin. Oturum açma ve kayıt gereksinimleri için Tamam durumu olduğunu onaylayın. Başarılı bir doğrulama şu resimdeki gibi görünür:  
![doğrulamayı Çalıştır](./media/azure-stack-validate-registration/registration-validation.png)


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
![Abonelik sahibi](./media/azure-stack-validate-registration/subscription-owner.png)
**neden** -hesap Azure aboneliğinin bir Yöneticisi değildir.   

**Çözüm** -Azure Stack dağıtım kullanım için faturalandırılır Azure aboneliğinde yönetici olan bir hesap kullanın.


### <a name="expired-or-temporary-password"></a>Süresi dolmuş veya geçici parola 
![süresi dolan parola](./media/azure-stack-validate-registration/expired-password.png)
**neden** -hesap parolası ya da süresi dolmuş veya geçici olduğundan oturum açamaz.     

**Çözüm** - PowerShell çalıştırın ve parolayı sıfırlamak için istemleri izleyin. 
  > `Login-AzureRMAccount` 

Alternatif olarak, oturum https://portal.azure.com gibi hesabı ve kullanıcı parolasını değiştirmeye zorlanır.


### <a name="microsoft-accounts-are-not-supported-for-registration"></a>Microsoft hesapları kaydı için desteklenmez  
![Desteklenmeyen hesap](./media/azure-stack-validate-registration/unsupported-account.png)
**neden** -bir Microsoft hesabı (örneğin, Outlook.com veya Hotmail.com) belirtildi.  Bu hesapları desteklenmez.

**Çözüm** -bir hesabı ve aboneliği bir bulut hizmeti sağlayıcısı (CSP) veya Kurumsal Anlaşma (EA) kullanın. 


### <a name="unknown-user-type"></a>Bilinmeyen kullanıcı türü  
![Bilinmeyen kullanıcı](./media/azure-stack-validate-registration/unknown-user.png)
**neden** -hesabı için belirtilen Azure Active Directory ortamını oturum açamaz. Bu örnekte, *AzureChinaCloud* olarak belirtilen *AzureEnvironment*.  

**Çözüm** -hesap belirtilen Azure ortam için geçerli olduğunu doğrulayın. PowerShell'de hesabının belirli bir ortam için geçerli olup olmadığını doğrulamak için aşağıdaki komutu çalıştırın.     
  > `Login-AzureRmAccount -EnvironmentName AzureChinaCloud`


## <a name="next-steps"></a>Sonraki Adımlar
[Azure kimlik doğrulama](azure-stack-validate-identity.md)
[Hazırlık raporunu görüntülemek](azure-stack-validation-report.md)
[genel Azure Stack tümleştirme konuları](azure-stack-datacenter-integration.md)

