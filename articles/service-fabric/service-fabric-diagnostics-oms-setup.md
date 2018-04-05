---
title: Azure Service Fabric - ayarlamak OMS günlük analizi ile izleme | Microsoft Docs
description: Operations Management Suite Görselleştirme ve olayları çözümlemek için Azure Service Fabric kümeleri izlemek için nasıl ayarlanacağını öğrenin.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 3/30/2018
ms.author: dekapur; srrengar
ms.openlocfilehash: 2589efa1808a394f2e32b842efa2ee70809da232
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="set-up-operations-management-suite-log-analytics-for-a-cluster"></a>Operations Management Suite günlük analizi bir küme için Ayarla

Bir Operations Management Suite (OMS) çalışma alanını Azure Resource Manager, PowerShell veya Azure Marketi kullanarak ayarlayabilirsiniz. Dağıtımınızın gelecekte kullanım için güncelleştirilmiş bir Resource Manager şablonu bulunduruyorsanız, OMS ortamınızı ayarlamak için aynı şablonu kullanın. Market aracılığıyla dağıtımı etkin Tanılama ile dağıtılan bir kümeye zaten sahipseniz daha kolay olur. OMS dağıtma hesabında abonelik düzeyinde erişimi yoksa, PowerShell veya Resource Manager şablonunu kullanarak dağıtın.

> [!NOTE]
> Kümenizi izlemek için OMS ayarlamak için tanılama küme düzeyi veya platform düzeyi olayları görüntülemek için etkin olması gerekir.

## <a name="deploy-oms-by-using-azure-marketplace"></a>Azure Market kullanarak OMS dağıtma

Bir küme dağıttıktan sonra bir OMS çalışma eklemek istiyorsanız, Azure Marketi portalda gidin ve Ara **Service Fabric Analytics**:

1. Seçin **yeni** sol gezinti menüsünde. 

2. Arama **Service Fabric Analytics**. Görüntülenen kaynağı seçin.

3. **Oluştur**’u seçin.

    ![OMS BT analizi pazarında](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. Service Fabric Analytics oluşturma penceresinde seçin **bir çalışma alanı seçin** için **OMS çalışma** alan ve ardından **yeni bir çalışma alanı oluşturma**. Gerekli girişleri doldurun. Burada tek gereksinim abonelik Service Fabric kümesi ve OMS çalışma alanı için aynı olmasıdır. Girişlerinizi doğrulandı, OMS çalışma alanınızı dağıtmak başlatılır. Dağıtımı yalnızca birkaç dakika sürer.

5. Tamamlandığında, seçin **oluşturma** Service Fabric Analytics oluşturma penceresinin altındaki yeniden. Yeni bir çalışma alanı altında gösterildiğini doğrulayın **OMS çalışma**. Bu eylem çözümü oluşturduğunuz çalışma alanına ekler.

Windows kullanıyorsanız, OMS küme olayları depolandığı depolama hesabına bağlanmak için aşağıdaki adımlarla devam edin. 

>[!NOTE]
>Bu deneyim Linux kümeleri için etkinleştirme henüz kullanılamıyor. 

### <a name="connect-the-oms-workspace-to-your-cluster"></a>OMS çalışma alanında, kümeye bağlanın 

1. Çalışma alanı kümenizden gelen tanılama verilerini bağlı gerekir. Service Fabric analiz çözümü oluşturduğunuz kaynak grubuna gidin. Seçin **ServiceFabric\<nameOfOMSWorkspace\>**  ve kendi genel bakış sayfasına gidin. Buradan, çözüm ayarları, çalışma alanı ayarları değiştirin ve OMS portalı erişebilirsiniz.

2. Sol gezinti menüsünde altında **çalışma veri kaynakları**seçin **depolama hesapları günlükleri**.

3. Üzerinde **depolama hesabı günlüklerine** sayfasında, **Ekle** kümenizin günlükleri için çalışma alanına eklemek için üst.

4. Seçin **depolama hesabı** kümenizdeki oluşturulan uygun hesap eklemek için. Varsayılan ad kullandıysanız depolama hesabıdır **sfdg\<resourceGroupName\>**. Ayrıca bu, küme için kullanılan değer denetleyerek dağıtmak için kullanılan Azure Resource Manager şablonu ile doğrulayabilirsiniz **applicationDiagnosticsStorageAccountName**. Adı gösterilmez, aşağı kaydırın ve seçin **daha fazla yük**. Depolama hesabı adı seçin.

5. Veri türü belirtin. Ayarlamak **Service Fabric olayları**.

6. Kaynak otomatik olarak ayarlandığından emin olun **WADServiceFabric\*EventTable**.

7. Seçin **Tamam** çalışma alanınızı kümenizin günlüklerine bağlanmak için.

    ![Depolama hesabı günlükleri için OMS ekleme](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

Çalışma alanınızı ait veri kaynaklarında depolama hesabınızın parçası günlükleri gibi hesap artık görünür.

Şimdi doğru kümenizin platform ve uygulama günlüğü tablosu için bağlı bir OMS günlük analizi çalışma alanındaki Service Fabric analiz çözümü eklediniz. Ek kaynaklar aynı şekilde çalışma alanına ekleyebilirsiniz.


## <a name="deploy-oms-by-using-a-resource-manager-template"></a>Resource Manager şablonu kullanarak OMS dağıtma

Bir Resource Manager şablonu kullanarak bir küme dağıttığınızda, şablonun yeni bir OMS çalışma alanı oluşturur, Service Fabric çözüm için çalışma alanına ekler ve uygun depolama tablolarından verileri okumak için yapılandırır.

Kullanın ve değiştirme [Bu örnek şablonu](https://github.com/krnese/azure-quickstart-templates/tree/master/service-fabric-oms) gereksinimlerinizi karşılayacak şekilde.

Aşağıdaki değişiklikleri yapın:
1. Ekleme `omsWorkspaceName` ve `omsRegion` tanımlanan parametreler için aşağıdaki kod parçacığını ekleyerek, parametreleri, *template.json* dosya. Uygun gördüğünüz şekilde varsayılan değerleri değiştirmek çekinmeyin. Ayrıca, iki yeni parametre eklemek, *parameters.json* dosya kaynak dağıtımı için değerleri tanımlamak için:
    
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

    `omsRegion` Değerlere sahip değerleri belirli bir dizi uymak. Küme dağıtımı için en yakın olan seçin.

2. OMS için tüm uygulama günlükleri gönderirseniz, ilk onaylayın `applicationDiagnosticsStorageAccountType` ve `applicationDiagnosticsStorageAccountName` şablonunuzdaki parametreleri olarak dahil edilir. Dahil edilmez, değişkenleri bölümüne ekleyin ve bunların değerleri gerektiği gibi düzenleyin. Ayrıca bunları parametre olarak önceki biçimi izleyerek ekleyebilirsiniz.

    ```json
    "applicationDiagnosticsStorageAccountType": "Standard_LRS",
    "applicationDiagnosticsStorageAccountName": "[toLower(concat('oms', uniqueString(resourceGroup().id), '3' ))]"
    ```

3. Service Fabric OMS çözümü için şablonun değişkenleri ekleyin:

    ```json
    "solution": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "solutionName": "ServiceFabric"
    ```

4. Aşağıdaki Service Fabric küme kaynağı burada bildirilmiş sonra kaynakları bölümünüzü sonuna ekleyin:

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
                "name": "[concat(parameters('applicationDiagnosticsStorageAccountName'),parameters('omsWorkspacename'))]",
                "type": "storageinsightconfigs",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]",
                    "[concat('Microsoft.Storage/storageAccounts/', parameters('applicationDiagnosticsStorageAccountName'))]"
                ],
                "properties": {
                    "containers": [ ],
                    "tables": [
                        "WADServiceFabric*EventTable",
                        "WADWindowsEventLogsTable",
                        "WADETWEventTable"
                    ],
                    "storageAccount": {
                        "id": "[resourceId('Microsoft.Storage/storageaccounts/', parameters('applicationDiagnosticsStorageAccountName'))]",
                        "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName')),'2015-06-15').key1]"
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
    
    > [!NOTE]
    > Eklediyseniz `applicationDiagnosticsStorageAccountName` bir değişken olarak için her referansı değiştirdiğinizden emin olun `variables('applicationDiagnosticsStorageAccountName')` yerine `parameters('applicationDiagnosticsStorageAccountName')`.

5. Kullanarak bir Resource Manager yükseltme kümenize olarak şablonu dağıtma `New-AzureRmResourceGroupDeployment` AzureRM PowerShell modülündeki API. Bir örnek komut şöyle olacaktır:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName "sfcluster1" -TemplateFile "<path>\template.json" -TemplateParameterFile "<path>\parameters.json"
    ``` 

    Azure Resource Manager, bu komutu mevcut bir kaynağı için bir güncelleştirme olarak algılar. Yalnızca, varolan dağıtım yürüten şablonu ve sağlanan yeni şablonu arasındaki değişiklikleri işler.

## <a name="deploy-oms-by-using-azure-powershell"></a>Azure PowerShell kullanarak OMS dağıtma

Kullanarak OMS günlük analizi kaynağınız PowerShell aracılığıyla dağıtabilirsiniz `New-AzureRmOperationalInsightsWorkspace` komutu. Bu yöntemi kullanmak için yüklediğinizden emin olun [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.1.1). Yeni bir OMS günlük analizi çalışma alanı oluşturmak ve Service Fabric çözüm eklemek için bu betiği kullanın: 

```PowerShell

$SubscriptionName = "<Name of your subscription>"
$ResourceGroup = "<Resource group name>"
$Location = "<Resource group location>"
$WorkspaceName = "<OMS Log Analytics workspace name>"
$solution = "ServiceFabric"

# Log in to Azure and access the correct subscription
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId $SubID 

# Create the resource group if needed
try {
    Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop
} catch {
    New-AzureRmResourceGroup -Name $ResourceGroup -Location $Location
}

New-AzureRmOperationalInsightsWorkspace -Location $Location -Name $WorkspaceName -Sku Standard -ResourceGroupName $ResourceGroup
Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName $ResourceGroup -WorkspaceName $WorkspaceName -IntelligencePackName $solution -Enabled $true

```

İşiniz bittiğinde, OMS günlük analizi uygun depolama hesabına bağlanmak için önceki bölümdeki adımları izleyin.

Ayrıca, diğer çözümleri ekleyebilir veya PowerShell kullanarak OMS çalışma alanınıza başka değişiklikler yapmayın. Daha fazla bilgi için bkz: [yönetmek günlük PowerShell kullanarak analizi](../log-analytics/log-analytics-powershell-workspace-configuration.md).

## <a name="next-steps"></a>Sonraki adımlar
* [OMS Aracısı'nı dağıtma](service-fabric-diagnostics-oms-agent.md) performans sayaçları toplamak ve docker istatistikleri ve kapsayıcılarınızı günlüklerini toplamak için düğümleriniz üzerine
* İle familiarized [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) günlük analizi bir parçası olarak sunulan özellikler
* [Günlük analizi özel görünümler oluşturmak için Görünüm Tasarımcısı kullanın](../log-analytics/log-analytics-view-designer.md)
