---
title: Azure Resource Manager ile dağıtım işlemlerini | Microsoft Docs
description: Portal, PowerShell, Azure CLI ve REST API'si ile Azure Resource Manager dağıtım işlemlerini görüntülemeyi açıklar.
services: azure-resource-manager,virtual-machines
documentationcenter: ''
tags: top-support-issue
author: tfitzmac
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-multiple
ms.workload: infrastructure
ms.date: 09/28/2018
ms.author: tomfitz
ms.openlocfilehash: 9bb6491565f685e8ca3d7a6271747a5df3629e81
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56269085"
---
# <a name="view-deployment-operations-with-azure-resource-manager"></a>Azure Resource Manager ile dağıtım işlemlerini görüntüleme

Azure portalı üzerinden bir dağıtım için işlemleri görüntüleyebilirsiniz. En çok başarısız olan işlemleri görüntülemek için bu makalede odaklanır. Bu nedenle dağıtım sırasında bir hata aldınız, işlemleri görüntülemek istiyorsanız olabilir. Portal, kolayca hataları bulmak ve olası düzeltmeleri belirlemek için etkinleştiren bir arayüz sağlar.

Denetim günlükleri, veya dağıtım işlemlerini bakarak dağıtımınızı giderebilirsiniz. Bu makalede her iki yöntem de gösterilir. Belirli dağıtım hatalarını çözme hakkında bilgi için bkz: [kaynakları Azure Resource Manager ile azure'a dağıtırken sık karşılaşılan hataları çözme](resource-manager-common-deployment-errors.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="portal"></a>Portal

Dağıtım işlemlerini görmek için aşağıdaki adımları kullanın:

1. Dağıtımı ile ilgili kaynak grubu için son dağıtım durumu dikkat edin. Daha fazla bilgi edinmek için bu durum seçebilirsiniz.
   
    ![Dağıtım durumu](./media/resource-manager-deployment-operations/deployment-status.png)
2. Son dağıtım geçmişini görürsünüz. Başarısız olan dağıtımı seçin.
   
    ![Dağıtım durumu](./media/resource-manager-deployment-operations/select-deployment.png)
3. Dağıtım başarısız olmasının bir açıklama görmek için bağlantıyı seçin. Aşağıdaki görüntüde, DNS kaydının benzersiz değil.  
   
    ![başarısız dağıtım görüntüleyin](./media/resource-manager-deployment-operations/view-error.png)
   
    Bu hata iletisi, sorun gidermeye başlamak yeterli olmalıdır. Ancak, hakkında daha fazla ayrıntı gerekiyorsa hangi görevlerin tamamlandığını, aşağıdaki adımlarda gösterildiği gibi işlemleri görüntüleyebilirsiniz.
4. Tüm dağıtım işlemlerini görüntüleyebilirsiniz. Daha fazla ayrıntı için herhangi bir işlem seçin.
   
    ![işlemlerini görüntüleme](./media/resource-manager-deployment-operations/view-operations.png)
   
    Bu durumda, depolama hesabı, sanal ağ ve kullanılabilirlik kümesi başarıyla oluşturulduğunu görürsünüz. Genel IP adresini başarısız oldu ve diğer kaynakları değil girişiminde bulunulması.
5. Dağıtım için olayları seçerek görüntüleyebilirsiniz **olayları**.
   
    ![Olayları görüntüle](./media/resource-manager-deployment-operations/view-events.png)
6. Dağıtımı için tüm olayları görmek ve daha fazla ayrıntı için herhangi birini seçin. Bağıntı kimlikleri dikkat edin. Bu değer, bir dağıtım sorunlarını gidermek için teknik destek ile çalışırken yararlı olabilir.
   
    ![olaylara bakın](./media/resource-manager-deployment-operations/see-all-events.png)

## <a name="powershell"></a>PowerShell
1. Bir dağıtımın genel durumunu almak için kullanın **Get-AzResourceGroupDeployment** komutu. 

  ```powershell
  Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup
  ```

   Ya da başarısız olan dağıtımları için sonuçları filtreleyebilirsiniz.

  ```powershell
  Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
  ```
   
2. Bağıntı Kimliğini almak için kullanın:

  ```powershell
  (Get-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -DeploymentName azuredeploy).CorrelationId
  ```

3. Her dağıtım, birden çok işlem içerir. Her işlem, dağıtım işleminde bir adımı temsil eder. Bir dağıtım ile sorun nedir bulmak için genellikle dağıtım işlemleri hakkındaki ayrıntıları görmek gerekir. İşlemlerle durumunu görebilirsiniz **Get-AzResourceGroupDeploymentOperation**.

  ```powershell 
  Get-AzResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName vmDeployment
  ```

    Her biri için aşağıdaki biçimde ile birden çok işlem döndürür:

  ```powershell
  Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
  OperationId    : A3EB2DA598E0A780
  Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                   duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                   serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
  PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                   serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}
  ```

4. Başarısız işlemler hakkında daha fazla bilgi almak için işlem özelliklerini almak **başarısız** durumu.

  ```powershell
  (Get-AzResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
  ```
   
    Her biri için aşağıdaki biçimde ile başarısız olan tüm işlemler döndürür:

  ```powershell
  provisioningOperation : Create
  provisioningState     : Failed
  timestamp             : 2016-06-14T21:54:55.1468068Z
  duration              : PT3.1449887S
  trackingId            : f4ed72f8-4203-43dc-958a-15d041e8c233
  serviceRequestId      : a426f689-5d5a-448d-a2f0-9784d14c900a
  statusCode            : BadRequest
  statusMessage         : @{error=}
  targetResource        : @{id=/subscriptions/{guid}/resourceGroups/ExampleGroup/providers/
                          Microsoft.Network/publicIPAddresses/myPublicIP;
                          resourceType=Microsoft.Network/publicIPAddresses; resourceName=myPublicIP}
  ```

    ServiceRequestId ve işlemi için izleme kimliğini not edin. ServiceRequestId bir dağıtım sorunlarını gidermek için teknik destek ile çalışırken yararlı olabilir. Belirli bir işlem odaklanmak için sonraki adımda Trackingıd kullanır.
5. Başarısız olan belirli bir işlemi durum iletisini almak için aşağıdaki komutu kullanın:

  ```powershell
  ((Get-AzResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
  ```

    Döndürür:

  ```powershell
  code           message                                                                        details
  ----           -------                                                                        -------
  DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}
  ```
6. Her Azure dağıtım işleminde istek ve yanıt içeriği de bulunmaktadır. Hangi, dağıtım sırasında Azure için gönderilen istek içeriği olan (örneğin, bir VM oluşturun. işletim sistemi diski ve diğer kaynaklar). Azure dağıtım isteğinizden geri gönderilen yanıt içeriktir. Dağıtım sırasında kullandığınız **DeploymentDebugLogLevel** parametresini kullanarak istek ve/veya yanıt günlüğünde tutulduğu belirtin. 

  Günlük bilgileri alın ve aşağıdaki PowerShell komutlarını kullanarak yerel olarak kaydedin:

  ```powershell
  (Get-AzResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.request | ConvertTo-Json |  Out-File -FilePath <PathToFile>

  (Get-AzResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.response | ConvertTo-Json |  Out-File -FilePath <PathToFile>
  ```

## <a name="azure-cli"></a>Azure CLI

1. Genel dağıtım durumu alma **azure grubu dağıtım show** komutu.

  ```azurecli
  az group deployment show -g ExampleGroup -n ExampleDeployment
  ```
  
2. Döndürülen değerlerden biri **Correlationıd**. Bu değer, ilgili olayları izlemek için kullanılır ve bir dağıtım sorunlarını gidermek için teknik destek ile çalışırken yararlı olabilir.

  ```azurecli
  az group deployment show -g ExampleGroup -n ExampleDeployment --query properties.correlationId
  ```

3. Bir dağıtım için işlemleri görmek için bu seçeneği kullanın:

  ```azurecli
  az group deployment operation list -g ExampleGroup -n ExampleDeployment
  ```

## <a name="rest"></a>REST

1. Dağıtım hakkında bilgi almak [şablon dağıtımı hakkında bilgi alma](https://docs.microsoft.com/rest/api/resources/deployments) işlemi.

  ```http
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

2. Dağıtımlar hakkında bilgi almak [tüm şablon dağıtım işlemlerini listelemek](https://docs.microsoft.com/rest/api/resources/deployments). 

  ```http
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

