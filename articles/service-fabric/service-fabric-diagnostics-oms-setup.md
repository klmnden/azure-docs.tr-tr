---
title: "Azure Service Fabric - OMS günlük analizi ile izleme ayarlama | Microsoft Docs"
description: "Azure Service Fabric kümeleri izleme olaylarını çözümleme ve görselleştirme için OMS ayarlamak öğrenin."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/31/2017
ms.author: dekapur
ms.openlocfilehash: 32c09f06cea97024437e7e339407d344194a14ae
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="set-up-oms-log-analytics-for-a-cluster"></a>Bir küme için OMS günlük analizi Ayarla

Bir OMS çalışma aracılığıyla Azure Resource Manager veya Azure Marketi'nden ayarlayabilirsiniz. Dağıtımınızın gelecekte kullanım için bir şablon korumak istediğinizde önceki kullanın. Market dağıtma etkin Tanılama ile dağıtılan bir kümeye zaten sahipseniz daha kolay olur.

> [!NOTE]
> Küme görüntülemek için tanılama kümeniz için etkinleştirilmiş olması gerekir / kümeniz için OMS başarılı bir şekilde ayarlamak için platform düzeyi olaylarını izleme.

## <a name="deploying-oms-using-a-resource-manager-template"></a>Resource Manager şablonu kullanarak OMS dağıtma

Resource Manager şablonu kullanarak bir küme dağıtırken, şablonun yeni bir OMS çalışma alanı oluşturmanız gerekir hizmeti yapı çözümü için ekleyip uygun depolama tablolarından verileri okumak için yapılandırabilirsiniz.

[Burada](https://azure.microsoft.com/resources/templates/service-fabric-oms/) gereksinimlerine uygun şekilde değiştirin ve kullanan bir örnek şablonudur. Bir OMS çalışma ayarı bulunabilir için farklı seçenekler size daha fazla şablonları [Service Fabric ve OMS şablonları](https://azure.microsoft.com/resources/templates/?term=service+fabric+OMS).

Ana değişikliklerinin şunlardır:

1. Ekleme `omsWorkspaceName` ve `omsRegion` parametrelerinizi için. Bu tanımlanan parametreler için aşağıdaki kod parçacığında ekleme anlamına gelir, *template.json* dosya. Uygun gördüğünüz şekilde varsayılan değerleri değiştirmek çekinmeyin. İki yeni parametre de eklemeniz gerekir, *parameters.json* kaynak dağıtımı için değerleri tanımlamak için:
    
    ```json
    "omsWorkspacename": {
        "type": "string",
        "defaultValue": "sfomsworkspace",
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

    `omsRegion` Değerlere sahip değerleri belirli bir dizi uymak. Küme dağıtımı için en yakın olan seçmeniz gerekir.

2. Tüm uygulama günlükleri için OMS göndereceği, onaylayın `applicationDiagnosticsStorageAccountType` ve `applicationDiagnosticsStorageAccountName` şablonunuzdaki parametreleri olarak dahil edilir. Değilse, bunları değişkenleri bölümüne ekleyin sözlüğüdür ve bunların değerleri gerektiği gibi düzenleyin. Yukarıda kullanılan biçim aşağıdaki isterseniz bunları parametre olarak dahil edebilirsiniz.

    ```json
    "applicationDiagnosticsStorageAccountType": "Standard_LRS",
    "applicationDiagnosticsStorageAccountName": "[toLower(concat('oms', uniqueString(resourceGroup().id), '3' ))]",
    ```

3. Service Fabric OMS çözümü için şablonun değişkenleri ekleyin:

    ```json
    "solution": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "solutionName": "ServiceFabric",
    ```

4. Aşağıdaki Service Fabric küme kaynağı burada bildirilmiş sonra kaynakları bölümünüzü sonuna ekleniyor.

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
            }
        ]
    },
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[variables('solution')]",
        "type": "Microsoft.OperationsManagement/solutions",
        "id": "[resourceId('Microsoft.OperationsManagement/solutions/', variables('solution'))]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('OMSWorkspacename'))]"
        ],
        "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        },
        "plan": {
            "name": "[variables('solution')]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('solutionName'))]",
            "promotionCode": ""
        }
    }
    ```

5. Şablon kümenizi Resource Manager yükseltme olarak dağıtın. Bu yapılır kullanarak `New-AzureRmResourceGroupDeployment` AzureRM PowerShell modülündeki API. Bir örnek komut şöyle olacaktır:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName "sfcluster1" -TemplateFile "<path>\template.json" -TemplateParameterFile "<path>\parameters.json"
    ``` 

    Azure Resource Manager bunu mevcut bir kaynağı için bir güncelleştirme olduğunu algılayabilir olacaktır. Ayrıca, varolan dağıtım yürüten şablonu ve sağlanan yeni şablonu arasındaki değişiklikleri yalnızca işleyecektir.

## <a name="deploying-oms-using-azure-marketplace"></a>Azure Market kullanarak OMS dağıtma

Bir küme dağıttıktan sonra bir OMS çalışma eklemek isterseniz, head üzerinden Azure Marketi'ndeki (Portal) için ve Ara *"Service Fabric Analytics"*.

1. Tıklayın **yeni** sol gezinti menüsünde. 

2. Arama *Service Fabric Analytics*. Görüntülenir kaynak'ı tıklatın.

3. Tıklayın **oluşturma**

    ![OMS BT analizi pazarında](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. Service Fabric Analytics oluşturma penceresinde **bir çalışma alanı seçin** için *OMS çalışma* alan ve ardından **yeni bir çalışma alanı oluşturma**. Gerekli girişleri doldurun - burada tek gereksinim abonelik Service Fabric kümesi ve OMS çalışma alanı için aynı olmalıdır. Girişlerinizi doğruladıktan sonra OMS çalışma dağıtmaya başlar. Bu, yalnızca birkaç dakika sürer.

5. Tamamlandığında tıklatarak **oluşturma** Service Fabric Analytics oluşturma penceresinin altındaki yeniden. Yeni bir çalışma alanı altında gösterildiğini doğrulayın *OMS çalışma*. Bu çözüm, yeni oluşturduğunuz çalışma alanına ekleyin.

6. Çalışma alanı hala kümenizden gelen tanılama verilerini bağlı olması gerekiyor. Service Fabric Analytics çözümde oluşturduğunuz kaynak grubuna gidin. Görmeniz gerekir bir *ServiceFabric (\<nameOfOMSWorkspace\>)*. Burada çözüm ayarları, çalışma alanı ayarları değiştirin ve OMS Portalı'na gidin, genel bakış sayfasına gitmek için çözüm tıklayın.

7. Sol gezinti menüsünde tıklatın **depolama hesapları günlükleri**altında *çalışma veri kaynakları*.

8. Üzerinde *depolama hesabı günlüklerine* sayfasında, **Ekle** kümenizin günlükleri için çalışma alanına eklemek için üst.

9. İçine tıklatın **depolama hesabı** kümenizdeki oluşturulan uygun hesap eklemek için. Varsayılan ad kullandıysanız, depolama hesabı adı *sfdg\<resourceGroupName\>*. Kümeniz için kullanılan değer denetleyerek dağıtmak için kullanılan Azure Resource Manager şablonu denetleyerek bu de onaylayabilirsiniz `applicationDiagnosticsStorageAccountName`. Aşağı kaydırın ve da sahip olabilirsiniz **daha fazla yük** adı görünmüyor durumunda. Seçmek için yukarı doğru depolama hesabının adına tıklayın.

10. Ardından, belirtmeniz gerekecektir *veri türü*, hangi ayarlanmalıdır **Service Fabric olayları**.

11. *Kaynak* otomatik olarak ayarlanması gerektiğini *WADServiceFabric\*EventTable*.

12. Tıklatın **Tamam** çalışma alanınızı kümenizin günlüklerine bağlanmak için.

    ![Depolama hesabı günlükleri için OMS ekleme](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

Hesap artık bir parçası olarak gösterilmesi gerekir, *depolama hesabı günlüklerine* alanınıza ait veri kaynaklarında.

Bu, artık Service Fabric analiz çözümü artık doğru kümenizin platform ve uygulama günlüğü tablosu bağlanmış bir OMS günlük analizi çalışma alanındaki eklediniz. Ek kaynaklar aynı şekilde çalışma alanına ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
* [OMS Aracısı'nı dağıtma](service-fabric-diagnostics-oms-agent.md) performans sayaçları toplamak ve docker istatistikleri ve kapsayıcılarınızı günlüklerini toplamak için düğümleriniz üzerine
* İle familiarized [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) günlük analizi bir parçası olarak sunulan özellikler
* [Günlük analizi özel görünümler oluşturmak için Görünüm Tasarımcısı kullanın](../log-analytics/log-analytics-view-designer.md)
