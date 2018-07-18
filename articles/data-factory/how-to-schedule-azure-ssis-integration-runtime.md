---
title: Azure SSIS tümleştirme çalışma zamanı zamanlama | Microsoft Docs
description: Bu makalede, Azure Otomasyonu ve Data Factory kullanarak Azure SSIS tümleştirme çalışma zamanını bir durdurmayla ve zamanlayın açıklar.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 07/16/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: f83715d2a382db271686210d9df285c255c09216
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39114003"
---
# <a name="how-to-start-and-stop-the-azure-ssis-integration-runtime-on-a-schedule"></a>Nasıl bir zamanlamaya göre Azure SSIS tümleştirme çalışma zamanını durdurmak ve başlatmak
Bu makalede, Azure Otomasyonu ve Azure Data Factory kullanarak bir Azure SSIS Integration runtime (IR) durdurmayla ve zamanlayın açıklar. Çalıştıran (SQL Server Integration Services) Azure SSIS tümleştirme çalışma zamanı (IR) kendisiyle ilişkilendirilmiş bir maliyeti vardır. Bu nedenle, genellikle yalnızca Azure'da SSIS paketleri çalıştırma ve onu ihtiyacınız kalmadığında IR durdurmak, ihtiyacınız olduğunda IR çalıştırmak istersiniz. Data Factory kullanıcı Arabirimi veya Azure PowerShell ile kullanabileceğiniz [el ile başlatın veya Azure SSIS IR'yi durdurma](manage-azure-ssis-integration-runtime.md)).

Örneğin, Azure Otomasyonu PowerShell runbook'unda için Web kancaları ile Web etkinliği oluşturursunuz ve bunlar arasında bir SSIS paketi yürütme etkinliği zincirleyebilir, yani. Web etkinliği başlatabilir ve Azure-SSIS IR önce zamanında ve paketinizi çalıştıktan sonra durdurabilirsiniz. SSIS paketi yürütme etkinliği hakkında daha fazla bilgi için bkz. [SSIS etkinliğini kullanarak Azure Data Factory'de bir SSIS paketi çalıştırmak](how-to-invoke-ssis-package-ssis-activity.md).

## <a name="overview-of-the-steps"></a>Adımlara genel bakış

Bu makalede açıklanan yüksek düzey adımlar şunlardır:

1. **Oluşturma ve Azure Otomasyonu runbook'u sınayın.** Bu adımda, başlatıldığında veya durdurulduğunda Azure SSIS IR betiği ile bir PowerShell runbook'u oluşturma Daha sonra hem başlatma ve durdurma senaryolarda runbook'u test etme ve IR başlatıldığında veya durdurulduğunda onaylayın. 
2. **Runbook için iki zamanlamaları oluşturun.** İlk zamanlama için runbook işlemi ile başlangıç yapılandırın. İkinci zamanlamasını, runbook ile durdurma işlemi yapılandırın. Her iki zamanlamalar için runbook'un çalıştığı temposu belirtin. Örneğin, 8'de her gün ve 23: 00 gündelik çalıştırmak için ikinci bir çalıştırma Birincisi zamanlama isteyebilirsiniz. İlk runbook çalıştığında, Azure SSIS IR başlatır. İkinci runbook çalıştığında, Azure SSIS IR durdurur. 
3. **Runbook için iki Web kancaları oluşturma**, bir başlatma işlemi diğeri durdurma işlemi. Web etkinliği bir Data Factory işlem hattında yapılandırırken bu Web kancaları URL'lerini kullanın. 
4. **Data Factory işlem hattı oluşturma**. Oluşturacağınız işlem hattının üç etkinliklerden oluşur. İlk **Web** etkinliği, Azure SSIS IR başlatmak için ilk Web kancası çağırır **Saklı yordam** etkinlik, SSIS paketi çalıştıran bir SQL betiği çalıştırır. İkinci **Web** etkinliği, Azure SSIS IR durdurur Saklı yordam etkinliği kullanarak bir SSIS paketi bir Data Factory işlem hattından çağırma hakkında daha fazla bilgi için bkz. [bir SSIS paketi çağırma](how-to-invoke-ssis-package-stored-procedure-activity.md). Ardından, işlem hattının çalıştırılmasına belirttiğiniz tempoda zamanlamak için bir zamanlama tetikleyicisi oluşturma.

## <a name="prerequisites"></a>Önkoşullar
Bir Azure SSIS tümleştirme çalışma zamanı zaten hazırlamadıysanız konusundaki yönergeleri izleyerek sağlama [öğretici](tutorial-create-azure-ssis-runtime-portal.md). 

## <a name="create-and-test-an-azure-automation-runbook"></a>Oluşturma ve Azure Otomasyonu runbook'u test etme
Bu bölümde, aşağıdaki adımları gerçekleştirin: 

1. Azure Otomasyonu hesabı oluşturun.
2. PowerShell runbook'u Azure Otomasyon hesabı oluşturun. Bu runbook'la ilişkilendirilmiş PowerShell betiğini başlatır veya bir Azure SSIS IR işlem parametresi için belirttiğiniz komut göre durdurur. 
3. Her iki başlangıçta runbook'u test etme ve çalışır durumda olduğunu onaylamak için senaryoları durdurun. 

### <a name="create-an-azure-automation-account"></a>Azure Otomasyonu hesabı oluşturma
Azure Otomasyonu hesabı yoksa, bu adımdaki yönergeleri izleyerek bir tane oluşturun. Ayrıntılı adımlar için bkz. [bir Azure Otomasyonu hesabını oluşturmak](../automation/automation-quickstart-create-account.md). Bu adım bir parçası olarak, oluşturduğunuz bir **Azure Farklı Çalıştır** hesabı (hizmet sorumlusu Azure Active Directory'nizde) ve eklemeniz **katkıda bulunan** Azure aboneliğinizin rol. Azure SSIS IR olan data factory içeren aboneliği aynı olduğundan emin olun Azure Otomasyonu, Azure Resource Manager'a kimliğini doğrulamak ve kaynaklarınız üzerinde çalışması için bu hesabı kullanır. 

1. **Microsoft Edge** veya **Google Chrome** web tarayıcısını açın. Şu anda Data Factory kullanıcı arabirimi yalnızca Microsoft Edge ve Google Chrome web tarayıcılarında desteklenmektedir.
2. [Azure Portal](https://portal.azure.com/)’da oturum açın.    
3. Seçin **yeni** sol menüsünde **izleme + Yönetim**seçip **Otomasyon**. 

    ![Yeni İzleme -> + Yönetimi Otomasyonu ->](./media/how-to-schedule-azure-ssis-integration-runtime/new-automation.png)
2. İçinde **Otomasyon hesabı Ekle** penceresinde aşağıdaki adımları uygulayın: 

    1. Belirtin bir **adı** Otomasyon hesabı için. 
    2. Seçin **abonelik** data factory Azure SSIS IR ile olan 
    3. İçin **kaynak grubu**seçin **Yeni Oluştur** yeni bir kaynak grubu oluşturun veya seçin **var olanı kullan** mevcut bir kaynak grubunu seçin. 
    4. Seçin bir **konumu** Otomasyon hesabı için. 
    5. Onaylayın **Çalıştır hesabı oluştur** ayarlanır **Evet**. Azure Active Directory'de Hizmet sorumlusu oluşturulur. Eklendiği **katkıda bulunan** Azure aboneliğinizin rolü
    6. Seçin **panoya Sabitle** portal panosunda görebilmesi için. 
    7. **Oluştur**’u seçin. 

        ![Yeni İzleme -> + Yönetimi Otomasyonu ->](./media/how-to-schedule-azure-ssis-integration-runtime/add-automation-account-window.png)
3. Gördüğünüz **dağıtım durumu** Panoda ve bildirimleri. 
    
    ![Otomasyon dağıtma](./media/how-to-schedule-azure-ssis-integration-runtime/deploying-automation.png) 
4. Başarıyla oluşturulduktan sonra Otomasyon hesabının giriş sayfasını görürsünüz. 

    ![Otomasyon giriş sayfası](./media/how-to-schedule-azure-ssis-integration-runtime/automation-home-page.png)

### <a name="import-data-factory-modules"></a>Data Factory modüllerini içeri aktarma

1. Seçin **modülleri** içinde **paylaşılan kaynakları** bölümünde, sol taraftaki menüde ve yüklü olup olmadığını doğrulayın **AzureRM.Profile** ve **AzureRM.DataFactoryV2** modüller listesinde.

    ![Gerekli modülleri doğrulayın](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image1.png)

2.  PowerShell Galerisi için Git [AzureRM.DataFactoryV2 Modülü](https://www.powershellgallery.com/packages/AzureRM.DataFactoryV2/)seçin **Azure Otomasyonu Dağıt**Otomasyon hesabınızı seçin ve ardından **Tamam**. Görüntülemek için geri dönün **modülleri** içinde **paylaşılan kaynakları** bölümünde, sol taraftaki menüde ve görene kadar bekleyin **durumu** , **AzureRM.DataFactoryV2** modülü değişiklik **kullanılabilir**.

    ![Data Factory modülü doğrulayın](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image2.png)

3.  PowerShell Galerisi için Git [AzureRM.Profile modülündeki](https://www.powershellgallery.com/packages/AzureRM.profile/), tıklayarak **Azure Otomasyonu Dağıt**Otomasyon hesabınızı seçin ve ardından **Tamam**. Görüntülemek için geri dönün **modülleri** içinde **paylaşılan kaynakları** bölümünde, sol taraftaki menüde ve görene kadar bekleyin **durumu** , **AzureRM.Profile**modülü değişiklik **kullanılabilir**.

    ![Profil modülü doğrulayın](media/how-to-schedule-azure-ssis-integration-runtime/automation-fix-image3.png)

### <a name="create-a-powershell-runbook"></a>PowerShell runbook’u oluşturma
Aşağıdaki yordam, bir PowerShell runbook'u oluşturmak için adımları sağlar. Ya da bu runbook'la ilişkilendirilmiş betik başlatır/durdurur belirtmek için komut tabanlı bir Azure SSIS IR **işlemi** parametresi. Bu bölümde, bir runbook oluşturmak için tüm ayrıntıları sağlamaz. Daha fazla bilgi için [bir runbook oluşturmak](../automation/automation-quickstart-create-runbook.md) makalesi.

1. Geçiş **runbook'ları** sekmesine tıklayın ve **+ runbook Ekle** araç çubuğundan. 

    ![Runbook düğmesi ekleme](./media/how-to-schedule-azure-ssis-integration-runtime/runbooks-window.png)
2. Seçin **yeni bir runbook oluşturmak**ve aşağıdaki adımları gerçekleştirin: 

    1. İçin **adı**, türü **StartStopAzureSsisRuntime**.
    2. İçin **Runbook türü**seçin **PowerShell**.
    3. **Oluştur**’u seçin.
    
        ![Runbook düğmesi ekleme](./media/how-to-schedule-azure-ssis-integration-runtime/add-runbook-window.png)
3. Runbook komut penceresine aşağıdaki betiği kopyala/yapıştır. Kaydedin ve ardından kullanarak runbook yayımlama **Kaydet** ve **Yayımla** araç çubuğundaki düğmeler. 

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
        Connect-AzureRmAccount `
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
        Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name $AzureSSISName -Force
    }
    elseif($Operation -eq "STOP" -or $operation -eq "stop")
    {
        "##### Stopping #####"
        Stop-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $DataFactoryName -Name $AzureSSISName -ResourceGroupName $ResourceGroupName -Force
    }  
    "##### Completed #####"    
    ```

    ![PowerShell runbook'unu Düzenle](./media/how-to-schedule-azure-ssis-integration-runtime/edit-powershell-runbook.png)
5. Runbook'u test etme seçerek **Başlat** araç çubuğunda. 

    ![Runbook düğmesini Başlat](./media/how-to-schedule-azure-ssis-integration-runtime/start-runbook-button.png)
6. İçinde **Runbook'u Başlat** penceresinde aşağıdaki adımları gerçekleştirin: 

    1. İçin **kaynak grubu adı**, Azure SSIS IR olan data factory ile kaynak grubunun adını girin 
    2. İçin **DATA FACTORY adı**, Azure SSIS IR sahip veri fabrikasının adını girin 
    3. İçin **AZURESSISNAME**, Azure SSIS IR adı girin 
    4. İçin **işlemi**, girin **Başlat**. 
    5. **Tamam**’ı seçin.  

        ![Runbook penceresi başlatın](./media/how-to-schedule-azure-ssis-integration-runtime/start-runbook-window.png)
7. İş penceresinde **çıkış** Döşe. İş çıktı penceresinde iletisini görene kadar bekleyin **### tamamlandı ###** gördükten sonra **### başlangıç ###**. Azure SSIS IR başlangıç yaklaşık 20 dakika sürer. Kapat **iş** penceresinde ve geri almak **Runbook** penceresi.

    ![Azure SSIS IR - başlatıldı](./media/how-to-schedule-azure-ssis-integration-runtime/start-completed.png)
8.  Önceki iki adımı yineleyin ancak kullanarak **Durdur** değeri olarak **işlemi**. Runbook tekrar seçerek başlayın **Başlat** araç çubuğunda. Kaynak grubu adı, veri fabrikası adı ve Azure SSIS IR adı belirtin. İçin **işlemi**, girin **Durdur**. 

    İş çıktı penceresinde iletisini görene kadar bekleyin **### tamamlandı ###** gördükten sonra **### durdurma ###**. Azure SSIS IR'yi durdurma Azure SSIS IR başlangıç olarak uzun almaz Kapat **iş** penceresinde ve geri almak **Runbook** penceresi.

## <a name="create-schedules-for-the-runbook-to-startstop-the-azure-ssis-ir"></a>Azure SSIS IR Başlat/Durdur için runbook'u için zamanlama oluşturma
Önceki bölümde oluşturduğunuz bir Azure Otomasyonu runbook başlatın veya bir Azure SSIS IR'yi durdurma Bu bölümde, runbook iki zamanlamaları oluşturun. İlk zamanlama yapılandırırken, işlem parametresi için başlangıç belirtin. Benzer şekilde, ikinci zamanlamasını yapılandırırken durdurma işlemi için belirtin. Zamanlamalar oluşturmaya yönelik ayrıntılı adımlar için bkz. [bir zamanlama oluşturmak](../automation/automation-schedules.md#creating-a-schedule).

1. İçinde **Runbook** penceresinde **zamanlamaları**seçip **+ zamanlama Ekle** araç. 

    ![Azure SSIS IR - başlatıldı](./media/how-to-schedule-azure-ssis-integration-runtime/add-schedules-button.png)
2. İçinde **zamanlamayı Runbook** penceresinde aşağıdaki adımları gerçekleştirin: 

    1. Seçin **bir zamanlamayı runbook'a bağlamak**. 
    2. Seçin **yeni bir zamanlama oluşturmak**.
    3. İçinde **yeni zamanlama** penceresinde, tür **IR Başlat günlük** için **adı**. 
    4. İçinde **başlatan bölüm**, saat için geçerli süresini geçen birkaç dakika zaman belirtin. 
    5. İçin **yinelenme**seçin **yinelenen**. 
    6. İçinde **Yinele her** bölümünden **gün**. 
    7. **Oluştur**’u seçin. 

        ![Azure SSIS IR başlangıç zamanlama](./media/how-to-schedule-azure-ssis-integration-runtime/new-schedule-start.png)
3. Geçiş **parametreler ve çalıştırma ayarları** sekmesi. Kaynak grubu adı, veri fabrikası adı ve Azure SSIS IR adı belirtin. İçin **işlemi**, girin **Başlat**. **Tamam**’ı seçin. Seçin **Tamam** zamanlamayı yeniden görmek için **zamanlamaları** runbook'un sayfası. 

    ![Azure SSIS IR başlamanızı zamanlama](./media/how-to-schedule-azure-ssis-integration-runtime/start-schedule.png)
4. Adlı bir zamanlama oluşturmak için önceki iki adımı yineleyin **IR'yi durdurma günlük**. Bu kez, saat için belirtilen en az 30 dakika sonra belirtin **IR Başlat günlük** zamanlama. İçin **işlemi**, belirtin **Durdur**. 
5. İçinde **Runbook** penceresinde **işleri** sol menüsünde. Belirtilen zamanlama tarafından oluşturulan işleri görmelisiniz süreleri ve bunların durumlarını. Runbook'u Test ettiğinizde ne açtığınız için benzer çıktısını gibi iş ayrıntılarını görebilirsiniz. 

    ![Azure SSIS IR başlamanızı zamanlama](./media/how-to-schedule-azure-ssis-integration-runtime/schedule-jobs.png)
6. İşlemi tamamladıktan sonra testi, zamanlama seçme ve bunları düzenleme ile devre dışı bırak **Hayır** için **etkin**. Seçin **zamanlamaları** sol menüde **Başlat IR günlük/durdurma IR günlük**seçip **Hayır** için **etkin**. 

## <a name="create-webhooks-to-start-and-stop-the-azure-ssis-ir"></a>Azure SSIS IR durdurmak ve başlatmak Web kancaları oluşturma
Yönergeleri [Web kancası oluşturma](../automation/automation-webhooks.md#creating-a-webhook) runbook iki Web kancaları oluşturma. Birinci başlangıç işlemi olarak belirtmek ve ikincisi için durdurma işlemi belirtin. Her iki Web kancasının yere (gibi bir metin dosyası veya bir OneNote Not Defteri gibi) için URL'leri kaydedin. Web etkinliği, Data Factory işlem hattında yapılandırırken bu URL'leri kullanın. Aşağıdaki görüntüde Azure SSIS IR başlatan bir Web kancası oluşturma örneği gösterilir:

1. İçinde **Runbook** penceresinde **Web kancaları** seçin ve soldaki menüden **+ Web kancası Ekle** araç. 

    ![Web kancaları -> Web kancası Ekle](./media/how-to-schedule-azure-ssis-integration-runtime/add-web-hook-menu.png)
2. İçinde **Web kancası Ekle** penceresinde **yeni Web kancası oluşturma**, ve aşağıdaki eylemleri gerçekleştirin: 

    1. İçin **adı**, girin **StartAzureSsisIR**. 
    2. Onaylayın **etkin** ayarlanır **Evet**. 
    3. Kopyalama **URL** ve başka bir yere kaydedin. Bu adım önemlidir. URL daha sonra görmez. 
    4. **Tamam**’ı seçin. 

        ![Yeni Web kancası penceresi](./media/how-to-schedule-azure-ssis-integration-runtime/new-web-hook-window.png)
3. Geçiş **parametreler ve çalıştırma ayarları** sekmesi. Kaynak grubu adı, veri fabrikasının adını ve Azure SSIS IR adı belirtin. İçin **işlemi**, girin **Başlat**. **Tamam**’a tıklayın. Sonra, **Oluştur**’a tıklayın. 

    ![Web kancası - parametreler ve çalıştırma ayarları](./media/how-to-schedule-azure-ssis-integration-runtime/webhook-parameters.png)
4. Adlı başka bir Web kancası oluşturmak için önceki üç adımı tekrarlayın **StopAzureSsisIR**. URL'yi kopyalamak unutmayın. Parametreler ve çalıştırma ayarları belirtirken girin **Durdur** için **işlemi**. 

İçin iki URL olmalıdır **StartAzureSsisIR** Web kancası diğeri **StopAzureSsisIR** Web kancası. Bu URL'leri, Azure SSIS IR Başlat/Durdur için bir HTTP POST isteği gönderebilirsiniz 

## <a name="create-and-schedule-a-data-factory-pipeline-that-startsstops-the-ir"></a>Oluşturma ve zamanlama IR başlatır/durdurur, bir Data Factory işlem hattı
Bu bölümde bir Web etkinliği önceki bölümde oluşturduğunuz Web kancaları çağırmak için nasıl kullanılacağını gösterir.

Oluşturacağınız işlem hattının üç etkinliklerden oluşur. 

1. İlk **Web** etkinliği, Azure SSIS IR başlatmak için ilk Web kancası çağırır 
2. **SSIS paketi yürütme** etkinlik veya **saklı yordam** etkinlik çalıştırmalarını SSIS paketi.
3. İkinci **Web** etkinliği, Azure SSIS IR durdurmak için Web kancası çağırır 

Oluşturma ve işlem hattını test sonra zamanlama tetikleyicisi oluşturma ve işlem hattı çalıştırmasıyla ilişkilendirme. Zamanlama tetikleyicisi, işlem hattı için bir zamanlama tanımlar. Günlük 23: 00 çalıştırmak için zamanlanan bir tetikleyici oluşturduğunuz düşünün. Tetikleyici işlem hattını 11 saat her gün çalıştırır. İşlem hattı Azure SSIS IR başlatır, SSIS paketi yürütür ve ardından Azure SSIS IR durdurur 

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.    
2. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/tutorial-create-azure-ssis-runtime-portal/new-data-factory-menu.png)
3. **Yeni veri fabrikası** sayfasında **ad** için **MyAzureSsisDataFactory** adını girin. 
      
     ![Yeni veri fabrikası sayfası](./media/tutorial-create-azure-ssis-runtime-portal/new-azure-data-factory.png)
 
   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Aşağıdaki hatayı alırsanız veri fabrikasının adını değiştirin (örneğin adınızMyAzureSsisDataFactory) ve yeniden oluşturmayı deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın.
  
       `Data factory name �MyAzureSsisDataFactory� is not available`
3. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
      - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
      - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
      Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
4. **Sürüm** için **V2**'yi seçin.
5. Data factory için **konum** seçin. Listede yalnızca veri fabrikası oluşturma için desteklenen konumlar gösterilir.
6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’a tıklayın.
8. Panoda şu kutucuğu ve üzerinde şu durumu görürsünüz: **Veri fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media/tutorial-create-azure-ssis-runtime-portal/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, resimde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
   ![Data factory giriş sayfası](./media/tutorial-create-azure-ssis-runtime-portal/data-factory-home-page.png)
10. Azure Data Factory Kullanıcı Arabirimini (UI) ayrı bir sekmede açmak için **Geliştir ve İzle**’ye tıklayın.

### <a name="create-a-pipeline"></a>İşlem hattı oluşturma

1. İçinde **başlama** sayfasında **işlem hattı Oluştur**. 

   ![Başlarken sayfası](./media/how-to-schedule-azure-ssis-integration-runtime/get-started-page.png)
2. İçinde **etkinlikleri** araç genişletin **genel**, sürükle-bırak **Web** etkinliğini işlem hattı tasarımcısının yüzeyine sürükleyin. İçinde **genel** sekmesinde **özellikleri** penceresini değiştirmek için etkinliğin adını **StartIR**.

   ![İlk Web etkinliği - Genel sekmesi](./media/how-to-schedule-azure-ssis-integration-runtime/first-web-activity-general-tab.png)
3. Geçiş **ayarları** sekmesinde **özellikleri** penceresinde ve aşağıdaki eylemleri gerçekleştirin: 

    1. İçin **URL**, Azure SSIS IR ile başlar Web kancası URL'sini yapıştırın 
    2. İçin **yöntemi**seçin **POST**. 
    3. İçin **gövdesi**, girin `{"message":"hello world"}`. 
   
        ![İlk Web etkinliği - Ayarlar sekmesi](./media/how-to-schedule-azure-ssis-integration-runtime/first-web-activity-settnigs-tab.png)

4. SSIS paketi yürütme etkinliği veya saklı yordam etkinliği sürükleyip **genel** bölümünü **etkinlikleri** araç kutusu. Etkinlik adı ayarlayın **RunSSISPackage**. 

5. SSIS paketi yürütme etkinliği seçin,'ndaki yönergeleri izleyin. [SSIS etkinliğini kullanarak Azure Data Factory'de bir SSIS paketi çalıştırmak](how-to-invoke-ssis-package-ssis-activity.md) etkinliği oluşturmayı tamamlamak için.  Azure-SSIS IR kullanılabilirliği için başlaması 30 dakika sürer. bu yana beklemeniz sık yeniden deneme girişimleri yeterli sayıda belirttiğinizden emin olun. 

    ![Yeniden deneme ayarları](media/how-to-schedule-azure-ssis-integration-runtime/retry-settings.png)

6. Saklı yordam etkinliği seçin,'ndaki yönergeleri izleyin. [saklı yordam etkinliği kullanarak Azure Data Factory'de bir SSIS paketi çağırma](how-to-invoke-ssis-package-stored-procedure-activity.md) etkinliği oluşturmayı tamamlamak için. Başlaması 30 dakika sürer. bu yana Azure-SSIS IR kullanılabilirliği için bekleyen bir Transact-SQL betiği yerleştirdiğinizden emin olun.
    ```sql
    DECLARE @return_value int, @exe_id bigint, @err_msg nvarchar(150)

    -- Wait until Azure-SSIS IR is started
    WHILE NOT EXISTS (SELECT * FROM [SSISDB].[catalog].[worker_agents] WHERE IsEnabled = 1 AND LastOnlineTime > DATEADD(MINUTE, -10, SYSDATETIMEOFFSET()))
    BEGIN
        WAITFOR DELAY '00:00:01';
    END

    EXEC @return_value = [SSISDB].[catalog].[create_execution] @folder_name=N'YourFolder',
        @project_name=N'YourProject', @package_name=N'YourPackage',
        @use32bitruntime=0, @runincluster=1, @useanyworker=1,
        @execution_id=@exe_id OUTPUT 

    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1

    EXEC [SSISDB].[catalog].[start_execution] @execution_id = @exe_id, @retry_count = 0

    -- Raise an error for unsuccessful package execution, check package execution status = created (1)/running (2)/canceled (3)/
    -- failed (4)/pending (5)/ended unexpectedly (6)/succeeded (7)/stopping (8)/completed (9) 
    IF (SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id = @exe_id) <> 7 
    BEGIN
        SET @err_msg=N'Your package execution did not succeed for execution ID: '+ CAST(@execution_id as nvarchar(20))
        RAISERROR(@err_msg, 15, 1)
    END
    ```

7. Connect **Web** etkinliğini **SSIS paketi yürütme** veya **saklı yordam** etkinlik. 

    ![Web ve saklı yordam etkinliklerini bağlama](./media/how-to-schedule-azure-ssis-integration-runtime/connect-web-sproc.png)

8. Sürükle ve bırak başka **Web** etkinliğinin sağındaki **SSIS paketi yürütme** etkinlik veya **saklı yordam** etkinlik. Etkinlik adı ayarlayın **StopIR**. 
9. Geçiş **ayarları** sekmesinde **özellikleri** penceresinde ve aşağıdaki eylemleri gerçekleştirin: 

    1. İçin **URL**, Azure SSIS IR durduran bir Web kancası URL'sini yapıştırın 
    2. İçin **yöntemi**seçin **POST**. 
    3. İçin **gövdesi**, girin `{"message":"hello world"}`.  
10. Connect **SSIS paketi yürütme** etkinlik veya **saklı yordam** son etkinlik **Web** etkinlik.

    ![Tam işlem hattı](./media/how-to-schedule-azure-ssis-integration-runtime/full-pipeline.png)
11. Tıklayarak işlem hattı ayarlarını doğrulamak **doğrulama** araç. Kapat **işlem hattı doğrulama raporu** tıklayarak **>>** düğmesi. 

    ![İşlem hattını doğrulama](./media/how-to-schedule-azure-ssis-integration-runtime/validate-pipeline.png)

### <a name="test-run-the-pipeline"></a>İşlem hattında test çalıştırması yapma

1. Seçin **Test çalıştırması** işlem hattının araç çubuğunda. Çıkışta gördüğünüz **çıkış** alt bölmesinden penceresi. 

    ![Test çalıştırması](./media/how-to-schedule-azure-ssis-integration-runtime/test-run-output.png)
2. İçinde **Runbook** sayfa Azure Otomasyonu hesabınızı işlerini Azure SSIS IR durdurmak ve başlatmak çalıştırdığını doğrulamak 

    ![Runbook işleri](./media/how-to-schedule-azure-ssis-integration-runtime/runbook-jobs.png)
3. SQL Server Management Studio'yu başlatın. İçinde **sunucuya Bağlan** penceresinde aşağıdaki eylemleri gerçekleştirin: 

    1. İçin **sunucu adı**, belirtin  **&lt;Azure SQL veritabanınızı&gt;. database.windows.net**.
    2. Seçin **Seçenekleri >>**.
    3. İçin **veritabanına bağlan**seçin **SSISDB**.
    4. **Bağlan**’ı seçin. 
    5. Genişletin **tümleştirme hizmetleri katalogları** -> **SSISDB** klasörünüze -> -> **projeleri** -> uygulamanızın SSIS Proje -> **paketleri** . 
    6. SSIS paketinizi sağ tıklatıp seçin **raporları** -> **standart raporlar** -> **tüm yürütmeleri**. 
    7. SSIS paketi çalıştırıldığını doğrulayın. 

        ![SSIS paketi Çalıştır doğrulayın](./media/how-to-schedule-azure-ssis-integration-runtime/verfiy-ssis-package-run.png)

### <a name="schedule-the-pipeline"></a>İşlem hattı zamanlama 
İşlem hattı beklediğiniz gibi çalıştığından, bu işlem hattı, belirtilen bir tempoda çalıştırılacak bir tetikleyici oluşturabilirsiniz. Zamanlama tetikleyicisi işlem hattı çalıştırmasıyla ilişkilendirme hakkında daha fazla ayrıntı için bkz. [işlem hattını bir zamanlamaya göre tetikleme](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

1. İşlem hattının araç çubuğunda **tetikleyici**seçip **yeni/Düzenle**. 

    ![Tetikleyici yeni/Düzenle->](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-new-menu.png)
2. İçinde **tetikleyici Ekle** penceresinde **+ yeni**.

    ![Tetikleyiciler - Yeni Ekle](./media/how-to-schedule-azure-ssis-integration-runtime/add-triggers-new.png)
3. İçinde **Yeni Tetikleyici**, aşağıdaki eylemleri gerçekleştirin: 

    1. İçin **adı**, tetikleyici için bir ad belirtin. Aşağıdaki örnekte, **günlük olarak çalıştırılması** tetikleyici adıdır. 
    2. İçin **türü**seçin **zamanlama**. 
    3. İçin **başlangıç tarihi**, başlangıç tarihi ve saati seçin. 
    4. İçin **yinelenme**, tetikleyici için temposu belirtin. Aşağıdaki örnekte, günlük bir kez. 
    5. İçin **son**, tarih ve saat seçerek belirtebilirsiniz **tarih** seçeneği. 
    6. Seçin **etkinleştirilmiş**. Tetikleyici, çözüm Data Factory'de yayımlamak hemen sonra etkinleştirilir. 
    7. **İleri**’yi seçin.

        ![Tetikleyici yeni/Düzenle->](./media/how-to-schedule-azure-ssis-integration-runtime/new-trigger-window.png)
4. İçinde **tetikleyici çalıştırma parametreleri** sayfasında uyarıyı gözden geçirin ve seçin **son**. 
5. Çözüm seçerek Data Factory'de yayımlamak **tümünü Yayımla** sol bölmesinde. 

    ![Tümünü Yayımla](./media/how-to-schedule-azure-ssis-integration-runtime/publish-all.png)

### <a name="monitor-the-pipeline-and-trigger-in-the-azure-portal"></a>Azure portalında tetikleyici ve işlem hattını izleme

1. Tetikleyici çalıştırmalarını izleme ve işlem hattı çalıştırmaları için kullandığınız **İzleyici** soldaki sekmesi. Ayrıntılı adımlar için bkz. [işlem hattını izleme](quickstart-create-data-factory-portal.md#monitor-the-pipeline).

    ![İşlem hattı çalıştırmaları](./media/how-to-schedule-azure-ssis-integration-runtime/pipeline-runs.png)
2. Çalıştıran bir işlem hattı çalıştırmasıyla ilişkili etkinlik çalıştırmalarını görüntülemek için ilk bağlantıyı seçin (**etkinlik çalıştırmalarını görüntüle**) içinde **eylemleri** sütun. İşlem hattının her etkinliği ile ilişkili üç Etkinlik çalıştırmalarını görürsünüz (ilk Web etkinliği, saklı yordam etkinliği ve ikinci Web etkinliği). İşlem hattı çalıştırmaları yeniden görüntülemek için geçiş yapmak için seçin **işlem hatları** üstündeki bağlantısı.

    ![Etkinlik çalıştırmaları](./media/how-to-schedule-azure-ssis-integration-runtime/activity-runs.png)
3. Tetikleyici çalıştırmaları seçerek de görüntüleyebilirsiniz **tetikleme çalıştırmalarını** yanındaki aşağı açılan listeden **işlem hattı çalıştırmalarını** en üstünde. 

    ![Tetikleyici çalıştırmaları](./media/how-to-schedule-azure-ssis-integration-runtime/trigger-runs.png)

### <a name="monitor-the-pipeline-and-trigger-with-powershell"></a>PowerShell ile tetikleyici ve işlem hattını izleme

İşlem hattı ve tetikleyici izlemek için aşağıdaki örneklerdeki gibi betikleri kullanın.

1. Bir işlem hattı çalıştırma durumunu alın.

  ```powershell
  Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $myPipelineRun
  ```

2. Tetikleyici hakkında bilgi alın.

  ```powershell
  Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name  "myTrigger"
  ```

3. Bir tetikleyici çalıştırma durumunu alın.

  ```powershell
  Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "myTrigger" -TriggerRunStartedAfter "2018-07-15" -TriggerRunStartedBefore "2018-07-16"
  ```

## <a name="next-steps"></a>Sonraki adımlar
İçin şu blog yayınına bakın:
-   [Modernleştirin ve ETL/ELT iş akışlarınızı SSIS etkinliklerle ADF işlem hatlarını genişletin](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/)

SSIS belgelerinde aşağıdaki makalelere bakın: 

- [Azure’da bir SSIS paketi dağıtma, çalıştırma ve izleme](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial)   
- [Azure’da SSIS kataloğuna bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database)
- [Azure’da paket yürütmeyi zamanlama](/sql/integration-services/lift-shift/ssis-azure-schedule-packages)
- [Windows kimlik doğrulaması ile şirket içi veri kaynaklarına bağlanma](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth)
