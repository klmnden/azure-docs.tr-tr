---
title: Kullanım raporlama API örnekleri ve Azure Active Directory B2C tanımlarında | Microsoft Docs
description: Kılavuz ve kullanıcılar, kimlik doğrulaması ve multi-Factor Authentication Azure AD B2C kiracısı üzerinde raporları almayı örnekler.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 08/04/2017
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: eb8c41f657d8901535ba284c147f4ca3aa86198d
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64703615"
---
# <a name="accessing-usage-reports-in-azure-ad-b2c-via-the-reporting-api"></a>Azure AD B2C raporlama API'si üzerinden kullanım raporlarında erişme

Azure Active Directory B2C (Azure AD B2C), kullanıcı oturum açma ve Azure multi-Factor Authentication göre kimlik doğrulaması sağlar. Kimlik doğrulaması uygulama ailenize son kullanıcılar için kimlik sağlayıcılar sağlanır. Kiracı, bunlar kaydetmek için kullanılan sağlayıcılar ve kimlik doğrulama sayısını türü tarafından kaydedilen kullanıcı sayısını bildiğinizde, aşağıdaki gibi sorulara yanıt verebilir:
* Son 10 gün içinde (örneğin, Microsoft veya LinkedIn hesabı) kimlik sağlayıcısının her türden kaç kullanıcının kayıtlı?
* Multi-Factor Authentication kullanarak kaç kimlik doğrulamaları geçen ay başarıyla tamamladınız mı?
* Bu ay kaç oturum-tabanlı kimlik doğrulamaları tamamlandı? Gün başına? Uygulama başına?
* Azure AD B2C kiracısı etkinliğim beklenen aylık maliyeti nasıl tahmin edebilirsiniz?

Bu makalede, kullanıcı, Faturalanabilir oturum-tabanlı kimlik doğrulaması ve multi-Factor Authentication sayısına göre faturalandırma etkinliğini bağlı raporlar odaklanır.


## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce bölümündeki adımları tamamlamanız gerekir [Azure AD raporlama API'lerini erişmek için Önkoşullar](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/). Bir uygulama oluşturmak için bir gizli dizi elde ve erişim izni hakları Azure AD B2C kiracınızın raporlar. *Bash betiği* ve *Python betiğini* örnekleri burada da sunulmaktadır burada. 

## <a name="powershell-script"></a>PowerShell betiği
Bu betik kullanarak dört kullanım raporlarını oluşturmayı gösterir. `TimeStamp` parametresi ve `ApplicationId` filtre.

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


## <a name="usage-report-definitions"></a>Kullanım rapor tanımları
* **tenantUserCount**: Kiracı tarafından her gün son 30 gün içinde kimlik sağlayıcısının türünü kullanıcıların sayısı. (İsteğe bağlı olarak, bir `TimeStamp` filtresi geçerli tarihin belirtilen tarihten itibaren kullanıcı sayısıyla sağlar). Raporu sağlar:
  * **TotalUserCount**: Tüm kullanıcı nesnelerinin sayısı.
  * **OtherUserCount**: Azure Active Directory Kullanıcıları (Azure AD B2C kullanıcıları değil) sayısı.
  * **LocalUserCount**: Azure AD B2C kullanıcı hesabı kimlik bilgileriyle Azure AD B2C kiracısı için yerel olarak oluşturulmuş sayısı.

* **AlternateIdUserCount**: Azure AD B2C kullanıcı sayısı ile dış kimlik sağlayıcısı kayıtlı (örneğin, Facebook, Microsoft hesabı veya başka bir Azure Active Directory kiracısı, ayrıca olarak adlandırılan bir `OrgId`).

* **b2cAuthenticationCountSummary**: Günlük sayısını gün ve kimlik doğrulama akışı bir tür tarafından son 30 gün içindeki Faturalanabilir kimlik doğrulama özeti.

* **b2cAuthenticationCount**: Kimlik doğrulamaları bir süre içinde sayısı. Son 30 gün varsayılandır.  (İsteğe bağlı: Başlangıç ve bitiş `TimeStamp` parametreleri tanımlayan belirli bir süre.) Çıktı içerir `StartTimeStamp` (Bu Kiracı için etkinlik en erken tarihi) ve `EndTimeStamp` (son güncelleştirme).

* **b2cMfaRequestCountSummary**: Günlük sayısını gün ve (SMS veya sesli) türüne göre çok faktörlü kimlik doğrulama özeti.


## <a name="limitations"></a>Sınırlamalar
Kullanıcı sayısı veriler için 24 48 saatte bir yenilenir. Kimlik doğrulamaları günde birkaç kez güncelleştirilir. Kullanırken `ApplicationId` filtre, bir boş rapor yanıt olabilir aşağıdaki koşullardan biri nedeniyle:
  * Kiracıda uygulama kimliği yok. Doğru olduğundan emin olun.
  * Uygulama kimliği var, ancak raporlama dönemi içindeki hiçbir veri bulunamadı. Tarih/saat parametrelerinizi inceleyin.


## <a name="next-steps"></a>Sonraki adımlar
### <a name="monthly-bill-estimates-for-azure-ad"></a>Azure AD için aylık faturanızda tahminleri
İle birlikte kullanıldığında [en güncel Azure AD B2C kullanılabilir fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory-b2c/), günlük, haftalık ve aylık Azure tüketim tahmin edebilirsiniz.  Genel maliyeti etkileyebilecek Kiracı davranışında değişiklik için planlama yaparken, tahmini özellikle yararlıdır. Gerçek maliyetlerini gözden geçirebilirsiniz, [bağlı bir Azure aboneliği](active-directory-b2c-how-to-enable-billing.md).

### <a name="options-for-other-output-formats"></a>Diğer çıkış biçimleri için seçenekleri
Aşağıdaki kod örnekleri, JSON, adı değeri listesi ve XML çıkışı göndermek gösterir:
```powershell
# to output to JSON use following line in the PowerShell sample
$myReport.Content | Out-File -FilePath name-your-file.json -Force

# to output the content to a name value list
($myReport.Content | ConvertFrom-Json).value | Out-File -FilePath name-your-file.txt -Force

# to output the content in XML use the following line
(($myReport.Content | ConvertFrom-Json).value | ConvertTo-Xml).InnerXml | Out-File -FilePath name-your-file.xml -Force
```
