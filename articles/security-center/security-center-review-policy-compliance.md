---
title: Azure REST API'si ile Güvenlik Merkezi ilke uyumluluğunu gözden geçirme | Microsoft Docs
description: Güvenlik Merkezi ilkelerle geçerli uyumluluğu gözden geçirmek için Azure REST API'lerini kullanmayı öğrenin.
services: security-center
documentationcenter: na
author: lleonard-msft
manager: MBaldwin
editor: ''
ms.assetid: 82D50B98-40F2-44B1-A445-4391EA9EBBAA
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: alleonar
ms.openlocfilehash: 1443486590859aac5591aff2ab0551bed9228d7b
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44301758"
---
# <a name="review-security-center-policy-compliance-using-rest-apis"></a>REST API'lerini kullanarak gözden geçirme Güvenlik Merkezi ilke uyumluluğu

Güvenlik Merkezi düzenli aralıklarla Azure kaynaklarınızın tanımlı güvenlik ilkelerinizi karşı doğrular. Güvenlik Merkezi ayrıca kendi uygulamalarınızı uyumluluğunu gözden geçirme olanak sağlayan bir REST API'si sağlar. Hizmet doğrudan sorgulamak veya JSON sonuçları diğer uygulamalara içeri aktarın. 

Burada, bir aboneliği ile ilişkili tüm Azure kaynaklarını setini öneriler almak öğrenin.

Geçerli kümesini öneriler almak için:
``` http
GET https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Security/tasks?api-version={api-version}
Content-Type: application/json   
Authorization: Bearer
```

## <a name="build-the-request"></a>Derleme isteği  

`{subscription-id}` Parametresi gereklidir ve ilkeler tanımlayarak Azure aboneliği için abonelik Kimliğini içermelidir. Birden fazla aboneliğiniz varsa, bkz. [birden çok abonelik ile çalışma](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#working-with-multiple-subscriptions).  

`api-version` Parametresi gereklidir. Şu anda bu uç noktalar için yalnızca desteklenen `api-version=2015-06-01-preview`. 

Aşağıdaki üst bilgiler gereklidir: 

|İstek üstbilgisi|Açıklama|  
|--------------------|-----------------|  
|*İçerik türü:*|Gereklidir. Kümesine `application/json`.|  
|*Yetkilendirme:*|Gereklidir. Geçerli bir kümesi `Bearer` [erişim belirteci](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |  

## <a name="response"></a>Yanıt  

Durum kodu 200 (Tamam) kullanarak Azure kaynaklarınızı güvenli hale getirmek için önerilen görevlerinin listesini içeren başarılı bir yanıt için döndürülür.

``` json
{  
  "value": [  
    {  
       "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Security/locations/{region}/tasks/{task-id}",
       "name": "{task_id}",
       "type": "Microsoft.Security/locations/{region}/tasks",
       "properties": {
       "state": "Active",
       "subState": "NA",
       "creationTimeUtc": "{create-time}",
       "lastStateChangeTimeUtc": "{last-state-change}",
       "securityTaskParameters": "{security-task-properties}"
    } 
  ]  
}  
```  

Her öğe **değer** bir öneriyi temsil eder:

|Yanıt özelliği|Açıklama|
|----------------|----------|
|**durumu** | Öneri olup olmadığını belirten `active` veya `resolved`. |
|**CreationTimeUtc** | Tarih ve saat, öneri ne zaman oluşturulduğunu gösteren, UTC diliminde saat. |
|**lastStateChangeUtc** | Tarih ve saat, son durum değişikliği, varsa, UTV. |
|**securityTaskParameters** | Öneri ayrıntıları; özellikleri, temel alınan öneri göre farklılık gösterir. |
||
  
Şu anda desteklenen önerileri için bkz: [güvenlik önerilerini uygulama](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

Diğer durum kodları hata koşulları belirtin. Bu gibi durumlarda yanıt nesnesini isteğin neden başarısız olduğunu açıklayan bir açıklama içerir.

``` json
{  
  "value": [  
    {  
      "description": "Error response describing why the operation failed."  
    }  
  ]  
}  
```  

## <a name="example-response"></a>Örnek yanıt  

``` json
{  
  "value": [  
        {
            "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Security/locations/{region}/tasks/{task-id}",
            "name": "{task_id}",
            "type": "Microsoft.Security/locations/{region}/tasks",
            "properties": {
                "state": "Active",
                "subState": "NA",
                "creationTimeUtc": "{create-time}",
                "lastStateChangeTimeUtc": "{last-state-change}",
                "securityTaskParameters": {
                    "vmId": "/subscriptions/{subscription-id}/resourceGroups/{resource_group}/providers/Microsoft.Compute/virtualMachines/{vm_name}",
                    "vmName": "{vm_name}",
                    "severity": "{severity}",
                    "isOsDiskEncrypted": {is_os_disk_encrypted},
                    "isDataDiskEncrypted": {is_data_disk_encrypted},
                    "name": "EncryptionOnVm",
                    "uniqueKey": "EncryptionOnVmTaskParameters_/subscriptions/{subscription-id}/resourceGroups/{resoource_group}/providers/Microsoft.Compute/virtualMachines/{vm_name}",
                    "resourceId": "/subscriptions/{subscription_id}/resourceGroups/{resource_group}/providers/Microsoft.Compute/virtualMachines/{vm_name}"
                }
            }
        },
        {
            "id": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Security/locations/{location}/tasks/{task-id}",
            "name": "{task-id}",
            "type": "Microsoft.Security/locations/{region}/tasks",
            "properties": {
                "state": "Active",
                "subState": "NA",
                "creationTimeUtc": "{create-time}",
                "lastStateChangeTimeUtc": "{last-state-change}",
                "securityTaskParameters": {
                    "serverName": "{sql-server-name}",
                    "serverId": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Sql/servers/{server-id}",
                    "name": "Enable auditing for the SQL server",
                    "uniqueKey": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Sql/servers/{server-id}/auditingPolicies/Default",
                    "resourceId": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Sql/servers/{server-id}"
                }
            }
        }  ]  
}  
```  

Bu yanıt iki öneriler gösterir. Listedeki her öğe için belirli bir öneri karşılık gelir. İlk Linux sanal makinesinde depolama şifreleme önerir ve ikinci bir SQL server için denetimi etkinleştirme önerir.

Öneriler, etkinleştirdiğiniz ilkelerine göre farklılık gösterir. Şu anda kullanılabilir öneriler de dahil olmak üzere daha fazla bilgi için bkz. [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](https://docs.microsoft.com/azure/security-center/security-center-recommendations).


## <a name="see-also"></a>Ayrıca bkz.  
- [Güvenlik ilkeleri ayarlama](https://docs.microsoft.com/azure/security-center/security-center-policies-overview)
- [Azure güvenlik kaynak sağlayıcısı REST API'si](https://msdn.microsoft.com/library/azure/mt704034.aspx)   
- [Azure REST API'si ile çalışmaya başlama](https://docs.microsoft.com/rest/api/azure/)   
- [Azure Güvenlik Merkezi PowerShell Modülü](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.22)
