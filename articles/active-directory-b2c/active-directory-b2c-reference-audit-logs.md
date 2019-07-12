---
title: Denetim, Azure Active Directory B2C'de örnekleri ve tanımları günlükleri | Microsoft Docs
description: Kılavuz ve örnekler, Azure AD B2C denetim günlüklerine erişim.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 08/04/2017
ms.author: marsma
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: 216f5413ce3dae1f2d040643a30a4d7db4a879b8
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67835408"
---
# <a name="accessing-azure-ad-b2c-audit-logs"></a>Azure AD B2C erişim denetim günlükleri

Azure Active Directory B2C (Azure AD B2C) verilen belirteçleri ve yönetici erişimi, B2C kaynaklara ilişkin etkinlik bilgileri içeren denetim günlüklerini gösterir. Bu makalede, Azure AD B2C kiracınız için bu verileri nasıl denetim günlüklerini ve yönergeler aracılığıyla sunulan bilgi kısa bir genel bakış sağlar.

> [!IMPORTANT]
> Denetim günlükleri, yalnızca yedi gün boyunca saklanır. İndirmek ve bir daha uzun bekletme süreleri gerektirip gerektirmediğini aşağıda gösterilen yöntemlerden birini kullanarak günlüklerinizi depolamak planlayın.

> [!NOTE]
> Kullanıcı oturum açma işlemleri için tek tek Azure AD B2C uygulamalarının altında göremez **kullanıcılar** bölümünü **Azure Active Directory** veya **Azure AD B2C** dikey pencereleri. Oturum açma işlemleri vardır, kullanıcı etkinliğini gösterir, ancak kullanıcının oturum açtığı B2C uygulamasına geri bağıntılı olamaz. Denetim günlükleri için bu makaledeki daha anlatıldığı gibi kullanmanız gerekir.

## <a name="overview-of-activities-available-in-the-b2c-category-of-audit-logs"></a>Denetim günlükleri B2C kategorisinde kullanılabilen etkinlikler genel bakış
**B2C** kategorisi denetim günlüklerinde aşağıdaki etkinlik türlerini içerir:

|Etkinlik türü |Açıklama  |
|---------|---------|
|Authorization |Yetkilendirme, bir kullanıcının erişim B2C kaynaklara (örneğin, yönetici B2C ilkelerinin bir listesini erişme) ilgili etkinlikleri         |
|Dizin |Azure portalını kullanarak bir yönetici kapattığında alınan dizin öznitelikleri ilgili etkinlikleri |
|Uygulama | B2C uygulamaları CRUD işlemleri |
|Anahtar |B2C anahtar kapsayıcısı içinde depolanan anahtarları CRUD işlemleri |
|Resource |B2C kaynaklarını (örneğin, ilkeleri ve kimlik sağlayıcıları) CRUD işlemleri
|Authentication |Belirteç verme ile kullanıcı kimlik bilgilerini ve doğrulama|

> [!NOTE]
> Kullanıcı nesnesi CRUD etkinlikleri için başvurmak **çekirdek dizin** kategorisi.

## <a name="example-activity"></a>Örnek etkinlik
Aşağıdaki örnek, bir kullanıcı bir dış kimlik sağlayıcısı oturum açtığında yakalanan verilerini gösterir: ![Azure portalında denetim günlüğü Etkinlik Ayrıntıları sayfası örneği](./media/active-directory-b2c-reference-audit-logs/audit-logs-example.png)

Etkinlik Ayrıntılar paneli aşağıdaki ilgili bilgileri içerir:

|`Section`|Alan|Açıklama|
|-------|-----|-----------|
| Etkinlik | Ad | Hangi etkinlik yapıldı. Örneğin, "bir id_token uygulamaya verme" (hangi sonucuna asıl kullanıcı oturum açma). |
| Başlatan (aktör) | ObjectId | **Nesne kimliği** B2C uygulamasının kullanıcı için oturum açmak için (Bu tanımlayıcıyı Azure portalında görünür değildir ancak örnek Graph API'si ile erişilebilir durumda). |
| Başlatan (aktör) | Spn | **Uygulama kimliği** kullanıcı için oturum açmak için B2C uygulaması. |
| Hedefler | ObjectId | **Nesne kimliği** oturum açmak için kullanıcının. |
| Ek Ayrıntılar | TenantId | **Kiracı kimliği** Azure AD B2C kiracısının. |
| Ek Ayrıntılar | `PolicyId` | **İlke kimliği** kullanıcının oturum açmak için kullanılan kullanıcı Akış (ilke). |
| Ek Ayrıntılar | ApplicationId | **Uygulama kimliği** kullanıcı için oturum açmak için B2C uygulaması. |

## <a name="accessing-audit-logs-through-the-azure-portal"></a>Azure portalı üzerinden denetim günlüklerine erişme
1. [Azure Portal](https://portal.azure.com) gidin. B2C dizininizde olduğundan emin olun.
2. Tıklayarak **Azure Active Directory** soldaki Sık Kullanılanlar çubuğuna

    ![Azure Active Directory düğmesi portalında sol taraftaki menüde](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-aad.png)

1. Altında **etkinlik**, tıklayarak **denetim günlükleri**

    ![Etkinlik menüsünün bölümünde denetim günlükleri düğmesi](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-section.png)

2. İçinde **kategori** dropbox, select **B2C**
3. Tıklayarak **Uygula**

    ![Kategori ve denetim günlüğü Filtresi Uygula düğmesi](./media/active-directory-b2c-reference-audit-logs/audit-logs-portal-category.png)

Son yedi gün günlüğe kaydedilen etkinlikler listesini görürsünüz.
- Kullanım **etkinlik kaynağı türü** yukarıda özetlenen etkinliği türlerine göre filtre uygulamak için açılır
- Kullanım **tarih aralığı** gösterilen etkinlikler tarih aralığını filtre uygulamak için açılır
- Bir bağlamsal kutusu sağdaki listede belirli bir satıra tıklarsanız etkinliği ile ilişkili ek öznitelikler gösterir
- Tıklayarak **indirme** etkinlikleri csv dosyası olarak karşıdan yüklemek için

> [!NOTE]
> Denetim günlüklerini giderek atabilirsiniz **Azure AD B2C** yerine **Azure Active Directory** soldaki Sık Kullanılanlar çubuğuna. Altında **etkinlikleri**, tıklayarak **denetim günlükleri**, benzer filtreleme yetenekleri ile aynı günlükleri burada bulabilirsiniz.

## <a name="accessing-audit-logs-through-the-azure-ad-reporting-api"></a>Azure AD raporlama API'sini kullanarak denetim günlüklerine erişme
Denetim günlükleri yayımlanır diğer etkinlikler aynı işlem hattı için Azure Active Directory aracılığıyla erişilebilir şekilde [Azure Active Directory raporlama API'SİYLE](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-audit-reference).

### <a name="prerequisites"></a>Önkoşullar
Azure AD raporlama API'si ile kimlik doğrulaması yapmak için önce bir uygulamayı kaydetmeniz gerekir. Adımları izlediğinizden emin olun [Azure AD raporlama API'lerini erişmek için Önkoşullar](https://azure.microsoft.com/documentation/articles/active-directory-reporting-api-getting-started/).

### <a name="accessing-the-api"></a>API erişimi
API aracılığıyla Azure AD B2C denetim günlükleri indirmek için günlüklere filtrelemek isteyebilirsiniz **B2C** kategorisi. Kategoriye göre filtrelemek için sorgu dizesi parametresi Azure AD raporlama API'si uç noktası, aşağıda gösterildiği gibi çağırırken kullanın:

`https://graph.windows.net/your-b2c-tentant.onmicrosoft.com/activities/audit?api-version=beta&$filter=category eq 'B2C'`

### <a name="powershell-script"></a>PowerShell betiği
Aşağıdaki betiği, Azure AD raporlama API'si sorgulama ve sonuçları bir JSON dosyası olarak depolamak için PowerShell kullanarak bir örnek sağlar:

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
    Do {
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
