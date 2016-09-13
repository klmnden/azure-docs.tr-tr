<properties 
    pageTitle="Öğretici: Kopyalama Sihirbazı'nı kullanarak bir işlem hattı oluşturma" 
    description="Bu öğreticide, Data Factory ile desteklenen Kopyalama Sihirbazı’nı kullanarak Kopyalama Etkinlikli bir Azure Data Factory işlem hattı oluşturursunuz" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/01/2016" 
    ms.author="spelluru"/>

# Öğretici: Data Factory Kopyalama Sihirbazı kullanarak Kopyalama Etkinliği ile işlem hattı oluşturma
> [AZURE.SELECTOR]
- [Öğreticiye Genel Bakış](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Data Factory Düzenleyici’yi kullanma](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [PowerShell’i kullanma](data-factory-copy-activity-tutorial-using-powershell.md)
- [Visual Studio’yu kullanma](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [REST API kullanma](data-factory-copy-activity-tutorial-using-rest-api.md) 
- [Kopyalama Sihirbazı'nı kullanma](data-factory-copy-data-wizard-tutorial.md)

Bu öğreticide, Data Factory Kopyalama Sihirbazı’nı kullanarak bir veri fabrikasında Kopyalama Etkinlikli işlem hattı oluşturacaksınız. İlk olarak, Azure portalını kullanarak bir veri fabrikası oluşturun. Ardından Kopyalama Sihirbazı’nı kullanarak Data Factory bağlı hizmetleri, veri kümeleri ve bir Azure blob depolama alanından Azure SQL veritabanına veri kopyalayan bir Kopyalama Etkinlikli işlem hattı oluşturun. Kopyalama etkinliği hakkında ayrıntılı bilgi için [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) makalesine bakın. 

> [AZURE.IMPORTANT] [Öğreticiye Genel Bakış](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) makalesini inceleyin ve bu öğreticiyi uygulamadan önce önkoşul adımlarını tamamlayın.

## Veri fabrikası oluşturma
Bu adımda, Azure portalı kullanarak **ADFTutorialDataFactory** adlı bir Azure veri fabrikası oluşturursunuz.

1.  [Azure portal](https://portal.azure.com)da oturum açtıktan sonra sol üst köşedeki **+ YENİ** öğesine tıklayın, **Oluştur** dikey penceresinde **Veri analizi**’ni seçin ve **Veri analizi** dikey penceresinde **Data Factory** öğesini seçin. 

    ![Yeni->DataFactory](./media/data-factory-copy-data-wizard-tutorial/new-data-factory-menu.png)

6. **Yeni data factory** dikey penceresinde:
    1. **Ad** için **ADFTutorialDataFactory** girin. 
    
        ![Yeni veri fabrikası dikey penceresi](./media/data-factory-copy-data-wizard-tutorial/getstarted-new-data-factory.png)
    2. **KAYNAK GRUBU ADI**’na tıklayın ve şunları yapın:
        1. **Yeni bir kaynak grubu oluştur**’a tıklayın.
        2. **Kaynak grubu oluştur** dikey penceresinde kaynak grubunun **adı** olarak **ADFTutorialResourceGroup** girin ve **Tamam**’a tıklayın. 

            ![Kaynak Grubu oluşturma](./media/data-factory-copy-data-wizard-tutorial/create-new-resource-group.png)

        Bu öğreticideki adımlardan bazıları kaynak grubu için şu adı kullandığınızı varsayar: **ADFTutorialResourceGroup**. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../resource-group-overview.md).  
7. **Yeni veri fabrikası** dikey penceresinde **Başlangıç Panosuna Ekle** öğesinin seçili olmasına özen gösterin.
8. **Yeni data factory** dikey penceresinde **Oluştur**’a tıklayın.

    Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Şu hatayı alırsanız: ** “ADFTutorialDataFactory” veri fabrikası adı yok**, veri fabrikasının adını değiştirin (örneğin, yournameADFTutorialDataFactory) ve oluşturmayı yeniden deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.  
     
    ![Data Factory adı yok](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-not-available.png)
    
    > [AZURE.NOTE] Data factory adı gelecekte bir DNS adı olarak kaydedilmiş olabilir; bu nedenle herkese görünür hale gelmiştir.  

9. Soldaki **BİLDİRİMLER** hub’ına tıklayın ve oluşturma işlemine ait bildirimleri arayın. Açıksa, **BİLDİRİMLER** dikey penceresini kapatmak için **X** işaretine tıklayın. 
10. Oluşturma işlemi tamamlandıktan sonra, aşağıdaki görüntüde gösterildiği gibi **DATA FACTORY** dikey penceresini görürsünüz.

    ![Data factory giriş sayfası](./media/data-factory-copy-data-wizard-tutorial/getstarted-data-factory-home-page.png)

## İşlem hattı oluşturma

1. Data Factory giriş sayfasında **Veri kopyala** kutucuğuna tıklayarak **Kopyalama Sihirbazı**’nı başlatın. 

    > [AZURE.NOTE] Web tarayıcısının "Yetkilendiriliyor..." durumunda takıldığını görürseniz **Üçüncü taraf tanımlama bilgilerini ve site verilerini engelle** ayarını devre dışı bırakın/işaretini kaldırın (ya da) etkin durumda bırakıp **login.microsoftonline.com** için bir özel durum oluşturun ve ardından sihirbazı yeniden başlatmayı deneyin.
2. **Özellikler** sayfasında:
    1. **Görev adı** için **CopyFromBlobToAzureSql** girin
    2. **Açıklama** girin (isteğe bağlı).
    3. **Başlangıç tarihi ve saati** ile **Bitiş tarihi ve saati**’ne dikkat edin. **Bitiş tarihi ve saati** değerini **Başlangıç tarihi ve saati** değerinden sonraki gün olacak şekilde değiştirin. 
    3. **İleri**’ye tıklayın.  

    ![Kopyalama Aracı - Özellikler sayfası](./media/data-factory-copy-data-wizard-tutorial/copy-tool-properties-page.png) 
3. **Kaynak veri deposu** sayfasında **Azure Blob Storage** kutucuğuna tıklayın. Kopyalama görevine yönelik kaynak veri deposunu belirtmek için bu sayfayı kullanın. Yeni bir veri deposu belirtmek için mevcut bir veri deposu bağlı hizmetini kullanabilirsiniz (veya) yeni bir veri deposu belirtebilirsiniz. Mevcut bir bağlı hizmeti kullanmak için **MEVCUT BAĞLI HİZMETLERDEN** öğesine tıklayın ve doğru bağlı hizmeti seçin. 

    ![Kopyalama Aracı - Kaynak veri deposu sayfası](./media/data-factory-copy-data-wizard-tutorial/copy-tool-source-data-store-page.png)
5. **Azure Blob depolama hesabı belirtin** sayfasında:
    1. **Bağlı hizmet adı** için **AzureStorageLinkedService** girin.
    2. **Hesap seçme yöntemi** için **Azure aboneliklerinden** seçimini onaylayın. 
    3. Seçtiğiniz abonelikte bulunan Azure depolama hesapları listesinden bir **Azure depolama hesabı** seçin. Ayrıca **Hesap seçme yöntemi** için **El ile gir** öğesini seçip **İleri**’ye tıklayarak depolama hesabı ayarlarını el ile girmeyi seçebilirsiniz. 

    ![Kopyalama Aracı - Azure Blob depolama hesabı belirtin](./media/data-factory-copy-data-wizard-tutorial/copy-tool-specify-azure-blob-storage-account.png)
6. **Girdi dosyası veya klasörü seçin** sayfasında:
    1. **adftutorial** klasörüne gidin.
    2. **emp.txt** dosyasını seçip **Seç** öğesine tıklayın
    3. **İleri**’ye tıklayın. 

    ![Kopyalama Aracı - Girdi dosyası veya klasörü seçin](./media/data-factory-copy-data-wizard-tutorial/copy-tool-choose-input-file-or-folder.png)
7. **Dosya biçimi ayarları** sayfasında **varsayılan** değerleri seçin ve **İleri**’ye tıklayın.

    ![Kopyalama Aracı - Dosya biçimi ayarları](./media/data-factory-copy-data-wizard-tutorial/copy-tool-file-format-settings.png)  
8. Hedef veri deposu sayfasında **Azure SQL Database** kutucuğuna ve **İleri**’ye tıklayın.
9. **Azure SQL veritabanı belirtin** sayfasında:
    1. **Bağlı hizmet adı** alanına **AzureSqlLinkedService** girin. 
    2. **Sunucu/veritabanı seçme yöntemi** ayarının **Azure aboneliklerinden** olarak belirlendiğini onaylayın.
    3. **Sunucu adı** ve **Veritabanı** öğelerini seçin.
    4. **Kullanıcı Adı** ve **Parola** girin.
    5. **İleri**’ye tıklayın.  
9. **Tablo eşleme** sayfasında **Hedef** alanı için aşağı açılan listeden **emp** öğesini seçin, **aşağı oka** tıklayarak (isteğe bağlı) şemayı ve verilerin önizlemesini görün.

    ![Kopyalama Aracı - Tablo eşleme](./media/data-factory-copy-data-wizard-tutorial/copy-tool-table-mapping-page.png) 
10. **Şema eşleme** sayfasında **İleri**’ye tıklayın.
11. **Özet** sayfasındaki bilgileri gözden geçirin ve **Son**’a tıklayın. Sihirbaz, veri fabrikasında (Kopyalama Sihirbazı’nı başlattığınız yer) iki bağlı hizmet, iki veri kümesi (girdi ve çıktı) ve bir işlem hattı oluşturur. 
12. **Dağıtım başarılı** sayfasında **Kopyalama işlem hattını izlemek için buraya tıklayın** öğesine tıklayın.

    ![Kopyalama Aracı - Dağıtım başarılı](./media/data-factory-copy-data-wizard-tutorial/copy-tool-deployment-succeeded.png)  
13. Oluşturduğunuz işlem hattını izleme hakkında bilgi almak için [İzleme Uygulamasını kullanarak işlem hattını izleme ve yönetme](data-factory-monitor-manage-app.md) bölümündeki yönergeleri kullanın.

    ![İzleme Uygulaması](./media/data-factory-copy-data-wizard-tutorial/monitoring-app.png) 
 

## Ayrıca Bkz.
| Konu | Açıklama |
| :---- | :---- |
| [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) | Bu makalede, öğreticide kullandığınız Kopyalama Etkinliği hakkında ayrıntılı bilgi sağlanmaktadır. |
| [Zamanlama ve yürütme](data-factory-scheduling-and-execution.md) | Bu makalede Azure Data Factory uygulama modelinin zamanlama ve yürütme yönleri açıklanmaktadır. |
| [İşlem hatları](data-factory-create-pipelines.md) | Bu makale, Azure Data Factory’de işlem hatlarının ve etkinliklerini anlamanıza ve senaryonuz ya da işletmeniz için uçtan uca veri odaklı iş akışları oluşturmak amacıyla bunları nasıl kullanacağınızı anlamanıza yardımcı olur. |
| [Veri kümeleri](data-factory-create-datasets.md) | Bu makale, Azure Data Factory’deki veri kümelerini anlamanıza yardımcı olur.
| [İzleme Uygulaması kullanılarak işlem hatlarını izleme ve yönetme](data-factory-monitor-manage-app.md) | Bu makalede İzleme ve Yönetim Uygulaması kullanılarak işlem hatlarını izleme, yönetme ve hatalarını ayıklama işlemleri açıklanmaktadır. 


<!--HONumber=sep16_HO1-->


