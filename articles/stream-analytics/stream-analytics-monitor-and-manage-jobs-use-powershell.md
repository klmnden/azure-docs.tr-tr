---
title: İzleme ve PowerShell kullanarak Azure akış analizi işleri yönetme
description: Bu makalede, Azure akış analizi işleri yönetmek ve izlemek için Azure PowerShell ve cmdlet'leri kullanmayı açıklar.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/28/2017
ms.openlocfilehash: 61c26c55725c19f526680d70f3621d41e9590965
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>İzleme ve akış analizi işleri Azure PowerShell cmdlet'leri ile yönetme
İzleme ve Azure PowerShell cmdlet'leri ve powershell komut dosyası ile temel akış analizi görevleri yürütün akış analizi kaynaklarını yönetme hakkında bilgi edinin.

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Akış analizi için Azure PowerShell cmdlet'lerini çalıştırmak için Önkoşullar
* Aboneliğinizde Azure kaynak grubu oluşturun. Örnek bir Azure PowerShell komut dosyası verilmiştir. Azure PowerShell bilgi için bkz: [yükleyin ve Azure PowerShell yapılandırma](/powershell/azure/overview);  

Azure PowerShell 0.9.8:  

         # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

Azure PowerShell 1.0:  

         # Log in to your Azure account
        Connect-AzureRmAccount

        # Select the Azure subscription you want to use to create the resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered to the subscription, remove remark symbol below (#) to run the Register-AzureProvider cmdlet to register the provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>



> [!NOTE]
> Program aracılığıyla oluşturulan stream Analytics işlerini izleme varsayılan olarak etkin gerekmez.  Azure Portalı'nda izleme iş İzleyici sayfasına giderek ve etkinleştir düğmesine tıklayarak el ile etkinleştirebilirsiniz veya bu program aracılığıyla konumunda bulunan adımları izleyerek yapabilirsiniz [Azure akış analizi - Stream Analytics işleri izleme Programlı şekilde](stream-analytics-monitor-jobs.md).
> 
> 

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>Akış analizi için Azure PowerShell cmdlet'leri
Azure Stream Analytics işlerini yönetmek ve izlemek için aşağıdaki Azure PowerShell cmdlet'leri kullanılabilir. Azure PowerShell farklı sürümlerini olduğuna dikkat edin. 
**Azure PowerShell 0.9.8, ilk komut listelenen örneklerde olduğu için Azure PowerShell 1.0 ikinci komuttur.** Azure PowerShell 1.0 komutları komutta her zaman "AzureRM" olacaktır.

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob | Get-AzureRMStreamAnalyticsJob
Azure aboneliği veya belirtilen kaynak grubunda tanımlanan tüm Stream Analytics işlerini listeler veya bir kaynak grubu içindeki belirli bir iş iş bilgilerini alır.

**Örnek 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob

Bu PowerShell komutunu Azure aboneliğindeki tüm Stream Analytics işleri hakkında bilgi verir.

**Örnek 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Bu PowerShell komutunu kaynak grubunda varsayılan orta ABD StreamAnalytics tüm Stream Analytics işleri hakkında bilgi verir.

**Örnek 3**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Bu PowerShell komutunu kaynak grubunda varsayılan orta ABD StreamAnalytics StreamingJob Stream Analytics işi hakkında bilgi verir.

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput | Get-AzureRMStreamAnalyticsInput
Tüm belirtilen Stream Analytics işinde tanımlı girdi listeler veya belirli bir giriş bilgilerini alır.

**Örnek 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Bu PowerShell komutunu iş StreamingJob tanımlanan tüm girişleri hakkında bilgi verir.

**Örnek 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Bu PowerShell komutunu iş StreamingJob tanımlanan EntryStream adlı giriş hakkında bilgi verir.

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput | Get-AzureRMStreamAnalyticsOutput
Tüm belirtilen Stream Analytics işinde tanımlanan çıkışları listeler veya belirli bir çıkış hakkındaki bilgileri alır.

**Örnek 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Bu PowerShell komutunu iş StreamingJob tanımlanan çıkışları hakkında bilgi verir.

**Örnek 2**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Bu PowerShell komutunu iş StreamingJob tanımlanan çıkış adlı çıktı hakkında bilgi verir.

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota | Get-AzureRMStreamAnalyticsQuota
Akış birimleri belirtilen bir bölgede, kota hakkındaki bilgileri alır.

**Örnek 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

Bu PowerShell komutunu Orta ABD bölgesinde kota ve akış birimleri kullanımı hakkında bilgi verir.

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation | GetAzureRMStreamAnalyticsTransformation
Stream Analytics işinde tanımlanan belirli bir dönüşüm hakkındaki bilgileri alır.

**Örnek 1**

Azure PowerShell 0.9.8:  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Bu PowerShell komutunu iş StreamingJob StreamingJob adlı dönüştürme hakkında bilgi döndürür.

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a>AzureStreamAnalyticsInput yeni | AzureRMStreamAnalyticsInput yeni
Akış analizi işi içinde yeni bir giriş oluşturur veya mevcut bir belirtilen giriş güncelleştirir.

Giriş adı .json dosyası veya komut satırında belirtilebilir. Her ikisi de belirtilirse, komut satırında adı bir dosya ile aynı olması gerekir.

Zaten bir girdi belirtin ve belirtmeyin Force parametresini cmdlet varolan giriş değiştirmek isteyip istemediğinizi sorar.

Belirtirseniz – Force parametresi ve var olan giriş adı, giriş onaysız değiştirilecek belirtin.

JSON dosya yapısı ve içeriği hakkında ayrıntılı bilgi için bkz [girişi oluşturun (Azure akış analizi)] [ msdn-rest-api-create-stream-analytics-input] bölümünü [Stream Analytics Yönetimi REST API Başvurusu Kitaplık][stream.analytics.rest.api.reference].

**Örnek 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Bu PowerShell komutunu Input.json dosyasından yeni bir giriş oluşturur. Giriş tanım dosyasında belirtilen ada sahip mevcut bir giriş zaten tanımlanmış ise, cmdlet değiştirmek isteyip istemediğinizi sorar.

**Örnek 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Bu PowerShell komutunu İşte EntryStream adlı yeni bir giriş oluşturur. Bu ada sahip mevcut bir giriş zaten tanımlanmış ise, cmdlet değiştirmek isteyip istemediğinizi sorar.

**Örnek 3**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Bu PowerShell komut dosyasından tanımıyla EntryStream adlı varolan giriş kaynağı tanımını değiştirir.

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a>AzureStreamAnalyticsJob yeni | AzureRMStreamAnalyticsJob yeni
Microsoft Azure'da yeni bir akış analizi işi oluşturur veya mevcut bir belirtilen iş tanımı güncelleştirir.

İş adı .json dosyası veya komut satırında belirtilebilir. Her ikisi de belirtilirse, komut satırında adı bir dosya ile aynı olması gerekir.

Zaten bir iş adı belirtin ve belirtmeyin Force parametresini cmdlet mevcut iş değiştirmek isteyip istemediğinizi sorar.

Belirtirseniz, Force parametresi ve var olan bir iş adı belirtin iş tanımı onaysız değiştirilecek.

JSON dosya yapısı ve içeriği hakkında ayrıntılı bilgi için bkz [Stream Analytics işi oluşturmak] [ msdn-rest-api-create-stream-analytics-job] bölümünü [Stream Analytics Yönetimi REST API Başvurusu Kitaplığı] [stream.analytics.rest.api.reference].

**Örnek 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Bu PowerShell komutunu JobDefinition.json tanımında yeni bir proje oluşturur. İş tanımı dosyasında belirtilen ada sahip mevcut bir iş zaten tanımlanmış ise, cmdlet değiştirmek isteyip istemediğinizi sorar.

**Örnek 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Bu PowerShell komutunu iş tanımı için StreamingJob yerini alır.

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a>AzureStreamAnalyticsOutput yeni | AzureRMStreamAnalyticsOutput yeni
Akış analizi işi yeni çıktısı oluşturur veya mevcut bir çıktı güncelleştirir.  

Çıktı adı .json dosyası veya komut satırında belirtilebilir. Her ikisi de belirtilirse, komut satırında adı bir dosya ile aynı olması gerekir.

Zaten bir çıktı belirtin ve belirtmeyin Force parametresini cmdlet varolan çıkış değiştirmek isteyip istemediğinizi sorar.

Belirtirseniz – Force parametresi ve mevcut bir çıktı adı, çıktı onaysız değiştirilecek belirtin.

JSON dosya yapısı ve içeriği hakkında ayrıntılı bilgi için bkz [oluşturma çıkış (Azure akış analizi)] [ msdn-rest-api-create-stream-analytics-output] bölümünü [Stream Analytics Yönetimi REST API Başvurusu Kitaplık][stream.analytics.rest.api.reference].

**Örnek 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Bu PowerShell komutunu iş StreamingJob "çıktı" adlı yeni bir çıkış oluşturur. Bu ada sahip mevcut bir çıktı zaten tanımlanmış ise, cmdlet değiştirmek isteyip istemediğinizi sorar.

**Örnek 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Bu PowerShell komutunu "çıktı" tanımı iş StreamingJob yerini alır.

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a>New-AzureStreamAnalyticsTransformation | New-AzureRMStreamAnalyticsTransformation
Akış analizi işi içinde yeni bir dönüştürme oluşturur veya mevcut dönüştürme güncelleştirir.

Dönüştürme adını .json dosyası veya komut satırında belirtilebilir. Her ikisi de belirtilirse, komut satırında adı bir dosya ile aynı olması gerekir.

Zaten bir dönüşüm belirtin ve belirtmeyin Force parametresini cmdlet varolan dönüştürme değiştirmek isteyip istemediğinizi sorar.

Belirtirseniz, Force parametresi ve varolan bir dönüşüm adını belirtin dönüştürme onaysız değiştirilecek.

JSON dosya yapısı ve içeriği hakkında ayrıntılı bilgi için bkz [oluşturma dönüştürme (Azure akış analizi)] [ msdn-rest-api-create-stream-analytics-transformation] bölümünü [Stream Analytics Yönetimi REST API'si Başvuru Kitaplığı][stream.analytics.rest.api.reference].

**Örnek 1**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Bu PowerShell komutunu iş StreamingJob StreamingJobTransform adlı yeni bir dönüştürme oluşturur. Varolan bir dönüşüm zaten bu ada sahip tanımlanmışsa, cmdlet değiştirmek isteyip istemediğinizi sorar.

**Örnek 2**

Azure PowerShell 0.9.8:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 Bu PowerShell komutunu StreamingJob iş StreamingJobTransform tanımında değiştirir.

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a>Remove-AzureStreamAnalyticsInput | Remove-AzureRMStreamAnalyticsInput
Zaman uyumsuz olarak belirli bir giriş Microsoft Azure Stream Analytics işinde siler.  
Belirtirseniz – Force parametresini giriş onay silinir.

**Örnek 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Bu PowerShell komutunu giriş EventStream iş StreamingJob kaldırır.  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a>Remove-AzureStreamAnalyticsJob | Remove-AzureRMStreamAnalyticsJob
Zaman uyumsuz olarak bir belirli Microsoft Azure akış analizi işi siler.  
Belirtirseniz – Force parametresini iş onay silinir.

**Örnek 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Bu PowerShell komutunu StreamingJob iş kaldırır.  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a>Remove-AzureStreamAnalyticsOutput | Remove-AzureRMStreamAnalyticsOutput
Zaman uyumsuz olarak belirli bir çıkış Microsoft Azure Stream Analytics işinde siler.  
Belirtirseniz – Force parametresini çıkış onay silinir.

**Örnek 1**

Azure PowerShell 0.9.8:  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Çıktıyı iş StreamingJob çıkış bu PowerShell komutunu kaldırır.  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a>Start-AzureStreamAnalyticsJob | Start-AzureRMStreamAnalyticsJob
Zaman uyumsuz olarak dağıtır ve Microsoft Azure Stream Analytics işini başlatır.

**Örnek 1**

Azure PowerShell 0.9.8:  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Azure PowerShell 1.0:  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Bu PowerShell komutunu StreamingJob özel çıkış başlangıç saati ile 12:12:12 12 Kasım 2012 için ayarlanmış işini başlatır UTC.

### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a>Stop-AzureStreamAnalyticsJob | Stop-AzureRMStreamAnalyticsJob
Zaman uyumsuz olarak Microsoft Azure üzerinde çalışan bir Stream Analytics işini durdurur ve kullanılmakta olan kaynakları serbest bırakır. İş düzenlenmesi ve yeniden gibi meta verileri ve iş tanımı aboneliğinizi Azure portalı ve yönetim API'si aracılığıyla içinde kullanılabilir kalır. Durdurulmuş durumdaki bir iş için ücret alınmaz.

**Örnek 1**

Azure PowerShell 0.9.8:  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Azure PowerShell 1.0:  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Bu PowerShell komutunu StreamingJob işini durdurur.  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a>Test-AzureStreamAnalyticsInput | Test-AzureRMStreamAnalyticsInput
Akış analizi için belirtilen giriş bağlantı olanağı sınar.

**Örnek 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Bu PowerShell komutunu StreamingJob içinde giriş EntryStream bağlantı durumunu sınar.  

### <a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a>Test-AzureStreamAnalyticsOutput | Test-AzureRMStreamAnalyticsOutput
Stream Analytics belirli bir çıktıya bağlanma yeteneğini sınar.

**Örnek 1**

Azure PowerShell 0.9.8:  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Bu PowerShell komut testleri StreamingJob çıkış bağlantı durumunu çıktı.  

## <a name="get-support"></a>Destek alın
Daha fazla yardım için deneyin bizim [Azure Stream Analytics forumumuzu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics). 

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

