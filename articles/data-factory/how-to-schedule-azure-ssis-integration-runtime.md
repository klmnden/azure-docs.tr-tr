---
title: Azure-SSIS tümleştirme çalışma zamanı zamanlama | Microsoft Docs
description: Bu makalede, Azure Data Factory kullanarak başlatma ve durdurma Azure-SSIS Integration Runtime'nın zamanlamak açıklar.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 1/9/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 54d7979f9fbe23e9372aa2702b46e42ca64496d2
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58621643"
---
# <a name="how-to-start-and-stop-azure-ssis-integration-runtime-on-a-schedule"></a>Nasıl bir zamanlamaya göre Azure-SSIS tümleştirme çalışma zamanını durdurmak ve başlatmak
Bu makalede, Azure Data Factory (ADF) kullanarak zamanlama başlatma ve durdurma Azure-SSIS Integration Runtime (IR) açıklar. Azure-SSIS IR, ADF kaynak SQL Server Integration Services (SSIS) paketlerini yürütmek için adanmış işlem ' dir. Azure-SSIS IR çalıştıran, kendisiyle ilişkili bir maliyeti vardır. Bu nedenle, genellikle, IR, SSIS paketlerini Azure'da yürütmek ve artık ihtiyacınız olmayan olduğunda, IR durdurmak yalnızca ihtiyacınız olduğunda çalıştırmak istediğiniz. Kullanabileceğiniz ADF kullanıcı arabirimi (UI) / uygulama veya Azure PowerShell ile [el ile başlatın veya durdurun, IR](manage-azure-ssis-integration-runtime.md)).

Alternatif olarak, örneğin, sabah günlük ETL iş yüklerinizi yürütme ile öğleden sonra aldıkları durdurmadan önce başlayan, IR zamanlamaya göre Başlat/Durdur için ADF işlem hatları Web etkinliği oluşturabilirsiniz.  Ayrıca, başlatma ve bu nedenle, IR Başlat/Durdur önce/sonra paketi yürütme zamanında isteğe bağlı olarak, IR durdurma iki Web etkinliği arasında bir SSIS paketi yürütme etkinliği zincirleyebilirsiniz. SSIS paketi yürütme etkinliği hakkında daha fazla bilgi için bkz. [ADF işlem hattında SSIS paketi yürütme etkinliği kullanan bir SSIS paketi çalıştırmak](how-to-invoke-ssis-package-ssis-activity.md) makalesi.

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

## <a name="prerequisites"></a>Önkoşullar
Azure-SSIS IR zaten sağladığınız değil, konusundaki yönergeleri izleyerek sağlama [öğretici](tutorial-create-azure-ssis-runtime-portal.md). 

## <a name="create-and-schedule-adf-pipelines-that-start-and-or-stop-azure-ssis-ir"></a>Oluşturma ve Azure-SSIS IR durdurmak ve başlatmak ADF işlem hatlarını zamanlama
Bu bölümde, Web etkinliği ADF işlem hatlarında Azure-SSIS IR zamanlamada başlatma/durdurma veya başlangıç & isteğe bağlı olarak durdurmak için nasıl kullanılacağını gösterir. Üç ardışık düzenleri oluşturmak için kılavuzluk eder: 

1. İlk işlem hattı, Azure-SSIS IR'yi başlatan bir Web etkinliği içeren 
2. İkinci işlem hattı, Azure-SSIS IR'yi durduran bir Web etkinliği içeren
3. Üçüncü bir işlem hattı, Azure-SSIS IR'yi Başlat/Durdur iki Web etkinliği zincirleme bir SSIS paketi yürütme etkinliği içeriyor 

Oluşturma ve bu işlem hatları test sonra zamanlama tetikleyicisi oluşturma ve herhangi bir işlem hattı ile ilişkilendirin. Zamanlama tetikleyicisi ilişkili işlem hattı çalıştırma zamanlaması tanımlar. 

Örneğin, iki Tetikleyiciler oluşturabilir, ilk günlük 6'da çalıştırmak için zamanlanan ve ikincisi günlük 6 saat çalışacak şekilde zamanlanmış sırasında ilk işlem hattı çalıştırmasıyla ilişkili ve ikinci işlem hattı çalıştırmasıyla ilişkili.  Bu şekilde, bir dönem arasında 6 AM için 18: 00, IR çalışırken, her gün günlük ETL iş yüklerinizi çalıştırmak için hazır olması.  

Gece yarısı her gün çalışacak şekilde zamanlanırsa üçüncü bir tetikleyici oluşturur ve üçüncü işlem hattı çalıştırmasıyla ilişkili, işlem hattı çalıştırmasını gece yarısı her gün başlangıç paketiniz, daha sonra yürütme, IR paket yürütme hemen önce çalıştırılır ve hemen IR hemen sonra paket yürütme durduruluyor, bu nedenle, IR hesaplamaları çalışmıyor olacaktır.

### <a name="create-your-adf"></a>ADF oluşturma

1. [Azure portalda](https://portal.azure.com/) oturum açın.    
2. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
   
3. İçinde **yeni veri fabrikası** want **MyAzureSsisDataFactory** için **adı**. 
      
   ![Yeni veri fabrikası sayfası](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   ADF adını genel olarak benzersiz olmalıdır. Aşağıdaki hata iletisini alırsanız, ADF (örneğin yournameMyAzureSsisDataFactory) adını değiştirin ve yeniden oluşturmayı deneyin. Bkz: [Data Factory - adlandırma kuralları](naming-rules.md) makalenin ADF yapıtlarının adlandırma kuralları hakkında bilgi edinin.
  
   `Data factory name MyAzureSsisDataFactory is not available`
      
4. Azure'ı seçin **abonelik** , ADF oluşturmak istediğiniz altında. 
5. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
   - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
   - Seçin **Yeni Oluştur**, yeni kaynak grubunuzun adını girin.   
         
   Kaynak grupları hakkında daha fazla bilgi için bkz: [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md) makalesi.
   
6. İçin **sürüm**seçin **V2** .
7. İçin **konumu**, aşağı açılan listeden ADF oluşturma için desteklenen konumlardan birini seçin.
8. **Panoya sabitle**’yi seçin.     
9. **Oluştur**’a tıklayın.
10. Azure panosunda durumuna sahip aşağıdaki kutucuğu görürsünüz: **Data Factory dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
   
11. Oluşturma işlemi tamamlandıktan sonra aşağıda gösterildiği gibi ADF sayfasında görebilirsiniz.
   
    ![Data factory giriş sayfası](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
   
12. Tıklayın **yazar ve İzleyici** ADF UI/uygulaması ayrı bir sekmede başlatmak için.

### <a name="create-your-pipelines"></a>İşlem hatlarınızı oluşturmak

1. İçinde **başlayalım** sayfasında **işlem hattı Oluştur**. 

   ![Başlarken sayfası](./media/how-to-schedule-azure-ssis-integration-runtime/get-started-page.png)
   
2. İçinde **etkinlikleri** araç genişletin **genel** menüsünde ve sürükle ve bırak bir **Web** etkinliğini işlem hattı tasarımcısının yüzeyine sürükleyin. İçinde **genel** sekmesini etkinlik Özellikler penceresi, etkinlik adını **startMyIR**. Geçiş **ayarları** sekmesini tıklatın ve aşağıdaki eylemleri gerçekleştirin.

    1. İçin **URL**, REST API'si için Azure-SSIS IR başlatan aşağıdaki URL'yi girin değiştirerek `{subscriptionId}`, `{resourceGroupName}`, `{factoryName}`, ve `{integrationRuntimeName}` , IR için gerçek değerlerle: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DataFactory/factories/{factoryName}/integrationRuntimes/{integrationRuntimeName}/start?api-version=2018-06-01` Alternatif olarak, ayrıca kopyalayın & kaynak kimliği, izleme sayfasından IR ADF UI/yukarıdaki URL'yi aşağıdaki bölümü değiştirmek için uygulamamı yapıştırın: `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DataFactory/factories/{factoryName}/integrationRuntimes/{integrationRuntimeName}`
    
       ![ADF SSIS IR kaynak kimliği](./media/how-to-schedule-azure-ssis-integration-runtime/adf-ssis-ir-resource-id.png)
  
    2. İçin **yöntemi**seçin **POST**. 
    3. İçin **gövdesi**, girin `{"message":"Start my IR"}`. 
    4. İçin **kimlik doğrulaması**seçin **MSI** , ADF için yönetilen kimlik kullanmak için bkz: [yönetilen kimliği için Data Factory](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) makale daha fazla bilgi için.
    5. İçin **kaynak**, girin `https://management.azure.com/`.
    
       ![ADF Web etkinlik zamanlaması SSIS IR](./media/how-to-schedule-azure-ssis-integration-runtime/adf-web-activity-schedule-ssis-ir.png)
  
3. Kopyalama etkinliği adı değiştirme, ikinci bir tane oluşturmak için ilk işlem hattı **stopMyIR** ve aşağıdaki özellikleri değiştirme.

    1. İçin **URL**, REST API'si için Azure-SSIS IR, durdurur aşağıdaki URL'yi girin değiştirerek `{subscriptionId}`, `{resourceGroupName}`, `{factoryName}`, ve `{integrationRuntimeName}` , IR için gerçek değerlerle: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DataFactory/factories/{factoryName}/integrationRuntimes/{integrationRuntimeName}/stop?api-version=2018-06-01`
    
    2. İçin **gövdesi**, girin `{"message":"Stop my IR"}`. 

4. Üçüncü bir işlem hattı oluşturmak, sürükle & bırak bir **SSIS paketi yürütme** etkinliğinden **etkinlikleri** toolbox işlem hattı tasarımcısının yüzeyine bırakın ve talimatları yapılandırmanız [ SSIS paketi yürütme etkinliği kullanarak ADF içinde bir SSIS paketi çağırma](how-to-invoke-ssis-package-ssis-activity.md) makalesi.  Alternatif olarak, bir **saklı yordam** etkinliği bunun yerine ve talimatları yapılandırın [ADF içinde depolanan yordam etkinliği kullanarak bir SSIS paketi çağırma](how-to-invoke-ssis-package-stored-procedure-activity.md) makalesi.  Ardından, benzer şekilde bu Web etkinlikler ilk/saniye işlem hatları, IR Başlat/Durdur iki Web etkinliği arasında SSIS paket/saklı yordamı Yürüt etkinliği zincirleyebilir, yani.

   ![İsteğe bağlı olarak SSIS IR ADF Web etkinliği](./media/how-to-schedule-azure-ssis-integration-runtime/adf-web-activity-on-demand-ssis-ir.png)

5. Yönetilen kimlik atamak için ADF bir **katkıda bulunan** rol kendisi, işlem hatları Web etkinliği içinde sağlanan Azure-SSIS Ir'ler Başlat/Durdur için REST API çağırabilmek için.  Azure portalında ADF sayfasında, tıklayın **erişim denetimi (IAM)**, tıklayın **+ rol ataması Ekle**ve ardından **rol ataması Ekle** dikey penceresinde aşağıdaki eylemleri gerçekleştirin.

    1. İçin **rol**seçin **katkıda bulunan**. 
    2. İçin **erişim Ata**seçin **Azure AD kullanıcı, Grup veya hizmet sorumlusu**. 
    3. İçin **seçin**için ADF adınızı arayın ve seçin. 
    4. **Kaydet**’e tıklayın.
    
   ![ADF yönetilen kimlik rol ataması](./media/how-to-schedule-azure-ssis-integration-runtime/adf-managed-identity-role-assignment.png)

6. ADF doğrulamak ve tüm ayarları tıklayarak işlem hattı **tüm doğrula / Validate** factory/işlem hattı araç. Kapat **Fabrika/işlem hattı doğrulama çıktı** tıklayarak **>>** düğmesi.  

   ![İşlem hattını doğrulama](./media/how-to-schedule-azure-ssis-integration-runtime/validate-pipeline.png)

### <a name="test-run-your-pipelines"></a>Test çalıştırması, işlem hatları

1. Seçin **Test çalıştırması** görmek ve her işlem hattı için araç çubuğunda **çıkış** alt bölmesinden penceresi. 

   ![Test çalıştırması](./media/how-to-schedule-azure-ssis-integration-runtime/test-run-output.png)
    
2. Üçüncü bir işlem hattı test etmek için SQL Server Management Studio (SSMS) başlatın. İçinde **sunucuya Bağlan** penceresinde aşağıdaki eylemleri gerçekleştirin. 

    1. İçin **sunucu adı**, girin  **&lt;Azure SQL veritabanı sunucu adınız&gt;. database.windows.net**.
    2. Seçin **Seçenekleri >>**.
    3. İçin **veritabanına bağlan**seçin **SSISDB**.
    4. **Bağlan**’ı seçin. 
    5. Genişletin **tümleştirme hizmetleri katalogları** -> **SSISDB** klasörünüze -> -> **projeleri** -> uygulamanızın SSIS Proje -> **paketleri** . 
    6. Çalıştırmak ve seçmek için belirtilen SSIS paketi sağ **raporları** -> **standart raporlar** -> **tüm yürütmeleri**. 
    7. Bunu çalıştırıldığını doğrulayın. 

   ![SSIS paketi Çalıştır doğrulayın](./media/how-to-schedule-azure-ssis-integration-runtime/verify-ssis-package-run.png)

### <a name="schedule-your-pipelines"></a>İşlem hatlarınızı zamanlama

İşlem hatlarınızı beklediğiniz gibi çalışır, belirtilen bir uyumla çalıştırılacak Tetikleyiciler oluşturabilirsiniz. Tetikleyici işlem hattı ile ilişkilendirme hakkında daha fazla ayrıntı için bkz. [işlem hattını bir zamanlamaya göre tetikleme](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule) makalesi.

1. İşlem hattı araç çubuğunda **tetikleyici** seçip **yeni/Düzenle**. 

   ![Tetikleyici yeni/Düzenle->](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-new-menu.png)

2. İçinde **tetikleyici Ekle** bölmesinde **+ yeni**.

   ![Tetikleyiciler - Yeni Ekle](./media/how-to-schedule-azure-ssis-integration-runtime/add-triggers-new.png)

3. İçinde **Yeni Tetikleyici** bölmesinde, aşağıdaki eylemleri gerçekleştirin: 

    1. İçin **adı**, tetikleyici için bir ad girin. Aşağıdaki örnekte, **günlük olarak çalıştırılması** Tetikleyici adı. 
    2. İçin **türü**seçin **zamanlama**. 
    3. İçin **başlangıç tarihi (UTC)**, başlangıç tarihini girin ve UTC saat. 
    4. İçin **yinelenme**, tetikleyici için bir temposu girin. Aşağıdaki örnekte olduğu **günlük** sonra. 
    5. İçin **son**seçin **Hayır son** veya bir bitiş tarihi ve saati seçtikten sonra girin **tarih**. 
    6. Seçin **etkinleştirildi** hemen tüm ADF ayarları yayımladıktan sonra tetikleyici etkinleştirmek için. 
    7. **İleri**’yi seçin.

   ![Tetikleyici yeni/Düzenle->](./media/how-to-schedule-azure-ssis-integration-runtime/new-trigger-window.png)
    
4. İçinde **tetikleyici çalıştırma parametreleri** sayfasında, herhangi bir uyarıyı gözden geçirin ve seçin **son**. 
5. Tüm ADF ayarları seçerek yayımlama **tümünü Yayımla** Fabrika araç. 

   ![Tümünü Yayımla](./media/how-to-schedule-azure-ssis-integration-runtime/publish-all.png)

### <a name="monitor-your-pipelines-and-triggers-in-azure-portal"></a>İşlem hatları ve tetikleyiciler Azure portalında izleme

1. Tetikleyici çalıştırmalarını izleme ve işlem hattı çalıştırmaları için kullandığınız **İzleyici** sol tarafındaki ADF UI/uygulama sekmesinde. Ayrıntılı adımlar için bkz. [işlem hattını izleme](quickstart-create-data-factory-portal.md#monitor-the-pipeline) makalesi.

   ![İşlem hattı çalıştırmaları](./media/how-to-schedule-azure-ssis-integration-runtime/pipeline-runs.png)

2. Çalıştıran bir işlem hattı çalıştırmasıyla ilişkili etkinlik çalıştırmalarını görüntülemek için ilk bağlantıyı seçin (**etkinlik çalıştırmalarını görüntüle**) içinde **eylemleri** sütun. Üçüncü işlem hattı için üç görürsünüz etkinlik çalıştığında, işlem hattının (durdurmak, IR için paketinizi yürütün ve Web etkinliği için saklı yordam etkinliği, IR başlatmak için Web etkinliği) Zincirli her etkinliği için bir tane. İşlem hattı görüntülemek için yeniden çalıştığından, seçin **işlem hatları** üstündeki bağlantısı.

   ![Etkinlik çalıştırmaları](./media/how-to-schedule-azure-ssis-integration-runtime/activity-runs.png)

3. Tetikleyici çalıştırmalarını görüntülemek için seçin **tetikleyici çalıştırmaları** altındaki aşağı açılan listeden **işlem hattı çalıştırmalarını** en üstünde. 

   ![Tetikleyici çalıştırmaları](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-runs.png)

### <a name="monitor-your-pipelines-and-triggers-with-powershell"></a>İşlem hatları ve tetikleyiciler PowerShell ile izleme

İşlem hatları ve tetikleyiciler izlemek için aşağıdaki örneklerdeki gibi betikleri kullanın.

1. Bir işlem hattı çalıştırma durumunu alın.

   ```powershell
   Get-AzDataFactoryV2PipelineRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $myPipelineRun
   ```

2. Bir tetikleyici hakkında bilgi alın.

   ```powershell
   Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name  "myTrigger"
   ```

3. Bir tetikleyici çalıştırma durumunu alın.

   ```powershell
   Get-AzDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "myTrigger" -TriggerRunStartedAfter "2018-07-15" -TriggerRunStartedBefore "2018-07-16"
   ```

## <a name="create-and-schedule-azure-automation-runbook-that-startsstops-azure-ssis-ir"></a>Oluşturma ve Azure-SSIS IR başlatır/durdurur, Azure Otomasyonu runbook zamanlama

Bu bölümde, PowerShell Betiği yürüten bir Azure Otomasyonu runbook'u oluşturmak bir zamanlamaya göre Azure-SSIS IR başlatma/durdurma öğreneceksiniz.  Önce/sonra IR öncesi/post-işleme için başlatma/durdurma ek betikler çalıştırmak istediğinizde bu kullanışlıdır.

### <a name="create-your-azure-automation-account"></a>Azure Otomasyonu hesabınızı oluşturun

Azure Otomasyonu hesabı zaten yoksa bu adımdaki yönergeleri izleyerek bir tane oluşturun. Ayrıntılı adımlar için bkz. [bir Azure Otomasyonu hesabını oluşturmak](../automation/automation-quickstart-create-account.md) makalesi. Bu adım bir parçası olarak, oluşturduğunuz bir **Azure Farklı Çalıştır** hesabı (hizmet sorumlusu Azure Active Directory'nizde) ve atayın bir **katkıda bulunan** Azure aboneliğinizdeki rol. Azure SSIS IR ile ADF içeren aynı abonelikte olduğundan emin olun Azure Otomasyonu, Azure Resource Manager'a kimliğini doğrulamak ve kaynaklarınız üzerinde çalışması için bu hesabı kullanır. 

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. ADF UI/uygulaması şu anda yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenir.
2. [Azure portalda](https://portal.azure.com/) oturum açın.    
3. Seçin **yeni** sol menüsünde **izleme + Yönetim**seçip **Otomasyon**. 

   ![Yeni İzleme -> + Yönetimi Otomasyonu ->](./media/how-to-schedule-azure-ssis-integration-runtime/new-automation.png)
    
2. İçinde **Otomasyon hesabı Ekle** bölmesinde, aşağıdaki eylemleri gerçekleştirin.

    1. İçin **adı**, Azure Automation hesabınız için bir ad girin. 
    2. İçin **abonelik**, Azure-SSIS IR ile ADF olan aboneliği seçin 
    3. İçin **kaynak grubu**seçin **Yeni Oluştur** yeni bir kaynak grubu oluşturmak için veya **var olanı kullan** var olanı seçin. 
    4. İçin **konumu**, Azure Automation hesabınız için bir konum seçin. 
    5. Onayla **oluşturma Azure farklı çalıştır hesabı** olarak **Evet**. Bir hizmet sorumlusu, Azure Active Directory'de oluşturulan ve atanmış bir **katkıda bulunan** Azure aboneliğinizdeki rol.
    6. Seçin **panoya Sabitle** kalıcı olarak Azure panosunda görüntülemek için. 
    7. **Oluştur**’u seçin. 

   ![Yeni İzleme -> + Yönetimi Otomasyonu ->](./media/how-to-schedule-azure-ssis-integration-runtime/add-automation-account-window.png)
   
3. Azure Otomasyonu hesabınızı Azure panosunu ve bildirimleri dağıtım durumunu görürsünüz. 
    
   ![Otomasyon dağıtma](./media/how-to-schedule-azure-ssis-integration-runtime/deploying-automation.png) 
    
4. Başarıyla oluşturulduktan sonra Azure Otomasyon hesabınızın giriş sayfasını görürsünüz. 

   ![Otomasyon giriş sayfası](./media/how-to-schedule-azure-ssis-integration-runtime/automation-home-page.png)

### <a name="import-adf-modules"></a>ADF modüllerini içeri aktarma

1. Seçin **modülleri** içinde **paylaşılan kaynakları** sahip olup olmadığını doğrulayın ve bölümünde, sol taraftaki menüde **AzureRM.DataFactoryV2**  +   **AzureRM.Profile** modüller listesinde.

   ![Gerekli modülleri doğrulayın](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image1.png)

2.  Yoksa **AzureRM.DataFactoryV2**PowerShell Galerisi için Git [AzureRM.DataFactoryV2 Modülü](https://www.powershellgallery.com/packages/AzureRM.DataFactoryV2/)seçin **Azure Otomasyonu Dağıt**, Azure'ı seçin Otomasyon hesabı ve ardından **Tamam**. Görüntülemek için geri dönün **modülleri** içinde **paylaşılan kaynakları** görene kadar bekleyin ve bölüm sol taraftaki menüde **durumu** , **AzureRM.DataFactoryV2** Modül değiştirildi **kullanılabilir**.

    ![Data Factory modülü doğrulayın](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image2.png)

3.  Yoksa **AzureRM.Profile**PowerShell Galerisi için Git [AzureRM.Profile modülündeki](https://www.powershellgallery.com/packages/AzureRM.profile/)seçin **Azure Otomasyonu Dağıt**, Azure Automation'ınızı seçin Hesap ve ardından **Tamam**. Görüntülemek için geri dönün **modülleri** içinde **paylaşılan kaynakları** görene kadar bekleyin ve bölüm sol taraftaki menüde **durumu** , **AzureRM.Profile** Modül değiştirildi **kullanılabilir**.

    ![Profil modülü doğrulayın](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image3.png)

### <a name="create-your-powershell-runbook"></a>PowerShell runbook'unuzu oluşturma

Aşağıdaki bölümde, bir PowerShell runbook'u oluşturmak için adımları sağlar. İle runbook veya ilişkili komut başlatır/Azure-SSIS IR için belirttiğiniz komut göre durdurur **işlemi** parametresi. Bu bölümde, bir runbook oluşturmak için tam ayrıntılarını sağlamaz. Daha fazla bilgi için [bir runbook oluşturmak](../automation/automation-quickstart-create-runbook.md) makalesi.

1. Geçiş **runbook'ları** sekmenize **+ runbook Ekle** araç çubuğundan. 

   ![Runbook düğmesi ekleme](./media/how-to-schedule-azure-ssis-integration-runtime/runbooks-window.png)
   
2. Seçin **yeni bir runbook oluşturmak** ve aşağıdaki eylemleri gerçekleştirin: 

    1. İçin **adı**, girin **StartStopAzureSsisRuntime**.
    2. İçin **Runbook türü**seçin **PowerShell**.
    3. **Oluştur**’u seçin.
    
   ![Runbook düğmesi ekleme](./media/how-to-schedule-azure-ssis-integration-runtime/add-runbook-window.png)
   
3. & Kopyala, runbook komut penceresine aşağıdaki PowerShell betiğini yapıştırın. Kaydedin ve ardından, runbook kullanarak yayımlama **Kaydet** ve **Yayımla** araç çubuğundaki düğmeler. 

    ```powershell
    Param
    (
          [Parameter (Mandatory= $true)]
          [String] $ResourceGroupName,
    
          [Parameter (Mandatory= $true)]
          [String] $DataFactoryName,
    
          [Parameter (Mandatory= $true)]
          [String] $AzureSSISName,
    
          [Parameter (Mandatory= $true)]
          [String] $Operation
    )
    
    $connectionName = "AzureRunAsConnection"
    try
    {
        # Get the connection "AzureRunAsConnection "
        $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         
    
        "Logging in to Azure..."
        Connect-AzAccount `
            -ServicePrincipal `
            -TenantId $servicePrincipalConnection.TenantId `
            -ApplicationId $servicePrincipalConnection.ApplicationId `
            -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint 
    }
    catch {
        if (!$servicePrincipalConnection)
        {
            $ErrorMessage = "Connection $connectionName not found."
            throw $ErrorMessage
        } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
        }
    }
    
    if($Operation -eq "START" -or $operation -eq "start")
    {
        "##### Starting #####"
        Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $AzureSSISName -Force
    }
    elseif($Operation -eq "STOP" -or $operation -eq "stop")
    {
        "##### Stopping #####"
        Stop-AzDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    }  
    "##### Completed #####"    
    ```

   ![PowerShell runbook'unu Düzenle](./media/how-to-schedule-azure-ssis-integration-runtime/edit-powershell-runbook.png)
    
4. Runbook'unuzda seçerek test **Başlat** araç çubuğunda. 

   ![Runbook düğmesini Başlat](./media/how-to-schedule-azure-ssis-integration-runtime/start-runbook-button.png)
    
5. İçinde **Runbook'u Başlat** bölmesinde, aşağıdaki eylemleri gerçekleştirin: 

    1. İçin **kaynak grubu adı**, Azure-SSIS IR ile ADF olan kaynak grubu adını girin 
    2. İçin **DATA FACTORY adı**, Azure-SSIS IR ile ADF adını girin 
    3. İçin **AZURESSISNAME**, Azure-SSIS IR adı girin 
    4. İçin **işlemi**, girin **Başlat**. 
    5. **Tamam**’ı seçin.  

   ![Runbook penceresi başlatın](./media/how-to-schedule-azure-ssis-integration-runtime/start-runbook-window.png)
   
6. İş penceresinde **çıkış** Döşe. Çıkış penceresinde öğesinin ileti beklemesi **### tamamlandı ###** gördükten sonra **### başlangıç ###**. Azure-SSIS IR başlangıç yaklaşık 20 dakika sürer. Kapat **iş** penceresi ve geri alma için **Runbook** penceresi.

   ![Azure SSIS IR - başlatıldı](./media/how-to-schedule-azure-ssis-integration-runtime/start-completed.png)
    
7. Kullanarak önceki iki adımı yineleyin **Durdur** değeri olarak **işlemi**. Runbook uygulamanızı tekrar seçerek başlayın **Başlat** araç çubuğunda. Kaynak grubu, ADF ve Azure-SSIS IR girin adları. İçin **işlemi**, girin **Durdur**. Çıkış penceresinde öğesinin ileti beklemesi **### tamamlandı ###** gördükten sonra **### durdurma ###**. Azure-SSIS IR'yi durdurma başlangıç olarak gibi uzun sürmez. Kapat **iş** penceresi ve geri alma için **Runbook** penceresi.

## <a name="create-schedules-for-your-runbook-to-startstop-azure-ssis-ir"></a>Azure-SSIS IR Başlat/Durdur için runbook'unuzda için zamanlamaları oluşturma

Önceki bölümde başlayın veya Azure-SSIS IR'yi durdurma, Azure Otomasyonu runbook oluşturdunuz. Bu bölümde, iki zamanlamaları, runbook için oluşturacaksınız. İlk zamanlama yapılandırırken belirttiğiniz **Başlat** için **işlemi**. Benzer şekilde, ikinci yapılandırırken belirttiğiniz **Durdur** için **işlemi**. Tabloları oluşturmak ayrıntılı adımlar için bkz. [bir zamanlama oluşturmak](../automation/shared-resources/schedules.md#creating-a-schedule) makalesi.

1. İçinde **Runbook** penceresinde **zamanlamaları**seçip **+ zamanlama Ekle** araç. 

   ![Azure SSIS IR - başlatıldı](./media/how-to-schedule-azure-ssis-integration-runtime/add-schedules-button.png)
   
2. İçinde **zamanlamayı Runbook** bölmesinde, aşağıdaki eylemleri gerçekleştirin: 

    1. Seçin **bir zamanlamayı runbook'a bağlamak**. 
    2. Seçin **yeni bir zamanlama oluşturmak**.
    3. İçinde **yeni zamanlama** bölmesinde girin **IR Başlat günlük** için **adı**. 
    4. İçin **başlar**, birkaç dakika geçecek geçerli saati bir saat girin. 
    5. İçin **yinelenme**seçin **yinelenen**. 
    6. İçin **Yinele her**, girin **1** seçip **gün**. 
    7. **Oluştur**’u seçin. 

   ![Azure SSIS IR başlangıç zamanlama](./media/how-to-schedule-azure-ssis-integration-runtime/new-schedule-start.png)
    
3. Geçiş **parametreler ve çalıştırma ayarları** sekmesi. Kaynak grubu, ADF ve Azure-SSIS IR belirleyin adı. İçin **işlemi**, girin **Başlat** seçip **Tamam**. Seçin **Tamam** zamanlamayı yeniden görmek için **zamanlamaları** runbook'unuzu sayfası. 

   ![Azure SSIS IR başlamanızı zamanlama](./media/how-to-schedule-azure-ssis-integration-runtime/start-schedule.png)
    
4. Adlı bir zamanlama oluşturmak için önceki iki adımı yineleyin **IR'yi durdurma günlük**. En az 30 için belirttiğiniz dakika süre geçtikten sonra bir saat girin **IR Başlat günlük** zamanlama. İçin **işlemi**, girin **Durdur** seçip **Tamam**. Seçin **Tamam** zamanlamayı yeniden görmek için **zamanlamaları** runbook'unuzu sayfası. 

5. İçinde **Runbook** penceresinde **işleri** sol menüsünde. Belirtilen zamanlamalarınızı tarafından oluşturulan işleri görmelisiniz süreleri ve bunların durumlarını. Çıktısını gibi iş ayrıntılarını görebilirsiniz, ne, sonra gördüğünüz benzer, runbook test. 

   ![Azure SSIS IR başlamanızı zamanlama](./media/how-to-schedule-azure-ssis-integration-runtime/schedule-jobs.png)
    
6. İşlemi tamamladıktan sonra testi, zamanlamalarınızı düzenleme tarafından devre dışı bırakın. Seçin **zamanlamaları** sol menüsünde **Başlat IR günlük/durdurma IR günlük**seçip **Hayır** için **etkin**. 

## <a name="next-steps"></a>Sonraki adımlar
İçin şu blog yayınına bakın:
-   [Modernleştirin ve ETL/ELT iş akışlarınızı SSIS etkinliklerle ADF işlem hatlarını genişletin](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/)

SSIS belgelerinde aşağıdaki makalelere bakın: 

- [Azure’da bir SSIS paketi dağıtma, çalıştırma ve izleme](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial)   
- [Azure’da SSIS kataloğuna bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Azure’da paket yürütmeyi zamanlama](/sql/integration-services/lift-shift/ssis-azure-schedule-packages)
- [Windows kimlik doğrulaması ile şirket içi veri kaynaklarına bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth)
