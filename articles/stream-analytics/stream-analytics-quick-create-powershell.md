---
title: Azure PowerShell kullanarak Stream Analytics işi oluşturma | Microsoft Docs
description: Bu hızlı başlangıçta Azure PowerShell modülü kullanarak bir Azure Stream Analytics işi dağıtma ve çalıştırma işleminin ayrıntıları verilmektedir.
services: stream-analytics
keywords: Stream analytics, Bulut işleri, Azure PowerShell, iş girdisi, iş çıktısı, iş dönüşümü
author: SnehaGunda
ms.author: sngun
ms.date: 03/16/2018
ms.topic: quickstart
ms.service: stream-analytics
ms.custom: mvc
manager: kfile
ms.openlocfilehash: 8a1036531ea0e7c1426224bc4d42c83e9049cabf
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="quickstart-create-a-stream-analytics-job-by-using-azure-powershell"></a>Hızlı başlangıç: Azure PowerShell kullanarak Stream Analytics işi oluşturma

Azure PowerShell modülü, PowerShell cmdlet’leri veya betikleri kullanarak Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta Azure PowerShell modülü kullanarak bir Azure Stream Analytics işi dağıtma ve çalıştırma işleminin ayrıntıları verilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.  

* Bu hızlı başlangıç, Azure PowerShell modülü 3.6 veya sonraki bir sürümü gerektirir. Yerel makinenizde yüklü sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse [Azure PowerShell Modülü yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) makalesine bakın. Bu makaledeki senaryoda, blob depolama alanından verileri okuma, verileri dönüştürme ve aynı blob depolama alanındaki farklı bir kapsayıcıya geri yazma işlemi açıklanmaktadır.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

`Connect-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve açılır tarayıcı penceresine Azure kimlik bilgilerinizi girin. Oturum açtıktan sonra, birden çok aboneliğiniz varsa aşağıdaki cmdlet’leri çalıştırarak bu hızlı başlangıç için kullanmak istediğiniz aboneliği seçin. <your subscription> değerini aboneliğinizin adıyla değiştirdiğinizden emin olun:  

```powershell
# Log in to your Azure account
Connect-AzureRmAccount

# Select the Azure subscription you want to use to create the resource group.
Get-AzureRmSubscription `
  -SubscriptionName “<your subscription>” | Select-AzureRmSubscription
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup) ile yeni bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```powershell
$resourceGroup = "ASAPSRG"
$location = "WestUS2"
New-AzureRMResourceGroup `
  -Name $resourceGroup `
  -Location $location 
```

## <a name="prepare-the-input-data"></a>Girdi verilerini hazırlama

Stream Analytics işini tanımlamadan önce işe girdi olarak yapılandırılan verileri hazırlamanız gerekir. İş için gereken girdi verilerini hazırlamak için aşağıdaki adımları uygulayın: 

1. GitHub’dan [sensör örnek verilerini](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/GettingStarted/HelloWorldASA-InputStream.json) indirin.  

2. [New-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/New-AzureRmStorageAccount) cmdlet’ini kullanarak LRS ile standart bir genel amaçlı depolama hesabı oluşturma  

3. Kullanılacak depolama hesabını tanımlayan depolama hesabı bağlamını alın. Depolama hesaplarıyla çalışırken kimlik bilgilerini tekrar tekrar sağlamak yerine bağlama başvurursunuz. Bu örnek, yerel olarak yedekli depolama(LRS) ve blob şifrelemesi (varsayılan olarak etkindir) ile mystorageaccount adlı bir depolama hesabı oluşturur.  

4. Ardından, [New-AzureStorageContainer](https://docs.microsoft.com/powershell/module/azure.storage/new-azurestoragecontainer) cmdlet’ini kullanarak 'blob' izinlerini dosyaların genel erişimine izin verecek şekilde ayarlayın ve daha önce indirdiğiniz [sensör örnek verilerini](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/GettingStarted/HelloWorldASA-InputStream.json) karşıya yükleyin. 

Bu adımlar aşağıdaki PowerShell betiği çalıştırılarak elde edilir:

```powershell
$storageAccountName = "mystorageaccount"
$storageAccount = New-AzureRmStorageAccount `
  -ResourceGroupName $resourceGroup `
  -Name $storageAccountName `
  -Location $location `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context
$containerName = "myinputcontainer"

New-AzureStorageContainer `
  -Name $containerName `
  -Context $ctx `
  -Permission blob

Set-AzureStorageBlobContent `
  -File "C:\HelloWorldASA-InputStream.json" `
  -Container $containerName `
  -Context $ctx  

$storageAccountKey = (Get-AzureRmStorageAccountKey `
  -ResourceGroupName $resourceGroup `
  -Name $storageAccountName).Value[0]
```

## <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

[New-AzureRMStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsjob?view=azurermps-5.4.0) cmdlet’i ile bir Stream Analytics işi oluşturun. Bu cmdlet iş adı, kaynak grubu adı ve iş tanımını parametre olarak alır. İş adı işinizi tanımlayan herhangi bir kolay ad olabilir. Stream Analytics işinin adı yalnızca alfasayısal karakter, kısa çizgi ve alt çizgi içerebilir ve 3 ila 63 karakter uzunluğunda olmalıdır. İş tanımı bir iş oluşturmak için gereken özellikleri içeren bir JSON dosyasıdır. Yerel makinenizde "JobDefinition.json" adlı bir dosya oluşturun ve içine aşağıdaki JSON verilerini ekleyin:

```json
{    
   "location":"Central US",  
   "properties":{    
      "sku":{    
         "name":"standard"  
      },  
      "eventsOutOfOrderPolicy":"drop",  
      "eventsOutOfOrderMaxDelayInSeconds":10,  
      "compatibilityLevel": 1.1
}
}
```

Sonra New-AzureRMStreamAnalyticsJob cmdlet’ini çalıştırın, jobDefinitionFile değişkeninin değerini iş tanımı JSON dosyanızı depoladığınız yolla değiştirdiğinizden emin olun. 

```powershell
$jobName = "MyStreamingJob"
$jobDefinitionFile = "C:\JobDefinition.json"
New-AzureRMStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  –File $jobDefinitionFile `
  –Name $jobName -Force 
```

## <a name="configure-input-to-the-job"></a>İş girdisini yapılandırma

[New-AzureRMStreamAnalyticsInput](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsinput?view=azurermps-5.4.0) cmdlet’ini kullanarak işinize bir girdi ekleyin. Bu cmdlet iş adı, iş girdisi adı, kaynak grubu adı ve iş girdisi tanımını parametre olarak alır. İş girdisi tanımı, işin girdisini yapılandırmak için gereken özellikleri içeren bir JSON dosyasıdır; bu örnekte girdi olarak bir blob depolama alanı oluşturacaksınız. Yerel makinenizde “JobInputDefinition.json” adlı bir dosya oluşturun ve içine aşağıdaki JSON verilerini ekleyin; **accountKey** değerini $storageAccountKey değerinde depolanmış değer olan depolama hesabınızın erişim anahtarı ile değiştirdiğinizden emin olun. 

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
                   "accountKey":"<Your storage account key>"
                }],
                "container": "myinputcontainer",
                "pathPattern": "",
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

Ardından New-AzureRMStreamAnalyticsInput cmdlet’ini çalıştırın; jobDefinitionFile değişkeninin değerini, iş girdisi tanımı JSON dosyasını depoladığınız yolla değiştirdiğinizden emin olun. 

```powershell
$jobInputName = "MyBlobInput"
$jobInputDefinitionFile = "C:\JobInputDefinition.json"
New-AzureRMStreamAnalyticsInput `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobInputDefinitionFile `
  -Name $jobInputName 
```

## <a name="configure-output-to-the-job"></a>İş çıktısını yapılandırma

[New-AzureRmStreamAnalyticsOutput](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticsoutput?view=azurermps-5.4.0) cmdlet’ini kullanarak işinize bir çıktı ekleyin. Bu cmdlet iş adı, iş çıktısı adı, kaynak grubu adı ve iş çıktısı tanımını parametre olarak alır. İş çıktısı tanımı, işin çıktısını yapılandırmak için gereken özellikleri içeren bir JSON dosyasıdır; bu örnekte çıktı olarak bir blob depolama alanı oluşturacaksınız. Yerel makinenizde “JobOutputDefinition.json” adlı bir dosya oluşturun ve içine aşağıdaki JSON verilerini ekleyin; **accountKey** değerini $storageAccountKey değerinde depolanmış değer olan depolama hesabınızın erişim anahtarı ile değiştirdiğinizden emin olun. 

```json
{
    "properties": {
        "datasource": {
            "type": "Microsoft.Storage/Blob",
            "properties": {
                "storageAccounts": [
                    {
                      "accountName": "mystorageaccount",
                      "accountKey": "<Your storage account key>"
                    }],
                "container": "myoutputcontainer",
                "pathPattern": "",
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

Ardından New-AzureRMStreamAnalyticsOutput cmdlet’ini çalıştırın; jobOutputDefinitionFile değişkeninin değerini, iş çıktısı tanımı JSON dosyasını depoladığınız yolla değiştirdiğinizden emin olun. 

```powershell
$jobOutputName = "MyBlobOutput"
$jobOutputDefinitionFile = "C:\JobOutputDefinition.json"
New-AzureRMStreamAnalyticsOutput `
  -ResourceGroupName $resourceGroup `
  –JobName $jobName `
  –File $jobOutputDefinitionFile `
  –Name $jobOutputName -Force 
```

## <a name="define-the-transformation-query"></a>Dönüşüm sorgusunu tanımlama

[New-AzureRmStreamAnalyticsTransformation](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/new-azurermstreamanalyticstransformation?view=azurermps-5.4.0) cmdlet’ini kullanarak işinize bir dönüşüm ekleyin. Bu cmdlet iş adı, iş dönüşümü adı, kaynak grubu adı ve iş dönüşümü tanımını parametre olarak alır. Yerel makinenizde "JobTransformationDefinition.json" adlı bir dosya oluşturun ve içine aşağıdaki JSON verilerini ekleyin. JSON dosyası, dönüşüm sorgusunu tanımlayan bir sorgu parametresi içerir:

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

Ardından New-AzureRMStreamAnalyticsTransformation cmdlet’ini çalıştırın; jobTransformationDefinitionFile değişkeninin değerini, iş dönüşüm tanımı JSON dosyasını depoladığınız yolla değiştirdiğinizden emin olun. 

```powershell
$jobTransformationName = "MyJobTransformation"
$jobTransformationDefinitionFile = "C:\JobTransformationDefinition.json"
New-AzureRMStreamAnalyticsTransformation `
  -ResourceGroupName $resourceGroup `
  –JobName $jobName `
  –File $jobTransformationDefinitionFile `
  –Name $jobTransformationName -Force
```

## <a name="start-the-stream-analytics-job-and-check-the-output"></a>Stream Analytics işini başlatıp çıktıyı denetleyin

[Start-AzureRMStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/start-azurermstreamanalyticsjob?view=azurermps-5.4.0) cmdlet’ini kullanarak işi başlatın. Bu cmdlet iş adı, kaynak grubu adı, çıktı başlangıç modu ve başlangıç saatini parametre olarak alır. OUtpputStartMode, JobStartTime, CustomTime veya LastOutputEventTime şeklinde iki değer kabul eder. Bu değerlerin her birinin ne anlama geldiği hakkında bilgi almak için PowerShell belgelerindeki [parametreler](https://docs.microsoft.com/powershell/module/azurerm.streamanalytics/start-azurermstreamanalyticsjob?view=azurermps-5.4.0) bölümüne bakın. Bu örnekte modu CustomTime olarak belirtebilir ve OutputStartTime için bir değer sağlayabilirsiniz. 

Saat değeri için, dosyanın karşıya yüklendiği saat geçerli saatten önce olduğu için dosyayı blob depolama alanına yüklediğiniz tarihten bir gün öncesini seçin. Aşağıdaki cmdlet’i çalıştırdıktan sonra iş başlarsa çıktı olarak “True” değeri döndürülür. Dönüştürülen verilerle birlikte depolama hesabında "Myoutputcontainer" adlı bir kapsayıcı oluşturulur. 

```powershell
Start-AzureRMStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -Name $jobName `
  -OutputStartMode CustomTime `
  -OutputStartTime 2018-03-11T14:45:12Z 
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, akış işini ve tüm ilgili kaynakları silin. İşin silinmesi, iş tarafından kullanılan akış birimlerinin faturalanmasını önler. İşi gelecekte kullanmayı planlıyorsanız, durdurup daha sonra gerektiğinde yeniden başlatabilirsiniz. Bu işi kullanmaya devam etmeyecekseniz aşağıdaki cmdlet’i çalıştırarak bu hızlı başlangıçla oluşturulan tüm kaynakları silin:

```powershell
Remove-AzureRmResourceGroup `
  -Name $resourceGroup 
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta basit bir Stream Analytics işi dağıttınız. Diğer girdi kaynaklarını yapılandırma ve gerçek zamanlı algılama hakkında bilgi almak için aşağıdaki makaleye geçin:

> [!div class="nextstepaction"]
> [Azure Stream Analytics kullanarak gerçek zamanlı sahtekarlık algılama](stream-analytics-real-time-fraud-detection.md)
