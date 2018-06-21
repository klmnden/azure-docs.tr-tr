---
title: İzleme Azure Kubernetes hizmet (AKS) sistem durumu (Önizleme) | Microsoft Docs
description: Bu makalede nasıl kolayca hızla barındırılan Kubernetes ortamınıza kullanımını anlamak için AKS kapsayıcı performansını gözden geçirebilirsiniz açıklanmaktadır.
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
ms.date: 06/19/2018
ms.author: magoedte
ms.openlocfilehash: 7c4294947cba72b1638e77c2dd8de1f5ee37b62a
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36286007"
---
# <a name="monitor-azure-kubernetes-service-aks-container-health-preview"></a>Azure Kubernetes hizmet (AKS) kapsayıcı (Önizleme) izlemesi

Bu makalede, ayarlama ve Azure Kubernetes hizmet (AKS) üzerinde barındırılan Kubernetes ortamlar için dağıtılmış, iş yüklerinin performansını izlemek için Azure İzleyici kapsayıcı sağlık kullanma açıklar.  Kubernetes kümenizin ve kapsayıcılarınızın izlenmesi, özellikle de birden fazla uygulama ile ölçekli olarak bir üretim kümesi çalıştırılırken kritik önem taşır.

Kapsayıcı sistem durumu toplama bellek ve işlemci ölçümleri denetleyicilerinden, düğümleri ve kapsayıcıları kullanılabilir ölçümler API aracılığıyla Kubernetes yetenekleriyle izleme performansı sağlar.  Kapsayıcı durumu etkinleştirdikten sonra bu ölçümleri otomatik olarak Linux için OMS aracısının kapsayıcılı bir sürümünü kullanarak için toplanan ve depolanan, [günlük analizi](../log-analytics/log-analytics-overview.md) çalışma.  Dahil edilen önceden tanımlanmış görünümleri residing kapsayıcı iş yükleri ve anlamaları için ne Kubernetes küme performans sağlığını etkileyen gösterilmektedir:  

* Düğüm ve kaynak performans sorunlarını tanımlamak için kendi ortalama işlemci ve bellek kullanımı çalışan hangi kapsayıcılar
* Kapsayıcı bir denetleyici ve/veya bir denetleyici veya pod genel performansı görmek için pod'ları içinde bulunduğu tanımlayın 
* Kaynak kullanımı pod destekleyen standart süreçler ilgisiz ana bilgisayarda çalışan iş yüklerinin gözden geçirin
* Kapasite gereksinimlerini tanımlamak ve onu karşılayabilir en fazla yük belirlemek yardımcı olmak için ortalama ve en yoğun yük altında küme davranışını anlama 

Docker ve Windows yönetme ve izleme de ilgileniyorsanız kapsayıcı ana görünümü yapılandırması, Denetim ve kaynak kullanımını görmek [kapsayıcı izlemesi çözümü](../log-analytics/log-analytics-containers.md).

## <a name="requirements"></a>Gereksinimler 
Desteklenen Önkoşullar anlamaları için başlatmadan önce aşağıdaki ayrıntıları gözden geçirin.

- AKS küme aşağıdaki sürümleri desteklenir: 1.7.7 için 1.9.6.
- Kapsayıcılı bir OMS Aracısı Linux sürümü için microsoft / oms:ciprod04202018 ve daha sonra. Bu aracı, kapsayıcı durumu onboarding işlemi sırasında otomatik olarak yüklenir.  
- Log Analytics çalışma alanı.  Yeni AKS kümenizi izleme etkinleştirdiğinizde ya da aracılığıyla bir tane oluşturabilirsiniz oluşturulabilir [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), veya [Azure portal](../log-analytics/log-analytics-quick-create-workspace.md).
- Kapsayıcı izleme etkinleştirmek için günlük analizi katkıda bulunan rolü üyesi.  Günlük analizi çalışma alanı erişimi denetleme hakkında daha fazla bilgi için bkz: [çalışma alanlarını yönet](../log-analytics/log-analytics-manage-access.md).

## <a name="components"></a>Bileşenler 

Bu özellik bir kapsayıcılı OMS Aracısı kümedeki tüm düğümlerin performans ve olay verilerini toplamak Linux için kullanır.  Aracıyı otomatik olarak dağıtılan ve kapsayıcı izleme etkinleştirdikten sonra belirtilen günlük analizi çalışma alanı ile kayıtlı. 

>[!NOTE] 
>AKS küme zaten dağıttıysanız, bu makalenin sonraki bölümlerinde gösterildiği gibi sağlanan bir Azure Resource Manager şablonu kullanarak izlemeyi etkinleştirin. Kullanamazsınız `kubectl` yükseltmek için silin, yeniden dağıtın veya aracıyı dağıtın.  
>

## <a name="sign-in-to-azure-portal"></a>Azure Portalında oturum açın
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın. 

## <a name="enable-container-health-monitoring-for-a-new-cluster"></a>Kapsayıcı, yeni küme için sistem durumu izlemeyi etkinleştir
Yalnızca Azure portalından dağıttığınızda AKS kümenizi izleme de etkinleştirebilirsiniz.  Hızlı Başlangıç makalesindeki adımları [bir Azure Kubernetes hizmet (AKS) kümeyi dağıtma](../aks/kubernetes-walkthrough-portal.md).  Üzerinde olduğunda **izleme** sayfasında, **Evet** seçeneği için **izlemesini etkinleştir** etkinleştirmek, mevcut bir seçin veya yeni bir günlük analizi çalışma alanı oluşturmak için.  

İzleme etkinleştirildikten sonra tüm yapılandırma görevlerini başarıyla tamamlandı, iki yoldan biriyle kümenizden performansını izleyebilirsiniz:

1. Seçerek doğrudan AKS kümeden **sistem durumu** sol bölmeden.<br><br> 
2. Tıklayarak **kapsayıcı sistem durumu izleme** döşeme AKS küme sayfasında seçilen küme için.  Azure İzleyicisi'nde seçin **sistem durumu** sol bölmeden.  

![Kapsayıcı durumu içinde AKS seçmek için seçenekleri](./media/monitoring-container-health/container-performance-and-health-select-01.png)

İzleme etkinleştirildikten sonra küme için işletimsel veri görebilmek için önce yaklaşık 15 dakika sürebilir.  

## <a name="enable-container-health-monitoring-for-existing-managed-clusters"></a>Kapsayıcı mevcut yönetilen kümeleri için sistem durumu izlemeyi etkinleştir
Zaten dağıtılmış AKS kapsayıcının izlemeyi etkinleştirme gerçekleştirilebilir Azure portalından veya PowerShell cmdlet'ini kullanarak sağlanan Azure Resource Manager şablonu ile **New-AzureRmResourceGroupDeployment** veya Azure CLI.  


### <a name="enable-from-azure-portal"></a>Azure portalından etkinleştirme
Azure portalından, AKS kapsayıcısının izlemeyi etkinleştirmek için aşağıdaki adımları gerçekleştirin.

1. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde yazın **kapsayıcıları**. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Seçin **Kubernetes Hizmetleri**.<br><br> ![Azure portal](./media/monitoring-container-health/azure-portal-01.png)<br><br>  
2. Kapsayıcılar, listeden bir kapsayıcı seçin.
3. Kapsayıcı genel bakış sayfasında seçin **kapsayıcı sistem durumu izleme** ve **kapsayıcı sistem durumu ve günlükleri için hazırlanma** sayfası görüntülenir.
4. Üzerinde **kapsayıcı sistem durumu ve günlükleri için hazırlanma** sayfasında, varolan bir günlük analizi varsa çalışma kümesi ile aynı abonelikte, aşağı açılan listeden seçin.  Listeden varsayılan çalışma alanını belirler ve konum AKS kapsayıcı abonelikte dağıtılmış. Ya da seçebilirsiniz **Yeni Oluştur** ve yeni bir çalışma alanı aynı abonelikte belirtin.<br><br> ![AKS kapsayıcı sistem durumu izlemeyi etkinleştir](./media/monitoring-container-health/container-health-enable-brownfield.png) 

    Seçerseniz **Yeni Oluştur**, **yeni çalışma alanı oluşturma** bölmesinde görünür. **Bölge** bölgeye varsayılan kapsayıcı kaynağınız oluşturulur ve varsayılan kabul edin veya farklı bir bölge seçin ve çalışma alanı için bir ad belirtin.  Tıklatın **oluşturma** seçiminizi kabul etmek için.<br><br> ![Kapsayıcı monintoring için çalışma tanımlayın](./media/monitoring-container-health/create-new-workspace-01.png)  

    >[!NOTE]
    >Batı Orta ABD bölgede yeni bir çalışma alanı oluşturamazsınız şu anda bu bölgede önceden var olan bir çalışma alanı yalnızca seçebilirsiniz.  Bu bölge listeden seçebilirsiniz olsa bile, dağıtım başlatılır, ancak daha sonra kısa bir süre içinde başarısız.  
    >
 
İzleme etkinleştirildikten sonra küme için işletimsel veri görebilmek için önce yaklaşık 15 dakika sürebilir. 

### <a name="enable-using-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak etkinleştirin
Bu yöntem iki JSON şablonları içerir, tek bir şablonda izlemeyi etkinleştirmek için yapılandırma belirtir ve bir JSON şablonu aşağıdakileri belirtmek için yapılandırdığınız parametre değerleri içerir:

* AKS kapsayıcı kaynak kimliği 
* Küme kaynak grubu içinde dağıtılan 
* Günlük analizi çalışma alanı ve çalışma oluşturmak için bölge 

Günlük analizi çalışma alanı el ile oluşturulması gerekir.  Çalışma alanı oluşturmak için bir aracılığıyla ayarlayabilirsiniz [Azure Resource Manager](../log-analytics/log-analytics-template-workspace-configuration.md), [PowerShell](https://docs.microsoft.com/azure/log-analytics/scripts/log-analytics-powershell-sample-create-workspace?toc=%2fpowershell%2fmodule%2ftoc.json), gelen [Azure portal](../log-analytics/log-analytics-quick-create-workspace.md).

PowerShell ile bir şablon kullanarak kaynakları dağıtma kavramları hakkında bilgi sahibi değilseniz bkz [Resource Manager şablonları ve Azure PowerShell ile kaynakları dağıtmak](../azure-resource-manager/resource-group-template-deploy.md)veya Azure CLI bakın [kaynaklarla dağıtma Resource Manager şablonları ve Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).

Azure CLI kullanmayı seçerseniz, önce yükleyip CLI yerel olarak kullanmak gerekir.  Gerekli olan Azure CLI Sürüm 2.0.27 çalıştırdığınız veya sonraki bir sürümü. Çalıştırma `az --version` sürümünü belirlemek için. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). 

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

2. Bu dosya olarak kaydetmek **existingClusterOnboarding.json** bir yerel klasöre.
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

4. Değeri düzenlemek **aksResourceId**, **aksResourceLocation** üzerinde bulabilirsiniz değerlerle **AKS genel bakış** AKS kümesi için sayfa.  Değeri **workspaceResourceId** çalışma alanı adı içerir, günlük analizi çalışma alanı tam kaynak kimliğidir.  Ayrıca için çalışma zamanı konumu belirtin **workspaceRegion**.    
5. Bu dosya olarak kaydetmek **existingClusterParam.json** bir yerel klasöre.
6. Bu şablonu dağıtmaya hazırsınız. 

    * Şablonu içeren klasöründen aşağıdaki PowerShell komutlarını kullanın:

        ```powershell
        New-AzureRmResourceGroupDeployment -Name OnboardCluster -ResourceGroupName ClusterResourceGroupName -TemplateFile .\existingClusterOnboarding.json -TemplateParameterFile .\existingClusterParam.json
        ```
        Yapılandırma değişikliği tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonucu içeren aşağıdakine benzer bir ileti görürsünüz:

        ```powershell
        provisioningState       : Succeeded
        ```

    * Linux üzerinde Azure CLI ile komutu çalıştırmak için:
    
        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --resource-group <ResourceGroupName> --template-file ./existingClusterOnboarding.json --parameters @./existingClusterParam.json
        ```

        Yapılandırma değişikliği tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonucu içeren aşağıdakine benzer bir ileti görürsünüz:

        ```azurecli
        provisioningState       : Succeeded
        ```
İzleme etkinleştirildikten sonra küme için işletimsel veri görebilmek için önce yaklaşık 15 dakika sürebilir.  

## <a name="verify-agent-deployed-successfully"></a>Aracı başarıyla dağıtıldığından emin olun
Düzgün şekilde dağıtıldığından OMS Aracısı doğrulamak için aşağıdaki komutu çalıştırın: ` kubectl get ds omsagent --namespace=kube-system`.

Çıktı aşağıdaki düzgün bir şekilde dağıtın belirten benzemelidir:

```
User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
```  

## <a name="view-performance-utilization"></a>Görünüm performans kullanımı
Kapsayıcı durumu açtığınızda, sayfa hemen Küme düğümlerinizi performans kullanımını gösterir.  AKS kümenizi hakkındaki bilgileri görüntüleme üç Perspektifler düzenlenmiştir:

- Düğümler 
- Denetleyiciler  
- Kapsayıcılar

Satır hiyerarşi, kümedeki bir düğüm başlayarak Kubernetes nesne modelini izler.  Düğümünü genişletin ve düğüm üzerinde çalışan bir veya daha fazla pod'ları görmek ve bir pod gruplandırılmış birden fazla kapsayıcı varsa, bunlar hiyerarşideki son satır olarak gösterilir.<br><br> ![Örnek Kubernetes düğümü hiyerarşisinde performans görünümü](./media/monitoring-container-health/container-performance-and-health-view-03.png)

Sayfanın üst kısmından denetleyicileri veya kapsayıcılarını seçin ve bu nesneler için durumu ve kaynak kullanımını inceleyin.  Ad alanı, hizmet ve düğüm göre filtre uygulamak için ekranın üstünde açılır kutularını kullanın. Bunun yerine gelen bellek kullanımı, gözden geçirmek istiyorsanız **ölçüm** aşağı açılan listesinde seçin **bellek RSS** veya **bellek çalışma kümesi**.  **Bellek RSS** yalnızca Kubernetes 1.8 ve sonraki sürümleri için desteklenir. Aksi takdirde değerlerini görmek **ortalama %** olarak gösteren *NaN %*, hangi tanımlanmamış veya unrepresentable bir değeri temsil eden bir sayısal veri türü değeri. 

![Kapsayıcı performans düğümleri performans görünümü](./media/monitoring-container-health/container-performance-and-health-view-04.png)

Varsayılan olarak, performans verileri son altı saatleri temel alır ancak penceresiyle değiştirebileceğiniz **zaman aralığı** açılan listesinden, sayfanın sağ üst köşesinde bulundu. Şu anda sayfa değil otomatik yenileme, bu nedenle el ile yenilemeniz gerekir. 

Aşağıdaki örnekte, düğüm için fark *aks-agentpool-3402399-0*, değeri **kapsayıcıları** kapsayıcıları dağıtılan toplam sayısı toplamını olduğu 10 ' dur.<br><br> ![Düğüm örnek başına kapsayıcıların dökümü](./media/monitoring-container-health/container-performance-and-health-view-07.png)<br><br> Kapsayıcılar, küme düğümleri arasında uygun dengesi yoksa hızla tanımlamanıza yardımcı olabilir.  

Aşağıdaki tabloda düğümleri görüntülediğinizde sunulan bilgiler açıklanmaktadır.

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Ana bilgisayar adı |
| Durum | Düğüm durumu Kubernetes görünümü |
| ORTALAMA % | Düğüm yüzde üzerinde seçili ölçüm için seçilen süre tabanlı ortalama. |
| ORTALAMA | Seçilen süre için ölçüm seçili gerçek değer göre ortalama düğüm.  Ortalama değer için bir düğüm CPU/bellek sınırını ölçülür; pod'ları ve kapsayıcıları ana bilgisayar tarafından bildirilen ortalama değer içindir. |
| Kapsayıcılar | Kapsayıcı sayısı. |
| Çalışma Süresi | Bir düğüm başlatıldığında veya yeniden başlatıldıktan sonra süresini temsil eder. |
| Pod | Yalnızca kapsayıcıları için. Bu, bulunan pods gösterir. |
| Denetleyiciler | Yalnızca kapsayıcıları ve pod'ları için. Hangi denetleyicisi, bulunan gösterir. Tüm pod'ları bir denetleyici olacağından bazı yok gösterebilir. | 
| Eğilim ortalama % | Kapsayıcı ve düğüm avg ölçüm dayalı çubuk grafik eğilim. |


Seçici seçin **denetleyicileri**.<br><br> ![Select denetleyicileri görünümü](./media/monitoring-container-health/container-performance-and-health-view-08.png)

Burada denetleyicilerinizi performans durumunu görebilirsiniz.<br><br> ![< ad > denetleyicileri performans görünümü](./media/monitoring-container-health/container-performance-and-health-view-05.png)

Satır hiyerarşi denetleyicisi ile başlar ve denetleyici genişletir ve bir veya daha fazla pod'ları ya da bir veya daha fazla kapsayıcıları görebilirsiniz.  Bir pod'ni genişletin ve son satırını pod gruplandırılmış kapsayıcı gösterir.  

Aşağıdaki tabloda denetleyicileri görüntülediğinizde sunulan bilgiler açıklanmaktadır.

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Denetleyicinin adı|
| Durum | Çalışma durumu ile gibi tamamlandığında kapsayıcıları durumunu *kesildi*, *başarısız* *durduruldu*, veya *duraklatıldı*. Kapsayıcı çalışıyor, ancak ya da düzgün sunulan veya aracı tarafından çekilmiş değildir ve 30 dakikadan fazla yanıt vermedi durumu oluştu, durum olacaktır *bilinmeyen*. |
| ORTALAMA % | Her varlığın seçili ölçümü için ortalama % ortalaması alma. |
| ORTALAMA | Ortalama CPU millicore veya bellek performansını kapsayıcısının biriken.  Ortalama değer pod için CPU/bellek sınırını ölçülür. |
| Kapsayıcılar | Denetleyici veya pod kapsayıcıları toplam sayısı. |
| Yeniden başlatma | Yeniden başlatma sayısını kapsayıcılardan biriken. |
| Çalışma Süresi | Bir kapsayıcı başlatıldığından bu yana süresini temsil eder. |
| Pod | Yalnızca kapsayıcıları için. Bu, bulunan pods gösterir. |
| Node | Yalnızca kapsayıcıları ve pod'ları için. Hangi denetleyicisi, bulunan gösterir. | 
| Eğilim ortalama % | Çubuk grafik eğilim kapsayıcısının ortalama ölçüm % sunma. |

Seçici seçin **kapsayıcıları**.<br><br> ![Select kapsayıcıları görüntüleyin](./media/monitoring-container-health/container-performance-and-health-view-09.png)

Kapsayıcılarınızı performans durumunu burada görebiliriz.<br><br> ![< ad > denetleyicileri performans görünümü](./media/monitoring-container-health/container-performance-and-health-view-06.png)

Aşağıdaki tabloda kapsayıcıları görüntülediğinizde sunulan bilgiler açıklanmaktadır.

| Sütun | Açıklama | 
|--------|-------------|
| Ad | Denetleyicinin adı|
| Durum | Kapsayıcıların durumu varsa alma. |
| ORTALAMA % | Her varlığın seçili ölçümü için ortalama % ortalaması alma. |
| ORTALAMA | Ortalama CPU millicore veya bellek performansını kapsayıcısının biriken. Ortalama değer pod için CPU/bellek sınırını ölçülür. |
| Kapsayıcılar | Denetleyici için kapsayıcı toplam sayısı.|
| Yeniden başlatma | Bir kapsayıcı başlatıldığından bu yana süresini temsil eder. |
| Çalışma Süresi | Bir kapsayıcı başlatıldı veya yeniden olduğundan süresini temsil eder. |
| Pod | Bu bilgi pod. |
| Node |  Kapsayıcı bulunduğu düğümü.  | 
| Eğilim ortalama % | Çubuk grafik eğilim kapsayıcısının ortalama ölçüm % sunma. |

## <a name="container-data-collection-details"></a>Kapsayıcı veri toplama ayrıntıları
Kapsayıcı durumu kapsayıcı konakları ve kapsayıcıları çeşitli performans ölçümleri ve günlük verilerini toplar. Verileri üç dakikada toplanır.

### <a name="container-records"></a>Kapsayıcı kayıtlarını

Aşağıdaki tablo, kapsayıcı durumu ve günlük arama sonuçlarında görünecek veri türleri tarafından toplanan kayıtları örnekleri gösterir.

| Veri türü | Günlük arama veri türü | Alanlar |
| --- | --- | --- |
| Konaklar ve kapsayıcıları için performans | `Perf` | Bilgisayar, ObjectName, CounterName &#40;% işlemci zamanı, Disk okuma MB, MB, bellek kullanımı MB Disk Yazar ağ bayt alma, ağ gönderme bayt, işlemci kullanımı sn, ağ&#41;, CounterValue, TimeGenerated, sayaç yolu, SourceSystem |
| Kapsayıcı envanteri | `ContainerInventory` | TimeGenerated, bilgisayar, ContainerHostname, görüntü, ImageTag, ContainerState, ExitCode, EnvironmentVar, komutu, CreatedTime, StartedTime, FinishedTime, SourceSystem, Containerıd, ImageID kapsayıcı adı |
| Kapsayıcı görüntü envanteri | `ContainerImageInventory` | TimeGenerated, bilgisayar, görüntü, ImageTag, ImageSize, VirtualSize, duraklatıldı, çalışıyor, durduruldu, başarısız, SourceSystem, ImageID, TotalContainer |
| Kapsayıcı günlük | `ContainerLog` | TimeGenerated, bilgisayar, görüntü kimliği, LogEntrySource, LogEntry, SourceSystem, Containerıd kapsayıcı adı |
| Kapsayıcı hizmeti oturum açma | `ContainerServiceLog`  | TimeGenerated, bilgisayar, TimeOfCommand, görüntü, komutu, SourceSystem, Containerıd |
| Kapsayıcı düğümü envanteri | `ContainerNodeInventory_CL`| TimeGenerated, bilgisayar, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, SourceSystem|
| Kapsayıcı işlemi | `ContainerProcess_CL` | TimeGenerated, bilgisayar, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, SourceSystem |
| Pod'ları Kubernetes kümedeki sayımı | `KubePodInventory` | TimeGenerated, bilgisayar, ClusterId, ContainerCreationTimeStamp, PodUid, PodCreationTimeStamp, ContainerRestartCount, PodRestartCount, PodStartTime, ContainerStartTime, ServiceName, ControllerKind, controllername öğesi, ContainerStatus, Containerıd, kapsayıcı adı, ad, PodLabel, Namespace, PodStatus, ClusterName, PodIp, SourceSystem |
| Kubernetes küme düğümleri parçası sayımı | `KubeNodeInventory` | TimeGenerated, bilgisayar, ClusterName, ClusterId, LastTransitionTimeReady, etiketler, durum, KubeletVersion, KubeProxyVersion, CreationTimeStamp, SourceSystem | 
| Kubernetes olayları | `KubeEvents_CL` | TimeGenerated, bilgisayar, ClusterId_s, FirstSeen_t, LastSeen_t, Count_d, ObjectKind_s, Namespace_s, Name_s, Reason_s, Type_s, TimeGenerated_s, SourceComponent_s, ClusterName_s, ileti, SourceSystem | 
| Kubernetes Küme Hizmetleri | `KubeServices_CL` | TimeGenerated, ServiceName_s, Namespace_s, SelectorLabels_s, ClusterId_s, ClusterName_s, ClusterIP_s, ServiceType_s, SourceSystem | 
| Kubernetes küme düğümleri parçası için performans ölçümleri | Perf &#124; burada ObjectName "K8SNode" == | Bilgisayar, ObjectName, CounterName &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, networkRxBytes, networkTxBytes, restartTimeEpoch, networkRxBytesPerSec, networkTxBytesPerSec, cpuAllocatableNanoCores, memoryAllocatableBytes, cpuCapacityNanoCores, memoryCapacityBytes&#41;, CounterValue, TimeGenerated, sayaç yolu, SourceSystem | 
| Kapsayıcıları Kubernetes kümesinin parçası için performans ölçümleri | Perf &#124; burada ObjectName "K8SContainer" == | CounterName &#40;cpuUsageNanoCores, memoryWorkingSetBytes, memoryRssBytes, restartTimeEpoch, cpuRequestNanoCores, memoryRequestBytes, cpuLimitNanoCores, memoryLimitBytes&#41;, CounterValue, TimeGenerated, sayaç yolu, SourceSystem | 

## <a name="search-logs-to-analyze-data"></a>Verileri çözümlemek için arama günlükleri
Günlük analizi için eğilimleri arayın, performans sorunlarını tanılamanıza yardımcı olabilir, yardımcı olabilecek tahmin correlate verileri veya geçerli küme yapılandırmasını en iyi durumda olup olmadığını belirleyin.  Önceden tanımlanmış günlük aramaları hemen kullanmaya başlamak için ya da istediğiniz gibi bilgileri döndürmek için özelleştirmek için sağlanır. 

Seçerek etkileşimli veri analizi çalışma alanında gerçekleştirebileceğiniz **Günlüğü Görüntüle** seçeneği, bir kapsayıcı genişlettiğinizde sağdaki üzerinde kullanılabilir.  **Arama oturum** sayfası sağ olduğunuz şirket Portalı'nda sayfanın üstünde görünür.<br><br> ![Günlük analizi veri çözümleme](./media/monitoring-container-health/container-performance-and-health-view-logs-01.png)   

Günlük analizi için iletilen kapsayıcı günlükleri çıktısı STDOUT ve STDERR alınır. Azure yönetilen Kubernetes (AKS) kapsayıcı durumunu izleme çünkü Kube sistem büyük oluşturulan veri hacmi nedeniyle bugün toplanmaz.     

### <a name="example-log-search-queries"></a>Örnek günlük arama sorguları
Genellikle, bir örnek veya iki ile başlayan ve bunları gereksinimlerinizi karşılayacak şekilde değiştirme sorgular oluşturmak kullanışlıdır. Daha gelişmiş sorgular oluşturmanıza yardımcı olmak için aşağıdaki örnek sorgularda ile deneyebilirsiniz.

| Sorgu | Açıklama | 
|-------|-------------|
| ContainerInventory<br> &#124;Bilgisayar, ad, resim, ImageTag, ContainerState, CreatedTime, StartedTime, FinishedTime proje<br> &#124;Tablo oluşturma | Tüm kapsayıcının yaşam döngüsü bilgi listesi| 
| KubeEvents_CL<br> &#124;Burada not(isempty(Namespace_s))<br> &#124;TimeGenerated desc sıralama<br> &#124;Tablo oluşturma | Kubernetes olayları|
| ContainerImageInventory<br> &#124;AggregatedValue özetlemek count() görüntü, ImageTag, tarafından çalışan = | Görüntü envanteri | 
| **Gelişmiş analizleri, çizgi grafiklerde seçin**:<br> Perf<br> &#124;Burada ObjectName "Kapsayıcı" ve CounterName == "% işlemci zamanı" ==<br> &#124;AvgCPUPercent özetlemek bin (TimeGenerated, 30 m), InstanceName tarafından avg(CounterValue) = | Kapsayıcı CPU | 
| **Gelişmiş analizleri, çizgi grafiklerde seçin**:<br> Perf &#124; burada ObjectName "Kapsayıcı" ve CounterName == "Bellek kullanımı MB" ==<br> &#124;AvgUsedMemory özetlemek bin (TimeGenerated, 30 m), InstanceName tarafından avg(CounterValue) = | Kapsayıcı bellek |

## <a name="how-to-stop-monitoring-with-container-health"></a>Kapsayıcı sistem durumu izleme durdurma
AKS kapsayıcısı'nın izleme etkinleştirdikten sonra artık izlemek istiyorsanız karar, yapabilecekleriniz *çıkma* sağlanan Azure Resource Manager şablonları ile PowerShell cmdlet'ini kullanarak  **Yeni-AzureRmResourceGroupDeployment** veya Azure CLI.  Bir JSON şablonu belirtir yapılandırmaya *çıkma* ve diğer JSON şablonunu yapılandırdığınız küme dağıtılmıştır AKS küme kaynağı kimliği ve kaynak grubunu belirlemek için parametre değerlerini içerir.  PowerShell ile bir şablon kullanarak kaynakları dağıtma kavramları hakkında bilgi sahibi değilseniz bkz [Resource Manager şablonları ve Azure PowerShell ile kaynakları dağıtmak](../azure-resource-manager/resource-group-template-deploy.md) veya Azure CLI bakın [kaynaklarla dağıtma Resource Manager şablonları ve Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).

Azure CLI kullanmayı seçerseniz, önce yükleyip CLI yerel olarak kullanmak gerekir.  Gerekli olan Azure CLI Sürüm 2.0.27 çalıştırdığınız veya sonraki bir sürümü. Çalıştırma `az --version` sürümünü belirlemek için. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli). 

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

2. Bu dosya olarak kaydetmek **OptOutTemplate.json** bir yerel klasöre.
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

4. Değeri düzenlemek **aksResourceId** ve **aksResourceLocation** üzerinde bulabilirsiniz AKS kümesinin değerlerle **özellikleri** seçilen küme için sayfa.<br><br> ![Kapsayıcı Özellikleri Sayfası](./media/monitoring-container-health/container-properties-page.png)<br>

    Üzerinde çalışırken **özellikleri** sayfasında, ayrıca kopyalamak **çalışma kaynak kimliği**.  Bu işlemin bir parçası olarak gerçekleştirilmez günlük analizi çalışma daha sonra silmek istediğiniz karar verirseniz bu gerekli bir değerdir.  

5. Bu dosya olarak kaydetmek **OptOutParam.json** bir yerel klasöre.
6. Bu şablonu dağıtmaya hazırsınız. 

    * Şablonu içeren klasöründen aşağıdaki PowerShell komutlarını kullanmak için:

        ```powershell
        Connect-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionName <yourSubscriptionName>
        New-AzureRmResourceGroupDeployment -Name opt-out -ResourceGroupName <ResourceGroupName> -TemplateFile .\OptOutTemplate.json -TemplateParameterFile .\OptOutParam.json
        ```

        Yapılandırma değişikliği tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonucu içeren aşağıdakine benzer bir ileti döndürdü:

        ```powershell
        ProvisioningState       : Succeeded
        ```

    * Linux üzerinde Azure CLI ile komutu çalıştırmak için:

        ```azurecli
        az login   
        az account set --subscription "Subscription Name" 
        az group deployment create --resource-group <ResourceGroupName> --template-file ./OptOutTemplate.json --parameters @./OptOutParam.json  
        ```

        Yapılandırma değişikliği tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonucu içeren aşağıdakine benzer bir ileti döndürdü:

        ```azurecli
        ProvisioningState       : Succeeded
        ```

Yalnızca küme izlemeyi desteklemek için çalışma alanı oluşturuldu ve artık gerekli olmadığında, el ile silmeniz gerekir. Çalışma alanını silmek konusunda bilgi sahibi değilseniz bkz [Azure portal ile Azure günlük analizi çalışma silme](../log-analytics/log-analytics-manage-del-workspace.md).  İlgili unutmayın **çalışma kaynak kimliği** biz 4. adımda daha önce kopyaladığınız, ihtiyacınız olacak.  

## <a name="troubleshooting"></a>Sorun giderme
Bu bölümde, kapsayıcı durumu ile ilgili sorunları gidermenize yardımcı olacak bilgiler sağlar.

Kapsayıcı durumu başarıyla etkin ve yapılandırılmış ancak durum bilgileri veya günlük bir günlük arama yaparken analizi sonuçlarında, sorunu tanılamalarına yardımcı olmak için aşağıdaki adımları gerçekleştirilemiyor görmekten.   

1. Aşağıdaki komutu çalıştırarak aracının durumunu denetleyin: `kubectl get ds omsagent --namespace=kube-system`

    Çıktı aşağıdaki aracı kümedeki tüm düğümde çalışan belirten benzemelidir.  Örneğin, bu küme iki düğüme sahip ve eşit sayıda düğüm değerine beklemelisiniz.  

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```
2. Çalıştığını doğrulamak için pod veya aşağıdaki komutu çalıştırarak değil durumu kontrol edin: `kubectl get pods --namespace=kube-system`

    Çıktı aşağıdaki durumuyla benzemelidir *çalıştıran* omsagent için:

    ```
    User@aksuser:~$ kubectl get pods --namespace=kube-system 
    NAME                                READY     STATUS    RESTARTS   AGE 
    aks-ssh-139866255-5n7k5             1/1       Running   0          8d 
    azure-vote-back-4149398501-7skz0    1/1       Running   0          22d 
    azure-vote-front-3826909965-30n62   1/1       Running   0          22d 
    omsagent-484hw                      1/1       Running   0          1d 
    omsagent-fkq7g                      1/1       Running   0          1d 
    ```

3. Aracı günlüklerini denetleyin. Kapsayıcılı Aracısı dağıtıldığında OMI komutlarını çalıştırarak hızlı bir denetim çalıştırır ve Docker sağlayıcı ve aracı sürümünü gösterir. Aracı edildi başarıyla verildi görmek için aşağıdaki komutu çalıştırın: `kubectl logs omsagent-484hw --namespace=kube-system`

    Durum aşağıdakine benzemelidir:

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

[Arama günlüklerini](../log-analytics/log-analytics-log-search.md) ayrıntılı kapsayıcı sistem ve uygulama performans bilgilerini görüntülemek için.  
