<properties
    pageTitle="İlk data factory’nizi derleme (Visual Studio) | Microsoft Azure"
    description="Bu öğreticide Visual Studio kullanarak örnek bir Azure Data Factory işlem hattı oluşturacaksınız."
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
    ms.topic="hero-article" 
    ms.date="05/16/2016"
    ms.author="spelluru"/>

# Microsoft Visual Studio kullanarak ilk Azure data factory’nizi derleme
> [AZURE.SELECTOR]
- [Öğreticiye Genel Bakış](data-factory-build-your-first-pipeline.md)
- [Data Factory Düzenleyici’yi kullanma](data-factory-build-your-first-pipeline-using-editor.md)
- [PowerShell’i kullanma](data-factory-build-your-first-pipeline-using-powershell.md)
- [Visual Studio’yu kullanma](data-factory-build-your-first-pipeline-using-vs.md)
- [Resource Manager Şablonunu kullanma](data-factory-build-your-first-pipeline-using-arm.md)


Bu makalede, ilk Azure data factory’nizi oluşturmak için Microsoft Visual Studio kullanmayı öğreneceksiniz. 

## Ön koşullar

1. Başka bir işlem yapmadan önce [Öğreticiye Genel Bakış](data-factory-build-your-first-pipeline.md) makalesinin tamamını okumanız ve önkoşul adımlarını tamamlamanız **gerekir**.
2. Bu makale, Azure Data Factory hizmetine kavramsal bir genel bakış sağlamaz. Hizmet hakkında ayrıntılı bir genel bakış için [Azure Data Factory'ye giriş](data-factory-introduction.md) makalesine gitmenizi öneririz.  

## Data Factory varlıklarını oluşturma ve dağıtma  

### Ön koşullar

Bilgisayarınızda şunların yüklü olması gerekir: 

- Visual Studio 2013 veya Visual Studio 2015
- Visual Studio 2013 veya Visual Studio 2015 için Azure SDK’sını indirin. [Azure İndirme Sayfası](https://azure.microsoft.com/downloads/)’na gidin ve **.NET** bölümündeki **VS 2013** veya **VS 2015**’e tıklayın.
- Visual Studio için en son Azure Data Factory eklentisini indirin: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) veya [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Visual Studio 2013 kullanıyorsanız, buradaki işlemleri yaparak eklentiyi güncelleştirebilirsiniz: Menüde **Araçlar** -> **Uzantılar ve Güncelleştirmeler** -> **Çevrimiçi** -> **Visual Studio Galerisi** -> **Visual Studio için Microsoft Azure Data Factory Araçları** -> **Güncelleştir**’e tıklayın. 
    
    

## Visual Studio projesi oluşturma 
1. **Visual Studio 2013** veya **Visual Studio 2015**’i başlatın. **Dosya**’ya tıklayın, **Yeni**’nin üzerine gelin ve **Proje**’ye tıklayın. **Yeni Proje** iletişim kutusu görmeniz gerekir.  
2. **Yeni Proje** iletişim kutusunda **DataFactory** şablonunu seçip **Boş Data Factory Projesi**’ne tıklayın.   

    ![Yeni proje iletişim kutusu](./media/data-factory-build-your-first-pipeline-using-vs/new-project-dialog.png)

3. Proje için bir **ad**, **konum** ve **çözüm** için ad girip **Tamam**’a tıklayın.

    ![Çözüm Gezgini](./media/data-factory-build-your-first-pipeline-using-vs/solution-explorer.png)

## Bağlı hizmetler oluşturma
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. İşlem hattında bir veya daha fazla etkinlik olabilir. Örneğin, verileri bir kaynaktan bir hedef veri deposuna kopyalamak için Kopyalama Etkinliği, girdi verilerini ürün çıktı verilerine dönüştürecek Hive betiğini çalıştırmak için de HDInsight Hive etkinliği. Data Factory çözümünüzü yayımladığınızda, daha sonra data factory için adı ve ayarları belirteceksiniz.

Bu adımda, Azure Storage hesabınızı ve isteğe bağlı Azure HDInsight kümesini data factory’nize bağlayacaksınız. Azure Storage hesabı bu örnekteki işlem hattı için girdi ve çıktı verilerini tutar. HDInsight bağlı hizmeti, bu örnekte işlem hattının etkinliğinde belirtilen Hive betiğini çalıştırmak için kullanılır. Senaryonuzda hangi veri deposu/işlem hizmetlerinin kullanılacağını belirtmek ve bağlı hizmetler oluşturarak bu hizmetleri veri fabrikanıza bağlamak için gereklidir.  

#### Azure Storage bağlı hizmeti oluşturma
Bu adımda, Azure Storage hesabınızı data factory’nize bağlayacaksınız. Bu öğreticide girdi/çıktı verilerin ve HQL betiğini depolamak için aynı Azure Storage hesabını kullanırsınız. 

4. Çözüm gezgininde **Bağlı Hizmetler**’e sağ tıklayın, **Ekle**’nin üzerine gelip **Yeni Öğe**’ye tıklayın.      
5. **Yeni Öğe Ekle** iletişim kutusunda **Azure Storage Bağlı Hizmeti**’ni listeden seçip **Ekle**’ye tıklayın. 
3. **accountname** ve **accountkey** sözcüklerini Azure depolama hesabınızın adı ve anahtarıyla değiştirin. Depolama erişim anahtarınızı nasıl alacağınız hakkında bilgi için bkz. [Depolama erişim anahtarlarını görüntüleme, kopyalama ve yeniden oluşturma](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)

    ![Azure Storage Bağlı Hizmeti](./media/data-factory-build-your-first-pipeline-using-vs/azure-storage-linked-service.png)

4. **AzureStorageLinkedService1.json** dosyasını kaydedin.

#### Azure HDInsight bağlı hizmeti oluşturma
Bu adımda, isteğe bağlı HDInsight kümesini data factory’nize bağlayacaksınız. HDInsight kümesi çalışma zamanında otomatik olarak oluşturulur ve işlenmesi bittiğinde ve belirtilen sürede boşta kalırsa silinir. İsteğe bağlı HDInsight kümesi yerine kendi HDInsight kümenizi kullanabilirsiniz. Ayrıntılar için bkz. [İşlem Bağlı Hizmetleri](data-factory-compute-linked-services.md). . 

1. **Çözüm Gezgini**’nde **Bağlı Hizmetler**’e sağ tıklayın, **Ekle**’nin üzerine gelip Y**eni Öğe**’ye tıklayın.
2. **İsteğe Bağlı HDInsight Bağlı Hizmeti**’ni seçin ve **Ekle**’ye tıklayın. 
3. **JSON**’u aşağıdakiyle değiştirin:

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "AzureStorageLinkedService1"
            }
          }
        }
    
    Aşağıdaki tabloda, kod parçacığında kullanılan JSON özellikleri için açıklamalar verilmektedir:
    
    Özellik | Açıklama
    -------- | -----------
    Sürüm | Oluşturulan HDInsight sürümünün 3.2 olması gerektiğini belirtir. 
    ClusterSize | Tek düğümlü HDInsight kümesi oluşturur. 
    TimeToLive | Silinmeden önce HDInsight kümesinin boşta kalma süresini belirtir.
    linkedServiceName | HDInsight tarafından oluşturulan günlükleri depolamak için kullanılacak depolama hesabını belirtir

    Şunlara dikkat edin: 
    
    - Data Factory **Windows tabanlı** bir HDInsight kümesini yukarıdaki JSON ile sizin için oluşturur. **Linux tabanlı** bir HDInsight kümesi de oluşturabilirsiniz. Ayrıntılar için bkz. [İsteğe Bağlı HDInsight Bağlı Hizmeti](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). 
    - İsteğe bağlı HDInsight kümesi yerine **kendi HDInsight kümenizi** kullanabilirsiniz. Ayrıntılar için bkz. [HDInsight Bağlı Hizmeti](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).
    - HDInsight kümesi JSON’da belirttiğiniz blob depolamada (**linkedServiceName**) bir **varsayılan kapsayıcı** oluşturur. HDInsight, küme silindiğinde bu kapsayıcıyı silmez. Bu tasarım gereğidir. İsteğe bağlı HDInsight bağlı hizmetiyle, HDInsight kümesi her oluşturulduğunda, burada mevcut canlı bir küme (**timeToLive**) olmadıkça bir dilim gerekir ve işlem bittiğinde silinir.
    
        Daha fazla dilim işlendikçe, Azure blob depolamanızda çok sayıda kapsayıcı göreceksiniz. İşlerin sorunları giderilmesi için bunlara gerek yoksa, depolama maliyetini azaltmak için bunları silmek isteyebilirsiniz. Bu kapsayıcıların adı şu deseni izler: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp". Azure blob depolamada kapsayıcı silmek için [Microsoft Storage Gezgini](http://storageexplorer.com/) gibi araçları kullanın.

    Ayrıntılar için bkz. [İsteğe Bağlı HDInsight Bağlı Hizmeti](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). 
4. **HDInsightOnDemandLinkedService1.json** dosyasını kaydedin.

## Veri kümeleri oluşturma
Bu adımda, Hive işlenmesi için girdi ve çıktı verilerini temsil edecek veri kümeleri oluşturacaksınız. Bu veri kümeleri, bu öğreticide daha önce oluşturduğunuz **AzureStorageLinkedService1** öğesine başvurur. Bağlı hizmet Azure Storage hesabını belirtirken, veri kümeleri de girdi ve çıktı verilerini tutan depolama biriminde kapsayıcı, klasör, dosya adı belirtir.   

#### Girdi veri kümesi oluşturma

1. **Çözüm Gezgini**’nde **Tablolar**’a sağ tıklayın, **Ekle**’nin üzerine gelip **Yeni Öğe**’ye tıklayın. 
2. Listeden **Azure Blob**’u seçin, dosya adını **InputDataSet.json** olarak değiştirin ve **Ekle**’ye tıklayın.
3. Düzenleyicideki **JSON**’u aşağıdakiyle değiştirin: 

    JSON parçacığında, işlem hattındaki etkinliğin girdi verilerini temsil eden **AzureBlobInput** adlı bir veri kümesi oluşturmaktasınız. Ek olarak, girdi verilerinin **adfgetstarted** adlı blob kapsayıcısında ve **inputdata** adlı klasörde bulunduğunu belirtin
        
        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService1",
                "typeProperties": {
                    "fileName": "input.log",
                    "folderPath": "adfgetstarted/inputdata",
                    "format": {
                        "type": "TextFormat",
                        "columnDelimiter": ","
                    }
                },
                "availability": {
                    "frequency": "Month",
                    "interval": 1
                },
                "external": true,
                "policy": {}
            }
        } 

    Aşağıdaki tabloda, kod parçacığında kullanılan JSON özellikleri için açıklamalar verilmektedir:

  	| Özellik | Açıklama |
  	| :------- | :---------- |
  	| type | Veriler Azure blob depolamada yer aldığından type özelliği AzureBlob olarak ayarlanmıştır. |  
  	| linkedServiceName | daha önce oluşturduğunuz AzureStorageLinkedService1’e başvurur. |
  	| fileName | Bu özellik isteğe bağlıdır. Bu özelliği atarsanız, tüm folderPath dosyaları alınır. Bu durumda, yalnızca input.log işlenir. |
  	| type | Günlük dosyaları metin biçiminde olduğundan TextFormat kullanacağız. | 
  	| columnDelimiter | Günlük dosyalarındaki sütunlar y (virgül) ile ayrılmıştır |
  	| frequency/interval | frequency Ay, interval de 1 olarak ayarlanmıştır; girdi dilimlerinin aylık olarak kullanılabileceğini belirtir. | 
  	| external | bu özellik, girdi verileri Data Factory hizmetiyle oluşturulmadıysa true olarak ayarlanır. | 
      
    
3. **InputDataset.json** dosyasını kaydedin. 

 
#### Çıktı veri kümesi oluşturma
Şimdi, Azure Blob depolamada depolanan çıktı verilerini göstermek için çıktı veri kümesi oluşturacaksınız. 

1. **Çözüm Gezgini**’nde **tablolar**’a sağ tıklayın, **Ekle**’nin üzerine gelip **Yeni Öğe**’ye tıklayın. 
2. Listeden **Azure Blob**’u seçin, dosya adını **OutputDataset.json** olarak değiştirin ve **Ekle**’ye tıklayın. 
3. Düzenleyicideki **JSON**’u aşağıdakiyle değiştirin: 

    JSON parçacığında, **AzureBlobOutput** adlı bir veri kümesi oluşturuyorsunuz ve Hive betiğinin oluşturacağı verilerin yapısını belirtiyorsunuz. Ek olarak, sonuçların **adfgetstarted** adlı blob kapsayıcısında ve **partitioneddata** adlı klasörde depolandığını belirtin. Burada, **availability** bölümü çıktı veri kümesinin aylık tabanda oluşturulduğunu belirtiyor.
    
        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService1",
            "typeProperties": {
              "folderPath": "adfgetstarted/partitioneddata",
              "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
              }
            },
            "availability": {
              "frequency": "Month",
              "interval": 1
            }
          }
        }

    Bu özelliklerin açıklamaları için **Girdi veri kümesi oluşturma** bölümüne bakın. Veri kümesi Data Factory hizmeti tarafından oluşturulduğundan çıktı veri kümesinde dış özellik ayarlamazsınız.

4. **OutputDataset.json** dosyasını kaydedin.


### İşlem hattı oluşturma
Bu adımda, **HDInsightHive** etkinliğiyle ilk işlem hattınızı oluşturacaksınız. Girdi diliminin ayda bir (frequency: Month, interval: 1) olarak kullanılabildiğini, çıktı diliminin ayda bir oluşturulduğunu ve etkinlik zamanlayıcı özelliğinin de ayda bir olarak ayarlandığını unutmayın (aşağıya bakın). Çıktı veri kümesi ve etkinlik zamanlayıcı ayarlarının eşleşmesi gerekir. Şu anda, çıktı veri kümesi zamanlamayı yönetendir; bu nedenle etkinlik hiçbir çıktı oluşturmasa bile sizin bir çıktı veri kümesi oluşturmanız gerekir. Etkinlik herhangi bir girdi almazsa, girdi veri kümesi oluşturma işlemini atlayabilirsiniz. Aşağıdaki JSON’da kullanılan özellikler bu bölümün sonunda anlatılmaktadır.

1. **Çözüm Gezgini**’nde **İşlem Hatları**’na sağ tıklayın, **Ekle**’nin üzerine gelip **Yeni Öğe**’ye tıklayın. 
2. Listeden **Hive Dönüşüm İşlem Hattı**’nı seçin ve **Ekle**’ye tıklayın. 
3. **JSON**’u aşağıdaki kod parçacığıyla değiştirin.

    > [AZURE.IMPORTANT] **storageaccountname**’i depolama adınızla değiştirin.

        {
            "name": "MyFirstPipeline",
            "properties": {
                "description": "My first Azure Data Factory pipeline",
                "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                            "scriptLinkedService": "AzureStorageLinkedService1",
                            "defines": {
                                "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                                "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                            }
                        },
                        "inputs": [
                            {
                                "name": "AzureBlobInput"
                            }
                        ],
                        "outputs": [
                            {
                                "name": "AzureBlobOutput"
                            }
                        ],
                        "policy": {
                            "concurrency": 1,
                            "retry": 3
                        },
                        "scheduler": {
                            "frequency": "Month",
                            "interval": 1
                        },
                        "name": "RunSampleHiveActivity",
                        "linkedServiceName": "HDInsightOnDemandLinkedService"
                    }
                ],
                "start": "2016-04-01T00:00:00Z",
                "end": "2016-04-02T00:00:00Z",
                "isPaused": false
            }
        }

    JSON parçacığında, HDInsight kümesinde Veri işleyecek Hive’ı kullanan etkinlikten oluşmuş bir işlem hattı oluşturuyorsunuz.
    
    JSON parçacığında, HDInsight kümesinde Veri işleyecek Hive’ı kullanan etkinlikten oluşmuş bir işlem hattı oluşturuyorsunuz.
    
    **partitionweblogs.hql** Hive betik dosyası Azure depolama hesabında (scriptLinkedService tarafından belirtilen **AzureStorageLinkedService1** adıyla) ve **adfgetstarted** kapsayıcısındaki **betik** klasöründe depolanır.

    Burada, **defines** bölümü hive betiğine Hive yapılandırma değerleri olarak (örn., ${hiveconf:inputtable}, ${hiveconf:partitionedtable}) geçirilecek çalışma zamanı ayarlarını belirtmek için kullanılır.

    İşlem hattının **start** ve **end** özellikleri işlem hattının etkin dönemini belirtir.

    JSON etkinliğinde, Hive betiğinin **linkedServiceName** – **HDInsightOnDemandLinkedService** tarafından belirtilen işlemde çalışacağını belirtirsiniz.

    > [AZURE.NOTE] Yukarıdaki örnekte kullanılan JSON özellikleri hakkında ayrıntılı bilgi için bkz. [İşlem hattı anatomisi](data-factory-create-pipelines.md#anatomy-of-a-pipeline). 
3. **HiveActivity1.json** dosyasını kaydedin.

### partitionweblogs.hql ve input.log dosyalarını bağımlılık olarak ekleme 

1. **Çözüm Gezgini** penceresinde **Bağımlılıklar**’a sağ tıklayın, **Ekle**’nin üzerine gelip **Mevcut Öğe**’ye tıklayın.  
2. **C:\ADFGettingStarted** yoluna gidin ve **partitionweblogs.hql**, **input.log** dosyalarını seçip **Ekle**’ye tıklayın. Bu iki dosyayı [Öğreticiye Genel Bakış](data-factory-build-your-first-pipeline.md)’taki ön koşulların bir parçası olarak oluşturmuştunuz.

Sonraki adımda çözümü yayımladığınızda, **partitionweblogs.hql** dosyası **adfgetstarted** blob kapsayıcısındaki betikler klasörüne yüklenir.   

### Data Factory varlıklarını yayımlama/dağıtma

18. Çözüm Gezgini’nde projeye sağ tıklayın ve ardından **Yayımla**’ya tıklayın. 
19. **Microsoft hesabınızda oturum açın** iletişim kutusunu görmezseniz, Azure aboneliğindeki kimlik bilgilerini hesap için girin ve **oturum aç**’a tıklayın.
20. Aşağıdaki iletişim kutusunu göreceksiniz:

    ![Yayımla iletişim kutusu](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)

21. Data factory yapılandırma sayfasında aşağıdakileri yapın: 
    1. **Yeni Data Factory Oluştur** seçeneğini seçin.
    2. **Ad** için **FirstDataFactoryUsingVS** girin. 
    
        > [AZURE.IMPORTANT] Azure Data Factory adı küresel olarak benzersiz olmalıdır. Yayımladığınızda **“FirstDataFactoryUsingVS” data factory adı yok** hatasını alırsanız adı değiştirin (örneğin, yournameFirstDataFactoryUsingVS). Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.
3. **Abonelik** alanı için doğru abonelik seçin. 4. oluşturulacak data factory için **kaynak grubu** seçin. 5. Data factory için **bölge** seçin. 6. **Öğeleri Yayımla** sayfasına geçmek için **İleri**’ye tıklayın. (Ad alanından çıkmak için, **İleri** düğmesi devre dışıysa **SEKME** tuşuna basın.) 
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

 
## İşlem hattını izleme

6. [Azure Portal](https://portal.azure.com/) oturumunu açın, şunları yapın:
    1. **Gözat**’a tıklayın ve **Veri fabrikaları**’nı seçin.
        ![Data factory’lere göz atma](./media/data-factory-build-your-first-pipeline-using-vs/browse-datafactories.png) 
    2. Data factory’ler listesinden **FirstDataFactoryUsingVS** öğesini seçin. 
7. Veri fabrikanızın giriş sayfasında penceresinde **Diyagram**’a tıklayın.
  
    ![Diyagram kutucuğu](./media/data-factory-build-your-first-pipeline-using-vs/diagram-tile.png)
7. Diyagram görünümünde, işlem hatlarına ve bu öğreticide kullanılan veri kümelerine bir genel bakış görürsünüz.
    
    ![Diyagram Görünümü](./media/data-factory-build-your-first-pipeline-using-vs/diagram-view-2.png) 
8. İşlem hattındaki tüm etkinlikleri görüntülemek için diyagramdaki işlem hattına sağ tıklayın ve Açık İşlem Hattı’na tıklayın. 

    ![İşlem hattı menüsünü açma](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-menu.png)
9. İşlem hattında HDInsightHive etkinliğini gördüğünüzü onaylayın. 
  
    ![İşlem hattı görünümünü açma](./media/data-factory-build-your-first-pipeline-using-vs/open-pipeline-view.png)

    Önceki görünüme dönmek için en üstteki içerik haritası menüsünde **Data factory**’ye tıklayın. 
10. **Diyagram Görünümü**’nde **AzureBlobInput** veri kümesine çift tıklayın. Dilimin **Hazır** durumunda olduğunu onaylayın. Dilimin Hazır durumda gösterilmesi birkaç dakika alabilir. Bir süre bekledikten sonra bu gerçekleşmiyorsa, girdi dosyasının (input.log) doğru kapsayıcıda (adfgetstarted) ve klasörde (inputdata) olup olmadığına lütfen bakın.

    ![Girdi dilimi hazır durumda](./media/data-factory-build-your-first-pipeline-using-vs/input-slice-ready.png)
11. **AzureBlobInput** dikey penceresini kapatmak için **X** işaretine tıklayın. 
12. **Diyagram Görünümü**’nde **AzureBlobOutput** veri kümesine çift tıklayın. Dilimin işlenmekte olduğunu göreceksiniz.

    ![Veri kümesi](./media/data-factory-build-your-first-pipeline-using-vs/dataset-blade.png)
9. İşlem tamamlandığında dilimi **Hazır** durumunda göreceksiniz.
    >[AZURE.IMPORTANT] İsteğe bağlı HDInsight kümesinin oluşturulması genellikle biraz zaman alır (yaklaşık 20 dakika).  

    ![Veri kümesi](./media/data-factory-build-your-first-pipeline-using-vs/dataset-slice-ready.png) 
    
10. Dilim **Hazır** durumunda olduğunda çıktı verileri için blob depolama alanınızın **adfgetstarted** kapsayıcısında **partitioneddata** klasörünü denetleyin.  
 
    ![çıktı verileri](./media/data-factory-build-your-first-pipeline-using-vs/three-ouptut-files.png)

Bu öğreticide oluşturduğunuz işlem hattını ve veri kümelerini izlemek üzere Azure Portal’ın ilişkin yönergeler için bkz. [Veri kümelerini ve işlem hatlarını izleme](data-factory-monitor-manage-pipelines.md).

Veri işlem hatlarınızı izlemek için İzleme ve Yönetme Uygulaması’nı da kullanabilirsiniz. Uygulamanın kullanımına ilişkin ayrıntılar için bkz. [İzleme Uygulaması kullanarak Azure Data Factory işlem hatlarını izleme ve yönetme](data-factory-monitor-manage-app.md). 

> [AZURE.IMPORTANT] Dilim başarıyla işlendiğinde girdi dosyası silinir. Bu nedenle, dilimi yeniden çalıştırmak veya öğreticiyi yeniden uygulamak isterseniz girdi dosyasını (input.log) adfgetstarted kapsayıcısının inputdata klasörüne yükleyin.
 

## Data factory’leri görüntülemek için Sunucu Gezgini’ni kullanın

1. **Visual Studio**’nun menüsünde **Görünüm**’e ve **Sunucu Gezgini**’ne tıklayın.
2. Sunucu Gezgini penceresinde, **Azure**’ü ve **Data Factory**’yi genişletin. **Visual Studio'da oturum açın**’ı görürseniz Azure aboneliğiyle ilişkili **hesabı** girin ve **Devam**’a tıklayın. **parola** girip **Oturum aç**’a tıklayın. Visual Studio, aboneliğinizdeki tüm Azure data factory’leri hakkında bilgi almaya çalışır. Bu işlemin durumunu **Data Factoy Görev Listesi** penceresinde görürsünüz.

    ![Sunucu Gezgini](./media/data-factory-build-your-first-pipeline-using-vs/server-explorer.png)
3. İstediğiniz data factory’ye sağ tıklayıp, mevcut bir data factory’ye dayandırılan Visual Studio projesi oluşturmak için **Data Factory’yi Yeni Projeye Aktar**’ı seçin.

    ![Data factory’yi dışarı aktarma](./media/data-factory-build-your-first-pipeline-using-vs/export-data-factory-menu.png)

## Visual Studio için Data Factory araçlarını güncelleştirme

Visual Studio için Azure Data Factory araçlarını güncelleştirmek üzere şunları yapın:

1. Menüde **Araçlar**’a tıklayın ve **Uzantılar ve Güncelleştirmeler**’i seçin.
2. Sol bölmede **Güncelleştirmeler**’i, sonra da **Visual Studio Galerisi**’ni seçin.
3. **Visual Studio için Azure Data Factory araçları**’nı seçin ve **Güncelleştir**’e tıklayın. Bu girişi görmüyorsanız araçların en son sürümü zaten yüklüdür. 

## Yapılandırma dosyalarını kullanma
Visual Studio'da bağlı hizmetler/tablolar/işlem hatları için her ortamda farklı özellik yapılandırmak amacıyla yapılandırma dosyalarını kullanabilirsiniz. 

Azure Storage bağlı hizmeti için aşağıdaki JSON tanımını dikkate alın. Data Factory varlıklarını dağıttığınız ortama dayanan accountname ve accountkey farklı değerlerini (Dev/Test/Production) **connectionString** olarak belirtmek için. Her ortam için ayrı yapılandırma dosyası kullanarak bunu yapabilirsiniz. 

    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "description": "",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    } 

### Yapılandırma dosyası ekleme
Aşağıdaki adımları uygulayarak her ortam için bir yapılandırma dosyası ekleyin:   

1. Visual Studio çözümünüzde Data Factory projenize sağ tıklayın, **Ekle**’nin üzerine gelin ve **Yeni öğe**’ye tıklayın.
2. Soldaki yüklenmiş şablonlar listesinden **Config**’i ve **Yapılandırma Dosyası**’nı seçin ve yapılandırma dosyası için bir **ad** girip **Ekle**’ye tıklayın.

    ![Yapılandırma dosyasını ekleme](./media/data-factory-build-your-first-pipeline-using-vs/add-config-file.png)
3. Aşağıda gösterilen biçimde yapılandırma parametrelerini ve değerlerini ekleyin:

        {
            "$schema": "http://datafactories.schema.management.azure.com/vsschemas/V1/Microsoft.DataFactory.Config.json",
            "AzureStorageLinkedService1": [
                {
                    "name": "$.properties.typeProperties.connectionString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            ],
            "AzureSqlLinkedService1": [
                {
                    "name": "$.properties.typeProperties.connectionString",
                    "value":  "Server=tcp:spsqlserver.database.windows.net,1433;Database=spsqldb;User ID=spelluru;Password=Sowmya123;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
                }
            ]
        }

    Bu örnek, Azure Storage bağlı hizmetinin ve Azure SQL bağlı hizmetinin connectionString özelliğini yapılandırır. Belirtilen adın sözdiziminin [JsonPath](http://goessner.net/articles/JsonPath/) olduğuna dikkat edin.   

    JSON’da aşağıda gösterildiği gibi bir dizi değere sahip özellik varsa:  

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
    
    Yapılandırma dosyasında aşağıdaki gibi yapılandırmalısınız (sıfır tabanlı dizin kullanılmış): 
        
        {
            "name": "$.properties.structure[0].name",
            "value": "FirstName"
        }
        {
            "name": "$.properties.structure[0].type",
            "value": "String"
        }
        {
            "name": "$.properties.structure[1].name",
            "value": "LastName"
        }
        {
            "name": "$.properties.structure[1].type",
            "value": "String"
        }

### Boşluklu özellik adları
Özellik adında boşluklar varsa, aşağıdaki örnekte (veritabanı sunucu adı) gösterildiği gibi köşeli ayraç kullanın: 

     {
         "name": "$.properties.activities[1].typeProperties.webServiceParameters.['Database server name']",
         "value": "MyAsqlServer.database.windows.net"
     }


### Yapılandırma kullanarak çözümü dağıtma
Azure Data Factory varlıklarını VS’de dağıttığınızda, bu yayımlama işlemi için kullanmak istediğiniz yapılandırmayı belirtebilirsiniz. 

Azure Data Factory projesindeki varlıkları yapılandırma dosyası kullanarak oluşturmak için:   

1. Data Factory projesine sağ tıklayıp, **Öğeleri Yayımla** iletişim kutusunu görüntülemek için **Yayımla**’ya tıklayın. 
2. Mevcut bir data factory seçip ya da **Data factory yapılandır** sayfasında yeni data factory oluşturulması için değerleri belirtip **İleri**’ye tıklayın.   
3. **Öğeleri Yayımla** sayfasında: **Dağıtım Yapılandırması seç** alanı için kullanılabilir yapılandırmaların açılan listesini görürsünüz.

    ![Yapılandırma dosyası seçme](./media/data-factory-build-your-first-pipeline-using-vs/select-config-file.png)

4. Kullanmak istediğiniz **yapılandırma dosyasını** seçip **İleri**’ye tıklayın. 
5. **Özet** sayfasında JSON dosyasının adını gördüğünüzü doğrulayıp **İleri**’ye tıklayın. 
6. Dağıtım işlemi sona erdikten sonra **Son**’a tıklayın. 

Dağıttığınızda, yapılandırma dosyasına ait değerler, varlıklar Azure Data Factory hizmetine dağıtılmadan önce Data Factory varlıkları (bağlı hizmetler, tablolar veya işlem hatları) için JSON dosyalarında özelliklere değer ayarlamak için kullanılır.   

## Özet 
Bu öğreticide, HDInsight hadoop kümesindeki Hive betiği çalıştırılarak verileri işlemek için bir Azure data factory oluşturdunuz. Aşağıdaki adımları uygulamak için Azure Portal’da Data Factory Düzenleyici’yi kullandınız:  

1.  Oluşturulan Azure **data factory**.
2.  Oluşturulan iki **bağlı hizmet**:
    1.  Girdi/çıktı dosyalarını tutan Azure blob depolamanızı data factory’ye bağlamak için **Azure Storage** bağlı hizmeti.
    2.  İsteğe bağlı HDInsight Hadoop kümesini data factory’ye bağlamak için isteğe bağlı **Azure HDInsight** bağlı hizmeti. Azure Data Factory, girdi verilerini işlemek, çıktı verilerini de oluşturmak için tam zamanında HDInsight Hadoop kümesi oluşturur. 
3.  İşlem hattındaki HDInsight Hive etkinliğiyle ilgili girdi ve çıktı verilerini açıklayan oluşturulan iki **veri kümesi**. 
4.  **HDInsight Hive** etkinliğine sahip oluşturulan bir **işlem hattı**.  


## Sonraki Adımlar
Bu makalede, isteğe bağlı HDInsight kümesinde bir Hive betiği çalıştıran dönüştürme etkinliğine (HDInsight Etkinliği) sahip işlem hattı oluşturdunuz. Verileri Azure Blob’tan Azure SQL’e kopyalamak için Kopyalama Etkinliği’nin kullanılması hakkında bilgi için bkz. [Öğretici: Verileri Azure blob’tan Azure SQL’e kopyalama](data-factory-get-started.md).
  
## Ayrıca Bkz.
| Konu | Açıklama |
| :---- | :---- |
| [Veri Dönüştürme Etkinlikleri](data-factory-data-transformation-activities.md) | Bu makalede, Azure Data Factory’nin desteklediği veri dönüştürme etkinliklerinin (bu öğreticide kullandığınız HDInsight Hive dönüştürmesi gibi) bir listesi sağlanmaktadır. | 
| [Zamanlama ve yürütme](data-factory-scheduling-and-execution.md) | Bu makalede Azure Data Factory uygulama modelinin zamanlama ve yürütme yönleri açıklanmaktadır. |
| [İşlem hatları](data-factory-create-pipelines.md) | Bu makalede Azure Data Factory’de işlem hatlarının ve etkinliklerin yanı sıra senaryonuz ya da işiniz için uçtan uca veri odaklı iş akışlarının nasıl desteklendiğini anlamanıza yardımcı olunmaktadır. |
| [Veri kümeleri](data-factory-create-datasets.md) | Bu makale Azure Data Factory’deki veri kümelerini anlamanıza yardımcı olacaktır.
| [İzleme Uygulaması kullanılarak işlem hatlarını izleme ve yönetme](data-factory-monitor-manage-app.md) | Bu makalede İzleme ve Yönetim Uygulaması kullanılarak işlem hatlarını izleme, yönetme ve hatalarını ayıklama işlemleri açıklanmaktadır. 


<!--HONumber=Jun16_HO2-->


