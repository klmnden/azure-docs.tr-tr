---
title: 'Azure yedekleme: REST API kullanarak yedekleme işlerini yönetme'
description: Yedeklemeyi yönetme ve Azure REST API kullanarak yedekleme işlerinin geri yükleme
services: backup
author: pvrk
manager: shivamg
keywords: REST API; Azure VM yedeklemesi; Azure VM geri yükleme;
ms.service: backup
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: pullabhk
ms.assetid: b234533e-ac51-4482-9452-d97444f98b38
ms.openlocfilehash: eb8b7dc77d180eb56c2585e93e60a36742f6c84c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60646631"
---
# <a name="track-backup-and-restore-jobs-using-rest-api"></a>REST API kullanarak yedekleme ve geri yükleme işlerini izleme

Azure Backup hizmeti yedekleme tetikleniyor gibi çeşitli senaryolarda arka planda, geri yükleme işlemleri, yedekleme devre dışı bırakma işleri tetikler. Bu işleri, kimlikleri kullanılarak izlenebilir.

## <a name="fetch-job-information-from-operations"></a>İş bilgileri işlemlerinden getirilemedi

Yedeklemeyi tetikleme gibi bir işlemin her zaman bir iş kimliği döndürür. İçin örn: Son yanıtı bir [yedekleme REST API işlemi tetikleyen](backup-azure-arm-userestapi-backupazurevms.md#example-responses-3) aşağıdaki gibidir:

```http
{
  "id": "cd153561-20d3-467a-b911-cc1de47d4763",
  "name": "cd153561-20d3-467a-b911-cc1de47d4763",
  "status": "Succeeded",
  "startTime": "2018-09-12T02:16:56.7399752Z",
  "endTime": "2018-09-12T02:16:56.7399752Z",
  "properties": {
    "objectType": "OperationStatusJobExtendedInfo",
    "jobId": "41f3e94b-ae6b-4a20-b422-65abfcaf03e5"
  }
}
```

Azure VM yedekleme işi "iş kimliği" alanı tarafından tanımlanır ve belirtildiği gibi izlenen [burada](https://docs.microsoft.com/rest/api/backup/jobdetails/) kullanarak basit bir *alma* isteği.

## <a name="tracking-the-job"></a>İş izleme

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupJobs/{jobName}?api-version=2017-07-01
```

`{jobName}` "JobId" yukarıda belirtilen. Yanıt her zaman 200 Tamam işin geçerli durumu gösteren "Durum" ile bir alandır. 'ExtendedInfo' bölümünde "Completed" veya "CompletedWithWarnings" olduğunda, iş hakkında daha fazla ayrıntı ortaya çıkarır.

### <a name="response"></a>Yanıt

|Ad  |Tür  |Açıklama  |
|---------|---------|---------|
|200 TAMAM     | [JobResource](https://docs.microsoft.com/rest/api/backup/jobdetails/get#jobresource)        | Tamam        |

#### <a name="example-response"></a>Örnek yanıt

Bir kez *alma* URI gönderildi, bir 200 (Tamam) yanıt döndürülür.

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: e9702101-9da2-4681-bdf3-a54e17329a56
x-ms-client-request-id: ba4dff71-1655-4c1d-a71f-c9869371b18b; ba4dff71-1655-4c1d-a71f-c9869371b18b
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14989
x-ms-correlation-request-id: e9702101-9da2-4681-bdf3-a54e17329a56
x-ms-routing-request-id: SOUTHINDIA:20180521T102317Z:e9702101-9da2-4681-bdf3-a54e17329a56
Cache-Control: no-cache
Date: Mon, 21 May 2018 10:23:17 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-RecoveryServices-ResourceGroup-centralindia/providers/microsoft.recoveryservices/vaults/abdemovault/backupJobs/7ddead57-bcb9-4269-ac31-6a1b57588700",
  "name": "7ddead57-bcb9-4269-ac31-6a1b57588700",
  "type": "Microsoft.RecoveryServices/vaults/backupJobs",
  "properties": {
    "jobType": "AzureIaaSVMJob",
    "duration": "00:20:23.0896697",
    "actionsInfo": [
      1
    ],
    "virtualMachineVersion": "Compute",
    "extendedInfo": {
      "tasksList": [
        {
          "taskId": "Take Snapshot",
          "duration": "00:00:00",
          "status": "Completed"
        },
        {
          "taskId": "Transfer data to vault",
          "duration": "00:00:00",
          "status": "Completed"
        }
      ],
      "propertyBag": {
        "VM Name": "uttestvmub1",
        "Backup Size": "2332 MB"
      }
    },
    "entityFriendlyName": "uttestvmub1",
    "backupManagementType": "AzureIaasVM",
    "operation": "Backup",
    "status": "Completed",
    "startTime": "2018-05-21T08:35:40.9488967Z",
    "endTime": "2018-05-21T08:56:04.0385664Z",
    "activityId": "7df8e874-1d66-4f81-8e91-da2fe054811d"
  }
}
}

```