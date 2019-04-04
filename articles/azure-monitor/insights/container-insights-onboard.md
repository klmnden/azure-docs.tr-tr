---
title: Kapsayıcılar için yerleşik Azure izleme | Microsoft Docs
description: Bu makalede nasıl eklemenize ve kapsayıcınızı nasıl performans gösterdiğini ve performans ile ilgili sorunları tanımlanan anlayabilmeniz kapsayıcılar için Azure İzleyicisi'ni yapılandırabilirsiniz.
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
ms.date: 03/27/2019
ms.author: magoedte
ms.openlocfilehash: f41198761604a16e5d8a983f8bf2200b32c06fb9
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58906030"
---
# <a name="how-to-onboard-azure-monitor-for-containers"></a>Kapsayıcılar için yerleşik Azure izleme  

Bu makalede Kubernetes ortamlara dağıtılmış ve barındırılan iş yüklerinin performansını izlemek için Azure İzleyici'kapsayıcıları için kurma [Azure Kubernetes hizmeti](https://docs.microsoft.com/azure/aks/).

Kapsayıcılar için Azure İzleyici yeni etkinleştirilebilir ya da bir veya daha fazla var olan dağıtımları AKS aşağıdakileri kullanarak desteklenen yöntemleri:

* Azure portalı, Azure PowerShell veya Azure CLI ile
* Kullanarak [Terraform ve AKS](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar 
Başlamadan önce aşağıdakilere sahip olduğunuzdan emin olun:

- **Log Analytics çalışma alanı.** Yeni AKS kümesini izlemeyi etkinleştirin veya AKS kümesi aboneliğin varsayılan kaynak grubunda bir varsayılan çalışma alanı oluşturma ekleme deneyimi sağlar, oluşturabilirsiniz. Kendiniz oluşturmayı seçerseniz, üzerinden oluşturabilirsiniz [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)temellidir [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json), veya [Azure portalında](../../azure-monitor/learn/quick-create-workspace.md).
- Üyesi olduğunuz **Log Analytics katkıda bulunan rolü** kapsayıcı izlemeyi etkinleştirmek için. Log Analytics çalışma alanına erişimi denetleme hakkında daha fazla bilgi için bkz. [çalışma alanlarını yönetme](../../azure-monitor/platform/manage-access.md).
- Üyesi olduğunuz **[sahibi](https://docs.microsoft.com/en-us/azure/role-based-access-control/built-in-role.mds#owner)** AKS kümesi Kaynak rolü. 

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

## <a name="components"></a>Bileşenler 

Yeteneğinizi performansını izlemek için Azure İzleyici kapsayıcılar için özel olarak geliştirilmiş bir Linux kapsayıcı bir Log Analytics aracısını kullanır. Bu özel aracı kümedeki tüm düğümlerden performans ve olay verilerini toplar ve aracıyı otomatik olarak dağıtılır ve dağıtım sırasında belirtilen Log Analytics çalışma alanı ile kayıtlı. Aracı sürümü microsoft, / oms:ciprod04202018 ya da sonraki ve bir tarihi şu biçimde temsil edilir: *mmddyyyy*. 

Aracıyı yeni bir sürümü yayımlandığında, Azure Kubernetes Service (AKS) barındırılan yönetilen Kubernetes kümeleri üzerinde otomatik olarak yükseltilir. Yayımlanan sürümleri takip etmek için bkz: [Aracı sürüm duyuruları](https://github.com/microsoft/docker-provider/tree/ci_feature_prod). 

>[!NOTE] 
>Bir AKS kümesi zaten dağıttıysanız, bu makalenin sonraki bölümlerinde gösterildiği gibi Azure CLI'yı veya sağlanan bir Azure Resource Manager şablonu kullanarak izlemeyi etkinleştirin. Kullanamazsınız `kubectl` yükseltmek için silmek, yeniden dağıtın veya aracıyı dağıtın. Şablon, aynı kaynak grubunda kümesi olarak dağıtılması gerekiyor.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
[Azure Portal](https://portal.azure.com) oturum açın. 

## <a name="enable-monitoring-for-a-new-cluster"></a>Yeni bir küme için izlemeyi etkinleştir
Dağıtım sırasında Terraform veya Azure CLI ile Azure portalında yeni bir AKS kümesi izlemesini etkinleştirebilirsiniz.  Portaldan etkinleştirmek için hızlı başlangıç makalesinde adımları [Azure Kubernetes Service (AKS) kümesini dağıtma](../../aks/kubernetes-walkthrough-portal.md) bölümü altındaki adımları **izleme sistem durumu ve günlükleri**.  

>[!NOTE]
>Aşağıdaki adımları portalında AKS kümesi için izlemeyi etkinleştirmek için hızlı başlangıç makalesinde, mevcut bir Log Analytics çalışma alanını seçin veya yeni bir tane oluşturun istenir. 

### <a name="enable-using-azure-cli"></a>Azure CLI kullanarak etkinleştirin
Azure CLI ile oluşturulan yeni bir AKS kümesi izlemeyi etkinleştirmek için hızlı başlangıç makalesinde bölümünde adım izleyin [oluşturma AKS kümesi](../../aks/kubernetes-walkthrough.md#create-aks-cluster).  

>[!NOTE]
>Azure CLI'yı kullanmayı seçerseniz, ilk CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLI Sürüm 2.0.59 çalıştırıyor olmanız gerekir veya üzeri. Sürümünüzü belirlemek için çalıştırma `az --version`. Gerekirse yükleyin veya Azure CLI'yı yükseltmek için bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). 
>

### <a name="enable-using-terraform"></a>Terraform kullanarak etkinleştirin
Eğer [Terraform kullanan yeni bir AKS kümesi dağıtma](../../terraform/terraform-create-k8s-cluster-with-tf-and-aks.md), profilinde gerekli bağımsız değişkenleri belirtmeniz [Log Analytics çalışma alanı oluşturmak için](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_workspace.html) değil seçerseniz, mevcut bir belirtmek. 

>[!NOTE]
>Terraform kullanmayı seçerseniz, Terraform'u Azure RM sağlayıcısı sürümü 1.17.0 çalıştırmalıdır veya üzeri.

Azure İzleyici kapsayıcılar için çalışma alanına eklemek için bkz [azurerm_log_analytics_solution](https://www.terraform.io/docs/providers/azurerm/r/log_analytics_solution.html) ve dahil ederek profilini tamamlayın [ **addon_profile** ](https://www.terraform.io/docs/providers/azurerm/r/kubernetes_cluster.html#addon_profile) belirtin **oms_agent**. 

İzleme etkin ve tüm yapılandırma görevleri başarıyla tamamlandıktan sonra iki yöntemden biriyle kümenizin performansı izleyebilirsiniz:

* Seçerek doğrudan AKS kümesinde **sistem durumu** sol bölmesinde.
* Seçerek **İzleyici kapsayıcı öngörüleri** AKS kümesi sayfasında seçilen küme için bir kutucuk. Azure İzleyicisi'nde sol bölmede seçin **sistem durumu**. 

  ![AKS, kapsayıcılar için Azure İzleyici seçmek için seçenekleri](./media/container-insights-onboard/kubernetes-select-monitoring-01.png)

İzleme etkinleştirdikten sonra küme için sistem durumu ölçümleri görmeden önce yaklaşık 15 dakika sürebilir. 

## <a name="enable-monitoring-for-existing-managed-clusters"></a>Mevcut yönetilen kümeleri için izlemeyi etkinleştir
Desteklenen yöntemlerden birini kullanarak dağıtılmış bir AKS kümesi izleme etkinleştirebilirsiniz:

* Azure CLI
* Terraform
* [Azure İzleyicisi'nden](#enable-from-azure-monitor-in-the-portal) veya [AKS kümeye doğrudan](#enable-directly-from-aks-cluster-in-the-portal) Azure portalında 
* İle [Azure Resource Manager şablonu ile sağlanan](#enable-using-an-azure-resource-manager-template) Azure PowerShell cmdlet'ini kullanarak `New-AzResourceGroupDeployment` veya Azure CLI ile. 

### <a name="enable-using-azure-cli"></a>Azure CLI kullanarak etkinleştirin
Aşağıdaki adım, Azure CLI kullanılarak AKS kümesini izlenmesini sağlar. Bu örnekte, başına oluşturmanız veya mevcut bir çalışma alanını belirtmeniz gerekmez. Bu komut bir bölgede zaten yoksa, varsayılan çalışma alanına AKS kümesi aboneliğin varsayılan kaynak grubu oluşturarak bu işlemi sizin için basitleştirir.  Oluşturulan varsayılan çalışma biçimi benzer *DefaultWorkspace -\<GUID >-\<bölge >*.  

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

### <a name="enable-using-terraform"></a>Terraform kullanarak etkinleştirin
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

### <a name="enable-from-azure-monitor-in-the-portal"></a>Portalda Azure İzleyicisi'nden etkinleştirin 
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

### <a name="enable-directly-from-aks-cluster-in-the-portal"></a>AKS kümesi portalında doğrudan etkinleştir
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

### <a name="enable-using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanarak etkinleştirin
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
