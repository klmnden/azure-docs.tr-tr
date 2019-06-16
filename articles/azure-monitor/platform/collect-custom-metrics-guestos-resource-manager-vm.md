---
title: Azure İzleyici ölçüm için konuk işletim sistemi ölçümleri Resource Manager şablonu kullanarak bir Windows sanal makinesi için depolama Gönder
description: Azure İzleyici ölçüm için konuk işletim sistemi ölçümleri Resource Manager şablonu kullanarak bir Windows sanal makinesi için depolama Gönder
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 5647802ff383ce046d108f25384df81bcbd08cd3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66129677"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-using-a-resource-manager-template-for-a-windows-virtual-machine"></a>Azure İzleyici ölçüm için konuk işletim sistemi ölçümleri Resource Manager şablonu kullanarak bir Windows sanal makinesi için depolama Gönder

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure İzleyicisi'ni kullanarak [tanılama uzantısını](diagnostics-extension-overview.md), konuk işletim sisteminden sanal makine, bulut hizmeti veya Service Fabric kümesinin bir parçası olarak çalıştırılan (konuk OS) ölçümlerini ve günlüklerini toplayabilir. Uzantı için telemetri gönderebilir [birçok farklı konumlarda.](https://docs.microsoft.com/azure/monitoring/monitoring-data-collection?toc=/azure/azure-monitor/toc.json)

Bu makalede, Azure İzleyici veri deposuna bir Windows sanal makine için konuk işletim sistemi performans ölçümleri gönderme işlemi açıklanmaktadır. Tanılama 1.11 sürüm ile başlayarak, doğrudan Azure ölçümleri mağazasından, burada standart platform zaten toplanan ölçümler izleyiciye ölçümleri yazabilirsiniz.

Bunları bu konumda depolama, platform ölçümler için aynı eylemleri erişmenize olanak sağlar. Eylemler yönlendirme neredeyse gerçek zamanlı uyarı verme, grafik, içerir ve bir REST API ve daha fazlasını erişim. Geçmişte, Azure depolama, ancak Azure İzleyici'veri deposu tanılama uzantısını yazıldı.

Resource Manager şablonları yeniyseniz öğrenin [şablon dağıtımları](../../azure-resource-manager/resource-group-overview.md) ve yapısını ve söz dizimi.

## <a name="prerequisites"></a>Önkoşullar

- Aboneliğiniz ile kaydedilmelidir [Microsoft.Insights](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services).

- Ya da gerek [Azure PowerShell](/powershell/azure) veya [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) yüklü.


## <a name="set-up-azure-monitor-as-a-data-sink"></a>Veri havuzu olarak Azure İzleyicisi'ni ayarlayın
Azure tanılama uzantısını "rota ölçüm ve günlükleri farklı konumlara veri havuzlarını" adlı bir özellik kullanır. Aşağıdaki adımlar, bir Resource Manager şablonu ve PowerShell kullanarak yeni "Azure İzleyici" veri havuzu bir VM dağıtmak için nasıl kullanılacağını gösterir.

## <a name="author-resource-manager-template"></a>Yazar Resource Manager şablonu
Bu örnekte, genel kullanıma açık örnek şablonu kullanabilirsiniz. Başlangıç şablonları altındadır https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows.

- **Azuredeploy.JSON** bir sanal makine dağıtımı için önceden yapılandırılmış bir Resource Manager şablonudur.

- **Azuredeploy.Parameters.JSON** hangi kullanıcı adı ve parola, VM için ayarlamak istediğiniz gibi bilgileri depolayan bir parametre dosyası. Dağıtım sırasında bu dosyasında ayarlanan parametreler Resource Manager şablonu kullanılmaktadır.

İndirip yerel olarak her iki dosyayı kaydedin.

### <a name="modify-azuredeployparametersjson"></a>Azuredeploy.Parameters.JSON değiştirme
Açık *azuredeploy.parameters.json* dosyası

1. İçin değerler girin **adminUsername** ve **adminPassword** VM için. Bu parametreler, VM için uzaktan erişim için kullanılır. Sağlayarak, sanal Makinenizin girmekten kaçınmak için yok değerleri bu şablonu kullanın içinde. Botlar internet kullanıcı adları ve parolalar genel GitHub depoları için tarayın. Bunlar, sanal makineleri ile bu varsayılan test olasılığı yüksektir.

1. Sanal makine için benzersiz bir dnsname oluşturun.

### <a name="modify-azuredeployjson"></a>Modify azuredeploy.json

Açık *azuredeploy.json* dosyası

Bir depolama hesabı Kimliğinize ekleme **değişkenleri** girişini sonra şablon bölümünü **storageAccountName.**

```json
// Find these lines.
"variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'sawinvm')]",

// Add this line directly below.
    "accountid": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
```

Bu yönetilen hizmet kimliği (MSI) uzantısı, şablon en üstündeki eklemek **kaynakları** bölümü. Uzantı, Azure İzleyici yayılan ölçümler kabul etmesini sağlar.

```json
//Find this code.
"resources": [
// Add this code directly below.
    {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "WADExtensionSetup",
        "apiVersion": "2015-05-01-preview",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]" ],
        "properties": {
            "publisher": "Microsoft.ManagedIdentity",
            "type": "ManagedIdentityExtensionForWindows",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "port": 50342
            }
        }
    },
```

Ekleme **kimlik** Azure sistem kimliği için MSI uzantısı atar emin olmak için VM kaynağı yapılandırması. Bu adım, VM Konuk ölçümleri kendisi hakkında Azure İzleyici'gönderebilir sağlar.

```json
// Find this section
                "subnet": {
            "id": "[variables('subnetRef')]"
            }
        }
        }
    ]
    }
},
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    // add these 3 lines below
    "identity": {
    "type": "SystemAssigned"
    },
    //end of added lines
    "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
    "hardwareProfile": {
    ...
```

Bir Windows sanal makinesinde tanılama uzantısını etkinleştirmek için aşağıdaki yapılandırma ekleyin. Basit bir Resource Manager tabanlı sanal makine için sanal makine için kaynakları diziye uzantısı yapılandırması ekleyebiliriz. "Havuzlarını" satırı&mdash; "AzMonSink" ve daha sonra bölümdeki karşılık gelen "SinksConfig"&mdash;doğrudan Azure İzleyici ölçümlerine yaymak uzantıyı etkinleştirin. Ekleyip performans sayaçları gerektiğinde çekinmeyin.


```json
        "networkProfile": {
            "networkInterfaces": [
            {
                "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
            ]
        },
"diagnosticsProfile": {
    "bootDiagnostics": {
    "enabled": true,
    "storageUri": "[reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob]"
    }
}
},
//Start of section to add
"resources": [
{
            "type": "extensions",
            "name": "Microsoft.Insights.VMDiagnosticsSettings",
            "apiVersion": "2015-05-01-preview",
            "location": "[resourceGroup().location]",
            "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
            ],
            "properties": {
            "publisher": "Microsoft.Azure.Diagnostics",
            "type": "IaaSDiagnostics",
            "typeHandlerVersion": "1.12",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "WadCfg": {
                "DiagnosticMonitorConfiguration": {
    "overallQuotaInMB": 4096,
    "DiagnosticInfrastructureLogs": {
                    "scheduledTransferLogLevelFilter": "Error"
        },
                    "Directories": {
                    "scheduledTransferPeriod": "PT1M",
    "IISLogs": {
                        "containerName": "wad-iis-logfiles"
                    },
                    "FailedRequestLogs": {
                        "containerName": "wad-failedrequestlogs"
                    }
                    },
                    "PerformanceCounters": {
                    "scheduledTransferPeriod": "PT1M",
                    "sinks": "AzMonSink",
                    "PerformanceCounterConfiguration": [
                        {
                        "counterSpecifier": "\\Memory\\Available Bytes",
                        "sampleRate": "PT15S"
                        },
                        {
                        "counterSpecifier": "\\Memory\\% Committed Bytes In Use",
                        "sampleRate": "PT15S"
                        },
                        {
                        "counterSpecifier": "\\Memory\\Committed Bytes",
                        "sampleRate": "PT15S"
                        }
                    ]
                    },
                    "WindowsEventLog": {
                    "scheduledTransferPeriod": "PT1M",
                    "DataSource": [
                        {
                        "name": "Application!*"
                        }
                    ]
                    },
                    "Logs": {
                    "scheduledTransferPeriod": "PT1M",
                    "scheduledTransferLogLevelFilter": "Error"
                    }
                },
                "SinksConfig": {
                    "Sink": [
                    {
                        "name" : "AzMonSink",
                        "AzureMonitor" : {}
                    }
                    ]
                }
                },
                "StorageAccount": "[variables('storageAccountName')]"
            },
            "protectedSettings": {
                "storageAccountName": "[variables('storageAccountName')]",
                "storageAccountKey": "[listKeys(variables('accountid'),'2015-06-15').key1]",
                "storageAccountEndPoint": "https://core.windows.net/"
            }
            }
        }
        ]
//End of section to add
```


Kaydet ve her iki dosyayı kapatın.


## <a name="deploy-the-resource-manager-template"></a>Resource Manager şablonu dağıtma

> [!NOTE]
> Azure tanılama uzantısı sürüm 1.5 veya üzeri çalıştıran olması ve sahip **autoUpgradeMinorVersion**: Resource Manager şablonunuzu 'true' olarak ayarlanan özelliği. VM başlatıldığında azure daha sonra uygun uzantıyı yükler. Bu ayarlar, şablonunuzda yoksa, bunları değiştirmek ve şablonu yeniden dağıtın.


Resource Manager şablonu dağıtmak için Azure PowerShell biz yararlanın.

1. PowerShell'i başlatın.
1. Azure kullanarak oturum açın `Login-AzAccount`.
1. Aboneliklerinizin listesi almak `Get-AzSubscription`.
1. Sanal makineyi oluşturma/güncelleştirme için kullanmakta olduğunuz aboneliği ayarlayın:

   ```powershell
   Select-AzSubscription -SubscriptionName "<Name of the subscription>"
   ```
1. Dağıtılan VM için yeni bir kaynak grubu oluşturmak için aşağıdaki komutu çalıştırın:

   ```powershell
    New-AzResourceGroup -Name "<Name of Resource Group>" -Location "<Azure Region>"
   ```
   > [!NOTE]
   > Unutmayın [özel ölçümler için etkin bir Azure bölgesi kullanın](metrics-custom-overview.md).

1. Resource Manager şablonu kullanarak VM dağıtmak için aşağıdaki komutları çalıştırın.
   > [!NOTE]
   > Varolan bir VM'yi güncelleştirmek isterseniz, eklemeniz yeterlidir *-artımlı modu* sonuna aşağıdaki komutu.

   ```powershell
   New-AzResourceGroupDeployment -Name "<NameThisDeployment>" -ResourceGroupName "<Name of the Resource Group>" -TemplateFile "<File path of your Resource Manager template>" -TemplateParameterFile "<File path of your parameters file>"
   ```

1. Dağıtım başarılı olduktan sonra VM için Azure İzleyici ölçümleri yayma Azure portalında olması gerekir.

   > [!NOTE]
   > Seçili vmSkuSize geçici hatalara çalıştırmanız gerekebilir. Böyle bir durumda azuredeploy.json dosyasını için geri dönün ve vmSkuSize parametresinin varsayılan değeri güncelleştirin. Bu durumda, "Standard_DS1_v2" çalışırken öneririz).

## <a name="chart-your-metrics"></a>Ölçümlerinizin grafiğini

1. Azure portalında oturum açın.

2. Sol menüden **İzleyici**.

3. İzleyici sayfasında **ölçümleri**.

   ![Ölçümleri sayfası](media/collect-custom-metrics-guestos-resource-manager-vm/metrics.png)

4. Toplama dönemini değiştirmek **son 30 dakika**.

5. Kaynak açılan menüsünde oluşturduğunuz sanal Makineyi seçin. Şablon adı değiştirmediyseniz olmalıdır *SimpleWinVM2*.

6. Ad alanları açılır menüde **azure.vm.windows.guest**

7. Menü ölçümleri açılan seçin **bellek\%Kaydedilmiş Bayt yüzdesi**.


## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [özel ölçümler](metrics-custom-overview.md).

