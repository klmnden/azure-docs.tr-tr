---
title: Azure Active Directory oturum açma etkinlik raporu API'si örnekleri | Microsoft Docs
description: Azure Active Directory Raporlama API'sini kullanmaya başlama
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: c41c1489-726b-4d3f-81d6-83beb932df9c
ms.service: active-directory
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 05/31/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: b02c0ac482c13868606674a097a84976780b7ef6
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "41920632"
---
# <a name="azure-active-directory-sign-in-activity-report-api-samples"></a>Azure Active Directory oturum açma etkinlik raporu API'si örnekleri
Bu makale, Azure Active Directory raporlama API'si hakkındaki makale koleksiyonunun bir parçasıdır.  
Azure AD raporlama, kod veya ilgili araçları kullanarak oturum açma etkinliği verilerine erişmenizi sağlayan bir API sunar.  
Bu makalenin kapsamı, **oturum açma etkinliği API'si** için örnek kod sağlamaktır.

Bkz.

* Kavram hakkında daha fazla bilgi için [denetim günlükleri](overview-reports.md#activity-reports)
* Raporlama API'si hakkında daha fazla bilgi için [Azure Active Directory Raporlama API'sini kullanmaya başlama](concept-reporting-api.md).


## <a name="prerequisites"></a>Ön koşullar
Bu makaledeki örnekleri kullanabilmeniz için [Azure AD raporlama API'sine erişim önkoşulları](howto-configure-prerequisites-for-reporting-api.md).  

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




## <a name="executing-the-script"></a>Betiği derleme
Betiği düzenlemeyi tamamladıktan sonra çalıştırın ve oturum açma günlükleri raporundan beklenen verilerin döndürüldüğünden emin olun.

Betik, oturum açma raporundan JSON biçiminde çıkış döndürür. Ayrıca aynı çıkışla bir `SignIns.json` dosyası da oluşturur. Betiği farklı raporlardan veri döndürecek şekilde değiştirebilir ve ihtiyacınız olmayan çıkış biçimlerini açıklama satırına döndürebilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
* Bu makaledeki örnekleri özelleştirmek ister misiniz? [Azure Active Directory oturum açma etkinliği API'si referansına](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin) bakın. 
* Azure Active Directory raporlama API'sini kullanma konusunda ayrıntılı inceleme için bkz. [Azure Active Directory raporlama API'sini kullanmaya başlama](concept-reporting-api.md).
* Azure Active Directory raporlama hakkında daha fazla bilgi için bkz. [Azure Active Directory Raporlama Kılavuzu](overview-reports.md).  

