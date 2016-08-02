<properties 
    pageTitle="Öğretici: Data Factory Düzenleyici kullanarak Kopyalama Etkinliği ile işlem hattı oluşturma" 
    description="Bu öğreticide, Azure Portal'daki Data Factory Düzenleyiciyi kullanarak Kopyalama Etkinliği ile bir Azure Data Factory işlem hattı oluşturacaksınız." 
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
    ms.date="05/16/2016" 
    ms.author="spelluru"/>

# Öğretici: Data Factory Düzenleyici kullanarak Kopyalama Etkinliği ile işlem hattı oluşturma
> [AZURE.SELECTOR]
- [Öğreticiye Genel Bakış](data-factory-get-started.md)
- [Data Factory Düzenleyici’yi kullanma](data-factory-get-started-using-editor.md)
- [Visual Studio’yu kullanma](data-factory-get-started-using-vs.md)
- [PowerShell’i kullanma](data-factory-monitor-manage-using-powershell.md)
- [Kopyalama Sihirbazı'nı kullanma](data-factory-copy-data-wizard-tutorial.md)


Bu öğreticide şu adımlar bulunur:

Adım | Açıklama
-----| -----------
[Azure Data Factory oluşturma](#create-data-factory) | Bu adımda, **ADFTutorialDataFactory** adlı bir Azure data factory oluşturacaksınız.  
[Bağlı hizmetler oluşturma](#create-linked-services) | Bu adımda, iki bağlı hizmet oluşturacaksınız: **AzureStorageLinkedService** ve **AzureSqlLinkedService**. AzureStorageLinkedService Azure depolamayı, AzureSqlLinkedService de Azure SQL veritabanını ADFTutorialDataFactory konumuna bağlar. İşlem hattıyla ilgili girdi verileri Azure blob depolamadaki bir blob kapsayıcısında yer alırken çıktı verileri de Azure SQL veritabanındaki bir tabloda depolanır. Bu nedenle, bu iki veri deposunu bağlı hizmet olarak data factory’ye ekliyorsunuz.      
[Girdi ve çıktı veri kümeleri oluşturma](#create-datasets) | Önceki adımda girdi/çıktı verilerini kapsayan veri depolarına başvuran bağlı hizmetleri oluşturdunuz. Bu adımda, **EmpTableFromBlob** ve **EmpSQLTable** adlı, veri depolarında depolanan girdi/çıktı verilerini temsil eden iki data factory tablosunu tanımlayacaksınız. EmpTableFromBlob için, kaynak verilere sahip bir blob’un bulunduğu blob kapsayıcısını, EmpSQLTable için de, çıktı verilerini depolayacak SQL tablosunu belirtirsiniz. Verilerin yapısı, verilerin kullanılabilirliği vb. diğer özellikleri de belirtirsiniz. 
[İşlem hattı oluşturma](#create-pipeline) | Bu adımda, ADFTutorialDataFactory’de **ADFTutorialPipeline** adlı işlem hattını oluşturacaksınız. İşlem hattında, girdi verilerini Azure blob’tan çıktı Azure SQL tablosuna kopyalayan **Kopyalama Etkinliği** vardır. Kopya Etkinliği Azure Data Factory’de veri taşımayı gerçekleştirir; etkinlikse, verileri çeşitli veri depolamaları arasında güvenli, güvenilir ve ölçeklendirilmiş bir yolla kopyalayabilen küresel olarak kullanılabilir bir hizmet tarafından desteklenir. Kopyalama etkinliği hakkında ayrıntılı bilgi için [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) makalesine bakın. 
[İşlem hattını izleme](#monitor-pipeline) | Bu adımda, girdi ve çıktı tablolarının dilimlerini Azure Portal’ı kullanarak izleyeceksiniz.

> [AZURE.IMPORTANT] Lütfen [Öğreticiye Genel Bakış](data-factory-get-started.md) makalesini inceleyip, bu öğreticiyi gerçekleştirmeden önce önkoşul adımlarını tamamlayın.

## Veri fabrikası oluşturma
Bu adımda **ADFTutorialDataFactory** adlı bir Azure data factory oluşturmak için Azure Portal’ı kullanırsınız.

1.  [Azure Portal][azure-portal]’da oturum açtıktan sonra sol alt köşede **YENİ**’ye tıklayın, **Oluştur** dikey penceresinde **Veri analizi**’ni seçin, **Veri analizi** dikey penceresinde de **Data Factory**’yi seçin. 

    ![Yeni->DataFactory][image-data-factory-new-datafactory-menu]    

6. **Yeni data factory** dikey penceresinde:
    1. **Ad** için **ADFTutorialDataFactory** girin. 
    
        ![Yeni veri fabrikası dikey penceresi][image-data-factory-getstarted-new-data-factory-blade]
    2. **KAYNAK GRUBU ADI**’na tıklayın ve şunları yapın:
        1. **Yeni bir kaynak grubu oluştur**’a tıklayın.
        2. **Kaynak grubu oluştur** dikey penceresinde kaynak grubunun **adı** olarak **ADFTutorialResourceGroup** girin ve **Tamam**’a tıklayın. 

            ![Kaynak Grubu oluşturma][image-data-factory-create-resource-group]

        Bu öğreticideki adımlardan bazıları kaynak grubu için şu adı kullandığınızı varsayar: **ADFTutorialResourceGroup**. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../resource-group-overview.md).  
7. **Yeni veri fabrikası** dikey penceresinde **Başlangıç Panosuna Ekle** öğesinin seçili olmasına özen gösterin.
8. **Yeni data factory** dikey penceresinde **Oluştur**’a tıklayın.

    Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Şu hatayı alırsanız: ** “ADFTutorialDataFactory” veri fabrikası adı yok**, veri fabrikasının adını değiştirin (örneğin, yournameADFTutorialDataFactory) ve oluşturmayı yeniden deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.  
     
    ![Data Factory adı yok][image-data-factory-name-not-available]
    
    > [AZURE.NOTE] Data factory adı gelecekte bir DNS adı olarak kaydedilmiş olabilir; bu nedenle herkese görünür hale gelmiştir.  
    > 
    > Data Factory örnekleri oluşturmak için, Azure aboneliğinde katılımcı/yönetici rolünüz olmalıdır

9. Soldaki **BİLDİRİMLER** hub’ına tıklayın ve oluşturma işlemine ait bildirimleri arayın. Açıksa, **BİLDİRİMLER** dikey penceresini kapatmak için **X** işaretine tıklayın. 
10. Oluşturma işlemi tamamlandıktan sonra, **DATA FACTORY** dikey penceresini aşağıda gösterildiği gibi görürsünüz.

    ![Data factory giriş sayfası][image-data-factory-get-stated-factory-home-page]

## Bağlı hizmetler oluşturma
Bağlı hizmetler veri depolarını veya işlem hizmetlerini Azure data factory’ye bağlar. Data store bir Azure Storage, Azure SQL Database veya şirket içi SQL Server veritabanı olabilir.

Bu adımda, iki bağlı hizmet oluşturacaksınız: **AzureStorageLinkedService** ve **AzureSqlLinkedService**. AzureStorageLinkedService bağlı hizmeti Azure Storage Hesabını, AzureSqlLinkedService de Azure SQL veritabanını **ADFTutorialDataFactory** konumuna bağlar. Daha sonra bu öğreticide, verileri AzureStorageLinkedService’teki bir blob kapsayıcısından AzureSqlLinkedService’teki bir SQL tablosuna kopyalayan bir işlem hattı oluşturacaksınız.

### Azure depolama hesabı için bağlı hizmet oluşturma
1.  **DATA FACTORY** dikey penceresinde **Geliştir ve dağıt** kutucuğuna tıklayarak data factory için **Düzenleyici**’yi başlatın.

    ![Geliştir ve Dağıt Kutucuğu][image-author-deploy-tile] 

     
5. **Düzenleyici**’de, araç çubuğundaki **Yeni veri deposu** düğmesine tıklayın ve açılan menüden **Azure depolama**’yı seçin. Sağ bölmede Azure depolama bağlı hizmeti oluşturmak için JSON şablonunu görmeniz gerekir. 

    ![Düzenleyici Yeni veri deposu düğmesi][image-editor-newdatastore-button]
    
6. Burada, **accountname** ve **accountkey** sözcüklerini Azure depolama hesabınıza ait hesap adı ve hesap anahtarı değerleriyle değiştirin. 

    ![Düzenleyici Blob Storage JSON](./media/data-factory-get-started-using-editor/getstarted-editor-blob-storage-json.png)    
    
    JSON özellikleri hakkında ayrıntılı bilgi için bkz. [JSON Betik Oluşturma Başvurusu](http://go.microsoft.com/fwlink/?LinkId=516971).

6. AzureStorageLinkedService’i dağıtmak için araç çubuğunda **Dağıt**’a tıklayın. Başlık çubuğunda **BAĞLI HİZMET BAŞARIYLA OLUŞTURULDU** iletisini gördüğünüzü onaylayın.

    ![Düzenleyici Blob Storage Dağıtma][image-editor-blob-storage-deploy]

### Azure SQL Database için bağlı hizmet oluşturma
1. **Data Factory Düzenleyici**’de, araç çubuğundaki **Yeni veri deposu** düğmesine tıklayın ve açılan menüden **Azure SQL veritabanı**’nı seçin. Sağ bölmede Azure SQL bağlı hizmeti oluşturmak için JSON şablonunu görmeniz gerekir.

    ![Düzenleyici Azure SQL Ayarları][image-editor-azure-sql-settings]

2. Burada, **servername**, **databasename**, **username@servername** ve **password** sözcüklerini Azure SQL sunucunuzun, veritabanınızın, kullanıcı hesabınızın adlarıyla ve parolasıyla değiştirin. 
3. AzureSqlLinkedService’i oluşturmak ve dağıtmak için araç çubuğunda **Dağıt**’a tıklayın. 
   

## Veri kümeleri oluşturma
Önceki adımda, Azure Storage hesabını ve Azure SQL veritabanını data factory’ye bağlamak için **AzureStorageLinkedService** ve **AzureSqlLinkedService** bağlı hizmetlerini oluşturdunuz; burada söz edilen data factory adı: **ADFTutorialDataFactory**. Bu adımda, **EmpTableFromBlob** ve **EmpSQLTable** adlı, sırasıyla AzureStorageLinkedService ve AzureSqlLinkedService’in başvurduğu veri depolarında depolanan girdi/çıktı verilerini temsil eden iki data factory tablosunu tanımlayacaksınız. EmpTableFromBlob için, kaynak verilere sahip bir blob’un bulunduğu blob kapsayıcısını, EmpSQLTable için de, çıktı verilerini depolayacak SQL tablosunu belirtirsiniz. 

### Girdi veri kümesi oluşturma 
Tablo dikdörtgen bir veri kümesidir ve bir şeması vardır. Bu adımda, **AzureStorageLinkedService** bağlı hizmetiyle temsil edilen Azure Storage’da bir blob kapsayıcısını belirten **EmpBlobTable** adlı bir tablo oluşturacaksınız.

1. Data Factory’deki **Düzenleyici**’de, araç çubuğunda yer alan **Yeni veri kümesi** kümesine, sonra da açılan menüden **Blob tablosu**’na tıklayın. 
2. Sağ bölmedeki JSON ifadesini aşağıdaki JSON parçacığıyla değiştirin: 

        {
          "name": "EmpTableFromBlob",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
              "folderPath": "adftutorial/",
              "fileName": "emp.txt",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "external": true,
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

        
     Şunlara dikkat edin: 
    
    - veri kümesi **türü** **AzureBlob** olarak ayarlanır.
    - **linkedServiceName** **AzureStorageLinkedService** olarak ayarlanır. Bu bağlı hizmeti 2. adımda oluşturmuştunuz.
    - **folderPath** **adftutorial** kapsayıcısı olarak ayarlanır. Ayrıca klasörün içinde blob adını belirtebilirsiniz. Blob adını belirtmediğinizden, kapsayıcıdaki tüm blob'lara ait veriler girdi verisi olarak kabul edilir.  
    - biçim **türü** **TextFormat** olarak ayarlanır
    - Metin dosyasında virgül karakteriyle (**columnDelimiter**) ayrılmış, **FirstName** ve **LastName** adlı iki alan vardır 
    - **kullanılabilirlik** **hourly** olarak ayarlanır (**frequency** **hour** ve **interval** **1** olarak ayarlanır), bu nedenle, Data Factory hizmeti belirttiğiniz blob kapsayıcısındaki (**adftutorial**) kök klasörde her saat başı girdi verilerini arayacaktır. 
    

    **Girdi** **tablosu** için bir **fileName** belirtmediyseniz, girdi klasörüne ait (**folderPath**) tüm dosyalar/blob’lar girdi olarak kabul edilir. JSON’da fileName belirtmediyseniz, yalnızca belirtilen dosya/blob girdi olarak kabul edilir.
 
    **Çıktı tablosu** için bir **fileName** belirtmezseniz, **folderPath**’de oluşturulan dosyaları şu biçimde adlandırılır: Data.&lt;Guid\&gt;.txt (örnek: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    **folderPath** ve **fileName** öğelerini dinamik olarak **SliceStart** zamanı temelinde ayarlamak için **partitionedBy** özelliğini kullanın. Aşağıdaki örnekte, folderPath SliceStart’taki (işlemdeki dilimin başlangıç zamanı) Yıl, Ay ve Gün öğelerini, fileName de SliceStart’taki Saat öğesini kullanır. Örneğin, dilim 2014-10-20T08:00:00 için oluşturulduysa, folderName wikidatagateway/wikisampledataout/2014/10/20, fileName de 08.csv olarak ayarlanır. 

        "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
        "fileName": "{Hour}.csv",
        "partitionedBy": 
        [
            { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
            { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
            { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
            { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
        ],

    JSON özellikleri hakkında ayrıntılı bilgi için bkz. [JSON Betik Oluşturma Başvurusu](http://go.microsoft.com/fwlink/?LinkId=516971).

2. **EmpTableFromBlob** tablosunu oluşturmak ve dağıtmak için araç çubuğunda **Dağıt**’a tıklayın. Düzenleyici’nin başlık çubuğunda **TABLO BAŞARIYLA OLUŞTURULDU** iletisini gördüğünüzü onaylayın.

### Çıktı veri kümesi oluşturma
Adımın bu bölümünde, **AzureSqlLinkedService** bağlı hizmetinin temsil ettiği Azure SQL veritabanındaki bir SQL tablosunu gösteren **EmpSQLTable** adlı bir çıktı tablosu oluşturacaksınız. 

1. Data Factory’deki **Düzenleyici**’de, araç çubuğunda yer alan **Yeni veri kümesi** kümesine, sonra da açılan menüden **Azure SQL tablosu**’na tıklayın. 
2. Sağ bölmedeki JSON ifadesini aşağıdaki JSON parçacığıyla değiştirin:

        {
          "name": "EmpSQLTable",
          "properties": {
            "structure": [
              {
                "name": "FirstName",
                "type": "String"
              },
              {
                "name": "LastName",
                "type": "String"
              }
            ],
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
              "tableName": "emp"
            },
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

        
     Şunlara dikkat edin: 
    
    * veri kümesi **türü** **AzureSQLTable** olarak ayarlanır.
    * **linkedServiceName** **AzureSqlLinkedService** olarak ayarlanır (bu bağlı hizmeti 2. adımda oluşturmuştunuz).
    * **tablename** **emp** olarak ayarlanır.
    * Veritabanındaki emp tablosunda **ID**, **FirstName** ve **LastName** adıyla üç sütun bulunur; ancak ID bir kimlik sütunudur, bu nedenle burada yalnızca **FirstName** ve **LastName** belirtmeniz yeterlidir.
    * **availability** **hourly** olarak ayarlanmıştır (**frequency** **hour**, **interval** de **1** olarak ayarlanmıştır).  Data Factory hizmeti Azure SQL veritabanındaki **emp** tablosunda her saat bir çıktı veri dilimi oluşturacaktır.


3. **EmpSQLTable** tablosunu oluşturmak ve dağıtmak için araç çubuğunda **Dağıt**’a tıklayın.


## İşlem hattı oluşturma
Su adımda, girdi olarak **EmpTableFromBlob**, çıktı olarak da **EmpSQLTable** kullanan bir **Kopyalama Etkinliği**’ne sahip işlem hattı oluşturacaksınız.

1. Data Factory’nin **Düzenleyici** konumunda, anaç çubuğundaki **Yeni işlem hattı** düğmesine tıklayın. Düğmeyi görmüyorsanız araç çubuğunda **... (Üç nokta)** seçeneğine tıklayın. Alternatif olarak, ağaç görünümünde **İşlem hatları**’na sağ tıklayın ve**Yeni işlem hattı**’na tıklayın.

    ![Düzenleyici Yeni İşlem Hattı Düğmesi][image-editor-newpipeline-button]
 
2. Sağ bölmedeki JSON ifadesini aşağıdaki JSON parçacığıyla değiştirin: 
        
        {
          "name": "ADFTutorialPipeline",
          "properties": {
            "description": "Copy data from a blob to Azure SQL table",
            "activities": [
              {
                "name": "CopyFromBlobToSQL",
                "description": "Push Regional Effectiveness Campaign data to Azure SQL database",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "EmpTableFromBlob"
                  }
                ],
                "outputs": [
                  {
                    "name": "EmpSQLTable"
                  }
                ],
                "typeProperties": {
                  "source": {
                    "type": "BlobSource"
                  },
                  "sink": {
                    "type": "SqlSink",
                    "writeBatchSize": 10000,
                    "writeBatchTimeout": "60:00:00"
                  }
                },
                "Policy": {
                  "concurrency": 1,
                  "executionPriorityOrder": "NewestFirst",
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2015-07-12T00:00:00Z",
            "end": "2015-07-13T00:00:00Z"
          }
        } 

    Şunlara dikkat edin:

    - Etkinlikler bölümünde, **türü** **CopyActivity** olarak ayarlanmış yalnızca bir etkinlik vardır.
    - Etkinlik girdisi **EmpTableFromBlob** olarak, etkinlik çıktısı da **EmpSQLTable** olarak ayarlanmıştır.
    - **dönüştürme** bölümünde **BlobSource** kaynak türü, **SqlSink** de havuz türü olarak belirtilir.

    **start** özelliğinin değerini geçerli günle, **end** değerini de sonraki günle değiştirin. Tarih saatin yalnızca tarih bölümünü belirtip saat bölümünü atlayabilirsiniz. Örneğin, "2015-02-03", "2015-02-03T00:00:00Z" ile eşdeğerdir
    
    Başlangıç ve bitiş tarih saatleri [ISO biçiminde](http://en.wikipedia.org/wiki/ISO_8601) olmalıdır. Örneğin: 2014-10-14T16:32:41Z. **end** saati isteğe bağlıdır; ancak bu öğreticide bunu kullanacağız. 
    
    **end** özelliği için değer belirtmezseniz "**start + 48 hours**" olarak hesaplanır. İşlem hattını süresiz olarak çalıştırmak için **end** özelliği değerini **9999-09-09** olarak ayarlayın.
    
    Yukarıdaki örnekte, her veri dilimi saatlik oluşturulduğundan 24 veri dilimi olacaktır.
    
    JSON özellikleri hakkında ayrıntılı bilgi için bkz. [JSON Betik Oluşturma Başvurusu](http://go.microsoft.com/fwlink/?LinkId=516971).

4. **ADFTutorialPipeline** tablosunu oluşturmak ve dağıtmak için araç çubuğunda **Dağıt**’a tıklayın. **İŞLEM HATTI BAŞARIYLA OLUŞTURULDU** iletisini gördüğünüzü onaylayın.
5. Şimdi, **Düzenleyici** dikey penceresini **X** işaretine tıklayarak kapatın. Araç çubuğu ve ağaç görünümüne sahip ADFTutorialDataFactory dikey penceresini kapatmak için de **X** işaretine bir kez daha tıklayın. **Kaydedilmemiş düzenlemeleriniz atılacak** iletisini görürseniz **Tamam**’a tıklayın.
6. **ADFTutorialDataFactory** için **DATA FACTORY** dikey penceresine dönmeniz gerekir.

**Tebrikler!** Başarılı bir şekilde Azure data factory, bağlı hizmetler, tablolar ve işlem hattı oluşturdunuz, işlem hattını zamanladınız.   
 
### Data factory’yi Diyagram Görünümünde görüntüleme 
1. **DATA FACTORY** dikey penceresinde **Diyagram**’a tıklayın.

    ![Data Factory Dikey Penceresi - Diyagram Kutucuğu][image-datafactoryblade-diagramtile]

2. Aşağıdakine benzer bir diyagram görmeniz gerekir: 

    ![Diyagram görünümü][image-data-factory-get-started-diagram-blade]

    İşlem hatlarını ve tabloları yakınlaştırabilir, uzaklaştırabilir, %100 yakınlaştırabilir, sığacak kadar yakınlaştırabilirsiniz ve çizgileri gösterebilirsiniz (seçilen öğelerin yukarı akış ve aşağı akış öğelerini vurgular).  Özelliklerini görmek için bir nesneye (girdi/çıktı tablosu veya işlem hattı) çift tıklayabilirsiniz. 
3. Diyagram Görünümü’nde **ADFTutorialPipeline**’a sağ tıklayın ve **Ardışık düzeni aç**’a tıklayın. İşlem hattında etkinlikleri, etkinliklerle ilgili girdi ve çıktı veri kümeleriyle birlikte görebilirsiniz. Bu öğreticide, işlem hattında girdi verisi olarak EmpTableBlob, çıktı verisi olarak da EmpSQLTable bulunan tek bir etkinliğiniz (Kopyalama Etkinliği) vardır.   

    ![İşlem Hattını Açma](./media/data-factory-get-started-using-editor/DiagramView-OpenPipeline.png)

4. Diyagram görünümüne dönmek için sol üst köşede yer alan içerik haritasındaki **Data factory**’ye tıklayın. Diyagram görünümü tüm işlem hatlarını görüntüler. Bu örnekte, yalnızca bir işlem hattı oluşturdunuz.   
 

## İşlem hattını izleme
Bu adımda, Azure data factory’de neler olduğunu izlemek için Azure Portal kullanacaksınız. Veri kümelerini ve işlem hatlarını izlemek için de PowerShell cmdlet'lerini kullanabilirsiniz. İzleme için cmdlet kullanma hakkında ayrıntılı bilgi için bkz. [PowerShell Cmdlet’lerini Kullanarak Data Factory’yi İzleme ve Yönetme][monitor-manage-using-powershell].

1. Açık değilse, [Azure Portal (Preview)][azure-portal] seçeneğine gidin. 
2. **ADFTutorialDataFactory** dikey penceresi açık değilse, **Startboard**’ta **ADFTutorialDataFactory**’ye tıklayarak açın. 
3. Bu dikey pencerede tabloların adlarını ve sayısını, oluşturduğunuz işlem hattını görmeniz gerekir.

    ![adların olduğu giriş sayfası][image-data-factory-get-started-home-page-pipeline-tables]

4. Şimdi, **Veri kümeleri** kutucuğuna tıklayın.
5. **Veri kümeleri** dikey penceresinde **EmpTableFromBlob**’a tıklayın. Bu, **ADFTutorialPipeline** girdi tablosudur.

    ![EmpTableFromBlob seçili olarak veri kümeleri][image-data-factory-get-started-datasets-emptable-selected]   
5. Geçerli zamana kadar olan veri dilimlerinin zaten oluşturulmuş olduğunu ve **Hazır** olduğunu unutmayın; çünkü **emp.txt** dosyası her zaman şu blob kapsayıcısında yer almaktadır: **adftutorial\input**. Alttaki **En son başarısız olan dilimler** bölümünde hiç dilim gösterilmediğini onaylayın.

    Hem **En son güncelleştirilen dilimler**, hem de **En son başarısız olan dilimler** listesi **SON GÜNCELLEŞTİRME ZAMANI**’na göre listelenir. Dilimin güncelleştirme zamanı aşağıdaki durumlarda değişir. 
    

    -  Dilimin durumunu el ile güncellersiniz; örneğin, **Set-AzureRmDataFactorySliceStatus**’ü kullanarak (veya) dilimle ilgili **DİLİM** dikey penceresinde **ÇALIŞTIR**’a tıklayarak.
    -  Bir yürütme nedeniyle dilim durumunu değiştirir (örn., çalışma başlatıldı, çalışma sonlandırıldı ve başarısız, çalışma sonlandırıldı ve başarılı vb.).
 
    Dilimlerin daha büyük listesini görmek için listeler başlığına veya **... (üç nokta)** seçeneğine tıklayın. Dilimlere filtre uygulamak için araç çubuğunda **Filtre**’ye tıklayın.  
    
    Bunun yerine, dilim başlangıç/bitiş zamanına göre sıralanmış veri dilimlerini görüntülemek için **Veri dilimleri (dilimlenme zamanına göre)** kutucuğuna tıklayın.   

    ![Dilimlenme Zamanına Göre Veri Dilimleri][DataSlicesBySliceTime]   

6. Şimdi, **Veri Kümeleri** dikey penceresinde, **EmpSQLTable** öğesine tıklayın. Bu, **ADFTutorialPipeline** çıktı tablosudur.

    ![veri kümeleri dikey penceresi][image-data-factory-get-started-datasets-blade]



     
6. **EmpSQLTable** dikey penceresini aşağıda gösterildiği gibi görmeniz gerekir:

    ![tablo dikey penceresi][image-data-factory-get-started-table-blade]
 
7. Geçerli zamana kadar olan veri dilimlerinin zaten oluşturulduğunu ve **Hazır** olduklarını unutmayın. Alttaki **Sorun dilimleri** bölümünde hiç dilim gösterilmiyor.
8. Tüm dilimleri görmek için **… (Üç nokta)** seçeneğine tıklayın.

    ![veri dilimleri dikey penceresi][image-data-factory-get-started-dataslices-blade]

9. Listeden herhangi bir veri dilimine tıklayın; **VERİ DİLİMİ** dikey penceresini görmeniz gerekir.

    ![veri dilimi dikey penceresi][image-data-factory-get-started-dataslice-blade]
  
    Dilim **Hazır** durumunda değilse, Hazır olmayan ve geçerli dilimin yürütülmesini engelleyen yukarı akış dilimlerini **Hazır olmayan yukarı akış dilimleri** listesinde görebilirsiniz. 

11. **VERİ DİLİMİ** dikey penceresinde, alttaki listede tüm etkinlik çalıştırmalarını görmelisiniz. **ETKİNLİK ÇALIŞMA AYRINTILARI** dikey penceresini görmek için bir **etkinlik çalıştırmasına**tıklayın. 

    ![Etkinlik Çalışma Ayrıntıları][image-data-factory-get-started-activity-run-details]

    
12. **ADFTutorialDataFactory** giriş dikey penceresine dönene kadar tüm dikey pencereleri kapatmak için **X** işaretine tıklayın.
14. (isteğe bağlı) **ADFTutorialDataFactory** için giriş sayfasındaki **İşlem hatları**’na, **İşlem hatları** dikey penceresinde **ADFTutorialPipeline**’a tıklayın, girdi tablolarında (**Tüketilen**) çıktı tablolarında (**Üretilen**) ayrıntılarına gidin.
15. **SQL Server Management Studio**’yu başlatın, Azure SQL Database’e bağlanın ve veritabanındaki **emp** tablosuna satırların eklenmiş olduğunu doğrulayın.

    ![sql sorgu sonuçları][image-data-factory-get-started-sql-query-results]


## Özet 
Bu öğreticide Azure blob’undan Azure SQL veritabanına veri kopyalamak üzere Azure data factory oluşturdunuz. Data factory, bağlı hizmetler, veri kümeleri ve işlem hattı oluşturmak için Azure Portal’ı kullandınız. Bu öğreticide gerçekleştirilen üst düzey adımları şunlardır:  

1.  Oluşturulan Azure **data factory**.
2.  Oluşturulan **bağlı hizmetler**:
    1. Girdi verilerini tutan Azure Storage hesabınıza bağlamak için **Azure Storage** bağlı hizmeti.    
    2. Çıktı verilerini tutan Azure SQL veritabanınıza bağlamak için **Azure SQL** bağlı hizmeti. 
3.  İşlem hatları için girdi verilerini ve çıktı verilerini açıklayan oluşturulan **veri kümeleri**.
4.  Kaynak olarak **BlobSource**’u, havuz olarak da **SqlSink**’i kapsayan **Kopyalama Etkinliği**’ne sahip oluşturulan **işlem hattı**.  


## Ayrıca Bkz.
| Konu | Açıklama |
| :---- | :---- |
| [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) | Bu makalede, öğreticide kullandığınız Kopyalama Etkinliği hakkında ayrıntılı bilgi sağlanmaktadır. |
| [Zamanlama ve yürütme](data-factory-scheduling-and-execution.md) | Bu makalede Azure Data Factory uygulama modelinin zamanlama ve yürütme yönleri açıklanmaktadır. |
| [İşlem hatları](data-factory-create-pipelines.md) | Bu makalede Azure Data Factory’de işlem hatlarının ve etkinliklerin yanı sıra senaryonuz ya da işiniz için uçtan uca veri odaklı iş akışlarının nasıl desteklendiğini anlamanıza yardımcı olunmaktadır. |
| [Veri kümeleri](data-factory-create-datasets.md) | Bu makale Azure Data Factory’deki veri kümelerini anlamanıza yardımcı olacaktır.
| [İzleme Uygulaması kullanılarak işlem hatlarını izleme ve yönetme](data-factory-monitor-manage-app.md) | Bu makalede İzleme ve Yönetim Uygulaması kullanılarak işlem hatlarını izleme, yönetme ve hatalarını ayıklama işlemleri açıklanmaktadır. 

<!--Link references-->
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[msdn-activities]: https://msdn.microsoft.com/library/dn834988.aspx
[msdn-linkedservices]: https://msdn.microsoft.com/library/dn834986.aspx
[data-factory-naming-rules]: https://msdn.microsoft.com/library/azure/dn835027.aspx

[azure-portal]: https://portal.azure.com/
[download-azure-powershell]: http://azure.microsoft.com/documentation/articles/install-configure-powershell
[sql-management-studio]: http://azure.microsoft.com/documentation/articles/sql-database-manage-azure-ssms/#Step2
[sql-cmd-exe]: https://msdn.microsoft.com/library/azure/ee336280.aspx

[monitor-manage-using-powershell]: data-factory-monitor-manage-using-powershell.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[data-factory-introduction]: data-factory-introduction.md
[data-factory-create-storage]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account



[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456

<!--Image references-->

[DataSlicesBySliceTime]: ./media/data-factory-get-started-using-editor/DataSlicesBySliceTime.png

[image-data-factory-getstarted-new-data-factory-blade]: ./media/data-factory-get-started-using-editor/getstarted-new-data-factory.png

[image-data-factory-get-stated-factory-home-page]: ./media/data-factory-get-started-using-editor/getstarted-data-factory-home-page.png

[image-author-deploy-tile]: ./media/data-factory-get-started-using-editor/getstarted-author-deploy-tile.png

[image-editor-newdatastore-button]: ./media/data-factory-get-started-using-editor/getstarted-editor-newdatastore-button.png

[image-editor-blob-storage-deploy]: ./media/data-factory-get-started-using-editor/getstarted-editor-blob-storage-deploy.png

[image-editor-azure-sql-settings]: ./media/data-factory-get-started-using-editor/getstarted-editor-azure-sql-settings.png

[image-editor-newpipeline-button]: ./media/data-factory-get-started-using-editor/getstarted-editor-newpipeline-button.png

[image-datafactoryblade-diagramtile]: ./media/data-factory-get-started-using-editor/getstarted-datafactoryblade-diagramtile.png


[image-data-factory-get-started-diagram-blade]: ./media/data-factory-get-started-using-editor/getstarted-diagram-blade.png

[image-data-factory-get-started-home-page-pipeline-tables]: ./media/data-factory-get-started-using-editor/getstarted-datafactory-home-page-pipeline-tables.png

[image-data-factory-get-started-datasets-blade]: ./media/data-factory-get-started-using-editor/getstarted-datasets-blade.png

[image-data-factory-get-started-table-blade]: ./media/data-factory-get-started-using-editor/getstarted-table-blade.png

[image-data-factory-get-started-dataslices-blade]: ./media/data-factory-get-started-using-editor/getstarted-dataslices-blade.png

[image-data-factory-get-started-dataslice-blade]: ./media/data-factory-get-started-using-editor/getstarted-dataslice-blade.png

[image-data-factory-get-started-sql-query-results]: ./media/data-factory-get-started-using-editor/getstarted-sql-query-results.png

[image-data-factory-get-started-datasets-emptable-selected]: ./media/data-factory-get-started-using-editor/DataSetsWithEmpTableFromBlobSelected.png

[image-data-factory-get-started-activity-run-details]: ./media/data-factory-get-started-using-editor/ActivityRunDetails.png

[image-data-factory-create-resource-group]: ./media/data-factory-get-started-using-editor/CreateNewResourceGroup.png


[image-data-factory-new-datafactory-menu]: ./media/data-factory-get-started-using-editor/NewDataFactoryMenu.png


[image-data-factory-name-not-available]: ./media/data-factory-get-started-using-editor/getstarted-data-factory-not-available.png
 


<!---HONumber=Jun16_HO2-->


