---
title: Bir Azure Resource Manager şablonu kullanarak Azure IOT Hub için ileti yönlendirmeyi yapılandırma | Microsoft Docs
description: Bir Azure Resource Manager şablonu kullanarak Azure IOT Hub için ileti yönlendirmeyi yapılandırma
author: robinsh
manager: philmeagit st
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: d7b8c0685cf92341241575d3e67c09a759f5c190
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59543772"
---
# <a name="tutorial-use-an-azure-resource-manager-template-to-configure-iot-hub-message-routing"></a>Öğretici: IOT Hub ileti yönlendirme yapılandırmak için bir Azure Resource Manager şablonu kullanma

[!INCLUDE [iot-hub-include-routing-intro](../../includes/iot-hub-include-routing-intro.md)]

[!INCLUDE [iot-hub-include-routing-create-resources](../../includes/iot-hub-include-routing-create-resources.md)]

## <a name="message-routing"></a>İleti yönlendirme

[!INCLUDE [iot-hub-include-create-routing-description](../../includes/iot-hub-include-create-routing-description.md)]

## <a name="download-the-template-and-parameters-file"></a>Şablon ve parametreleri dosyasını indirin

Bu öğreticinin ikinci bölümü, indirin ve IOT Hub'ına ileti göndermek için bir Visual Studio uygulamayı çalıştırın. Azure Resource Manager şablonu ve parametre dosyasını yanı sıra, Azure CLI ve PowerShell betikleri içeren, indirme klasörü yok.

Bir tane indirin [Azure IOT C# örnekleri](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) şimdi. Master.zip dosyanın sıkıştırmasını açın. Resource Manager şablonu ve parametreleri dosyası olan olarak//iot-hub/Tutorials/Routing/SimulatedDevice/resources **template_iothub.json** ve **template_iothub_parameters.json**.

## <a name="create-your-resources"></a>Kaynaklarınızı oluşturun

Tüm kaynaklarınızı oluşturmak için bir Azure Kaynak Yöneticisi (RM) şablonunu kullanma dağıtacağız. Azure CLI ve PowerShell betikleri birkaç satır aynı anda çalıştırabilirsiniz. RM şablonu, tek bir adımda dağıtılır. Bu makalede, ayrı ayrı her biri anlamanıza yardımcı olması için bölümleri gösterilmektedir. Ardından, şablonu dağıtmak ve test için sanal cihaz oluşturmak nasıl gösterilir. Şablon dağıtıldıktan sonra ileti yönlendirme yapılandırması portalında görüntüleyebilirsiniz.

IOT hub'ı adı ve depolama hesabı adı gibi genel olarak benzersiz olması gereken birkaç kaynak adları vardır. Adlandırma kaynakları daha kolay hale getirmek için bu kaynak adları geçerli tarih/saatten oluşturulan rastgele alfasayısal değer eklemek için ayarlanır. 

Şablon bakarsanız, burada değişkenleri için geçirilen parametre bu kaynakları ayarlanmıştır ve birleştirme görürsünüz *randomValue* parametre. 

Aşağıdaki bölümde, kullanılan parametreler açıklanmaktadır.

### <a name="parameters"></a>Parametreler

Bu parametreleri çoğunu varsayılan değerlere sahiptir. İle biten olanları **_in** ile birleştirilmiş *randomValue* genel olarak benzersiz olacak şekilde. 

**randomValue**: Bu değer, geçerli şablon dağıtırken tarih oluşturulur. Bu alan şablonunda oluşturuldukça Parametreler dosyasında değil.

**Subscriptionıd**: Bu alan, şablonu dağıttığınız aboneliğe ayarlanır. Bu alan, ayarlandığından Parametreler dosyasında değil.

**IoTHubName_in**: Genel olarak benzersiz olması randomValue ile birleştirilmiş temel IOT hub'ı ad alanıdır.

**Konum**: Bu alan, içine, "gibi westus" dağıtmakta olduğunuz Azure bölgesidir.

**consumer_group**: Bu alan, yönlendirme uç noktası aracılığıyla gelen iletiler için tüketici grubudur. Azure Stream analytics'te sonuçları filtrelemek için kullanılır. Örneğin, var. tüm akış her şeyi alın veya veri kümesine consumer_group ile gelen varsa **Contoso**, sonra da, bir Azure Stream Analytics akışı (ve Power BI raporu) yalnızca girişler gösterecek şekilde ayarlayabilirsiniz. Bu alan, bu öğreticinin 2 kullanılır.

**sku_name**: Bu alan için IOT hub'ı ölçeklendirme olur. Bu değer, S1 olmalıdır veya üzeri; Ücretsiz katmanı, birden fazla uç noktası izin vermediğinden bu öğretici için çalışmaz.

**sku_units**: Bu alan gider **sku_name**, kullanılabilir bir IOT Hub birimlerinin sayısı.

**d2c_partitions**: Olay akışı için kullanılan bölüm sayısı alandır.

**storageAccountName_in**: Bu alan, oluşturulacak depolama hesabının adıdır. İletiler, depolama hesabındaki bir kapsayıcıya yönlendirilir. Bu alan, küresel olarak benzersiz olacak şekilde randomValue ile birleştirilir.

**storageContainerName**: Bu alanı depolama hesabına yönlendirilmesi ileti depolandığı kapsayıcı adıdır.

**storage_endpoint**: İleti yönlendirme tarafından kullanılan depolama hesabı uç noktası için ad alanıdır.

**service_bus_namespace_in**: Oluşturulacak Service Bus ad alanıdır. Bu değer, genel olarak benzersiz olacak şekilde randomValue ile birleştirilir.

**service_bus_queue_in**: Bu alan iletileri yönlendirmek için kullanılan Service Bus kuyruğu adıdır. Bu değer, genel olarak benzersiz olacak şekilde randomValue ile birleştirilir.

**AuthRules_sb_queue**: Sıra için bağlantı dizesini almak için kullanılan hizmet veri yolu kuyruğu için yetkilendirme kuralları alandır.

### <a name="variables"></a>Değişkenler

Bu değerler, şablonda kullanılır ve çoğunlukla parametrelerinden türetilir.

**queueAuthorizationRuleResourceId**: Service Bus kuyruğu için yetkilendirme kuralı için ResourceId alandır. ResourceId sırayla sıra için bağlantı dizesini almak için kullanılır.

**iotHubName**: Bu alan birleştirilmiş randomValue atandıktan sonra IOT hub'ı adıdır. 

**StorageAccountName**: Bu alan birleştirilmiş randomValue atandıktan sonra depolama hesabının adıdır. 

**service_bus_namespace**: Bu alan, birleştirilmiş randomValue atandıktan sonra ad alanıdır.

**service_bus_queue**: Bu alan, birleştirilmiş randomValue atandıktan sonra Service Bus kuyruk adı olur.

**sbVersion**: Kullanılacak hizmet veri yolu API'sini sürümü. Bu durumda, "2017-04-01" olur.

### <a name="resources-storage-account-and-container"></a>Kaynaklar: Depolama hesabı ve kapsayıcı

Oluşturulan ilk kaynak iletileri yönlendirilen kapsayıcısı ile birlikte depolama hesabıdır. Kapsayıcı, depolama hesabı altındaki bir kaynaktır. Bunun bir `dependsOn` yan tümcesi depolama hesabı için depolama hesabı gerekmeden, kapsayıcıya önce oluşturulabilir.

Bu bölümde benzer aşağıda verilmiştir:

```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2018-07-01",
    "location": "[parameters('location')]",
    "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
    },
    "kind": "Storage",
    "properties": {},
    "resources": [
        {
        "type": "blobServices/containers",
        "apiVersion": "2018-07-01",
        "name": "[concat('default/', parameters('storageContainerName'))]",
        "properties": {
            "publicAccess": "None"
            } ,
        "dependsOn": [
            "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
            ]
        }
    ]
}
```

### <a name="resources-service-bus-namespace-and-queue"></a>Kaynaklar: Service Bus ad alanı ve kuyruk

Oluşturulan ikinci Service Bus ad alanı, Service Bus kuyruğuna iletiler yönlendirilen yanı sıra bir kaynaktır. SKU standart olarak ayarlanır. API sürümü değişkenlerinden alınır. Ayrıca, bu bölümde (durum: etkin) dağıttığında, Service Bus ad alanı etkinleştirmek için ayarlanır. 

```json
{
    "type": "Microsoft.ServiceBus/namespaces",
    "comments": "The Sku should be 'Standard' for this tutorial.",
    "sku": {
        "name": "Standard",
        "tier": "Standard"
    },
    "name": "[variables('service_bus_namespace')]",
    "apiVersion": "[variables('sbVersion')]",
    "location": "[parameters('location')]",
    "properties": {
        "provisioningState": "Succeeded",
        "metricId": "[concat('a4295411-5eff-4f81-b77e-276ab1ccda12:', variables('service_bus_namespace'))]",
        "serviceBusEndpoint": "[concat('https://', variables('service_bus_namespace'),'.servicebus.windows.net:443/')]",
        "status": "Active"
    },
    "dependsOn": []
}
```

Bu bölümde, Service Bus kuyruğu oluşturur. Bu komut dosyasının parçası olan bir `dependsOn` ad alanı sağlar yan tümcesi, kuyrukta oluşturulur.

```json
{
    "type": "Microsoft.ServiceBus/namespaces/queues",
    "name": "[concat(variables('service_bus_namespace'), '/', variables('service_bus_queue'))]",
    "apiVersion": "[variables('sbVersion')]",
    "location": "[parameters('location')]",
    "scale": null,
    "properties": {},
    "dependsOn": [
        "[resourceId('Microsoft.ServiceBus/namespaces', variables('service_bus_namespace'))]"
    ]
}
```

### <a name="resources-iot-hub-and-message-routing"></a>Kaynaklar: IOT Hub ve ileti yönlendirme

Depolama hesabı ve Service Bus kuyruğu oluşturulan, iletileri kendisine yönlendiren IOT hub'ı oluşturun. RM şablonu kullanan `dependsOn` Service Bus kaynakları ve depolama hesabı oluşturmadan önce hub'ı oluşturma dener biçimde yan tümceleri. 

IOT hub'ı bölümün ilk bölümü aşağıda verilmiştir. Bu şablonun parçası bağımlılıklarını ayarlar ve özellikler ile başlar.

```json
{
    "apiVersion": "2018-04-01",
    "type": "Microsoft.Devices/IotHubs",
    "name": "[variables('IoTHubName')]",
    "location": "[parameters('location')]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "[resourceId('Microsoft.ServiceBus/namespaces', variables('service_bus_namespace'))]",
        "[resourceId('Microsoft.ServiceBus/namespaces/queues', variables('service_bus_namespace'), variables('service_bus_queue'))]"
    ],
    "properties": {
        "eventHubEndpoints": {}
            "events": {
                "retentionTimeInDays": 1,
                "partitionCount": "[parameters('d2c_partitions')]"
                }
            },
```

Sonraki bölümde IOT Hub ileti yönlendirme yapılandırması için bir bölümüdür. Öncelikle uç noktalar için bölümüdür. Service Bus kuyruğu ve bağlantı dizeleri dahil olmak üzere depolama hesabına yönlendirme uç noktaları bu şablonunun parçası ayarlar.

Sıra için bağlantı dizesi oluşturmak için alınan satır içi olduğu queueAuthorizationRulesResourcedId gerekir. Depolama hesabı için bağlantı dizesi oluşturmak için birincil depolama anahtarı almak ve sonra bağlantı dizesini biçiminde kullanın.

Blob biçimi ayarlandığı uç nokta yapılandırması da olduğu `AVRO` veya `JSON`.

[!INCLUDE [iot-hub-include-blob-storage-format](../../includes/iot-hub-include-blob-storage-format.md)]

 ```json
"routing": {
    "endpoints": {
        "serviceBusQueues": [
        {
            "connectionString": "[Concat('Endpoint=sb://',variables('service_bus_namespace'),'.servicebus.windows.net/;SharedAccessKeyName=',parameters('AuthRules_sb_queue'),';SharedAccessKey=',listkeys(variables('queueAuthorizationRuleResourceId'),variables('sbVersion')).primaryKey,';EntityPath=',variables('service_bus_queue'))]",
            "name": "[parameters('service_bus_queue_endpoint')]",
            "subscriptionId": "[parameters('subscriptionId')]", 
            "resourceGroup": "[resourceGroup().Name]"
        }
        ],
        "serviceBusTopics": [],
        "eventHubs": [],
        "storageContainers": [
            {
                "connectionString": 
                "[Concat('DefaultEndpointsProtocol=https;AccountName=',variables('storageAccountName'),';AccountKey=',listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName')), providers('Microsoft.Storage', 'storageAccounts').apiVersions[0]).keys[0].value)]",
                "containerName": "[parameters('storageContainerName')]",
                "fileNameFormat": "{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}",
                "batchFrequencyInSeconds": 100,
                "maxChunkSizeInBytes": 104857600,
                "encoding": "avro",
                "name": "[parameters('storage_endpoint')]",
                "subscriptionId": "[parameters('subscriptionId')]",
                "resourceGroup": "[resourceGroup().Name]"
            }
        ]
    },
```

Sonraki bölümde, uç noktalara ileti yollarını içindir. Bir kümesi var. Yukarı her uç nokta için bir Service Bus kuyruğu için ve bir depolama hesabı kapsayıcısının olmasını

Depolama'ya yönlendirilen iletileri için sorgu koşulu olduğunu unutmayın `level="storage"`, ve Service Bus kuyruğuna yönlendirilen iletileri için sorgu koşulu `level="critical"`.

```json
"routes": [
    {
        "name": "contosoStorageRoute",
        "source": "DeviceMessages",
        "condition": "level=\"storage\"",
        "endpointNames": [
            "[parameters('storage_endpoint')]"
            ],
        "isEnabled": true
    },
    {
        "name": "contosoSBQueueRoute",
        "source": "DeviceMessages",
        "condition": "level=\"critical\"",
        "endpointNames": [
            "[parameters('service_bus_queue_endpoint')]"
            ],
        "isEnabled": true
    }
],
```

Bu json varsayılan bilgileri ve hub için SKU içeren IOT hub'ı bölümün geri kalanında gösterir.

```json
            "fallbackRoute": {
                "name": "$fallback",
                "source": "DeviceMessages",
                "condition": "true",
                "endpointNames": [
                    "events"
                ],
                "isEnabled": true
            }
        },
        "storageEndpoints": {
            "$default": {
                "sasTtlAsIso8601": "PT1H",
                "connectionString": "",
                "containerName": ""
            }
        },
        "messagingEndpoints": {
            "fileNotifications": {
                "lockDurationAsIso8601": "PT1M",
                "ttlAsIso8601": "PT1H",
                "maxDeliveryCount": 10
            }
        },
        "enableFileUploadNotifications": false,
        "cloudToDevice": {
            "maxDeliveryCount": 10,
            "defaultTtlAsIso8601": "PT1H",
            "feedback": {
                "lockDurationAsIso8601": "PT1M",
                "ttlAsIso8601": "PT1H",
                "maxDeliveryCount": 10
            }
        }
    },
    "sku": {
        "name": "[parameters('sku_name')]",
        "capacity": "[parameters('sku_units')]"
    }
}
```

### <a name="resources-service-bus-queue-authorization-rules"></a>Kaynaklar: Service Bus kuyruğu yetkilendirme kuralları

Service Bus kuyruğu yetkilendirme kuralı, Service Bus kuyruğu için bağlantı dizesini almak için kullanılır. Bunu kullanan bir `dependsOn` yan tümcesi emin olmak için önce Service Bus ad alanı ve Service Bus kuyruğu oluşturulmadı.

```json
{
    "type": "Microsoft.ServiceBus/namespaces/queues/authorizationRules",
    "name": "[concat(variables('service_bus_namespace'), '/', variables('service_bus_queue'), '/', parameters('AuthRules_sb_queue'))]",
    "apiVersion": "[variables('sbVersion')]",
    "location": "[parameters('location')]",
    "scale": null,
    "properties": {
        "rights": [
            "Send"
        ]
    },
    "dependsOn": [
        "[resourceId('Microsoft.ServiceBus/namespaces', variables('service_bus_namespace'))]",
        "[resourceId('Microsoft.ServiceBus/namespaces/queues', variables('service_bus_namespace'), variables('service_bus_queue'))]"
    ]
},
```

### <a name="resources-consumer-group"></a>Kaynaklar: Tüketici grubu

Bu bölümde, bu öğreticinin ikinci bölümünde Azure Stream Analytics tarafından kullanılacak bir tüketici grubu için IOT hub'ı veri oluşturun.

```json
{
    "type": "Microsoft.Devices/IotHubs/eventHubEndpoints/ConsumerGroups",
    "name": "[concat(variables('iotHubName'), '/events/',parameters('consumer_group'))]",
    "apiVersion": "2018-04-01",
    "dependsOn": [
        "[concat('Microsoft.Devices/IotHubs/', variables('iotHubName'))]"
    ]
}
```

### <a name="resources-outputs"></a>Kaynaklar: Çıkışlar

Görüntülenecek geri dağıtım betiği için bir değer göndermek istiyorsanız, bir çıkış bölümü kullanın. Bu şablon parçası, Service Bus kuyruğu için bağlantı dizesini döndürür. Bir değer gerekli değildir döndüren, çağıran kodun sonuçları döndürmek nasıl bir örnek olarak eklemiştir.

```json
"outputs": {
    "sbq_connectionString": {
      "type": "string",
      "value": "[Concat('Endpoint=sb://',variables('service_bus_namespace'),'.servicebus.windows.net/;SharedAccessKeyName=',parameters('AuthRules_sb_queue'),';SharedAccessKey=',listkeys(variables('queueAuthorizationRuleResourceId'),variables('sbVersion')).primaryKey,';EntityPath=',variables('service_bus_queue'))]"
    }
  }
```

## <a name="deploy-the-rm-template"></a>RM şablonu dağıtma

Şablonu Azure'a dağıtmak için şablonu ve parametre dosyasını Azure Cloud shell'e yüklemeniz ve sonra şablonu dağıtmak için bir komut yürütün. Azure Cloud Shell'i açın ve oturum açın. Bu örnek PowerShell'i kullanmaktadır.

Dosyaları karşıya yüklemek için seçin **dosyaları karşıya yükleme/indirme** simgesi menü çubuğundaki karşıya yükleme seçin.

![Cloud Shell menü çubuğu vurgulanmış dosyaları karşıya yükleme/indirme](media/tutorial-routing-config-message-routing-RM-template/CloudShell_upload_files.png)

Yerel diskinizde dosyaları bulmak ve onları seçin ve ardından açılır dosya Gezgini'ni **açık**.

Dosyalar karşıya yüklendikten sonra sonuçları iletişim aşağıdaki görüntüye benzer bir şey gösterir.

![Cloud Shell menü çubuğu vurgulanmış dosyaları karşıya yükleme/indirme](media/tutorial-routing-config-message-routing-RM-template/CloudShell_upload_results.png)

Dosyalar, Cloud Shell'i örneğiniz tarafından kullanılan paylaşımına yüklenir. 

Dağıtım gerçekleştirmek için betiği çalıştırın. Bu komut dosyasının son satırının döndürülmesi için--Service Bus kuyruğu bağlantı dizesi ayarlanmış değişken alır.

Bu değişkenler bu komut dosyasında ayarlanır.

**$RGName** şablonu dağıtmak hangi kaynak grubu adı. Bu alan, şablonu dağıtmadan önce oluşturulur.

**$location** "westus" gibi bir şablon için kullanılacak Azure konumdur.

**deploymentname** bir addır döndüren değişken değerini almak için dağıtımına atanamıyor.

PowerShell Betiği aşağıda verilmiştir. Bu PowerShell Betiği kopyalayın ve Cloud Shell penceresine yapıştırın ve sonra çalıştırmak için Enter tuşuna basın.

```powershell
$RGName="ContosoResources"
$location = "westus"
$deploymentname="contoso-routing"

# Remove the resource group if it already exists. 
#Remove-AzResourceGroup -name $RGName 
# Create the resource group.
New-AzResourceGroup -name $RGName -Location $location 

# Set a path to the parameter file. 
$parameterFile = "$HOME/template_iothub_parameters.json"
$templateFile = "$HOME/template_iothub.json"

# Deploy the template.
New-AzResourceGroupDeployment `
    -Name $deploymentname `
    -ResourceGroupName $RGName `
    -TemplateParameterFile $parameterFile `
    -TemplateFile $templateFile `
    -verbose

# Get the returning value of the connection string.
(Get-AzResourceGroupDeployment -ResourceGroupName $RGName -Name $deploymentname).Outputs.sbq_connectionString.value
```

Betik hataları varsa, betik üzerinde yerel olarak düzenleme, yeniden Cloud shell'e yükleyin ve betiğini yeniden çalıştırın. Betik çalıştırma işlemi başarıyla sonlandıktan sonra sonraki adıma devam edin.

## <a name="create-simulated-device"></a>Sanal cihaz oluşturma

[!INCLUDE [iot-hub-include-create-simulated-device-portal](../../includes/iot-hub-include-create-simulated-device-portal.md)]

## <a name="view-message-routing-in-the-portal"></a>Portalda ileti yönlendirme görüntüleyin

[!INCLUDE [iot-hub-include-view-routing-in-portal](../../includes/iot-hub-include-view-routing-in-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Tüm kaynakları ayarlamak ve ileti yollarını yapılandırılmış göre işlemek ve yönlendirilmiş iletiler hakkındaki bilgileri görüntülemek hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [2. Kısım - ileti yönlendirme sonuçlarını görüntüleme](tutorial-routing-view-message-routing-results.md)
