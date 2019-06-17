---
title: Azure automation'da bir runbook ile log Analytics veri toplama | Microsoft Docs
description: Adım adım öğretici, Log Analytics tarafından depoda analiz verilerini toplamak için Azure automation'da bir runbook oluşturma işleminde size yol gösterir.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: a831fd90-3f55-423b-8b20-ccbaaac2ca75
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 05/27/2017
ms.author: bwren
ms.openlocfilehash: 67378a5911e5bd83888342aa3773f7f5ed4ccf29
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60454194"
---
# <a name="collect-data-in-log-analytics-with-an-azure-automation-runbook"></a>Azure Otomasyonu runbook'u Log analytics'te verileri toplama

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bir çeşitli kaynaklardan da dahil olmak üzere önemli miktarda Log analytics'te verileri toplayabilir [veri kaynakları](../../azure-monitor/platform/agent-data-sources.md) aracılarda da [Azure'dan toplanan veriler](../../azure-monitor/platform/collect-azure-metrics-logs.md). Veri toplamak gereken, bu standart kaynakları aracılığıyla erişilebilir durumda değil ancak bir senaryo vardır. Bu durumlarda, kullandığınız [HTTP veri toplayıcı API'sini](../../azure-monitor/platform/data-collector-api.md) herhangi bir REST API istemcisinden Log Analytics'e veri yazmak için. Bu veri toplamayı gerçekleştirmek için genel bir yöntemi, Azure Automation'da bir runbook kullanıyor.

Bu öğreticide oluşturma ve Log Analytics'e veri yazmak için Azure Otomasyonu'nda runbook zamanlama işleminde size yol gösterir.

## <a name="prerequisites"></a>Önkoşullar
Bu senaryo, aşağıdaki kaynakları Azure aboneliğinizde yapılandırılmış gerektirir. Her ikisi de ücretsiz bir hesap olabilir.

- [Log Analytics çalışma alanı](../../azure-monitor/learn/quick-create-workspace.md).
- [Azure Otomasyonu hesabı](../..//automation/automation-quickstart-create-account.md).

## <a name="overview-of-scenario"></a>Senaryoya genel bakış
Bu öğreticide, Otomasyon işleri hakkında bilgi toplayan bir runbook yazacaksınız. Azure Otomasyonu Düzenleyicisi'nde bir betik yazma ve başlatmak için PowerShell ile Azure automation'daki Runbook'lar uygulanır. Gerekli bilgileri toplayacağınızı doğruladıktan sonra verileri Log Analytics'e yazmak ve özel veri türü doğrulamak. Son olarak, düzenli aralıklarla runbook'u başlatmak için bir zamanlama oluşturacaksınız.

> [!NOTE]
> Azure Otomasyonu, iş bilgileri bu runbook Log Analytics'e göndermek için yapılandırabilirsiniz. Bu senaryo öncelikle öğretici desteklemek için kullanılır ve bir test çalışma alanına veri göndermek önerilir.

## <a name="1-install-data-collector-api-module"></a>1. Veri Toplayıcı API'sini modülünü yükleme
Her [HTTP veri toplayıcı API'sini istekten](../../azure-monitor/platform/data-collector-api.md#create-a-request) uygun şekilde biçimlendirilmesi gerekir ve bir yetkilendirme üst bilgisi ekleyin. Runbook'unuza bunu yapabilirsiniz, ancak bu süreci kolaylaştırır, modül kullanarak gereken kod miktarını azaltır. Kullanabileceğiniz bir modül [OMSIngestionAPI Modülü](https://www.powershellgallery.com/packages/OMSIngestionAPI) PowerShell galerisinde.

Kullanılacak bir [Modülü](../../automation/automation-integration-modules.md) bir runbook'ta Otomasyon hesabınızda yüklenmelidir.  Aynı hesaptaki herhangi bir runbook, ardından modüldeki işlevleri kullanabilirsiniz. Yeni modül seçerek yükleyebileceğiniz **varlıklar** > **modülleri** > **Modül Ekle** Otomasyon hesabınızdaki.

PowerShell Galerisi rağmen Bu öğretici için bu seçeneği kullanabilmeniz için Otomasyon hesabınıza doğrudan bir modül dağıtmak için hızlı bir seçenek sağlar.

![OMSIngestionAPI Modülü](media/runbook-datacollect/OMSIngestionAPI.png)

1. Git [PowerShell Galerisi](https://www.powershellgallery.com/).
2. Arama **OMSIngestionAPI**.
3. Tıklayarak **Azure Otomasyonu Dağıt** düğmesi.
4. Otomasyon hesabınızı seçin ve tıklayın **Tamam** modülü yüklemek için.

## <a name="2-create-automation-variables"></a>2. Otomasyon değişkenleri oluşturma
[Otomasyon değişkenleri](../../automation/automation-variables.md) Otomasyon hesabınızdaki tüm runbook'lar tarafından kullanılabilen değerleri tutar. Runbook'ları daha esnek, gerçek runbook düzenleme olmadan bu değerleri değiştirmek olanak tanıyarak yaptıkları. Log Analytics çalışma alanı anahtarını ve kimliği her isteğin HTTP veri toplayıcı API'sini gerektirir ve değişken varlıklar, bu bilgileri depolamak idealdir.

![Değişkenler](media/runbook-datacollect/variables.png)

1. Azure portalında, Otomasyon hesabınıza gidin.
2. Seçin **değişkenleri** altında **paylaşılan kaynaklar**.
2. Tıklayın **değişken Ekle** ve aşağıdaki tabloda iki değişken oluşturun.

| Özellik | Çalışma alanı kimliği değeri | Çalışma alanı anahtarı değeri |
|:--|:--|:--|
| Ad | Çalışma alanı kimliği | WorkspaceKey |
| Tür | String | String |
| Değer | Log Analytics çalışma alanınızın çalışma alanı Kimliğini yapıştırın. | Yapıştır oturum birincil veya ikincil anahtar Log Analytics çalışma alanınızın. |
| Şifreli | Hayır | Evet |

## <a name="3-create-runbook"></a>3. Runbook oluşturma

Azure Otomasyonu, düzenlemek ve runbook'unuzu test portalda bir düzenleyici içerir. Kod Düzenleyicisi ile çalışmak üzere kullanma seçeneğiniz [doğrudan PowerShell](../../automation/automation-edit-textual-runbook.md) veya [grafik runbook'u oluşturma](../../automation/automation-graphical-authoring-intro.md). Bu öğreticide, bir PowerShell Betiği ile çalışır.

![Runbook’u düzenleme](media/runbook-datacollect/edit-runbook.png)

1. Otomasyon hesabınıza gidin.
2. Tıklayın **runbook'ları** > **runbook Ekle** > **yeni bir runbook oluşturmak**.
3. Runbook adı, türü **toplama Otomasyon işleri**. Runbook türü için **PowerShell**.
4. Tıklayın **Oluştur** runbook oluşturma ve Düzenleyicisi'ni başlatın.
5. Aşağıdaki kodu kopyalayıp runbook'a yapıştırın. Kod açıklaması için koddaki açıklamalara bakın.
    ```
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
    Connect-AzAccount `
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
    $jobs = Get-AzAutomationJob -ResourceGroupName $resourceGroupName -AutomationAccountName $automationAccountName -StartTime (Get-Date).AddHours(-1)
    
    if ($jobs -ne $null) {
        # Convert the job data to json
        $body = $jobs | ConvertTo-Json
    
        # Write the body to verbose output so we can inspect it if verbose logging is on for the runbook.
        Write-Verbose $body
    
        # Send the data to Log Analytics.
        Send-OMSAPIIngestionFile -customerId $customerId -sharedKey $sharedKey -body $body -logType $logType -TimeStampField CreationTime
    }
    ```

## <a name="4-test-runbook"></a>4. Runbook testi
Azure Otomasyonu, bir ortama içerir [runbook'unuzu test](../../automation/automation-testing-runbook.md) yayımlamadan önce. Runbook tarafından toplanan verileri incelemek ve bunu Log Analytics'e üretime yayımlamadan önce beklendiği gibi yazdığını doğrulayın.

![Runbook testi](media/runbook-datacollect/test-runbook.png)

1. Tıklayın **Kaydet** runbook'u kaydetmek.
1. Tıklayın **Test bölmesi** runbook test ortamında açın.
1. Runbook parametreleri olduğundan, bunlar için değerleri girin istenir. Kaynak grubu adını girin ve Otomasyon hesabı, iş bilgileri toplamak için anlatılmıştır.
1. Tıklayın **Başlat** runbook başlatma.
1. Runbook durumu ile başlar **sıraya alınan** için geçmeden önce **çalıştıran**.
1. Json biçiminde toplanan işleri ile runbook ayrıntılı çıkış görüntülenmesi gerekir. İş listelenirse, ardından oluşmuş olabilir Otomasyon hesabında son bir saat içinde oluşturulan iş yok. Tüm runbook Otomasyon hesabında başlatmayı deneyin ve testi yeniden gerçekleştirin.
1. Çıkış Log Analytics'e sonrası komut hataları göstermez emin olun. Bir ileti aşağıdakine benzer olması gerekir.

    ![POST çıkış](media/runbook-datacollect/post-output.png)

## <a name="5-verify-records-in-log-analytics"></a>5. Log Analytics kayıtları doğrulayın
Runbook testi tamamlandı ve çıkış başarıyla alındı doğrulandı sonra kayıtları kullanarak oluşturulduğunu doğrulayabilirsiniz bir [Log analytics'te günlük araması](../../azure-monitor/log-query/log-query-overview.md).

![Günlük çıktısı](media/runbook-datacollect/log-output.png)

1. Azure portalında Log Analytics çalışma alanınızı seçin.
2. Tıklayarak **günlük arama**.
3. Aşağıdaki komutu yazın `Type=AutomationJob_CL` ve arama düğmesine tıklayın. Kayıt türü betikte belirtilmemiş _CL içerdiğine dikkat edin. Soneki, özel günlük türü olduğunu belirtmek için günlük türü otomatik olarak eklenir.
4. Döndürülen runbook beklendiği gibi çalıştığını belirten bir veya daha fazla kayıt görmeniz gerekir.

## <a name="6-publish-the-runbook"></a>6. Runbook yayımlama
Runbook doğru şekilde çalıştığını doğruladıktan sonra üretim ortamında çalıştırabilmeniz için bunu yayımlamanız gerekir. Düzenle ve runbook'u yayımlanan sürümü değiştirmeden test devam edebilirsiniz.

![Runbook yayımlama](media/runbook-datacollect/publish-runbook.png)

1. Otomasyon hesabınıza döndürür.
2. Tıklayarak **runbook'ları** seçip **toplama Otomasyon işleri**.
3. Tıklayın **Düzenle** ardından **yayımlama**.
4. Tıklayın **Evet** , önceden yayımlanmış sürümünün üzerine istediğinizi doğrulamanız sorulduğunda.

## <a name="7-set-logging-options"></a>7. Günlüğe kaydetme seçeneklerini ayarlama
Test için görmeye [ayrıntılı çıkış](../../automation/automation-runbook-output-and-messages.md#message-streams) betikte $VerbosePreference değişkenini ayarlamak için. Üretim için ayrıntılı çıkış görüntülemek istiyorsanız, runbook için günlük özelliklerini ayarlamanız gerekir. Bu öğreticide kullanılan runbook için bu Log Analytics'e gönderilen json verilerini görüntüler.

![Günlüğe kaydetme ve izleme](media/runbook-datacollect/logging.png)

1. Runbook'unuzda özelliklerinde seçin **günlüğe kaydetme ve izleme** altında **Runbook ayarlarını**.
2. Ayarını değiştirme **ayrıntılı kayıtları günlüğe** için **üzerinde**.
3. **Kaydet**’e tıklayın.

## <a name="8-schedule-runbook"></a>8. Runbook zamanlama
İzleme verilerini toplayan bir runbook başlatmak için en yaygın yolu, otomatik olarak çalışacak şekilde zamanlamak sağlamaktır. Oluşturarak bunu bir [Azure Otomasyonu zamanlama](../../automation/automation-schedules.md) ve runbook uygulamanıza ekleme.

![Runbook zamanlama](media/runbook-datacollect/schedule-runbook.png)

1. Runbook'unuzda özelliklerini seçin **zamanlamaları** altında **kaynakları**.
2. Tıklayın **zamanlama Ekle** > **bir zamanlamayı runbook'a bağlamak** > **yeni bir zamanlama oluşturmak**.
5. Zamanlama için aşağıdaki değerleri girin ve tıklatın **Oluştur**.

| Özellik | Değer |
|:--|:--|
| Ad | AutomationJobs saatlik |
| Başlatır | Geçerli saati aşan en az 5 dakika için istediğiniz zaman seçin. |
| Yinelenme | Yinelenen |
| Yineleme her | 1 saat |
| Süre sonu Ayarla | Hayır |

Zamanlama oluşturulduktan sonra bu zamanlamanın runbook her başlatıldığında kullanılacak parametre değerlerini ayarlamak gerekir.

1. Tıklayın **parametrelerini yapılandırma ve çalıştırma ayarları**.
1. Değerleri girin, **ResourceGroupName** ve **AutomationAccountName**.
1. **Tamam**'ı tıklatın.

## <a name="9-verify-runbook-starts-on-schedule"></a>9. Runbook başlatıldığında zamanlamaya göre doğrulayın
Bir runbook her başlatıldığında [bir iş oluşturulur](../../automation/automation-runbook-execution.md) ve günlüğe herhangi bir çıktı. Aslında, runbook toplama aynı işleri şunlardır. Runbook zamanlama için başlangıç saatini geçtikten sonra runbook için iş kontrol ederek beklenen şekilde başladığını doğrulayabilirsiniz.

![İşler](media/runbook-datacollect/jobs.png)

1. Runbook'unuzda özelliklerini seçin **işleri** altında **kaynakları**.
2. Runbook'un başlatıldığı her saat için işlerinin listesini görmeniz gerekir.
3. Ayrıntılarını görüntülemek için işler birine tıklayın.
4. Tıklayarak **tüm günlükler** günlükleri görüntülemek ve runbook'tan çıkış.
5. Aşağıdaki görüntüye benzer bir giriş bulunacak En Alta kaydırın.<br>![Ayrıntılı](media/runbook-datacollect/verbose.png)
6. Bu giriş, Log Analytics'e gönderilen ayrıntılı json verilerini görüntülemek için tıklayın.

## <a name="next-steps"></a>Sonraki adımlar
- Kullanım [Görünüm Tasarımcısı](../../azure-monitor/platform/view-designer.md) Log Analytics deposuna derledik verileri görüntüleyen bir görünüm oluşturmak için.
- Runbook'unuza paketini bir [yönetimi çözümü](../../azure-monitor/insights/solutions-creating.md) müşterilere dağıtmak için.
- Daha fazla bilgi edinin [Log Analytics](https://docs.microsoft.com/azure/log-analytics/).
- Daha fazla bilgi edinin [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/).
- Daha fazla bilgi edinin [HTTP veri toplayıcı API'sini](../../azure-monitor/platform/data-collector-api.md).
