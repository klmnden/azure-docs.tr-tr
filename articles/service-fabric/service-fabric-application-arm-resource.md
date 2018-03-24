---
title: Dağıtma ve yükseltme uygulamaları ve Hizmetleri Azure Resource Manager ile | Microsoft Docs
description: Uygulamaları ve Hizmetleri Azure Resource Manager şablonu kullanarak bir Service Fabric kümesi dağıtmayı öğrenin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/06/2017
ms.author: dekapur
ms.openlocfilehash: a49c40efd1bab83dd274fd126c383a2d68354f75
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="manage-applications-and-services-as-azure-resource-manager-resources"></a>Uygulamalar ve hizmetler Azure Resource Manager kaynaklarını yönetme

Service Fabric kümenizi Azure Resource Manager aracılığıyla üzerine uygulamaları ve Hizmetleri dağıtabilirsiniz. Bu uygulamaları dağıtma ve PowerShell veya CLI aracılığıyla küme hazır olmasını beklemek zorunda sonra yönetme yerine, artık uygulamalar ve hizmetler JSON içinde express ve bunları kümenizi aynı Resource Manager şablonu dağıtma olduğunu anlamına gelir. Uygulama kayıt, sağlama ve dağıtım tüm işlem tek bir adımda gerçekleşir.

Bu, tüm Kurulum, idare veya kümenizde gerektiren küme yönetimi uygulamaları dağıtmak önerilen yoludur. Bu içerir [düzeltme eki Orchestration uygulama](service-fabric-patch-orchestration-application.md), Watchdogs veya diğer uygulamalar veya hizmetler dağıtılmadan önce kümenizdeki çalıştırması gereken herhangi bir uygulama. 

Uygun olduğunda, uygulamalarınızı geliştirmek için Resource Manager kaynakları olarak yönetin:
* Denetim izi: Kaynak Yöneticisi her işlemi denetler ve bir ayrıntılı tutar *etkinlik günlüğü* yardımcı olacak, bu uygulamalar ve kümenizi yapılan değişiklikleri izleme.
* Rol tabanlı erişim denetimi (RBAC): kümesi üzerinde dağıtılmış uygulamaların yanı sıra kümeleri erişimi yönetme aynı Resource Manager şablonu yapılabilir.
* Azure Kaynak Yöneticisi'ni (Azure portalı) aracılığıyla bir tane--küme ve kritik uygulama dağıtımlarını yönetmek için Mağazanız olur.

Aşağıdaki kod parçacığını bir şablon yönetilen kaynaklar farklı türde gösterir:

```json
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applications",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
    "location": "[variables('clusterLocation')]",
},
{
    "apiVersion": "2017-07-01-preview",
    "type": "Microsoft.ServiceFabric/clusters/applications/services",
    "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
    "location": "[variables('clusterLocation')]"
}
```


## <a name="add-a-new-application-to-your-resource-manager-template"></a>Resource Manager şablonu yeni bir uygulama Ekle

1. Kümenizin Resource Manager şablonu dağıtımı için hazırlayın. Bkz: [Azure Kaynak Yöneticisi'ni kullanarak bir Service Fabric kümesi oluştur](service-fabric-cluster-creation-via-arm.md) Bunun hakkında daha fazla bilgi için.
2. Bazı kümede dağıtmayı planladığınız uygulamalar düşünün. Her zaman, diğer uygulamaların çalışıyor olacak tüm bağımlılıkları devam edebilir var mı? Herhangi bir küme idare veya Kurulum uygulamaları dağıtmayı planlıyor musunuz? Bu tür uygulamalar, en iyi yukarıda açıklandığı gibi bir Resource Manager şablonu ile yönetilir. 
3. Bu şekilde olmasını istediğiniz hangi uygulamaların dağıttığını dahil edilir sonra uygulamaları paketlenmiş daraltılmış ve bir dosya paylaşımında put gerekmez. Paylaşımı bir REST uç noktası için Azure kaynak dağıtımı sırasında kullanmak için Yöneticisi üzerinden erişilebilir olması gerekir.
4. Aşağıda, küme bildirimi için Resource Manager şablonunda her uygulamanın özellikleri açıklar. Bu özellikler, çoğaltma veya örnek sayısının ve kaynaklar (diğer uygulamalara veya hizmetlere) arasında hiçbir bağımlılık zincirleri içerir. Kapsamlı özelliklerinin listesi için bkz: [REST API'si Swagger Spec](https://aka.ms/sfrpswaggerspec). Bu uygulamanın yerini almaz veya hizmeti bildirimleri, ancak bunun yerine kümenin Resource Manager şablonu parçası olarak bunları nedir bazıları açıklanmaktadır unutmayın. Durum bilgisiz hizmet dağıtma içeren bir örnek şablonu işte *Service1* ve durum bilgisi olan hizmet *Service2* parçası olarak *Application1*:

  ```json
  {
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "clusterName": {
        "type": "string",
        "defaultValue": "Cluster",
        "metadata": {
          "description": "Name of your cluster - Between 3 and 23 characters. Letters and numbers only."
        }
      },
      "applicationTypeName": {
        "type": "string",
        "defaultValue": "ApplicationType",
        "metadata": {
          "description": "The application type name."
        }
      },
      "applicationTypeVersion": {
        "type": "string",
        "defaultValue": "1",
        "metadata": {
          "description": "The application type version."
        }
      },
      "appPackageUrl": {
        "type": "string",
        "metadata": {
          "description": "The URL to the application package sfpkg file."
        }
      },
      "applicationName": {
        "type": "string",
        "defaultValue": "Application1",
        "metadata": {
          "description": "The name of the application resource."
        }
      },
      "serviceName": {
        "type": "string",
        "defaultValue": "Application1~Service1",
        "metadata": {
          "description": "The name of the service resource in the format of {applicationName}~{serviceName}."
        }
      },
      "serviceTypeName": {
        "type": "string",
        "defaultValue": "Service1Type",
        "metadata": {
          "description": "The name of the service type."
        }
      },
      "serviceName2": {
        "type": "string",
        "defaultValue": "Application1~Service2",
        "metadata": {
          "description": "The name of the service resource in the format of {applicationName}~{serviceName}."
        }
      },
      "serviceTypeName2": {
        "type": "string",
        "defaultValue": "Service2Type",
        "metadata": {
          "description": "The name of the service type."
        }
      }
    },
    "variables": {
      "clusterLocation": "[resourcegroup().location]"
    },
    "resources": [
      {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters/applicationTypes",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [],
        "properties": {
          "provisioningState": "Default"
        }
      },
      {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters/applicationTypes/versions",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationTypeName'), '/', parameters('applicationTypeVersion'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applicationTypes/', parameters('applicationTypeName'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "appPackageUrl": "[parameters('appPackageUrl')]"
        }
      },
      {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters/applications",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applicationTypes/', parameters('applicationTypeName'), '/versions/', parameters('applicationTypeVersion'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "typeName": "[parameters('applicationTypeName')]",
          "typeVersion": "[parameters('applicationTypeVersion')]",
          "parameters": {},
          "upgradePolicy": {
            "upgradeReplicaSetCheckTimeout": "01:00:00.0",
            "forceRestart": "false",
            "rollingUpgradeMonitoringPolicy": {
              "healthCheckWaitDuration": "00:02:00.0",
              "healthCheckStableDuration": "00:05:00.0",
              "healthCheckRetryTimeout": "00:10:00.0",
              "upgradeTimeout": "01:00:00.0",
              "upgradeDomainTimeout": "00:20:00.0"
            },
            "applicationHealthPolicy": {
              "considerWarningAsError": "false",
              "maxPercentUnhealthyDeployedApplications": "50",
              "defaultServiceTypeHealthPolicy": {
                "maxPercentUnhealthyServices": "50",
                "maxPercentUnhealthyPartitionsPerService": "50",
                "maxPercentUnhealthyReplicasPerPartition": "50"
              }
            }
          }
        }
      },
      {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters/applications/services",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applications/', parameters('applicationName'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "serviceKind": "Stateless",
          "serviceTypeName": "[parameters('serviceTypeName')]",
          "instanceCount": "-1",
          "partitionDescription": {
            "partitionScheme": "Singleton"
          },
          "correlationScheme": [],
          "serviceLoadMetrics": [],
          "servicePlacementPolicies": []
        }
      },
      {
        "apiVersion": "2017-07-01-preview",
        "type": "Microsoft.ServiceFabric/clusters/applications/services",
        "name": "[concat(parameters('clusterName'), '/', parameters('applicationName'), '/', parameters('serviceName2'))]",
        "location": "[variables('clusterLocation')]",
        "dependsOn": [
          "[concat('Microsoft.ServiceFabric/clusters/', parameters('clusterName'), '/applications/', parameters('applicationName'))]"
        ],
        "properties": {
          "provisioningState": "Default",
          "serviceKind": "Stateful",
          "serviceTypeName": "[parameters('serviceTypeName2')]",
          "targetReplicaSetSize": "3",
          "minReplicaSetSize": "2",
          "replicaRestartWaitDuration": "00:01:00.0",
          "quorumLossWaitDuration": "00:02:00.0",
          "standByReplicaKeepDuration": "00:00:30.0",
          "partitionDescription": {
            "partitionScheme": "UniformInt64Range",
            "count": "5",
            "lowKey": "1",
            "highKey": "5"
          },
          "hasPersistedState": "true",
          "correlationScheme": [],
          "serviceLoadMetrics": [],
          "servicePlacementPolicies": [],
          "defaultMoveCost": "Low"
        }
      }
    ]
  }
  ```

  > [!NOTE] 
  > *ApiVersion* ayarlanmalıdır `"2017-07-01-preview"`. Küme zaten dağıtılmış olduğu sürece bu şablonu ayrıca küme bağımsız olarak dağıtılabilir.

5. Dağıtma! 

## <a name="manage-an-existing-application-via-resource-manager"></a>Varolan bir uygulama aracılığıyla Kaynak Yöneticisi'ni yönetme

Kümenizi zaten ve bazı uygulamaları olarak kaynak kaynakları zaten, üzerinde uygulamaları kaldırma yerine dağıtılan Yöneticisi yönetmek isterdiniz ve bunları yeniden dağıtırken, uygulamalar için aynı API'lerini kullanarak PUT çağrı kullanabilirsiniz alın Resource Manager kaynakları olarak onaylandı. 


## <a name="next-steps"></a>Sonraki adımlar

* Kullanım [Service Fabric CLI](service-fabric-cli.md) veya [PowerShell](service-fabric-deploy-remove-applications.md) kümenize diğer uygulamaları dağıtmak için. 
* [Service Fabric kümesi yükseltme](service-fabric-cluster-upgrade.md)

