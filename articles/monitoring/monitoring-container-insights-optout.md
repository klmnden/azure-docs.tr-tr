---
title: İzleme bilgisayarınızı Azure Kubernetes Hizmeti Durdur kümesine nasıl | Microsoft Docs
description: Bu makalede, Azure AKS kümenizi kapsayıcılar için Azure İzleyici ile izleme nasıl kesmek açıklanır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/13/2018
ms.author: magoedte
ms.openlocfilehash: 2b989fbebe237e4e3746ef2f237193587173dfe4
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46963415"
---
# <a name="how-to-stop-monitoring-your-azure-kubernetes-service-aks-azure-monitor-for-containers"></a>İzleme, kapsayıcılar için Azure Kubernetes Service (AKS) Azure İzleyicisi durdurma

Sonra AKS kümenizi izleme etkinleştirirseniz, artık bunu izlemek istediğiniz karar, yapabilecekleriniz *çıkma* PowerShell cmdlet'i ile sağlanan Azure Resource Manager şablonları kullanarak  **Yeni-AzureRmResourceGroupDeployment** veya Azure CLI. Bir JSON şablonunu belirtir yapılandırmayı *çıkma*. Diğer küme olarak dağıtılan AKS küme kaynağı kimliği ve kaynak grubu belirtmek için yapılandırdığınız parametre değerlerini içerir. 

Bir şablon kullanarak kaynakları dağıtma kavramıyla bilmiyorsanız, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md)
* [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../azure-resource-manager/resource-group-template-deploy-cli.md)

Azure CLI'yı kullanmayı seçerseniz, ilk CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLI Sürüm 2.0.27 çalıştırıyor olmanız gerekir veya üzeri. Sürümünüzü belirlemek için çalıştırma `az --version`. Gerekirse yükleyin veya Azure CLI'yı yükseltmek için bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). 

## <a name="create-template"></a>Şablon oluşturma

1. Aşağıdaki JSON söz dizimini kopyalayıp dosyanıza yapıştırın:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "aksResourceId": {
           "type": "string",
           "metadata": {
             "description": "AKS Cluster Resource ID"
           }
       },
      "aksResourceLocation": {
        "type": "string",
        "metadata": {
           "description": "Location of the AKS resource e.g. \"East US\""
         }
       }
    },
    "resources": [
      {
        "name": "[split(parameters('aksResourceId'),'/')[8]]",
        "type": "Microsoft.ContainerService/managedClusters",
        "location": "[parameters('aksResourceLocation')]",
        "apiVersion": "2018-03-31",
        "properties": {
          "mode": "Incremental",
          "id": "[parameters('aksResourceId')]",
          "addonProfiles": {
            "omsagent": {
              "enabled": false,
              "config": null
            }
           }
         }
       }
      ]
    }
    ```

2. Bu dosyayı farklı Kaydet **OptOutTemplate.json** yerel bir klasöre.
3. Aşağıdaki JSON söz dizimi dosyanıza yapıştırın:

    ```json
    {
     "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
     "contentVersion": "1.0.0.0",
     "parameters": {
       "aksResourceId": {
         "value": "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
      },
      "aksResourceLocation": {
        "value": "<aksClusterRegion>"
        }
      }
    }
    ```

4. Değerlerini düzenleyin **aksResourceId** ve **aksResourceLocation** bulabileceğiniz AKS kümesinin değerleri kullanarak **özellikleri** seçili kümenin sayfası .

    ![Kapsayıcı Özellikleri Sayfası](./media/monitoring-container-health/container-properties-page.png)

    Üzerinde çalışırken **özellikleri** sayfasında, ayrıca kopyalayın **çalışma alanı kaynak kimliği**. Daha sonra Log Analytics çalışma alanını silmek istediğinize karar verirseniz, bu değer gereklidir. Log Analytics çalışma alanı siliniyor, bu işlemin bir parçası olarak yapılmaz. 

5. Bu dosyayı farklı Kaydet **OptOutParam.json** yerel bir klasöre.
6. Bu şablonu dağıtmaya hazırsınız. 

## <a name="remove-the-solution-using-azure-cli"></a>Azure CLI kullanarak çözümünü Kaldır
Çözüm kaldırın ve AKS kümenizde yapılandırma temizlemek için Linux'ta Azure CLI ile aşağıdaki komutu yürütün.

```azurecli
az login   
az account set --subscription "Subscription Name" 
az group deployment create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
```

Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuçları içeren aşağıdakine benzer bir ileti döndürülür:

```azurecli
ProvisioningState       : Succeeded
```

## <a name="remove-the-solution-using-powershell"></a>PowerShell kullanarak çözümünü Kaldır

Çözüm kaldırın ve AKS kümenizi yapılandırmasından temizlemek için şablonu içeren klasörde şu PowerShell komutlarını yürütün.    

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName <yourSubscriptionName>
New-AzureRmResourceGroupDeployment -Name opt-out -ResourceGroupName <ResourceGroupName> -TemplateFile .\OptOutTemplate.json -TemplateParameterFile .\OptOutParam.json
```

Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuçları içeren aşağıdakine benzer bir ileti döndürülür:

```powershell
ProvisioningState       : Succeeded
```

Yalnızca küme İzleme'yi desteklemek için çalışma alanı oluşturuldu ve artık gerekli olmadığında, el ile silmeniz gerekir. Bir çalışma alanını silme konusunda bilgi sahibi değilseniz bkz [Azure portalı ile bir Azure Log Analytics çalışma alanını silme](../log-analytics/log-analytics-manage-del-workspace.md). Hakkında unutmayın **çalışma alanı kaynak kimliği** biz 4. adımda daha önce kopyaladığınız, ihtiyacınız olacak. 

