---
title: İzleme Azure Kubernetes Service (AKS) sistem durumu (Önizleme) | Microsoft Docs
description: Bu makalede, barındırılan Kubernetes ortamınızı kullanımını hızla anlamanız için AKS kapsayıcı performansı nasıl kolayca inceleyebilirsiniz açıklanır.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/15/2018
ms.author: magoedte
ms.openlocfilehash: 8027149f3e5ace163bf380bc5362fcb101397986
ms.sourcegitcommit: 744747d828e1ab937b0d6df358127fcf6965f8c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42059246"
---
# <a name="monitor-azure-kubernetes-service-aks-container-health-preview"></a>Azure Kubernetes Service (AKS) kapsayıcı durumu (Önizleme) izleme

Bu makalede, ayarlama ve Kubernetes ortamlara dağıtılır ve Azure Kubernetes Service (AKS) üzerinde barındırılan iş yüklerinin performansını izlemek için Azure İzleyici kapsayıcısı durumuna kullanma açıklar. Özellikle birden çok uygulama ile ölçekli olarak bir üretim kümesi çalıştırırken Kubernetes kümenizin ve kapsayıcılarınızın izlenmesi kritik önem taşır.

Kapsayıcı durumunun performans toplama bellek ve işlemci ölçümleri denetleyicileri, düğümleri ve Kubernetes ölçümler API aracılığıyla kullanılabilir olan kapsayıcıları yeteneği sağlar. Kapsayıcı durumunun etkinleştirdikten sonra Bu ölçümler otomatik olarak sizin için Linux için Log Analytics aracısını kapsayıcı bir sürümü aracılığıyla toplanan ve depolanan, [Log Analytics](../log-analytics/log-analytics-overview.md) çalışma. Dahil edilen önceden tanımlanmış görünümleri kaynaklarınızda bulunan kapsayıcı iş yüklerinin ve destesinin ne Kubernetes kümesi performans durumunun etkiler görüntüle:  

* Düğüm ve bunların ortalama işlemci ve bellek kullanımı çalışan kapsayıcılar belirleyin. Bu bilgi, kaynak darboğazları belirlemenize yardımcı olabilir.
* Kapsayıcı bir denetleyici veya bir pod içinde bulunduğu belirleyin. Bu bilgi, denetleyicinin ya da pod'ın genel performansını görüntülemenize yardımcı olabilir. 
* Pod destekleyen standart işlemlere ilgisiz, ana bilgisayarda çalışan iş yüklerini kaynak kullanımını gözden geçirin.
* Ortalama ve en yoğun iş yükü altında kümeye davranışını anlayın. Bu bilgi, kapasite gereksinimlerini tanımlama ve kümenin karşılayabileceği en fazla yükü belirlemek yardımcı olabilir. 

Docker ve Windows yönetme ve izleme de ilgileniyorsanız kapsayıcı konakları görünümü yapılandırma, denetleme ve kaynak kullanımını görmek [kapsayıcı izleme çözümü](../log-analytics/log-analytics-containers.md).

## <a name="prerequisites"></a>Önkoşullar 
Başlamadan önce aşağıdakilere sahip olduğunuzdan emin olun:

- Bir yeni veya mevcut AKS kümesi.
- Linux sürümü için kapsayıcı bir Log Analytics aracısını microsoft / oms:ciprod04202018 veya üzeri. Sürüm numarası, aşağıdaki biçimde bir tarih tarafından temsil edilen: *mmddyyyy*. Aracı, kapsayıcı durumu ekleme sırasında otomatik olarak yüklenir. 
- Log Analytics çalışma alanı. Yeni AKS kümesini izlemeyi etkinleştirin veya AKS kümesi aboneliğin varsayılan kaynak grubunda bir varsayılan çalışma alanı oluşturma ekleme deneyimi sağlar, oluşturabilirsiniz. Kendiniz oluşturmayı seçerseniz, üzerinden oluşturabilirsiniz [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md)temellidir [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), veya [Azure portalında](../log-analytics/log-analytics-quick-create-workspace.md).
- Kapsayıcı izlemeyi etkinleştirmek için Log Analytics katkıda bulunan rolü. Log Analytics çalışma alanına erişimi denetleme hakkında daha fazla bilgi için bkz. [çalışma alanlarını yönetme](../log-analytics/log-analytics-manage-access.md).

[!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

## <a name="components"></a>Bileşenler 

Performans izleme olanağı, kümedeki tüm düğümlerin performans ve olay verilerini toplar ve Linux için kapsayıcı bir Log Analytics aracısını kullanır. Aracı otomatik olarak dağıtılan ve kapsayıcı izleme etkinleştirdikten sonra belirtilen Log Analytics çalışma alanı ile kayıtlı. 

>[!NOTE] 
>Bir AKS kümesi zaten dağıttıysanız, bu makalenin sonraki bölümlerinde gösterildiği gibi Azure CLI'yı veya sağlanan bir Azure Resource Manager şablonu kullanarak izlemeyi etkinleştirin. Kullanamazsınız `kubectl` yükseltmek için silmek, yeniden dağıtın veya aracıyı dağıtın. 
>

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın
[Azure Portal](https://portal.azure.com) oturum açın. 

## <a name="enable-container-health-monitoring-for-a-new-cluster"></a>Kapsayıcı, yeni bir küme için sistem durumu izlemeyi etkinleştir
Dağıtım sırasında Azure portalında veya Azure CLI ile yeni bir AKS kümesi izlemeyi etkinleştirebilirsiniz. Hızlı Başlangıç makalesinde adımları [Azure Kubernetes Service (AKS) kümesini dağıtma](../aks/kubernetes-walkthrough-portal.md) portaldan etkinleştirmek istiyorsanız. Üzerinde **izleme** sayfası için **izlemeyi etkinleştirme** seçeneği için **Evet**ve ardından mevcut bir Log Analytics çalışma alanını seçin veya yeni bir tane oluşturun. 

Azure CLI ile oluşturulan yeni bir AKS kümesi izlemeyi etkinleştirmek için hızlı başlangıç makalesinde bölümünde adım izleyin [oluşturma AKS kümesi](../aks/kubernetes-walkthrough.md#create-aks-cluster).  

>[!NOTE]
>Azure CLI'yı kullanmayı seçerseniz, ilk CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLI Sürüm 2.0.43 çalıştırıyor olmanız gerekir veya üzeri. Sürümünüzü belirlemek için çalıştırma `az --version`. Gerekirse yükleyin veya Azure CLI'yı yükseltmek için bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). 
>

İzleme etkin ve tüm yapılandırma görevleri başarıyla tamamlandıktan sonra iki yöntemden biriyle kümenizin performansı izleyebilirsiniz:

* Seçerek doğrudan AKS kümesinde **sistem durumu** sol bölmesinde.
* Seçerek **kapsayıcı sistem durumu izleme** AKS kümesi sayfasında seçilen küme için bir kutucuk. Azure İzleyicisi'nde sol bölmede seçin **sistem durumu**. 

  ![AKS kapsayıcı durumunun seçmek için seçenekleri](./media/monitoring-container-health/container-performance-and-health-select-01.png)

İzleme etkinleştirdikten sonra küme için işletimsel veri görmeden önce yaklaşık 15 dakika sürebilir. 

## <a name="enable-container-health-monitoring-for-existing-managed-clusters"></a>Kapsayıcı mevcut yönetilen kümeleri için sistem durumu izlemeyi etkinleştir
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

### <a name="enable-monitoring-in-the-azure-portal"></a>Azure portalında izlemeyi etkinleştir
Azure portalında AKS kapsayıcınızı izlemeyi etkinleştirmek için aşağıdakileri yapın:

1. Azure portalında **tüm hizmetleri**. 
2. Kaynak listesinde yazmaya başlayın **kapsayıcıları**.  
    Girişinize bağlı listesini filtreler. 
3. Seçin **Kubernetes Hizmetleri**.  

    ![Kubernetes Hizmetleri Bağla](./media/monitoring-container-health/azure-portal-01.png)

4. Kapsayıcılar listesinde, bir kapsayıcıyı seçin.
5. Kapsayıcı genel bakış sayfasında **kapsayıcı sistem durumu izleme**.  
6. Üzerinde **kapsayıcı durumunun ve günlükleri ekleme** sayfasında, mevcut bir Log Analytics varsa küme ile aynı abonelikte çalışma alanı seçin, aşağı açılan listesinde.  
    Listenin varsayılan çalışma alanını ve AKS kapsayıcı abonelikte dağıtılmış konumunu belirler. 

    ![AKS kapsayıcı sistem durumu izlemeyi etkinleştir](./media/monitoring-container-health/container-health-enable-brownfield-02.png)

>[!NOTE]
>Küme izleme verilerini depolamak için yeni bir Log Analytics çalışma alanı oluşturmak istiyorsanız,'ndaki yönergeleri izleyin [Log Analytics çalışma alanı oluşturma](../log-analytics/log-analytics-quick-create-workspace.md). AKS kapsayıcı dağıtılan aynı abonelikte çalışma alanı oluşturmak emin olun. 
 
İzleme etkinleştirdikten sonra küme için işletimsel veri görmeden önce yaklaşık 15 dakika sürebilir. 

### <a name="enable-monitoring-by-using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanarak izlemeyi etkinleştir
Bu yöntem, iki JSON şablonları içerir. Yapılandırmayı, izlemeyi etkinleştirmek için bir şablon belirtir ve diğer aşağıdaki belirtmek için yapılandırdığınız parametre değerlerini içerir:

* AKS kapsayıcı kaynak kimliği. 
* Kümenin dağıtıldığı kaynak grubu.
* Log Analytics çalışma alanı ve çalışma alanında oluşturmak için bölge. 

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
        "name": "[Concat('ContainerInsights', '(', split(parameters('workspaceResourceId'),'/')[8], ')')]",
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
       "$schema": "https://schema.management.azure.com/  schemas/2015-01-01/deploymentParameters.json#",
       "contentVersion": "1.0.0.0",
       "parameters": {
         "aksResourceId": {
           "value": "/subscriptions/<SubscroptiopnId>/resourcegroups/<ResourceGroup>/providers/Microsoft.ContainerService/managedClusters/<ResourceName>"
       },
       "aksResourceLocation": {
         "value": "East US"
       },
       "workspaceResourceId": {
         "value": "/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroup>/providers/Microsoft.OperationalInsights/workspaces/<workspaceName>"
       },
       "workspaceRegion": {
         "value": "eastus"
       }
     }
    }
    ```

4. Değerlerini düzenleyin **aksResourceId** ve **aksResourceLocation** değerleri kullanarak **AKS'ye genel bakış** AKS kümesi sayfası. Değeri **workspaceResourceId** çalışma alanı adı içerir, Log Analytics çalışma alanının tam kaynak kimliği. Ayrıca çalışma alanı için konumu belirtin **workspaceRegion**. 
5. Bu dosyayı farklı Kaydet **existingClusterParam.json** yerel bir klasöre.
6. Bu şablonu dağıtmaya hazırsınız. 

    * Şablonu içeren klasörde aşağıdaki PowerShell komutlarını kullanın:

        ```powershell
        New-AzureRmResourceGroupDeployment -Name OnboardCluster -ResourceGroupName ClusterResourceGroupName -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
        ```
        Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, aşağıdakine benzer ve sonucu içeren bir ileti görüntülenir:

        ```powershell
        provisioningState       : Succeeded
        ```

    * Linux üzerinde Azure CLI kullanarak aşağıdaki komutu çalıştırmak için:
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
        ```

        Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, aşağıdakine benzer ve sonucu içeren bir ileti görüntülenir:

        ```azurecli
        provisioningState       : Succeeded
        ```
İzleme etkinleştirdikten sonra küme için işletimsel veri görmeden önce yaklaşık 15 dakika sürebilir. 

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

## <a name="view-performance-utilization"></a>Görünüm performans kullanımı
Kapsayıcı durumunun açtığınızda, sayfanın hemen, tüm küme performans kullanımını gösterir. AKS kümenizi hakkında bilgi görüntüleme, dört Perspektifler düzenlenmiştir:

- Küme
- Düğümler 
- Denetleyiciler  
- Kapsayıcılar

Üzerinde **küme** sekmesinde, dört satırı performans grafiklerini kümenizin temel performans ölçümlerini görüntüleyin. 

![Örnek performans grafiklerini küme sekmesinde](./media/monitoring-container-health/container-health-cluster-perfview.png)

Performans grafiği dört performans ölçümlerini görüntüler:

- **Düğüm CPU kullanımını&nbsp;%**: toplu bir perspektif, tüm küme için CPU kullanımı. Zaman aralığı için sonuç seçerek filtreleyebilirsiniz **ortalama**, **Min**, **Max**, **50**, **90'ıncı**, ve **95** yüzdebirliklerini Seçici grafiğin içinde ya da tek tek veya birlikte. 
- **Düğüm bellek kullanımını&nbsp;%**: toplu bir perspektif, tüm küme için bellek kullanımı. Zaman aralığı için sonuç seçerek filtreleyebilirsiniz **ortalama**, **Min**, **Max**, **50**, **90'ıncı**, ve **95** yüzdebirliklerini Seçici grafiğin içinde ya da tek tek veya birlikte. 
- **Düğüm sayısı**: düğüm sayısı ve Kubernetes durumu. Durumlar temsil küme düğümlerinin *tüm*, *hazır*, ve *hazır değil* ve ayrı olarak filtrelenen veya grafiğin üstünde Seçici içinde birleştirilmiş. 
- **Etkinlik pod sayısının**: bir pod sayısı ve Kubernetes durumu. Temsil edilen pod'ları, durumlar *tüm*, *bekleyen*, *çalıştıran*, ve *bilinmeyen* ve ayrı olarak filtrelenen veya içinde birleşik grafiğin üstünde Seçici. 

İçin değiştirdiğinizde **düğümleri**, **denetleyicileri**, ve **kapsayıcıları** sekmesinde, otomatik olarak sağ tarafında sayfasının görüntülenmesini olan özellik bölmesi.  Bu, Kubernetes nesneleri düzenlemek için tanımladığınız etiketleri dahil olmak üzere seçili öğenin özelliklerini gösterir.  Tıklayarak **>>** bölmesinde bölmesinde view\hide için bağlantı.  

![Örnek Kubernetes Perspektifler Özellikler bölmesi](./media/monitoring-container-health/perspectives-preview-pane-01.png)

Özellikler bölmesinde güncelleştirmeleri hiyerarşideki nesneler genişlettikçe tabanlı seçili nesne üzerinde. Bölmesinden da önceden tanımlanmış günlük aramalarının Kubernetes olaylarıyla tıklayarak görüntüleyebilirsiniz **görünümü Kubernetes olay günlüklerini** bölmenin üstündeki bağlantısı. Kubernetes günlük verilerini görüntüleme hakkında ek bilgi için bkz: [arama verileri çözümlemek için günlükleri](#search-logs-to-analyze-data).

Geçiş **düğümleri** sekmesi ve satır hiyerarşi izler Kubernetes nesne modeli, kümenizdeki bir düğümü başlatma. Düğümünü genişletin ve düğüm üzerinde çalışan bir veya daha fazla pod'ları görüntüleyebilirsiniz. Birden fazla kapsayıcı için bir pod gruplandırılmışsa, hiyerarşideki son satırı olarak görüntülenir. Ayrıca, ana bilgisayar işlemci veya bellek baskısı varsa kaç pod harici ilgili iş konakta çalışan görüntüleyebilirsiniz.

![Performans Görünümü'nde örnek Kubernetes düğüm hiyerarşisi](./media/monitoring-container-health/container-health-nodes-view.png)

Denetleyicileri veya sayfanın üstündeki kapsayıcılar'ı seçin ve bu nesneler için durumu ve kaynak kullanımını gözden geçirin. Açılan kutu üstündeki ad alanı, hizmet ve düğüm göre filtrelemek için kullanın. Bunun yerine, bellek kullanımı, gözden geçirmek istiyorsanız **ölçüm** aşağı açılan listesinden **bellek RSS** veya **bellek çalışma kümesi**. **Bellek RSS** yalnızca Kubernetes sürüm 1.8 ve üzeri desteklenir. Aksi takdirde, değerlerini görüntüleyin **Min&nbsp; %**  olarak *NaN&nbsp;%*, tanımlanmamış bir temsil eden sayısal veri türü değeri olan veya çıkarıldığında değeri. 

![Kapsayıcı düğümlerini performans görünümü](./media/monitoring-container-health/container-health-node-metric-dropdown.png)

Varsayılan olarak, son altı saat üzerinde performans verilerini bağlıdır, ancak penceresini kullanarak değiştirebileceğiniz **zaman aralığı** sağ üst köşedeki aşağı açılan listesi. Şu anda sayfa değil otomatik yenileme, bu nedenle el ile yenilemeniz gerekir. Seçerek zaman aralığı içinde sonuçlarını filtreleyebilirsiniz **ortalama**, **Min**, **Max**, **50**, **90'ıncı**, ve **95** yüzdebirlik Seçici içinde. 

![Verileri filtreleme için yüzdebirlik seçimi](./media/monitoring-container-health/container-health-metric-percentile-filter.png)

Aşağıdaki örnekte, düğümü Not *aks nodepool 3977305*, değeri **kapsayıcıları** görüntülemenizden bu yana dağıtılan kapsayıcıların toplam sayısı 5 olacaktır.

![Kapsayıcı düğümü örnek başına dökümü](./media/monitoring-container-health/container-health-nodes-containerstotal.png)

Kapsayıcı düğümleri arasında uygun bir denge olup olmadığını hızlıca belirlemenize yardımcı olabilir. 

Düğümleri görüntülediğinizde sunulan bilgiler aşağıdaki tabloda açıklanmıştır:

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Ana bilgisayar adı. |
| Durum | Düğüm durumu görünümünü Kubernetes. |
| Ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;% | Seçili süre ve çözüm oranlarına dayanarak ortalama düğüm yüzdesi. |
| Ortalama, Min, maks., 50, 90 | Seçili, saati süresi sırasında ve çözüm oranlarına dayanarak ortalama düğümleri gerçek değeri. Ortalama değer, bir düğüm için CPU/bellek sınırını zamandan ölçülür; pod'ların ve kapsayıcılar için ana bilgisayar tarafından bildirilen ortalama değeri var. |
| Kapsayıcılar | Kapsayıcı sayısı. |
| Çalışma Süresi | Bir düğüm başlatıldığında veya yeniden başlatıldıktan sonra süresini temsil eder. |
| Denetleyiciler | Yalnızca kapsayıcılar ve pod'ları için. Bu, içinde bulunan hangi denetleyicisinin gösterir. Tüm pod'ların bazı görüntülenebilir bir denetleyici olduğundan **yok**. | 
| Eğilim ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;% | Çubuk grafik eğilim sunma denetleyicisini yüzdebirlik ölçüm yüzdesi. |


Seçicide seçin **denetleyicileri**.

![Select denetleyicileri görüntüle](./media/monitoring-container-health/container-health-controllers-tab.png)

Burada denetleyicilerinizi performans durumunu görüntüleyebilirsiniz.

![< ad > denetleyicileri performans görünümü](./media/monitoring-container-health/container-health-controllers-view.png)

Satır hiyerarşi denetleyicisi ile başlar ve denetleyici genişletir. Bir veya daha fazla kapsayıcıları görebilirsiniz. Pod'u genişletin, bir pod ve kapsayıcı gruplandırılmış son satır görüntüler. 

Denetleyicileri görüntülediğinizde görüntülenen bilgileri aşağıdaki tabloda açıklanmıştır:

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Denetleyicinin adı.|
| Durum | Toplama durumunun durumu ile çalıştırılması tamamlandıktan sonra kapsayıcıların *Tamam*, *kesildi*, *başarısız* *durduruldu*, veya *Duraklatıldı*. Kapsayıcıyı çalıştıran, ancak durumu ya da düzgün şekilde görüntülenmesini veya aracı tarafından toplanmış değil ve 30 dakikadan fazla yanıt vermedi: durumudur *bilinmeyen*. Ek ayrıntılar durumu simgesinin aşağıdaki tabloda verilmiştir.|
| Ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;% | Her varlık için yüzdebirlik ve seçili ölçümün ortalama yüzdesini ortalama dökümü. |
| Ortalama, Min, maks., 50, 90  | Döküm kapsayıcı seçilen yüzdelik dilim için ortalama CPU millicore veya bellek performansını. Ortalama değer, bir pod için CPU/bellek sınırını zamandan ölçülür. |
| Kapsayıcılar | Denetleyici veya pod için kapsayıcı toplam sayısı. |
| Yeniden başlatma | Döküm kapsayıcılardan yeniden sayısı. |
| Çalışma Süresi | Kapsayıcı başladıktan sonra süresini temsil eder. |
| Node | Yalnızca kapsayıcılar ve pod'ları için. Bu, bulunan hangi denetleyicisinin gösterir. | 
| Eğilim ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;%| Denetleyicinin yüzdebirlik ölçüm temsil eden çubuk grafik eğilim. |

Durum alanı simgeleri kapsayıcıları çevrimiçi durumunu gösterir:
 
| Simge | Durum | 
|--------|-------------|
| ![Hazır çalışan durum simgesi](./media/monitoring-container-health/container-health-ready-icon.png) | (Hazır) çalıştıran|
| ![Bekleyen veya duraklatıldı durum simgesi](./media/monitoring-container-health/container-health-waiting-icon.png) | Bekleyen veya duraklatıldı|
| ![Son durum simgesi çalıştıran bildirdi.](./media/monitoring-container-health/container-health-grey-icon.png) | Son çalıştırma bildirdi ancak 30 dakikadan fazla yanıt henüz|
| ![Başarılı durum simgesi](./media/monitoring-container-health/container-health-green-icon.png) | Başarılı bir şekilde durdurulmuş veya yanıt vermemesine başarısız|

Durum simgesi ne pod sağlar göre sayısını görüntüler. İki en kötü durum gösterir ve durum geldiğinizde, kapsayıcıda tüm pod'ların bir döküm durumunu görüntüler. Hazır durumda değilse, durum değeri görüntüler **(0)**. 

Seçicide seçin **kapsayıcıları**.

![Select kapsayıcıları görüntüleyin](./media/monitoring-container-health/container-health-containers-tab.png)

Burada kapsayıcılarınızı performans durumunu görüntüleyebilirsiniz.

![< ad > denetleyicileri performans görünümü](./media/monitoring-container-health/container-health-containers-view.png)

Kapsayıcıları görüntülediğinizde görüntülenen bilgileri aşağıdaki tabloda açıklanmıştır:

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Denetleyicinin adı.|
| Durum | Kapsayıcılar, varsa durumu. Durum simgesi, ek ayrıntılar sonraki tabloda verilmiştir.|
| Ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;% | Görüntülemenizden bu yana her varlık için yüzdebirlik ve seçili ölçümün ortalama yüzdesi. |
| Ortalama, Min, maks., 50, 90  | Döküm kapsayıcı seçilen yüzdelik dilim için ortalama CPU millicore veya bellek performansını. Ortalama değer, bir pod için CPU/bellek sınırını zamandan ölçülür. |
| Pod | Pod bulunduğu kapsayıcısı.| 
| Node |  Kapsayıcı bulunduğu düğümü. | 
| Yeniden başlatma | Kapsayıcı başladıktan sonra süresini temsil eder. |
| Çalışma Süresi | Kapsayıcı kullanmaya ya da yeniden beri süresini temsil eder. |
| Eğilim ortalama&nbsp;%, Min&nbsp;en fazla %&nbsp;% 50&nbsp;% 90'ıncı&nbsp;% | Kapsayıcı ölçüm ortalama yüzdesini temsil eder bir çubuk grafik eğilimini. |

Durum alanı simgeleri, aşağıdaki tabloda açıklandığı gibi çevrimiçi, pod'ların durumlarını gösterir:
 
| Simge | Durum | 
|--------|-------------|
| ![Hazır çalışan durum simgesi](./media/monitoring-container-health/container-health-ready-icon.png) | (Hazır) çalıştıran|
| ![Bekleyen veya duraklatıldı durum simgesi](./media/monitoring-container-health/container-health-waiting-icon.png) | Bekleyen veya duraklatıldı|
| ![Son durum simgesi çalıştıran bildirdi.](./media/monitoring-container-health/container-health-grey-icon.png) | Son çalıştırma bildirdi ancak 30 dakikadan fazla yanıt henüz|
| ![Sonlandırılan durum simgesi](./media/monitoring-container-health/container-health-terminated-icon.png) | Başarılı bir şekilde durdurulmuş veya yanıt vermemesine başarısız|
| ![Başarısız durum simgesi](./media/monitoring-container-health/container-health-failed-icon.png) | Durumu başarısız |

## <a name="container-data-collection-details"></a>Kapsayıcı veri toplama ayrıntıları
Kapsayıcı konağında ve kapsayıcılar, kapsayıcı durumu çeşitli performans ölçümleri ve günlük verilerini toplar. Üç dakikada bir toplanan veriler.

### <a name="container-records"></a>Kapsayıcı kayıt

Kapsayıcı durumunun ve günlük araması sonuçlarında görüntülenen veri türleri tarafından toplanan kayıtları örnekleri aşağıdaki tabloda görüntülenir:

| Veri türü | Günlük araması'nda veri türü | Alanlar |
| --- | --- | --- |
| Konaklar ve kapsayıcılar için performans | `Perf` | Bilgisayar, ObjectName, CounterName &#40;% işlemci zamanı, Disk okuma MB, MB, bellek kullanımı MB Disk Yazar ağ bayt alma, ağ gönderme bayt, işlemci kullanımı sn, ağ&#41;, Ort, TimeGenerated, sayaç yolu, Analytics'teki |
| Kapsayıcı envanteri | `ContainerInventory` | TimeGenerated, bilgisayar, kapsayıcı adı, ContainerHostname, görüntü, ImageTag, ContainerState, ExitCode, EnvironmentVar, komut, oluşturulma zamanı, StartedTime, FinishedTime, Analytics'teki, Containerıd, ImageID |
| Kapsayıcı görüntüsü envanterinin | `ContainerImageInventory` | TimeGenerated, bilgisayar, görüntü, ImageTag, ImageSize, VirtualSize, çalışıyor, durduruldu, durduruldu, başarısız, Analytics'teki, ImageID TotalContainer |
| Kapsayıcı günlüğü | `ContainerLog` | TimeGenerated, bilgisayar, görüntü kimliği, LogEntrySource LogEntry, Analytics'teki, Containerıd kapsayıcı adı |
| Kapsayıcı hizmeti günlüğü | `ContainerServiceLog`  | TimeGenerated, bilgisayar, TimeOfCommand, görüntü, komut, Analytics'teki, Containerıd |
| Kapsayıcı düğümü envanteri | `ContainerNodeInventory_CL`| TimeGenerated, bilgisayar, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, Analytics'teki|
| Kapsayıcı işlemi | `ContainerProcess_CL` | TimeGenerated, bilgisayar, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, Analytics'teki |
| Bir Kubernetes kümesinin pod envanterini | `KubePodInventory` | TimeGenerated, bilgisayar, Lclusterıd, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, HizmetAdı, ControllerKind, ControllerName, ContainerStatus Containerıd, kapsayıcı adı, ad, PodLabel, Namespace, PodStatus, ClusterName, Podıp, Analytics'teki |
| Bir Kubernetes kümesinin düğümleri bölümünün envanteri | `KubeNodeInventory` | TimeGenerated, bilgisayar, ClusterName, Lclusterıd, LastTransitionTimeReady, etiketler, durum, KubeletVersion, KubeProxyVersion, CreationTimeStamp, Analytics'teki | 
| Kubernetes olayları | `KubeEvents_CL` | TimeGenerated, bilgisayar, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, Message, Analytics'teki | 
| Kubernetes kümesindeki Hizmetleri | `KubeServices_CL` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, Analytics'teki | 
| Kubernetes küme düğümleri parçası için performans ölçümleri | Perf &#124; nerede ObjectName "K8SNode" == | Bilgisayar, ObjectName, CounterName &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, networkRxBytes, networkTxBytes, restartTimeEpoch, networkRxBytesPerSec, networkTxBytesPerSec, cpuAllocatableNanoCores, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes&#41;, Ort, TimeGenerated, sayaç yolu, Analytics'teki | 
| Kubernetes kümesine kapsayıcıları parçası için performans ölçümleri | Perf &#124; nerede ObjectName "K8SContainer" == | CounterName &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, restartTimeEpoch, cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryLimitBytes&#41;, Ort, TimeGenerated, sayaç yolu, Analytics'teki | 

## <a name="search-logs-to-analyze-data"></a>Verileri çözümlemek için günlüklerinde arama yapma
Log Analytics'e eğilimlerini arayın, performans sorunlarını tanılamanıza yardımcı olur, yardımcı olabilecek tahmin ve performanstaki veri geçerli küme yapılandırmasını en uygun şekilde çalışıp çalışmadığını belirleyin. Önceden tanımlanmış günlük aramaları, hemen kullanmaya başlayın ya da istediğiniz gibi bilgileri döndürmek için özelleştirmek için sağlanır. 

Etkileşimli veri analizi seçerek çalışma alanında gerçekleştirebilirsiniz **görünümü Kubernetes olay günlüklerini** veya **kapsayıcı günlüklerini görüntüleme** önizleme bölmesinde seçeneği. **Günlük araması** bulunduğunuz Azure portal sayfasının sağındaki sayfası görüntülenir.

![Log analytics'te verilerini çözümleme](./media/monitoring-container-health/container-health-log-search-example.png)   

Log Analytics'e iletilir kapsayıcı günlüklerini çıktısı, STDOUT ve STDERR alınır. Kapsayıcı durumunun, Azure tarafından yönetilen Kubernetes (AKS) izleme için Kube-system büyük oluşturulan veri hacmi nedeniyle bugün toplanmaz. 

### <a name="example-log-search-queries"></a>Örnek günlük arama sorguları
Genellikle, bir örnek veya iki ile başlayıp ardından bunları gereksinimlerinize uyacak şekilde değiştirin sorguları oluşturmak kullanışlıdır. Daha gelişmiş sorgular oluşturmanıza yardımcı olmak için aşağıdaki örnek sorgularda ile denemeler yapabilirsiniz:

| Sorgu | Açıklama | 
|-------|-------------|
| ContainerInventory<br> &#124;Proje bilgisayar, ad, resim, ImageTag, ContainerState, oluşturulma zamanı, StartedTime, FinishedTime<br> &#124;Tablo oluşturma | Tüm bir kapsayıcının yaşam döngüsü bilgilerini Listele| 
| KubeEvents_CL<br> &#124;Burada not(isempty(Namespace_s))<br> &#124;TimeGenerated desc göre sırala<br> &#124;Tablo oluşturma | Kubernetes olayları|
| ContainerImageInventory<br> &#124;Summarize aggregatedvalue = count() ImageTag, görüntü tarafından çalıştırma | Görüntü envanteri | 
| **Advanced Analytics, çizgi grafiklerde seçin**:<br> Perf<br> &#124;Burada ObjectName "Container" ve CounterName == "% işlemci zamanı" ==<br> &#124;AvgCPUPercent özetlemek avg(CounterValue) tarafından bin (TimeGenerated, 30 dakika), InstanceName = | Kapsayıcı CPU | 
| **Advanced Analytics, çizgi grafiklerde seçin**:<br> Perf &#124; nerede ObjectName "Container" ve CounterName == "Bellek kullanım MB" ==<br> &#124;AvgUsedMemory özetlemek avg(CounterValue) tarafından bin (TimeGenerated, 30 dakika), InstanceName = | Kapsayıcı belleği |

## <a name="how-to-stop-monitoring-with-container-health"></a>İle kapsayıcı durumunun izlenmesi durdurma
Sonra AKS kapsayıcı izleme etkinleştirirseniz, artık bunu izlemek istediğiniz karar, yapabilecekleriniz *çıkma* PowerShell cmdlet'i ile sağlanan Azure Resource Manager şablonları kullanarak  **Yeni-AzureRmResourceGroupDeployment** veya Azure CLI. Bir JSON şablonunu belirtir yapılandırmayı *çıkma*. Diğer küme olarak dağıtılan AKS küme kaynağı kimliği ve kaynak grubu belirtmek için yapılandırdığınız parametre değerlerini içerir. 

Bir şablon kullanarak kaynakları dağıtma kavramıyla bilmiyorsanız, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md)
* [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../azure-resource-manager/resource-group-template-deploy-cli.md)

Azure CLI'yı kullanmayı seçerseniz, ilk CLI'yi yerel olarak yükleyip kullanmayı gerekir. Azure CLI Sürüm 2.0.27 çalıştırıyor olmanız gerekir veya üzeri. Sürümünüzü belirlemek için çalıştırma `az --version`. Gerekirse yükleyin veya Azure CLI'yı yükseltmek için bkz: [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). 

### <a name="create-and-execute-template"></a>Oluşturma ve şablon yürütme

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

    * Şablonu içeren klasörde şu PowerShell komutlarını kullanmak için:

        ```powershell
        Connect-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionName <yourSubscriptionName>
        New-AzureRmResourceGroupDeployment -Name opt-out -ResourceGroupName <ResourceGroupName> -TemplateFile .\OptOutTemplate.json -TemplateParameterFile .\OptOutParam.json
        ```

        Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuçları içeren aşağıdakine benzer bir ileti döndürülür:

        ```powershell
        ProvisioningState       : Succeeded
        ```

    * Komut, Azure CLI ile Linux üzerinde çalıştırmak için:

        ```azurecli
        az login   
        az account set --subscription "Subscription Name" 
        az group deployment create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
        ```

        Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuçları içeren aşağıdakine benzer bir ileti döndürülür:

        ```azurecli
        ProvisioningState       : Succeeded
        ```

Yalnızca küme İzleme'yi desteklemek için çalışma alanı oluşturuldu ve artık gerekli olmadığında, el ile silmeniz gerekir. Bir çalışma alanını silme konusunda bilgi sahibi değilseniz bkz [Azure portalı ile bir Azure Log Analytics çalışma alanını silme](../log-analytics/log-analytics-manage-del-workspace.md). Hakkında unutmayın **çalışma alanı kaynak kimliği** biz 4. adımda daha önce kopyaladığınız, ihtiyacınız olacak. 

## <a name="troubleshooting"></a>Sorun giderme
Bu bölümde, kapsayıcı durumu ile ilgili sorunları gidermenize yardımcı olacak bilgiler sağlar.

Kapsayıcı durumunun başarıyla etkinleştirilmiş ve yapılandırılmış, ancak bir günlük araması yaptığınızda, durum bilgisi ya da sonuçları Log Analytics'te görüntüleyemezsiniz, aşağıdakileri yaparak sorunu tanılamanıza yardımcı olabilir: 

1. Aşağıdaki komutu çalıştırarak aracının durumunu denetleyin: 

    `kubectl get ds omsagent --namespace=kube-system`

    Doğru şekilde dağıtıldığını gösteren aşağıdaki çıktıya benzemelidir:

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```  
2. Aracı sürümü ile çözüm dağıtım durumunu denetlemek *06072018* veya aşağıdaki komutu çalıştırarak daha sonra:

    `kubectl get deployment omsagent-rs -n=kube-system`

    Doğru şekilde dağıtıldığını gösteren aşağıdaki çıktıya benzemelidir:

    ```
    User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
    NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
    omsagent   1         1         1            1            3h
    ```

3. Aşağıdaki komutu, çalışan tarafından çalışıyor olduğunu doğrulamak için pod durumunu kontrol edin: `kubectl get pods --namespace=kube-system`

    Durumu aşağıdaki çıktıya benzemelidir *çalıştıran* omsagent için:

    ```
    User@aksuser:~$ kubectl get pods --namespace=kube-system 
    NAME                                READY     STATUS    RESTARTS   AGE 
    aks-ssh-139866255-5n7k5             1/1       Running   0          8d 
    azure-vote-back-4149398501-7skz0    1/1       Running   0          22d 
    azure-vote-front-3826909965-30n62   1/1       Running   0          22d 
    omsagent-484hw                      1/1       Running   0          1d 
    omsagent-fkq7g                      1/1       Running   0          1d 
    ```

4. Aracı günlüklerini denetleyin. Kapsayıcı Aracısı dağıtıldığında OMI komutları çalıştırarak hızlı bir denetim çalıştırır ve sağlayıcı ve aracı sürümünü gösterir. 

5. Aracıyı eklenen başarıyla sağlandığını doğrulamak için aşağıdaki komutu çalıştırın: `kubectl logs omsagent-484hw --namespace=kube-system`

    Durum şuna benzemelidir:

    ```
    User@aksuser:~$ kubectl logs omsagent-484hw --namespace=kube-system
    :
    :
    instance of Container_HostInventory
    {
        [Key] InstanceID=3a4407a5-d840-4c59-b2f0-8d42e07298c2
        Computer=aks-nodepool1-39773055-0
        DockerVersion=1.13.1
        OperatingSystem=Ubuntu 16.04.3 LTS
        Volume=local
        Network=bridge host macvlan null overlay
        NodeRole=Not Orchestrated
        OrchestratorType=Kubernetes
    }
    Primary Workspace: b438b4f6-912a-46d5-9cb1-b44069212abc    Status: Onboarded(OMSAgent Running)
    omi 1.4.2.2
    omsagent 1.6.0.23
    docker-cimprov 1.0.0.31
    ```

## <a name="next-steps"></a>Sonraki adımlar

Ayrıntılı kapsayıcı sistem durumu ve uygulama performans bilgilerini görüntülemek için bkz: [arama günlüklerini](../log-analytics/log-analytics-log-search.md). 
