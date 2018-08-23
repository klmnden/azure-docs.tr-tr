---
title: Azure Active Directory raporlama denetim API'si örnekleri | Microsoft Docs
description: Azure Active Directory Raporlama API'sini kullanmaya başlama
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: de8b8ec3-49b3-4aa8-93fb-e38f52c99743
ms.service: active-directory
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 05/30/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 345daaa8ac991cad9f098b317a9b59d52de73ead
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "41918357"
---
# <a name="azure-active-directory-reporting-audit-api-samples"></a>Azure Active Directory raporlama denetim API'si örnekleri
Bu makale, Azure Active Directory raporlama API'si hakkındaki makale koleksiyonunun bir parçasıdır.  
Azure AD raporlama, kod veya ilgili araçları kullanarak veri denetimi yapmanızı sağlayan bir API sunar.
Bu makalenin kapsamı, **denetim API'si** için örnek kod sağlamaktır.

Bkz.

* Kavram hakkında daha fazla bilgi için [denetim günlükleri](overview-reports.md#activity-reports)
* Raporlama API'si hakkında daha fazla bilgi için [Azure Active Directory Raporlama API'sini kullanmaya başlama](concept-reporting-api.md).

Sorularınız, sorunlarınız veya geri bildiriminiz için lütfen [AAD Raporlama Yardımı](mailto:aadreportinghelp@microsoft.com) ile iletişime geçin.


## <a name="prerequisites"></a>Ön koşullar
Bu makaledeki örnekleri kullanabilmeniz için [Azure AD raporlama API'sine erişim önkoşulları](howto-configure-prerequisites-for-reporting-api.md).  

## <a name="known-issue"></a>Bilinen sorun
Kiracınız AB bölgesindeyse App Auth çalışmaz. Biz bu sorunu giderene kadar Denetim API'sine erişmek için lütfen User Auth kullanın. 

## <a name="powershell-script"></a>PowerShell betiği


```powershell

# This script will require the Web Application and permissions setup in Azure Active Directory
$clientID       = "<appid>"             # ApplicationId
$clientSecret   = "<key>"         # Should be a ~44 character string insert your info here
$loginURL       = "https://login.windows.net/"
$tenantdomain   = "<domain>"            # For example, contoso.onmicrosoft.com
$msgraphEndpoint = "https://graph.microsoft.com"
$countOfAuditDocsToBeSavedInAFile = 2000
    
# Get an Oauth 2 access token based on client id, secret and tenant domain
$body       = @{grant_type="client_credentials";resource=$msgraphEndpoint;client_id=$clientID;client_secret=$clientSecret}
$oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body
    
if ($oauth.access_token -ne $null) {
    $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}
    
    $url = "$msgraphEndpoint/beta/auditLogs/directoryAudits"
    Write-Output "Fetching data using Uri: $url"
    $i=0
    $docCount=0
    Do{
        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)
        $jsonReport = ($myReport.Content | ConvertFrom-Json).value
        $fetchedRecordCount = $jsonReport.Count
        $docCount = $docCount + $fetchedRecordCount
        $totalFetchedRecordCount = $totalFetchedRecordCount + $fetchedRecordCount
        Write-Output "Fetched $fetchedRecordCount records and saved into Audits$i.json"
        if($docCount -le $countOfAuditDocsToBeSavedInAFile)
        {
            $myReport.Content | Out-File -FilePath Audits$i.json -append  -Force        
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

### <a name="executing-the-powershell-script"></a>Powershell betiğini yürütme
Komut dosyasını düzenlemeyi tamamladıktan sonra çalıştırın ve Denetim günlükleri raporundan beklenen verilerin döndürüldüğünden emin olun.

Betik, denetim raporundan JSON biçiminde çıkış döndürür. Ayrıca aynı çıkışla bir `Audits.json` dosyası da oluşturur. Betiği farklı raporlardan veri döndürecek şekilde değiştirebilir ve ihtiyacınız olmayan çıkış biçimlerini açıklama satırına döndürebilirsiniz.





## <a name="next-steps"></a>Sonraki adımlar
* Bu makaledeki örnekleri özelleştirmek ister misiniz? [Azure Active Directory denetim API'si referansına](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) bakın. 
* Azure Active Directory raporlama API'sini kullanma konusunda ayrıntılı inceleme için bkz. [Azure Active Directory raporlama API'sini kullanmaya başlama](concept-reporting-api.md).
* Azure Active Directory raporlama hakkında daha fazla bilgi için bkz. [Azure Active Directory Raporlama Kılavuzu](overview-reports.md).  

