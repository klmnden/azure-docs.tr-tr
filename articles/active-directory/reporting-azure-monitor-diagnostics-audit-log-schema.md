---
title: Azure İzleyici (Önizleme) Azure Active Directory denetim günlüğü şemada yorumlama | Microsoft Docs
description: Azure İzleyici (Önizleme) kullanmak için Azure AD denetim günlüğü şeması açıklayın
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: e1ae8e2a4dc9ef9c21300ebfc4df8c0f1c5819f2
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240013"
---
# <a name="interpret-the-azure-active-directory-audit-logs-schema-in-azure-monitor-preview"></a>Azure İzleyici (Önizleme) Azure Active Directory denetim günlüklerini şemada yorumlama

Bu makalede, Azure AD denetim günlüğü şeması Azure İzleyici'de açıklanmaktadır. Her ayrı bir günlük girişi olarak gösterildiği gibi bir JSON blobu olarak biçimlendirilip metin olarak depolanır iki örnek aşağıda. 

```json
{ 
    "records": [ 
    { 
        "time": "2018-03-17T00:14:31.2585575Z", 
        "operationName": "Change password (self-service)",
        "operationVersion": "1.0",
        "category": "Audit", 
        "tenantId": "bf85dc9d-cb43-44a4-80c4-469e8c58249e", 
        "resultType": "Success", 
        "resultSignature": "-1", 
        "resultDescription": "None", 
        "durationMs": "-1", 
        "correlationId": "60d5e89a-b890-413f-9e25-a047734afe9f", 
        "identity": "sreens@wingtiptoysonline.com", 
        "Level": "Informational", 
        "location": "WUS", 
        "properties": { 
            "identityType": "UPN", 
            "operationType": "Update", 
            "additionalDetails": "None", 
            "additionalTargets": "", 
            "targetUpdatedProperties": "", 
            "targetResourceType": "UPN__TenantContextID__PUID__ObjectID__ObjectClass", 
            "targetResourceName": "sreens@wingtiptoysonline.com__bf85dc9d-cb43-44a4-80c4-469e8c58249e__1003BFFD9FEB17DB__7a408bdd-7d97-4574-8511-dd747b56465d__User", 
            "auditEventCategory": "UserManagement" 
        } 
    } 
    ] 
} 
```

```json
{ 
    "records": [ 
    { 
        "time": "2018-03-18T19:47:43.0368859Z", 
        "operationName": "Update service principal.", 
        "operationVersion": "1.0", 
        "category": "Audit", 
        "tenantId": "bf85dc9d-cb43-44a4-80c4-469e8c58249e", 
        "resultType": "Success", 
        "resultSignature": "-1", 
        "durationMs": "-1", 
        "callerIpAddress": "<null>", 
        "correlationId": "14916c7a-5a7d-44e8-9b06-74b49efb08ee", 
        "identity": "NA", 
        "Level": "Informational", 
        "properties": { 
            "identityType": "NA", 
            "operationType": "Update", 
            "additionalDetails": {}, 
            "additionalTargets": "", 
            "targetUpdatedProperties": [ 
            { 
                "Name": "Included Updated Properties", 
                "OldValue": null, 
                "NewValue": "" 
            }, 
            { 
                "Name": "TargetId.ServicePrincipalNames", 
                "OldValue": null, 
                "NewValue": "http://adapplicationregistry.onmicrosoft.com/salesforce.com/primary;cd3ed3de-93ee-400b-8b19-b61ef44a0f29" 
            } 
            ], 
        "targetResourceType": "Other__ObjectID__ObjectClass__Name__AppId__SPN", 
        "targetResourceName": "ServicePrincipal_ea70a262-4da3-440a-b396-9734ddfd9df2__ea70a262-4da3-440a-b396-9734ddfd9df2__ServicePrincipal__Salesforce__cd3ed3de-93ee-400b-8b19-b61ef44a0f29__http://adapplicationregistry.onmicrosoft.com/salesforce.com/primary;cd3ed3de-93ee-400b-8b19-b61ef44a0f29", 
        "auditEventCategory": "ApplicationManagement" 
      } 
    } 
    ] 
} 
```

| Alan adı | Açıklama |
|------------|-------------|
| time       | Tarih ve saat (UTC) |
| operationName | İşlemin adı |
| operationVersion | İstemci tarafından istenen REST API sürümü |
| category | Şu anda *denetim* desteklenen tek değerdir |
| Kiracı kimliği | Günlükleri ile ilişkili Kiracı GUID |
| resultType | Neden işlemi, olabilir *başarı* veya *hatası* |
| resultSignature |  Eşlenmemiş budur ve bu alan güvenle yok sayabilirsiniz. | 
| resultDescription | Ek açıklama sonucun mevcut olduğunda | 
| durationMs |  Eşlenmemiş budur ve bu alan güvenle yok sayabilirsiniz. |
| callerIpAddress | İsteği gerçekleştiren istemcinin IP adresi | 
| correlationId | İstemci tarafından geçirilen isteğe bağlı bir GUID. Bu, sunucu tarafı işlemleri performanstaki istemci tarafı işlemleri yardımcı olabilir ve izleme hizmetlerinde span günlükleri yararlı olur. |
| identity | İsteği yapılırken sunulan belirteçten kimliği. Bir kullanıcı hesabı, sistem hesabı veya hizmet sorumlusu olabilir. |
| düzey | İleti türü. Denetim günlükleri için her zaman budur *bilgilendirici* |
| location | Veri merkezi konumu |
| properties | Bir denetim günlüğüne ilgili desteklenen özellikleri listeler. Daha fazla bilgi için aşağıdaki tablo. | 


| Özellik adı | Açıklama |
|---------------|-------------|
| AuditEventCategory | Denetim olayı türü. Olabilir *kullanıcı yönetimi*, *Uygulama Yönetimi* vs.|
| Kimlik türü | *Uygulama* veya *kullanıcı* |
| İşlem Türü | Olabilir *Ekle*, *güncelleştirme*, *Sil* veya *diğer* |
| Hedef Kaynak Türü | Üzerinde işlem gerçekleştirilmeden hedef kaynak türü belirtir. Olabilir *uygulama*, *kullanıcı*, *rol*, *İlkesi* | 
| Hedef kaynak adı | Hedef kaynağın adı. Örneğin, bu bir uygulama adı, bir rol adı, kullanıcı asıl adı veya bir hizmet asıl adı olabilir |
| additionalTargets | Belirli işlemleri için ek özellikleri listeler. Örneğin, bir güncelleştirme işlemi için eski değerleri ve yeni değerleri altında listelenen *targetUpdatedProperties* | 

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici'de oturum açma günlükleri şema yorumlama](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)
* [Azure tanılama günlükleri hakkında daha fazla bilgi](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)
* [Sık sorulan sorular ve bilinen sorunlar](reporting-azure-monitor-diagnostics-overview.md#frequently-asked-questions)