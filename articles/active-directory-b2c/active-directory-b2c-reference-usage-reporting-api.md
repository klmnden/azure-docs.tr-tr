---
title: 'Azure Active Directory B2C: Kullanım raporlama API örnekleri ve tanımları | Microsoft Docs'
description: Kılavuzu ve kullanıcıları, kimlik doğrulamaları ve çok faktörlü kimlik doğrulamaları Azure AD B2C kiracısı üzerinde raporları almayı örnekleri
services: active-directory-b2c
documentationcenter: dev-center-name
author: davidmu1
manager: mtillman
editor: ''
ms.service: active-directory-b2c
ms.topic: article
ms.workload: identity
ms.date: 08/04/2017
ms.author: davidmu
ms.openlocfilehash: 07029181423927f0796cb85e728df416d01466e8
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="accessing-usage-reports-in-azure-ad-b2c-via-the-reporting-api"></a>Raporlama API'si aracılığıyla Azure AD B2C kullanım raporlarında erişme

Azure Active Directory B2C (Azure AD B2C) kullanıcı oturum açma ve Azure multi-Factor Authentication göre kimlik doğrulaması sağlar. Kimlik doğrulaması uygulama ailenizin son kullanıcılar için kimlik sağlayıcıları sağlanır. Kiracı, kaydettirmek için kullanılan sağlayıcıları ve kimlik doğrulama sayısını türüne göre kayıtlı kullanıcıların sayısını bildiğinizde gibi sorulara cevap verebilirsiniz.
* Kimlik sağlayıcısı (örneğin, bir Microsoft ya da LinkedIn hesabı) her tür kaç kullanıcının son 10 gün içinde kayıtlı?
* Çok faktörlü kimlik doğrulaması kullanarak kaç kimlik doğrulamaları son bir ayda başarıyla tamamladınız mı?
* Kaç tane oturum bileşen tabanlı kimlik doğrulamaları bu ay tamamlandığını? Gün başına? Uygulama başına?
* Azure AD B2C Kiracı etkinlik beklenen aylık maliyetini nasıl tahmin edebilirsiniz?

Bu makalede, kullanıcılar, Faturalanabilir oturum bileşen tabanlı kimlik doğrulamaları ve çok faktörlü kimlik doğrulamaları sayısına göre fatura etkinliğine bağlı raporları odaklanır.


## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce bölümündeki adımları tamamlamanız gerekir [Azure AD API'leri raporlama erişmek için Önkoşullar](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/). Bir uygulama oluşturmak, bir gizlilik elde ve erişim izni Azure AD B2C kiracınızın raporları hakkı. *Komut dosyası bash* ve *Python betiği* örnekler da sağlanmıştır burada. 

## <a name="powershell-script"></a>PowerShell betiği
Bu komut dosyasını kullanarak dört kullanım raporları oluşturulmasını gösterir `TimeStamp` parametre ve `ApplicationId` Filtresi.

```powershell
# This script will require the Web Application and permissions setup in Azure Active Directory

# Constants
$ClientID      = "your-client-application-id-here"  
$ClientSecret  = "your-client-application-secret-here"
$loginURL      = "https://login.microsoftonline.com"
$tenantdomain  = "your-b2c-tenant-domain.onmicrosoft.com"  
# Get an Oauth 2 access token based on client id, secret and tenant domain
$body          = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth         = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body
if ($oauth.access_token -ne $null) {
    $headerParams  = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    Write-host Data from the tenantUserCount report
    Write-host ====================================================
     # Returns a JSON document for the report
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the tenantUserCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/tenantUserCount?%24filter=TimeStamp+gt+2016-10-15&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCountSummary report
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=TimeStamp+gt+2016-09-20+and+TimeStamp+lt+2016-10-03&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cAuthenticationCount report with ApplicationId filter
    Write-host ====================================================
    # Returns a JSON document for the " " report
        $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cAuthenticationCount?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCountSummary
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCount report with datetime filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCount?%24filter=TimeStamp+gt+2016-09-10+and+TimeStamp+lt+2016-10-04&api-version=beta")
    Write-host $myReport.Content

    Write-host Data from the b2cMfaRequestCount report with ApplicationId filter
    Write-host ====================================================
    $myReport = (Invoke-WebRequest -Headers $headerParams -Uri "https://graph.windows.net/$tenantdomain/reports/b2cMfaRequestCountSummary?%24filter=ApplicationId+eq+ada78934-a6da-4e69-b816-10de0d79db1d&api-version=beta")
     Write-host $myReport.Content

} else {
    Write-Host "ERROR: No Access Token"
    }
```


## <a name="usage-report-definitions"></a>Kullanım Raporu tanımları
* **tenantUserCount**: Kiracı son 30 gün içinde gün başına kimlik sağlayıcısı türü tarafından kullanıcıların sayısı. (İsteğe bağlı olarak, bir `TimeStamp` filtre geçerli tarih için belirtilen bir tarihten itibaren kullanıcı sayısına sağlar). Rapor sağlar:
  * **TotalUserCount**: tüm kullanıcı nesnelerinin sayısı.
  * **OtherUserCount**: Azure Active Directory Kullanıcıları (Azure AD B2C kullanıcılara değil) sayısı.
  * **LocalUserCount**: Azure AD B2C kullanıcı hesaplarının sayısı kimlik bilgileriyle Azure AD B2C kiracısı yerel oluşturuldu.

* **AlternateIdUserCount**: Azure AD B2C kullanıcı sayısını kayıtlı olan dış kimlik sağlayıcısı (örneğin, Facebook, bir Microsoft hesabı veya başka bir Azure Active Directory Kiracı da denir bir `OrgId`).

* **b2cAuthenticationCountSummary**: Özet Faturalanabilir kimlik doğrulamaları gün ve kimlik doğrulama akışı türü tarafından son 30 gün boyunca günlük sayısı.

* **b2cAuthenticationCount**: kimlik doğrulamaları bir süre içinde sayısı. Son 30 gün varsayılandır.  (İsteğe bağlı: Başlangıç ve bitiş `TimeStamp` parametreleri tanımlayan belirli bir zaman dilimi.) Çıktı içerir `StartTimeStamp` (Bu Kiracı için etkinliğin en erken tarihi) ve `EndTimeStamp` (son güncelleştirme).

* **b2cMfaRequestCountSummary**: gün ve türüne (SMS veya sesli) göre çok faktörlü kimlik doğrulama günlük sayısını özeti.


## <a name="limitations"></a>Sınırlamalar
Kullanıcı sayısı verileri 48 için 24 saatte bir yenilenir. Kimlik doğrulamaları günde birkaç kez güncelleştirilir. Kullanırken `ApplicationId` filtre, bir boş rapor yanıt olabilir aşağıdaki koşullardan biri nedeniyle:
  * Uygulama Kimliği Kiracı içinde yok. Doğru olduğundan emin olun.
  * Uygulama kimliği var, ancak hiçbir veri raporlama döneminde bulunamadı. Tarih/saat parametrelerinizi inceleyin.


## <a name="next-steps"></a>Sonraki adımlar
### <a name="monthly-bill-estimates-for-azure-ad"></a>Azure AD için aylık faturanızı tahmin eder
İle birleştirildiğinde [en güncel Azure AD B2C kullanılabilir fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory-b2c/), günlük, haftalık ve aylık Azure tüketim tahmin edebilirsiniz.  Genel maliyeti etkileyebilecek Kiracı davranış değişiklikleri planlarken tahmini özellikle yararlı olur. Gerçek maliyetlerini gözden geçirebilirsiniz, [Azure aboneliğine bağlı](active-directory-b2c-how-to-enable-billing.md).

### <a name="options-for-other-output-formats"></a>Diğer çıkış biçimleri için seçenekleri
Aşağıdaki kod örnekleri JSON, bir ad değeri listesi ve XML Çıkış göndermeyi gösterir:
```powershell
# to output to JSON use following line in the PowerShell sample
$myReport.Content | Out-File -FilePath name-your-file.json -Force

# to output the content to a name value list
($myReport.Content | ConvertFrom-Json).value | Out-File -FilePath name-your-file.txt -Force

# to output the content in XML use the following line
(($myReport.Content | ConvertFrom-Json).value | ConvertTo-Xml).InnerXml | Out-File -FilePath name-your-file.xml -Force
```
