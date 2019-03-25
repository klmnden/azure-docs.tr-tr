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
ms.topic: conceptual
ms.date: 03/23/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 03/23/2019
ms.openlocfilehash: 3dfb87e5f6c231831cd9c007b19ad001e1fce326
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403196"
---
# <a name="validate-azure-identity"></a>Azure kimlik doğrulama

Azure Stack hazırlık Denetleyicisi aracını kullanın (**AzsReadinessChecker**), Azure Active Directory (Azure AD) Azure Stack ile kullanmak hazır olduğunu doğrulamak için. Azure Stack dağıtıma başlamadan önce Azure kimlik çözümü doğrulayın.  

Hazırlık denetleyicisi doğrular:

- Azure Active Directory (Azure AD) Azure Stack için kimlik sağlayıcısı.
- Kullanmayı planlıyorsanız Azure AD hesabı Azure Active Directory genel Yöneticisi oturum açabilir.

Doğrulama, ortamınızı Azure AD'NİZDE kullanıcılar, uygulamaları, grupları ve Azure Stack gelen hizmet sorumluları hakkında bilgi depolamak Azure Stack için hazır olmasını sağlar.

## <a name="get-the-readiness-checker-tool"></a>Hazırlık Denetleyicisi Aracı'nı edinin

Azure Stack hazırlık Denetleyicisi Aracı (AzsReadinessChecker) en son sürümünü indirin [PowerShell Galerisi](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki Önkoşullar gereklidir:

**Aracın çalıştığı bilgisayarda:**

- Windows 10 veya Windows Server 2016, internet bağlantısı.
- PowerShell 5.1 veya üzeri. Sürümünüzü denetlemek için aşağıdaki PowerShell komutunu çalıştırın ve daha sonra gözden **ana** sürümü ve **küçük** sürümleri:  

  ```powershell
  $PSVersionTable.PSVersion
  ```

- [Azure Stack için yapılandırılmış PowerShell](azure-stack-powershell-install.md).
- En son sürümünü [Microsoft Azure Stack hazırlık denetleyicisi](https://aka.ms/AzsReadinessChecker) aracı.

**Azure Active Directory ortamı:**

- Azure Stack için kullanın ve bir Azure Active Directory genel Yöneticisi olduğundan emin olmak için Azure AD hesabını belirleyin.
- Azure AD Kiracı adınızın belirleyin. Kiracı adı, Azure Active Directory'niz için birincil etki alanı adı olmalıdır; Örneğin, **contoso.onmicrosoft.com**.
- Kullanacağınız Azure ortamı tanımlayabilirsiniz. Ortam adı parametresi için desteklenen değerler şunlardır: **AzureCloud**, **AzureChinaCloud**, veya **AzureUSGovernment**kullandığınız Azure aboneliği bağlı olarak.

## <a name="steps-to-validate-azure-identity"></a>Azure kimlik doğrulamaya yönelik adımları

1. Önkoşulları karşılayan bir bilgisayarda, yükseltilmiş bir PowerShell komut istemi açın ve ardından yüklemek için aşağıdaki komutu çalıştırın **AzsReadinessChecker**:  

   ```powershell
   Install-Module Microsoft.AzureStack.ReadinessChecker -Force
   ```

2. PowerShell isteminden ayarlamak için aşağıdaki komutu çalıştırın **$serviceAdminCredential** Azure AD kiracınız için Hizmet Yöneticisi olarak.  Değiştirin **serviceadmin\@contoso.onmicrosoft.com** hesabı ve Kiracı adınızla:

   ```powershell
   $serviceAdminCredential = Get-Credential serviceadmin@contoso.onmicrosoft.com -Message "Enter credentials for service administrator of Azure Active Directory tenant"
   ```

3. PowerShell komut isteminde, Azure ad doğrulamayı başlatmak için aşağıdaki komutu çalıştırın:

   - Ortam adı değerini belirtin **AzureEnvironment**. Ortam adı parametresi için desteklenen değerler şunlardır: **AzureCloud**, **AzureChinaCloud**, veya **AzureUSGovernment**kullandığınız Azure aboneliği bağlı olarak.
   - Değiştirin **contoso.onmicrosoft.com** Azure Active Directory Kiracı adınızla.

   ```powershell
   Invoke-AzsAzureIdentityValidation -AADServiceAdministrator $serviceAdminCredential -AzureEnvironment <environment name> -AADDirectoryTenantName contoso.onmicrosoft.com
   ```

4. Aracı çalıştırıldıktan sonra çıkışını gözden geçirin. Durumu doğrulamak **Tamam** yükleme gereksinimleri için. Başarılı bir doğrulama şu resimdeki gibi görünür:

   ```shell
   Invoke-AzsAzureIdentityValidation v1.1809.1005.1 started.
   Starting Azure Identity Validation

   Checking Installation Requirements: OK

   Finished Azure Identity Validation

   Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
   Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
   Invoke-AzsAzureIdentityValidation Completed
   ```

## <a name="report-and-log-file"></a>Rapor ve günlük dosyası

Her zaman doğrulama çalışır ve sonuçları günlükleri **AzsReadinessChecker.log** ve **AzsReadinessCheckerReport.json**. Doğrulama sonuçları PowerShell'de bu dosyalarının konumunu görüntüler.

Bu dosyalar yardımcı olabilecek Azure Stack dağıtma veya doğrulama sorunları araştırmak için önce doğrulama durumu paylaşın. Her iki dosya her sonraki doğrulama denetimi sonuçlarını kalıcı hale getirin. Rapor dağıtım takım onayınız kimlik yapılandırması sağlar. Günlük dosyası dağıtım veya destek takım doğrulama sorunları araştırmanıza yardımcı olabilir.

Varsayılan olarak, her iki dosya için yazılan **C:\Users\<kullanıcıadı > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json**.  

- Kullanım **- OutputPath** ***&lt;yolu&gt;*** sonunda, farklı rapor konumunu belirtmek için çalışma komut satırı parametresi.
- Kullanım **- CleanReport** sonunda aracından, önceki çalıştırmaları hakkında bilgi temizlemek için bir run komutu, parametre **AzsReadinessCheckerReport.json**.

Daha fazla bilgi için [Azure Stack doğrulama raporu](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Doğrulama hataları

Doğrulama denetimi başarısız olursa, hata hakkındaki ayrıntılar PowerShell penceresinde görüntüler. Araç ayrıca AzsReadinessChecker.log dosyasına bilgileri günlüğe kaydeder.

Aşağıdaki örnekler, yaygın doğrulama hataları hakkında rehberlik sağlar.

### <a name="expired-or-temporary-password"></a>Süresi dolmuş veya geçici parola

```shell
Invoke-AzsAzureIdentityValidation v1.1809.1005.1 started.
Starting Azure Identity Validation

Checking Installation Requirements: Fail
Error Details for Service Administrator Account admin@contoso.onmicrosoft.com
The password for account  has expired or is a temporary password that needs to be reset before continuing. Run Login-AzureRMAccount, login with  credentials and follow the prompts to reset.
Additional help URL https://aka.ms/AzsRemediateAzureIdentity

Finished Azure Identity Validation

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsAzureIdentityValidation Completed
```

**Neden** -hesap parolası ya da süresi dolmuş veya geçici olduğundan oturum açamazsınız.

**Çözüm** - PowerShell'de aşağıdaki komutu çalıştırın ve ardından parolayı sıfırlamak için istemleri izleyin:

```powershell
Login-AzureRMAccount
```

Alternatif olarak, oturumu [Azure portalında](https://portal.azure.com) gibi hesap sahibini ve kullanıcı parolasını değiştirmeye zorlanır.

### <a name="unknown-user-type"></a>Bilinmeyen kullanıcı türü

```shell
Invoke-AzsAzureIdentityValidation v1.1809.1005.1 started.
Starting Azure Identity Validation

Checking Installation Requirements: Fail
Error Details for Service Administrator Account admin@contoso.onmicrosoft.com
Unknown user type detected. Check the account  is valid for AzureChinaCloud
Additional help URL https://aka.ms/AzsRemediateAzureIdentity

Finished Azure Identity Validation

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsAzureIdentityValidation Completed
```

**Neden** -belirtilen Azure Active Directory hesabı oturum açılamıyor (**AADDirectoryTenantName**). Bu örnekte, **AzureChinaCloud** olarak belirtilen **AzureEnvironment**.

**Çözüm** -hesap belirtilen Azure ortam için geçerli olduğunu doğrulayın. PowerShell'de, hesap için belirli bir ortama geçerli olduğunu doğrulamak için aşağıdaki komutu çalıştırın:

```powershell
Login-AzureRmAccount –EnvironmentName AzureChinaCloud
```

### <a name="account-is-not-an-administrator"></a>Hesap bir yönetici değil

```shell
Invoke-AzsAzureIdentityValidation v1.1809.1005.1 started.
Starting Azure Identity Validation

Checking Installation Requirements: Fail
Error Details for Service Administrator Account admin@contoso.onmicrosoft.com
The Service Admin account you entered 'admin@contoso.onmicrosoft.com' is not an administrator of the Azure Active Directory tenant 'contoso.onmicrosoft.com'.
Additional help URL https://aka.ms/AzsRemediateAzureIdentity

Finished Azure Identity Validation

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsAzureIdentityValidation Completed
```

**Neden** -hesap başarıyla oturum açabilir ancak hesap Azure Active Directory Yöneticisi değil (**AADDirectoryTenantName**).  

**Çözümleme** -oturum [Azure portalında](https://portal.azure.com) hesap sahibi olarak Git **Azure Active Directory**, ardından **kullanıcılar**, ardından **seçin Kullanıcı**, ardından **dizin rolü**ve ardından kullanıcı olun bir **genel yönetici**. Hesabı ise bir **kullanıcı**Git **Azure Active Directory** > **özel etki alanı adları**ve adı, için sağlanan dize onaylayın  **AADDirectoryTenantName** bu dizin için birincil etki alanı adı olarak işaretlenir. Bu örnekte, olan **contoso.onmicrosoft.com**.

Azure Stack etki alanı adının birincil etki alanı adı olmasını gerektirir.

## <a name="next-steps"></a>Sonraki Adımlar

[Azure kaydı doğrula](azure-stack-validate-registration.md)  
[Hazırlık raporunu görüntüle](azure-stack-validation-report.md)  
[Genel Azure Stack tümleştirme konuları](azure-stack-datacenter-integration.md)  
