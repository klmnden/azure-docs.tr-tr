---
title: Bir Windows sanal makine ölçek kümesi için bir Azure Resource Manager şablonu kullanarak konuk işletim sistemi ölçümler Azure İzleyici ölçüm depoya gönder
description: Bir Windows sanal makine ölçek kümesi için bir Resource Manager şablonu kullanarak konuk işletim sistemi ölçümler Azure İzleyici ölçüm depoya gönder
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: 573c205cd2e208a1cb2b526d96fb08ca21331c80
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58481332"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-by-using-an-azure-resource-manager-template-for-a-windows-virtual-machine-scale-set"></a>Bir Windows sanal makine ölçek kümesi için bir Azure Resource Manager şablonu kullanarak konuk işletim sistemi ölçümler Azure İzleyici ölçüm depoya gönder

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure İzleyicisi'ni kullanarak [Windows Azure tanılama (WAD) uzantısı](diagnostics-extension-overview.md), bir sanal makine, bulut hizmeti veya Azure Service Fabric kümesi bir parçası olarak çalıştırılan ölçümleri ve konuk işletim sistemi (konuk OS) günlüklerini toplayabilir. Uzantı, daha önce bağlı makalede listelenen birçok farklı konumlara telemetri gönderebilir.  

Bu makalede, Windows sanal makine ölçek kümesi Azure İzleyici veri deposuna için gönderme konuk işletim sistemi performans ölçümlerini işlemi açıklanır. Windows Azure tanılama sürüm 1.11 ile başlayarak, doğrudan Azure İzleyici ölçümleri ölçümlerini mağazasından, burada standart platform zaten toplanan ölçümler yazabilirsiniz. Bu konumda depolayarak bunları, platform ölçümleri için kullanılabilen aynı eylemleri erişebilirsiniz. Grafik, yönlendirme, REST API ve diğer erişmek, neredeyse gerçek zamanlı uyarı verme eylemleri içerir. Geçmişte, Azure depolama, ancak Azure İzleyici'veri deposu için Windows Azure tanılama uzantısını yazıldı.  

Resource Manager şablonları yeniyseniz öğrenin [şablon dağıtımları](../../azure-resource-manager/resource-group-overview.md) ve yapısını ve söz dizimi.  

## <a name="prerequisites"></a>Önkoşullar

- Aboneliğiniz ile kaydedilmelidir [Microsoft.Insights](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services). 

- İhtiyacınız [Azure PowerShell](/powershell/azure) yüklü veya [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview). 


## <a name="set-up-azure-monitor-as-a-data-sink"></a>Veri havuzu olarak Azure İzleyicisi'ni ayarlayın 
Azure tanılama uzantısını adında bir özellik kullanır **veri havuzları** rota ölçümlerini ve günlüklerini farklı konumlara için. Aşağıdaki adımlar, bir Resource Manager şablonu ve PowerShell kullanarak yeni Azure Monitor veri havuzu bir VM dağıtmak için nasıl kullanılacağını gösterir. 

## <a name="author-a-resource-manager-template"></a>Resource Manager şablonu yazma 
Bu örnekte, bir herkese kullanabilirsiniz [örnek şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-autoscale):  

- **Azuredeploy.JSON** bir sanal makine ölçek kümesi dağıtımı için önceden yapılandırılmış bir Resource Manager şablonudur.

- **Azuredeploy.Parameters.JSON** hangi kullanıcı adı ve parola, sanal Makineniz için ayarlamak istediğiniz gibi bilgileri depolayan bir parametre dosyasıdır. Dağıtım sırasında bu dosyasında ayarlanan parametreler Resource Manager şablonu kullanılmaktadır. 

İndirip yerel olarak her iki dosyayı kaydedin. 

###  <a name="modify-azuredeployparametersjson"></a>Azuredeploy.Parameters.JSON değiştirme
Açık **azuredeploy.parameters.json** dosyası:  
 
- Sağlayan bir **vmSKU** dağıtmak istediğiniz. Standart_d2_v3 öneririz. 
- Belirtin bir **windowsOSVersion** , sanal makine ölçek kümesi için istediğiniz. 2016-Datacenter öneririz. 
- Sanal makine ölçek kümesi kullanarak dağıtılacak kaynak adı bir **vmssName** özelliği. Bir örnek **VMSS WAD TEST**.    
- Kullanarak sanal makine ölçek çalıştırmak istediğiniz VM'lerin sayısını **Instancecount** özelliği.
- İçin değerler girin **adminUsername** ve **adminPassword** için sanal makine ölçek kümesi. Bu parametreler ölçek kümesindeki sanal makineler için uzaktan erişim için kullanılır. Sağlayarak, sanal makinenizin zorunluluğundan **olmayan** dışındaki bu şablonu kullanın. Botlar internet kullanıcı adları ve parolalar genel GitHub depoları için tarayın. Bunlar, sanal makineleri ile bu varsayılan test olasılığınız vardır. 


###  <a name="modify-azuredeployjson"></a>Modify azuredeploy.json
Açık **azuredeploy.json** dosya. 

Depolama hesabı bilgileri Resource Manager şablonunda tutacak bir değişken ekleyin. Azure İzleyici ölçüm deponun ve burada belirttiğiniz depolama hesabı için tüm günlükleri ya da performans sayaçları tanılama yapılandırma dosyasında belirtilen yazılır: 

```json
"variables": { 
//add this line       
"storageAccountName": "[concat('storage', uniqueString(resourceGroup().id))]", 
```
 
Sanal makine ölçek kümesi tanımı kaynakları bölümünü bulun ve ekleyin **kimlik** yapılandırma bölümü. Bu ayrıca, Azure, sistem kimliğini atar sağlar. Bu adım ayrıca ölçek kümesindeki sanal makineler Konuk ölçümleri kendileri hakkında Azure İzleyici'gönderebilir sağlar:  

```json
    { 
      "type": "Microsoft.Compute/virtualMachineScaleSets", 
      "name": "[variables('namingInfix')]", 
      "location": "[resourceGroup().location]", 
      "apiVersion": "2017-03-30", 
      //add these lines below
      "identity": { 
           "type": "systemAssigned" 
       }, 
       //end of lines to add
```

İçinde sanal makine ölçek kümesi kaynak, Bul **virtualMachineProfile** bölümü. Adlı yeni bir profil Ekle **extensionsProfile** uzantıları yönetmek için.  


İçinde **extensionprofile öğesine**, gösterildiği gibi yeni bir uzantı şablonuna ekleme **VMSS WAD uzantısı** bölümü.  Bu bölüm, yönetilen kimlikleri yayılan ölçümleri Azure İzleyici tarafından kabul edilen sağlayan Azure kaynaklarını uzantısı için değildir. **Adı** herhangi bir ad alanı içerebilir. 

Aşağıdaki kod MSI uzantıdan de yapılandırma ve tanılama uzantısı bir uzantı kaynak olarak sanal makine ölçek kümesi kaynağına ekler. Ekleyip performans sayaçları gerektiğinde çekinmeyin: 

```json
          "extensionProfile": { 
            "extensions": [ 
            // BEGINNING of added code  
            // Managed identities for Azure resources   
                { 
                 "name": "VMSS-WAD-extension", 
                 "properties": { 
                       "publisher": "Microsoft.ManagedIdentity", 
                       "type": "ManagedIdentityExtensionForWindows", 
                       "typeHandlerVersion": "1.0", 
                       "autoUpgradeMinorVersion": true, 
                       "settings": { 
                             "port": 50342 
                           }, 
                       "protectedSettings": {} 
                     } 
                                
            }, 
            // add diagnostic extension. (Remove this comment after pasting.)
            { 
              "name": "[concat('VMDiagnosticsVmExt','_vmNodeType0Name')]", 
              "properties": { 
                   "type": "IaaSDiagnostics", 
                   "autoUpgradeMinorVersion": true, 
                   "protectedSettings": { 
                        "storageAccountName": "[variables('storageAccountName')]", 
                        "storageAccountKey": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName')),'2015-05-01-preview').key1]", 
                        "storageAccountEndPoint": "https://core.windows.net/" 
                   }, 
                   "publisher": "Microsoft.Azure.Diagnostics", 
                   "settings": { 
                        "WadCfg": { 
                              "DiagnosticMonitorConfiguration": { 
                                   "overallQuotaInMB": "50000", 
                                   "PerformanceCounters": { 
                                       "scheduledTransferPeriod": "PT1M", 
                                       "sinks": "AzMonSink", 
                                       "PerformanceCounterConfiguration": [ 
                                          { 
              "counterSpecifier": "\\Memory\\% Committed Bytes In Use", 
                                              "sampleRate": "PT15S" 
           }, 
           { 
              "counterSpecifier": "\\Memory\\Available Bytes", 
              "sampleRate": "PT15S" 
           }, 
           { 
              "counterSpecifier": "\\Memory\\Committed Bytes", 
              "sampleRate": "PT15S" 
           } 
                                       ] 
                                 }, 
                                 "EtwProviders": { 
                                       "EtwEventSourceProviderConfiguration": [ 
                                           { 
                                              "provider": "Microsoft-ServiceFabric-Actors", 
                                              "scheduledTransferKeywordFilter": "1", 
                                              "scheduledTransferPeriod": "PT5M", 
                                              "DefaultEvents": { 
                                              "eventDestination": "ServiceFabricReliableActorEventTable" 
                                           } 
                                           }, 
                                           { 
                                              "provider": "Microsoft-ServiceFabric-Services", 
                                              "scheduledTransferPeriod": "PT5M", 
                                              "DefaultEvents": { 
                                                   "eventDestination": "ServiceFabricReliableServiceEventTable" 
                                              } 
                                           } 
                                     ], 
                                     "EtwManifestProviderConfiguration": [ 
                                           { 
                                              "provider": "cbd93bc2-71e5-4566-b3a7-595d8eeca6e8", 
                                              "scheduledTransferLogLevelFilter": "Information", 
                                              "scheduledTransferKeywordFilter": "4611686018427387904", 
                                              "scheduledTransferPeriod": "PT5M", 
                                              "DefaultEvents": { 
                                                   "eventDestination": "ServiceFabricSystemEventTable" 
                                              } 
                                          } 
                                     ] 
                               } 
                               }, 
                               "SinksConfig": { 
                                     "Sink": [ 
                                          { 
                                              "name": "AzMonSink", 
                                              "AzureMonitor": {} 
                                          } 
                                      ] 
                               } 
                         }, 
                         "StorageAccount": "[variables('storageAccountName')]" 
                         }, 
                        "typeHandlerVersion": "1.11" 
                  } 
           } 
            ] 
          }
          }
      }
    },
    //end of added code plus a few brackets. Be sure that the number and type of brackets match properly when done. 
    {
      "type": "Microsoft.Insights/autoscaleSettings",
...
```


Ekleme bir **dependsOn** doğru sırayla oluşturulduğu emin olmak depolama hesabı için: 

```json
"dependsOn": [ 
"[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]", 
"[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]" 
//add this line below
"[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]" 
```

Şablon zaten oluşturduysanız değil, bir depolama hesabı oluşturun: 

```json
"resources": [
// add this code    
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2015-05-01-preview",
    "location": "[resourceGroup().location]",
    "properties": {
    "accountType": "Standard_LRS"
    }
},
// end added code
{ 
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[variables('virtualNetworkName')]",
```

Kaydet ve her iki dosyayı kapatın. 

## <a name="deploy-the-resource-manager-template"></a>Resource Manager şablonu dağıtma 

> [!NOTE]  
> Azure tanılama uzantısı sürüm 1.5 veya üzeri çalıştırmalıdır **ve** sahip **autoUpgradeMinorVersion:** özelliğini **true** , Kaynak Yöneticisi'nde şablonu. VM başlatıldığında azure daha sonra uygun uzantıyı yükler. Bu ayarlar, şablonunuzda yoksa, bunları değiştirmek ve şablonu yeniden dağıtın. 


Resource Manager şablonu dağıtmak için Azure PowerShell kullanın:  

1. PowerShell'i başlatın. 
1. Oturum açmak için Azure kullanarak `Login-AzAccount`.
1. Aboneliklerinizin listesi almak `Get-AzSubscription`.
1. Abonelik oluşturmak veya sanal makineyi güncelleştirmek ayarlayın: 

   ```powershell
   Select-AzSubscription -SubscriptionName "<Name of the subscription>" 
   ```
1. Dağıtılan VM için yeni bir kaynak grubu oluşturun. Şu komutu çalıştırın: 

   ```powershell
    New-AzResourceGroup -Name "VMSSWADtestGrp" -Location "<Azure Region>" 
   ```

   > [!NOTE]  
   > Özel ölçümler için etkin bir Azure bölgesi kullanmanız gerektiğini unutmayın. Kullanmayı unutmayın bir [özel ölçümler için etkin bir Azure bölgesine](https://github.com/MicrosoftDocs/azure-docs-pr/pull/metrics-custom-overview.md#supported-regions).
 
1. VM dağıtmak için aşağıdaki komutları çalıştırın:  

   > [!NOTE]  
   > Mevcut bir ölçek kümesini güncelleştirmek istiyorsanız, ekleme **-artımlı modu** sonuna kadar komutu. 
 
   ```powershell
   New-AzResourceGroupDeployment -Name "VMSSWADTest" -ResourceGroupName "VMSSWADtestGrp" -TemplateFile "<File path of your azuredeploy.JSON file>" -TemplateParameterFile "<File path of your azuredeploy.parameters.JSON file>"  
   ```

1. Dağıtım başarılı olduktan sonra Azure portalında sanal makine ölçek kümesi bulmanız gerekir. Bu, Azure İzleyici ölçümlerine yayma. 

   > [!NOTE]  
   > Seçili geçici hataları karşılaşabileceğiniz **vmSkuSize**. Bu durumda, geri gidin, **azuredeploy.json** dosya ve varsayılan değer olan güncelleştirme **vmSkuSize** parametresi. Denemenizi öneririz **Standard_DS1_v2**. 


## <a name="chart-your-metrics"></a>Ölçümlerinizin grafiğini 

1. Azure Portal’da oturum açın. 

1. Sol taraftaki menüde **İzleyici**. 

1. Üzerinde **İzleyici** sayfasında **ölçümleri**. 

   ![İzleyici - ölçümler sayfası](media/collect-custom-metrics-guestos-resource-manager-vmss/metrics.png) 

1. Toplama dönemini değiştirmek **son 30 dakika**.  

1. Kaynak açılan menüsünde oluşturduğunuz sanal makine ölçek kümesi'ni seçin.  

1. Ad alanları açılır menüde **azure.vm.windows.guest**. 

1. Ölçümleri açılan menüde **bellek\%Kaydedilmiş Bayt yüzdesi**.  

Ardından da boyutlarını üzerinde bu ölçüm için belirli bir VM'nin grafik veya ölçek kümesindeki her VM çizmek için kullanmayı da tercih edebilirsiniz. 



## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [özel ölçümler](metrics-custom-overview.md).


