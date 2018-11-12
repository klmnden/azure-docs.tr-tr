---
title: Kapsayıcılar (Önizleme) için yerleşik Azure izleme | Microsoft Docs
description: Bu makalede nasıl eklemenize ve kapsayıcınızı nasıl performans gösterdiğini ve performans ile ilgili sorunları tanımlanan anlayabilmeniz kapsayıcılar için Azure İzleyicisi'ni yapılandırabilirsiniz.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/05/2018
ms.author: magoedte
ms.openlocfilehash: 2b7045f74a22732337ceb8dc9136da1c93ee7c2c
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2018
ms.locfileid: "51037796"
---
# <a name="how-to-onboard-azure-monitor-for-containers-preview"></a>Kapsayıcılar (Önizleme) için yerleşik Azure izleme 
Bu makalede Kubernetes ortamlara dağıtılmış ve barındırılan iş yüklerinin performansını izlemek için Azure İzleyici'kapsayıcıları için kurma [Azure Kubernetes hizmeti](https://docs.microsoft.com/azure/aks/).

## <a name="prerequisites"></a>Önkoşullar 
Başlamadan önce aşağıdakilere sahip olduğunuzdan emin olun:

- Bir yeni veya mevcut AKS kümesi.
- Linux sürümü için kapsayıcı bir Log Analytics aracısını microsoft / oms:ciprod04202018 veya üzeri. Sürüm numarası, aşağıdaki biçimde bir tarih tarafından temsil edilen: *mmddyyyy*. Aracı, bu özelliğin ekleme sırasında otomatik olarak yüklenir. 
- Log Analytics çalışma alanı. Yeni AKS kümesini izlemeyi etkinleştirin veya AKS kümesi aboneliğin varsayılan kaynak grubunda bir varsayılan çalışma alanı oluşturma ekleme deneyimi sağlar, oluşturabilirsiniz. Kendiniz oluşturmayı seçerseniz, üzerinden oluşturabilirsiniz [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md)temellidir [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), veya [Azure portalında](../log-analytics/log-analytics-quick-create-workspace.md).
- Kapsayıcı izlemeyi etkinleştirmek için Log Analytics katkıda bulunan rolü. Log Analytics çalışma alanına erişimi denetleme hakkında daha fazla bilgi için bkz. [çalışma alanlarını yönetme](../log-analytics/log-analytics-manage-access.md).

[!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

## <a name="components"></a>Bileşenler 

Performans izleme olanağı, kümedeki tüm düğümlerin performans ve olay verilerini toplar ve Linux için kapsayıcı bir Log Analytics aracısını kullanır. Aracı otomatik olarak dağıtılan ve kapsayıcı izleme etkinleştirdikten sonra belirtilen Log Analytics çalışma alanı ile kayıtlı. 

>[!NOTE] 
>Bir AKS kümesi zaten dağıttıysanız, bu makalenin sonraki bölümlerinde gösterildiği gibi Azure CLI'yı veya sağlanan bir Azure Resource Manager şablonu kullanarak izlemeyi etkinleştirin. Kullanamazsınız `kubectl` yükseltmek için silmek, yeniden dağıtın veya aracıyı dağıtın. Şablon kümeyle aynı kaynak grubunda dağıtılması gerekiyor."

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
[Azure Portal](https://portal.azure.com) oturum açın. 

## <a name="enable-monitoring-for-a-new-cluster"></a>Yeni bir küme için izlemeyi etkinleştir
Dağıtım sırasında Azure portalında veya Azure CLI ile yeni bir AKS kümesi izlemeyi etkinleştirebilirsiniz. Hızlı Başlangıç makalesinde adımları [Azure Kubernetes Service (AKS) kümesini dağıtma](../aks/kubernetes-walkthrough-portal.md) portaldan etkinleştirmek istiyorsanız. Üzerinde **izleme** sayfası için **izlemeyi etkinleştirme** seçeneği için **Evet**ve ardından mevcut bir Log Analytics çalışma alanını seçin veya yeni bir tane oluşturun. 

Azure CLI ile oluşturulan yeni bir AKS kümesi izlemeyi etkinleştirmek için hızlı başlangıç makalesinde bölümünde adım izleyin [oluşturma AKS kümesi](../aks/kubernetes-walkthrough.md#create-aks-cluster).  

>[!NOTE]
>Azure CLI'yı kullanmayı seçerseniz, ilk CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLI Sürüm 2.0.43 çalıştırıyor olmanız gerekir veya üzeri. Sürümünüzü belirlemek için çalıştırma `az --version`. Gerekirse yükleyin veya Azure CLI'yı yükseltmek için bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). 
>

İzleme etkin ve tüm yapılandırma görevleri başarıyla tamamlandıktan sonra iki yöntemden biriyle kümenizin performansı izleyebilirsiniz:

* Seçerek doğrudan AKS kümesinde **sistem durumu** sol bölmesinde.
* Seçerek **İzleyici kapsayıcı öngörüleri** AKS kümesi sayfasında seçilen küme için bir kutucuk. Azure İzleyicisi'nde sol bölmede seçin **sistem durumu**. 

  ![AKS, kapsayıcılar için Azure İzleyici seçmek için seçenekleri](./media/monitoring-container-insights-onboard/kubernetes-select-monitoring-01.png)

İzleme etkinleştirdikten sonra küme için sistem durumu ölçümleri görmeden önce yaklaşık 15 dakika sürebilir. 

## <a name="enable-monitoring-for-existing-managed-clusters"></a>Mevcut yönetilen kümeleri için izlemeyi etkinleştir
AKS kümesi ya da Azure CLI'yı portalından veya sağlanan Azure Resource Manager şablonu ile PowerShell cmdlet'ini kullanarak dağıtılmış izleme etkinleştirebilirsiniz `New-AzureRmResourceGroupDeployment`. 

### <a name="enable-monitoring-using-azure-cli"></a>Azure CLI kullanarak izlemeyi etkinleştirin
Aşağıdaki adım, Azure CLI kullanılarak AKS kümesini izlenmesini sağlar. Bu örnekte, başına oluşturmanız veya mevcut bir çalışma alanını belirtmeniz gerekmez. Bu komut bir bölgede zaten yoksa, varsayılan çalışma alanına AKS kümesi aboneliğin varsayılan kaynak grubu oluşturarak bu işlemi sizin için basitleştirir.  Oluşturulan varsayılan çalışma biçimi benzer *DefaultWorkspace -<GUID>-<Region>*.  

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

### <a name="enable-monitoring-from-azure-monitor"></a>Azure İzleyici'den izlemeyi etkinleştir
AKS kümenizin Azure portalında Azure İzleyicisi'nden izlemeyi etkinleştirmek için aşağıdakileri yapın:

1. Azure portalında **İzleyici**. 
2. Seçin **kapsayıcılar (Önizleme)** listeden.
3. Üzerinde **İzleyicisi - kapsayıcılar (Önizleme)** sayfasında **olmayan izlenen kümeleri**.
4. İzlenen kümelerinin listesini, kapsayıcı listede bulun ve tıklayın **etkinleştirme**.   
5. Üzerinde **kapsayıcı durumunun ve günlükleri ekleme** sayfasında, mevcut bir Log Analytics varsa küme ile aynı abonelikte çalışma alanı, aşağı açılan listeden seçin.  
    Listenin varsayılan çalışma alanını ve AKS kapsayıcı abonelikte dağıtılmış konumunu belirler. 

    ![AKS kapsayıcı ınsights izlemeyi etkinleştir](./media/monitoring-container-insights-onboard/kubernetes-onboard-brownfield-01.png)

    >[!NOTE]
    >Küme izleme verilerini depolamak için yeni bir Log Analytics çalışma alanı oluşturmak istiyorsanız,'ndaki yönergeleri izleyin [Log Analytics çalışma alanı oluşturma](../log-analytics/log-analytics-quick-create-workspace.md). AKS kapsayıcı dağıtılan aynı abonelikte çalışma alanı oluşturmak emin olun. 
 
İzleme etkinleştirdikten sonra küme için sistem durumu ölçümleri görmeden önce yaklaşık 15 dakika sürebilir. 

### <a name="enable-monitoring-from-aks-cluster-in-the-portal"></a>AKS kümesi portalında izlemeyi etkinleştir
Azure portalında AKS kapsayıcınızı izlemeyi etkinleştirmek için aşağıdakileri yapın:

1. Azure portalda **Tüm hizmetler**’i seçin. 
2. Kaynak listesinde yazmaya başlayın **kapsayıcıları**.  
    Girişinize bağlı listesini filtreler. 
3. Seçin **Kubernetes Hizmetleri**.  

    ![Kubernetes Hizmetleri Bağla](./media/monitoring-container-insights-onboard/portal-search-containers-01.png)

4. Kapsayıcılar listesinde, bir kapsayıcıyı seçin.
5. Kapsayıcı genel bakış sayfasında **kapsayıcı sistem durumu izleme**.  
6. Üzerinde **kapsayıcı durumunun ve günlükleri ekleme** sayfasında, mevcut bir Log Analytics varsa küme ile aynı abonelikte çalışma alanı seçin, aşağı açılan listesinde.  
    Listenin varsayılan çalışma alanını ve AKS kapsayıcı abonelikte dağıtılmış konumunu belirler. 

    ![AKS kapsayıcı sistem durumu izlemeyi etkinleştir](./media/monitoring-container-insights-onboard/kubernetes-onboard-brownfield-02.png)

    >[!NOTE]
    >Küme izleme verilerini depolamak için yeni bir Log Analytics çalışma alanı oluşturmak istiyorsanız,'ndaki yönergeleri izleyin [Log Analytics çalışma alanı oluşturma](../log-analytics/log-analytics-quick-create-workspace.md). AKS kapsayıcı dağıtılan aynı abonelikte çalışma alanı oluşturmak emin olun. 
 
İzleme etkinleştirdikten sonra küme için işletimsel veri görmeden önce yaklaşık 15 dakika sürebilir. 

### <a name="enable-monitoring-by-using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanarak izlemeyi etkinleştir
Bu yöntem, iki JSON şablonları içerir. Yapılandırmayı, izlemeyi etkinleştirmek için bir şablon belirtir ve diğer aşağıdaki belirtmek için yapılandırdığınız parametre değerlerini içerir:

* AKS kapsayıcı kaynak kimliği. 
* Kümenin dağıtıldığı kaynak grubu.
* Log Analytics çalışma alanı ve çalışma alanında oluşturmak için bölge. 

>[!NOTE]
>Şablon, aynı kaynak grubunda kümesi olarak dağıtılması gerekiyor.
>

Log Analytics çalışma alanını el ile oluşturulması gerekir. Çalışma alanı oluşturmak için bunu aracılığıyla ayarlayabilirsiniz [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md)temellidir [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), veya [Azure portalında](../log-analytics/log-analytics-quick-create-workspace.md).

Bir şablon kullanarak kaynakları dağıtma kavramıyla alışkın değilseniz, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md)
* [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../azure-resource-manager/resource-group-template-deploy-cli.md)

Azure CLI'yı kullanmayı seçerseniz, ilk CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLI Sürüm 2.0.27 çalıştırıyor olmanız gerekir veya üzeri. Sürümünüzü belirlemek için çalıştırma `az --version`. Gerekirse yükleyin veya Azure CLI'yı yükseltmek için bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). 

#### <a name="create-and-execute-a-template"></a>Oluşturma ve bir şablonu yürütme

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
    },
    "workspaceRegion": {
    "type": "string",
    "metadata": {
       "description": "Azure Monitor Log Analytics workspace region"
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
     },
    {
        "type": "Microsoft.Resources/deployments",
        "name": "[Concat('ContainerInsights', '-',  uniqueString(parameters('workspaceResourceId')))]", 
        "apiVersion": "2017-05-10",
        "subscriptionId": "[split(parameters('workspaceResourceId'),'/')[2]]",
        "resourceGroup": "[split(parameters('workspaceResourceId'),'/')[4]]",
        "properties": {
            "mode": "Incremental",
            "template": {
                "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                "contentVersion": "1.0.0.0",
                "parameters": {},
                "variables": {},
                "resources": [
                    {
                        "apiVersion": "2015-11-01-preview",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "location": "[parameters('workspaceRegion')]",
                        "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                        "properties": {
                            "workspaceResourceId": "[parameters('workspaceResourceId')]"
                        },
                        "plan": {
                            "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
                            "product": "[Concat('OMSGallery/', 'ContainerInsights')]",
                            "promotionCode": "",
                            "publisher": "Microsoft"
                        }
                    }
                ]
            },
            "parameters": {}
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
           "value": "/subscriptions/<SubscriptiopnId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
       },
       "aksResourceLocation": {
         "value": "<aksClusterLocation>"
       },
       "workspaceResourceId": {
         "value": "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroup>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>"
       },
       "workspaceRegion": {
         "value": "<workspaceLocation>"
       }
     }
    }
    ```

4. Değerlerini düzenleyin **aksResourceId** ve **aksResourceLocation** değerleri kullanarak **AKS'ye genel bakış** AKS kümesi sayfası. Değeri **workspaceResourceId** çalışma alanı adı içerir, Log Analytics çalışma alanının tam kaynak kimliği. Ayrıca çalışma alanı için konumu belirtin **workspaceRegion**. 
5. Bu dosyayı farklı Kaydet **existingClusterParam.json** yerel bir klasöre.
6. Bu şablonu dağıtmaya hazırsınız. 

    * Şablonu içeren klasörde aşağıdaki PowerShell komutlarını kullanın:

        ```powershell
        New-AzureRmResourceGroupDeployment -Name OnboardCluster -ClusterResourceGroupName ClusterResourceGroupName -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
        ```
        Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, aşağıdakine benzer ve sonucu içeren bir ileti görüntülenir:

        ```powershell
        provisioningState       : Succeeded
        ```

    * Azure CLI kullanarak aşağıdaki komutu çalıştırmak için:
    
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
az aks show -g <resoourceGroupofAKSCluster> -n <nameofAksCluster>
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

* Çözüm ekleme girişimi sırasında sorunlarla karşılaşırsanız, gözden [sorun giderme kılavuzu](monitoring-container-insights-troubleshoot.md)

* AKS kümesi düğümlerin ve pod'ları için sistem durumu ölçümleri yakalamak için etkinleştirilmiş izleme ile bu sistem durumu ölçümleri Azure portalında kullanılabilir. Kapsayıcılar için Azure İzleyicisi'ni kullanma konusunda bilgi almak için bkz: [görünümü Azure Kubernetes hizmeti sistem durumu](monitoring-container-insights-analyze.md).