---
title: Güvenlik Merkezi ilke uyumluluğu Azure REST API ile gözden geçirme | Microsoft Docs
description: Güvenlik Merkezi ilkelerle geçerli uyumluluk gözden geçirmek için Azure REST API'lerini kullanmayı öğrenin.
services: security-center
documentationcenter: na
author: lleonard-msft
manager: MBaldwin
editor: ''
ms.assetid: 82D50B98-40F2-44B1-A445-4391EA9EBBAA
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2017
ms.author: alleonar
ms.openlocfilehash: 6c6764eec59633f0bdd0fa396c1581117a0c1e1d
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="review-security-center-policy-compliance-using-rest-apis"></a>REST API'lerini kullanarak gözden geçirme Güvenlik Merkezi ilke uyumluluğu

Güvenlik Merkezi düzenli aralıklarla Azure kaynaklarınızın tanımlı güvenlik ilkelerinizi karşı doğrular. Güvenlik Merkezi, ayrıca, uyumluluk kendi uygulamalardan gözden geçirmenize olanak sağlayan bir REST API'si sağlar; Hizmet doğrudan sorgu veya diğer uygulamalara JSON sonuçları alın. 

Burada, bir aboneliği ile ilişkili tüm Azure kaynakları önerileri geçerli kümesini almak öğrenin.

Geçerli dizi öneriyi almak için:
``` http
GET https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Security/tasks?api-version={api-version}
Content-Type: application/json   
Authorization: Bearer
```

## <a name="build-the-request"></a>Oluşturma isteği  

`{subscription-id}` Parametresi gereklidir ve politikalarını Azure aboneliği için abonelik Kimliğini içermelidir. Birden çok aboneliğiniz varsa, bkz: [birden çok abonelik ile çalışma](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#working-with-multiple-subscriptions).  

`api-version` Parametresi gereklidir. Şu anda bu uç noktalar yalnızca desteklenen `api-version=2015-06-01-preview`. 

Aşağıdaki üst bilgiler gereklidir: 

|İstek üstbilgisi|Açıklama|  
|--------------------|-----------------|  
|*Content-Type:*|Gereklidir. Kümesine `application/json`.|  
|*Yetkilendirme:*|Gereklidir. Geçerli bir ayarla `Bearer` [erişim belirteci](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |  

## <a name="response"></a>Yanıt  

Durum kodu 200 (Tamam) Azure kaynaklarınızı güvenli hale getirmek için önerilen görevlerin bir listesi içeren başarılı bir yanıt için döndürülür.

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

Her öğe **değeri** bir öneri temsil eder:

|Yanıt özelliği|Açıklama|
|----------------|----------|
|**Durumu** | Öneri olup olmadığını belirten `active` veya `resolved`. |
|**creationTimeUtc** | Tarih ve öneri ne zaman oluşturulduğunu gösteren UTC saat. |
|**lastStateChangeUtc** | Tarih ve saat, son durum değişikliği, varsa, UTV. |
|**securityTaskParameters** | Öneri ayrıntılarını; Özellikler, temel alınan öneri göre farklılık gösterir. |
||
  
Şu anda desteklenen önerileri için bkz: [uygulayan güvenlik önerileri](https://docs.microsoft.com/azure/security-center/security-center-recommendations).

Diğer durum kodları hata koşulları belirtin. Bu durumlarda, isteğin neden başarısız açıklayan bir açıklama yanıt nesnesini içerir.

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

Bu yanıt iki öneriler gösterir; Listedeki her bir öğe için belirli bir öneri karşılık gelir. Linux sanal makine depolama şifreleme ilk önerir ve ikinci bir SQL server için denetimi etkinleştirmek önerir.

Öneriler, etkinleştirdikten ilkelerine göre değişir. Şu anda kullanılabilir öneriler dahil olmak üzere daha fazla bilgi için bkz: [Azure Güvenlik Merkezi'nde güvenlik önerilerini yönetme](https://docs.microsoft.com/azure/security-center/security-center-recommendations).


## <a name="see-also"></a>Ayrıca bkz.  
- [Güvenlik ilkeleri ayarlama](https://docs.microsoft.com/azure/security-center/security-center-policies-overview)
- [Azure güvenlik kaynak sağlayıcısı REST API'si](https://msdn.microsoft.com/library/azure/mt704034.aspx)   
- [Azure REST API'si ile çalışmaya başlama](https://docs.microsoft.com/rest/api/azure/)   
- [Azure Güvenlik Merkezi PowerShell Modülü](https://www.powershellgallery.com/packages/Azure-Security-Center/0.0.22)
