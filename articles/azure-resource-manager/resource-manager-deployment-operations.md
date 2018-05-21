---
title: Dağıtım işlemlerini Azure Resource Manager ile | Microsoft Docs
description: Portal, PowerShell, Azure CLI ve REST API ile Azure Resource Manager dağıtım işlemlerini görüntülemeyi açıklar.
services: azure-resource-manager,virtual-machines
documentationcenter: ''
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ''
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-multiple
ms.workload: infrastructure
ms.date: 04/23/2018
ms.author: tomfitz
ms.openlocfilehash: 523ea3bf5d41231ab3281f9d8eb1fac8c3dfb55f
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="view-deployment-operations-with-azure-resource-manager"></a>Görünüm dağıtım işlemlerini Azure Resource Manager ile

Azure Portalı aracılığıyla bir dağıtım için işlemleri görüntüleyebilirsiniz. Başarısız olan işlemleri görüntülemek için bu makalede odaklanır şekilde dağıtımı sırasında bir hata aldınız zaman işlemleri içinde görüntüleme en ilgi çekici olabilir. Portal, kolayca hataları bulma ve olası düzeltmeleri belirlemek sağlayan bir arabirim sağlar.

Denetim günlüklerini ya da dağıtım işlemlerini bakarak dağıtımınızı giderebilirsiniz. Bu makalede her iki yöntem gösterilmektedir. Belirli dağıtım hatalarını çözme konusunda daha fazla yardım için bkz: [kaynakları Azure Azure Resource Manager ile dağıtırken sık karşılaşılan hataları gidermek](resource-manager-common-deployment-errors.md).

## <a name="portal"></a>Portal
Dağıtım işlemlerini görmek için aşağıdaki adımları kullanın:

1. Dağıtımı ile ilgili kaynak grubu için son dağıtım durumunu dikkat edin. Daha fazla bilgi almak için bu durum seçebilirsiniz.
   
    ![Dağıtım durumu](./media/resource-manager-deployment-operations/deployment-status.png)
2. En son dağıtım geçmişini görürsünüz. Başarısız olan dağıtımı seçin.
   
    ![Dağıtım durumu](./media/resource-manager-deployment-operations/select-deployment.png)
3. Dağıtım neden başarısız açıklamasını görmek için bağlantıya seçin. Aşağıdaki resimde DNS kaydı benzersiz değil.  
   
    ![başarısız dağıtım görüntüleyin](./media/resource-manager-deployment-operations/view-error.png)
   
    Bu hata iletisi, sorun giderme başlatmak yeterli olmalıdır. Ancak, hakkında daha fazla ayrıntı gerekiyorsa hangi görevlerin tamamlandığını, aşağıdaki adımlarda gösterildiği gibi işlemleri görüntüleyebilirsiniz.
4. Tüm dağıtım işlemlerini görüntüleyebilirsiniz. Daha fazla ayrıntı için herhangi bir işlem seçin.
   
    ![Görünüm işlemleri](./media/resource-manager-deployment-operations/view-operations.png)
   
    Bu durumda, depolama hesabı, sanal ağ ve kullanılabilirlik kümesi başarıyla oluşturulduğunu görürsünüz. Genel IP adresi başarısız oldu ve diğer kaynakları değil girişiminde bulunuldu.
5. Dağıtım için olayları seçerek görüntüleyebilirsiniz **olayları**.
   
    ![olaylarını görüntüle](./media/resource-manager-deployment-operations/view-events.png)
6. Dağıtımı için tüm olayları görmek ve daha fazla ayrıntı için herhangi bir tanesini seçin. Bağıntı kimlikleri dikkat edin. Bu değer, bir dağıtım gidermek için teknik destek ile çalışırken yararlı olabilir.
   
    ![olaylara bakın](./media/resource-manager-deployment-operations/see-all-events.png)

## <a name="powershell"></a>PowerShell
1. Bir dağıtımın genel durumunu almak için **Get-AzureRmResourceGroupDeployment** komutu. 

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup
  ```

   Veya, başarısız olan dağıtımları için sonuçları filtreleyebilirsiniz.

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
  ```
   
2. Her dağıtımda birden çok işlemlerini içerir. Her işlem, dağıtım işlemi bir adımda temsil eder. Neyin dağıtımla yanlış gittiğini bulmak için genellikle dağıtım işlemleri ile ilgili ayrıntıları görmek gerekir. İşlemlerle durumunu görebilirsiniz **Get-AzureRmResourceGroupDeploymentOperation**.

  ```powershell 
  Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName vmDeployment
  ```

    Aşağıdaki biçimde her biri birden çok işlemleriyle döndürür:

  ```powershell
  Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
  OperationId    : A3EB2DA598E0A780
  Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                   duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                   serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
  PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                   serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}
  ```

3. Başarısız işlemler hakkında daha fazla bilgi almak için işlemleriyle özelliklerini almak **başarısız** durumu.

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
  ```
   
    Aşağıdaki biçimde her biri ile başarısız olan tüm işlemler döndürür:

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

    ServiceRequestId ve işlem için Trackingıd unutmayın. ServiceRequestId bir dağıtım gidermek için teknik destek ile çalışırken yararlı olabilir. Belirli bir işlemi odaklanmak için sonraki adımda Trackingıd kullanır.
4. Belirli bir başarısız işlem durum iletisini almak için aşağıdaki komutu kullanın:

  ```powershell
  ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
  ```

    Hangi döndürür:

  ```powershell
  code           message                                                                        details
  ----           -------                                                                        -------
  DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}
  ```
4. Her dağıtım işlem Azure, istek ve yanıt içeriği içerir. Ne, dağıtım sırasında Azure'a gönderilen istek içeriği olduğu (örneğin, bir VM oluşturma işletim sistemi diski ve diğer kaynakları). Azure yanıt içeriği ne geri dağıtımı isteğinden sayısıdır. Dağıtım sırasında kullandığınız **DeploymentDebugLogLevel** isteğin ve/veya yanıt günlüğüne korunur belirtmek için parametre. 

  Günlükteki bu bilgileri almak ve aşağıdaki PowerShell komutlarını kullanarak yerel olarak kaydedin:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.request | ConvertTo-Json |  Out-File -FilePath <PathToFile>

  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.response | ConvertTo-Json |  Out-File -FilePath <PathToFile>
  ```

## <a name="azure-cli"></a>Azure CLI

1. Dağıtımla genel durumunu elde **azure Grup dağıtım Göster** komutu.

  ```azurecli
  az group deployment show -g ExampleGroup -n ExampleDeployment
  ```
  
1. Döndürülen değerlerinden biri **correlationıd değeri**. Bu değer, ilgili olayları izlemek için kullanılır ve bir dağıtım gidermek için teknik destek ile çalışırken yararlı olabilir.

  ```azurecli
  az group deployment show -g ExampleGroup -n ExampleDeployment --query properties.correlationId
  ```

1. Bir dağıtım için işlemleri görmek için kullanın:

  ```azurecli
  az group deployment operation list -g ExampleGroup -n ExampleDeployment
  ```

## <a name="rest"></a>REST

1. Olan bir dağıtım hakkında bilgi almak [şablon dağıtımı hakkında bilgi alma](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get) işlemi.

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
  ```

    Yanıtta, özellikle dikkat edin **provisioningState**, **correlationıd değeri**, ve **hata** öğeleri. **Correlationıd değeri** ilgili olayları izlemek için kullanılır ve bir dağıtım gidermek için teknik destek ile çalışırken yararlı olabilir.

  ```json
  { 
    ...
    "properties": {
      "provisioningState":"Failed",
      "correlationId":"d5062e45-6e9f-4fd3-a0a0-6b2c56b15757",
      ...
      "error":{
        "code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see http://aka.ms/arm-debug for usage details.",
        "details":[{"code":"Conflict","message":"{\r\n  \"error\": {\r\n    \"message\": \"Conflict\",\r\n    \"code\": \"Conflict\"\r\n  }\r\n}"}]
      }  
    }
  }
  ```

2. Dağıtımlar hakkında bilgi almak [tüm şablon dağıtım işlemlerini listelemek](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_List). 

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}
  ```
   
    İsteğin ve/veya yanıt bilgileri ne belirttiğiniz üzerinde göre yanıt içeriyor **debugSetting** dağıtımı sırasında özelliği.

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
* Belirli dağıtım hatalarını çözme konusunda daha fazla yardım için bkz: [kaynakları Azure Azure Resource Manager ile dağıtırken sık karşılaşılan hataları gidermek](resource-manager-common-deployment-errors.md).
* Diğer Eylemler türlerini izlemek için etkinlik günlükleri kullanma hakkında bilgi edinmek için bkz: [görüntülemek Azure kaynaklarınızı yönetmek için etkinlik günlükleri](resource-group-audit.md).
* Dağıtımınız, yürütmeden önce doğrulamak için bkz: [Azure Resource Manager şablonu ile bir kaynak grubu dağıtma](resource-group-template-deploy.md).

