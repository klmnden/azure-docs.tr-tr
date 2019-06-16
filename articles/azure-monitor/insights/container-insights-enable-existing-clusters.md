---
title: Dağıtılan bir Azure Kubernetes Service (AKS) kümesini izleme | Microsoft Docs
description: Aboneliğinizde zaten dağıtılmış kapsayıcılar için Azure İzleyici ile bir Azure Kubernetes Service (AKS) kümesini izleme etkinleştirmeyi öğrenin.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/25/2019
ms.author: magoedte
ms.openlocfilehash: ae340fb11d422b7516cc315e78be974d22239503
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65074688"
---
# <a name="enable-monitoring-of-azure-kubernetes-service-aks-cluster-already-deployed"></a>Azure Kubernetes Service (AKS) kümesini zaten dağıtılmış izlemeyi etkinleştir

Bu makalede, kapsayıcılar için Azure üzerinde barındırılan yönetilen Kubernetes kümesini izlemek için İzleyici ayarlanacağı açıklanır [Azure Kubernetes hizmeti](https://docs.microsoft.com/azure/aks/) zaten dağıtılan aboneliğinizde.

Desteklenen yöntemlerden birini kullanarak dağıtılmış bir AKS kümesi izleme etkinleştirebilirsiniz:

* Azure CLI
* Terraform
* [Azure İzleyicisi'nden](#enable-from-azure-monitor-in-the-portal) veya [AKS kümeye doğrudan](#enable-directly-from-aks-cluster-in-the-portal) Azure portalında 
* İle [Azure Resource Manager şablonu ile sağlanan](#enable-using-an-azure-resource-manager-template) Azure PowerShell cmdlet'ini kullanarak `New-AzResourceGroupDeployment` veya Azure CLI ile. 

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com) oturum açın. 

## <a name="enable-using-azure-cli"></a>Azure CLI kullanarak etkinleştirin

Aşağıdaki adım, Azure CLI kullanılarak AKS kümesini izlenmesini sağlar. Bu örnekte, başına oluşturmanız veya mevcut bir çalışma alanını belirtmeniz gerekmez. Bu komut bir bölgede zaten yoksa, varsayılan çalışma alanına AKS kümesi aboneliğin varsayılan kaynak grubu oluşturarak bu işlemi sizin için basitleştirir.  Oluşturulan varsayılan çalışma biçimi benzer *DefaultWorkspace -\<GUID >-\<bölge >* .  

```azurecli
az aks enable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG  
```

Çıktı şuna benzer:

```azurecli
provisioningState       : Succeeded
```

Yerine mevcut bir çalışma alanı ile tümleştirme, bu çalışma belirlemek için aşağıdaki komutu kullanın.

```azurecli
az aks enable-addons -a monitoring -n MyExistingManagedCluster -g MyExistingManagedClusterRG --workspace-resource-id <ExistingWorkspaceResourceID> 
```

Çıktı şuna benzer:

```azurecli
provisioningState       : Succeeded
```

## <a name="enable-using-terraform"></a>Terraform kullanarak etkinleştirin

1. Ekleme **oms_agent** mevcut eklenti profili [azurerm_kubernetes_cluster kaynak](https://www.terraform.io/docs/providers/azurerm/d/kubernetes_cluster.html#addon_profile)

   ```
   addon_profile {
    oms_agent {
      enabled                    = true
      log_analytics_workspace_id = "${azurerm_log_analytics_workspace.test.id}"
     }
   }
   ```

2. Ekleme [azurerm_log_analytics_solution](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_solution.html) Terraform belgeleri yer alan adımları uygulayarak.

## <a name="enable-from-azure-monitor-in-the-portal"></a>Portalda Azure İzleyicisi'nden etkinleştirin 

AKS kümenizin Azure portalında Azure İzleyicisi'nden izlemeyi etkinleştirmek için aşağıdakileri yapın:

1. Azure portalında **İzleyici**. 
2. Seçin **kapsayıcıları** listeden.
3. Üzerinde **İzleyicisi - kapsayıcıları** sayfasında **olmayan izlenen kümeleri**.
4. İzlenen kümelerinin listesini, kapsayıcı listede bulun ve tıklayın **etkinleştirme**.   
5. Üzerinde **kapsayıcılar için Azure İzleyici ekleme** sayfasında, mevcut bir Log Analytics varsa küme ile aynı abonelikte çalışma alanı, aşağı açılan listeden seçin.  
    Listenin varsayılan çalışma alanını ve AKS kapsayıcı abonelikte dağıtılmış konumunu belirler. 

    ![AKS kapsayıcı ınsights izlemeyi etkinleştir](./media/container-insights-onboard/kubernetes-onboard-brownfield-01.png)

    >[!NOTE]
    >Küme izleme verilerini depolamak için yeni bir Log Analytics çalışma alanı oluşturmak istiyorsanız,'ndaki yönergeleri izleyin [Log Analytics çalışma alanı oluşturma](../../azure-monitor/learn/quick-create-workspace.md). AKS kapsayıcı dağıtılan aynı abonelikte çalışma alanı oluşturmak emin olun. 
 
İzleme etkinleştirdikten sonra küme için sistem durumu ölçümleri görmeden önce yaklaşık 15 dakika sürebilir. 

## <a name="enable-directly-from-aks-cluster-in-the-portal"></a>AKS kümesi portalında doğrudan etkinleştir

Azure portalında AKS kümelerinizi birinden doğrudan izlemeyi etkinleştirmek için aşağıdakileri yapın:

1. Azure portalda **Tüm hizmetler**’i seçin. 
2. Kaynak listesinde yazmaya başlayın **kapsayıcıları**.  
    Girişinize bağlı listesini filtreler. 
3. Seçin **Kubernetes Hizmetleri**.  

    ![Kubernetes Hizmetleri Bağla](./media/container-insights-onboard/portal-search-containers-01.png)

4. Kapsayıcılar listesinde, bir kapsayıcıyı seçin.
5. Kapsayıcı genel bakış sayfasında **kapsayıcıları izleme**.  
6. Üzerinde **kapsayıcılar için Azure İzleyici ekleme** sayfasında, mevcut bir Log Analytics varsa küme ile aynı abonelikte çalışma alanı, aşağı açılan listeden seçin.  
    Listenin varsayılan çalışma alanını ve AKS kapsayıcı abonelikte dağıtılmış konumunu belirler. 

    ![AKS kapsayıcı sistem durumu izlemeyi etkinleştir](./media/container-insights-onboard/kubernetes-onboard-brownfield-02.png)

    >[!NOTE]
    >Küme izleme verilerini depolamak için yeni bir Log Analytics çalışma alanı oluşturmak istiyorsanız,'ndaki yönergeleri izleyin [Log Analytics çalışma alanı oluşturma](../../azure-monitor/learn/quick-create-workspace.md). AKS kapsayıcı dağıtılan aynı abonelikte çalışma alanı oluşturmak emin olun. 
 
İzleme etkinleştirdikten sonra küme için işletimsel veri görmeden önce yaklaşık 15 dakika sürebilir. 

## <a name="enable-using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanarak etkinleştirin

Bu yöntem, iki JSON şablonları içerir. Yapılandırmayı, izlemeyi etkinleştirmek için bir şablon belirtir ve diğer aşağıdaki belirtmek için yapılandırdığınız parametre değerlerini içerir:

* AKS kapsayıcı kaynak kimliği. 
* Kümenin dağıtıldığı kaynak grubu.

>[!NOTE]
>Şablon, aynı kaynak grubunda kümesi olarak dağıtılması gerekiyor.
>

Log Analytics çalışma alanını Azure PowerShell veya CLI kullanarak izlemeyi etkinleştirmeden önce oluşturulması gerekir. Çalışma alanı oluşturmak için bunu aracılığıyla ayarlayabilirsiniz [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)temellidir [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json), veya [Azure portalında](../../azure-monitor/learn/quick-create-workspace.md).

Bir şablon kullanarak kaynakları dağıtma kavramıyla alışkın değilseniz, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/resource-group-template-deploy.md)
* [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-cli.md)

Azure CLI'yı kullanmayı seçerseniz, ilk CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLI Sürüm 2.0.59 çalıştırıyor olmanız gerekir veya üzeri. Sürümünüzü belirlemek için çalıştırma `az --version`. Gerekirse yükleyin veya Azure CLI'yı yükseltmek için bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). 

### <a name="create-and-execute-a-template"></a>Oluşturma ve bir şablonu yürütme

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
    },
    "workspaceResourceId": {
      "type": "string",
      "metadata": {
         "description": "Azure Monitor Log Analytics Resource ID"
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
          "enabled": true,
          "config": {
            "logAnalyticsWorkspaceResourceID": "[parameters('workspaceResourceId')]"
          }
         }
       }
      }
     }
     ]
    }
    ```

2. Bu dosyayı farklı Kaydet **existingClusterOnboarding.json** yerel bir klasöre.
3. Aşağıdaki JSON söz dizimi dosyanıza yapıştırın:

    ```json
    {
       "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "aksResourceId": {
           "value": "/subscriptions/<SubscriptionId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
       },
       "aksResourceLocation": {
         "value": "<aksClusterLocation>"
       },
       "workspaceResourceId": {
         "value": "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroup>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>"
       }  
     }
    }
    ```

4. Değerlerini düzenleyin **aksResourceId** ve **aksResourceLocation** değerleri kullanarak **AKS'ye genel bakış** AKS kümesi sayfası. Değeri **workspaceResourceId** çalışma alanı adı içerir, Log Analytics çalışma alanının tam kaynak kimliği. 
5. Bu dosyayı farklı Kaydet **existingClusterParam.json** yerel bir klasöre.
6. Bu şablonu dağıtmaya hazırsınız. 

   * Azure PowerShell ile dağıtmak için şablonu içeren klasörde aşağıdaki komutları kullanın:

       ```powershell
       New-AzResourceGroupDeployment -Name OnboardCluster -ResourceGroupName <ResourceGroupName> -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
       ```
       Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, aşağıdakine benzer ve sonucu içeren bir ileti görüntülenir:

       ```powershell
       provisioningState       : Succeeded
       ```

   * Azure CLI ile dağıtmak için aşağıdaki komutları çalıştırın:
    
       ```azurecli
       az login
       az account set --subscription "Subscription Name"
       az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
       ```

       Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, aşağıdakine benzer ve sonucu içeren bir ileti görüntülenir:

       ```azurecli
       provisioningState       : Succeeded
       ```
     İzleme etkinleştirdikten sonra küme için sistem durumu ölçümleri görmeden önce yaklaşık 15 dakika sürebilir. 

## <a name="verify-agent-and-solution-deployment"></a>Aracı ve çözüm dağıtımı doğrulama

Aracı sürümü ile *06072018* veya daha sonra hem aracıyı hem de çözüm başarıyla dağıtıldı doğrulayabilirsiniz. Aracıyı önceki sürümleriyle birlikte, yalnızca aracı dağıtımı doğrulayabilirsiniz.

### <a name="agent-version-06072018-or-later"></a>Aracı 06072018 veya sonraki bir sürümü

Aracı başarıyla dağıtıldığını doğrulamak için aşağıdaki komutu çalıştırın. 

```
kubectl get ds omsagent --namespace=kube-system
```

Doğru şekilde dağıtıldığını gösteren aşağıdaki çıktıya benzemelidir:

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

Çözümün dağıtımı doğrulamak için aşağıdaki komutu çalıştırın:

```
kubectl get deployment omsagent-rs -n=kube-system
```

Doğru şekilde dağıtıldığını gösteren aşağıdaki çıktıya benzemelidir:

```
User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
omsagent   1         1         1            1            3h
```

### <a name="agent-version-earlier-than-06072018"></a>Aracı sürümü 06072018 öncesi

Log Analytics aracı sürümü önce kullanıma sunduğunu doğrulamak için *06072018* düzgün bir şekilde aşağıdaki komutu çalıştırarak dağıtılır:  

```
kubectl get ds omsagent --namespace=kube-system
```

Doğru şekilde dağıtıldığını gösteren aşağıdaki çıktıya benzemelidir:  

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-configuration-with-cli"></a>CLI ile yapılandırmayı görüntüle

Kullanım `aks show` etkin çözüm olduğu gibi ayrıntılarını almak için komut, Log Analytics çalışma alanı ResourceId nedir ve küme hakkında özet ayrıntıları.  

```azurecli
az aks show -g <resourceGroupofAKSCluster> -n <nameofAksCluster>
```

Birkaç dakika sonra komut tamamlanır ve JSON biçimli çözümü hakkında bilgi döndürür.  Komutun sonuçlarını izleme eklenti profilini göstermesi gerekir ve aşağıdaki örnek çıktıya benzer:

```
"addonProfiles": {
    "omsagent": {
      "config": {
        "logAnalyticsWorkspaceResourceID": "/subscriptions/<WorkspaceSubscription>/resourceGroups/<DefaultWorkspaceRG>/providers/Microsoft.OperationalInsights/workspaces/<defaultWorkspaceName>"
      },
      "enabled": true
    }
  }
```

## <a name="next-steps"></a>Sonraki adımlar

* Çözüm ekleme girişimi sırasında sorunlarla karşılaşırsanız, gözden [sorun giderme kılavuzu](container-insights-troubleshoot.md)

* AKS kümesi düğümlerin ve pod'ları için sistem durumu ölçümleri yakalamak için etkinleştirilmiş izleme ile bu sistem durumu ölçümleri Azure portalında kullanılabilir. Kapsayıcılar için Azure İzleyicisi'ni kullanma konusunda bilgi almak için bkz: [görünümü Azure Kubernetes hizmeti sistem durumu](container-insights-analyze.md).
