---
title: Azure PowerShell kullanarak Stream Analytics işi oluşturma
description: Bu hızlı başlangıçta Azure PowerShell modülü kullanarak bir Azure Stream Analytics işi dağıtma ve çalıştırma işleminin ayrıntıları verilmektedir.
services: stream-analytics
author: SnehaGunda
ms.author: sngun
ms.date: 05/14/2018
ms.topic: quickstart
ms.service: stream-analytics
ms.custom: mvc
manager: kfile
ms.openlocfilehash: 2b5d8bfd6dbe36637a0c6873e941118e7ee71b80
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34212441"
---
# <a name="quickstart-create-a-stream-analytics-job-by-using-azure-powershell"></a>Hızlı başlangıç: Azure PowerShell kullanarak Stream Analytics işi oluşturma

Azure PowerShell modülü, PowerShell cmdlet’leri veya betikleri kullanarak Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta Azure PowerShell modülü kullanarak bir Azure Stream Analytics işi dağıtma ve çalıştırma işleminin ayrıntıları verilmektedir. 

Örnek iş, Azure blob depolamadaki bir blobdan akış verilerini okur. Bu hızlı başlangıçta kullanılan girdi verileri dosyası yalnızca tanım amaçlı statik veriler içerir. Gerçek dünya senaryolarında bir Stream Analytics işi için akışı yapılan girdi verilerini kullanırsınız. Ardından iş, 100°’nin üzerinde olduğunda ortalama sıcaklığı hesaplamak için Stream Analytics sorgu dilini kullanarak verileri dönüştürür. Son olarak, el edilen çıktıyı başka bir dosyaya yazar. 

## <a name="before-you-begin"></a>Başlamadan önce

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.  

* Bu hızlı başlangıç, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Yerel makinenizde yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse [Azure PowerShell Modülü yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) makalesine bakın. 


## <a name="sign-in-to-azure"></a>Azure'da oturum açma

`Connect-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve açılır tarayıcı penceresine Azure kimlik bilgilerinizi girin:

```powershell
# Log in to your Azure account
Connect-AzureRmAccount
```

Oturum açtıktan sonra, birden çok aboneliğiniz varsa aşağıdaki cmdlet’leri çalıştırarak bu hızlı başlangıç için kullanmak istediğiniz aboneliği seçin. <your subscription name> değerini aboneliğinizin adıyla değiştirdiğinizden emin olun:  

```powershell
# List all available subscriptions.
Get-AzureRmSubscription

# Select the Azure subscription you want to use to create the resource group and resources.
Get-AzureRmSubscription -SubscriptionName "<your subscription name>" | Select-AzureRmSubscription
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) ile yeni bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```powershell
$resourceGroup = "StreamAnalyticsRG"
$location = "WestUS2"
New-AzureRmResourceGroup `
   -Name $resourceGroup `
   -Location $location 
```

## <a name="prepare-the-input-data"></a>Girdi verilerini hazırlama

Stream Analytics işini tanımlamadan önce, işe girdi olarak yapılandırılan verileri hazırlayın.

1. GitHub’dan [sensör örnek verilerini](https://raw.githubusercontent.com/Azure/azure-stream-analytics/master/Samples/GettingStarted/HelloWorldASA-InputStream.json) indirin. Bağlantıya sağ tıklayın ve **Bağlantıyı Farklı Kaydet...** ya da **Hedefi farklı kaydet**’i seçin.

2. Aşağıdaki PowerShell kod bloğu, iş için gereken giriş verilerini hazırlamak üzere birden fazla komut yürütür. Kodu anlamak için bölümleri gözden geçirin. 

   1. [New-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/New-AzureRmStorageAccount) cmdlet’ini kullanarak standart bir genel amaçlı depolama hesabı oluşturun.  Bu örnek, yerel olarak yedekli depolama(LRS) ve blob şifrelemesi (varsayılan olarak etkindir) ile mystorageaccount adlı bir depolama hesabı oluşturur.  

   2. Kullanılacak depolama hesabını tanımlayan `$storageAccount.Context` depolama hesabı bağlamını alın. Depolama hesaplarıyla çalışırken kimlik bilgilerini tekrar tekrar sağlamak yerine bağlama başvurursunuz. 

   3. [New-AzureStorageContainer](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontainer) komutunu kullanarak bir depolama kapsayıcısı oluşturun ve daha önce indirdiğiniz [sensör örnek verilerini](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/GettingStarted/HelloWorldASA-InputStream.json) karşıya yükleyin. 

   4. Kod tarafından çıkarılan depolama anahtarını kopyalayın ve daha sonra akış işinin girdi ve çıktılarını oluşturmak için JSON dosyalarına yapıştırın.

   ```powershell
   $storageAccountName = "mystorageaccount"
   $storageAccount = New-AzureRmStorageAccount `
     -ResourceGroupName $resourceGroup `
     -Name $storageAccountName `
     -Location $location `
     -SkuName Standard_LRS `
     -Kind Storage
   
   $ctx = $storageAccount.Context
   $containerName = "streamanalytics"
   
   New-AzureStorageContainer `
     -Name $containerName `
     -Context $ctx
   
   Set-AzureStorageBlobContent `
     -File "c:\HelloWorldASA-InputStream.json" `
     -Blob "input/HelloWorldASA-InputStream.json" `
     -Container $containerName `
     -Context $ctx  
   
   $storageAccountKey = (Get-AzureRmStorageAccountKey `
     -ResourceGroupName $resourceGroup `
     -Name $storageAccountName).Value[0]
   
   Write-Host "The <storage account key> placeholder needs to be replaced in your input and output json files with this key value:" 
   Write-Host $storageAccountKey -ForegroundColor Cyan
   ```

## <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

[New-AzureRmStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsjob?view=azurermps-5.4.0) cmdlet’i ile bir Stream Analytics işi oluşturun. Bu cmdlet iş adı, kaynak grubu adı ve iş tanımını parametre olarak alır. İş adı, işinizi tanımlayan herhangi bir kolay ad olabilir. Yalnızca alfasayısal karakter, kısa çizgi ve alt çizgi içerebilir ve 3 ila 63 karakter uzunluğunda olmalıdır. İş tanımı bir iş oluşturmak için gereken özellikleri içeren bir JSON dosyasıdır. Yerel makinenizde `JobDefinition.json` adlı bir dosya oluşturun ve içine aşağıdaki JSON verilerini ekleyin:

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

Ardından, `New-AzureRmStreamAnalyticsJob` cmdlet'ini çalıştırın. `jobDefinitionFile` değişkeninin değerini, iş tanımı JSON dosyasını depoladığınız yol ile değiştirdiğinizden emin olun. 

```powershell
$jobName = "MyStreamingJob"
$jobDefinitionFile = "C:\JobDefinition.json"
New-AzureRmStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -File $jobDefinitionFile `
  -Name $jobName `
  -Force 
```

## <a name="configure-input-to-the-job"></a>İş girdisini yapılandırma

[New-AzureRmStreamAnalyticsInput](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsinput?view=azurermps-5.4.0) cmdlet’ini kullanarak işinize bir girdi ekleyin. Bu cmdlet iş adı, iş girdisi adı, kaynak grubu adı ve iş girdisi tanımını parametre olarak alır. İş girdisi tanımı işin girdisini yapılandırmak için gereken özellikleri içeren bir JSON dosyasıdır. Bu örnekte, girdi olarak bir blob depolama oluşturacaksınız. 

Yerel makinenizde `JobInputDefinition.json` adlı bir dosya oluşturun ve içine aşağıdaki JSON verilerini ekleyin. `accountKey` değerini, depolama hesabınızın $storageAccountKey değerinde depolanmış değeri olan erişim anahtarıyla değiştirdiğinizden emin olun. 

```json
{
    "properties": {
        "type": "Stream",
        "datasource": {
            "type": "Microsoft.Storage/Blob",
            "properties": {
                "storageAccounts": [
                {
                   "accountName": "mystorageaccount",
                   "accountKey":"<storage account key>"
                }],
                "container": "streamanalytics",
                "pathPattern": "input/",
                "dateFormat": "yyyy/MM/dd",
                "timeFormat": "HH"
            }
        },
        "serialization": {
            "type": "Json",
            "properties": {
                "encoding": "UTF8"
            }
        }
    },
    "name": "MyBlobInput",
    "type": "Microsoft.StreamAnalytics/streamingjobs/inputs"
}
```

Ardından, `New-AzureRmStreamAnalyticsInput` cmdlet’ini çalıştırın ve `jobDefinitionFile` değişkeninin değerini, iş girişi tanımı JSON dosyasını depoladığınız yol ile değiştirdiğinizden emin olun. 

```powershell
$jobInputName = "MyBlobInput"
$jobInputDefinitionFile = "C:\JobInputDefinition.json"
New-AzureRmStreamAnalyticsInput `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobInputDefinitionFile `
  -Name $jobInputName 
```

## <a name="configure-output-to-the-job"></a>İş çıktısını yapılandırma

[New-AzureRmStreamAnalyticsOutput](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsoutput?view=azurermps-5.4.0) cmdlet’ini kullanarak işinize bir çıktı ekleyin. Bu cmdlet iş adı, iş çıktısı adı, kaynak grubu adı ve iş çıktısı tanımını parametre olarak alır. İş çıktısı tanımı işin çıktısını yapılandırmak için gereken özellikleri içeren bir JSON dosyasıdır. Bu örnek, çıktı olarak blob depolama kullanır. 

Yerel makinenizde `JobOutputDefinition.json` adlı bir dosya oluşturun ve içine aşağıdaki JSON verilerini ekleyin. `accountKey` değerini, depolama hesabınızın $storageAccountKey değerinde depolanmış değeri olan erişim anahtarıyla değiştirdiğinizden emin olun. 

```json
{
    "properties": {
        "datasource": {
            "type": "Microsoft.Storage/Blob",
            "properties": {
                "storageAccounts": [
                    {
                      "accountName": "mystorageaccount",
                      "accountKey": "<storage account key>"
                    }],
                "container": "streamanalytics",
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
    "name": "MyBlobOutput",
    "type": "Microsoft.StreamAnalytics/streamingjobs/outputs"
}
```

Ardından, `New-AzureRmStreamAnalyticsOutput` cmdlet'ini çalıştırın. `jobOutputDefinitionFile` değişkeninin değerini, iş çıktısı tanımı JSON dosyasını depoladığınız yol ile değiştirdiğinizden emin olun. 

```powershell
$jobOutputName = "MyBlobOutput"
$jobOutputDefinitionFile = "C:\JobOutputDefinition.json"
New-AzureRmStreamAnalyticsOutput `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobOutputDefinitionFile `
  -Name $jobOutputName -Force 
```

## <a name="define-the-transformation-query"></a>Dönüşüm sorgusunu tanımlama

[New-AzureRmStreamAnalyticsTransformation](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticstransformation?view=azurermps-5.4.0) cmdlet’ini kullanarak işinize bir dönüşüm ekleyin. Bu cmdlet iş adı, iş dönüşümü adı, kaynak grubu adı ve iş dönüşümü tanımını parametre olarak alır. Yerel makinenizde `JobTransformationDefinition.json` adlı bir dosya oluşturun ve içine aşağıdaki JSON verilerini ekleyin. JSON dosyası, dönüşüm sorgusunu tanımlayan bir sorgu parametresi içerir:

```json
{     
   "name":"MyTransformation",  
   "type":"Microsoft.StreamAnalytics/streamingjobs/transformations",  
   "properties":{    
      "streamingUnits":1,  
      "script":null,  
      "query":" SELECT System.Timestamp AS OutputTime, dspl AS SensorName, Avg(temp) AS AvgTemperature INTO MyBlobOutput FROM MyBlobInput TIMESTAMP BY time GROUP BY TumblingWindow(second,30),dspl HAVING Avg(temp)>100"  
   }  
}
```

Ardından, `New-AzureRmStreamAnalyticsTransformation` cmdlet'ini çalıştırın. `jobTransformationDefinitionFile` değişkeninin değerini, iş dönüşümü tanımı JSON dosyasını depoladığınız yol ile değiştirdiğinizden emin olun. 

```powershell
$jobTransformationName = "MyJobTransformation"
$jobTransformationDefinitionFile = "C:\JobTransformationDefinition.json"
New-AzureRmStreamAnalyticsTransformation `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobTransformationDefinitionFile `
  -Name $jobTransformationName -Force
```

## <a name="start-the-stream-analytics-job-and-check-the-output"></a>Stream Analytics işini başlatıp çıktıyı denetleyin

[Start-AzureRmStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/start-azurermstreamanalyticsjob?view=azurermps-5.4.0) cmdlet’ini kullanarak işi başlatın. Bu cmdlet iş adı, kaynak grubu adı, çıktı başlangıç modu ve başlangıç saatini parametre olarak alır. `OutputStartMode`; `JobStartTime`, `CustomTime` veya `LastOutputEventTime` değerlerini kabul eder. Bu değerlerin her birinin ne anlama geldiği hakkında daha fazla bilgi için PowerShell belgelerindeki [parametreler](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/start-azurermstreamanalyticsjob?view=azurermps-5.4.0) bölümüne bakın. Bu örnekte, modu `CustomTime` olarak belirtin ve `OutputStartTime` için bir değer sağlayın. 

Saat için `2018-01-01` değerini seçin. Bu başlangıç tarihi, örnek verilerdeki olay zaman damgasından önce olduğu için seçilir. Aşağıdaki cmdlet’i çalıştırdıktan sonra iş başlarsa çıktı olarak `True` değeri döndürülür. Depolama kapsayıcısında, dönüştürülmüş verilerle birlikte bir çıktı klasörü oluşturulur. 

```powershell
Start-AzureRmStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -Name $jobName `
  -OutputStartMode CustomTime `
  -OutputStartTime 2018-01-01T00:00:00Z 
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, akış işini ve tüm ilgili kaynakları silin. İşin silinmesi, iş tarafından kullanılan akış birimlerinin faturalanmasını önler. İşi gelecekte kullanmayı planlıyorsanız, silme işlemini atlayıp işi şimdilik durdurabilirsiniz. Bu işi kullanmaya devam etmeyecekseniz aşağıdaki cmdlet’i çalıştırarak bu hızlı başlangıçla oluşturulan tüm kaynakları silin:

```powershell
Remove-AzureRmResourceGroup `
  -Name $resourceGroup 
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta basit bir Stream Analytics işi dağıttınız. Diğer girdi kaynaklarını yapılandırma ve gerçek zamanlı algılama hakkında bilgi almak için aşağıdaki makaleye geçin:

> [!div class="nextstepaction"]
> [Azure Stream Analytics kullanarak gerçek zamanlı sahtekarlık algılama](stream-analytics-real-time-fraud-detection.md)
