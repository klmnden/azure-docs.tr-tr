---
title: "İzleme ve tanılama için Windows Azure kapsayıcı hizmeti doku | Microsoft Docs"
description: "Windows Azure Service Fabric bağımsızlıklar kapsayıcısı için izleme ve tanılama ayarlayın."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/20/2017
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: 16c9926d44f972d2b38028cb6ab1420de6b60533
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-windows-containers-on-service-fabric-using-oms"></a>Service Fabric OMS kullanarak Windows kapsayıcılarında izleme

Bu üç öğreticinin bir parçasıdır ve Service Fabric bağımsızlıklar Windows kapsayıcılarınızı izlemek için OMS ayarı aracılığıyla anlatılmaktadır.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * OMS Service Fabric kümesi için yapılandırın
> * Görüntülemek ve günlükleri kapsayıcıları ve düğüm sorgulamak için bir OMS çalışma alanını kullanın
> * Kapsayıcı ve düğüm ölçümleri seçmek için OMS Aracısı'nı yapılandırma

## <a name="prerequisites"></a>Ön koşullar
Bu öğreticiye başlamadan önce aşağıdakileri yapmanız gerekir:
- Azure üzerinde bir kümeniz olduğuna veya [Bu öğretici ile oluşturun](service-fabric-tutorial-create-cluster-azure-ps.md)
- [Kapsayıcılı bir uygulamayı kümeye dağıtır](service-fabric-host-app-in-a-container.md)

## <a name="setting-up-oms-with-your-cluster-in-the-resource-manager-template"></a>Resource Manager şablonu kümenizdeki ile OMS ayarlama

Kullandığınız durumda [sağlanan şablonu](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Tutorial) Bu öğreticinin ilk bölümü genel bir Service Fabric Azure Resource Manager şablon aşağıdaki eklemeler içermelidir. Büyük/küçük harf kendi, oluşan bir küme sahip OMS ile kapsayıcıları izleme için ayarlamak için aradığınız:
* Kaynak Yöneticisi şablonunuzu aşağıdaki değişiklikleri yapın.
* Kümenizi yükseltmek için PowerShell'i kullanarak dağıtmak [şablon dağıtma](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm). Azure Resource Manager yükseltme olarak döküm, böylece, mevcut olduğundan gerçekleştirir.

### <a name="adding-oms-to-your-cluster-template"></a>Küme şablonunuzu OMS ekleme

Aşağıdaki değişiklikleri yapın, *template.json*:

1. OMS çalışma konum ekleyin ve için ad, *parametreleri* bölümü:
    
    ```json
    "omsWorkspacename": {
      "type": "string",
      "defaultValue": "[toLower(concat('sf',uniqueString(resourceGroup().id)))]",
      "metadata": {
        "description": "Name of your OMS Log Analytics Workspace"
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
        "description": "Specify the Azure Region for your OMS workspace"
      }
    }
    ```

    İçin kullanılan değer değiştirmek için aynı parametreleri ekleyin, *template.parameters.json* ve orada kullanılan değerlerle değiştirin.

2. Çözüm adı ve çözüme eklemek, *değişkenleri*: 
    
    ```json
    "omsSolutionName": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "omsSolution": "ServiceFabric"
    ```

3. OMS Microsoft İzleme Aracısı, bir sanal makine uzantısı olarak ekleyin. Sanal makine ölçek ayarlar kaynak bulun: *kaynakları* > *"apiVersion": "[variables('vmssApiVersion')]"*. Altında *özellikleri* > *virtualMachineProfile* > *extensionProfile* > *uzantıları*, altında aşağıdaki uzantı açıklaması ekleme *ServiceFabricNode* uzantısı: 
    
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

4. OMS çalışma tek tek bir kaynak olarak ekleyin. İçinde *kaynakları*, kaynak sanal makine ölçek kurmasından sonra aşağıdakileri ekleyin:
    
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

[Burada](https://github.com/ChackDan/Service-Fabric/blob/master/ARM%20Templates/Tutorial/azuredeploy.json) gerektiğinde başvurabileceğiniz bu değişiklikleri tümüne sahip bir örnek (kullanılan kısımda bu öğreticinin) şablonudur. Bu değişiklikleri bir OMS günlük analizi çalışma alanı, kaynak grubuna ekler. Çalışma alanı Service Fabric platform olayları ile yapılandırılmış depolama tablolardan seçmek için yapılandırılmış [Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) aracı. OMS Aracısı (Microsoft İzleme Aracısı) de bir sanal makine uzantısı - olarak, kümedeki her düğümde eklendi bu kümenizi ölçeklendirmenize göre aracı otomatik olarak her makinede yapılandırılan ve aynı çalışma alanına sayfaya anlamına gelir.

Geçerli kümenizi yükseltmek için yeni değişikliklerinizi şablonla dağıtın. Bu tamamlandıktan sonra kaynak grubunda OMS Kaynakları görmeniz gerekir. Küme hazır olduğunda, ona Kapsayıcılı uygulamanızı dağıtın. Sonraki adımda, biz kapsayıcıları izleme işlevini ayarlama.

## <a name="add-the-container-monitoring-solution-to-your-oms-workspace"></a>Kapsayıcı izleme çözümü, OMS çalışma alanınızla ekleyin

Çalışma alanınızda kapsayıcı çözümünü kurmak için arama *kapsayıcı izlemesi çözümü* ve kapsayıcıları kaynağı oluşturun (izleme + Yönetimi altında kategori).

![Kapsayıcıları çözüm ekleme](./media/service-fabric-tutorial-monitoring-wincontainers/containers-solution.png)

İstendiğinde *OMS çalışma*, kaynak grubunda oluşturuldu çalışma alanını seçin ve tıklatın **oluşturma**. Bu ekler bir *kapsayıcı izlemesi çözümü* sunucunuzdan çalışma alanınıza otomatik olarak docker günlükleri ve istatistikleri toplamaya başlamak şablon tarafından dağıtılan OMS Aracısı neden olur. 

Geri gidin, *kaynak grubu*, burada yeni eklenen izleme çözümü görmelisiniz. Giriş sayfası içine tıklarsanız, sahip olduğunuz kapsayıcı görüntü sayısını göstermelidir çalışıyor. 

*My fabrikam kapsayıcıdan 5 örneklerini çalıştığını unutmayın [bölüm iki](service-fabric-host-app-in-a-container.md) Öğreticisi*

![Kapsayıcı çözüm giriş sayfası](./media/service-fabric-tutorial-monitoring-wincontainers/solution-landing.png)

İçine tıklatarak **kapsayıcı monitör çözümü** yanı sıra ile birden fazla panel kaydırma günlük analizi sorguları çalıştırmak izin veren daha ayrıntılı bir Pano için yönlendirir. 

*Eylül 2017 itibariyle, çözüm aracılığıyla bazı güncelleştirmeleri işaretleneceğini Not - birden çok orchestrators aynı çözüme tümleştirme üzerinde çalışırken Kubernetes olaylar hakkında alabilirsiniz Hataları yoksay.*

Aracı docker günlüklerini çekme beri gösteren için varsayılan olarak *stdout* ve *stderr*. Sağa kaydırın, kapsayıcı görüntü envanter, durum, ölçümleri ve daha yararlı veri almak için çalıştırabilir örnek sorgular görürsünüz. 

![Kapsayıcı çözüm Panosu](./media/service-fabric-tutorial-monitoring-wincontainers/container-metrics.png)

Bu panoları hiçbirine tıklayarak görüntülenen değer oluşturmak günlük analizi sorgunun sürer. Sorgu ile değiştirmek  *\**  tüm çekilen yukarı günlükleri türlerini görmek için. Buradan, Sorgulayabileceğiniz günlükleri, kapsayıcı performans için filtre veya Service Fabric platform etkinliklerine bakın. Aracılarınızı de sürekli küme yapılandırmanızı değişirse veri hala, tüm makinelerden toplandığını emin olmak için bakabilirsiniz her düğüm, gelen sinyali yayma.   

![Kapsayıcı sorgu](./media/service-fabric-tutorial-monitoring-wincontainers/query-sample.png)

## <a name="configure-oms-agent-to-pick-up-performance-counters"></a>Performans sayaçları seçmek için OMS Aracısı'nı yapılandırma

OMS Aracısı'nı kullanmanın başka bir faydası, OMS UI deneyimi almak istediğiniz performans sayaçlarını değiştirme yeteneğini yerine Azure tanılama aracısını yapılandırmak ve kaynak yöneticisi yapmak zorunda şablon yükseltme her zaman bağlı. Bunu yapmak için tıklatın **OMS portalı** kapsayıcı izleme (veya Service Fabric) çözümünüzün giriş sayfasında.

![OMS portalı](./media/service-fabric-tutorial-monitoring-wincontainers/oms-portal.png)

Bu çözümlerinizi görüntüleyebileceğiniz alanınıza OMS portalında alın, özel panolar oluşturun yanı OMS Aracısı'nı yapılandırın. 
* Tıklayın **dişlisine Tekerlek** açık ekranın sağ üst köşesinde bulunan *ayarları* menüsü.
* Tıklayın **bağlı kaynakları** > **Windows sunucuları** yüklü olduğunu doğrulamak için *5 Windows bilgisayarları bağlı*.
* Tıklayın **veri** > **Windows performans sayaçlarını** için arama yapın ve yeni performans sayaçları ekleyin. Burada için diğer sayaçları arama seçeneği yanı sıra toplamak önerileri OMS performans sayaçları için bir listesini görürsünüz. Tıklatın **Seçili performans sayaçlarını Ekle** önerilen ölçümleri toplamaya başlamak için.

    ![Performans sayaçları](./media/service-fabric-tutorial-monitoring-wincontainers/perf-counters.png)

Azure Portalı'nda **yenileme** kapsayıcı izleme çözümünüzde birkaç dakika sonra görmeye başlamanız gerekir *bilgisayar performansı* gelen verileri. Bu, kaynaklarınızı nasıl kullanıldığını anlamanıza yardımcı olur. Bu ölçümleri, kümenizi ölçekleme konusunda uygun kararlar veya beklendiği gibi bir küme Yük Dengeleme olmadığını onaylamak için de kullanabilirsiniz.

*Not: saat filtrelerinizi bu ölçümleri kullanmak için uygun şekilde ayarlandığından emin olun.* 

![Performans sayacı 2](./media/service-fabric-tutorial-monitoring-wincontainers/perf-counters2.png)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * OMS Service Fabric kümesi için yapılandırın
> * Görüntülemek ve günlükleri kapsayıcıları ve düğüm sorgulamak için bir OMS çalışma alanını kullanın
> * Kapsayıcı ve düğüm ölçümleri seçmek için OMS Aracısı'nı yapılandırma

Kapsayıcılı uygulamanız için izleme işlevini ayarlama, aşağıdakileri deneyin:

* Yukarıdaki gibi benzer adımları izleyerek OMS Linux kümesi ayarlayın. Başvuru [bu şablonu](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux) Resource Manager şablonunuzda değişiklik yapmak için.
* Ayarlamak için OMS yapılandırma [uyarı otomatik](../log-analytics/log-analytics-alerts.md) algılama ve tanılama yardımcı olmak için.
* Service Fabric'ın listesi keşfedin [performans sayaçları önerilen](service-fabric-diagnostics-event-generation-perf.md) kümeleriniz için yapılandırılır.
* İle familiarized [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) özellikleri günlük analizi bir parçası olarak sunulan.