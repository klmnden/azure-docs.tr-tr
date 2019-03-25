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
ms.topic: conceptual
ms.date: 03/23/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 03/23/2019
ms.openlocfilehash: a777fc1d9052eb58bbebd319fe6cc7f42a09cb9a
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403621"
---
# <a name="validate-azure-registration"></a>Azure kaydı doğrula

Azure Stack hazırlık Denetleyicisi aracını kullanın (**AzsReadinessChecker**), Azure aboneliğinizin Azure Stack ile kullanmak hazır olduğunu doğrulamak için. Azure Stack dağıtıma başlamadan önce kayıt doğrulayın. Hazırlık Denetleyicisi'ni, doğrular:

- Kullandığınız Azure aboneliği desteklenen bir türdür. Abonelikler, bir bulut hizmeti sağlayıcısı (CSP) veya Kurumsal Anlaşma (EA) olmalıdır.
- Aboneliğinizin Azure ile kaydolmak için kullandığınız hesap Azure'da oturum açabilir ve bir abonelik sahibi.

Azure Stack kayıt hakkında daha fazla bilgi için bkz: [kaydetme Azure Stack Azure ile](azure-stack-registration.md).

## <a name="get-the-readiness-checker-tool"></a>Hazırlık Denetleyicisi Aracı'nı edinin

En son sürümünü indirin **AzsReadinessChecker** gelen [PowerShell Galerisi](https://aka.ms/AzsReadinessChecker).  

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki Önkoşullar gereklidir:

**Aracın çalıştığı bilgisayarda:**

- Windows 10 veya Windows Server 2016, internet bağlantısı.
- PowerShell 5.1 veya üzeri. Sürümünüzü denetlemek için aşağıdaki PowerShell cmdlet'ini çalıştırın ve daha sonra gözden **ana** ve **küçük** sürümleri:  

  ```powershell
  $PSVersionTable.PSVersion
  ```

- [Azure Stack için yapılandırılmış PowerShell](azure-stack-powershell-install.md).
- En son sürümünü [Microsoft Azure Stack hazırlık denetleyicisi](https://aka.ms/AzsReadinessChecker).  

**Azure Active Directory ortamı:**

- Kullanıcı adı ve parola, Azure Stack ile kullandığınız Azure aboneliği sahibi olan bir hesap için bu seçeneği belirleyin.  
- Abonelik kimliği için kullanacağınız Azure aboneliği tanımlayın.
- Tanımlamak **AzureEnvironment** kullanacaksınız. Ortam adı parametresi için desteklenen değerler şunlardır: **AzureCloud**, **AzureChinaCloud**, veya **AzureUSGovernment**işiniz hangi Azure aboneliğine bağlı olarak kullanıyor.

## <a name="steps-to-validate-azure-registration"></a>Azure kaydı doğrulamak için adımlar

1. Önkoşulları karşılayan bir bilgisayarda, yükseltilmiş bir PowerShell istemi açın ve ardından yüklemek için aşağıdaki komutu çalıştırın **AzsReadinessChecker**:

   ```powershell
   Install-Module Microsoft.AzureStack.ReadinessChecker -Force
   ```

2. PowerShell isteminden ayarlamak için aşağıdaki komutu çalıştırın `$registrationCredential` abonelik sahibi olan hesabının olarak. Değiştirin `subscriptionowner@contoso.onmicrosoft.com` hesabı ve Kiracı adınızla:

   ```powershell
   $registrationCredential = Get-Credential subscriptionowner@contoso.onmicrosoft.com -Message "Enter Credentials for Subscription Owner"
   ```

   > [!NOTE]
   > Paylaşılan hizmetler veya IUR aboneliklerini kullanırken bir CSP, bu ilgili AAD'den bir kullanıcının kimlik bilgilerini sağlamanız gerekir. Genellikle bu şuna benzeyecektir `subscriptionowner@iurcontoso.onmicrosoft.com`. Kullanıcı, önceki adımda açıklandığı gibi uygun kimlik bilgileri olmalıdır.

3. PowerShell isteminden ayarlamak için aşağıdaki komutu çalıştırın `$subscriptionID` olarak kullanılacak Azure aboneliği. Değiştirin `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` kendi abonelik kimliği:

   ```powershell
   $subscriptionID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
   ```

4. PowerShell isteminden aboneliğinizin doğrulamayı başlatmak için aşağıdaki komutu çalıştırın:

   - Değer için `AzureEnvironment` olarak **AzureCloud**, **AzureGermanCloud**, veya **AzureChinaCloud**.  
   - Azure Active Directory yöneticiniz ve Azure Active Directory Kiracı adınızı sağlayın.

   ```powershell
   Invoke-AzsRegistrationValidation -RegistrationAccount $registrationCredential -AzureEnvironment AzureCloud -RegistrationSubscriptionID $subscriptionID
   ```

5. Aracı çalıştırıldıktan sonra çıkışını gözden geçirin. Durum hem oturum açma ve kayıt gereksinimleri için doğru olduğunu onaylayın. Doğrulama başarılı çıkış aşağıdaki örneğe benzer görünür:

   ```shell
   Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
   Checking Registration Requirements: OK
   Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
   Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
   Invoke-AzsRegistrationValidation Completed
   ```

## <a name="report-and-log-file"></a>Rapor ve günlük dosyası

Her zaman doğrulama çalışır ve sonuçları günlükleri **AzsReadinessChecker.log** ve **AzsReadinessCheckerReport.json**. PowerShell'de doğrulama sonuçları birlikte bu dosyalarının konumunu görüntüler.

Bu dosyalar yardımcı olabilecek Azure Stack dağıtma veya doğrulama sorunları araştırmak için önce doğrulama durumu paylaşın. Her iki dosya her sonraki doğrulama denetimi sonuçlarını kalıcı hale getirin. Rapor dağıtım takım onayınız kimlik yapılandırması sağlar. Günlük dosyası dağıtım veya destek takım doğrulama sorunları araştırmanıza yardımcı olabilir.

Varsayılan olarak, her iki dosya için yazılan **C:\Users\<kullanıcıadı > \AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json**.  

- Kullanım **- OutputPath** ***&lt;yolu&gt;*** sonunda, farklı rapor konumunu belirtmek için çalışma komut satırı parametresi.
- Kullanım **- CleanReport** sonunda aracından, önceki çalıştırmaları hakkında bilgi temizlemek için bir run komutu, parametre **AzsReadinessCheckerReport.json**.

Daha fazla bilgi için [Azure Stack doğrulama raporu](azure-stack-validation-report.md).

## <a name="validation-failures"></a>Doğrulama hataları

Doğrulama denetimi başarısız olursa, hata hakkındaki ayrıntılar PowerShell penceresinde görüntüler. Araç ayrıca bilgileri günlüğe kaydeder **AzsReadinessChecker.log** dosya.

Aşağıdaki örnekler, yaygın doğrulama hataları hakkında rehberlik sağlar:

### <a name="user-must-be-an-owner-of-the-subscription"></a>Kullanıcı, aboneliğin sahibi olmanız gerekir

```shell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail
Error Details for registration account admin@contoso.onmicrosoft.com:
The user admin@contoso.onmicrosoft.com is role(s) Reader for subscription 3f961d1c-d1fb-40c3-99ba-44524b56df2d. User must be an owner of the subscription to be used for registration.
Additional help URL https://aka.ms/AzsRemediateRegistration

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
```

**Neden** -hesap Azure aboneliğinin bir Yöneticisi değildir.

**Çözüm** -Azure Stack dağıtım kullanım için faturalandırılır Azure aboneliğinde yönetici olan bir hesap kullanın.

### <a name="expired-or-temporary-password"></a>Süresi dolmuş veya geçici parola

```shell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail
Error Details for registration account admin@contoso.onmicrosoft.com:
Checking Registration failed with: Retrieving TenantId for subscription [subscription ID] using account admin@contoso.onmicrosoft.com failed with AADSTS50055: Force Change Password.
Trace ID: [Trace ID]
Correlation ID: [Correlation ID]
Timestamp: 2018-10-22 11:16:56Z: The remote server returned an error: (401) Unauthorized.

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
```

**Neden** -parola ya da süresi dolmuş veya geçici olduğundan hesap, oturum açamazsınız.

**Çözüm** - PowerShell'de aşağıdaki komutu çalıştırın ve parolayı sıfırlamak için istemleri izleyin.

```powershell
Login-AzureRMAccount
```

Alternatif olarak, oturumu [Azure portalında](https://portal.azure.com) gibi hesap sahibini ve kullanıcı parolasını değiştirmeye zorlanır.

### <a name="unknown-user-type"></a>Bilinmeyen kullanıcı türü  

```shell
Invoke-AzsRegistrationValidation v1.1809.1005.1 started.
Checking Registration Requirements: Fail
Error Details for registration account admin@contoso.onmicrosoft.com:
Checking Registration failed with: Retrieving TenantId for subscription <subscription ID> using <account> failed with unknown_user_type: Unknown User Type

Log location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessChecker.log
Report location (contains PII): C:\Users\username\AppData\Local\Temp\AzsReadinessChecker\AzsReadinessCheckerReport.json
Invoke-AzsRegistrationValidation Completed
```

**Neden** -hesabı için belirtilen Azure Active Directory ortamını oturum açamazsınız. Bu örnekte, **AzureChinaCloud** olarak belirtilen **AzureEnvironment**.  

**Çözüm** -hesap belirtilen Azure ortam için geçerli olduğunu doğrulayın. PowerShell'de, hesap için belirli bir ortama geçerli olduğunu doğrulamak için aşağıdaki komutu çalıştırın:

```powershell
Login-AzureRmAccount -EnvironmentName AzureChinaCloud
```

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure kimlik doğrulama](azure-stack-validate-identity.md)
- [Hazırlık raporunu görüntüle](azure-stack-validation-report.md)
- [Genel Azure Stack tümleştirme konuları](azure-stack-datacenter-integration.md)
