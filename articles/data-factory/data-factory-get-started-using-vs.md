<properties 
    pageTitle="Öğretici: Visual Studio kullanarak Kopyalama Etkinliği ile işlem hattı oluşturma" 
    description="Bu öğreticide, Visual Studio kullanarak Kopyalama Etkinliği ile bir Azure Data Factory işlem hattı oluşturacaksınız." 
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

# Öğretici: Visual Studio kullanarak Kopyalama Etkinliği ile işlem hattı oluşturma
> [AZURE.SELECTOR]
- [Öğreticiye Genel Bakış](data-factory-get-started.md)
- [Data Factory Düzenleyici’yi kullanma](data-factory-get-started-using-editor.md)
- [Visual Studio’yu kullanma](data-factory-get-started-using-vs.md)
- [PowerShell’i kullanma](data-factory-monitor-manage-using-powershell.md)
- [Kopyalama Sihirbazı'nı kullanma](data-factory-copy-data-wizard-tutorial.md)

Bu öğreticide Visual Studio 2013 kullanarak yapacaklarınız:

1. İki bağlı hizmet oluşturun: **AzureStorageLinkedService1** ve **AzureSqlinkedService1**. AzureStorageLinkedService1 bağlı hizmeti Azure depolamayı, AzureSqlLinkedService1 de Azure SQL veritabanını data factory: **ADFTutorialDataFactoryVS** konumuna bağlar. İşlem hattıyla ilgili girdi verileri Azure blob depolamadaki bir blob kapsayıcısında yer alırken çıktı verileri de Azure SQL veritabanındaki bir tabloda depolanır. Bu nedenle, bu iki veri deposunu bağlı hizmet olarak data factory’ye ekliyorsunuz.
2. **EmpTableFromBlob** ve **EmpSQLTable** adlı, veri depolarında depolanan girdi/çıktı verilerini temsil eden iki data factory tablosu oluşturun. EmpTableFromBlob için, kaynak verilere sahip bir blob’un bulunduğu blob kapsayıcısını, EmpSQLTable için de, çıktı verilerini depolayacak SQL tablosunu belirtirsiniz. Verilerin yapısı, verilerin kullanılabilirliği vb. diğer özellikleri de belirtirsiniz.
3. ADFTutorialDataFactoryVS’de **ADFTutorialPipeline** adlı işlem hattını oluşturun. İşlem hattında, girdi verilerini Azure blob’tan çıktı Azure SQL tablosuna kopyalayan **Kopyalama Etkinliği** vardır. Kopya Etkinliği Azure Data Factory’de veri taşımayı gerçekleştirir; etkinlikse, verileri çeşitli veri depolamaları arasında güvenli, güvenilir ve ölçeklendirilmiş bir yolla kopyalayabilen küresel olarak kullanılabilir bir hizmet tarafından desteklenir. Kopyalama etkinliği hakkında ayrıntılı bilgi için [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) makalesine bakın. 
4. Data factory oluşturup bağlı hizmetleri, tabloları ve işlem hattını dağıtın.    

## Ön koşullar
Lütfen [Öğreticiye Genel Bakış](data-factory-get-started.md) makalesini inceleyip, bu öğreticiyi gerçekleştirmeden önce ön koşul adımlarını tamamlayın.

Bilgisayarınızda şunların yüklü olması gerekir: 
- Visual Studio 2013
- Visual Studio 2013 için Azure SDK’sını indirin. [Azure İndirme Sayfası](https://azure.microsoft.com/downloads/)’na gidin ve **.NET** bölümündeki **VS 2013 yükleme**’ye tıklayın.

## Visual Studio projesi oluşturma 
1. **Visual Studio 2013**’ü başlatın. **Dosya**’ya tıklayın, **Yeni**’nin üzerine gelin ve **Proje**’ye tıklayın. **Yeni Proje** iletişim kutusu görmeniz gerekir.  
2. **Yeni Proje** iletişim kutusunda **DataFactory** şablonunu seçip **Boş Data Factory Projesi**’ne tıklayın. DataFactory şablonunu görmüyorsanız, Visual Studio’yu kapatın, Visual Studio 2013 için Azure SDK'sını yükleyin ve Visual Studio’yu yeniden açın.  

    ![Yeni proje iletişim kutusu](./media/data-factory-get-started-using-vs/new-project-dialog.png)

3. Proje için bir **ad**, **konum** ve **çözüm** için ad girip **Tamam**’a tıklayın.

    ![Çözüm Gezgini](./media/data-factory-get-started-using-vs/solution-explorer.png)   

## Bağlı hizmetler oluşturma
Bağlı hizmetler veri depolarını veya işlem hizmetlerini Azure data factory’ye bağlar. Data store bir Azure Storage, Azure SQL Database veya şirket içi SQL Server veritabanı olabilir.

Bu adımda, iki bağlı hizmet oluşturacaksınız: **AzureStorageLinkedService1** ve **AzureSqlLinkedService1**. AzureStorageLinkedService1 bağlı hizmeti Azure Storage Hesabını, AzureSqlLinkedService de Azure SQL veritabanını data factory: **ADFTutorialDataFactory** konumuna bağlar. 

### Azure Storage bağlı hizmeti oluşturma

4. Çözüm gezgininde **Bağlı Hizmetler**’e sağ tıklayın, **Ekle**’nin üzerine gelip **Yeni Öğe**’ye tıklayın.      
5. **Yeni Öğe Ekle** iletişim kutusunda **Azure Storage Bağlı Hizmeti**’ni listeden seçip **Ekle**’ye tıklayın. 

    ![Yeni Bağlı Hizmet](./media/data-factory-get-started-using-vs/new-linked-service-dialog.png)
 
3. **accountname** ve **accountkey** sözcüklerini Azure depolama hesabınızın adı ve anahtarıyla değiştirin. 

    ![Azure Storage Bağlı Hizmeti](./media/data-factory-get-started-using-vs/azure-storage-linked-service.png)

4. **AzureStorageLinkedService1.json** dosyasını kaydedin.

### Azure SQL bağlı hizmeti oluşturma

5. **Çözüm Gezgini**’nde bir kez daha **Bağlı Hizmetler** düğümüne sağ tıklayın, **Ekle**’nin üzerine gelip **Yeni Öğe**’ye tıklayın. 
6. Bu sefer, **Azure SQL Bağlı Hizmeti**’ni seçin ve **Ekle**’ye tıklayın. 
7. **AzureSqlLinkedService1.json dosyasında**, **servername**, **databasename**, **username@servername** ve **password** sözcüklerini Azure SQL sunucunuzun, veritabanınızın, kullanıcı hesabınızın adlarıyla ve parolasıyla değiştirin.    
8.  **AzureSqlLinkedService1.json** dosyasını kaydedin. 


## Veri kümeleri oluşturma
Önceki adımda, Azure Storage hesabını ve Azure SQL veritabanını data factory’ye bağlamak için **AzureStorageLinkedService1** ve **AzureSqlLinkedService1** bağlı hizmetlerini oluşturdunuz; burada söz edilen data factory adı: **ADFTutorialDataFactory**. Bu adımda, **EmpTableFromBlob** ve **EmpSQLTable** adlı, sırasıyla AzureStorageLinkedService1 ve AzureSqlLinkedService1’in başvurduğu veri depolarında depolanan girdi/çıktı verilerini temsil eden iki data factory tablosunu tanımlayacaksınız. EmpTableFromBlob için, kaynak verilere sahip bir blob’un bulunduğu blob kapsayıcısını, EmpSQLTable için de, çıktı verilerini depolayacak SQL tablosunu belirtirsiniz.

### Girdi veri kümesi oluşturma

9. **Çözüm Gezgini**’nde **Tablolar**’a sağ tıklayın, **Ekle**’nin üzerine gelip **Yeni Öğe**’ye tıklayın.
10. **Yeni Öğe Ekle** iletişim kutusunda, **Azure Blob**’u seçin ve **Ekle**’ye tıklayın.   
10. JSON metnini aşağıdaki metinle değiştirin ve **AzureBlobLocation1.json** dosyasını kaydedin. 

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
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
              "folderPath": "adftutorial/",
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

### Çıktı veri kümesi oluşturma

11. **Çözüm Gezgini**’nde bir kez daha **Tablolar**’a sağ tıklayın, **Ekle**’nin üzerine gelip **Yeni Öğe**’ye tıklayın.
12. **Yeni Öğe Ekle** iletişim kutusunda, **Azure SQL**’i seçin ve **Ekle**’ye tıklayın. 
13. JSON metnini aşağıdaki JSON ile değiştirin ve **AzureSqlTableLocation1.json** dosyasını kaydedin.

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
            "linkedServiceName": "AzureSqlLinkedService1",
            "typeProperties": {
              "tableName": "emp"
            },
            "availability": {
              "frequency": "Hour",
              "interval": 1
            }
          }
        }

## İşlem hattı oluşturma 
Şu ana kadar girdi/çıktı bağlı hizmetleri ve tablolarını oluşturdunuz. Şimdi de, verileri Azure blob’tan Azure SQL veritabanına kopyalamak için **Kopyalama Etkinliği**’ne sahip bir işlem hattı oluşturacaksınız. 


1. **Çözüm Gezgini**’nde **İşlem Hatları**’na sağ tıklayın, **Ekle**’nin üzerine gelip **Yeni Öğe**’ye tıklayın.  
15. **Yeni Öğe Ekle** iletişim kutusunda **Veri İşlem Hattı Kopyala**’yı seçip **Ekle**’ye tıklayın. 
16. JSON’yi aşağıdaki JSON ile değiştirin ve **CopyActivity1.json** dosyasını kaydedin.
            
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
                  "style": "StartOfInterval",
                  "retry": 0,
                  "timeout": "01:00:00"
                }
              }
            ],
            "start": "2015-07-12T00:00:00Z",
            "end": "2015-07-13T00:00:00Z",
            "isPaused": false
          }
        }

## Data Factory varlıklarını yayımlama/dağıtma
  
18. Çözüm Gezgini’nde projeye sağ tıklayın ve ardından **Yayımla**’ya tıklayın. 
19. **Microsoft hesabınızda oturum açın** iletişim kutusunu görmezseniz, Azure aboneliğindeki kimlik bilgilerini hesap için girin ve **oturum aç**’a tıklayın.
20. Aşağıdaki iletişim kutusunu göreceksiniz:

    ![Yayımla iletişim kutusu](./media/data-factory-get-started-using-vs/publish.png)

21. Data factory yapılandırma sayfasında aşağıdakileri yapın: 
    1. **Yeni Data Factory Oluştur** seçeneğini seçin.
    2. **Ad** için **VSTutorialFactory** girin.  
    
        > [AZURE.NOTE]  
        > Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Yayımlandığında data factory adıyla ilgili bir hata alırsanız data factory adını değiştirip (örneğin, yournameVSTutorialFactory) yayımlamayı yeniden deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.
        
    3. **Abonelik** alanı için doğru abonelik seçin. 
    4. oluşturulacak data factory için **kaynak grubu** seçin. 
    5. Data factory için **bölge** seçin. 
    6. **Öğeleri Yayımla** sayfasına geçmek için **İleri**’ye tıklayın. 
23. **Öğeleri Yayımla** sayfasında tüm Data Factory varlıklarının işaretli olmasını sağlayın ve **Özet** sayfasına geçmek için **İleri**’ye tıklayın.     
24. Özeti gözden geçirin, dağıtım işlemini başlatmak ve **Dağıtım Durumu**’nu görüntülemek için **İleri**’ye tıklayın.
25. **Dağıtım Durumu** sayfasında dağıtım sürecinin durumunu görmelisiniz. Dağıtımını gerçekleştirdikten sonra Son'a tıklayın. 

Lütfen şunlara dikkat edin: 

- Şu hatayı alırsanız: "**Abonelik, Microsoft.DataFactory ad alanını kullanacak şekilde kaydedilmemiş**", aşağıdakilerden birini yapın ve yeniden yayımlamayı deneyin: 

    - Azure PowerShell’de Data Factory sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Data Factory sağlayıcısının kayıtlı olduğunu onaylamak için aşağıdaki komutu çalıştırabilirsiniz. 
    
            Get-AzureRmResourceProvider
    - Azure aboneliğini kullanarak [Azure portalında](https://portal.azure.com) oturum açın ve Data Factory dikey penceresine gidin (ya da) Azure portalında bir data factory oluşturun. Bu otomatik olarak sağlayıcıyı sizin için kaydeder.
-   Data factory adı gelecekte bir DNS adı olarak kaydedilmiş olabilir; bu nedenle herkese görünür hale gelmiştir.
-   Data Factory örnekleri oluşturmak için, Azure aboneliğinde katılımcı/yönetici rolünüz olmalıdır

## Özet
Bu öğreticide Azure blob’undan Azure SQL veritabanına veri kopyalamak üzere Azure data factory oluşturdunuz. Data factory, bağlı hizmetler, veri kümeleri ve işlem hattı oluşturmak için Visual Studio’yu kullandınız. Bu öğreticide gerçekleştirilen üst düzey adımları şunlardır:  

1.  Oluşturulan Azure **data factory**.
2.  Oluşturulan **bağlı hizmetler**:
    1. Girdi verilerini tutan Azure Storage hesabınıza bağlamak için **Azure Storage** bağlı hizmeti.    
    2. Çıktı verilerini tutan Azure SQL veritabanınıza bağlamak için **Azure SQL** bağlı hizmeti. 
3.  İşlem hatları için girdi verilerini ve çıktı verilerini açıklayan oluşturulan **veri kümeleri**.
4.  Kaynak olarak **BlobSource**’u, havuz olarak da **SqlSink**’i kapsayan **Kopyalama Etkinliği**’ne sahip oluşturulan **işlem hattı**. 


## Data factory’leri görüntülemek için Sunucu Gezgini’ni kullanın

1. **Visual Studio**’nun menüsünde **Görünüm**’e ve **Sunucu Gezgini**’ne tıklayın.
2. Sunucu Gezgini penceresinde, **Azure**’ü ve **Data Factory**’yi genişletin. **Visual Studio'da oturum açın**’ı görürseniz Azure aboneliğiyle ilişkili **hesabı** girin ve **Devam**’a tıklayın. **parola** girip **Oturum aç**’a tıklayın. Visual Studio, aboneliğinizdeki tüm Azure data factory’leri hakkında bilgi almaya çalışır. Bu işlemin durumunu **Data Factoy Görev Listesi** penceresinde görürsünüz.
    ![Sunucu Gezgini](./media/data-factory-get-started-using-vs/server-explorer.png)
3. İstediğiniz data factory’ye sağ tıklayıp, mevcut bir data factory’ye dayandırılan Visual Studio projesi oluşturmak için Data Factory’yi Yeni Projeye Aktar’ı seçin.
    ![Data factory’yi VS projesine aktarma](./media/data-factory-get-started-using-vs/export-data-factory-menu.png)  

## Visual Studio için Data Factory araçlarını güncelleştirme
Visual Studio için Azure Data Factory araçlarını güncelleştirmek üzere şunları yapın:

1. Menüde **Araçlar**’a tıklayın ve **Uzantılar ve Güncelleştirmeler**’i seçin. 
2. Sol bölmede **Güncelleştirmeler**’i, sonra da **Visual Studio Galerisi**’ni seçin.
4. **Visual Studio için Azure Data Factory araçları**’nı seçin ve **Güncelleştir**’e tıklayın. Bu girişi görmüyorsanız araçların en son sürümü zaten yüklüdür. 

Bu öğreticide oluşturduğunuz işlem hattını ve veri kümelerini izlemek üzere Azure Portal’ın ilişkin yönergeler için bkz. [Veri kümelerini ve işlem hatlarını izleme](data-factory-get-started-using-editor.md#monitor-pipeline).

## Ayrıca Bkz.
| Konu | Açıklama |
| :---- | :---- |
| [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md) | Bu makalede, öğreticide kullandığınız Kopyalama Etkinliği hakkında ayrıntılı bilgi sağlanmaktadır. |
| [Zamanlama ve yürütme](data-factory-scheduling-and-execution.md) | Bu makalede Azure Data Factory uygulama modelinin zamanlama ve yürütme yönleri açıklanmaktadır. |
| [İşlem hatları](data-factory-create-pipelines.md) | Bu makalede Azure Data Factory’de işlem hatlarının ve etkinliklerin yanı sıra senaryonuz ya da işiniz için uçtan uca veri odaklı iş akışlarının nasıl desteklendiğini anlamanıza yardımcı olunmaktadır. |
| [Veri kümeleri](data-factory-create-datasets.md) | Bu makale Azure Data Factory’deki veri kümelerini anlamanıza yardımcı olacaktır.
| [İzleme Uygulaması kullanılarak işlem hatlarını izleme ve yönetme](data-factory-monitor-manage-app.md) | Bu makalede İzleme ve Yönetim Uygulaması kullanılarak işlem hatlarını izleme, yönetme ve hatalarını ayıklama işlemleri açıklanmaktadır. 



<!----HONumber=Jun16_HO2-->


