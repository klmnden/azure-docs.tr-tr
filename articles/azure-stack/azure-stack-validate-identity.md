---
title: Azure yığını için Azure kimlik doğrulama | Microsoft Docs
description: Azure kimliğini doğrulamak için Azure yığın hazırlık Denetleyicisi'ni kullanın.
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
ms.openlocfilehash: fe5e7281cbe01ad11f667729df344f91ef1327d2
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="validate-azure-identity"></a>Azure kimlik doğrulama 
Azure Active Directory'yi (Azure AD) ile Azure yığın kullanıma hazır olduğunu doğrulamak için Azure yığın hazırlık Denetleyicisi aracını (AzsReadinessChecker) kullanın. Bir Azure yığın dağıtımına başlamadan önce Azure kimlik çözümü doğrulayın.  

Hazırlık denetleyicisi doğrular:
 - Azure Active Directory (Azure AD) Azure yığınının kimlik sağlayıcısı.
 - Kullanmayı planladığınız Azure AD hesabının, Azure Active Directory genel yönetici olarak oturum açabilir. 

Doğrulama ortamınızı Azure, Azure AD'de kullanıcıları, uygulamalar, grupları ve Azure yığından hizmet sorumluları hakkında bilgi depolamak için yığın için hazır olmasını sağlar.

## <a name="get-the-readiness-checker-tool"></a>Hazırlık Denetleyicisi Aracı'nı edinin
Azure yığın hazırlık Denetleyicisi aracını (AzsReadinessChecker) en son sürümünü indirin [PSGallery](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Önkoşullar
Aşağıdaki önkoşulların yerinde olması gerekir.

**Aracın çalıştığı bilgisayar:**
 - Windows 10 veya Windows Server 2016 Internet bağlantısına sahip.
 - PowerShell 5.1 veya üstü. Sürümünüzü denetlemek için aşağıdaki PowerShell cmd çalıştırın ve daha sonra gözden *ana* sürümü ve *küçük* sürümleri:  

   > `$PSVersionTable.PSVersion`
 - Yapılandırma [Azure yığını için PowerShell](azure-stack-powershell-install.md). 
 - En son sürümünü [Microsoft Azure yığın hazırlık denetleyicisi](https://aka.ms/AzsReadinessChecker) aracı.

**Azure Active Directory ortamında:**
 - Azure AD hesabının, Azure yığını için kullanabilir ve Azure Active Directory genel yönetici olduğundan emin olun tanımlayın.
 - Azure AD Kiracı adınızı tanımlayın. Kiracı adı olmalıdır *birincil* için Azure Active Directory etki alanı adı. Örneğin, *contoso.onmicrosoft.com*. 
 - Kullanacağınız AzureEnvironement tanımlayın: *AzureCloud*, *AzureGermanCloud*, veya *AzureChinaCloud*.

## <a name="validate-azure-identity"></a>Azure kimlik doğrulama 
1. Önkoşulları karşılayan bir bilgisayar, bir yönetici PowerShell istemi açın ve ardından AzsReadinessChecker yüklemek için aşağıdaki komutu çalıştırın:  

   > `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

2. Ayarlamak için aşağıdaki PowerShell isteminden çalıştırıp *$serviceAdminCredential* olarak Azure AD Kiracınız için Hizmet Yöneticisi.  Değiştir *serviceadmin@contoso.onmicrosoft.com* hesabı ve Kiracı. 
   > `$serviceAdminCredential = Get-Credential serviceadmin@contoso.onmicrosoft.com -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant"` 

3. PowerShell isteminden Azure ad doğrulamayı başlatmak için aşağıdakini çalıştırın. 
   - Değer AzureEnvironment belirtin *AzureCloud*, *AzureGermanCloud*, veya *AzureChinaCloud*.  
   - Azure Active Directory Kiracı değiştirmek için adınızı belirtmek *contoso.onmicrosoft.com*. 

   > `Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -AzureEnvironment AzureCloud -AADDirectoryTenantName contoso.onmicrosoft.com`
4. Aracı çalıştıktan sonra çıktıyı gözden geçirin. Durum onaylayın **Tamam** oturum açma ve Kurulum gereksinimleri için. Başarılı bir doğrulama aşağıdaki görüntü gibi görünür: 
 
![Çalışma doğrulama](./media/azure-stack-validate-identity/validation.png)


## <a name="report-and-log-file"></a>Rapor ve günlük dosyası
Her zaman doğrulama çalıştırır, sonuçları günlüklerini **AzsReadinessChecker.log** ve **AzsReadinessCheckerReport.json**. Bu dosyaların konumunu PowerShell'de doğrulama sonuçlarını görüntüler.

Bu dosyalar yardımcı olabilecek Azure yığın dağıtmak veya doğrulama sorunları araştırmak için önce doğrulama durumu paylaşın.  Her iki dosya her sonraki doğrulama denetiminin sonuçlarını kalıcı olmasını sağlar. Rapor dağıtım takım onayınız kimlik yapılandırmasının sağlar. Günlük dosyası, doğrulama sorunları araştırmak dağıtım veya destek ekibiniz yardımcı olabilir. 

Varsayılan olarak, her iki dosya yazılan *C:\Users\<kullanıcı adı > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
 - Kullanım **- OutputPath** ***&lt;yolu&gt;*** farklı rapor konumunu belirtmek için çalışma komut satırı sonunda parametresi.   
 - Kullanım **- CleanReport** parametre bilgilerini temizlemek için run komutunun sonuna *AzsReadinessCheckerReport.json*.  Önceki hakkında aracını çalıştırır. 

Daha fazla bilgi için [Azure yığın doğrulama raporu](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Doğrulama hataları
Bir doğrulama denetimi başarısız olursa, hata hakkındaki ayrıntılar PowerShell penceresinde görüntüler. Araç ayrıca AzsReadinessChecker.log bilgileri günlüğe kaydeder.

Aşağıdaki örnekler, ortak doğrulama hataları hakkında rehberlik sağlar.

### <a name="expired-or-temporary-password"></a>Süresi dolmuş ya da geçici parola 
 
![Parolanın süresi](./media/azure-stack-validate-identity/expired-password.png)
**neden** -hesap parolasını ya da süresi dolmuş veya geçici olduğundan oturum açamaz.     

**Çözümleme** - PowerShell içinde aşağıdaki komutu çalıştırın ve ardından parolayı sıfırlamak için istemleri izleyin.  
> `Login-AzureRMAccount`

Alternatif olarak, oturum https://portal.azure.com hesabı ve kullanıcı parolayı değiştirmek için zorlanır gibi.
### <a name="unknown-user-type"></a>Bilinmeyen kullanıcı türü 
 
![Bilinmeyen kullanıcı](./media/azure-stack-validate-identity/unknown-user.png)
**neden** -hesap belirtilen Azure Active Directory'ye (AADDirectoryTenantName) oturum açamaz. Bu örnekte, *AzureChinaCloud* olarak belirtilen *AzureEnvironment*.

**Çözümleme** -hesabın belirtilen Azure ortamı için geçerli olduğunu doğrulayın. PowerShell'de, hesap belirli bir ortam için geçerli olduğunu doğrulayın için aşağıdakini çalıştırın: Login-AzureRmAccount – EnvironmentName AzureChinaCloud 
### <a name="account-is-not-an-administrator"></a>Hesap bir yönetici değil 
 
![Yöneticisi değil](./media/azure-stack-validate-identity/not-admin.png)

**Neden** -hesap başarıyla oturum açabilseniz hesap Azure Active Directory (AADDirectoryTenantName) bir yönetici değil.  

**Çözümleme** -oturum aç https://portal.azure.com hesabıyla Git **Azure Active Directory** > **kullanıcılar** > *kullanıcıyıseçin*  >  **Dizin rolünü**ve ardından kullanıcı olduğundan emin olun bir **genel yönetici**.  Bir kullanıcı hesabıysa Git **Azure Active Directory** > **özel etki alanı** adları ve adı, sağlanan olduğunu onaylayın *AADDirectoryTenantName* olduğu Bu dizin için birincil etki alanı adı olarak işaretlenmiş.  Bu örnekte, olan *contoso.onmicrosoft.com*. 

Azure yığını, etki alanı adının birincil etki alanı adı olmasını gerektirir.

## <a name="next-steps"></a>Sonraki Adımlar
[Azure kaydı doğrula](azure-stack-validate-registration.md)  
[Hazırlık raporunu görüntüle](azure-stack-validation-report.md)  
[Genel Azure yığın tümleştirme konuları](azure-stack-datacenter-integration.md)  

