---
title: Azure İzleyici'de Azure Active Directory oturum açma günlüğü şeması | Microsoft Docs
description: Azure AD oturum açma kullanılmak üzere Azure izleyici günlüğü şeması açıklayın
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: a8ac6c56dca100ea9836158f46881c4eb12213e1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60285193"
---
# <a name="interpret-the-azure-ad-sign-in-logs-schema-in-azure-monitor"></a>Azure İzleyici Azure AD oturum açma günlükleri şemada yorumlama

Bu makalede, Azure İzleyici'de Azure Active Directory (Azure AD) oturum açma günlük şema açıklanır. Oturum açma işlemleri için ilgili bilgilerin çoğunu altında sağlanır *özellikleri* özniteliği `records` nesne.


```json
{ 
    "time": "2019-03-12T16:02:15.5522137Z", 
    "resourceId": "/tenants/<TENANT ID>/providers/Microsoft.aadiam",
    "operationName": "Sign-in activity", 
    "operationVersion": "1.0", 
    "category": "SignInLogs", 
    "tenantId": "<TENANT ID>", 
    "resultType": "50140", 
    "resultSignature": "None", 
    "resultDescription": "This error occurred due to 'Keep me signed in' interrupt when the user was signing-in.", 
    "durationMs": 0, 
    "callerIpAddress": "<CALLER IP ADDRESS>", 
    "correlationId": "a75a10bd-c126-486b-9742-c03110d36262", 
    "identity": "Timothy Perkins", 
    "Level": 4, 
    "location": "US", 
    "properties": 
        {
            "id":"0231f922-93fa-4005-bb11-b344eca03c01",
            "createdDateTime":"2019-03-12T16:02:15.5522137+00:00",
            "userDisplayName":"Timothy Perkins",
            "userPrincipalName":"<USER PRINCIPAL NAME>",
            "userId":"<USER ID>",
            "appId":"<APPLICATION ID>",
            "appDisplayName":"Azure Portal",
            "ipAddress":"<IP ADDRESS>",
            "status":
            {
                "errorCode":50140,
                "failureReason":"This error occurred due to 'Keep me signed in' interrupt when the user was signing-in."
            },
            "clientAppUsed":"Browser",
            "deviceDetail":
            {
                "operatingSystem":"Windows 10",
                "browser":"Chrome 72.0.3626"
            },
            "location":
                {
                    "city":"Bellevue",
                    "state":"Washington",
                    "countryOrRegion":"US",
                    "geoCoordinates":
                    {
                        "latitude":45,
                        "longitude":122
                    }
                },
            "correlationId":"a75a10bd-c126-486b-9742-c03110d36262",
            "conditionalAccessStatus":"notApplied",
            "appliedConditionalAccessPolicies":
            [
                {
                    "id":"ae11ffaa-9879-44e0-972c-7538fd5c4d1a",
                    "displayName":"Hr app access policy",
                    "enforcedGrantControls":
                    [
                        "Mfa"
                    ],
                    "enforcedSessionControls":
                    [
                    ],
                    "result":"notApplied"
                },
                {
                    "id":"b915a70b-2eee-47b6-85b6-ff4f4a66256d",
                    "displayName":"MFA for all but global support access",
                    "enforcedGrantControls":[],
                    "enforcedSessionControls":[],
                    "result":"notEnabled"
                },
                {
                    "id":"830f27fa-67a8-461f-8791-635b7225caf1",
                    "displayName":"Header Based Application Control",
                    "enforcedGrantControls":["Mfa"],
                    "enforcedSessionControls":[],
                    "result":"notApplied"
                },
                {
                    "id":"8ed8d7f7-0a2e-437b-b512-9e47bed562e6",
                    "displayName":"MFA for everyones",
                    "enforcedGrantControls":[],
                    "enforcedSessionControls":[],
                    "result":"notEnabled"
                },
                {
                    "id":"52924e0f-798b-4afd-8c42-49055c7d6395",
                    "displayName":"Device compliant",
                    "enforcedGrantControls":[],
                    "enforcedSessionControls":[],
                    "result":"notEnabled"
                },
             ],
            "isInteractive":true,
            "tokenIssuerType":"AzureAD",
            "authenticationProcessingDetails":[],
            "networkLocationDetails":[],
            "processingTimeInMilliseconds":0,
            "riskDetail":"hidden",
            "riskLevelAggregated":"hidden",
            "riskLevelDuringSignIn":"hidden",
            "riskState":"none",
            "riskEventTypes":[],
            "resourceDisplayName":"windows azure service management api",
            "resourceId":"797f4846-ba00-4fd7-ba43-dac1f8f63013",
            "authenticationMethodsUsed":[]
        }
}
```


## <a name="field-descriptions"></a>Alan açıklamaları

| Alan adı | Açıklama |
|------------|-------------|
| Zaman | Tarih ve UTC diliminde saat. |
| ResourceId | Bu değer eşlenmemiş ve bu alan güvenle yok sayabilirsiniz.  |
| OperationName | Oturum açma işlemleri için bu değer her zaman, *oturum açma etkinliği*. |
| operationVersion | İstemci tarafından istenen REST API sürümü. |
| Kategori | Oturum açma işlemleri için bu değer her zaman, *Signın*. | 
| TenantId | Kiracı günlükleri ile ilişkili olan GUID. |
| ResultType | Oturum açma işleminin sonucu olabilir *başarı* veya *hatası*. | 
| resultSignature | Hata kodu, oturum açma işlemi içerir. |
| ResultDescription | Oturum açma işlemi için hata açıklamasını sağlar. |
| süre (MS) |  Bu değer eşlenmemiş ve bu alan güvenle yok sayabilirsiniz.|
| callerIpAddress | İsteği gerçekleştiren istemcinin IP adresi. | 
| CorrelationId | İstemci tarafından geçirilen isteğe bağlı bir GUID. Hizmetleri span günlükleri izlerken yararlıdır ve bu değer, sunucu tarafı işlemleri performanstaki istemci tarafı işlemleri yardımcı olabilir. |
| Kimlik | İstek yapıldığında, sunulan belirteçten kimliği. Bir kullanıcı hesabı, sistem hesabı veya hizmet sorumlusu olabilir. |
| Düzey | İleti türü sağlar. Denetim için her zaman olduğu *bilgilendirici*. |
| Location | Oturum açma etkinliği konumunu sağlar. |
| Özellikler | Oturum açma ile ilişkili olan tüm özellikleri listeler. Daha fazla bilgi için [Microsoft Graph API Başvurusu](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin). Bu şema aynı öznitelik adları okunabilirlik için kaynak olarak kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici denetim günlükleri şemasını yorumlama](reference-azure-monitor-audit-log-schema.md)
* [Azure tanılama günlükleri hakkında daha fazla bilgi](../../azure-monitor/platform/diagnostic-logs-overview.md)