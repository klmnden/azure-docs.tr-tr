---
title: İle Azure Automation runbook günlük analizi veri toplama | Microsoft Docs
description: Adım adım öğretici, Azure Automation'ın günlük analizi tarafından analiz için OMS depoya verileri toplamak için bir runbook oluşturmada size yol göstermektedir.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: a831fd90-3f55-423b-8b20-ccbaaac2ca75
ms.service: operations-management-suite
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2017
ms.author: bwren
ms.openlocfilehash: 0784e2317fbc98561b486547654ca27bb30e76c3
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
ms.locfileid: "31597376"
---
# <a name="collect-data-in-log-analytics-with-an-azure-automation-runbook"></a>Günlük analizi olarak toplamak ve bir Azure Otomasyonu runbook'u
Günlük analizi veri önemli miktarda bir çeşitli kaynaklardan dahil olmak üzere toplayabilirsiniz [veri kaynakları](../log-analytics/log-analytics-data-sources.md) aracılar üzerinde hem de [Azure'dan toplanan veriler](../log-analytics/log-analytics-azure-storage.md).  Burada veri toplamanız gerekir, bu standart kaynakları aracılığıyla erişilebilir değil ancak bir senaryo vardır.  Bu durumlarda, kullandığınız [HTTP veri toplayıcı API](../log-analytics/log-analytics-data-collector-api.md) herhangi bir REST API istemciden için günlük analizi veri yazmak için.  Bu veri toplama gerçekleştirmek için ortak bir yöntemi, Azure Automation'da bir runbook kullanıyor.   

Bu öğreticide oluşturma ve bir runbook için günlük analizi veri yazmak için Azure automation'da zamanlama işlemi açıklanmaktadır.


## <a name="prerequisites"></a>Önkoşullar
Bu senaryo, Azure aboneliğinizde yapılandırılmış aşağıdaki kaynaklara gerektirir.  Her ikisi de ücretsiz bir hesap olabilir.

- [Günlük analizi çalışma alanı](../log-analytics/log-analytics-get-started.md).
- [Azure Otomasyon hesabı](../automation/automation-offering-get-started.md).

## <a name="overview-of-scenario"></a>Senaryoya genel bakış
Bu öğretici için Otomasyon işleri hakkında bilgi toplar bir runbook yazacaksınız.  Azure Otomasyonu runbook'ları yazma ve komut dosyası Azure Otomasyon Düzenleyicisi'nde test tarafından başlayacaksınız şekilde PowerShell ile uygulanır.  Gerekli bilgileri topladığınız doğrulayın sonra bu veri yazmak için günlük analizi ve özel veri türü doğrulayın.  Son olarak, düzenli aralıklarla runbook'u başlatmak için bir zamanlama oluşturacaksınız.

> [!NOTE]
> Azure Otomasyonu işi bilgisi bu runbook için günlük analizi göndermek için yapılandırabilirsiniz.  Bu senaryoda öncelikle öğretici desteklemek için kullanılır ve test çalışma alanına verileri gönder önerilir.  


## <a name="1-install-data-collector-api-module"></a>1. Veri Toplayıcı API modülünü yükleme
Her [HTTP veri toplayıcı API isteğinden](../log-analytics/log-analytics-data-collector-api.md#create-a-request) uygun şekilde biçimlendirilmiş olması gerekir ve bir authorization üstbilgisi ekleyin.  Bunu runbook'unuzda yapabilirsiniz ancak bu işlemi basitleştirir bir modül kullanarak gerekli kod miktarını düşürebilir.  Kullanabileceğiniz bir modül [OMSIngestionAPI Modülü](https://www.powershellgallery.com/packages/OMSIngestionAPI) PowerShell galerisinde.

Kullanılacak bir [Modülü](../automation/automation-integration-modules.md) bir runbook'ta Otomasyon hesabınızda yüklenmelidir.  Aynı hesapta herhangi bir runbook, ardından modülünde işlevlerini kullanabilirsiniz.  Yeni bir modül seçerek yükleyebilirsiniz **varlıklar** > **modülleri** > **bir modül Ekle** Otomasyon hesabınızda.  

PowerShell Galerisi yine de Bu öğretici için bu seçeneği kullanabilmeniz için bir modül Otomasyon hesabınızı doğrudan dağıtmak için hızlı bir seçeneği sunar.  

![OMSIngestionAPI Modülü](media/operations-management-suite-runbook-datacollect/OMSIngestionAPI.png)

1. Git [PowerShell Galerisi](https://www.powershellgallery.com/).
2. Arama **OMSIngestionAPI**.
3. Tıklayın **Azure Otomasyonu Dağıt** düğmesi.
4. Otomasyon hesabınızı seçin ve tıklatın **Tamam** modülünü yüklemek için.


## <a name="2-create-automation-variables"></a>2. Otomasyon değişkenleri oluşturma
[Otomasyon değişkenleri](..\automation\automation-variables.md) Otomasyon hesabınızda tüm runbook'lar tarafından kullanılan değerleri tutun.  Runbook'ları gerçek runbook düzenleme olmadan bu değerleri değiştirmek olanak sağlayarak daha esnek hale. HTTP veri toplayıcı API'sinden her istek kimliği ve anahtarı OMS çalışma alanının gerektirir ve bu bilgileri depolamak değişken varlıkları idealdir.  

![Değişkenler](media/operations-management-suite-runbook-datacollect/variables.png)

1. Azure Portal'da, Automation hesabınızı gidin.
2. Seçin **değişkenleri** altında **paylaşılan kaynakları**.
2. Tıklatın **değişken Ekle** ve aşağıdaki tabloda iki değişken oluşturun.

| Özellik | Çalışma alanı kimliği değeri | Çalışma alanı anahtarı değeri |
|:--|:--|:--|
| Ad | Workspaceıd | WorkspaceKey |
| Tür | Dize | Dize |
| Değer | Günlük analizi çalışma alanınız çalışma Kimliğinde yapıştırın. | Yapıştırma oturum birincil veya ikincil anahtarı günlük analizi çalışma alanınızın. |
| Şifreli | Hayır | Evet |



## <a name="3-create-runbook"></a>3. Runbook oluşturma

Azure Otomasyonu, düzenlemek ve runbook'unuzu test portalında bir düzenleyici içerir.  Çalışmak için komut dosyası Düzenleyicisi'ni kullanmak için seçeneğiniz [doğrudan PowerShell](../automation/automation-edit-textual-runbook.md) veya [grafik runbook oluşturma](../automation/automation-graphical-authoring-intro.md).  Bu öğreticide, bir PowerShell Betiği ile çalışır. 

![Runbook’u düzenleme](media/operations-management-suite-runbook-datacollect/edit-runbook.png)

1. Otomasyon hesabınıza gidin.  
2. Tıklatın **Runbook'lar** > **runbook Ekle** > **yeni bir runbook oluşturmak**.
3. Runbook ad için **toplama Otomasyon işleri**.  Runbook türü için **PowerShell**.
4. Tıklatın **oluşturma** runbook oluşturma ve Düzenleyicisi'ni başlatın.
5. Aşağıdaki kodu kopyalayıp runbook'a yapıştırın.  Kod açıklaması için komut dosyası yer alan yorumlara bakın.
    
        # Get information required for the automation account from parameter values when the runbook is started.
        Param
        (
            [Parameter(Mandatory = $True)]
            [string]$resourceGroupName,
            [Parameter(Mandatory = $True)]
            [string]$automationAccountName
        )
        
        # Authenticate to the Automation account using the Azure connection created when the Automation account was created.
        # Code copied from the runbook AzureAutomationTutorial.
        $connectionName = "AzureRunAsConnection"
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         
        Connect-AzureRmAccount `
            -ServicePrincipal `
            -TenantId $servicePrincipalConnection.TenantId `
            -ApplicationId $servicePrincipalConnection.ApplicationId `
            -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
        
        # Set the $VerbosePreference variable so that we get verbose output in test environment.
        $VerbosePreference = "Continue"
        
        # Get information required for Log Analytics workspace from Automation variables.
        $customerId = Get-AutomationVariable -Name 'WorkspaceID'
        $sharedKey = Get-AutomationVariable -Name 'WorkspaceKey'
        
        # Set the name of the record type.
        $logType = "AutomationJob"
        
        # Get the jobs from the past hour.
        $jobs = Get-AzureRmAutomationJob -ResourceGroupName $resourceGroupName -AutomationAccountName $automationAccountName -StartTime (Get-Date).AddHours(-1)
        
        if ($jobs -ne $null) {
            # Convert the job data to json
            $body = $jobs | ConvertTo-Json
        
            # Write the body to verbose output so we can inspect it if verbose logging is on for the runbook.
            Write-Verbose $body
        
            # Send the data to Log Analytics.
            Send-OMSAPIIngestionFile -customerId $customerId -sharedKey $sharedKey -body $body -logType $logType -TimeStampField CreationTime
        }


## <a name="4-test-runbook"></a>4. Runbook sınaması
Azure Otomasyonu içeren bir ortama [runbook'unuzu test](../automation/automation-testing-runbook.md) yayımlamadan önce.  Runbook tarafından toplanan verileri incelemek ve üretim yayımlamadan önce beklendiği gibi günlük analizi Yazar olduğunu doğrulayın. 
 
![Runbook sınaması](media/operations-management-suite-runbook-datacollect/test-runbook.png)

6. Tıklatın **kaydetmek** runbook'u kaydetmek için.
1. Tıklatın **Test bölmesi** runbook test ortamında açın.
3. Runbook parametreleri olduğundan, bunlar için değerler girin istenir.  Kaynak grubunun adını girin ve automation hesabı, iş bilgilerini toplamak için olacaktır.
4. Tıklatın **Başlat** runbook'u başlatmak için.
3. Runbook durumu ile başlar **sıraya alınan** için geçmeden önce **çalıştıran**.  
3. Json biçiminde toplanan işleri ile runbook ayrıntılı çıktı görüntülemesi gerekir.  Hiç iş listelenmiyorsa, ardından karşılaşılmış son bir saatte automation hesabında oluşturulan iş yok.  Herhangi bir runbook Otomasyon hesabında başlatmayı deneyin ve test yeniden gerçekleştirin.
4. Çıktı için günlük analizi post komutta hataları göstermez emin olun.  Aşağıdakine benzer bir ileti olmalıdır.

    ![POST çıktı](media/operations-management-suite-runbook-datacollect/post-output.png)

## <a name="5-verify-records-in-log-analytics"></a>5. Günlük analizi kayıtlarını doğrulama
Runbook testi tamamlandı ve çıktı başarıyla alındı doğruladıktan sonra kayıtları kullanılarak oluşturulan doğrulayabilirsiniz bir [günlük analizi günlük aramada](../log-analytics/log-analytics-log-searches.md).

![Günlük çıktısı](media/operations-management-suite-runbook-datacollect/log-output.png)

1. Azure portalında günlük analizi çalışma alanınızı seçin.
2. Tıklayın **oturum arama**.
3. Aşağıdaki komutu yazın `Type=AutomationJob_CL` ve arama düğmesini tıklatın. Kayıt türü betik belirlenmezse _CL içerdiğine dikkat edin.  Bu soneki özel günlük türü olduğunu belirtmek için günlük türü otomatik olarak eklenir.
4. Runbook beklendiği gibi çalıştığını gösteren döndürülen bir veya daha fazla kayıt görmeniz gerekir.


## <a name="6-publish-the-runbook"></a>6. Runbook yayımlama
Runbook düzgün çalıştığını doğruladıktan sonra üretim ortamında çalışması için yayımlayın gerekir.  Düzenle ve yayımlanan sürümü değiştirmeden runbook'u test devam edebilirsiniz.  

![Runbook yayımlama](media/operations-management-suite-runbook-datacollect/publish-runbook.png)

1. Otomasyon hesabınızı döndür.
2. Tıklayın **Runbook'lar** seçip **toplama Otomasyon işleri**.
3. Tıklatın **Düzenle** ve ardından **yayımlama**.
4. Tıklatın **Evet** önceden yayımlanmış sürümün üzerine istediğinizi doğrulamanız istendiğinde.

## <a name="7-set-logging-options"></a>7. Günlüğe kaydetme seçeneklerini ayarlama 
Test için görmeye [ayrıntılı çıktı](../automation/automation-runbook-output-and-messages.md#message-streams) komut dosyasında $VerbosePreference değişkenini olduğundan.  Üretim için ayrıntılı çıktı görüntülemek istiyorsanız runbook için günlük özelliklerini ayarlamanız gerekir.  Bu öğreticide kullanılan runbook için bu günlük analizi için gönderilen json verilerini görüntüler.

![Günlüğe kaydetme ve izleme](media/operations-management-suite-runbook-datacollect/logging.png)

1. Runbook'unuzda özelliklerinde seçin **günlüğe kaydetme ve izleme** altında **Runbook ayarlarını**.
2. Ayarını değiştirmek **ayrıntılı kayıtları günlüğe** için **üzerinde**.
3. **Kaydet**’e tıklayın.

## <a name="8-schedule-runbook"></a>8. Runbook zamanlama
İzleme verilerini toplayan bir runbook'u başlatmak için en yaygın otomatik olarak çalışacak şekilde zamanlamak için yoludur.  Oluşturarak bunu bir [Azure Otomasyonu zamanlama](../automation/automation-schedules.md) ve runbook'a ekleme.

![Runbook zamanlama](media/operations-management-suite-runbook-datacollect/schedule-runbook.png)

1. Runbook'unuzda özelliklerini seçin **zamanlamaları** altında **kaynakları**.
2. Tıklatın **bir zamanlama Ekle** > **runbook'a bir zamanlama Bağla** > **yeni bir zamanlama oluşturmak**.
5. Zamanlama için aşağıdaki değerleri yazın ve'ı tıklatın **oluşturma**.

| Özellik | Değer |
|:--|:--|
| Ad | AutomationJobs saatlik |
| Başlangıç | Herhangi bir geçerli saati aşan en az 5 dakika zaman'ı seçin. |
| Yineleme | Yinelenen |
| Yinelenme: | 1 saat |
| Kümesi süre sonu | Hayır |

Zamanlama oluşturulduktan sonra bu zamanlamanın runbook her başlatıldığında kullanılacak parametre değerlerini ayarlamak gerekir.

6. Tıklatın **parametreleri yapılandırmak ve çalıştırma ayarlarını**.
7. Değerlerini doldurun, **ResourceGroupName** ve **AutomationAccountName**.
8. **Tamam**’a tıklayın. 

## <a name="9-verify-runbook-starts-on-schedule"></a>9. Zamanlama runbook başlatır doğrulayın
Bir runbook başlatıldığında, her [bir iş oluşturulur](../automation/automation-runbook-execution.md) ve oturum herhangi bir çıktı.  Aslında, runbook toplama aynı işleri bunlar.  Runbook zamanlama için başlangıç saatini geçtikten sonra runbook için iş denetleyerek beklendiği gibi başlatır doğrulayabilirsiniz.

![İşler](media/operations-management-suite-runbook-datacollect/jobs.png)

1. Runbook'unuzda özelliklerini seçin **işleri** altında **kaynakları**.
2. İşlerin listesini runbook başlatıldığında her zaman için görmeniz gerekir.
3. Ayrıntılarını görüntülemek için işleri birini tıklatın.
4. Tıklayın **tüm günlükleri** günlükleri görüntülemek ve runbook'tan çıkış.
5. Bir giriş aşağıdaki görüntü benzer bulmak için aşağıya kaydırın.<br>![Ayrıntılı](media/operations-management-suite-runbook-datacollect/verbose.png)
6. Günlük analizi için gönderilen ayrıntılı json verilerini görüntülemek için bu giriş'i tıklatın.



## <a name="next-steps"></a>Sonraki adımlar
- Kullanım [Görünüm Tasarımcısı](../log-analytics/log-analytics-view-designer.md) günlük analizi depoya derledik verileri görüntüleyen bir görünüm oluşturmak için.
- Runbook'unuzda paketini bir [yönetim çözümü](operations-management-suite-solutions-creating.md) müşterilere dağıtmak için.
- Daha fazla bilgi edinmek [günlük analizi](https://docs.microsoft.com/azure/log-analytics/).
- Daha fazla bilgi edinmek [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/).
- Daha fazla bilgi edinmek [HTTP veri toplayıcı API](../log-analytics/log-analytics-data-collector-api.md).
