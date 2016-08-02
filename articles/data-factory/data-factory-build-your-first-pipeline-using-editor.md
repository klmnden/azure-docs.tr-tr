<properties
    pageTitle="İlk data factory’nizi derleme (Azure Portal) | Microsoft Azure"
    description="Bu öğreticide, Azure Portal'daki Data Factory Düzenleyiciyi kullanarak örnek bir Azure Data Factory işlem hattı oluşturacaksınız."
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

# İlk Azure data factory’nizi Azure Portal/Data Factory Düzenleyici kullanarak derleme
> [AZURE.SELECTOR]
- [Öğreticiye Genel Bakış](data-factory-build-your-first-pipeline.md)
- [Data Factory Düzenleyici’yi kullanma](data-factory-build-your-first-pipeline-using-editor.md)
- [PowerShell’i kullanma](data-factory-build-your-first-pipeline-using-powershell.md)
- [Visual Studio’yu kullanma](data-factory-build-your-first-pipeline-using-vs.md)
- [Resource Manager Şablonunu kullanma](data-factory-build-your-first-pipeline-using-arm.md)

Bu makalede, ilk Azure data factory’nizi oluşturmak için [Azure Portal](https://portal.azure.com/) kullanmayı öğreneceksiniz. 

## Ön koşullar

1. Başka bir işlem yapmadan önce [Öğreticiye Genel Bakış](data-factory-build-your-first-pipeline.md) makalesinin tamamını okumanız ve önkoşul adımlarını tamamlamanız **gerekir**.
2. Bu makale, Azure Data Factory hizmetine kavramsal bir genel bakış sağlamaz. Hizmet hakkında ayrıntılı bir genel bakış için [Azure Data Factory'ye giriş](data-factory-introduction.md) makalesine gitmenizi öneririz.  

## Veri fabrikası oluşturma
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. İşlem hattında bir veya daha fazla etkinlik olabilir. Örneğin, verileri bir kaynaktan bir hedef veri deposuna kopyalamak için Kopyalama Etkinliği, girdi verilerini ürün çıktı verilerine dönüştürecek Hive betiğini çalıştırmak için de HDInsight Hive etkinliği. Bu adımda data factory oluşturmayla başlayalım. 

1.  [Azure Portal](https://portal.azure.com/) oturumunu açtıktan sonra şunları yapın:
    1.  Sol menüde **YENİ**’ye tıklayın. 
    2.  **Oluştur** dikey penceresinde **Veri analizi**’ne tıklayın.
    3.  **Veri analizi** dikey penceresinde **Data Factory**’ye tıklayın.

        ![Dikey pencere oluşturma](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)

2.  **Yeni data factory** dikey penceresinde, Ad olarak **GetStartedDF** girin.

    ![Yeni veri fabrikası dikey penceresi](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

    > [AZURE.IMPORTANT] Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. Şu hatayı alırsanız: ** “GetStartedDF” data factory adı yok**, data factory adını değiştirin (örneğin, yournameGetStartedDF) ve oluşturmayı yeniden deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.
    > 
    > Data factory adı gelecekte bir DNS adı olarak kaydedilmiş olabilir; bu nedenle herkese görünür hale gelmiştir.
    > 
    > Data Factory örnekleri oluşturmak için, Azure aboneliğinde katılımcı/yönetici rolünüz olmalıdır


3.  Data factory’yi oluşturmak istediğiniz **Azure aboneliği**’ni seçin. 
4.  Mevcut bir **kaynak grubu** seçin ya da yeni bir kaynak grubu oluşturun. Öğretici amacı için, şu adla bir kaynak grubu oluşturun: **ADFGetStartedRG**.    
5.  **Yeni data factory** dikey penceresinde **Oluştur**’a tıklayın.
6.  Oluşturulmakta olan data factory’yi Azure Portal’ın **Başlangıç panosu**’nda şöyle görürsünüz:   

    ![Data factory durumu oluşturma](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
7. Tebrikler! İlk data factory’nizi başarıyla oluşturdunuz. Data factory sorunsuz oluşturulduktan sonra data factory sayfasını görürsünüz, burada size data factory içeriği gösterilir.  

    ![Data Factory dikey penceresi](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

İşlem hattı oluşturmadan önce, öncelikle birkaç Data Factory varlığı oluşturmanız gerekir. Veri depolarını/işlemlerini veri deponuza bağlamak için önce bağlı hizmetleri oluşturun, bağlı veri depolarında veriyi tanıtmak için girdi ve çıktı veri kümelerini tanımlayın, sonra da bu veri kümelerini kullanan etkinliğin bulunduğu işlem hattını oluşturun. 

## Bağlı hizmetler oluşturma
Bu adımda, Azure Storage hesabınızı ve isteğe bağlı Azure HDInsight kümesini data factory’nize bağlayacaksınız. Azure Storage hesabı bu örnekteki işlem hattı için girdi ve çıktı verilerini tutar. HDInsight bağlı hizmeti, bu örnekte işlem hattının etkinliğinde belirtilen Hive betiğini çalıştırmak için kullanılır. Senaryonuzda hangi veri deposu/işlem hizmetlerinin kullanılacağını belirtmek ve bağlı hizmetler oluşturarak bu hizmetleri veri fabrikanıza bağlamak için gereklidir.  

### Azure Storage bağlı hizmeti oluşturma
Bu adımda, Azure Storage hesabınızı data factory’nize bağlayacaksınız. Bu öğreticide girdi/çıktı verilerin ve HQL betiğini depolamak için aynı Azure Storage hesabını kullanırsınız. 

1.  **GetStartedDF** için **DATA FACTORY** dikey penceresinde **Geliştir ve dağıt**’a tıklayın. Böylece, Data Factory Düzenleyici başlatılır. 
     
    ![Geliştir ve dağıt kutucuğu](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2.  **Yeni data store**’a tıklayın ve **Azure depolama**’yı seçin.
    
    ![Azure Storage bağlı hizmeti](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)

    Düzenleyicide Azure Storage bağlı hizmeti oluşturmak için JSON betiğini görmeniz gerekir. 
4. **accountname** sözcüğünü Azure depolama hesabınızın adıyla, **accountkey** sözcüğünü de Azure depolama hesabının erişim anahtarıyla değiştirin. Depolama erişim anahtarınızı nasıl alacağınız hakkında bilgi için bkz. [Depolama erişim anahtarlarını görüntüleme, kopyalama ve yeniden oluşturma](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)
5. Bağlı hizmeti dağıtmak için komut çubuğunda **Dağıt**’a tıklayın.

    ![Dağıt düğmesi](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   Bağlı hizmet sorunsuz dağıtıldıktan sonra **Taslak-1** penceresi artık görünmemelidir; soldaki ağaç görünümünde **AzureStorageLinkedService** göreceksiniz. 
    ![Menüde Storage Bağlı Hizmeti](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)   

 
### Azure HDInsight bağlı hizmeti oluşturma
Bu adımda, isteğe bağlı HDInsight kümesini data factory’nize bağlayacaksınız. HDInsight kümesi çalışma zamanında otomatik olarak oluşturulur ve işlenmesi bittiğinde ve belirtilen sürede boşta kalırsa silinir. 

1. **Data Factory Düzenleyici**’de komut çubuğundaki **Yeni işlem**’e tıklayıp **İsteğe bağlı HDInsight kümesi**’ni seçin.

    ![Yeni işlem](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. Aşağıdaki kod parçacığını kopyalayıp **Taslak-1** penceresine yapıştırın. JSON parçacığı, istek üzerine HDInsight kümesi oluşturmak için kullanılan özellikleri tanımlar. 

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "AzureStorageLinkedService"
            }
          }
        }
    
    Aşağıdaki tabloda, kod parçacığında kullanılan JSON özellikleri için açıklamalar verilmektedir:
    
  	| Özellik | Açıklama |
  	| :------- | :---------- |
  	| Sürüm | Oluşturulan HDInsight sürümünün 3.2 olması gerektiğini belirtir. | 
  	| ClusterSize | Tek düğümlü HDInsight kümesi oluşturur. | 
  	| TimeToLive | Silinmeden önce HDInsight kümesinin boşta kalma süresini belirtir. |
  	| linkedServiceName | HDInsight tarafından oluşturulan günlükleri depolamak için kullanılacak depolama hesabını belirtir. |

    Şunlara dikkat edin: 
    
    - Data Factory **Windows tabanlı** bir HDInsight kümesini yukarıdaki JSON ile sizin için oluşturur. **Linux tabanlı** bir HDInsight kümesi de oluşturabilirsiniz. Ayrıntılar için bkz. [İsteğe Bağlı HDInsight Bağlı Hizmeti](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). 
    - İsteğe bağlı HDInsight kümesi yerine **kendi HDInsight kümenizi** kullanabilirsiniz. Ayrıntılar için bkz. [HDInsight Bağlı Hizmeti](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).
    - HDInsight kümesi JSON’da belirttiğiniz blob depolamada (**linkedServiceName**) bir **varsayılan kapsayıcı** oluşturur. HDInsight, küme silindiğinde bu kapsayıcıyı silmez. Bu tasarım gereğidir. İsteğe bağlı HDInsight bağlı hizmetiyle, HDInsight kümesi her oluşturulduğunda, burada mevcut canlı bir küme (**timeToLive**) olmadıkça bir dilim gerekir ve işlem bittiğinde silinir.
    
        Daha fazla dilim işlendikçe, Azure blob depolamanızda çok sayıda kapsayıcı göreceksiniz. İşlerin sorunları giderilmesi için bunlara gerek yoksa, depolama maliyetini azaltmak için bunları silmek isteyebilirsiniz. Bu kapsayıcıların adı şu deseni izler: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp". Azure blob depolamada kapsayıcı silmek için [Microsoft Storage Gezgini](http://storageexplorer.com/) gibi araçları kullanın.

    Ayrıntılar için bkz. [İsteğe Bağlı HDInsight Bağlı Hizmeti](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).
3. Bağlı hizmeti dağıtmak için komut çubuğunda **Dağıt**’a tıklayın. 
4. Hem **AzureStorageLinkedService**, hem de **HDInsightOnDemandLinkedService** öğesinin soldaki ağaç görünümünde olduğunu onaylayın.

    ![Bağlı hizmetlerin bulunduğu ağaç görünümü](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## Veri kümeleri oluşturma
Bu adımda, Hive işlenmesi için girdi ve çıktı verilerini temsil edecek veri kümeleri oluşturacaksınız. Bu veri kümeleri, bu öğreticide daha önce oluşturduğunuz **AzureStorageLinkedService** öğesine başvurur. Bağlı hizmet Azure Storage hesabını belirtirken, veri kümeleri de girdi ve çıktı verilerini tutan depolama biriminde kapsayıcı, klasör, dosya adı belirtir.   

### Girdi veri kümesi oluşturma

1. **Data Factory Düzenleyici**’de komut çubuğundaki **Yeni veri kümesi**’ne tıklayın ve **Azure Blob depolama**’yı seçin.

    ![Yeni veri kümesi](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. Aşağıdaki kod parçacığını kopyalayıp Taslak-1 penceresine yapıştırın. JSON parçacığında, işlem hattındaki etkinliğin girdi verilerini temsil eden **AzureBlobInput** adlı bir veri kümesi oluşturmaktasınız. Ek olarak, girdi verilerinin **adfgetstarted** adlı blob kapsayıcısında ve **inputdata** adlı klasörde bulunduğunu belirtin.
        
        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "AzureStorageLinkedService",
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
  	| linkedServiceName | daha önce oluşturduğunuz AzureStorageLinkedService’e başvurur. |
  	| fileName | Bu özellik isteğe bağlıdır. Bu özelliği atarsanız, tüm folderPath dosyaları alınır. Bu durumda, yalnızca input.log işlenir. |
  	| type | Günlük dosyaları metin biçiminde olduğundan TextFormat kullanacağız. | 
  	| columnDelimiter | Günlük dosyalarındaki sütunlar y (virgül) ile ayrılmıştır |
  	| frequency/interval | frequency Ay, interval de 1 olarak ayarlanmıştır; girdi dilimlerinin aylık olarak kullanılabileceğini belirtir. | 
  	| external | bu özellik, girdi verileri Data Factory hizmetiyle oluşturulmadıysa true olarak ayarlanır. | 
      
    
3. Yeni oluşturulan veri kümesini dağıtmak için komut çubuğunda **Dağıt**’a tıklayın. Veri kümesini soldaki ağaç görünümünde görmeniz gerekir. 


### Çıktı veri kümesi oluşturma
Şimdi, Azure Blob depolamada depolanan çıktı verilerini göstermek için çıktı veri kümesi oluşturacaksınız. 

1. **Data Factory Düzenleyici**’de komut çubuğundaki **Yeni veri kümesi**’ne tıklayın ve **Azure Blob depolama**’yı seçin.  
2. Aşağıdaki kod parçacığını kopyalayıp Taslak-1 penceresine yapıştırın. JSON parçacığında, **AzureBlobOutput** adlı bir veri kümesi oluşturuyorsunuz ve Hive betiğinin oluşturacağı verilerin yapısını belirtiyorsunuz. Ek olarak, sonuçların **adfgetstarted** adlı blob kapsayıcısında ve **partitioneddata** adlı klasörde depolandığını belirtin. Burada, **availability** bölümü çıktı veri kümesinin aylık tabanda oluşturulduğunu belirtiyor.
    
        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
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
3. Yeni oluşturulan veri kümesini dağıtmak için komut çubuğunda **Dağıt**’a tıklayın.
4. Veri kümesinin başarıyla oluşturulduğunu doğrulayın.

    ![Bağlı hizmetlerin bulunduğu ağaç görünümü](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## İşlem hattı oluşturma
Bu adımda, **HDInsightHive** etkinliğiyle ilk işlem hattınızı oluşturacaksınız. Girdi diliminin ayda bir (frequency: Month, interval: 1) olarak kullanılabildiğini, çıktı diliminin ayda bir oluşturulduğunu ve etkinlik zamanlayıcı özelliğinin de ayda bir olarak ayarlandığını unutmayın (aşağıya bakın). Çıktı veri kümesi ve etkinlik zamanlayıcı ayarlarının eşleşmesi gerekir. Şu anda, çıktı veri kümesi zamanlamayı yönetendir; bu nedenle etkinlik hiçbir çıktı oluşturmasa bile sizin bir çıktı veri kümesi oluşturmanız gerekir. Etkinlik herhangi bir girdi almazsa, girdi veri kümesi oluşturma işlemini atlayabilirsiniz. Aşağıdaki JSON’da kullanılan özellikler bu bölümün sonunda anlatılmaktadır. 

1. **Data Factory Düzenleyici**’de, **Üç nokta (…) Daha fazla komut**’ sonra da **Yeni işlem hattı**’na tıklayın.
    
    ![yeni işlem hattı düğmesi](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. Aşağıdaki kod parçacığını kopyalayıp Taslak-1 penceresine yapıştırın.

    > [AZURE.IMPORTANT] **storageaccountname**’i JSON’daki depolama adınızla değiştirin.
        
        {
            "name": "MyFirstPipeline",
            "properties": {
                "description": "My first Azure Data Factory pipeline",
                "activities": [
                    {
                        "type": "HDInsightHive",
                        "typeProperties": {
                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                            "scriptLinkedService": "AzureStorageLinkedService",
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
    
    **partitionweblogs.hql** Hive betik dosyası Azure depolama hesabında (scriptLinkedService tarafından belirtilen **AzureStorageLinkedService** adıyla) ve **adfgetstarted** kapsayıcısındaki **betik** klasöründe depolanır.

    Burada, **defines** bölümü hive betiğine Hive yapılandırma değerleri olarak (örn., ${hiveconf:inputtable}, ${hiveconf:partitionedtable}) geçirilecek çalışma zamanı ayarlarını belirtmek için kullanılır.

    İşlem hattının **start** ve **end** özellikleri işlem hattının etkin dönemini belirtir.

    JSON etkinliğinde, Hive betiğinin **linkedServiceName** – **HDInsightOnDemandLinkedService** tarafından belirtilen işlemde çalışacağını belirtirsiniz.

    > [AZURE.NOTE] Yukarıdaki örnekte kullanılan JSON özellikleri hakkında ayrıntılı bilgi için bkz. [İşlem hattı anatomisi](data-factory-create-pipelines.md#anatomy-of-a-pipeline). 

3. Şunları onaylayın: 
    1. Azure blob depolamada **adfgetstarted** kapsayıcısının **inputdata** klasöründe **input.log** dosyasının olduğunu
    2. Azure blob depolamada **adfgetstarted** kapsayıcısının **script** klasöründe **partitionweblogs.hql** dosyasının olduğunu. Bu dosyaları görmüyorsanız lütfen [Öğreticiye Genel Bakış](data-factory-build-your-first-pipeline.md)’taki önkoşul adımları tamamlayın. 
    3. **storageaccountname**’i JSON işlem hattındaki depolama adınızla değiştirdiğinizi onaylayın. 
2. İşlem hattını dağıtmak için komut çubuğunda **Dağıt**’a tıklayın. **start** ve **end** zamanları geçmişe ayarlanmış ve **isPaused** yanlış olarak ayarlanmış olduğundan işlem hattı (işlem hattında etkinlik) dağıtıldıktan hemen sonra çalışır. 
4. İşlem hattını ağaç görünümünde gördüğünüzü onaylayın.

    ![İşlem hattının bulunduğu ağaç görünümü](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
5. Tebrikler, ilk işlem hattınızı başarıyla oluşturdunuz.

## İşlem hattını izleme

6. Data Factory Düzenleyici dikey penceresini kapatmak ve Data Factory dikey penceresine dönmek için **X** işaretine, sonra da **Diyagram**’a tıklayın.
  
    ![Diyagram kutucuğu](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
7. Diyagram görünümünde, işlem hatlarına ve bu öğreticide kullanılan veri kümelerine bir genel bakış görürsünüz.
    
    ![Diyagram Görünümü](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png) 
8. İşlem hattındaki tüm etkinlikleri görüntülemek için diyagramdaki işlem hattına sağ tıklayın ve Açık İşlem Hattı’na tıklayın. 

    ![İşlem hattı menüsünü açma](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
9. İşlem hattında HDInsightHive etkinliğini gördüğünüzü onaylayın. 
  
    ![İşlem hattı görünümünü açma](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    Önceki görünüme dönmek için en üstteki içerik haritası menüsünde **Data factory**’ye tıklayın. 
10. **Diyagram Görünümü**’nde **AzureBlobInput** veri kümesine çift tıklayın. Dilimin **Hazır** durumunda olduğunu onaylayın. Dilimin Hazır durumda gösterilmesi birkaç dakika alabilir. Bir süre bekledikten sonra bu gerçekleşmiyorsa, girdi dosyasının (input.log) doğru kapsayıcıda (adfgetstarted) ve klasörde (inputdata) olup olmadığına lütfen bakın.

    ![Girdi dilimi hazır durumda](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
11. **AzureBlobInput** dikey penceresini kapatmak için **X** işaretine tıklayın. 
12. **Diyagram Görünümü**’nde **AzureBlobOutput** veri kümesine çift tıklayın. Dilimin işlenmekte olduğunu göreceksiniz.

    ![Veri kümesi](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
9. İşlem tamamlandığında dilimi **Hazır** durumunda göreceksiniz.
    >[AZURE.IMPORTANT] İsteğe bağlı HDInsight kümesinin oluşturulması genellikle biraz zaman alır (yaklaşık 20 dakika).  

    ![Veri kümesi](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png) 
    
10. Dilim **Hazır** durumunda olduğunda çıktı verileri için blob depolama alanınızın **adfgetstarted** kapsayıcısında **partitioneddata** klasörünü denetleyin.  
 
    ![çıktı verileri](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)

Daha fazla ayrıntı için [Azure portal dikey penceresi kullanılarak işlem hatlarını izleme ve yönetme](data-factory-monitor-manage-pipelines.md) makalesine bakın. 

> [AZURE.IMPORTANT] Dilim başarıyla işlendiğinde girdi dosyası silinir. Bu nedenle, dilimi yeniden çalıştırmak veya öğreticiyi yeniden uygulamak isterseniz girdi dosyasını (input.log) adfgetstarted kapsayıcısının inputdata klasörüne yükleyin.

## Özet 
Bu öğreticide, HDInsight hadoop kümesindeki Hive betiği çalıştırılarak verileri işlemek için bir Azure data factory oluşturdunuz. Aşağıdaki adımları uygulamak için Azure Portal’da Data Factory Düzenleyici’yi kullandınız:  

1.  Oluşturulan Azure **data factory**.
2.  Oluşturulan iki **bağlı hizmet**:
    1.  Girdi/çıktı dosyalarını tutan Azure blob depolamanızı data factory’ye bağlamak için **Azure Storage** bağlı hizmeti.
    2.  İsteğe bağlı HDInsight Hadoop kümesini data factory’ye bağlamak için isteğe bağlı **Azure HDInsight** bağlı hizmeti. Azure Data Factory, girdi verilerini işlemek, çıktı verilerini de oluşturmak için tam zamanında HDInsight Hadoop kümesi oluşturur. 
3.  İşlem hattındaki HDInsight Hive etkinliğiyle ilgili girdi ve çıktı verilerini açıklayan oluşturulan iki **veri kümesi**. 
4.  **HDInsight Hive** etkinliğine sahip oluşturulan bir **işlem hattı**. 

## Sonraki Adımlar
Bu makalede, isteğe bağlı HDInsight kümesinde bir Hive betiği çalıştıran dönüştürme etkinliğine (HDInsight Etkinliği) sahip işlem hattı oluşturdunuz. Verileri Azure Blob’tan Azure SQL’e kopyalamak için Kopyalama Etkinliği’nin kullanılması hakkında bilgi için bkz. [Öğretici: Verileri Azure blob’tan Azure SQL’e kopyalama](./data-factory-get-started.md).

## Ayrıca Bkz.
| Konu | Açıklama |
| :---- | :---- |
| [Veri Dönüştürme Etkinlikleri](data-factory-data-transformation-activities.md) | Bu makalede, Azure Data Factory’nin desteklediği veri dönüştürme etkinliklerinin (bu öğreticide kullandığınız HDInsight Hive dönüştürmesi gibi) bir listesi sağlanmaktadır. | 
| [Zamanlama ve yürütme](data-factory-scheduling-and-execution.md) | Bu makalede Azure Data Factory uygulama modelinin zamanlama ve yürütme yönleri açıklanmaktadır. |
| [İşlem hatları](data-factory-create-pipelines.md) | Bu makalede Azure Data Factory’de işlem hatlarının ve etkinliklerin yanı sıra senaryonuz ya da işiniz için uçtan uca veri odaklı iş akışlarının nasıl desteklendiğini anlamanıza yardımcı olunmaktadır. |
| [Veri kümeleri](data-factory-create-datasets.md) | Bu makale Azure Data Factory’deki veri kümelerini anlamanıza yardımcı olacaktır.
| [İzleme Uygulaması kullanılarak işlem hatlarını izleme ve yönetme](data-factory-monitor-manage-app.md) | Bu makalede İzleme ve Yönetim Uygulaması kullanılarak işlem hatlarını izleme, yönetme ve hatalarını ayıklama işlemleri açıklanmaktadır. 

  




<!--HONumber=Jun16_HO2-->


