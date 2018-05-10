---
title: Azure Service Fabric’te Windows Kapsayıcıları için İzleme ve Tanılama | Microsoft Docs
description: Bu öğreticide, Azure Service Fabric’te düzenlenen Windows Kapsayıcıları için izleme ve tanılama özelliğini ayarlayacaksınız.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/20/2017
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: 087dafe426b835d447c69a44f6842c41a48cec8c
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="tutorial-monitor-windows-containers-on-service-fabric-using-log-analytics"></a>Öğretici: Log Analytics kullanarak Service Fabric’te Windows kapsayıcılarını izleme

Bu, öğreticinin üçüncü bölümüdür ve Log Analytics’i Service Fabric’te düzenlenen Windows kapsayıcılarınızı izleyecek şekilde ayarlamanız konusunda size yol gösterir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Service Fabric kümeniz için Log Analytics’i yapılandırma
> * Kapsayıcılarınızdaki ve düğümlerinizdeki günlükleri görüntülemek ve sorgulamak için bir Log Analytics çalışma alanı kullanma
> * OMS aracısını kapsayıcı ve düğüm ölçümlerini alacak şekilde yapılandırma

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce karşılamanız gereken ön koşullar şunlardır:
- Azure’da bir kümeye sahip olmak veya [bu öğreticide küme oluşturmak](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
- [Buna kapsayıcılı bir uygulama dağıtmak](service-fabric-host-app-in-a-container.md)

## <a name="setting-up-log-analytics-with-your-cluster-in-the-resource-manager-template"></a>Kaynak Yöneticisi şablonunda kümenizle Log Analytics’i ayarlama

Bu öğreticinin ilk bölümünde [sağlanan şablonu](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Tutorial) kullandıysanız, genel bir Service Fabric Azure Resource Manager şablonuna aşağıdaki eklemeler zaten yapılmıştır. Kendinize ait bir kümeniz varsa ve bunu Log Analytics ile kapsayıcıları izlemek üzere ayarlamak istiyorsanız:
* Resource Manager şablonunuzda aşağıdaki değişiklikleri yapın.
* [Şablonu dağıtarak](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-creation-via-arm) kümenizi yükseltmek için PowerShell ile dağıtın. Azure Resource Manager, kaynağın mevcut olduğunu algılar ve yükseltme olarak kullanıma sunar.

### <a name="adding-log-analytics-to-your-cluster-template"></a>Log Analytics’i küme şablonunuza ekleme

*template.json* dosyanızda aşağıdaki değişiklikleri yapın:

1. *Parametreler* bölümünüze Log Analytics çalışma alanı konumunu ve adını ekleyin:
    
    ```json
    "omsWorkspacename": {
      "type": "string",
      "defaultValue": "[toLower(concat('sf',uniqueString(resourceGroup().id)))]",
      "metadata": {
        "description": "Name of your Log Analytics Workspace"
      }
    },
    "omsRegion": {
      "type": "string",
      "defaultValue": "East US",
      "allowedValues": [
        "West Europe",
        "East US",
        "Southeast Asia"
      ],
      "metadata": {
        "description": "Specify the Azure Region for your Log Analytics workspace"
      }
    }
    ```

    Bunlardan birinde kullanılan değeri değiştirmek için *template.parameters.json* dosyanıza aynı parametreleri ekleyin ve orada kullanılan değerleri değiştirin.

2. Çözüm adını ve çözümü *değişkenlerinize* ekleyin: 
    
    ```json
    "omsSolutionName": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "omsSolution": "ServiceFabric"
    ```

3. OMS Microsoft Monitoring Agent’ı sanal makine uzantısı olarak ekleyin. Sanal makine ölçek kümeleri kaynağını bulun: *resources* > *"apiVersion": "[variables('vmssApiVersion')]"*. *properties* > *virtualMachineProfile* > *extensionProfile* > *extensions* yolunda, *ServiceFabricNode* uzantısının altına şu uzantı açıklamasını ekleyin: 
    
    ```json
    {
        "name": "[concat(variables('vmNodeType0Name'),'OMS')]",
        "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "workspaceId": "[reference(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')), '2015-11-01-preview').customerId]"
            },
            "protectedSettings": {
                "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')),'2015-11-01-preview').primarySharedKey]"
            }
        }
    },
    ```

4. Log Analytics çalışma alanını ayrı bir kaynak olarak ekleyin. *resources* kısmında, sanal makine ölçek kümeleri kaynağından sonra şunu ekleyin:
    
    ```json
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[parameters('omsWorkspacename')]",
        "type": "Microsoft.OperationalInsights/workspaces",
        "properties": {
            "sku": {
                "name": "Free"
            }
        },
        "resources": [
            {
                "apiVersion": "2015-11-01-preview",
                "name": "[concat(variables('applicationDiagnosticsStorageAccountName'),parameters('omsWorkspacename'))]",
                "type": "storageinsightconfigs",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]",
                    "[concat('Microsoft.Storage/storageAccounts/', variables('applicationDiagnosticsStorageAccountName'))]"
                ],
                "properties": {
                    "containers": [ ],
                    "tables": [
                        "WADServiceFabric*EventTable",
                        "WADWindowsEventLogsTable",
                        "WADETWEventTable"
                    ],
                    "storageAccount": {
                        "id": "[resourceId('Microsoft.Storage/storageaccounts/', variables('applicationDiagnosticsStorageAccountName'))]",
                        "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('applicationDiagnosticsStorageAccountName')),'2015-06-15').key1]"
                    }
                }
            },
            {
                "apiVersion": "2015-11-01-preview",
                "name": "System",
                "type": "datasources",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
                ],
                "kind": "WindowsEvent",
                "properties": {
                    "eventLogName": "System",
                    "eventTypes": [
                        {
                            "eventType": "Error"
                        },
                        {
                            "eventType": "Warning"
                        },
                        {
                            "eventType": "Information"
                        }
                    ]
                }
            }
        ]
    },
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[variables('omsSolutionName')]",
        "type": "Microsoft.OperationsManagement/solutions",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('OMSWorkspacename'))]"
        ],
        "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        },
        "plan": {
            "name": "[variables('omsSolutionName')]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('omsSolution'))]",
            "promotionCode": ""
        }
    },
    ```

[Burada](https://github.com/ChackDan/Service-Fabric/blob/master/ARM%20Templates/Tutorial/azuredeploy.json), bu değişikliklerin tümünü içeren, gerektiği şekilde başvurabileceğiniz bir örnek şablon (bu öğreticinin ilk bölümünde kullanılan) verilmiştir. Bu değişiklikler, kaynak grubunuza bir Log Analytics çalışma alanı ekler. Çalışma alanı, [Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) aracısıyla yapılandırılan depolama tablolarından Service Fabric platform olaylarını alacak şekilde yapılandırılır. OMS aracısı da (Microsoft Monitoring Agent) kümenizdeki her düğüme sanal makine uzantısı olarak eklenmiştir. Böylece siz kümenizi ölçekledikçe aracı her bir makinede otomatik olarak yapılandırılır ve aynı çalışma alanına bağlanır.

Geçerli kümenizi yükseltmek için şablonu yeni değişikliklerinizle dağıtın. Bu işlem tamamlandığında kaynak grubunuzda Log Analytics kaynaklarını görürsünüz. Küme hazır olduğunda kümeye kapsayıcılı uygulamanızı dağıtın. Sonraki adımda, kapsayıcıları izleme özelliğini ayarlayacağız.

## <a name="add-the-container-monitoring-solution-to-your-log-analytics-workspace"></a>Log Analytics çalışma alanınıza Kapsayıcı İzleme Çözümünü ekleme

Çalışma alanınızda Kapsayıcı çözümünü ayarlamak için *Kapsayıcı İzleme Çözümü* araması yapın ve Kapsayıcılar kaynağı oluşturun (İzleme ve Yönetim kategorisi altında).

![Kapsayıcılar çözümü ekleme](./media/service-fabric-tutorial-monitoring-wincontainers/containers-solution.png)

*Log Analytics Çalışma Alanı* için istem görüntülendiğinde kaynak grubunuzda oluşturulan çalışma alanını seçin ve **Oluştur**’a tıklayın. Bu, çalışma alanınıza bir *Kapsayıcı İzleme Çözümü* ekler, docker günlüklerinin ve istatistiklerinin toplanmaya başlanması için otomatik olarak OMS’nin şablon tarafından dağıtılmasını sağlar. 

*Kaynak grubunuza* geri dönün. Yeni eklenen izleme çözümünü burada görürsünüz. Bu çözüme tıklarsanız giriş sayfası, çalıştırdığınız kapsayıcı görüntülerinin sayısını size gösterir. 

*Öğreticinin [ikinci bölümünde](service-fabric-host-app-in-a-container.md) fabrikam kapsayıcımın 5 örneğini çalıştırdığıma dikkat edin*

![Kapsayıcı çözümü giriş sayfası](./media/service-fabric-tutorial-monitoring-wincontainers/solution-landing.png)

**Kapsayıcı İzleme Çözümü**’ne tıkladığınızda, birden fazla panelde gezinmenin yanı sıra Log Analytics’te sorgu çalıştırmanızı sağlayan daha ayrıntılı bir panoya yönlendirilirsiniz. 

*Eylül 2017 itibarıyla çözümde bazı güncelleştirmeler yapılacağını unutmayın. Birden fazla düzenleyiciyi aynı çözümle tümleştirme üzerinde çalıştığımız için Kubernetes olaylarıyla ilgili aldığınız hataları yoksayabilirsiniz.*

Aracı, docker günlüklerini aldığından varsayılan olarak *stdout* ve *stderr*’i gösterecek şekilde ayarlanır. Sağa kaydırdığınızda kapsayıcı görüntüsü envanterinin, durumun, ölçümlerin yanı sıra diğer faydalı verileri almak için çalıştırabileceğiniz örnek sorguları da görürsünüz. 

![Kapsayıcı çözümü panosu](./media/service-fabric-tutorial-monitoring-wincontainers/container-metrics.png)

Bu panellerden herhangi birine tıklamanız sizi görüntülenen değeri oluşturan Log Analytics sorgusuna yönlendirir. Alınmakta olan tüm farklı günlük türlerini görmek için sorguyu *\** olarak değiştirin. Buradan kapsayıcı performansını ve günlükleri sorgulayabilir, bunlar için filtreleme yapabilir veya Service Fabric platform olaylarına bakabilirsiniz. Aynı zamanda aracılarınız her düğümden sürekli olarak bir sinyal yayar. Böylece, küme yapılandırmanızın değişmesi durumunda verilerin tüm makinelerinizden toplanmaya devam ettiğinden emin olmak için bu sinyallere bakabilirsiniz.   

![Kapsayıcı sorgusu](./media/service-fabric-tutorial-monitoring-wincontainers/query-sample.png)

## <a name="configure-oms-agent-to-pick-up-performance-counters"></a>OMS aracısını performans sayaçlarını alacak şekilde yapılandırma

OMS aracısını kullanmanın diğer bir avantajı da her seferinde Azure tanılama aracısını yapılandırmak ve Resource Manager şablonuna dayalı bir yükseltme gerçekleştirmek zorunda kalmadan OMS UI deneyimi aracılığıyla almak istediğiniz performans sayaçlarını değiştirebilmenizdir. Bunu yapmak için Kapsayıcı İzleme (veya Service Fabric) çözümünüzün giriş sayfasında **OMS Portalı**’na tıklayın.

![OMS portalı](./media/service-fabric-tutorial-monitoring-wincontainers/oms-portal.png)

Bu sizi OMS portalındaki çalışma alanınıza yönlendirir. Burada çözümlerinizi görüntüleyebilir, özel panolar oluşturabilir ve OMS aracısını yapılandırabilirsiniz. 
* Ekranınızın sağ üst köşesindeki **dişli çarka** tıklayarak *Ayarlar* menüsünü açın.
* **Bağlı Kaynaklar** > **Windows Sunucuları**’na tıklayarak *5 Windows Bilgisayarı Bağlı* ifadesini gördüğünüzden emin olun.
* Yeni performans sayaçlarını aramak ve eklemek için **Veriler** > **Windows Performans Sayaçları**’na tıklayın. Burada, toplayabileceğiniz performans sayaçlarına ilişkin Log Analytics önerilerinin listesinin yanı sıra başka sayaçları arama seçeneğini de görürsünüz. Önerilen ölçümleri toplamaya başlamak için **Seçili performans sayaçlarını ekle** seçeneğine tıklayın.

    ![Performans sayaçları](./media/service-fabric-tutorial-monitoring-wincontainers/perf-counters.png)

Azure portalına geri döndüğünüzde Kapsayıcı İzleme Çözümünüzü birkaç dakika içinde **yenileyin**. Bunu yaptığınızda *Bilgisayar Performansı* verilerini görmeye başlarsınız. Bu, kaynaklarınızın nasıl kullanıldığını anlamanıza yardımcı olur. Kümenizi ölçeklendirme konusunda doğru kararlar vermek veya bir kümenin yükünüzü beklenen şekilde dengeleyip dengelemediğini doğrulamak için de bu ölçümleri kullanabilirsiniz.

*Not: Zaman filtrelerinin bu ölçümleri kullanabilmeniz için uygun şekilde ayarlandığından emin olun.* 

![Performans sayaçları 2](./media/service-fabric-tutorial-monitoring-wincontainers/perf-counters2.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Service Fabric kümeniz için Log Analytics’i yapılandırma
> * Kapsayıcılarınızdaki ve düğümlerinizdeki günlükleri görüntülemek ve sorgulamak için bir Log Analytics çalışma alanı kullanma
> * Log Analytics aracısını kapsayıcı ve düğüm ölçümlerini alacak şekilde yapılandırma

Kapsayıcılı uygulamanız için izlemeyi ayarladığınıza göre artık aşağıdaki deneyebilirsiniz:

* Yukarıdaki adımların aynısını uygulayarak bir Linux kümesi için Log Analytics’i ayarlayın. Kaynak Yöneticisi şablonunuzda değişiklik yapmak için [bu şablona](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux) başvurun.
* Algılama ve tanılama konusunda yardımcı olması için Log Analytics’i [otomatik uyarı verme](../log-analytics/log-analytics-alerts.md) özelliğini ayarlayacak şekilde yapılandırın.
* Service Fabric'in kümeleriniz için yapılandırılacak [önerilen performans sayaçları](service-fabric-diagnostics-event-generation-perf.md) listesini keşfedin.
* Log Analytics’in bir parçası olarak sunulan [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) özellikleri hakkında bilgi sahibi olun.