---
title: İzleme ve PowerShell kullanarak Azure Stream Analytics işlerini yönetme
description: Bu makalede, Azure Stream Analytics işlerini yönetmek ve izlemek için Azure PowerShell ve cmdlet'leri kullanmayı açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 27ee1980fd60a2e301830f198a5f65c4d89df59f
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59046538"
---
# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>İzleme ve Stream Analytics işlerini Azure PowerShell cmdlet'leriyle yönetme
İzleme ve Azure PowerShell cmdlet'leri ve powershell betikleri ile temel bir Stream Analytics görevleri yürütme Stream Analytics kaynaklarını yönetme hakkında bilgi edinin.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Stream Analytics için Azure PowerShell cmdlet'lerini çalıştırmak için Önkoşullar
* Aboneliğinizde bir Azure kaynak grubu oluşturun. Örnek Azure PowerShell Betiği verilmiştir. Azure PowerShell için bilgi [yüklemek ve Azure PowerShell yapılandırma](/powershell/azure/overview);  

Azure PowerShell 0.9.8:  

```powershell
# Log in to your Azure account
Add-AzureAccount
# Select the Azure subscription you want to use to create the resource group if you have more han one subscription on your account.
Select-AzureSubscription -SubscriptionName <subscription name>
# If Stream Analytics has not been registered to the subscription, remove remark symbol below (#)to run the Register-AzureProvider cmdlet to register the provider namespace.
#Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
# Create an Azure resource group
New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
```

Azure PowerShell 1.0:

```powershell
# Log in to your Azure account
Connect-AzAccount
# Select the Azure subscription you want to use to create the resource group.
Get-AzSubscription –SubscriptionName "your sub" | Select-AzSubscription
# If Stream Analytics has not been registered to the subscription, remove remark symbol below (#)to run the Register-AzureProvider cmdlet to register the provider namespace.
#Register-AzResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'
# Create an Azure resource group
New-AzResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>
```


> [!NOTE]
> Stream Analytics işleri programlı olarak oluşturulmuş varsayılan olarak etkin izleme yok.  Azure Portalı'nda izleme iş izleme sayfasına giderek ve etkinleştir düğmesine tıklayarak el ile etkinleştirebilirsiniz veya bunu programlı olarak adresinde yer alan adımları izleyerek yapabilirsiniz [Azure Stream Analytics - Stream Analytics işleri İzle Program aracılığıyla](stream-analytics-monitor-jobs.md).
> 
> 

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Stream Analytics için Azure PowerShell cmdlet'leri
Aşağıdaki Azure PowerShell cmdlet'lerini, Azure Stream Analytics işlerini yönetmek ve izlemek için kullanılabilir. Azure PowerShell farklı olduğuna dikkat edin. 
**Listelenen örnekler için Azure PowerShell 0.9.8, ilk komut olduğundan ikinci komut, Azure PowerShell 1.0 için olur.** Azure PowerShell 1.0 komutlarını komut "Az" her zaman sahip olur.

### <a name="get-azurestreamanalyticsjob--get-azstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob | Get-AzStreamAnalyticsJob
Azure aboneliği veya belirtilen kaynak grubunda tanımlanan tüm Stream Analytics işlerini listeler ve bir kaynak grubu içindeki belirli bir işin iş bilgilerini alır.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsJob
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsJob
```

Bu PowerShell komutunu Azure aboneliğinde tüm Stream Analytics işleri hakkında bilgi döndürür.

**Örnek 2**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 
```

Bu PowerShell komutunu kaynak grubunda varsayılan orta ABD StreamAnalytics tüm Stream Analytics işleri hakkında bilgi döndürür.

**Örnek 3**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob
```

Bu PowerShell komutunu kaynak grubunda varsayılan orta ABD StreamAnalytics StreamingJob Stream Analytics işi hakkında bilgi döndürür.

### <a name="get-azurestreamanalyticsinput--get-azstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput | Get-AzStreamAnalyticsInput
Belirtilen bir Stream Analytics işinde tanımlanan girişleri listeler veya belirli bir giriş bilgilerini alır.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob
```

Bu PowerShell komutunu StreamingJob iş içinde tanımlanan tüm girişleri hakkında bilgi döndürür.

**Örnek 2**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream
```

Bu PowerShell komutunu StreamingJob iş içinde tanımlanan EntryStream adlı giriş hakkında bilgi döndürür.

### <a name="get-azurestreamanalyticsoutput--get-azstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput | Get-AzStreamAnalyticsOutput
Belirtilen bir Stream Analytics işinde tanımlanan çıkışları listeler veya belirli bir çıktısına hakkındaki bilgileri alır.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob
```

Bu PowerShell komutunu StreamingJob iş içinde tanımlanan çıkış hakkında bilgi döndürür.

**Örnek 2**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output
```

Bu PowerShell komutunu StreamingJob iş içinde tanımlanan çıkış adlı çıktı hakkında bilgi döndürür.

### <a name="get-azurestreamanalyticsquota--get-azstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota | Get-AzStreamAnalyticsQuota
Akış birimleri belirli bir bölgede, kota hakkındaki bilgileri alır.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsQuota –Location "Central US" 
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsQuota –Location "Central US" 
```

Bu PowerShell komutunu Orta ABD bölgesinde kota ve akış birimi kullanımı hakkında bilgi döndürür.

### <a name="get-azurestreamanalyticstransformation--get-azstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation | Get-AzStreamAnalyticsTransformation
Stream Analytics işinde tanımlanan belirli bir dönüştürme hakkında bilgi alır.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob
```

Azure PowerShell 1.0:  

```powershell
Get-AzStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob
```

Bu PowerShell komutunu StreamingJob adlı StreamingJob işinde dönüştürme hakkında bilgi döndürür.

### <a name="new-azurestreamanalyticsinput--new-azstreamanalyticsinput"></a>Yeni AzureStreamAnalyticsInput | Yeni AzStreamAnalyticsInput
Bir Stream Analytics işi içinde yeni bir giriş oluşturur veya mevcut bir belirtilen giriş güncelleştirir.

Giriş adını .json dosyası veya komut satırında belirtilebilir. Her ikisi de belirtilirse, komut satırında adı bir dosya ile aynı olmalıdır.

Zaten bir girdi belirtin ve belirtmeyin Force parametresini cmdlet var olan girdiyi Değiştir gerekip gerekmediğini sorar.

Belirtirseniz Force parametresini ve var olan bir giriş adı, giriş onaysız değiştirilecek belirtin.

JSON dosya yapısı ve içeriği hakkında ayrıntılı bilgi için başvurmak [giriş oluşturma (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-input] bölümünü [Stream Analytics Yönetimi REST API Başvurusu Kitaplık][stream.analytics.rest.api.reference].

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 
```

Bu PowerShell komutunu, söz konusu Input.json dosyasından yeni bir giriş oluşturur. Giriş tanımı dosyasında belirtilen ada sahip mevcut bir giriş zaten tanımlıysa, cmdlet değiştirmek gerekip gerekmediğini sorar.

**Örnek 2**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream
```

Bu PowerShell komutunu yeni bir giriş EntryStream adlı işi oluşturur. Bu ada sahip mevcut bir giriş zaten tanımlıysa, cmdlet değiştirmek gerekip gerekmediğini sorar.

**Örnek 3**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force
```

Bu PowerShell komut dosyasındaki tanımıyla EntryStream adlı mevcut giriş kaynağı tanımını değiştirir.

### <a name="new-azurestreamanalyticsjob--new-azstreamanalyticsjob"></a>Yeni AzureStreamAnalyticsJob | Yeni AzStreamAnalyticsJob
Microsoft Azure'da yeni bir Stream Analytics işi oluşturur veya mevcut bir belirtilen iş tanımını güncelleştirir.

İş adı bir .json dosyası veya komut satırında belirtilebilir. Her ikisi de belirtilirse, komut satırında adı bir dosya ile aynı olmalıdır.

Zaten bir iş adı belirtin ve belirtmeyin Force parametresini cmdlet var olan bir işi değiştirin gerekip gerekmediğini sorar.

Belirtirseniz Force parametresini ve varolan bir proje adı belirtirseniz iş tanımı onaysız değiştirilecektir.

JSON dosya yapısı ve içeriği hakkında ayrıntılı bilgi için başvurmak [Stream Analytics işi oluşturma] [ msdn-rest-api-create-stream-analytics-job] bölümünü [Stream Analytics Yönetimi REST API Başvurusu Kitaplığı] [stream.analytics.rest.api.reference].

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 
```

Bu PowerShell komutunu JobDefinition.json tanımında yeni bir proje oluşturur. İş tanımı dosyasında belirtilen ada sahip mevcut bir proje zaten tanımlıysa, cmdlet değiştirmek gerekip gerekmediğini sorar.

**Örnek 2**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force
```

Bu PowerShell komutunu StreamingJob iş tanımını değiştirir.

### <a name="new-azurestreamanalyticsoutput--new-azstreamanalyticsoutput"></a>New-AzureStreamAnalyticsOutput | New-AzStreamAnalyticsOutput
Bir Stream Analytics işi içinde yeni bir çıktı oluşturur veya mevcut bir çıktı güncelleştirir.  

Çıktı adı bir .json dosyası veya komut satırında belirtilebilir. Her ikisi de belirtilirse, komut satırında adı bir dosya ile aynı olmalıdır.

Zaten bir çıktı belirtin ve belirtmeyin Force parametresini cmdlet'i mevcut çıkış değiştirin gerekip gerekmediğini sorar.

Belirtirseniz Force parametresini ve var olan bir çıktı adı, çıktı onaysız değiştirilecek belirtin.

JSON dosya yapısı ve içeriği hakkında ayrıntılı bilgi için başvurmak [çıktı oluşturma (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-output] bölümünü [Stream Analytics Yönetimi REST API Başvurusu Kitaplık][stream.analytics.rest.api.reference].

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output
```

Bu PowerShell komutunu StreamingJob işinde "çıkış" adlı yeni bir çıktı oluşturur. Bu ada sahip mevcut bir çıkış tanımlanmış olması durumunda, cmdlet değiştirmek gerekip gerekmediğini sorar.

**Örnek 2**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force
```

Bu PowerShell komutunu StreamingJob işinde "çıkış" tanımını değiştirir.

### <a name="new-azurestreamanalyticstransformation--new-azstreamanalyticstransformation"></a>Yeni AzureStreamAnalyticsTransformation | Yeni AzStreamAnalyticsTransformation
Bir Stream Analytics işi içinde yeni bir dönüşüm oluşturur veya mevcut dönüştürmeyi güncelleştirir.

Dönüşümü adı, .json dosyası veya komut satırında belirtilebilir. Her ikisi de belirtilirse, komut satırında adı bir dosya ile aynı olmalıdır.

Zaten bir dönüştürme belirtin ve belirtmeyin Force parametresini cmdlet'i mevcut dönüştürmeyi değiştirmek gerekip gerekmediğini sorar.

Belirtirseniz Force parametresini ve varolan bir dönüştürme adı belirtirseniz dönüşümü onaysız değiştirilecektir.

JSON dosya yapısı ve içeriği hakkında ayrıntılı bilgi için başvurmak [dönüştürme oluşturma (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-transformation] bölümünü [Stream Analytics Yönetimi REST API'si Referans kitaplığında][stream.analytics.rest.api.reference].

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform
```

Bu PowerShell komutunu StreamingJobTransform StreamingJob işinde adlı yeni bir dönüşüm oluşturur. Bu ada sahip mevcut bir dönüştürme zaten tanımlanmış, cmdlet değiştirmek gerekip gerekmediğini isteyin.

**Örnek 2**

Azure PowerShell 0.9.8:  

```powershell
New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force
```

Azure PowerShell 1.0:  

```powershell
New-AzStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force
```

 Bu PowerShell komutunu StreamingJob iş StreamingJobTransform tanımında değiştirir.

### <a name="remove-azurestreamanalyticsinput--remove-azstreamanalyticsinput"></a>Remove-AzureStreamAnalyticsInput | Remove-AzStreamAnalyticsInput
Zaman uyumsuz olarak bir Microsoft Azure Stream Analytics işinde belirli bir giriş siler.  
Belirtirseniz Force parametresi onay giriş silinecek.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream
```

Azure PowerShell 1.0:  

```powershell
Remove-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream
```

Bu PowerShell komutunu kullanarak işin içinde StreamingJob giriş EventStream kaldırır.  

### <a name="remove-azurestreamanalyticsjob--remove-azstreamanalyticsjob"></a>Remove-AzureStreamAnalyticsJob | Remove-AzStreamAnalyticsJob
Zaman uyumsuz olarak Microsoft azure'da özel bir Stream Analytics işi siler.  
Belirtirseniz Force parametresi onay iş silinir.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 
```

Azure PowerShell 1.0:  

```powershell
Remove-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 
```

Bu PowerShell komutunu StreamingJob işi kaldırır.  

### <a name="remove-azurestreamanalyticsoutput--remove-azstreamanalyticsoutput"></a>Remove-AzureStreamAnalyticsOutput | Remove-AzStreamAnalyticsOutput
Zaman uyumsuz olarak bir Microsoft Azure Stream Analytics işinde belirli bir çıktısına siler.  
Belirtirseniz Force parametresini çıkış onaysız silinecek.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output
```

Azure PowerShell 1.0:  

```powershell
Remove-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output
```

Bu PowerShell komut çıktı StreamingJob işinde çıkış kaldırır.  

### <a name="start-azurestreamanalyticsjob--start-azstreamanalyticsjob"></a>Başlangıç AzureStreamAnalyticsJob | Başlangıç AzStreamAnalyticsJob
Zaman uyumsuz olarak dağıtır ve Microsoft Azure'da bir Stream Analytics işi başlatır.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z
```

Azure PowerShell 1.0:  

```powershell
Start-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z
```

Bu PowerShell komutunu StreamingJob özel çıkış başlangıç saati ile 12:12:12 12 Aralık 2012'ye ayarlayın bir iş başladığı UTC.

### <a name="stop-azurestreamanalyticsjob--stop-azstreamanalyticsjob"></a>Stop-AzureStreamAnalyticsJob | Stop-AzStreamAnalyticsJob
Zaman uyumsuz olarak Microsoft Azure'da çalışan bir Stream Analytics işini durdurur ve kullanılmakta olan kaynakları serbest bırakır. İş düzenleme ve yeniden başlatılacak şekilde iş tanımı ve meta verileri Azure portalı ve yönetim API'leri aracılığıyla, aboneliğiniz kapsamındaki kullanılabilir kalır. Durduruldu durumundaki bir iş için ücretlendirilmez.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 
```

Azure PowerShell 1.0:  

```powershell
Stop-AzStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 
```

Bu PowerShell komutunu StreamingJob işini durdurur.  

### <a name="test-azurestreamanalyticsinput--test-azstreamanalyticsinput"></a>Test-AzureStreamAnalyticsInput | Test-AzStreamAnalyticsInput
Stream Analytics için belirtilen bir giriş bağlanma olanağı sınar.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream
```

Azure PowerShell 1.0:  

```powershell
Test-AzStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream
```

Bu PowerShell komutunu StreamingJob içinde giriş EntryStream bağlantı durumunu sınar.  

### <a name="test-azurestreamanalyticsoutput--test-azstreamanalyticsoutput"></a>Test-AzureStreamAnalyticsOutput | Test-AzStreamAnalyticsOutput
Stream Analytics belirli bir çıktıya bağlanma olanağı sınar.

**Örnek 1**

Azure PowerShell 0.9.8:  

```powershell
Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output
```

Azure PowerShell 1.0:  

```powershell
Test-AzStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output
```

Bu PowerShell komutunu StreamingJob çıkış bağlantı durumunu çıktı sınar.  

## <a name="get-support"></a>Destek alın
Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics). 

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[msdn-switch-azuremode]: https://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301

