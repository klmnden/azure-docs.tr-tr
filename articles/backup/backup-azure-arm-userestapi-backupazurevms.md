---
title: "Azure yedekleme: REST API kullanarak Azure Vm'lerini yedekleme"
description: Azure sanal makine REST API kullanarak yedekleme, yedekleme işlemlerini yönetme
services: backup
author: pvrk
manager: shivamg
keywords: REST API; Azure VM yedeklemesi; Azure VM geri yükleme;
ms.service: backup
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: pullabhk
ms.assetid: b80b3a41-87bf-49ca-8ef2-68e43c04c1a3
ms.openlocfilehash: 295c4fed9ab674f0c9e812c02f6b82ee53ef1b91
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274860"
---
# <a name="back-up-an-azure-vm-using-azure-backup-via-rest-api"></a>REST API aracılığıyla Azure Backup'ı kullanarak Azure VM yedekleme

Bu makalede REST API aracılığıyla Azure Backup'ı kullanarak bir Azure VM yedeklemelerini yönetme açıklar. Daha önce korumasız bir Azure sanal makine için ilk kez korumayı yapılandırma, korumalı bir Azure VM için bir isteğe bağlı yedekleme tetikleyin ve burada açıklandığı gibi yedeklenen sanal makine REST API aracılığıyla yedekleme özelliklerini değiştirin.

Başvurmak [kasası oluşturma](backup-azure-arm-userestapi-createorupdatevault.md) ve [ilkesi oluşturma](backup-azure-arm-userestapi-createorupdatepolicy.md) yeni kasalar ve ilkeleri oluşturmak için REST API öğreticiler.

Bir VM'yi kurtarma Hizmetleri kasasına "testVault", "testVaultRG" kaynak grubunda mevcut bir kaynak grubu "testRG" altında "testVM" ("DefaultPolicy" olarak adlandırılır) varsayılan ilkeyle korumak istediğiniz varsayalım.

## <a name="configure-backup-for-an-unprotected-azure-vm-using-rest-api"></a>REST API kullanarak korumasız Azure VM için yedeklemeyi yapılandırma

### <a name="discover-unprotected-azure-vms"></a>Korumasız Azure Vm'leri keşfedin

İlk olarak, kasa Azure VM belirlemek mümkün olması gerekir. Bunu kullanarak tetiklenir [yenileme işlemi](https://docs.microsoft.com/rest/api/backup/protectioncontainers/refresh). Zaman uyumsuz olduğu *POST* kasa xenapp'i işlemi geçerli abonelikte tüm korumasız bir VM'yi en son listesini alır ve 'bunları önbelleğe'. VM önbelleğe' sonra' Kurtarma Hizmetleri sanal Makineye erişmek ve korumak mümkün olacaktır.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupname}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/refreshContainers?api-version=2016-12-01
```

POST URI'ya sahip `{subscriptionId}`, `{vaultName}`, `{vaultresourceGroupName}`, `{fabricName}` parametreleri. `{fabricName}` "Azure". Bizim örneğimizde göre `{vaultName}` "testVault" olduğundan ve `{vaultresourceGroupName}` "testVaultRG" olduğu. URI'de verilen tüm gerekli parametreleri gibi ayrı bir istek gövdesi için gerek yoktur.

```http
POST https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/refreshContainers?api-version=2016-12-01
```

#### <a name="responses"></a>Responses

'Yenile' işlem bir [zaman uyumsuz işlem](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Bu işlem, ayrı ayrı izlenmesi gereken başka bir işlem oluşturur anlamına gelir.

İki yanıt döndürür: 202 (kabul edildi başka bir işlem oluşturulurken) ve ardından 200 (Tamam) Bu işlem tamamlandığında.

|Ad  |Tür  |Açıklama  |
|---------|---------|---------|
|204 No Content     |         |  Tamam düğmesiyle döndürülen içerik yok      |
|202 kabul edildi     |         |     Kabul edildi    |

##### <a name="example-responses"></a>Örnek yanıt

Bir kez *POST* istek gönderildi, 202 (kabul edildi) yanıt döndürülür.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
X-Content-Type-Options: nosniff
x-ms-request-id: 43cf550d-e463-421c-8922-37e4766db27d
x-ms-client-request-id: 4910609f-bb9b-4c23-8527-eb6fa2d3253f; 4910609f-bb9b-4c23-8527-eb6fa2d3253f
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 43cf550d-e463-421c-8922-37e4766db27d
x-ms-routing-request-id: SOUTHINDIA:20180521T105701Z:43cf550d-e463-421c-8922-37e4766db27d
Cache-Control: no-cache
Date: Mon, 21 May 2018 10:57:00 GMT
Location: https://management.azure.com/subscriptions//00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/operationResults/aad204aa-a5cf-4be2-a7db-a224819e5890?api-version=2016-12-01
X-Powered-By: ASP.NET
```

İle basit bir "Konum" üst bilgisi kullanılarak elde edilen işlem izlemek *alma* komutu

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/operationResults/aad204aa-a5cf-4be2-a7db-a224819e5890?api-version=2016-12-01
```

Tüm Azure VM'ler bulunduktan sonra GET komutu 204 (içerik yok) bir yanıt döndürür. Kasa abonelik içindeki herhangi bir VM bulabilir.

```http
HTTP/1.1 204 NoContent
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: cf6cd73b-9189-4942-a61d-878fcf76b1c1
x-ms-client-request-id: 25bb6345-f9fc-4406-be1a-dc6db0eefafe; 25bb6345-f9fc-4406-be1a-dc6db0eefafe
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14997
x-ms-correlation-request-id: cf6cd73b-9189-4942-a61d-878fcf76b1c1
x-ms-routing-request-id: SOUTHINDIA:20180521T105825Z:cf6cd73b-9189-4942-a61d-878fcf76b1c1
Cache-Control: no-cache
Date: Mon, 21 May 2018 10:58:25 GMT
X-Powered-By: ASP.NET
```

### <a name="selecting-the-relevant-azure-vm"></a>İlgili Azure VM'yi seçme

 "Önbelleğe alma" yapıldığını doğrulayabilirsiniz [tüm korunabilir öğelerin listesi](https://docs.microsoft.com/rest/api/backup/backupprotectableitems/list) aboneliği altında istediğiniz VM'yi yanıtta bulun. [Bu işlem yanıtı](#example-responses-1) ayrıca nasıl kurtarma hizmetleri tanımlayan bir VM hakkında bilgi sağlar.  Desen ile ilgili bilgi sahibi olduktan sonra bu adımı atlayabilir ve doğrudan devam [korunmasını](#enabling-protection-for-the-azure-vm).

Bu işlem bir *alma* işlemi.

```http
GET https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupProtectableItems?api-version=2016-12-01&$filter=backupManagementType eq 'AzureIaasVM'
```

*Alma* URI'ya sahip tüm gerekli parametreleri. Hiçbir ek istek gövdesi gerekli değildir.

##### <a name="responses-1"></a>Yanıtları

|Ad  |Tür  |Açıklama  |
|---------|---------|---------|
|200 TAMAM     | [WorkloadProtectableItemResourceList](https://docs.microsoft.com/rest/api/backup/backupprotectableitems/list#workloadprotectableitemresourcelist)        |       Tamam |

##### <a name="example-responses-1"></a>Örnek yanıt

Bir kez *alma* istek gönderildi, bir 200 (Tamam) yanıt döndürülür.

```http
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: 7c2cf56a-e6be-4345-96df-c27ed849fe36
x-ms-client-request-id: 40c8601a-c217-4c68-87da-01db8dac93dd; 40c8601a-c217-4c68-87da-01db8dac93dd
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14979
x-ms-correlation-request-id: 7c2cf56a-e6be-4345-96df-c27ed849fe36
x-ms-routing-request-id: SOUTHINDIA:20180521T071408Z:7c2cf56a-e6be-4345-96df-c27ed849fe36
Cache-Control: no-cache
Date: Mon, 21 May 2018 07:14:08 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainerv2;testRG;testVM/protectableItems/vm;iaasvmcontainerv2;testRG;testVM",
      "name": "iaasvmcontainerv2;testRG;testVM",
      "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectableItems",
      "properties": {
        "virtualMachineId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
        "virtualMachineVersion": "Compute",
        "resourceGroup": "testRG",
        "backupManagementType": "AzureIaasVM",
        "protectableItemType": "Microsoft.Compute/virtualMachines",
        "friendlyName": "testVM",
        "protectionState": "NotProtected"
      }
    },……………..

```

> [!TIP]
> Değerleri sayısı bir *alma* için 200 'page' yanıt sınırlıdır. URL yanıtlar sonraki kümesini almak için 'nextLink' alanını kullanın.

Yanıt ve her korumasız Azure Vm'lerinin listesini içeren `{value}` yedeklemeyi yapılandırmak için Azure kurtarma hizmeti tarafından gerekli tüm bilgileri içerir. Yedeklemeyi yapılandırmak için Not `{name}` alan ve `{virtualMachineId}` alanındaki `{properties}` bölümü. Aşağıda belirtildiği gibi bu alan değerlerinin iki değişken oluşturun.

- containerName = "iaasvmcontainer;" +`{name}`
- protectedItemName = "vm;" + `{name}`
- `{virtualMachineId}` Daha sonra kullanılan [istek gövdesi](#example-request-body)

Örnekte, yukarıdaki değerler için çevir:

- containerName = "iaasvmcontainer;iaasvmcontainerv2;testRG;testVM"
- protectedItemName = "vm;iaasvmcontainerv2;testRG;testVM"

### <a name="enabling-protection-for-the-azure-vm"></a>Azure VM için korumayı etkinleştirme

İlgili VM "önbelleğe alınmış" ve "tanımlanan" sonra korumak için ilkeyi seçin. Kasadaki mevcut ilkeleri hakkında daha fazla bilgi edinmek için bkz [ilke API listesinde](https://docs.microsoft.com/rest/api/backup/backuppolicies/list). Ardından [uygun ilke](https://docs.microsoft.com/rest/api/backup/protectionpolicies/get) ilke adına başvuran tarafından. İlkeleri oluşturmak için başvurmak [ilke öğretici oluşturma](backup-azure-arm-userestapi-createorupdatepolicy.md). "DefaultPolicy" seçildiğinde, aşağıdaki örnekteki.

Koruma etkinleştirme zaman uyumsuz bir *PUT* 'korumalı öğesi' oluşturan bir işlem.

```http
https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{vaultresourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}?api-version=2016-12-01
```

`{containerName}` Ve `{protectedItemName}` üzerinde oluşturulmuş gibi. `{fabricName}` "Azure". Bizim örneğimizde, bunun için çevirir:

```http
PUT https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;iaasvmcontainerv2;testRG;testVM?api-version=2016-12-01
```

#### <a name="create-the-request-body"></a>İstek gövdesi oluşturma

Korumalı bir öğe oluşturmak için istek gövdesi bileşenlerinin aşağıda verilmiştir.

|Ad  |Tür  |Açıklama  |
|---------|---------|---------|
|properties     | AzureIaaSVMProtectedItem        |ProtectedItem kaynak özellikleri         |

İstek gövdesini ve diğer ayrıntıları tanımlarını tam listesi için başvurmak [korumalı öğe REST API belge oluşturma](https://docs.microsoft.com/rest/api/backup/protecteditems/createorupdate#request-body).

##### <a name="example-request-body"></a>Örnek istek gövdesi

Aşağıdaki istek gövdesi, korumalı bir öğe oluşturmak için gereken özellikleri tanımlar.

```json
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/DefaultPolicy"
  }
}
```

`{sourceResourceId}` Olduğu `{virtualMachineId}` gelen yukarıda sözü edilen [korunabilir öğe listesi yanıtı](#example-responses-1).

#### <a name="responses"></a>Responses

Korumalı bir öğe oluşturma bir [zaman uyumsuz işlem](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Bu işlem, ayrı ayrı izlenmesi gereken başka bir işlem oluşturur anlamına gelir.

İki yanıt döndürür: 202 (kabul edildi başka bir işlem oluşturulurken) ve ardından 200 (Tamam) Bu işlem tamamlandığında.

|Ad  |Tür  |Açıklama  |
|---------|---------|---------|
|200 TAMAM     |    [ProtectedItemResource](https://docs.microsoft.com/rest/api/backup/protecteditemoperationresults/get#protecteditemresource)     |  Tamam       |
|202 kabul edildi     |         |     Kabul edildi    |

##### <a name="example-responses"></a>Örnek yanıt

Gönderdiğiniz sonra *PUT* istek korumalı öğe oluşturma veya güncelleştirme, ilk yanıt, 202 (kabul edildi). bir konum üst bilgisi ya da Azure async başlığı.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2016-12-01
X-Content-Type-Options: nosniff
x-ms-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-client-request-id: e1f94eef-9b2d-45c4-85b8-151e12b07d03; e1f94eef-9b2d-45c4-85b8-151e12b07d03
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: db785be0-bb20-4598-bc9f-70c9428b170b
x-ms-routing-request-id: SOUTHINDIA:20180521T073907Z:db785be0-bb20-4598-bc9f-70c9428b170b
Cache-Control: no-cache
Date: Mon, 21 May 2018 07:39:06 GMT
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationResults/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2016-12-01
X-Powered-By: ASP.NET
```

Ardından ile basit bir konum üst bilgisi veya Azure-AsyncOperation başlığı kullanılarak elde edilen işlemi izlemek *alma* komutu.

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2016-12-01
```

İşlem tamamlandığında, yanıt gövdesi korumalı öğenin içeriği ile 200 (Tamam) döndürür.

```json
{
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM",
  "name": "VM;testRG;testVM",
  "type": "Microsoft.RecoveryServices/vaults/backupFabrics/protectionContainers/protectedItems",
  "properties": {
    "friendlyName": "testVM",
    "virtualMachineId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "protectionStatus": "Healthy",
    "protectionState": "IRPending",
    "healthStatus": "Passed",
    "lastBackupStatus": "",
    "lastBackupTime": "2001-01-01T00:00:00Z",
    "protectedItemDataId": "17592691116891",
    "extendedInfo": {
      "recoveryPointCount": 0,
      "policyInconsistent": false
    },
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "backupManagementType": "AzureIaasVM",
    "workloadType": "VM",
    "containerName": "iaasvmcontainerv2;testRG;testVM",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/DefaultPolicy",
    "policyName": "DefaultPolicy"
  }
}
```

Bu, sanal makine için koruma etkinleştirildikten ve ilk yedekleme İlkesi zamanlamaya göre tetiklenen doğrular.

## <a name="trigger-an-on-demand-backup-for-a-protected-azure-vm"></a>Korumalı bir Azure VM için bir isteğe bağlı yedekleme tetikleyin

Bir Azure VM yedeklemesi için yapılandırıldıktan sonra yedekleme İlkesi zamanlamaya göre gerçekleşir. İlk zamanlanmış yedekleme için bekleyin ya da her zaman talep üzerine yedekleme tetikleyin. İsteğe bağlı yedeklemeler için elde tutma yedekleme ilkesinin saklamadan ayrıdır ve belirli bir tarih-saat belirtilebilir. Belirtilmezse, isteğe bağlı yedekleme tetikleyicinin günden itibaren 30 gün olduğu varsayılır.

İsteğe bağlı yedekleme tetikleniyor olduğu bir *POST* işlemi.

```http
POST https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}/backup?api-version=2016-12-01
```

`{containerName}` Ve `{protectedItemName}` oluşturulmuş gibi [yukarıda](#responses-1). `{fabricName}` "Azure". Bizim örneğimizde, bunun için çevirir:

```http
POST https://management.azure.com/Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;iaasvmcontainerv2;testRG;testVM/backup?api-version=2016-12-01
```

### <a name="create-the-request-body"></a>İstek gövdesi oluşturma

Aşağıdaki isteğe bağlı yedekleme tetiklemek için istek gövdesi bileşenleridir.

|Ad  |Tür  |Açıklama  |
|---------|---------|---------|
|properties     | [IaaSVMBackupRequest](https://docs.microsoft.com/rest/api/backup/backups/trigger#iaasvmbackuprequest)        |BackupRequestResource özellikleri         |

İstek gövdesini ve diğer ayrıntıları tanımlarını tam listesi için başvurmak [tetikleme yedeklemeler için korumalı öğeler REST API belge](https://docs.microsoft.com/rest/api/backup/backups/trigger#request-body).

#### <a name="example-request-body"></a>Örnek istek gövdesi

Aşağıdaki istek gövdesi, korumalı bir öğe için bir yedek tetiklemek için gereken özellikleri tanımlar. Bekletme belirtilmezse yedekleme işinin tetikleyicisinin tarihten itibaren 30 gün boyunca saklanır.

```json
{
   "properties": {
    "objectType": "IaasVMBackupRequest",
    "recoveryPointExpiryTimeInUTC": "2018-12-01T02:16:20.3156909Z"
  }
}
```

### <a name="responses"></a>Responses

İsteğe bağlı yedekleme tetikleniyor olduğu bir [zaman uyumsuz işlem](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Bu işlem, ayrı ayrı izlenmesi gereken başka bir işlem oluşturur anlamına gelir.

İki yanıt döndürür: 202 (kabul edildi başka bir işlem oluşturulurken) ve ardından 200 (Tamam) Bu işlem tamamlandığında.

|Ad  |Tür  |Açıklama  |
|---------|---------|---------|
|202 kabul edildi     |         |     Kabul edildi    |

##### <a name="example-responses-3"></a>Örnek yanıt

Gönderdiğiniz sonra *POST* isteği için bir isteğe bağlı yedekleme, ilk yanıt, 202 (kabul edildi). bir konum üst bilgisi ya da Azure async başlığı.

```http
HTTP/1.1 202 Accepted
Pragma: no-cache
Retry-After: 60
Azure-AsyncOperation: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testVaultRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/b8daecaa-f8f5-44ed-9f18-491a9e9ba01f?api-version=2016-12-01
X-Content-Type-Options: nosniff
x-ms-request-id: 7885ca75-c7c6-43fb-a38c-c0cc437d8810
x-ms-client-request-id: 7df8e874-1d66-4f81-8e91-da2fe054811d; 7df8e874-1d66-4f81-8e91-da2fe054811d
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-writes: 1199
x-ms-correlation-request-id: 7885ca75-c7c6-43fb-a38c-c0cc437d8810
x-ms-routing-request-id: SOUTHINDIA:20180521T083541Z:7885ca75-c7c6-43fb-a38c-c0cc437d8810
Cache-Control: no-cache
Date: Mon, 21 May 2018 08:35:41 GMT
Location: https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testVaultRG;testVM/protectedItems/vm;testRG;testVM/operationResults/b8daecaa-f8f5-44ed-9f18-491a9e9ba01f?api-version=2016-12-01
X-Powered-By: ASP.NET
```

Ardından ile basit bir konum üst bilgisi veya Azure-AsyncOperation başlığı kullanılarak elde edilen işlemi izlemek *alma* komutu.

```http
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;testRG;testVM/operationsStatus/a0866047-6fc7-4ac3-ba38-fb0ae8aa550f?api-version=2016-12-01
```

İşlem tamamlandığında, yanıt gövdesi içinde elde edilen yedekleme işi kimliği 200 (Tamam) döndürür.

```json
HTTP/1.1 200 OK
Pragma: no-cache
X-Content-Type-Options: nosniff
x-ms-request-id: a8b13524-2c95-445f-b107-920806f385c1
x-ms-client-request-id: 5a63209d-3708-4e69-a75f-9499f4c8977c; 5a63209d-3708-4e69-a75f-9499f4c8977c
Strict-Transport-Security: max-age=31536000; includeSubDomains
x-ms-ratelimit-remaining-subscription-reads: 14995
x-ms-correlation-request-id: a8b13524-2c95-445f-b107-920806f385c1
x-ms-routing-request-id: SOUTHINDIA:20180521T083723Z:a8b13524-2c95-445f-b107-920806f385c1
Cache-Control: no-cache
Date: Mon, 21 May 2018 08:37:22 GMT
Server: Microsoft-IIS/8.0
X-Powered-By: ASP.NET

{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "00000000-0000-0000-0000-000000000000",
  "status": "Succeeded",
  "startTime": "2018-05-21T08:35:40.9488967Z",
  "endTime": "2018-05-21T08:35:40.9488967Z",
  "properties": {
    "objectType": "OperationStatusJobExtendedInfo",
    "jobId": "7ddead57-bcb9-4269-ac31-6a1b57588700"
  }
}
```

Yedekleme işi uzun süren bir işlem olduğundan, onu açıklandığı şekilde izlenmesi gereken [belge REST API kullanarak işleri izleme](backup-azure-arm-userestapi-managejobs.md#tracking-the-job).

## <a name="modify-the-backup-configuration-for-a-protected-azure-vm"></a>Korumalı bir Azure VM yedekleme yapılandırmasını değiştirme

### <a name="changing-the-policy-of-protection"></a>Koruma ilkesini değiştirme

İle VM korumalı ilkeyi değiştirmek için aynı biçimde kullanabilirsiniz [korunmasını](#enabling-protection-for-the-azure-vm). Yalnızca yeni ilke kimliği sağlayın. [istek gövdesi](#example-request-body) ve isteği gönderin. İçin örn: 'ProdPolicy' için 'DefaultPolicy' nden testVM ilkeyi değiştirmek için istek gövdesinde 'ProdPolicy' kimliği sağlayın.

```http
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/microsoft.recoveryservices/vaults/testVault/backupPolicies/ProdPolicy"
  }
}
```

Yanıt belirtildiği gibi aynı biçimde izleyeceği [korumayı etkinleştirmek için](#responses-2)

### <a name="stop-protection-but-retain-existing-data"></a>Korumayı durdurur ancak mevcut verileri tut

Korunan bir sanal makine üzerindeki korumayı kaldırır ancak zaten yedeklenmiş verileri tutmak için ilke istek gövdesinde kaldırın ve isteği gönderin. İlke ilişkilendirmesini kaldırıldıktan sonra yedeklemeler artık tetiklenir ve yeni kurtarma noktası oluşturulur.

```http
{
  "properties": {
    "protectedItemType": "Microsoft.Compute/virtualMachines",
    "sourceResourceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testRG/providers/Microsoft.Compute/virtualMachines/testVM",
    "policyId": ""
  }
}
```

Yanıt belirtildiği gibi aynı biçimde izleyeceği [isteğe bağlı yedekleme tetiklemek](#example-responses-3). Sonuç iş açıklandığı şekilde izlenmesi gereken [belge REST API kullanarak işleri izleme](backup-azure-arm-userestapi-managejobs.md#tracking-the-job).

### <a name="stop-protection-and-delete-data"></a>Korumayı durdurma ve verileri silme

Korunan bir sanal makine üzerindeki korumayı kaldırma ve yedekleme verileri de silmek için bir silme işlemi ayrıntılı olarak gerçekleştirmek [burada](https://docs.microsoft.com/rest/api/backup/protecteditems/delete).

Korumayı durdurma ve verileri silme bir *Sil* işlemi.

```http
DELETE https://management.azure.com/Subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.RecoveryServices/vaults/{vaultName}/backupFabrics/{fabricName}/protectionContainers/{containerName}/protectedItems/{protectedItemName}?api-version=2016-12-01
```

`{containerName}` Ve `{protectedItemName}` oluşturulmuş gibi [yukarıda](#responses-1). `{fabricName}` "Azure" dir. Bizim örneğimizde, bunun için çevirir:

```http
DELETE https://management.azure.com//Subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testVaultRG/providers/Microsoft.RecoveryServices/vaults/testVault/backupFabrics/Azure/protectionContainers/iaasvmcontainer;iaasvmcontainerv2;testRG;testVM/protectedItems/vm;iaasvmcontainerv2;testRG;testVM?api-version=2016-12-01
```

### <a name="responses-2"></a>Yanıtları

*SİLME* koruma bir [zaman uyumsuz işlem](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations). Bu işlem, ayrı ayrı izlenmesi gereken başka bir işlem oluşturur anlamına gelir.

İki yanıt döndürür: 202 (kabul edildi başka bir işlem oluşturulurken) ve ardından 204 (Bu işlem tamamlandığında NoContent).

|Ad  |Tür  |Açıklama  |
|---------|---------|---------|
|204 NoContent     |         |  NoContent       |
|202 kabul edildi     |         |     Kabul edildi    |

## <a name="next-steps"></a>Sonraki adımlar

[Verileri bir Azure sanal makine yedekten geri yükleyin](backup-azure-arm-userestapi-restoreazurevms.md).

Azure Backup REST API'leri hakkında daha fazla bilgi için aşağıdaki belgelere bakın:

- [Azure kurtarma Hizmetleri Sağlayıcısı REST API'si](/rest/api/recoveryservices/)
- [Azure REST API'si ile çalışmaya başlama](/rest/api/azure/)
