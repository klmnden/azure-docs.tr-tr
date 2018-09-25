---
title: Azure İzleyici ölçüm için konuk işletim sistemi ölçümleri Resource Manager şablonu kullanarak bir Windows sanal makinesi için depolama Gönder
description: Azure İzleyici ölçüm için konuk işletim sistemi ölçümleri Resource Manager şablonu kullanarak bir Windows sanal makinesi için depolama Gönder
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: 4ed911766a14dd35ea662326a5d50df11cf81698
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46984090"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-using-a-resource-manager-template-for-a-windows-virtual-machine"></a>Azure İzleyici ölçüm için konuk işletim sistemi ölçümleri Resource Manager şablonu kullanarak bir Windows sanal makinesi için depolama Gönder

Azure İzleyici [Windows Azure tanılama uzantısını](azure-diagnostics.md) (WAD), konuk işletim sistemi (konuk OS) çalışan bir sanal makine, bulut hizmeti veya Service Fabric kümesinin parçası olarak ölçüm ve günlükleri toplamanızı sağlar.  Uzantı, daha önce bağlı makalede listelenen birçok farklı konumlara telemetri gönderebilir.  

Bu makalede, bir Windows sanal makine için Azure İzleyici veri deposuna Gönder konuk işletim sistemi performans ölçümlerine işlemi açıklanmaktadır. Sürüm 1.11 WAD ile başlayarak, doğrudan Azure İzleyici ölçümleri mağazaya standart platform ölçümleri zaten burada toplanan ölçümler yazabilirsiniz. Bunları bu konumda depolama, platform ölçümleri için kullanılabilen aynı eylemleri erişmenize olanak sağlar.  Eylemler, neredeyse gerçek zamanlı uyarı verme, grafik, yönlendirme dahil, REST API ve daha fazlasını erişim.  Geçmişte, Azure depolama, ancak Azure İzleyici'veri deposu WAD uzantısı yazıldı.   

Resource Manager şablonları yeniyseniz öğrenin [şablon dağıtımları](../azure-resource-manager/resource-group-overview.md)ve yapısını ve söz dizimi.  

## <a name="pre-requisites"></a>Ön koşullar

- Aboneliğiniz ile kaydedilmelidir [Microsoft.Insights](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) 

- Ya da gerek [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) yüklü veya [Azure CloudShell](https://docs.microsoft.com/azure/cloud-shell/overview.md) 

 
## <a name="set-up-azure-monitor-as-a-data-sink"></a>Veri havuzu olarak Azure İzleyicisi'ni ayarlayın 
Azure tanılama uzantısını "rota ölçüm ve günlükleri farklı konumlara veri havuzlarını" adlı bir özellik kullanır.  Aşağıdaki adımlar Resource Manager şablonu ve PowerShell kullanarak yeni "Azure İzleyici" veri havuzu bir VM dağıtmak için nasıl kullanılacağını gösterir. 

## <a name="author-resource-manager-template"></a>Yazar Resource Manager şablonu 
Bu örnekte, genel kullanıma açık örnek şablonu kullanabilirsiniz. Başlangıç şablonları altındadır https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows 

- **Azuredeploy.JSON** bir sanal makine dağıtımı için önceden yapılandırılmış bir Resource Manager şablonudur. 

- **Azuredeploy.Parameters.JSON** hangi kullanıcı adı ve parola, VM için ayarlamak istediğiniz gibi bilgileri depolayan bir parametre dosyası. Dağıtım sırasında bu dosyasında ayarlanan parametreler Resource Manager şablonu kullanılmaktadır. 

İndirip yerel olarak her iki dosyayı kaydedin. 

###  <a name="modify-azuredeployparametersjson"></a>Azuredeploy.Parameters.JSON değiştirme
Açık *azuredeploy.parameters.json* dosyası 

1. İçin değerler girin *adminUsername* ve *adminPassword* VM için. Bu parametreler, VM için uzaktan erişim için kullanılır. Bu şablon dışındaki sağlayarak, sanal Makinenizin zorunda kalmamak için kullanmayın. Botlar internet kullanıcı adları ve parolalar genel Github depoları için tarayın. Bunlar, sanal makineleri ile bu varsayılan test olasılığı yüksektir.  

1. Sanal makine için benzersiz bir dnsname oluşturun.  

### <a name="modify-azuredeployjson"></a>Azuredeploy.JSON değiştirme

Açık *azuredeploy.json* dosyası 

Bir depolama hesabı Kimliğinize ekleme **değişkenleri** girişini sonra şablon bölümünü **storageAccountName**.  

```json
// Find these lines 
"variables": { 
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'sawinvm')]", 

// Add this line directly below.  
    "accountid": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]", 
```

Bu yönetilen hizmet kimliği (MSI) uzantı şablon üst kısmında "Kaynaklar" bölümünü ekleyin.  Uzantı, Azure İzleyici ölçümleri yayılan kabul etmesini sağlar.  

```json
//Find this code 
"resources": [
// Add this code directly below
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

"Kimlik" yapılandırması Azure atar, sistem kimliğini MSI uzantısı emin olmak için VM kaynağı ekleyin. Bu adım, VM Konuk ölçümleri kendisi hakkında Azure İzleyici'gönderebilir sağlar 

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

Bir Windows sanal makine üzerinde tanılama uzantısını etkinleştirmek için aşağıdaki yapılandırma ekleyin.  Basit bir Resource Manager tabanlı sanal makine için uzantı yapılandırması kaynakları diziye sanal makine için ekleyebiliriz. Satırın "havuzlarını": "AzMonSink" ve "uzantıyı etkinleştirmek karşılık gelen SinksConfig" bölümdeki doğrudan Azure İzleyici ölçümlerine yayılamıyor. Gerektiği gibi performans sayaçlarını Ekle/Kaldır çekinmeyin.  


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
            "typeHandlerVersion": "1.4", 
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


Kaydet ve her iki dosyayı Kapat 
 

## <a name="deploy-the-resource-manager-template"></a>Resource Manager şablonu dağıtma 

> [!NOTE]
> Azure tanılama uzantısı sürüm 1.5 çalıştırılması gerekir ya da daha yüksek ve "autoUpgradeMinorVersion" sahip: 'true' olarak Resource Manager şablonunuzu özellik kümesi.  VM başlatıldığında azure daha sonra uygun uzantıyı yükler. Bu ayarlar, şablonunuzda yoksa bunları değiştirmek ve şablonu yeniden dağıtın. 


Resource Manager şablonu dağıtmak için Azure PowerShell yararlanılacaktır.  

1. PowerShell’i başlatın 
1. Azure'ı kullanarak oturum açma `Login-AzureRmAccount`
1. Kullanarak aboneliklerinizin listesi alma `Get-AzureRmSubscription`
1. Oluşturma/sanal makinede güncelleştirmekte aboneliği ayarlamak 

   ```PowerShell
   Select-AzureRmSubscription -SubscriptionName "<Name of the subscription>" 
   ```
1. Dağıtılan, VM için yeni bir kaynak grubu oluşturma çalıştırmak aşağıdaki komutu 

   ```PowerShell
    New-AzureRmResourceGroup -Name "<Name of Resource Group>" -Location "<Azure Region>" 
   ```
   > [!NOTE] 
   > Unutmayın [özel ölçümler için etkin bir Azure bölgesi kullanın](metrics-custom-overview.md). 
 
1. VM dağıtmak için aşağıdaki komutları yürütün  
   > [!NOTE] 
   > Varolan bir VM'yi güncelleştirmek isterseniz, eklemeniz yeterlidir *-artımlı modu* sonuna aşağıdaki komutu. 
 
   ```PowerShell
   New-AzureRmResourceGroupDeployment -Name "<NameThisDeployment>" -ResourceGroupName "<Name of the Resource Group>" -TemplateFile "<File path of your Resource Manager template>" -TemplateParameterFile "<File path of your parameters file>" 
   ```
  
1. Dağıtım başarılı olduktan sonra Azure portalında VM'yi bulun olmalıdır ve Azure İzleyici ölçümlerine yayma. 

   > [!NOTE] 
   > Seçili vmSkuSize geçici hatalara çalıştırabilirsiniz. Böyle bir durumda azuredeploy.json dosyasını için geri dönün ve vmSkuSize parametresinin varsayılan değeri güncelleştirin. Bu durumda, "Standard_DS1_v2" çalışırken öneririz). 

## <a name="chart-your-metrics"></a>Ölçümlerinizin grafiğini 

1. Oturum açma için Azure portalı 

1. Sol taraftaki menüde tıklayın **İzleyicisi** 

1. İzleyici sayfasında tıklayın **ölçümleri**. 

   ![Ölçümleri sayfası](./media/metrics-store-custom-rest-api/metrics.png) 

1. Toplama dönemini değiştirmek **son 30 dakika**.  

1. Kaynak açılan menü, yeni oluşturduğunuz sanal Makineyi seçin. Şablon adı değiştirmediyseniz olmalıdır *SimpleWinVM2*.  

1. Ad alanları açılır seçin **azure.vm.windows.guest** 

1. Ölçümler, açılan menü, seçin **bellek\%Kaydedilmiş Bayt yüzdesi**.  
 

## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [özel ölçümler](metrics-custom-overview.md).