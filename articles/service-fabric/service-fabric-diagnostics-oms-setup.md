---
title: Azure Service Fabric - ayarlamak günlük analizi ile izleme | Microsoft Docs
description: Günlük analizi Görselleştirme ve olayları çözümlemek için Azure Service Fabric kümeleri izlemek için nasıl ayarlanacağını öğrenin.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 4/03/2018
ms.author: srrengar
ms.openlocfilehash: 90a28162fb1f455c154ad4d2da7beac6bc785bc7
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36301045"
---
# <a name="set-up-log-analytics-for-a-cluster"></a>Bir küme için günlük analizi Ayarla

Günlük analizi küme düzeyi olayları izlemek için Bizim önerimiz ' dir. Günlük analizi çalışma alanı Azure Resource Manager, PowerShell veya Azure Market üzerinden ayarlayabilirsiniz. Dağıtımınızın gelecekte kullanım için güncelleştirilmiş bir Resource Manager şablonu bulunduruyorsanız, günlük analizi ortamınızı ayarlamak için aynı şablonu kullanın. Market aracılığıyla dağıtımı etkin Tanılama ile dağıtılan bir kümeye zaten sahipseniz daha kolay olur. Abonelik düzeyinde erişim için dağıtıyorsanız hesabı yoksa, PowerShell veya Resource Manager şablonunu kullanarak dağıtın.

> [!NOTE]
> Kümenizi izlemek için günlük analizi ayarlamak için tanılama küme düzeyi veya platform düzeyi olayları görüntülemek için etkin olması gerekir. Başvurmak [Windows kümelerinde tanılama ayarlamak nasıl](service-fabric-diagnostics-event-aggregation-wad.md) ve [Linux kümeleri tanılamada ayarlamak nasıl](service-fabric-diagnostics-event-aggregation-lad.md) daha fazla bilgi için

## <a name="deploy-a-log-analytics-workspace-by-using-azure-marketplace"></a>Azure Market kullanarak günlük analizi çalışma alanı dağıtma

Bir küme dağıttıktan sonra günlük analizi çalışma alanı eklemek istiyorsanız, Azure Marketi portalda gidin ve Ara **Service Fabric Analytics**. Bu veriler için Service Fabric belirli olan bir özel Service Fabric dağıtımlar için çözümüdür. Bu işlem çözüm (Öngörüler görüntülemek için Pano) ve çalışma (temel küme veri toplama) oluşturur.

1. Seçin **yeni** sol gezinti menüsünde. 

2. Arama **Service Fabric Analytics**. Görüntülenen kaynağı seçin.

3. **Oluştur**’u seçin.

    ![Service Fabric analizi pazarında](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. Service Fabric Analytics oluşturma penceresinde seçin **bir çalışma alanı seçin** için **OMS çalışma** alan ve ardından **yeni bir çalışma alanı oluşturma**. Gerekli girişleri doldurun. Burada tek gereksinim abonelik Service Fabric kümesi ve çalışma alanı için aynı olmasıdır. Girişlerinizi doğrulandı, çalışma alanınızı dağıtmaya başlar. Dağıtımı yalnızca birkaç dakika sürer.

5. Tamamlandığında, seçin **oluşturma** Service Fabric Analytics oluşturma penceresinin altındaki yeniden. Yeni bir çalışma alanı altında gösterildiğini doğrulayın **OMS çalışma**. Bu eylem çözümü oluşturduğunuz çalışma alanına ekler.

Windows kullanıyorsanız, küme olayları depolandığı günlük analizi depolama hesabına bağlanmak için aşağıdaki adımlarla devam edin. 

>[!NOTE]
>Bu deneyim Linux kümeleri için etkinleştirme henüz kullanılamıyor. 

### <a name="connect-the-log-analytics-workspace-to-your-cluster"></a>Günlük analizi çalışma alanı, kümeye bağlanın 

1. Çalışma alanı kümenizden gelen tanılama verilerini bağlı gerekir. Service Fabric analiz çözümü oluşturduğunuz kaynak grubuna gidin. Seçin **ServiceFabric\<nameOfWorkspace\>**  ve kendi genel bakış sayfasına gidin. Buradan, çözüm ayarları, çalışma alanı ayarları ve erişim günlük analizi çalışma alanı değiştirebilirsiniz.

2. Sol gezinti menüsünde altında **çalışma veri kaynakları**seçin **depolama hesapları günlükleri**.

3. Üzerinde **depolama hesabı günlüklerine** sayfasında, **Ekle** kümenizin günlükleri için çalışma alanına eklemek için üst.

4. Seçin **depolama hesabı** kümenizdeki oluşturulan uygun hesap eklemek için. Varsayılan ad kullandıysanız depolama hesabıdır **sfdg\<resourceGroupName\>**. Ayrıca bu, küme için kullanılan değer denetleyerek dağıtmak için kullanılan Azure Resource Manager şablonu ile doğrulayabilirsiniz **applicationDiagnosticsStorageAccountName**. Adı gösterilmez, aşağı kaydırın ve seçin **daha fazla yük**. Depolama hesabı adı seçin.

5. Veri türü belirtin. Ayarlamak **Service Fabric olayları**.

6. Kaynak otomatik olarak ayarlandığından emin olun **WADServiceFabric\*EventTable**.

7. Seçin **Tamam** çalışma alanınızı kümenizin günlüklerine bağlanmak için.

    ![Günlük analizi depolama hesabı günlüklerine ekleme](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

Çalışma alanınızı ait veri kaynaklarında depolama hesabınızın parçası günlükleri gibi hesap artık görünür.

Şimdi doğru kümenizin platform ve uygulama günlüğü tablosu için bağlı bir günlük analizi çalışma alanındaki Service Fabric analiz çözümü eklediniz. Ek kaynaklar aynı şekilde çalışma alanına ekleyebilirsiniz.


## <a name="deploy-log-analytics-by-using-a-resource-manager-template"></a>Günlük analizi Resource Manager şablonu kullanarak dağıtın

Bir Resource Manager şablonu kullanarak bir küme dağıttığınızda, şablonun yeni bir günlük analizi çalışma alanı oluşturur, Service Fabric çözüm için çalışma alanına ekler ve uygun depolama tablolarından verileri okumak için yapılandırır.

Kullanın ve değiştirme [Bu örnek şablonu](https://github.com/krnese/azure-quickstart-templates/tree/master/service-fabric-oms) gereksinimlerinizi karşılayacak şekilde.

Aşağıdaki değişiklikleri yapın:
1. Ekleme `omsWorkspaceName` ve `omsRegion` tanımlanan parametreler için aşağıdaki kod parçacığını ekleyerek, parametreleri, *template.json* dosya. Uygun gördüğünüz şekilde varsayılan değerleri değiştirmek çekinmeyin. Ayrıca, iki yeni parametre eklemek, *parameters.json* dosya kaynak dağıtımı için değerleri tanımlamak için:
    
    ```json
    "omsWorkspacename": {
        "type": "string",
        "defaultValue": "sfomsworkspace",
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

    `omsRegion` Değerlere sahip değerleri belirli bir dizi uymak. Küme dağıtımı için en yakın olan seçin.

2. Günlük analizi için tüm uygulama günlükleri gönderirseniz, ilk onaylayın `applicationDiagnosticsStorageAccountType` ve `applicationDiagnosticsStorageAccountName` şablonunuzdaki parametreleri olarak dahil edilir. Dahil edilmez, değişkenleri bölümüne ekleyin ve bunların değerleri gerektiği gibi düzenleyin. Ayrıca bunları parametre olarak önceki biçimi izleyerek ekleyebilirsiniz.

    ```json
    "applicationDiagnosticsStorageAccountType": "Standard_LRS",
    "applicationDiagnosticsStorageAccountName": "[toLower(concat('oms', uniqueString(resourceGroup().id), '3' ))]"
    ```

3. Service Fabric çözümü için şablonun değişkenleri ekleyin:

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

## <a name="deploy-log-analytics-by-using-azure-powershell"></a>Azure PowerShell kullanarak günlük analizi dağıtma

Kullanarak günlük analizi kaynağınız PowerShell aracılığıyla dağıtabilirsiniz `New-AzureRmOperationalInsightsWorkspace` komutu. Bu yöntemi kullanmak için yüklediğinizden emin olun [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.1.1). Yeni bir günlük analizi çalışma alanı oluşturmak ve Service Fabric çözüm eklemek için bu betiği kullanın: 

```PowerShell

$SubscriptionName = "<Name of your subscription>"
$ResourceGroup = "<Resource group name>"
$Location = "<Resource group location>"
$WorkspaceName = "<Log Analytics workspace name>"
$solution = "ServiceFabric"

# Log in to Azure and access the correct subscription
Connect-AzureRmAccount
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

İşiniz bittiğinde, uygun depolama hesabı için günlük analizi bağlanmak için önceki bölümdeki adımları izleyin.

Ayrıca, diğer çözümleri ekleyebilir veya PowerShell kullanarak günlük analizi çalışma alanınız için başka değişiklikler yapmayın. Daha fazla bilgi için bkz: [yönetmek günlük PowerShell kullanarak analizi](../log-analytics/log-analytics-powershell-workspace-configuration.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Günlük analizi aracı dağıtım](service-fabric-diagnostics-oms-agent.md) performans sayaçları toplamak ve docker istatistikleri ve kapsayıcılarınızı günlüklerini toplamak için düğümleriniz üzerine
* İle familiarized [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) günlük analizi bir parçası olarak sunulan özellikler
* [Günlük analizi özel görünümler oluşturmak için Görünüm Tasarımcısı kullanın](../log-analytics/log-analytics-view-designer.md)
