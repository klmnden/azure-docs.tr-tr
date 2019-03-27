---
title: Azure PowerShell kullanarak Stream Analytics işi oluşturma
description: Bu hızlı başlangıçta, dağıtmak ve Azure Stream Analytics işi çalıştırmak için Azure PowerShell modülünü nasıl yapılacağı açıklanır.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 12/20/2018
ms.topic: quickstart
ms.service: stream-analytics
ms.custom: mvc
ms.openlocfilehash: f46f437ffd79ae9d0457606a72719ef13314aa1c
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58442962"
---
# <a name="quickstart-create-a-stream-analytics-job-using-azure-powershell"></a>Hızlı Başlangıç: Azure PowerShell kullanarak Stream Analytics işi oluşturma

Azure PowerShell modülü, PowerShell cmdlet'leri veya betikleri kullanarak Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta Azure PowerShell modülü kullanarak bir Azure Stream Analytics işi dağıtma ve çalıştırma işleminin ayrıntıları verilmektedir.

Örnek Proje bir IOT Hub CİHAZDAN akış verilerini okur. Giriş verilerini bir Raspberry Pi çevrimiçi simülatör tarafından oluşturulur. Ardından, Stream Analytics işi 27 ° büyük bir sıcaklık iletilerine filtre uygulamak için Stream Analytics sorgu dili kullanarak verileri dönüştürür. Son olarak, blob depolama alanındaki bir dosyaya elde edilen çıkış olayları yazar.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

* Bu hızlı başlangıçta, Azure PowerShell modülünü gerektirir. Yerel makinenizde yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](https://docs.microsoft.com/powershell/azure/install-Az-ps).

* Bazı IOT Hub eylemleri Azure PowerShell tarafından desteklenmez ve tamamlanan kullanarak Azure CLI sürümünü 2.0.24 veya sonraki bir sürümü gerekir ve Azure CLI için IOT uzantısı. [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ve `az extension add --name azure-cli-iot-ext` IOT uzantısını yüklemek için.


## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure aboneliğinizde oturum açın `Connect-AzAccount` komutunu ve açılır tarayıcı penceresine Azure kimlik bilgilerinizi girin:

```powershell
# Connect to your Azure account
Connect-AzAccount
```

Birden fazla aboneliğiniz varsa aşağıdaki cmdlet'leri çalıştırarak bu hızlı başlangıç için kullanmak istediğiniz aboneliği seçin. `<your subscription name>` değerini aboneliğinizin adıyla değiştirdiğinizden emin olun:

```powershell
# List all available subscriptions.
Get-AzSubscription

# Select the Azure subscription you want to use to create the resource group and resources.
Get-AzSubscription -SubscriptionName "<your subscription name>" | Select-AzSubscription
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir Azure kaynak grubu oluşturun [yeni AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup). Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```powershell
$resourceGroup = "StreamAnalyticsRG"
$location = "WestUS2"
New-AzResourceGroup `
    -Name $resourceGroup `
    -Location $location
```

## <a name="prepare-the-input-data"></a>Girdi verilerini hazırlama

Stream Analytics işini tanımlamadan önce, işe girdi olarak yapılandırılan verileri hazırlayın.

Aşağıdaki Azure CLI kod bloğu, iş için gereken girdi verilerini hazırlamak için birçok komutları yapar. Kodu anlamak için bölümleri gözden geçirin.

1. PowerShell penceresinde çalıştırın [az login](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest) Azure hesabınızda oturum açmak için komutu.

    Başarıyla oturum açtığınızda, Azure CLI, aboneliklerinizin listesini döndürür. Bu hızlı başlangıcı ve çalıştırma için kullanmakta olduğunuz aboneliği kopyalama [az hesabı kümesi](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#change-the-active-subscription) bu aboneliği seçmek için komutu. PowerShell ile bir önceki bölümdeki seçtiğiniz aynı aboneliği seçin. Değiştirdiğinizden emin olun `<your subscription name>` aboneliğinizin adıyla.

    ```azurecli
    az login

    az account set --subscription "<your subscription>"
    ```

2. Kullanarak IOT Hub oluşturma [az IOT hub oluşturma](../iot-hub/iot-hub-create-using-cli.md#create-an-iot-hub) komutu. Bu örnek adlı bir IOT Hub oluşturulur **MyASAIoTHub**. IOT hub'ı adlarının benzersiz olduğundan, kendi IOT hub'ı adınızla görünmesi gerekir. SKU aboneliğinizle varsa ücretsiz katmanı kullanmak için F1'e ayarlayın. Aksi halde, sonraki en düşük katmanlı seçin.

    ```azurecli
    az iot hub create --name "<your IoT Hub name>" --resource-group $resourceGroup --sku S1
    ```

    IOT Hub bağlantı dizesini kullanarak IOT hub'ı oluşturulduktan sonra alma [az iot hub show-connection-string](https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest) komutu. Tüm bağlantı dizesini kopyalayın ve IOT Hub, Stream Analytics işinin girdisi olarak eklediğinizde için kaydedin.

    ```azurecli
    az iot hub show-connection-string --hub-name "MyASAIoTHub"
    ```

3. Kullanarak IOT hub'a bir cihaz eklemek [az ıothub cihaz kimliği oluşturma](../iot-hub/quickstart-send-telemetry-c.md#register-a-device) komutu. Bu örnek adlı bir cihaz oluşturur **MyASAIoTDevice**.

    ```azurecli
    az iot hub device-identity create --hub-name "MyASAIoTHub" --device-id "MyASAIoTDevice"
    ```

4. Cihaz bağlantı dizesi kullanarak [az IOT hub cihaz kimliği show-connection-string](/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity#ext-azure-cli-iot-ext-az-iot-hub-device-identity-show-connection-string) komutu. Tüm bağlantı dizesini kopyalayın ve Raspberry Pi simülatör oluştururken için kaydedin.

    ```azurecli
    az iot hub device-identity show-connection-string --hub-name "MyASAIoTHub" --device-id "MyASAIoTDevice" --output table
    ```

    **Örnek çıktı:**

    ```azurecli
    HostName=MyASAIoTHub.azure-devices.net;DeviceId=MyASAIoTDevice;SharedAccessKey=a2mnUsg52+NIgYudxYYUNXI67r0JmNubmfVafojG8=
    ```

## <a name="create-blob-storage"></a>BLOB Depolama oluşturma

Aşağıdaki Azure PowerShell kod bloğu, iş çıktısı için kullanılan blob depolama alanı oluşturmak için komutları kullanır. Kodu anlamak için bölümleri gözden geçirin.

1. Genel amaçlı standart depolama hesabı kullanarak oluşturduğunuz [yeni AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/New-azStorageAccount) cmdlet'i.  Bu örnek adlı bir depolama hesabı oluşturur **myasaquickstartstorage** ile yerel olarak yedekli depolama(lrs) ve blob şifrelemesi (varsayılan olarak etkindir).

2. Kullanılacak depolama hesabını tanımlayan `$storageAccount.Context` depolama hesabı bağlamını alın. Depolama hesaplarıyla çalışırken kimlik bilgilerini tekrar tekrar sağlamak yerine bağlama başvurursunuz.

3. Kullanarak bir depolama kapsayıcısı oluşturma [yeni AzStorageContainer](https://docs.microsoft.com/powershell/module/az.storage/new-azstoragecontainer).

4. Kod tarafından yüzdelik depolama anahtarı kopyalayın ve daha sonra akış işin çıktı oluşturmak için bu anahtara kaydedin.

    ```powershell
    $storageAccountName = "myasaquickstartstorage"
    $storageAccount = New-AzStorageAccount `
      -ResourceGroupName $resourceGroup `
      -Name $storageAccountName `
      -Location $location `
      -SkuName Standard_LRS `
      -Kind Storage

    $ctx = $storageAccount.Context
    $containerName = "container1"

    New-AzStorageContainer `
      -Name $containerName `
      -Context $ctx

    $storageAccountKey = (Get-AzStorageAccountKey `
      -ResourceGroupName $resourceGroup `
      -Name $storageAccountName).Value[0]

    Write-Host "The <storage account key> placeholder needs to be replaced in your output json files with this key value:"
    Write-Host $storageAccountKey -ForegroundColor Cyan
    ```

## <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

İle bir Stream Analytics işi oluşturma [yeni AzStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/az.streamanalytics/new-azstreamanalyticsjob) cmdlet'i. Bu cmdlet iş adı, kaynak grubu adı ve iş tanımını parametre olarak alır. İş adı, işinizi tanımlayan herhangi bir kolay ad olabilir. Alfasayısal karakterler, tire, olabilir ve alt çizgi ve 3 ila 63 karakter uzunluğunda olmalıdır. İş tanımı bir iş oluşturmak için gereken özellikleri içeren bir JSON dosyasıdır. Yerel makinenizde `JobDefinition.json` adlı bir dosya oluşturun ve içine aşağıdaki JSON verilerini ekleyin:

```json
{
  "location":"WestUS2",
  "properties":{
    "sku":{
      "name":"standard"
    },
    "eventsOutOfOrderPolicy":"adjust",
    "eventsOutOfOrderMaxDelayInSeconds":10,
    "compatibilityLevel": 1.1
  }
}
```

Ardından, `New-AzStreamAnalyticsJob` cmdlet'ini çalıştırın. Değiştirin `jobDefinitionFile` iş tanımı JSON dosyasını sakladığınız yoluyla değişken.

```powershell
$jobName = "MyStreamingJob"
$jobDefinitionFile = "C:\JobDefinition.json"
New-AzStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -File $jobDefinitionFile `
  -Name $jobName `
  -Force
```

## <a name="configure-input-to-the-job"></a>İş girdisini yapılandırma

Kullanarak işinize bir girdi ekleyin [yeni AzStreamAnalyticsInput](https://docs.microsoft.com/powershell/module/az.streamanalytics/new-azstreamanalyticsinput) cmdlet'i. Bu cmdlet iş adı, iş girdisi adı, kaynak grubu adı ve iş girdisi tanımını parametre olarak alır. İş girdisi tanımı işin girdisini yapılandırmak için gereken özellikleri içeren bir JSON dosyasıdır. Bu örnekte, giriş olarak bir blob depolama alanı oluşturacaksınız.

Yerel makinenizde `JobInputDefinition.json` adlı bir dosya oluşturun ve içine aşağıdaki JSON verilerini ekleyin. Değerini değiştirdiğinizden emin olun `accesspolicykey` ile `SharedAccessKey` önceki bölümde kaydettiğiniz IOT Hub bağlantı dizesine kısmı.

```json
{
    "properties": {
        "type": "Stream",
        "datasource": {
            "type": "Microsoft.Devices/IotHubs",
            "properties": {
                "iotHubNamespace": "MyASAIoTHub",
                "sharedAccessPolicyName": "iothubowner",
                "sharedAccessPolicyKey": "accesspolicykey",
                "endpoint": "messages/events",
                "consumerGroupName": "$Default"
                }
        },
        "compression": {
            "type": "None"
        },
        "serialization": {
            "type": "Json",
            "properties": {
                "encoding": "UTF8"
            }
        }
    },
    "name": "IoTHubInput",
    "type": "Microsoft.StreamAnalytics/streamingjobs/inputs"
}
```

Ardından, çalıştırma `New-AzStreamAnalyticsInput` cmdlet'ini değerini değiştirdiğinizden emin olun `jobDefinitionFile` iş girdisi tanımı JSON dosyasını sakladığınız yoluyla değişken.

```powershell
$jobInputName = "IoTHubInput"
$jobInputDefinitionFile = "C:\JobInputDefinition.json"
New-AzStreamAnalyticsInput `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobInputDefinitionFile `
  -Name $jobInputName
```

## <a name="configure-output-to-the-job"></a>İş çıktısını yapılandırma

Kullanarak işinize bir çıktı eklemek [yeni AzStreamAnalyticsOutput](https://docs.microsoft.com/powershell/module/az.streamanalytics/new-azstreamanalyticsoutput) cmdlet'i. Bu cmdlet iş adı, iş çıktısı adı, kaynak grubu adı ve iş çıktısı tanımını parametre olarak alır. İş çıktısı tanımı işin çıktısını yapılandırmak için gereken özellikleri içeren bir JSON dosyasıdır. Bu örnek, çıktı olarak blob depolama kullanır.

Yerel makinenizde `JobOutputDefinition.json` adlı bir dosya oluşturun ve içine aşağıdaki JSON verilerini ekleyin. `accountKey` değerini, depolama hesabınızın $storageAccountKey değerinde depolanmış değeri olan erişim anahtarıyla değiştirdiğinizden emin olun.

```json
{
    "properties": {
        "datasource": {
            "type": "Microsoft.Storage/Blob",
            "properties": {
                "storageAccounts": [
                    {
                      "accountName": "asaquickstartstorage",
                      "accountKey": "<storage account key>"
                    }
                ],
                "container": "container1",
                "pathPattern": "output/",
                "dateFormat": "yyyy/MM/dd",
                "timeFormat": "HH"
            }
        },
        "serialization": {
            "type": "Json",
            "properties": {
                "encoding": "UTF8",
                "format": "LineSeparated"
            }
        }
    },
    "name": "BlobOutput",
    "type": "Microsoft.StreamAnalytics/streamingjobs/outputs"
}
```

Ardından, `New-AzStreamAnalyticsOutput` cmdlet'ini çalıştırın. `jobOutputDefinitionFile` değişkeninin değerini, iş çıktısı tanımı JSON dosyasını depoladığınız yol ile değiştirdiğinizden emin olun.

```powershell
$jobOutputName = "BlobOutput"
$jobOutputDefinitionFile = "C:\JobOutputDefinition.json"
New-AzStreamAnalyticsOutput `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobOutputDefinitionFile `
  -Name $jobOutputName -Force
```

## <a name="define-the-transformation-query"></a>Dönüşüm sorgusunu tanımlama

Kullanarak işinize bir dönüşüm ekleyin [yeni AzStreamAnalyticsTransformation](https://docs.microsoft.com/powershell/module/az.streamanalytics/new-azstreamanalyticstransformation) cmdlet'i. Bu cmdlet iş adı, iş dönüşümü adı, kaynak grubu adı ve iş dönüşümü tanımını parametre olarak alır. Yerel makinenizde `JobTransformationDefinition.json` adlı bir dosya oluşturun ve içine aşağıdaki JSON verilerini ekleyin. JSON dosyası, dönüşüm sorgusunu tanımlayan bir sorgu parametresi içerir:

```json
{
    "name":"MyTransformation",
    "type":"Microsoft.StreamAnalytics/streamingjobs/transformations",
    "properties":{
        "streamingUnits":1,
        "script":null,
        "query":" SELECT * INTO BlobOutput FROM IoTHubInput HAVING Temperature > 27"
    }
}
```

Ardından, `New-AzStreamAnalyticsTransformation` cmdlet'ini çalıştırın. Değerini değiştirdiğinizden emin olun `jobTransformationDefinitionFile` iş dönüşüm tanımı JSON dosyasını sakladığınız yoluyla değişken.

```powershell
$jobTransformationName = "MyJobTransformation"
$jobTransformationDefinitionFile = "C:\JobTransformationDefinition.json"
New-AzStreamAnalyticsTransformation `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobTransformationDefinitionFile `
  -Name $jobTransformationName -Force
```
## <a name="run-the-iot-simulator"></a>IOT simülatör'ü çalıştırma

1. Açık [Raspberry Pi Azure IOT çevrimiçi simülatör](https://azure-samples.github.io/raspberry-pi-web-simulator/).

2. Satır 15 yer tutucunun bir önceki bölümde kaydettiğiniz tüm Azure IOT Hub cihaz bağlantı dizesiyle değiştirin.

3. **Çalıştır**’a tıklayın. Çıktı, IOT Hub'ına gönderilen iletileri ve sensör verilerini göstermelidir.

    ![Raspberry Pi Azure IOT çevrimiçi simülatör](./media/stream-analytics-quick-create-powershell/ras-pi-connection-string.png)

## <a name="start-the-stream-analytics-job-and-check-the-output"></a>Stream Analytics işini başlatıp çıktıyı denetleyin

Kullanarak işi başlatın [başlangıç AzStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/az.streamanalytics/start-azstreamanalyticsjob) cmdlet'i. Bu cmdlet iş adı, kaynak grubu adı, çıktı başlangıç modu ve başlangıç saatini parametre olarak alır. `OutputStartMode`; `JobStartTime`, `CustomTime` veya `LastOutputEventTime` değerlerini kabul eder. Bu değerlerin her birinin ne anlama geldiği hakkında daha fazla bilgi için PowerShell belgelerindeki [parametreler](https://docs.microsoft.com/powershell/module/az.streamanalytics/start-azstreamanalyticsjob) bölümüne bakın.

Aşağıdaki cmdlet’i çalıştırdıktan sonra iş başlarsa çıktı olarak `True` değeri döndürülür. Depolama kapsayıcısında, dönüştürülmüş verilerle birlikte bir çıktı klasörü oluşturulur.

```powershell
Start-AzStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -Name $jobName `
  -OutputStartMode 'JobStartTime'
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, akış işini ve tüm ilgili kaynakları silin. İşin silinmesi, iş tarafından kullanılan akış birimlerinin faturalanmasını önler. İşi gelecekte kullanmayı planlıyorsanız, silme işlemini atlayıp işi şimdilik durdurabilirsiniz. Bu işi kullanmaya devam edecek değilseniz, aşağıdaki cmdlet'i çalıştırarak bu hızlı başlangıç ile oluşturulan tüm kaynakları silin:

```powershell
Remove-AzResourceGroup `
  -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta PowerShell kullanarak basit bir Stream Analytics işi dağıttınız. Stream Analytics işlerini [Azure portalı](stream-analytics-quick-create-portal.md) ve [Visual Studio](stream-analytics-quick-create-vs.md)’yu kullanarak da dağıtabilirsiniz.

Diğer girdi kaynaklarını yapılandırma ve gerçek zamanlı algılama hakkında bilgi almak için aşağıdaki makaleye geçin:

> [!div class="nextstepaction"]
> [Azure Stream Analytics kullanarak gerçek zamanlı sahtekarlık algılama](stream-analytics-real-time-fraud-detection.md)
