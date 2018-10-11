---
title: Azure Stack için Azure kimlik doğrulama | Microsoft Docs
description: Azure kimlik doğrulamak için Azure Stack hazırlık Denetleyicisi'ni kullanın.
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
ms.date: 05/08/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 9c7ac89d1f12e8ec033b201f2c2dd845c11486e2
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077826"
---
# <a name="validate-azure-identity"></a>Azure kimlik doğrulama 
Azure Active Directory (Azure AD) Azure Stack ile kullanmak hazır olduğunu doğrulamak için Azure Stack hazırlık Denetleyicisi Aracı (AzsReadinessChecker) kullanın. Azure Stack dağıtıma başlamadan önce Azure kimlik çözümü doğrulayın.  

Hazırlık denetleyicisi doğrular:
 - Azure Active Directory (Azure AD) Azure Stack için kimlik sağlayıcısı.
 - Kullanmayı planlıyorsanız Azure AD hesabı Azure Active Directory genel Yöneticisi oturum açabilir. 

Doğrulama, ortamınızı Azure AD'NİZDE kullanıcılar, uygulamaları, grupları ve Azure Stack gelen hizmet sorumluları hakkında bilgi depolamak Azure Stack için hazır olmasını sağlar.

## <a name="get-the-readiness-checker-tool"></a>Hazırlık Denetleyicisi Aracı'nı edinin
Azure Stack hazırlık Denetleyicisi Aracı (AzsReadinessChecker) en son sürümünü indirin [PSGallery](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Önkoşullar
Aşağıdaki önkoşulların karşılanması gerekir.

**Aracın çalıştığı bilgisayarda:**
 - Windows 10 veya Windows Server 2016, internet bağlantısı.
 - PowerShell 5.1 veya üzeri. Sürümünüzü denetlemek için aşağıdaki PowerShell cmd'yi çalıştırın ve daha sonra gözden *ana* sürümü ve *küçük* sürümleri:  

   > `$PSVersionTable.PSVersion`
 - Yapılandırma [Azure Stack için PowerShell](azure-stack-powershell-install.md). 
 - En son sürümünü [Microsoft Azure Stack hazırlık denetleyicisi](https://aka.ms/AzsReadinessChecker) aracı.

**Azure Active Directory ortamı:**
 - Azure Stack için kullanacağınızı ve Azure Active Directory genel yönetici olduğundan emin olun, Azure AD hesabı belirleyin.
 - Azure AD Kiracı adınızın belirleyin. Kiracı adı olmalıdır *birincil* için Azure Active Directory etki alanı adı. Örneğin, *contoso.onmicrosoft.com*. 
 - Kullanacağınız AzureEnvironement tanımlayın: *AzureCloud*, *AzureGermanCloud*, veya *AzureChinaCloud*.

## <a name="validate-azure-identity"></a>Azure kimlik doğrulama 
1. Önkoşulları karşılayan bir bilgisayarda, yönetici bir PowerShell istemi açın ve ardından AzsReadinessChecker yüklemek için aşağıdaki komutu çalıştırın:  

   > `Install-Module Microsoft.AzureStack.ReadinessChecker -Force`

2. PowerShell isteminden ayarlamak için aşağıdaki komutu çalıştırın *$serviceAdminCredential* Azure AD Kiracınız için Hizmet Yöneticisi olarak.  Değiştirin *serviceadmin@contoso.onmicrosoft.com* hesabı ve Kiracı. 
   > `$serviceAdminCredential = Get-Credential serviceadmin@contoso.onmicrosoft.com -Message "Enter Credentials for Service Administrator of Azure Active Directory Tenant"` 

3. Azure ad doğrulamayı başlatmak için aşağıdaki PowerShell isteminde çalıştırın. 
   - Değer AzureEnvironment belirtin *AzureCloud*, *AzureGermanCloud*, veya *AzureChinaCloud*.  
   - Azure Active Directory Kiracınızın değiştirmek için adının belirtin *contoso.onmicrosoft.com*. 

   > `Start-AzsReadinessChecker -AADServiceAdministrator $serviceAdminCredential -AzureEnvironment AzureCloud -AADDirectoryTenantName contoso.onmicrosoft.com`
4. Aracı çalıştırıldıktan sonra çıkışını gözden geçirin. Durumu doğrulamak **Tamam** hem oturum hem de yükleme gereksinimleri. Başarılı bir doğrulama şu resimdeki gibi görünür: 
 
![doğrulamayı Çalıştır](./media/azure-stack-validate-identity/validation.png)


## <a name="report-and-log-file"></a>Rapor ve günlük dosyası
Her zaman doğrulama çalışır ve sonuçları günlükleri **AzsReadinessChecker.log** ve **AzsReadinessCheckerReport.json**. Doğrulama sonuçları PowerShell'de bu dosyalarının konumunu görüntüler.

Bu dosyalar yardımcı olabilecek Azure Stack dağıtma veya doğrulama sorunları araştırmak için önce doğrulama durumu paylaşın.  Her iki dosya her sonraki doğrulama denetimi sonuçlarını kalıcı hale getirin. Rapor dağıtım takım onayınız kimlik yapılandırması sağlar. Günlük dosyası dağıtım veya destek takım doğrulama sorunları araştırmanıza yardımcı olabilir. 

Varsayılan olarak, her iki dosya için yazılan *C:\Users\<kullanıcıadı > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json*.  
 - Kullanım **- OutputPath** ***&lt;yolu&gt;*** sonunda, farklı rapor konumunu belirtmek için çalışma komut satırı parametresi.   
 - Kullanım **- CleanReport** parametre bilgilerini temizlemek için bir run komutu sonunda *AzsReadinessCheckerReport.json*.  Önceki hakkında aracını çalıştırır. 

Daha fazla bilgi için [Azure Stack doğrulama raporu](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Doğrulama hataları
Doğrulama denetimi başarısız olursa, hata hakkındaki ayrıntılar PowerShell penceresinde görüntüler. Araç ayrıca AzsReadinessChecker.log için bilgileri günlüğe kaydeder.

Aşağıdaki örnekler, yaygın doğrulama hataları hakkında rehberlik sağlar.

### <a name="expired-or-temporary-password"></a>Süresi dolmuş veya geçici parola 
 
![Parolanın süresi](./media/azure-stack-validate-identity/expired-password.png)
**neden** -hesap parolası ya da süresi dolmuş veya geçici olduğundan oturum açamaz.     

**Çözüm** - PowerShell'de aşağıdaki komutu çalıştırın ve ardından parolayı sıfırlamak için istemleri izleyin.  
> `Login-AzureRMAccount`

Alternatif olarak, oturum https://portal.azure.com gibi hesabı ve kullanıcı parolasını değiştirmeye zorlanır.
### <a name="unknown-user-type"></a>Bilinmeyen kullanıcı türü 
 
![Bilinmeyen kullanıcı](./media/azure-stack-validate-identity/unknown-user.png)
**neden** -hesap belirtilen Azure Active Directory'ye (AADDirectoryTenantName) oturum açamaz. Bu örnekte, *AzureChinaCloud* olarak belirtilen *AzureEnvironment*.

**Çözüm** -hesap belirtilen Azure ortam için geçerli olduğunu doğrulayın. PowerShell'de, hesap için belirli bir ortama geçerli olduğunu doğrulamak için aşağıdaki komutu çalıştırın: Login-AzureRmAccount-EnvironmentName AzureChinaCloud 
### <a name="account-is-not-an-administrator"></a>Hesap bir yönetici değil 
 
![Yöneticisi değil](./media/azure-stack-validate-identity/not-admin.png)

**Neden** -hesap başarıyla oturum açabilseniz hesap Azure Active Directory (AADDirectoryTenantName) bir yönetici değil.  

**Çözüm** -oturum https://portal.azure.com hesabıyla Git **Azure Active Directory** > **kullanıcılar** > *kullanıcıyıseçin*  >  **Dizin rolü**ve ardından kullanıcı olun bir **genel yönetici**.  Bir kullanıcı hesabı ise Git **Azure Active Directory** > **özel etki alanı** adları ve adı, için sağlanan dize onaylayın *AADDirectoryTenantName* olduğu Bu dizin için birincil etki alanı adı olarak işaretlenmiş.  Bu örnekte, olan *contoso.onmicrosoft.com*. 

Azure Stack etki alanı adının birincil etki alanı adı olmasını gerektirir.

## <a name="next-steps"></a>Sonraki Adımlar
[Azure kaydı doğrula](azure-stack-validate-registration.md)  
[Hazırlık raporunu görüntüle](azure-stack-validation-report.md)  
[Genel Azure Stack tümleştirme konuları](azure-stack-datacenter-integration.md)  

