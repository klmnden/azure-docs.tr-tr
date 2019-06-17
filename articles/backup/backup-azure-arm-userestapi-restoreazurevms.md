---
title: "Azure yedekleme: REST API kullanarak Azure Vm'leri geri yükleme"
description: REST API kullanarak Azure VM yedeklemesi geri yükleme işlemlerini yönetme
services: backup
author: pvrk
manager: shivamg
keywords: REST API; Azure VM yedeklemesi; Azure VM geri yükleme;
ms.service: backup
ms.topic: conceptual
ms.date: 09/12/2018
ms.author: pullabhk
ms.assetid: b8487516-7ac5-4435-9680-674d9ecf5642
ms.openlocfilehash: 4a65e8a855b9be797c1ceeacf4b74fea74697d00
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60646665"
---
# <a name="restore-azure-virtual-machines-using-rest-api"></a>REST API kullanarak Azure sanal makineleri geri yükleme

Azure Yedekleme'yi kullanarak bir Azure sanal makine yedeklemesi tamamlandıktan sonra bir tüm Azure sanal makineler veya diskleri veya dosyaları aynı yedek kopyadan geri yükleyebilirsiniz. Bu makalede bir Azure VM veya REST API'yi kullanarak diskleri geri yükleme.

İçin geri yükleme işlemi, ilgili kurtarma noktasını belirlemek öncelikle varsa.

## <a name="select-recovery-point"></a>Kurtarma noktası seçin

Bir yedekleme öğesinin kullanılabilir kurtarma noktalarını kullanarak listelenebilir [kurtarma noktası REST API listesinde](https://docs.microsoft.com/rest/api/backup/recoverypoints/list). Basit bir *alma* işlemi ile ilgili tüm değerleri.

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints?api-version=2016-12-01
```

`{containerName}` Ve `{protectedItemName}` oluşturulmuş gibi [burada](backup-azure-arm-userestapi-backupazurevms.md#example-responses-1). `{fabricName}` "Azure" dir.

*Alma* URI'ya sahip tüm gerekli parametreleri. Ek istek gövdesi için gerek yoktur

### <a name="responses"></a>Responses

|Ad  |Tür  |Açıklama  |
|---------|---------|---------|
|200 TAMAM     |   [RecoveryPointResourceList](https://docs.microsoft.com/rest/api/backup/recoverypoints/list#recoverypointresourcelist)      |       Tamam  |

#### <a name="example-response"></a>Örnek yanıt

Bir kez *alma* URI gönderildi, bir 200 (Tamam) yanıt döndürülür.

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: 03453538-2f8d-46de-8374-143ccdf60f40
x-ms-client-request-id: c48f4436-ce3f-42da-b537-12710d4d1a24; c48f4436-ce3f-42da-b537-12710d4d1a24
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14998
x-ms-correlation-request-id: 03453538-2f8d-46de-8374-143ccdf60f40
x-ms-routing-request-id: SOUTHINDIA:20180604T071851Z:03453538-2f8d-46de-8374-143ccdf60f40
Cache-Control: no-cache
Date: Mon, 04 Jun 2018 07:18:51 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/VM;testRG;testVM/recoveryPoints/20982486783671",
      "name": "20982486783671",
      "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints",
      "properties": {
        "objectType": "IaasVMRecoveryPoint",
        "recoveryPointType": "AppConsistent",
        "recoveryPointTime": "2018-06-04T06:06:00.5121087Z",
        "recoveryPointAdditionalInfo": "",
        "sourceVMStorageType": "NormalStorage",
        "isSourceVMEncrypted": false,
        "isInstantIlrSessionActive": false,
        "recoveryPointTierDetails": [
          {
            "type": 1,
            "status": 1
          },
          {
            "type": 2,
            "status": 1
          }
        ],
        "isManagedVirtualMachine": true,
        "virtualMachineSize": "Standard_A1_v2",
        "originalStorageAccountOption": false
      }
    },
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/VM;testRG;testVM/recoveryPoints/23358112038108",
      "name": "23358112038108",
      "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints",
      "properties": {
        "objectType": "IaasVMRecoveryPoint",
        "recoveryPointType": "CrashConsistent",
        "recoveryPointTime": "2018-06-03T06:20:56.3043878Z",
        "recoveryPointAdditionalInfo": "",
        "sourceVMStorageType": "NormalStorage",
        "isSourceVMEncrypted": false,
        "isInstantIlrSessionActive": false,
        "recoveryPointTierDetails": [
          {
            "type": 1,
            "status": 1
          },
          {
            "type": 2,
            "status": 1
          }
        ],
        "isManagedVirtualMachine": true,
        "virtualMachineSize": "Standard_A1_v2",
        "originalStorageAccountOption": false
      }
    },
......
```

Kurtarma noktası ile tanımlanan `{name}` yukarıdaki yanıt kodu.

## <a name="restore-disks"></a>Diskleri geri yükle

Yedekleme verilerini bir VM'den oluşturulmasını özelleştirmek için bir gereksinimi varsa, bir yalnızca bir seçilen depolama hesabına disk geri yükleme ve gereksinimlerine göre bu diskleri VM oluşturma. Depolama hesabı kurtarma Hizmetleri kasasıyla aynı bölgede olmalıdır ve bölgesel olarak yedekli olmamalıdır. Yedeklenen sanal makine ("vmconfig.json") yapılandırmasını yanı sıra diskleri belirli bir depolama hesabında depolanır.

Diskleri geri yükleme tetikleniyor olduğu bir *POST* isteği. Disk geri yükleme işlemini hakkında daha fazla bilgi edinmek için bkz ["geri yüklemeyi tetikleyecek" REST API](https://docs.microsoft.com/rest/api/backup/restores/trigger).

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/recoveryPoints/{recoveryPointId}/restore?api-version=2016-12-01
```

`{containerName}` Ve `{protectedItemName}` oluşturulmuş gibi [burada](backup-azure-arm-userestapi-backupazurevms.md#example-responses-1). `{fabricName}` "Azure" olduğundan ve `{recoveryPointId}` olduğu `{name}` belirtilen kurtarma noktası alanı [yukarıda](#example-response).

### <a name="create-request-body"></a>İstek gövdesi oluşturma

Aşağıdaki disk geri yükleme yoluyla bir Azure VM yedeklemesi tetiklemek için istek gövdesi bileşenleridir.

|Ad  |Tür  |Açıklama  |
|---------|---------|---------|
|properties     | [IaaSVMRestoreRequest](https://docs.microsoft.com/rest/api/backup/restores/trigger#iaasvmrestorerequest)        |    RestoreRequestResourceProperties     |

İstek gövdesini ve diğer ayrıntıları tanımlarını tam listesi için başvurmak [tetikleme geri REST API belge](https://docs.microsoft.com/rest/api/backup/restores/trigger#request-body).

#### <a name="example-request"></a>Örnek istek

Aşağıdaki istek gövdesi bir diski geri yüklemeyi tetikleyecek gerekli özelliklerini tanımlar.

```json
{
  "properties": {
    "objectType": "IaasVMRestoreRequest",
    "recoveryPointId": "20982486783671",
    "recoveryType": "RestoreDisks",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "storageAccountId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Storage/storageAccounts/testAccount",
    "region": "westus",
    "createNewCloudService": false,
    "originalStorageAccountOption": false,
    "encryptionDetails": {
      "encryptionEnabled": false
    }
  }
}
```

### <a name="response"></a>Yanıt

Bir diski geri yükleme tetikleniyor olduğu bir [zaman uyumsuz işlem](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Bu işlem, ayrı ayrı izlenmesi gereken başka bir işlem oluşturur anlamına gelir.

İki yanıt döndürür: 202 (kabul edildi başka bir işlem oluşturulurken) ve ardından 200 (Tamam) Bu işlem tamamlandığında.

|Ad  |Tür  |Açıklama  |
|---------|---------|---------|
|202 kabul edildi     |         |     Kabul edildi    |

#### <a name="example-responses"></a>Örnek yanıt

Gönderdiğiniz sonra *POST* tetiklemek URI diskleri geri yükleme, bir konum üst bilgisi veya Azure async üstbilgi 202 (kabul edildi) ilk yanıt.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/781a0f18-e250-4d73-b059-5e9ffed4069e?api-version=2016-12-01
X-Content-Type-Options: nosniff
x-ms-request-id: 893fe372-8d6c-4c56-b589-45a95eeef95f
x-ms-client-request-id: a15ce064-25bd-4ac6-87e5-e3bc6ec65c0b; a15ce064-25bd-4ac6-87e5-e3bc6ec65c0b
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1198
x-ms-correlation-request-id: 893fe372-8d6c-4c56-b589-45a95eeef95f
x-ms-routing-request-id: SOUTHINDIA:20180604T130003Z:893fe372-8d6c-4c56-b589-45a95eeef95f
Cache-Control: no-cache
Date: Mon, 04 Jun 2018 13:00:03 GMT
Location: https://management.azure.com/subscriptions//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationResults/781a0f18-e250-4d73-b059-5e9ffed4069e?api-version=2016-12-01
X-Powered-By: ASP.NET
```

Ardından ile basit bir konum üst bilgisi veya Azure-AsyncOperation başlığı kullanılarak elde edilen işlemi izlemek *alma* komutu.

```http
GET https://management.azure.com/subscriptions//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationResults/781a0f18-e250-4d73-b059-5e9ffed4069e?api-version=2016-12-01
```

İşlem tamamlandığında, yanıt gövdesi içinde elde edilen geri yükleme işi kimliği 200 (Tamam) döndürür.

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: ea2a8011-eb83-4a4b-9ed2-e694070a966a
x-ms-client-request-id: a7f3a144-ed80-41ee-bffe-ae6a90c35a2f; a7f3a144-ed80-41ee-bffe-ae6a90c35a2f
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14973
x-ms-correlation-request-id: ea2a8011-eb83-4a4b-9ed2-e694070a966a
x-ms-routing-request-id: SOUTHINDIA:20180604T130917Z:ea2a8011-eb83-4a4b-9ed2-e694070a966a
Cache-Control: no-cache
Date: Mon, 04 Jun 2018 13:09:17 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "id": "781a0f18-e250-4d73-b059-5e9ffed4069e",
  "name": "781a0f18-e250-4d73-b059-5e9ffed4069e",
  "status": "Succeeded",
  "startTime": "2018-06-04T13:00:03.8068176Z",
  "endTime": "2018-06-04T13:00:03.8068176Z",
  "properties": {
    "objectType": "OperationStatusJobExtendedInfo",
    "jobId": "3021262a-fb3a-4538-9b37-e3e97a386093"
  }
}
```

Yedekleme işi uzun süren bir işlem olduğundan, onu açıklandığı şekilde izlenmesi gereken [belge REST API kullanarak işleri izleme](backup-azure-arm-userestapi-managejobs.md#tracking-the-job).

Uzun süre çalışan iş tamamlandığında, diskleri ve yapılandırmayı yedeklenen sanal makinenin ("VMConfig.json") belirli bir depolama hesabında mevcut olacaktır.

## <a name="restore-as-another-virtual-machine"></a>Başka bir sanal makineyi geri yükleme

[Kurtarma noktasını seçin](#select-recovery-point) ve istek gövdesi verilerden bir kurtarma noktası ile başka bir Azure sanal makinesi oluşturmak için aşağıdaki belirtildiği gibi oluşturun.

Aşağıdaki istek gövdesi, bir sanal makine geri yüklemeyi tetikleyecek gerekli özelliklerini tanımlar.

```json
{
  "parameters": {
        "subscriptionId": "00000000-0000-0000-0000-000000000000",
        "resourceGroupName": "testVaultRG",
        "vaultName": "testVault",
        "fabricName": "Azure",
        "containerName": "IaasVMContainer;iaasvmcontainerv2;testRG;testVM",
        "protectedItemName": "VM;iaasvmcontainerv2;testRG;testVM",
        "recoveryPointId": "348916168024334",
        "api-version": "2016-12-01",
      "parameters": {
        "properties": {
          "objectType":  "IaasVMRestoreRequest",
          "recoveryPointId": "348916168024334",
          "recoveryType": "AlternateLocation",
          "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
          "targetVirtualMachineId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG/providers/Microsoft.Compute/virtualmachines/targetVM",
          "targetResourceGroupId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG",
          "storageAccountId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Storage/storageAccounts/testingAccount",
          "virtualNetworkId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG/providers/Microsoft.Network/virtualNetworks/testNet",
          "subnetId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/targetRG/providers/Microsoft.Network/virtualNetworks/testNet/subnets/default",
          "region": "westus",
          "createNewCloudService": false,
          "originalStorageAccountOption": false,
          "encryptionDetails": {
            "encryptionEnabled": false
          }
        }
      }
    }
}
```

Yanıt aynı şekilde ele alınması [diskleri geri yüklemek için yukarıda açıklanan](#response).

## <a name="next-steps"></a>Sonraki adımlar

Azure Backup REST API'leri hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

- [Azure kurtarma Hizmetleri Sağlayıcısı REST API'si](/rest/api/recoveryservices/)
- [Azure REST API'si ile çalışmaya başlama](/rest/api/azure/)