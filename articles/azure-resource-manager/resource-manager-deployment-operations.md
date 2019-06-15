---
title: Dağıtım geçmişi ile Azure Resource Manager | Microsoft Docs
description: Portal, PowerShell, Azure CLI ve REST API'si ile Azure Resource Manager dağıtım işlemlerini görüntülemeyi açıklar.
tags: top-support-issue
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: tomfitz
ms.openlocfilehash: 58d22e3fcae5c30e5d7dcc39b317afeef4a693ee
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65605903"
---
# <a name="view-deployment-history-with-azure-resource-manager"></a>Azure Resource Manager dağıtım geçmişini görüntüle

Azure Resource Manager deployment geçmişinizi görüntüleyin ve belirli işlemleri son dağıtımlarında incelemenizi sağlar. Dağıtılan kaynakları görmek ve hatalar hakkında bilgi edinin.

Belirli dağıtım hatalarını çözme hakkında bilgi için bkz: [kaynakları Azure Resource Manager ile azure'a dağıtırken sık karşılaşılan hataları çözme](resource-manager-common-deployment-errors.md).

## <a name="portal"></a>Portal

Dağıtım geçmişinden bir dağıtım hakkında bilgi almak için.

1. İncelemek istediğiniz kaynak grubunu seçin.

1. Altındaki bağlantıyı seçin **dağıtımları**.

   ![Dağıtım geçmişi seçin](./media/resource-manager-deployment-operations/select-deployment-history.png)

1. Dağıtımlardan biri dağıtım geçmişinden seçin.

   ![Dağıtımını seçin](./media/resource-manager-deployment-operations/select-details.png)

1. Dağıtılan kaynakların bir listesini de dahil olmak üzere dağıtımın bir özeti görüntülenir.

    ![Dağıtım özeti](./media/resource-manager-deployment-operations/view-deployment-summary.png)

1. Dağıtım için kullanılan şablonu görmek için seçin **şablon**. Şablonu yeniden yükleyebilirsiniz.

    ![Şablonu Göster](./media/resource-manager-deployment-operations/show-template-from-history.png)

1. Dağıtım başarısız olursa, bir hata iletisi görürsünüz. Daha fazla ayrıntı için hata iletisini seçin.

    ![başarısız dağıtım görüntüleyin](./media/resource-manager-deployment-operations/show-error.png)

1. Ayrıntılı hata iletisi görüntülenir.

    ![Hata ayrıntılarını görüntüle](./media/resource-manager-deployment-operations/show-details.png)

1. Bağıntı kimliği ilgili olayları izlemek için kullanılır ve bir dağıtım sorunlarını gidermek için teknik destek ile çalışırken yararlı olabilir.

    ![Bağıntı Kimliğini alın](./media/resource-manager-deployment-operations/get-correlation-id.png)

1. Başarısız olan adımı hakkında daha fazla bilgi için seçin **işlem ayrıntıları**.

    ![Dağıtım işlemlerini belirleyin](./media/resource-manager-deployment-operations/select-deployment-operations.png)

1. Bu aşama dağıtımın ayrıntılarını görürsünüz.

    ![İşlem ayrıntıları göster](./media/resource-manager-deployment-operations/show-operation-details.png)

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bir dağıtımın genel durumunu almak için kullanın **Get-AzResourceGroupDeployment** komutu.

```azurepowershell-interactive
Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup
```

Ya da başarısız olan dağıtımları için sonuçları filtreleyebilirsiniz.

```azurepowershell-interactive
Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
```

Bağıntı kimliği ilgili olayları izlemek için kullanılır ve bir dağıtım sorunlarını gidermek için teknik destek ile çalışırken yararlı olabilir. Bağıntı Kimliğini almak için kullanın:

```azurepowershell-interactive
(Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -DeploymentName azuredeploy).CorrelationId
```

Her dağıtım, birden çok işlem içerir. Her işlem, dağıtım işleminde bir adımı temsil eder. Bir dağıtım ile sorun nedir bulmak için genellikle dağıtım işlemleri hakkındaki ayrıntıları görmek gerekir. İşlemlerle durumunu görebilirsiniz **Get-AzResourceGroupDeploymentOperation**.

```azurepowershell-interactive
Get-AzResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName azuredeploy
```

Her biri için aşağıdaki biçimde ile birden çok işlem döndürür:

```powershell
Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
OperationId    : A3EB2DA598E0A780
Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2019-05-13T21:42:40.7151512Z;
                duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}
```

Başarısız işlemler hakkında daha fazla bilgi almak için işlem özelliklerini almak **başarısız** durumu.

```azurepowershell-interactive
(Get-AzResourceGroupDeploymentOperation -DeploymentName azuredeploy -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
```

Her biri için aşağıdaki biçimde ile başarısız olan tüm işlemler döndürür:

```powershell
provisioningOperation : Create
provisioningState     : Failed
timestamp             : 2019-05-13T21:42:40.7151512Z
duration              : PT3.1449887S
trackingId            : f4ed72f8-4203-43dc-958a-15d041e8c233
serviceRequestId      : a426f689-5d5a-448d-a2f0-9784d14c900a
statusCode            : BadRequest
statusMessage         : @{error=}
targetResource        : @{id=/subscriptions/{guid}/resourceGroups/ExampleGroup/providers/
                       Microsoft.Network/publicIPAddresses/myPublicIP;
                       resourceType=Microsoft.Network/publicIPAddresses; resourceName=myPublicIP}
```

ServiceRequestId ve işlemi için izleme kimliğini not edin. ServiceRequestId bir dağıtım sorunlarını gidermek için teknik destek ile çalışırken yararlı olabilir. Trackingıd, belirli bir işlem odaklanmak için sonraki adımda kullanacaksınız.

Başarısız olan belirli bir işlemi durum iletisini almak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
((Get-AzResourceGroupDeploymentOperation -DeploymentName azuredeploy -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
```

Döndürür:

```powershell
code           message                                                                        details
----           -------                                                                        -------
DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}
```

Her Azure dağıtım işleminde istek ve yanıt içeriği de bulunmaktadır. Dağıtım sırasında kullandığınız **DeploymentDebugLogLevel** istek ve/veya yanıt tutulduğunu belirlemek için parametre.

Günlük bilgileri alın ve aşağıdaki PowerShell komutlarını kullanarak yerel olarak kaydedin:

```powershell
(Get-AzResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.request | ConvertTo-Json |  Out-File -FilePath <PathToFile>

(Get-AzResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.response | ConvertTo-Json |  Out-File -FilePath <PathToFile>
```

## <a name="azure-cli"></a>Azure CLI

Bir dağıtımın genel durumunu almak için kullanın **azure grubu dağıtım show** komutu.

```azurecli-interactive
az group deployment show -g ExampleGroup -n ExampleDeployment
```
  
Bağıntı kimliği ilgili olayları izlemek için kullanılır ve bir dağıtım sorunlarını gidermek için teknik destek ile çalışırken yararlı olabilir.

```azurecli-interactive
az group deployment show -g ExampleGroup -n ExampleDeployment --query properties.correlationId
```

Bir dağıtım için işlemleri görmek için bu seçeneği kullanın:

```azurecli-interactive
az group deployment operation list -g ExampleGroup -n ExampleDeployment
```

## <a name="rest"></a>REST

Bir dağıtımı hakkında bilgi almak için kullanın [şablon dağıtımı hakkında bilgi alma](https://docs.microsoft.com/rest/api/resources/deployments) işlemi.

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
```

Yanıt olarak, özellikle dikkat edin **provisioningState**, **Correlationıd**, ve **hata** öğeleri. **Correlationıd** ilgili olayları izlemek için kullanılır ve bir dağıtım sorunlarını gidermek için teknik destek ile çalışırken yararlı olabilir.

```json
{ 
 ...
 "properties": {
   "provisioningState":"Failed",
   "correlationId":"d5062e45-6e9f-4fd3-a0a0-6b2c56b15757",
   ...
   "error":{
     "code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-debug for usage details.",
     "details":[{"code":"Conflict","message":"{\r\n  \"error\": {\r\n    \"message\": \"Conflict\",\r\n    \"code\": \"Conflict\"\r\n  }\r\n}"}]
   }  
 }
}
```

Dağıtımları hakkında bilgi almak için kullanın [tüm şablon dağıtım işlemlerini listelemek](https://docs.microsoft.com/rest/api/resources/deployments). 

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}
```
   
Yanıt, belirtilen üzerinde temel istek ve/veya yanıt bilgi içeriyor **debugSetting** özellik dağıtım sırasında.

```json
{
 ...
 "properties": 
 {
   ...
   "request":{
     "content":{
       "location":"West US",
       "properties":{
         "accountType": "Standard_LRS"
       }
     }
   },
   "response":{
     "content":{
       "error":{
         "message":"Conflict","code":"Conflict"
       }
     }
   }
 }
}
```

## <a name="next-steps"></a>Sonraki adımlar
* Belirli dağıtım hatalarını çözme hakkında bilgi için bkz: [kaynakları Azure Resource Manager ile azure'a dağıtırken sık karşılaşılan hataları çözme](resource-manager-common-deployment-errors.md).
* Diğer tür eylem izlemek için etkinlik günlüklerini kullanma hakkında bilgi edinmek için [Azure kaynaklarını yönetmek için etkinlik günlüklerini görüntüleme](resource-group-audit.md).
* Dağıtımınız, yürütmeden önce doğrulamak için bkz: [Azure Resource Manager şablonu ile bir kaynak grubu dağıtma](resource-group-template-deploy.md).

