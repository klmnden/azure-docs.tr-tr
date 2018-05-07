---
title: 'Azure Active Directory B2C: denetim günlüklerini örnekleri ve tanımları'
description: Kılavuzu ve Azure AD B2C denetim günlüklerini erişim örnekleri
services: active-directory-b2c
author: sromeroz
manager: sasubram
ms.author: sezambra
ms.service: active-directory-b2c
ms.topic: article
ms.workload: identity
ms.date: 08/04/2017
ms.openlocfilehash: 8cc48853b0677230c4e19df4f0ecd93ce88b119d
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
---
# <a name="accessing-azure-ad-b2c-audit-logs"></a>Denetim günlüklerini Azure AD B2C erişme

Azure Active Directory B2C (Azure AD B2C) belirteçleri ve yönetici erişimi verilen B2C kaynaklarla ilgili etkinlik bilgilerini içeren denetim günlüklerini yayar. Bu makalede, Azure AD B2C kiracınızın için bu verilere erişme konusunda denetim günlüklerini ve yönergeleri kullanılabilir bilgi kısa bir genel bakış sağlar.

> [!IMPORTANT]
> Denetim günlüklerini yalnızca yedi gün boyunca saklanır. Karşıdan yüklemek ve daha uzun bir bekletme dönemi gerektirip gerektirmediğini aşağıda gösterilen yöntemlerden birini kullanarak günlüklerinizi depolamak planlayın. 

##<a name="overview-of-activities-available-in-the-b2c-category-of-audit-logs"></a>Denetim günlüklerini B2C kategorisinde kullanılabilen etkinlikler genel bakış
**B2C** denetim günlüklerini kategorisinde aşağıdaki etkinlik türlerini içerir:
|Etkinlik türü |Açıklama  |
|---------|---------|
|Yetkilendirme |Bir kullanıcının erişim B2C kaynaklara yetkilendirme (örneğin, yönetici B2C ilkelerinin bir listesini erişme) ile ilgili etkinlikleri         |
|Dizin |Bir yönetici Azure Portalı'nı kullanarak oturum açtığında alınan dizin öznitelikleri ilgili etkinlikleri |
|Uygulama | CRUD işlemleri B2C uygulamalar hakkında |
|Anahtar |B2C anahtar kapsayıcısında depolanan anahtarlar CRUD işlemleri |
|Kaynak |CRUD işlemleri B2C kaynaklar (örneğin, ilkeleri ve kimlik sağlayıcıları)
|Kimlik Doğrulaması |Kullanıcı kimlik bilgilerini ve belirteç verme doğrulama|

> [!NOTE]
> Kullanıcı nesnesi CRUD etkinlikleri için başvurmak **çekirdek dizin** kategorisi.

##<a name="example-activity"></a>Örnek etkinlik
Aşağıdaki örnek, bir kullanıcı bir dış kimlik sağlayıcısı oturum açtığında Yakalanan veriler gösterir: ![denetim günlüklerini - örnek](./media/active-directory-b2c-reference-audit-logs/audit-logs-example.png)

##<a name="accessing-audit-logs-through-the-azure-portal"></a>Denetim günlüklerini Azure portalına erişim
1. [Azure Portal](https://portal.azure.com) gidin. B2C dizininizde olduğundan emin olun.
2. Tıklayın **Azure Active Directory** soldaki Sık Kullanılanlar çubuğuna 
    
    ![Denetim günlükleri - AAD düğmesi](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-aad.png)

1. Altında **etkinlik**, tıklayın **denetim günlükleri**

    ![Denetim günlükleri - Logs Bölümü](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-section.png)

2. İçinde **kategori** dropbox, select **B2C**
3. Tıklayın **Uygula**

    ![Denetim günlükleri - kategorisi](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-category.png)

Son yedi gün içinde oturum etkinliklerin listesini görürsünüz. 
- Kullanım **etkinlik kaynak türü** yukarıda özetlenen etkinlik türlerine göre filtre uygulamak için açılır
- Kullanım **tarih aralığı** gösterilen etkinlikleri tarih aralığını filtrelemek için açılır
- Bağlamsal kutusunun sağındaki listesinde belirli bir satırda tıklarsanız, etkinliği ile ilişkili ek öznitelikler gösterir
- Tıklayın **karşıdan** etkinlikleri bir csv dosyası olarak karşıdan yüklemek için

##<a name="accessing-audit-logs-through-the-azure-ad-reporting-api"></a>Denetim günlüklerini Azure AD raporlama API'si üzerinden erişme
Denetim günlüklerini yayımlanır diğer etkinlikler olarak aynı ardışık düzene Azure Active Directory aracılığıyla erişilebilir şekilde [Azure Active Directory raporlama API'si](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-api-audit-reference). 

###<a name="prerequisites"></a>Önkoşullar
Azure AD raporlama API'si ile kimlik doğrulaması yapmak için önce bir uygulama kaydetmeniz gerekir. İçindeki adımları takip ettiğinizden emin olun [Azure AD API'leri raporlama erişmek için Önkoşullar](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/).

###<a name="accesing-the-api"></a>API belirtilmemelidir
API aracılığıyla Azure AD B2C denetim günlükleri indirmek için günlükleri için filtre uygulamak istediğiniz **B2C** kategorisi. Kategoriye göre filtre uygulamak için Azure AD raporlama API uç noktası, aşağıda gösterildiği gibi çağrılırken sorgu dizesi parametresi kullanın:

`https://graph.windows.net/your-b2c-tentant.onmicrosoft.com/activities/audit?api-version=beta&$filter=category eq 'B2C'`

###<a name="powershell-script"></a>PowerShell betiği
Aşağıdaki komut dosyası Azure AD raporlama API'si sorgulayabilir ve sonuçları bir JSON dosyası olarak depolamak için PowerShell kullanmaya ilişkin bir örnek sağlar:

```powershell
# This script will require registration of a Web Application in Azure Active Directory (see https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/)

# Constants
$ClientID       = "your-client-application-id-here"       # Insert your application's Client ID, a Globally Unique ID (registered by Global Admin)
$ClientSecret   = "your-client-application-secret-here"   # Insert your application's Client Key/Secret string
$loginURL       = "https://login.microsoftonline.com"     
$tenantdomain   = "your-b2c-tenant.onmicrosoft.com"       # AAD B2C Tenant; for example, contoso.onmicrosoft.com
$resource       = "https://graph.windows.net"             # Azure AD Graph API resource URI
$7daysago       = "{0:s}" -f (get-date).AddDays(-7) + "Z" # Use 'AddMinutes(-5)' to decrement minutes, for example
Write-Output "Searching for events starting $7daysago"

# Create HTTP header, get an OAuth2 access token based on client id, secret and tenant domain
$body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

# Parse audit report items, save output to file(s): auditX.json, where X = 0 thru n for number of nextLink pages
if ($oauth.access_token -ne $null) {   
    $i=0
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
    $url = 'https://graph.windows.net/' + $tenantdomain + '/activities/audit?api-version=beta&$filter=category eq ''B2C''and activityDate gt ' + $7daysago 

    # loop through each query page (1 through n)
    Do{
        # display each event on the console window
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output ($event | ConvertTo-Json)
        }

        # save the query page to an output file
        Write-Output "Save the output to a file audit$i.json"
        $myReport.Content | Out-File -FilePath audit$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)
} else {
    Write-Host "ERROR: No Access Token"
}
```

