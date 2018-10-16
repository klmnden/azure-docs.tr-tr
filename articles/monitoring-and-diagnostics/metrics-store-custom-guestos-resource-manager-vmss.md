---
title: Resource Manager şablonu kullanarak bir Windows sanal makine ölçek kümesi için Azure İzleyici ölçüm için konuk işletim sistemi ölçümleri depolamak Gönder
description: Resource Manager şablonu kullanarak bir Windows sanal makine ölçek kümesi için Azure İzleyici ölçüm için konuk işletim sistemi ölçümleri depolamak Gönder
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: howto
ms.date: 09/24/2018
ms.author: ancav
ms.component: metrics
ms.openlocfilehash: 7b600bd699ce7f9e4a6c7cba1a41b6bdece16bf0
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49343738"
---
# <a name="send-guest-os-metrics-to-the-azure-monitor-metric-store-using-a-resource-manager-template-for-a-windows-virtual-machine-scale-set"></a>Resource Manager şablonu kullanarak bir Windows sanal makine ölçek kümesi için Azure İzleyici ölçüm için konuk işletim sistemi ölçümleri depolamak Gönder

Azure İzleyici [Windows Azure tanılama uzantısını](azure-diagnostics.md) (WAD), konuk işletim sistemi (konuk OS) çalışan bir sanal makine, bulut hizmeti veya Service Fabric kümesinin parçası olarak ölçüm ve günlükleri toplamanızı sağlar.  Uzantı, daha önce bağlı makalede listelenen birçok farklı konumlara telemetri gönderebilir.  

Bu makalede, Windows sanal makine ölçek kümesi Azure İzleyici veri deposuna için gönderme konuk işletim sistemi performans ölçümlerini işlemi açıklanır. Sürüm 1.11 WAD ile başlayarak, doğrudan Azure İzleyici ölçümleri mağazaya standart platform ölçümleri zaten burada toplanan ölçümler yazabilirsiniz. Bunları bu konumda depolama, platform ölçümleri için kullanılabilen aynı eylemleri erişmenize olanak sağlar.  Eylemler, neredeyse gerçek zamanlı uyarı verme, grafik, yönlendirme dahil, REST API ve daha fazlasını erişim.  Geçmişte, Azure depolama, ancak Azure İzleyici'veri deposu WAD uzantısı yazıldı.  

Resource Manager şablonları yeniyseniz öğrenin [şablon dağıtımları](../azure-resource-manager/resource-group-overview.md)ve yapısını ve söz dizimi.  

## <a name="pre-requisites"></a>Ön koşullar

- Aboneliğiniz ile kaydedilmelidir [Microsoft.Insights](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) 

- Ya da gerek [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-6.8.1) yüklü veya [Azure CloudShell](https://docs.microsoft.com/azure/cloud-shell/overview.md) 


## <a name="set-up-azure-monitor-as-a-data-sink"></a>Veri havuzu olarak Azure İzleyicisi'ni ayarlayın 
Azure tanılama uzantısını "rota ölçüm ve günlükleri farklı konumlara veri havuzlarını" adlı bir özellik kullanır.  Aşağıdaki adımlar Resource Manager şablonu ve PowerShell kullanarak yeni "Azure İzleyici" veri havuzu bir VM dağıtmak için nasıl kullanılacağını gösterir. 

## <a name="author-resource-manager-template"></a>Yazar Resource Manager şablonu 
Bu örnekte, genel kullanıma açık örnek şablonu kullanabilirsiniz. Başlangıç şablonları altındadır https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-autoscale  

- **Azuredeploy.JSON** bir sanal makine ölçek kümesi dağıtımı için önceden yapılandırılmış bir Resource Manager şablonu

- **Azuredeploy.Parameters.JSON** hangi kullanıcı adı ve parola, VM için ayarlamak istediğiniz gibi bilgileri depolayan bir parametre dosyası. Dağıtım sırasında bu dosyasında ayarlanan parametreler Resource Manager şablonu kullanılmaktadır. 

İndirip yerel olarak her iki dosyayı kaydedin. 

###  <a name="modify-azuredeployparametersjson"></a>Azuredeploy.Parameters.JSON değiştirme
Açık *azuredeploy.parameters.json* dosyası 

- Sağlayan bir **vmSKU** dağıtmak istiyor musunuz (standart_d2_v3 önerilir) 
- Belirtin bir **windowsOSVersion** (2016-Datacenter öneririz), sanal makine ölçek kümesi için istediğiniz 
- Sanal makine ölçek kümesi kullanarak dağıtılacak kaynak adı bir **vmssName** özelliği. Örneğin, *VMSS WAD TEST*.    
- Kullanarak sanal makine ölçek kümesi üzerinde çalıştırılmasını istediğiniz VM'lerin sayısını **Instancecount** özelliği
- İçin değerler girin **adminUsername** ve **adminPassword** için sanal makine ölçek kümesi. Bu parametreler ölçek kümesindeki sanal makineler için uzaktan erişim için kullanılır. Bu parametreler, VM için uzaktan erişim için kullanılır. Bu şablon dışındaki sağlayarak, sanal Makinenizin zorunda kalmamak için kullanmayın. Botlar internet kullanıcı adları ve parolalar genel Github depoları için tarayın. Bunlar, sanal makineleri ile bu varsayılan test olasılığı yüksektir. 


###  <a name="modify-azuredeployjson"></a>Azuredeploy.JSON değiştirme
Açık *azuredeploy.json* dosyası 

Depolama hesabı bilgileri Resource Manager şablonunda tutacak bir değişken ekleyin. Yine de bir depolama hesabı tanılama uzantısını yüklemesinin bir parçası olarak sağlamanız gerekir. Tüm günlükleri ve/veya tanılama yapılandırma dosyasında belirtilen performans sayaçları Azure İzleyici ölçüm deposuna gönderilen ek olarak belirtilen depolama hesabına yazılır. 

```json
"variables": { 
//add this line       
"storageAccountName": "[concat('storage', uniqueString(resourceGroup().id))]", 
 ```
 
Kaynaklar bölümünde sanal makine ölçek kümesi tanımı bulun ve "kimlik" bölümü yapılandırmaya ekleyin. Bu, Azure, sistem kimliğini atar sağlar. Bu adım, Konuk ölçümleri kendileri hakkında Azure İzleyici'yayar ölçek kümesindeki sanal makineler sağlar.  

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


İçinde **extensionprofile öğesine**, gösterildiği gibi yeni bir uzantı şablonuna ekleme **VMSS WAD uzantısı bölümüne**.  Bu bölüm, yönetilen kimlikleri yayılan ölçümleri Azure İzleyici tarafından kabul edilen sağlayan Azure kaynaklarını uzantısı için değildir. **Adı** herhangi bir ad alanı içerebilir. 

MSI uzantısı altındaki koddan de yapılandırma ve tanılama uzantısı bir uzantı kaynak olarak bir sanal makine ölçek kümesi kaynak ekler. Gerektiği gibi performans sayaçlarını Ekle/Kaldır çekinmeyin. 

```json
          "extensionProfile": { 
            "extensions": [ 
            // BEGINNING of added code  
            // Managed identites for Azure resources   
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


Depolama hesabı, doğru sırada oluşturulmasını sağlamak için bir dependsOn ekleyin. 
```json
"dependsOn": [ 
"[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]", 
"[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]" 
//add this line below
"[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]" 
 ```

Bir şablonda oluşturulmamış bir depolama hesabı oluşturun.  
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
> Azure tanılama uzantısı sürüm 1.5 çalıştırılması gerekir ya da daha yüksek ve "autoUpgradeMinorVersion" sahip: özelliğini *true* Resource Manager şablonunuzda.  VM başlatıldığında azure daha sonra uygun uzantıyı yükler. Bu ayarlar, şablonunuzda yoksa bunları değiştirmek ve şablonu yeniden dağıtın. 


Resource Manager şablonu dağıtmak için Azure PowerShell yararlanılacaktır.  

1. PowerShell’i başlatın 
1. Azure kullanarak oturum açın `Login-AzureRmAccount`
1. Kullanarak aboneliklerinizin listesi alma `Get-AzureRmSubscription`
1. Oluşturma/sanal makinede güncelleştirmekte aboneliği ayarlamak 

   ```PowerShell
   Select-AzureRmSubscription -SubscriptionName "<Name of the subscription>" 
   ```
1. Dağıtılan, VM için yeni bir kaynak grubu oluşturma çalıştırmak aşağıdaki komutu 

   ```PowerShell
    New-AzureRmResourceGroup -Name "VMSSWADtestGrp" -Location "<Azure Region>" 
   ```

   > [!NOTE]  
   > Özel ölçümler için etkin bir Azure bölgesi kullanmanız gerektiğini unutmayın. Bkz. 
 
1. VM dağıtmak için aşağıdaki komutları yürütün  
   > [!NOTE] 
   > Mevcut bir ölçek kümesini güncelleştirmek isterseniz, eklemeniz yeterlidir *-artımlı modu* sonuna aşağıdaki komutu. 
 
   ```PowerShell
   New-AzureRmResourceGroupDeployment -Name "VMSSWADTest" -ResourceGroupName "VMSSWADtestGrp" -TemplateFile "<File path of your azuredeploy.JSON file>" -TemplateParameterFile "<File path of your azuredeploy.parameters.JSON file>"  
   ```

1. Dağıtım başarılı olduktan sonra Azure portalında sanal makine ölçek kümesi bulmak olmalıdır ve Azure İzleyici ölçümlerine yayma. 

   > [!NOTE] 
   > Seçili vmSkuSize geçici hatalara çalıştırabilirsiniz. Böyle bir durumda azuredeploy.json dosyasını için geri dönün ve vmSkuSize parametresinin varsayılan değeri güncelleştirin. Bu durumda, "Standard_DS1_v2" çalışırken öneririz). 


## <a name="chart-your-metrics"></a>Ölçümlerinizin grafiğini 

1. Azure portalında oturum açın 

1. Sol taraftaki menüde **İzleyicisi** 

1. İzleyici sayfasında tıklayın **ölçümleri**. 

   ![Ölçümleri sayfası](./media/metrics-store-custom-rest-api/metrics.png) 

1. Toplama dönemini değiştirmek **son 30 dakika**.  

1. Kaynak açılan yeni oluşturduğunuz sanal makine ölçek kümesi'ni seçin.  

1. Aşağı açılan ad alanlarında seçin **azure.vm.windows.guest** 

1. Ölçümler, açılan menü, seçin **bellek\%Kaydedilmiş Bayt yüzdesi**.  

Ardından da bu ölçüm, belirli bir sanal makine ölçek kümesindeki için grafik ya da her bir VM ölçek kümesindeki çizmek için bu ölçüme göre boyutlar kullanmayı seçebilirsiniz. 



## <a name="next-steps"></a>Sonraki adımlar
- Daha fazla bilgi edinin [özel ölçümler](metrics-custom-overview.md).

