---
title: "Azure Active Directory oturum açma etkinliği raporu API örnekleri | Microsoft Docs"
description: "Azure Active Directory raporlama API'sini kullanmaya başlama"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: c41c1489-726b-4d3f-81d6-83beb932df9c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/15/2018
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 158f121880dbc1fc1495c8599367502ffd93ce20
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Azure Active Directory oturum açma etkinliği raporu API örnekleri
Bu konuda, Azure Active Directory hakkındaki konuları API raporlama koleksiyonu bir parçasıdır.  
Azure AD raporlama kodu veya ilgili araçları kullanarak oturum açma etkinliği veri erişmenizi sağlayan bir API ile sağlar.  
Örnek kod için ile sağlamak için bu konunun kapsamı olan **etkinlik API oturum aç**.

Bkz.:

* [Denetim günlükleri](active-directory-reporting-azure-portal.md#activity-reports) daha fazla kavramsal bilgi için
* [Azure Active Directory raporlama API'si ile çalışmaya başlama](active-directory-reporting-api-getting-started.md) raporlama API'si hakkında daha fazla bilgi için.


## <a name="prerequisites"></a>Önkoşullar
Bu konudaki örnekler kullanmadan önce tamamlanması gereken [Azure AD raporlama API'si erişmek için Önkoşullar](active-directory-reporting-api-prerequisites.md).  

## <a name="powershell-script"></a>PowerShell betiği
    # This script will require the Web Application and permissions setup in Azure Active Directory
    $ClientID       = "<clientId>"             # Should be a ~35 character string insert your info here
    $ClientSecret   = "<clientSecret>"         # Should be a ~44 character string insert your info here
    $loginURL       = "https://login.microsoftonline.com/"
    $tenantdomain   = "<tenantDomain>"
    $ daterange            # For example, contoso.onmicrosoft.com

    $7daysago = "{0:s}" -f (get-date).AddDays(-7) + "Z"
    # or, AddMinutes(-5)

    Write-Output $7daysago

    # Get an Oauth 2 access token based on client id, secret and tenant domain
    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}

    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

    $url = "https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&`$filter=signinDateTime ge $7daysago"

    $i=0

    Do{
        Write-Output "Fetching data using Uri: $url"
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        Write-Output "Save the output to a file SigninActivities$i.json"
        Write-Output "---------------------------------------------"
        $myReport.Content | Out-File -FilePath SigninActivities$i.json -Force
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'
        $i = $i+1
    } while($url -ne $null)

    } else {

        Write-Host "ERROR: No Access Token"
    }




## <a name="executing-the-script"></a>Komut dosyası yürütme
Komut dosyası düzenlemeyi tamamladıktan sonra çalıştırın ve denetim beklenen verilerden rapor günlüklerini doğrulayın döndürülür.

Komut çıktısı raporundan oturum açma JSON biçiminde döndürür. Ayrıca oluşturur bir `SigninActivities.json` aynı çıkış dosyası. Veri diğer raporlar ve yorum ihtiyacınız olmayan çıkış biçimleri çıkışı döndürmek için komut dosyasını değiştirerek deneyebilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
* Bu konudaki örnekler özelleştirme ister misiniz? Kullanıma [Azure Active Directory oturum açma etkinliği API Başvurusu](active-directory-reporting-api-sign-in-activity-reference.md). 
* Azure Active Directory'ı Raporlama API'si kullanan bir tam genel bakış görmek istiyorsanız bkz [Azure Active raporlama API'si Directory ile çalışmaya başlama](active-directory-reporting-api-getting-started.md).
* Azure Active Directory raporlama hakkında daha fazla bilgi edinmek istiyorsanız, bkz: [Azure Active Directory raporlama Kılavuzu](active-directory-reporting-guide.md).  

