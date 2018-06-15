---
title: Azure Active Directory oturum açma etkinliği raporu API örnekleri | Microsoft Docs
description: Azure Active Directory raporlama API'sini kullanmaya başlama
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: c41c1489-726b-4d3f-81d6-83beb932df9c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 05/31/2018
ms.author: dhanyahk;rolyon
ms.reviewer: dhanyahk
ms.openlocfilehash: 466755d7d1cc7fbf4006826ac849b74ba306bae9
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34698586"
---
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Azure Active Directory oturum açma etkinliği raporu API örnekleri
Bu makalede Azure Active Directory'ı Raporlama API'si hakkında makale koleksiyonu bir parçasıdır.  
Azure AD raporlama kodu veya ilgili araçları kullanarak oturum açma etkinliği veri erişmenizi sağlayan bir API ile sağlar.  
Örnek kod için ile sağlamak için bu makalenin kapsamı olan **etkinlik API oturum aç**.

Bkz:

* [Denetim günlükleri](active-directory-reporting-azure-portal.md#activity-reports) daha fazla kavramsal bilgi için
* [Azure Active Directory raporlama API'si ile çalışmaya başlama](active-directory-reporting-api-getting-started.md) raporlama API'si hakkında daha fazla bilgi için.


## <a name="prerequisites"></a>Önkoşullar
Bu makaledeki örnekler kullanmadan önce tamamlanması gereken [Azure AD raporlama API'si erişmek için Önkoşullar](active-directory-reporting-api-prerequisites.md).  

## <a name="powershell-script"></a>PowerShell betiği

```powershell

# This script will require the Web Application and permissions setup in Azure Active Directory
$clientID       = "<appid>"             # ApplicationId
$clientSecret   = "<key>"         # Should be a ~44 character string insert your info here
$loginURL       = "https://login.windows.net/"
$tenantdomain   = "<domain>"            # For example, contoso.onmicrosoft.com
$msgraphEndpoint = "https://graph.microsoft.com"
$countOfSignInDocsToBeSavedInAFile = 2000
    
# Get an Oauth 2 access token based on client id, secret and tenant domain
$body       = @{grant_type="client_credentials";resource=$msgraphEndpoint;client_id=$clientID;client_secret=$clientSecret}
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body
    
if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
    
    $url = "$msgraphEndpoint/beta/auditLogs/signIns"
    Write-Output "Fetching data using Uri: $url"
    $i=0
    $docCount=0
    Do{
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        $jsonReport = ($myReport.Content | ConvertFrom-Json).value
        $fetchedRecordCount = $jsonReport.Count
        $docCount = $docCount + $fetchedRecordCount
        $totalFetchedRecordCount = $totalFetchedRecordCount + $fetchedRecordCount
        Write-Output "Fetched $fetchedRecordCount records and saved into SignIns$i.json"
        if($docCount -le $countOfSignInDocsToBeSavedInAFile)
        {
            $myReport.Content | Out-File -FilePath SignIns$i.json -append  -Force       
        }
        else
        {           
            $docCount=0
            $i = $i+1
        }
            
        #Get url from next link
        $url = ($myReport.Content | ConvertFrom-Json).'@odata.nextLink'         
    }while($url -ne $null)
    Write-Output "Total Fetched record count is : $totalFetchedRecordCount"
                
} else {
    Write-Host "ERROR: No Access Token"
}

```




## <a name="executing-the-script"></a>Komut dosyası yürütme
Komut dosyası düzenlemeyi tamamladıktan sonra çalıştırın ve oturum açma beklenen verilerden rapor günlüklerini doğrulayın döndürülür.

Komut çıktısı raporundan oturum açma JSON biçiminde döndürür. Ayrıca oluşturur bir `SignIns.json` aynı çıkış dosyası. Veri diğer raporlar ve yorum ihtiyacınız olmayan çıkış biçimleri çıkışı döndürmek için komut dosyasını değiştirerek deneyebilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
* Bu makaledeki örnekler özelleştirme ister misiniz? Kullanıma [Azure Active Directory oturum açma etkinliği API Başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin). 
* Azure Active Directory'ı Raporlama API'si kullanan bir tam genel bakış görmek istiyorsanız bkz [Azure Active raporlama API'si Directory ile çalışmaya başlama](active-directory-reporting-api-getting-started.md).
* Azure Active Directory raporlama hakkında daha fazla bilgi edinmek istiyorsanız, bkz: [Azure Active Directory raporlama Kılavuzu](active-directory-reporting-guide.md).  

