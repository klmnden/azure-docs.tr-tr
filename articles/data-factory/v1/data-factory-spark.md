---
title: "Azure Data Factory Spark programlardan çağırma | Microsoft Docs"
description: "MapReduce etkinliği kullanarak Azure data factory Spark programlardan çağırma öğrenin."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: fd98931c-cab5-4d66-97cb-4c947861255c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 5220ca664d5c7584f3aada0bb707099f91d5650f
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="invoke-spark-programs-from-azure-data-factory-pipelines"></a>Azure Data Factory işlem hatlarını Spark programlardan çağırma

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive etkinliği](data-factory-hive-activity.md)
> * [Pig etkinliği](data-factory-pig-activity.md)
> * [MapReduce etkinliği](data-factory-map-reduce.md)
> * [Hadoop akış etkinliği](data-factory-hadoop-streaming-activity.md)
> * [Spark etkinliği](data-factory-spark.md)
> * [Machine Learning Batch Yürütme Etkinliği](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Kaynak Güncelleştirme Etkinliği](data-factory-azure-ml-update-resource-activity.md)
> * [Saklı Yordam Etkinliği](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL Etkinliği](data-factory-usql-activity.md)
> * [.NET özel etkinlik](data-factory-use-custom-activities.md)

> [!NOTE]
> Bu makale, genel olarak kullanılabilir (GA) olduğu Azure Data Factory, 1 sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [Data Factory sürüm 2 kullanarak spark etkinliğini verileri](../transform-data-using-spark.md).

## <a name="introduction"></a>Giriş
Spark etkinlik biridir [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) Azure Data Factory tarafından desteklenir. Bu etkinlik, Azure hdınsight'ta Apache Spark kümenizde belirtilen Spark programı çalıştırır.    

> [!IMPORTANT]
> - Spark etkinliği Azure Data Lake Store birincil depolama alanı olarak kullanın Hdınsight Spark kümeleri desteklemez.
> - Spark etkinlik (kendi) Hdınsight Spark kümeleri yalnızca varolan destekler. Bir isteğe bağlı Hdınsight bağlı hizmeti desteklemiyor.

## <a name="walkthrough-create-a-pipeline-with-spark-activity"></a>İzlenecek yol: Spark etkinliği ile işlem hattı oluşturma
Spark etkinliği ile bir Data Factory işlem hattı oluşturmak için genel adımlar şunlardır.  

1. Veri fabrikası oluşturma.
2. Hdınsight Spark kümenize data factory ile ilişkili Azure depolama alanınızı bağlamak için bir Azure depolama bağlantılı hizmeti oluşturun.     
2. Azure hdınsight'ta Apache Spark kümenizin data factory'ye bağlamak için bir Azure Hdınsight bağlı hizmeti oluşturma.
3. Azure Storage bağlı hizmeti başvuran bir veri kümesi oluşturun. Şu anda, üretilen hiçbir çıktı olsa bile bir çıkış veri kümesi bir etkinlik için belirtmeniz gerekir.  
4. #2'de oluşturulan Hdınsight bağlı hizmeti başvurduğu Spark etkinliği ile işlem hattı oluşturacaksınız. Etkinlik, bir çıkış veri kümesi önceki adımda oluşturduğunuz veri kümesi ile yapılandırılır. Çıktı veri kümesi ne zamanlama (saatlik, günlük, vs.) sürücüleri ' dir. Bu nedenle, etkinlik gerçekten bir çıktı üretmez olsa da, çıktı veri kümesi belirtmeniz gerekir.

### <a name="prerequisites"></a>Ön koşullar
1. Create bir **genel amaçlı Azure depolama hesabı** Kılavuzu'ndaki yönergeleri izleyen tarafından: [depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account).  
2. Create bir **Azure hdınsight'ta Apache Spark kümesi** öğreticideki yönergeler aşağıdaki tarafından: [Azure hdınsight'ta Apache Spark oluşturma küme](../../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md). Bu kümeyle #1. adımda oluşturduğunuz Azure depolama hesabı ilişkilendirin.  
3. Karşıdan yükle ve python komut dosyasını gözden **test.py** konumunda bulunan: [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).  
3.  Karşıya yükleme **test.py** için **pyFiles** klasöründe **adfspark** , Azure Blob depolamada kapsayıcı. Bunlar yoksa kapsayıcıyı ve klasör oluşturun.

### <a name="create-data-factory"></a>Veri fabrikası oluşturma
Bu adımda data factory oluşturmayla başlayalım.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüde **YENİ**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın.
3. İçinde **yeni data factory** dikey penceresinde girin **SparkDF** adı.

   > [!IMPORTANT]
   > Azure data factory adı **küresel olarak benzersiz** olmalıdır. Hata görürseniz: **veri fabrikası adı "SparkDF" kullanılabilir değil**. (Örneğin, yournameSparkDFdate ve oluşturmayı yeniden deneyin. veri fabrikasının adını değiştirin Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.   
4. Data factory’yi oluşturmak istediğiniz **Azure aboneliği**’ni seçin.
5. Var olan seçin **kaynak grubu** veya bir Azure kaynak grubu oluşturun.
6. Seçin **panoya Sabitle** seçeneği.  
6. **Yeni data factory** dikey penceresinde **Oluştur**’a tıklayın.

   > [!IMPORTANT]
   > Data Factory örnekleri oluşturmak için abonelik/kaynak grubu düzeyinde [Data Factory Katılımcısı](../../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) rolünün üyesi olmanız gerekir.
7. Oluşturulmakta veri fabrikası gördüğünüz **Pano** şekilde Azure portalının:   
8. Data factory sorunsuz oluşturulduktan sonra data factory sayfasını görürsünüz, burada size data factory içeriği gösterilir. Data factory sayfasını görmüyorsanız Panoda veri fabrikanızın kutucuğa tıklayın.

    ![Data Factory dikey penceresi](./media/data-factory-spark/data-factory-blade.png)

### <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Bu adımda, veri fabrikası ve diğer Azure depolama alanınızı veri fabrikanıza bağlamak için Spark kümenizin bağlamak için iki bağlı hizmet oluşturun.  

#### <a name="create-azure-storage-linked-service"></a>Azure Storage bağlı hizmeti oluşturma
Bu adımda, Azure Depolama hesabınızı veri fabrikanıza bağlarsınız. Bu kılavuzda daha sonra bir adımda oluşturduğunuz bir veri kümesi bu bağlı hizmete başvuruyor. Sonraki adımda tanımladığınız Hdınsight bağlı hizmeti bu bağlantılı hizmeti çok ifade eder.  

1. Tıklatın **yazar ve dağıtma** üzerinde **Data Factory** veri fabrikanızın dikey. Data Factory Düzenleyicisi’ni görmeniz gerekir.
2. **Yeni data store**’a tıklayın ve **Azure depolama**’yı seçin.

   ![Yeni veri deposu - Azure Depolama - menü](./media/data-factory-spark/new-data-store-azure-storage-menu.png)
3. Görmeniz gerekir **JSON betiği** bağlantılı hizmetinin Düzenleyicisi'nde bir Azure depolama alanı oluşturmak için.

   ![Azure Storage bağlı hizmeti](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. Değiştir **hesap adı** ve **hesap anahtarı** Azure depolama hesabınızın adını ve erişim anahtarına sahip. Depolama erişim anahtarınızı nasıl alabileceğinizi öğrenmek için [Depolama hesabınızı yönetme](../../storage/common/storage-create-storage-account.md#manage-your-storage-account) sayfasındaki depolama erişim anahtarlarını görüntüleme, kopyalama ve yeniden oluşturma bilgilerine bakın.
5. Bağlantılı hizmet dağıtmak için **dağıtma** komut çubuğunda. Bağlı hizmet sorunsuz dağıtıldıktan sonra **Taslak-1** penceresi artık görünmemelidir; soldaki ağaç görünümünde **AzureStorageLinkedService** görürsünüz.

#### <a name="create-hdinsight-linked-service"></a>Hdınsight bağlı hizmeti oluşturma
Bu adımda, Hdınsight Spark kümenizin data factory'ye bağlamak için Azure Hdınsight bağlı hizmeti oluşturun. Hdınsight kümesi, bu örnekteki ardışık Spark etkinliğinde belirtilen Spark programı çalıştırmak için kullanılır.  

1. Düğmeyi görmüyorsanız araç çubuğunda **... Daha fazla** araç çubuğunda tıklatın **yeni işlem**ve ardından **Hdınsight kümesi**.

    ![Hdınsight bağlı hizmeti oluşturma](media/data-factory-spark/new-hdinsight-linked-service.png)
2. Aşağıdaki kod parçacığını kopyalayıp **Taslak-1** penceresine yapıştırın. JSON Düzenleyicisi'nde, aşağıdaki adımları uygulayın:
    1. Belirtin **URI** Hdınsight Spark kümesi için. Örneğin: `https://<sparkclustername>.azurehdinsight.net/`.
    2. Adını belirtin **kullanıcı** Spark küme erişimine sahiptir.
    3. Belirtin **parola** kullanıcı için.
    4. Belirtin **Azure depolama bağlantılı hizmeti** Hdınsight Spark kümesi ile ilişkili. Bu örnek,: **AzureStorageLinkedService**.

    ```json
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<sparkclustername>.azurehdinsight.net/",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    > [!IMPORTANT]
    > - Spark etkinliği Azure Data Lake Store birincil depolama alanı olarak kullanın Hdınsight Spark kümeleri desteklemez.
    > - Yalnızca (kendi) Hdınsight Spark kümesinde varolan Spark etkinlik destekler. Bir isteğe bağlı Hdınsight bağlı hizmeti desteklemiyor.

    Bkz: [Hdınsight bağlı hizmeti](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) Hdınsight hakkında ayrıntılı bilgi için bağlı hizmeti.
3.  Bağlantılı hizmet dağıtmak için **dağıtma** komut çubuğunda.  

### <a name="create-output-dataset"></a>Çıktı veri kümesi oluşturma
Çıktı veri kümesi ne zamanlama (saatlik, günlük, vs.) sürücüleri ' dir. Bu nedenle, etkinlik gerçekten herhangi bir çıktı üretmez olsa da ardışık düzeninde bir çıkış veri kümesi için spark etkinlik belirtmeniz gerekir. Bir giriş veri kümesi etkinliğin belirlenmesi isteğe bağlıdır.

1. **Data Factory Düzenleyicisi**’nde komut çubuğundaki **... Diğer**, **Yeni veri kümesi** öğelerine tıklayın ve **Azure Blob depolama** öğesini seçin.  
2. Aşağıdaki kod parçacığını kopyalayıp Taslak-1 penceresine yapıştırın. JSON parçacığı adlı veri kümesini tanımlamaktadır **OutputDataset**. Ayrıca, sonuçlar adlı blob kapsayıcısında depolanır belirttiğiniz **adfspark** ve adlı klasörde **pyFiles/çıkış**. Daha önce belirtildiği gibi bu veri kümesi kukla bir veri kümesi gereklidir. Bu örnekte Spark program herhangi bir çıktı üretmez. **Kullanılabilirlik** bölümü belirtiyor çıktı veri kümesi günlük üretilir.  

    ```json
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "sparkoutput.txt",
                "folderPath": "adfspark/pyFiles/output",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }
    ```
3. Veri kümesi dağıtmak için **dağıtma** komut çubuğunda.


### <a name="create-pipeline"></a>İşlem hattı oluşturma
Bu adımda oluşturduğunuz sahip işlem hattı bir **HDInsightSpark** etkinlik. Şu anda, çıktı veri kümesi zamanlamayı yönetendir; bu nedenle etkinlik hiçbir çıktı oluşturmasa bile sizin bir çıktı veri kümesi oluşturmanız gerekir. Etkinlik herhangi bir girdi almazsa, girdi veri kümesi oluşturma işlemini atlayabilirsiniz. Bu nedenle, hiçbir girdi veri kümesi bu örnekte belirtilir.

1. İçinde **Data Factory düzenleyici**, tıklatın **... Daha fazla** komut çubuğu ve ardından **yeni işlem hattı**.
2. Taslak-1 penceresine betiği aşağıdaki kod ile değiştirin:

    ```json
    {
        "name": "SparkPipeline",
        "properties": {
            "activities": [
                {
                    "type": "HDInsightSpark",
                    "typeProperties": {
                        "rootPath": "adfspark\\pyFiles",
                        "entryFilePath": "test.py",
                        "getDebugInfo": "Always"
                    },
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ],
                    "name": "MySparkActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-02-05T00:00:00Z",
            "end": "2017-02-06T00:00:00Z"
        }
    }
    ```
    Aşağıdaki noktalara dikkat edin:
    - **Türü** özelliği ayarlanmış **HDInsightSpark**.
    - **RootPath** ayarlanır **adfspark\\pyFiles** burada adfspark, Azure Blob kapsayıcısında ve pyFiles kapsayıcıdaki ince klasördür. Bu örnekte, Azure Blob Storage Spark kümesi ile ilişkili adrestir. Farklı bir Azure depolama dosyayı karşıya yükleyebilirsiniz. Bunu yaparsanız, bu depolama hesabı data factory'ye bağlamak için bir Azure depolama bağlantılı hizmeti oluşturun. Ardından, bağlı hizmetin adı için bir değer olarak belirtin **sparkJobLinkedService** özelliği. Bkz: [Spark etkinlik özellikleri](#spark-activity-properties) bu özellik ve Spark etkinlik tarafından desteklenen diğer özellikleri hakkında ayrıntılı bilgi için.  
    - **EntryFilePath** ayarlanır **test.py**, python dosyası değil.
    - **Getdebugınfo** özelliği ayarlanmış **her zaman**, günlük dosyaları her zaman anlamına gelir (başarılı veya başarısız) oluşturulur.

        > [!IMPORTANT]
        > Bu özelliği ayarlamak değil, öneririz `Always` bir üretim ortamında bir sorunu gidermeye çalışıyor değilseniz.
    - **Çıkarır** bölümü bir çıkış veri kümesi yok. Spark program herhangi bir çıktı üretmez olsa bile bir çıkış veri kümesi belirtmeniz gerekir. Çıktı veri kümesi, ardışık düzen (saatlik, günlük, vs.) için zamanlamayı sürücüleri.  

        Spark etkinliği tarafından desteklenen özellikler hakkında daha fazla ayrıntı için bkz: [Spark etkinlik özellikleri](#spark-activity-properties) bölümü.
3. İşlem hattını dağıtmak için **dağıtma** komut çubuğunda.

### <a name="monitor-pipeline"></a>İşlem hattını izleme
1. Tıklatın **X** Data Factory Düzenleyici dikey penceresini kapatmak ve Data Factory giriş sayfasına geri gidin. Tıklatın **izleme ve yönetme** başka bir sekmede izleme uygulamayı başlatmak için.

    ![İzleme ve yönetme bölmesi](media/data-factory-spark/monitor-and-manage-tile.png)
2. Değişiklik **başlangıç zamanı** en üstte filtre **1/2/2017**, tıklatıp **Uygula**.
3. Yalnızca bir gününü (2017-02-01) başlangıç ve bitiş zamanları (2017-02-02) ardışık arasında olarak yalnızca bir etkinlik penceresini görmeniz gerekir. Veri dilimi olduğunu onaylayın **hazır** durumu.

    ![İşlem hattını izleme](media/data-factory-spark/monitor-and-manage-app.png)    
4. Seçin **etkinlik penceresini** Çalıştır etkinliği hakkındaki ayrıntıları görmek için. Bir hata varsa, sağ bölmede hakkındaki ayrıntıları bakın.

### <a name="verify-the-results"></a>Sonuçları doğrulayın

1. Başlatma **Jupyter not defteri** giderek Hdınsight Spark kümenizin: https://CLUSTERNAME.azurehdinsight.net/jupyter. Ayrıca, Hdınsight Spark kümeniz için küme Panosu başlatın ve sonra başlatın **Jupyter not defteri**.
2. Tıklatın **yeni** -> **PySpark** yeni bir not defteri başlatmak için.

    ![Yeni Jupyter not defteri](media/data-factory-spark/jupyter-new-book.png)
3. Metin kopyalama/yapıştırma ve tuşuna basarak aşağıdaki komutu çalıştırın **SHIFT + ENTER** ikinci ifade sonunda.  

    ```sql
    %%sql

    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```
4. Hvac tablodan veri gördüğünüzü onaylayın:  

    ![Jupyter sorgu sonuçları](media/data-factory-spark/jupyter-notebook-results.png)

<!-- Removed bookmark #run-a-hive-query-using-spark-sql since it doesn't exist in the target article -->
Bkz: [bir Spark SQL sorgusu çalıştırmanız](../../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md) bölüm ayrıntılı yönergeler için. 

### <a name="troubleshooting"></a>Sorun giderme
Ayarladığınız bu yana **Getdebugınfo** için **her zaman**, gördüğünüz bir **günlük** alt klasöründe **pyFiles** klasörü, Azure Blob kapsayıcısında. Günlük dosyası günlük klasöründeki ek ayrıntılar sağlar. Bu günlük dosyası bir hata olduğunda özellikle yararlıdır. Bir üretim ortamında ayarlamak isteyebilirsiniz **hatası**.

Daha fazla sorun giderme için aşağıdaki adımları uygulayın:


1. Gidin `https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.

    ![YARN kullanıcı arabirimini uygulama](media/data-factory-spark/yarnui-application.png)  
2. Tıklatın **günlükleri** Çalıştır biri için çalışır.

    ![Uygulama sayfası](media/data-factory-spark/yarn-applications.png)
3. Günlük sayfasındaki ek hata bilgileri görmeniz gerekir.

    ![Günlük hatası](media/data-factory-spark/yarnui-application-error.png)

Aşağıdaki bölümlerde, veri fabrikası Apache Spark kümesi ve Spark etkinliği kullanmak için Data Factory varlıklarını hakkında bilgi sağlar.

## <a name="spark-activity-properties"></a>Spark etkinlik özellikleri
Spark etkinliği ile işlem hattı örnek JSON tanımını şöyledir:    

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "arguments": [ "arg1", "arg2" ],
                    "sparkConfig": {
                        "spark.python.worker.memory": "512m"
                    }
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "description": "This activity invokes the Spark program",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-01T00:00:00Z",
        "end": "2017-02-02T00:00:00Z"
    }
}
```

Aşağıdaki tabloda JSON tanımında kullanılan JSON özellikleri açıklanmaktadır:

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| ad | İşlem hattında etkinlik adı. | Evet |
| açıklama | Etkinlik yaptığı açıklayan metin. | Hayır |
| type | Bu özellik için HDInsightSpark ayarlamanız gerekir. | Evet |
| linkedServiceName | Spark programın çalıştığı Hdınsight bağlı hizmetin adı. | Evet |
| rootPath | Azure Blob kapsayıcısı ve Spark dosyasını içeren klasör. Dosya adı büyük/küçük harf duyarlıdır. | Evet |
| entryFilePath | Spark kod/paketi kök klasörüne göreli yolu. | Evet |
| className | Uygulamanın Java/Spark ana sınıfı | Hayır |
| Bağımsız değişkenler | Spark programın komut satırı bağımsız değişkenleri listesi. | Hayır |
| proxyUser | Spark program yürütülmeye kimliğine bürünmek için kullanıcı hesabı | Hayır |
| sparkConfig | Konu başlığı altında listelenen Spark yapılandırma özellikleri için değerleri belirtin: [Spark yapılandırması - uygulama özellikleri](https://spark.apache.org/docs/latest/configuration.html#available-properties). | Hayır |
| Getdebugınfo | Hdınsight küme tarafından kullanılan Azure depolama Spark günlük dosyalarının ne zaman kopyalanır belirtir (veya) sparkJobLinkedService tarafından belirtilen. İzin verilen değerler: None, her zaman veya hata. Varsayılan değer: yok. | Hayır |
| sparkJobLinkedService | Azure Storage bağlı Spark iş dosyası, bağımlılıklar ve günlükleri tutan hizmeti.  Bu özellik için bir değer belirtmezseniz, Hdınsight kümesi ile ilişkili depolama kullanılır. | Hayır |

## <a name="folder-structure"></a>Klasör yapısı
Spark etkinliği bir satır içi komut dosyası Pig olarak desteklemez ve Hive etkinlikleri yapın. Spark işleri de Pig/Hive işleri daha fazla genişletilebilir. Spark işleri, birden çok bağımlılıkları gibi sağlayabilirsiniz jar (java sınıf yerleştirilen) paketleri, python dosyaları (PYTHONPATH yerleştirilmiş) ve diğer dosyaları.

Hdınsight bağlı hizmeti tarafından başvurulan Azure Blob Depolama alanında aşağıdaki klasör yapısını oluşturun. Ardından, bağımlı dosyaları tarafından temsil edilen kök klasöründe uygun alt klasörlerdeki karşıya **entryFilePath**. Örneğin, python dosyalarını pyFiles alt klasöre ve jar dosyalarını kök klasörünün Kavanoz alt karşıya yükleyin. Çalışma zamanında Data Factory hizmeti Azure Blob depolama alanına aşağıdaki klasör yapısını bekler:     

| Yol | Açıklama | Gerekli | Tür |
| ---- | ----------- | -------- | ---- |
| . | Depolama bağlantılı hizmeti Spark işinde kök yolu  | Evet | Klasör |
| &lt;Kullanıcı tanımlı&gt; | Spark iş girişi dosyasına işaret eden yolu | Evet | Dosya |
| . / jar'lar | Bu klasörü altındaki tüm dosyaları karşıya ve küme java sınıf yolu yerleştirilmiş | Hayır | Klasör |
| . / pyFiles | Bu klasörü altındaki tüm dosyaları karşıya ve küme PYTHONPATH yerleştirilmiş | Hayır | Klasör |
| . / dosyaları | Bu klasörü altındaki tüm dosyaları karşıya ve yürütücü çalışma dizini yerleştirilmiş | Hayır | Klasör |
| . / arşivler | Bu klasörü altındaki tüm dosyaları sıkıştırılmamış | Hayır | Klasör |
| . / günlükleri | Spark kümesi günlüklerinden depolandığı klasörü.| Hayır | Klasör |

Burada, Azure Blob Depolama Hdınsight bağlı hizmeti tarafından başvurulan iki Spark iş dosyalarını içeren bir depolama alanı için bir örnek verilmiştir.

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
```
