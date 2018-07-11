---
title: İzleme Azure Kubernetes Service (AKS) sistem durumu (Önizleme) | Microsoft Docs
description: Bu makalede, barındırılan Kubernetes ortamınızı kullanımını hızla anlamanız için AKS kapsayıcı performansı nasıl kolayca inceleyebilirsiniz açıklanır.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/08/2018
ms.author: magoedte
ms.openlocfilehash: a94f7289c75a4f4d466542c608d81cf5b954f4b1
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37917359"
---
# <a name="monitor-azure-kubernetes-service-aks-container-health-preview"></a>Azure Kubernetes Service (AKS) kapsayıcı durumu (Önizleme) izleme

Bu makalede, ayarlama ve Azure Kubernetes Service (AKS) barındırılan Kubernetes ortamlara dağıtılan iş yüklerinizin performansını izlemek için Azure İzleyici kapsayıcısı durumuna kullanma açıklar.  Kubernetes kümenizin ve kapsayıcılarınızın izlenmesi, özellikle de birden fazla uygulama ile ölçekli olarak bir üretim kümesi çalıştırılırken kritik önem taşır.

Kapsayıcı durumunun performans toplama bellek ve işlemci ölçümleri denetleyicileri, düğümleri ve kullanılabilir ölçümler API aracılığıyla kubernetes'te kapsayıcı tarafından yeteneği sağlar.  Kapsayıcı durumunun etkinleştirdikten sonra Bu ölçümler otomatik olarak, Linux için OMS Aracısı kapsayıcıya alınmış bir sürümü kullanılarak toplanmış ve depolanmış, [Log Analytics](../log-analytics/log-analytics-overview.md) çalışma.  Dahil edilen önceden tanımlanmış görünümleri kaynaklarınızda bulunan kapsayıcı iş yüklerinin ve, anlayabilmeniz ne performans sistem Kubernetes kümesinin etkilediğini gösterir:  

* Kapsayıcıların ne çalışan düğümü ve kaynak performans sorunlarını tanımlamak için ortalama işlemci ve bellek kullanımı
* Kapsayıcı bir denetleyici ve/veya bir denetleyici veya pod genel performansını görmek için pod'ların yer aldığı tanımlayın 
* Standart işlemlere pod destekleyen ilgisi olmayan bir konakta çalışan iş yüklerinin kaynak kullanımını gözden geçirin
* Küme kapasitesi gereksinimlerini tanımlama ve sürdürmek en yüksek yük belirlemek için ortalama ve en yoğun yük altında davranışını anlama 

Docker ve Windows yönetme ve izleme de ilgileniyorsanız kapsayıcı konakları görünümü yapılandırma, denetleme ve kaynak kullanımını görmek [kapsayıcı izleme çözümü](../log-analytics/log-analytics-containers.md).

## <a name="requirements"></a>Gereksinimler 
Desteklenen önkoşulları anlayabilmeniz başlamadan önce aşağıdaki ayrıntıları gözden geçirin.

- Yeni veya mevcut bir AKS kümesi
- Linux sürümü için kapsayıcı bir OMS Aracısı microsoft / oms:ciprod04202018 ve daha sonra. Sürüm numarasını temsil edilen bir tarihe göre aşağıdaki biçimi - *mmddyyyy*.  Kapsayıcı durumunun ekleme sırasında otomatik olarak yüklenir.  
- Log Analytics çalışma alanı.  Yeni AKS kümesini izleme etkinleştirmek ya da aracılığıyla bir tane oluşturabilirsiniz oluşturulabilir [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), veya [Azure portalında](../log-analytics/log-analytics-quick-create-workspace.md).
- Kapsayıcı izlemeyi etkinleştirmek için Log Analytics katkıda bulunan rolü üyesi.  Log Analytics çalışma alanına erişimi denetleme hakkında daha fazla bilgi için bkz. [çalışma alanlarını yönetme](../log-analytics/log-analytics-manage-access.md).

## <a name="components"></a>Bileşenler 

Bu özellik için bir kümedeki tüm düğümlerin performans ve olay verilerini toplamak Linux kapsayıcı bir OMS Aracısı kullanır.  Aracı otomatik olarak dağıtılan ve kapsayıcı izleme etkinleştirdikten sonra belirtilen Log Analytics çalışma alanı ile kayıtlı. 

>[!NOTE] 
>Bir AKS kümesi zaten dağıttıysanız, bu makalenin sonraki bölümlerinde gösterildiği gibi sağlanan bir Azure Resource Manager şablonu kullanarak izlemeyi etkinleştirin. Kullanamazsınız `kubectl` yükseltmek için silmek, yeniden dağıtın veya aracıyı dağıtın.  
>

## <a name="sign-in-to-azure-portal"></a>Azure Portalında oturum açın
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın. 

## <a name="enable-container-health-monitoring-for-a-new-cluster"></a>Kapsayıcı, yeni bir küme için sistem durumu izlemeyi etkinleştir
Yeni bir AKS kümesini Azure portalından dağıtımı sırasında izlemeyi etkinleştirebilirsiniz.  Hızlı Başlangıç makalesinde adımları [Azure Kubernetes Service (AKS) kümesini dağıtma](../aks/kubernetes-walkthrough-portal.md).  Üzerinde olduğunda **izleme** sayfasında **Evet** seçeneği için **izlemeyi etkinleştirme** etkinleştirmek, mevcut bir seçin veya yeni bir Log Analytics çalışma alanı oluşturma.  

İzleme etkinleştirildikten sonra tüm yapılandırma görevleri başarıyla tamamlandı, iki yoldan biriyle kümenizden performansını izleyebilirsiniz:

1. Seçerek doğrudan AKS kümeden **sistem durumu** sol bölmeden.<br><br> 
2. Tıklayarak **kapsayıcı sistem durumu izleme** AKS kümesi sayfasında seçilen küme için bir kutucuk.  Azure İzleyicisi'nde seçin **sistem durumu** sol bölmeden.  

![AKS kapsayıcı sistem seçilecek seçenekleri](./media/monitoring-container-health/container-performance-and-health-select-01.png)

İzleme etkinleştirildikten sonra küme için işletimsel verileri görebilmek için önce yaklaşık 15 dakika sürebilir.  

## <a name="enable-container-health-monitoring-for-existing-managed-clusters"></a>Kapsayıcı mevcut yönetilen kümeleri için sistem durumu izlemeyi etkinleştir
Azure portalından veya PowerShell cmdlet'i kullanılarak belirtilen Azure Resource Manager şablonu ile zaten dağıtılmış bir AKS kümesi izlemesini etkinleştirebilirsiniz **New-AzureRmResourceGroupDeployment** veya Azure CLI.  


### <a name="enable-from-azure-portal"></a>Azure portalından etkinleştirme
Azure portalında AKS kapsayıcınızı izlemeyi etkinleştirmek için aşağıdaki adımları gerçekleştirin.

1. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde yazın **kapsayıcıları**. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **Kubernetes Hizmetleri**.<br><br> ![Azure portal](./media/monitoring-container-health/azure-portal-01.png)<br><br>  
2. Kapsayıcılar, listeden bir kapsayıcı seçin.
3. Kapsayıcı genel bakış sayfasında **kapsayıcı sistem durumu izleme** ve **kapsayıcı durumunun ve günlükleri ekleme** sayfası görüntülenir.
4. Üzerinde **kapsayıcı durumunun ve günlükleri ekleme** sayfasında, mevcut bir Log Analytics varsa küme ile aynı abonelikte çalışma alanı, aşağı açılan listeden seçin.  Varsayılan çalışma listesi belirler ve konum AKS kapsayıcı abonelikte dağıtılmış.<br><br> ![AKS kapsayıcı sistem durumu izlemeyi etkinleştir](./media/monitoring-container-health/container-health-enable-brownfield-02.png) 

>[!NOTE]
>Küme izleme verilerini depolamak için yeni bir Log Analytics çalışma alanı oluşturmak istiyorsanız, adımları [Cretae bir Log Analytics çalışma alanı](../log-analytics/log-analytics-quick-create-workspace.md) ve AKS kapsayıcı aynı abonelikte çalışma alanı oluşturduğunuzdan emin olun Dağıttı.  
>
 
İzleme etkinleştirildikten sonra küme için işletimsel verileri görebilmek için önce yaklaşık 15 dakika sürebilir. 

### <a name="enable-using-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak etkinleştirin
Bu yöntem iki JSON şablonları içerir, yapılandırmayı, izlemeyi etkinleştirmek için bir şablon belirtir ve parametre değerlerini aşağıdaki belirtmek için yapılandırmak için bir JSON şablonunu içerir:

* AKS kapsayıcı kaynak kimliği 
* Küme kaynak grubu içinde dağıtılan 
* Log Analytics çalışma alanı ve çalışma alanında oluşturmak için bölge 

Log Analytics çalışma alanını el ile oluşturulması gerekir.  Çalışma alanı oluşturmak için bir aracılığıyla ayarlayabilirsiniz [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), gelen [Azure portalında](../log-analytics/log-analytics-quick-create-workspace.md).

PowerShell ile bir şablon kullanarak kaynakları dağıtma kavramlarına aşina değilseniz bkz [kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md)veya Azure CLI bakın [kaynakları dağıtma Resource Manager şablonları ve Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).

Azure CLI'yı kullanmayı seçerseniz, önce CLI yerel olarak yükleyip kullanmayı gerekir.  Gerekli Azure CLI Sürüm 2.0.27 çalıştırdığınızı veya üzeri. Çalıştırma `az --version` sürümünü belirlemek için. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). 

#### <a name="create-and-execute-template"></a>Oluşturma ve şablon yürütme

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
3. Aşağıdaki JSON söz dizimini kopyalayıp dosyanıza yapıştırın:

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

4. Değeri Düzenle **aksResourceId**, **aksResourceLocation** bulabileceğiniz değerlere sahip **AKS'ye genel bakış** AKS kümesi sayfası.  Değeri **workspaceResourceId** çalışma alanı adı içerir, Log Analytics çalışma alanının tam kaynak kimliği.  Ayrıca için çalışma zamanı konumunu belirtin **workspaceRegion**.    
5. Bu dosyayı farklı Kaydet **existingClusterParam.json** yerel bir klasöre.
6. Bu şablonu dağıtmaya hazırsınız. 

    * Şablonu içeren klasörden aşağıdaki PowerShell komutlarını kullanın:

        ```powershell
        New-AzureRmResourceGroupDeployment -Name OnboardCluster -ResourceGroupName ClusterResourceGroupName -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
        ```
        Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuç içeren aşağıdakine benzer bir ileti görürsünüz:

        ```powershell
        provisioningState       : Succeeded
        ```

    * Komut, Azure CLI ile Linux üzerinde çalıştırmak için:
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
        ```

        Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuç içeren aşağıdakine benzer bir ileti görürsünüz:

        ```azurecli
        provisioningState       : Succeeded
        ```
İzleme etkinleştirildikten sonra küme için işletimsel verileri görebilmek için önce yaklaşık 15 dakika sürebilir.  

## <a name="verify-agent-and-solution-deployment"></a>Aracı ve çözüm dağıtımı doğrulama
Aracı sürümü ile *06072018* ve üzeri, hem aracıyı hem de çözüm başarıyla dağıtıldı doğrulayamadı.  Aracıyı önceki sürümleriyle aracı dağıtımı doğrulayabilirsiniz.

### <a name="agent-version-06072018-and-higher"></a>Aracı sürümü 06072018 ve üzeri
Aracı başarıyla dağıtıldığını doğrulamak için aşağıdaki komutu çalıştırın.   

```
kubectl get ds omsagent --namespace=kube-system
```

Çıktı aşağıdaki düzgün bir şekilde dağıtmak belirten benzemelidir:

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

Çözümün dağıtımı doğrulamak için aşağıdaki komutu çalıştırın:

```
kubectl get deployment omsagent-rs -n=kube-system
```

Çıktı aşağıdaki düzgün bir şekilde dağıtmak belirten benzemelidir:

```
User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
omsagent   1         1         1            1            3h
```

### <a name="agent-version-earlier-than-06072018"></a>Aracı sürümü 06072018 öncesi

Önce serbest OMS Aracısı sürümünü doğrulamak için *06072018* düzgün bir şekilde aşağıdaki komutu çalıştırarak dağıtılır:  

```
kubectl get ds omsagent --namespace=kube-system
```

Çıktı aşağıdaki düzgün bir şekilde dağıtmak belirten benzemelidir:  

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-performance-utilization"></a>Görünüm performans kullanımı
Kapsayıcı durumunun açtığınızda, sayfanın hemen Küme düğümlerinizi performans kullanımını gösterir.  AKS kümenizi hakkında bilgi görüntüleme, üç Perspektifler düzenlenmiştir:

- Düğümler 
- Denetleyiciler  
- Kapsayıcılar

Satır hiyerarşi kümenizdeki bir düğümü başlangıç Kubernetes nesne modelini izler.  Düğümünü genişletin düğüm üzerinde çalışan bir veya daha fazla pod bakın ve birden fazla kapsayıcı için bir pod gruplandırılmış varsa, bunlar hiyerarşideki son satır olarak gösterilir.<br><br> ![Performans Görünümü'nde örnek Kubernetes düğüm hiyerarşisi](./media/monitoring-container-health/container-performance-and-health-view-03.png)

Sayfanın üst kısmından denetleyicileri veya kapsayıcılar'ı seçin ve bu nesneler için durumu ve kaynak kullanımını gözden geçirin.  Açılır liste kutusu ekranın en üstünde ad alanı, hizmet ve düğüm göre filtrelemek için kullanın. Bunun yerine, gelen bellek kullanımı, gözden geçirmek istiyorsanız **ölçüm** açılan listesini seçin **bellek RSS** veya **bellek çalışma kümesi**.  **Bellek RSS** yalnızca Kubernetes sürüm 1.8 ve üzeri için desteklenir. Aksi takdirde, değerler için bkz: **ortalama %** olarak gösteren *NaN %*, sayısal veri türü tanımlanmamış veya çıkarıldığında bir değeri temsil eden bir değer. 

![Kapsayıcı performansı düğümleri performans görünümü](./media/monitoring-container-health/container-performance-and-health-view-04.png)

Varsayılan olarak, son altı saat üzerinde performans verilerini dayanır ancak penceresiyle değiştirebilirsiniz **zaman aralığı** sayfanın sağ üst köşesindeki açılır listede bulunamadı. Şu anda sayfa değil otomatik yenileme, bu nedenle el ile yenilemeniz gerekir. 

Aşağıdaki örnekte, düğüm için fark *aks agentpool 3402399 0*, değeri **kapsayıcıları** toplu dağıtılan kapsayıcıların toplam sayısı 10 olacaktır.<br><br> ![Kapsayıcı düğümü örnek başına dökümü](./media/monitoring-container-health/container-performance-and-health-view-07.png)<br><br> Kapsayıcı düğümleri arasında uygun bir denge yoksa hızlıca belirlemenize yardımcı olabilir.  

Aşağıdaki tabloda, düğümleri görüntülediğinizde sunulan bilgiler açıklanmaktadır.

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Ana bilgisayar adı |
| Durum | Kubernetes görünümünü düğüm durumu |
| ORTALAMA % | Seçili süre için seçilen bir ölçüme göre tabanlı düğüm ortalama yüzdesi. |
| ORTALAMA | Seçili süre için ölçüm seçili dayanarak gerçek değeri ortalama düğüm.  Ortalama değer, bir düğüm için CPU/bellek sınırını zamandan ölçülür; pod'ların ve kapsayıcılar için ana bilgisayar tarafından bildirilen ortalama değeri var. |
| Kapsayıcılar | Kapsayıcı sayısı. |
| Çalışma Süresi | Bir düğüm başlatıldığında veya yeniden başlatıldıktan sonra süresini temsil eder. |
| Pod | Yalnızca kapsayıcılar için. Bu, bulunan pods gösterir. |
| Denetleyiciler | Yalnızca kapsayıcılar ve pod'ları için. Bu, bulunan hangi denetleyicisinin gösterir. Bazı yok gösterebilir. Bu nedenle tüm pod'ların bir denetleyici olacaktır. | 
| Eğilim ortalama % | Kapsayıcı ve düğüm ortalama ölçüm % göre çubuk grafik eğilim. |


Seçiciden seçin **denetleyicileri**.<br><br> ![Select denetleyicileri görüntüle](./media/monitoring-container-health/container-performance-and-health-view-08.png)

Burada denetleyicilerinizi performans durumunu görebilirsiniz.<br><br> ![< ad > denetleyicileri performans görünümü](./media/monitoring-container-health/container-performance-and-health-view-05.png)

Satır hiyerarşi denetleyicisi ile başlar ve denetleyici genişletir ve pod'ların bir veya daha fazla ya da bir veya daha fazla kapsayıcı görürsünüz.  Bir pod'ı genişletin ve son satırını pod'u gruplandırılmış kapsayıcıyı göster.  

Aşağıdaki tabloda denetleyicileri görüntülediğinizde sunulan bilgiler açıklanmaktadır.

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Denetleyicinin adı|
| Durum | Durumu ile çalıştırılması tamamlandığında kapsayıcıları durumunu *kesildi*, *başarısız* *durduruldu*, veya *duraklatıldı*. Kapsayıcıyı çalıştıran, ancak durumu ya da düzgün şekilde sunulan veya aracı tarafından toplanmış değil ve 30 dakikadan fazla yanıtlamada: durum olacaktır *bilinmeyen*. |
| ORTALAMA % | Ortalama her varlığın seçilen ölçüm için ortalama % yukarı yuvarlayın. |
| ORTALAMA | Ortalama CPU millicore veya bellek performansını kapsayıcının toplanan.  Ortalama değer, bir pod için CPU/bellek sınırını zamandan ölçülür. |
| Kapsayıcılar | Denetleyici veya pod için kapsayıcı toplam sayısı. |
| Yeniden başlatma | Yeniden başlatma sayısını kapsayıcılardan döndürün. |
| Çalışma Süresi | Kapsayıcı başladıktan sonra süresini temsil eder. |
| Pod | Yalnızca kapsayıcılar için. Bu, bulunan pods gösterir. |
| Node | Yalnızca kapsayıcılar ve pod'ları için. Bu, bulunan hangi denetleyicisinin gösterir. | 
| Eğilim ortalama % | Çubuk grafik eğilim kapsayıcının ölçüm ortalama % sunma. |

Seçiciden seçin **kapsayıcıları**.<br><br> ![Select kapsayıcıları görüntüleyin](./media/monitoring-container-health/container-performance-and-health-view-09.png)

Kapsayıcılarınızı performans durumunu burada görebiliriz.<br><br> ![< ad > denetleyicileri performans görünümü](./media/monitoring-container-health/container-performance-and-health-view-06.png)

Aşağıdaki tabloda, kapsayıcıları görüntülediğinizde sunulan bilgiler açıklanmaktadır.

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Denetleyicinin adı|
| Durum | Kapsayıcı durumunun dökümünü varsa döndürün. |
| ORTALAMA % | Ortalama her varlığın seçilen ölçüm için ortalama % yukarı yuvarlayın. |
| ORTALAMA | Ortalama CPU millicore veya bellek performansını kapsayıcının toplanan. Ortalama değer, bir pod için CPU/bellek sınırını zamandan ölçülür. |
| Kapsayıcılar | Kapsayıcılar için denetleyici toplam sayısı.|
| Yeniden başlatma | Kapsayıcı başladıktan sonra süresini temsil eder. |
| Çalışma Süresi | Kapsayıcı kullanmaya ya da yeniden beri süresini temsil eder. |
| Pod | Pod bilgiler nerede yer alıyor. |
| Node |  Kapsayıcı bulunduğu düğümü.  | 
| Eğilim ortalama % | Çubuk grafik eğilim kapsayıcının ölçüm ortalama % sunma. |

## <a name="container-data-collection-details"></a>Kapsayıcı veri koleksiyonu ayrıntıları
Kapsayıcı konağında ve kapsayıcılar, kapsayıcı durumu çeşitli performans ölçümleri ve günlük verilerini toplar. Üç dakikada bir toplanan veriler.

### <a name="container-records"></a>Kapsayıcı kayıt

Aşağıdaki tabloda, kapsayıcı durumu ve günlük arama sonuçlarında görünmesini veri türleri tarafından toplanan kayıtları örneklerini gösterir.

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
Log Analytics'e eğilimlerini arayın, performans sorunlarını tanılamanıza yardımcı olur, yardımcı olabilecek tahmin ve performanstaki veri geçerli küme yapılandırmasını en uygun şekilde çalışıp çalışmadığını belirleyin.  Önceden tanımlanmış günlük aramalarının hemen kullanmaya başlayın ya da istediğiniz gibi bilgileri döndürmek için özelleştirmek için sağlanır. 

Etkileşimli analiz veri seçerek çalışma alanında gerçekleştirebilirsiniz **Günlüğü Görüntüle** seçeneği, bir kapsayıcı genişlettiğinizde sağdaki üzerinde kullanılabilir.  **Günlük arama** sayfası doğru açtığınız portalda sayfanın üstünde görünür.<br><br> ![Log analytics'te verilerini çözümleme](./media/monitoring-container-health/container-performance-and-health-view-logs-01.png)   

Log Analytics'e iletilir kapsayıcı günlüklerini çıktısı, STDOUT ve STDERR alınır. Kapsayıcı durumunun, Azure yönetilen Kubernetes (AKS) izleme için Kube-system büyük oluşturulan veri hacmi nedeniyle bugün toplanmaz.     

### <a name="example-log-search-queries"></a>Örnek günlük arama sorguları
Genellikle, bir örnek veya iki ile başlayan ve sonra gereksinimlerinizi karşılayacak şekilde değiştirerek sorguları oluşturmak kullanışlıdır. Daha gelişmiş sorgular oluşturmanıza yardımcı olması için aşağıdaki örnek sorgularda ile denemeler yapabilirsiniz.

| Sorgu | Açıklama | 
|-------|-------------|
| ContainerInventory<br> &#124;Proje bilgisayar, ad, resim, ImageTag, ContainerState, oluşturulma zamanı, StartedTime, FinishedTime<br> &#124;Tablo oluşturma | Tüm kapsayıcıların yaşam döngüsü bilgilerini Listele| 
| KubeEvents_CL<br> &#124;Burada not(isempty(Namespace_s))<br> &#124;TimeGenerated desc göre sırala<br> &#124;Tablo oluşturma | Kubernetes olayları|
| ContainerImageInventory<br> &#124;Summarize aggregatedvalue = count() ImageTag, görüntü tarafından çalıştırma | Görüntü envanteri | 
| **Advanced Analytics, çizgi grafiklerde seçin**:<br> Perf<br> &#124;Burada ObjectName "Container" ve CounterName == "% işlemci zamanı" ==<br> &#124;AvgCPUPercent özetlemek avg(CounterValue) tarafından bin (TimeGenerated, 30 dakika), InstanceName = | Kapsayıcı CPU | 
| **Advanced Analytics, çizgi grafiklerde seçin**:<br> Perf &#124; nerede ObjectName "Container" ve CounterName == "Bellek kullanım MB" ==<br> &#124;AvgUsedMemory özetlemek avg(CounterValue) tarafından bin (TimeGenerated, 30 dakika), InstanceName = | Kapsayıcı belleği |

## <a name="how-to-stop-monitoring-with-container-health"></a>İle kapsayıcı durumunun izlenmesi durdurma
AKS kapsayıcınızı izleme etkinleştirdikten sonra artık bunu izlemek istiyorsanız karar, yapabilecekleriniz *çıkma* PowerShell cmdlet'i ile sağlanan Azure Resource Manager şablonları kullanarak  **Yeni-AzureRmResourceGroupDeployment** veya Azure CLI.  Bir JSON şablonunu belirtir yapılandırmayı *çıkma* ve parametre değerlerini Yapılandır kümenin dağıtıldığı AKS küme kaynağı kimliği ve kaynak grubu belirtmek için bir JSON şablonunu içerir.  PowerShell ile bir şablon kullanarak kaynakları dağıtma kavramlarına aşina değilseniz bkz [kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md) veya Azure CLI bakın [kaynakları dağıtma Resource Manager şablonları ve Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).

Azure CLI'yı kullanmayı seçerseniz, önce CLI yerel olarak yükleyip kullanmayı gerekir.  Gerekli Azure CLI Sürüm 2.0.27 çalıştırdığınızı veya üzeri. Çalıştırma `az --version` sürümünü belirlemek için. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). 

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
              "config": {
                "logAnalyticsWorkspaceResourceID": null
              }
            }
           }
         }
       }
      ]
    }
    ```

2. Bu dosyayı farklı Kaydet **OptOutTemplate.json** yerel bir klasöre.
3. Aşağıdaki JSON söz dizimini kopyalayıp dosyanıza yapıştırın:

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

4. Değeri Düzenle **aksResourceId** ve **aksResourceLocation** bulabileceğiniz AKS kümesinin değerlerle **özellikleri** seçili kümenin sayfası.<br><br> ![Kapsayıcı Özellikleri Sayfası](./media/monitoring-container-health/container-properties-page.png)<br>

    Üzerinde çalışırken **özellikleri** sayfasında, ayrıca kopyalayın **çalışma alanı kaynak kimliği**.  Bu işlemin bir parçası olarak yapılmaz, Log Analytics çalışma alanı daha sonra silmek istediğinize karar verirseniz, bu değer gereklidir.  

5. Bu dosyayı farklı Kaydet **OptOutParam.json** yerel bir klasöre.
6. Bu şablonu dağıtmaya hazırsınız. 

    * Şablonu içeren klasörden aşağıdaki PowerShell komutlarını kullanmak için:

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

Yalnızca küme İzleme'yi desteklemek için çalışma alanı oluşturuldu ve artık gerekli olmadığında, el ile silmeniz gerekir. Bir çalışma alanını silme konusunda bilgi sahibi değilseniz bkz [Azure portalı ile bir Azure Log Analytics çalışma alanını silme](../log-analytics/log-analytics-manage-del-workspace.md).  Hakkında unutmayın **çalışma alanı kaynak kimliği** biz 4. adımda daha önce kopyaladığınız, ihtiyacınız olacak.  

## <a name="troubleshooting"></a>Sorun giderme
Bu bölümde, kapsayıcı durumu ile ilgili sorunları gidermenize yardımcı olacak bilgiler sağlar.

Kapsayıcı durumunun başarıyla etkinleştirilmiş ve yapılandırılmış, ancak tüm durum bilgilerini veya bir günlük araması yaptığınızda Log analytics'te sonuçlar, sorunun tanılanmasına yardımcı olmak için aşağıdaki adımları gerçekleştirilemiyor görüyorsunuz değilse.   

1. Aşağıdaki komutu çalıştırarak aracının durumunu denetleyin: 

    `kubectl get ds omsagent --namespace=kube-system`

    Çıktı aşağıdaki düzgün bir şekilde dağıtmak belirten benzemelidir:

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```  
2. Aracı sürümü ile çözüm dağıtım durumunu denetlemek *06072018* veya aşağıdaki komutu çalıştırarak daha yüksek:

    `kubectl get deployment omsagent-rs -n=kube-system`

    Çıktı aşağıdaki düzgün bir şekilde dağıtmak belirten benzemelidir:

    ```
    User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
    NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
    omsagent   1         1         1            1            3h
    ```

3. Pod çalıştığını doğrulamak için aşağıdaki komutu çalıştırarak değil veya durumu kontrol edin: `kubectl get pods --namespace=kube-system`

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

4. Aracı günlüklerini denetleyin. Kapsayıcı Aracısı dağıtıldığında OMI komutları çalıştırarak hızlı bir denetim çalıştırır ve Docker sağlayıcı ve aracı sürümünü gösterir. Aracı başarıyla eklendi, görmek için aşağıdaki komutu çalıştırın: `kubectl logs omsagent-484hw --namespace=kube-system`

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

[Arama günlüklerini](../log-analytics/log-analytics-log-search.md) ayrıntılı kapsayıcı sistem durumu ve uygulama performans bilgilerini görüntülemek için.  