---
title: Azure Data factory'den Spark programlarını çağırma | Microsoft Docs
description: MapReduce etkinliğini kullanarak bir Azure Data factory'den Spark programlarını çağırma hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: ''
editor: ''
ms.assetid: fd98931c-cab5-4d66-97cb-4c947861255c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 95c49eec6964984894f75ecd0a9e50c9c947683b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61257651"
---
# <a name="invoke-spark-programs-from-azure-data-factory-pipelines"></a>Azure Data Factory işlem hatlarını Spark programlarını çağırma

> [!div class="op_single_selector" title1="Dönüştürme etkinlikleri"]
> * [Hive etkinliği](data-factory-hive-activity.md)
> * [Pig etkinliği](data-factory-pig-activity.md)
> * [MapReduce etkinliği](data-factory-map-reduce.md)
> * [Hadoop akış etkinliğinde](data-factory-hadoop-streaming-activity.md)
> * [Spark etkinliği](data-factory-spark.md)
> * [Machine Learning Batch yürütme etkinliği](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning kaynak güncelleştirme etkinliği](data-factory-azure-ml-update-resource-activity.md)
> * [Saklı yordam etkinliği](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL etkinliği](data-factory-usql-activity.md)
> * [.NET özel etkinliği](data-factory-use-custom-activities.md)

> [!NOTE]
> Bu makale, Azure Data Factory’nin genel kullanıma açık olan 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [Data Factory'de Apache Spark etkinliği kullanarak verileri dönüştürme](../transform-data-using-spark.md).

## <a name="introduction"></a>Giriş
Spark etkinliğini biridir [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) Data Factory tarafından desteklenen. Bu etkinlik, Azure HDInsight Spark kümesinde belirtilen Spark programını çalıştırır. 

> [!IMPORTANT]
> - Spark etkinliği, HDInsight Spark kümeleri, birincil depolama olarak Azure Data Lake Store kullanma desteklemiyor.
> - Spark etkinliği, HDInsight Spark kümeleri (kendi) yalnızca mevcut destekler. Bunu yapmak için bir isteğe bağlı HDInsight bağlı hizmeti desteklemiyor.

## <a name="walkthrough-create-a-pipeline-with-a-spark-activity"></a>Çözüm: Spark etkinliği ile işlem hattı oluşturma
Spark etkinliği ile bir veri fabrikası işlem hattı oluşturmak için tipik adımları şunlardır: 

* Veri fabrikası oluşturma.
* Data Factory, HDInsight Spark kümesi ile ilişkili depolama alanınızı bağlamak için bir Azure depolama bağlı hizmeti oluşturma.
* HDInsight Spark kümenizde veri fabrikasına bağlamak için bir HDInsight bağlı hizmeti oluşturma.
* Depolama bağlı hizmetini ifade eder bir veri kümesi oluşturursunuz. Şu anda, üretilen çıktı olsa bile bir çıktı veri kümesi bir etkinliğin belirtmeniz gerekir. 
* Oluşturduğunuz HDInsight bağlı hizmetini ifade eder Spark etkinliği ile işlem hattı oluşturma. Etkinlik, bir çıktı veri kümesi olarak önceki adımda oluşturduğunuz veri kümesi ile yapılandırılır. Çıktı veri kümesi (saatlik, günlük) zamanlamayı yönetendir; ' dir. Bu nedenle, etkinlik, gerçekten bir çıktı oluşturmasa olsa da, çıktı veri kümesi belirtmeniz gerekir.

### <a name="prerequisites"></a>Önkoşullar
1. Genel amaçlı depolama hesabı'ndaki yönergeleri takip ederek oluşturma [depolama hesabı oluşturma](../../storage/common/storage-quickstart-create-account.md).

1. Öğreticide yönergeleri takip ederek HDInsight Spark kümesi oluşturma [HDInsight Spark kümesi oluşturma](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). Bu küme ile 1. adımda oluşturduğunuz depolama hesabını ilişkilendirebilirsiniz.

1. İndirin ve Python betik dosyasını gözden geçirerek **test.py** konumundaki [ https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py ](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).

1. Karşıya yükleme **test.py** için **pyFiles** klasöründe **adfspark** blob depolama alanınızda kapsayıcı. Mevcut olmaması durumunda ise kapsayıcıyı ve klasörü oluşturun.

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Veri fabrikası oluşturmak için bu adımları izleyin:

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. **Yeni** > **Veri ve Analiz** > **Data Factory**’yi seçin.

1. Üzerinde **yeni veri fabrikası** dikey altında **adı**, girin **SparkDF**.

   > [!IMPORTANT]
   > Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. "Veri fabrikası adı SparkDF kullanılamıyor" hatayı görürseniz veri fabrikasının adını değiştirin. Örneğin, yournameSparkDFdate kullanın ve veri fabrikasını yeniden oluşturun. Adlandırma kuralları hakkında daha fazla bilgi için bkz. [Data Factory: Adlandırma kuralları](data-factory-naming-rules.md).

1. Abonelik bölümünde, veri fabrikasını oluşturmak istediğiniz **Azure aboneliğini** seçin.

1. Mevcut bir kaynak grubunu seçin veya bir Azure kaynak grubu oluşturun.

1. **Panoya sabitle** onay kutusunu seçin.

1. **Oluştur**’u seçin.

   > [!IMPORTANT]
   > Data Factory örnekleri oluşturmak için abonelik/kaynak grubu düzeyinde [Data Factory katılımcısı](../../role-based-access-control/built-in-roles.md#data-factory-contributor) rolünün üyesi olmanız gerekir.

1. Azure portal'ın panosunda oluşturulduğundan, data factory görürsünüz.

1. Veri fabrikası oluşturulduktan sonra veri fabrikasının içeriğinin gösterildiği **Veri fabrikası** sayfasını görürsünüz. Görmüyorsanız **veri fabrikası** sayfasında, veri fabrikanızın panodaki bir kutucuk seçin.

    ![Data Factory dikey penceresi](./media/data-factory-spark/data-factory-blade.png)

### <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Bu adımda iki bağlı hizmet oluşturursunuz. Bir hizmet Spark kümenizin veri fabrikanıza bağlar ve diğer hizmet depolama alanınızı veri fabrikanıza bağlar. 

#### <a name="create-a-storage-linked-service"></a>Depolama bağlı hizmeti oluşturma
Bu adımda, depolama hesabınızı veri fabrikanıza bağlarsınız. Bu kılavuzda daha sonra bir adımda oluşturduğunuz veri kümesi bu bağlı hizmetini ifade eder. Sonraki adımda tanımladığınız HDInsight bağlı hizmeti, bu bağlı hizmetle çok ifade eder. 

1. Üzerinde **veri fabrikası** dikey penceresinde **yazar ve dağıtma**. Data Factory Düzenleyicisi görünür.

1. **Yeni veri deposu**’na tıklayın ve **Azure Depolama**’yı seçin.

   ![Yeni veri deposu](./media/data-factory-spark/new-data-store-azure-storage-menu.png)

1. JSON Kod Düzenleyicisi'nde bir depolama bağlı hizmeti görünür oluşturmak için kullanın.

   ![AzureStorageLinkedService](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)

1. Değiştirin **hesap adı** ve **hesap anahtarı** depolama hesabınızın adını ve erişim anahtarına sahip. Depolama erişim anahtarınızı nasıl alabileceğinizi öğrenmek için [Depolama hesabınızı yönetme](../../storage/common/storage-account-manage.md#access-keys) sayfasındaki depolama erişim anahtarlarını görüntüleme, kopyalama ve yeniden oluşturma yönergelerine bakın.

1. Bağlı hizmeti dağıtmak için seçebileceğiniz **Dağıt** komut çubuğunda. Bağlı hizmet başarıyla dağıtıldıktan sonra Draft-1 penceresi kaybolur. Soldaki ağaç görünümünde **AzureStorageLinkedService** öğesini görürsünüz.

#### <a name="create-an-hdinsight-linked-service"></a>HDInsight bağlı hizmeti oluşturma
Bu adımda, HDInsight Spark kümeniz veri fabrikasına bağlamak için bir HDInsight bağlı hizmeti oluşturma. HDInsight kümesi, bu örnekteki işlem hattı Spark etkinliğini belirtilen Spark programını çalıştırmak için kullanılır. 

1. Data Factory Düzenleyicisi'nde seçin **daha fazla** > **yeni işlem** > **HDInsight küme**.

    ![HDInsight bağlı hizmeti oluşturma](media/data-factory-spark/new-hdinsight-linked-service.png)

1. Aşağıdaki kod parçacığını kopyalayıp Taslak-1 penceresine yapıştırın. JSON Düzenleyicisi'nde, aşağıdaki adımları uygulayın:

    a. HDInsight Spark kümesi URI'sini belirtin. Örneğin: `https://<sparkclustername>.azurehdinsight.net/`.

    b. Spark kümesi erişimi olan kullanıcının adını belirtin.

    c. Kullanıcının parolasını belirtin.

    d. HDInsight Spark kümesi ile ilişkili depolama bağlı hizmeti belirtin. Bu örnekte, buna AzureStorageLinkedService var.

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
    > - Spark etkinliği, HDInsight Spark kümeleri, birincil depolama olarak Azure Data Lake Store kullanma desteklemiyor.
    > - Spark etkinliği, HDInsight Spark kümeleri (kendi) yalnızca mevcut destekler. Bunu yapmak için bir isteğe bağlı HDInsight bağlı hizmeti desteklemiyor.

    HDInsight bağlı hizmeti hakkında daha fazla bilgi için bkz. [HDInsight bağlı hizmeti](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).

1. Bağlı hizmeti dağıtmak için seçebileceğiniz **Dağıt** komut çubuğunda. 

### <a name="create-the-output-dataset"></a>Çıktı veri kümesini oluşturma
Çıktı veri kümesi (saatlik, günlük) zamanlamayı yönetendir; ' dir. Bu nedenle etkinlik hiçbir çıktı oluşturmasa bile olsa, bir çıktı veri kümesi için Spark etkinliğini işlem hattı, belirtmeniz gerekir. Etkinliği için girdi veri kümesi belirtme isteğe bağlıdır.

1. Data Factory Düzenleyicisi’nde **Diğer** > **Yeni veri kümesi** > **Azure Blob depolama**’yı seçin.

1. Aşağıdaki kod parçacığını kopyalayıp Taslak-1 penceresine yapıştırın. JSON kod parçacığında adlı bir veri kümesi tanımlar **OutputDataset**. Ayrıca, sonuçları adlı blob kapsayıcısında depolanan belirttiğiniz **adfspark** ve adlı klasörde **pyFiles/çıkış**. Daha önce belirtildiği gibi bu veri kümesi bir işlevsiz veri kümesidir. Spark programı bu örnekte, hiçbir çıktı oluşturmasa. **Kullanılabilirlik** bölümü çıktı veri kümesi günlük olarak oluşturulduğunu belirtir. 

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
1. Veri kümesini dağıtmak için seçebileceğiniz **Dağıt** komut çubuğunda.


### <a name="create-a-pipeline"></a>İşlem hattı oluşturma
Bu adımda, bir HDInsightSpark etkinliği ile işlem hattı oluşturma. Şu anda, zamanlama çıktı veri kümesi tarafından yönetildiğinden, etkinlik hiçbir çıktı oluşturmasa bile bir çıktı veri kümesi oluşturmanız gerekir. Etkinlik herhangi bir girdi almazsa, girdi veri kümesi oluşturma işlemini atlayabilirsiniz. Bu nedenle, bu örnekte, giriş veri kümesi belirtildi.

1. Data Factory Düzenleyicisi’nde **Diğer** > **Yeni işlem hattı**’nı seçin.

1. Taslak-1 penceresine betikte aşağıdaki betikle değiştirin:

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

    a. **Türü** özelliği **HDInsightSpark**.

    b. **RootPath** özelliği **adfspark\\pyFiles** burada adfspark blob kapsayıcısını ve pyFiles kapsayıcıdaki dosya klasörü. Bu örnekte, Spark kümesi ile ilişkili bir blob depolama alanıdır. Farklı bir depolama hesabı için dosyayı karşıya yükleyebilirsiniz. Bunu yaparsanız, depolama hesabınızı veri fabrikasına bağlamak için bir depolama bağlı hizmeti oluşturma. Ardından için bir değer olarak bağlı hizmetin adı belirtin **sparkJobLinkedService** özelliği. Bu özellik ve Spark etkinliği tarafından desteklenen diğer özellikler hakkında daha fazla bilgi için bkz. [Spark etkinliği özellikleri](#spark-activity-properties).

    c. **EntryFilePath** özelliği **test.py**, Python dosyası olduğu.

    d. **Getdebugınfo** özelliği **her zaman**, günlük dosyaları her zaman anlamına gelir (başarı veya başarısızlık) oluşturulur.

    > [!IMPORTANT]
    > Bu özellik ayarlanmamışsa, öneririz `Always` bir üretim ortamında bir sorunu gidermeye çalışıyorsanız sürece.

    e. **Çıkarır** bölümünde bir çıktı veri kümesi bulunur. Spark programının hiçbir çıktı oluşturmasa bile bir çıktı veri kümesi belirtmelisiniz. Çıktı veri kümesi (saatlik, günlük) işlem hattı için zamanlamayı beraberinde getirir. 

    Spark etkinliği tarafından desteklenen özellikler hakkında daha fazla bilgi için bkz [Spark etkinliği özellikleri](#spark-activity-properties).

1. İşlem hattını dağıtmak için seçebileceğiniz **Dağıt** komut çubuğunda.

### <a name="monitor-a-pipeline"></a>İşlem hattını izleme
1. Üzerinde **veri fabrikası** dikey penceresinde **izleme ve yönetme** izleme uygulaması başka bir sekmede başlatmak için.

    ![İzleme ve Yönetme kutucuğu](media/data-factory-spark/monitor-and-manage-tile.png)

1. Değişiklik **başlangıç zamanı** filtre en üstte **1/2/2017**seçip **Uygula**.

1. Yalnızca bir gününü (2017-02-01) başlangıç ve bitiş saatlerini (2017-02-02) işlem hattı arasındaki olduğundan yalnızca bir etkinlik penceresi görüntülenir. Veri dilimi olduğunu onaylayın **hazır** durumu.

    ![İşlem hattını izleme](media/data-factory-spark/monitor-and-manage-app.png)

1. İçinde **etkinlik pencereleri** listesinde, ayrıntılarını görmek için bir etkinlik seçin. Bir hata varsa, sağ bölmede ilgili ayrıntıları görürsünüz.

### <a name="verify-the-results"></a>Sonuçları doğrulayın

1. Jupyter Not Defteri, HDInsight Spark kümeniz adresine giderek başlatın [bu Web sitesi](https://CLUSTERNAME.azurehdinsight.net/jupyter). HDInsight Spark için de bir küme Panosu açabilirsiniz küme ve Jupyter not defteri başlatın.

1. Seçin **yeni** > **PySpark** yeni bir not defteri başlatmak için.

    ![Yeni Jupyter notebook](media/data-factory-spark/jupyter-new-book.png)

1. Kopyalama ve yapıştırma metin ve sonunda ikinci deyim, SHIFT + Enter tuşuna basarak tarafından aşağıdaki komutu çalıştırın:

    ```sql
    %%sql

    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```
1. Hvac tablodaki verileri gördüğünüzü onaylayın. 

    ![Jupyter sorgu sonuçları](media/data-factory-spark/jupyter-notebook-results.png)

<!-- Removed bookmark #run-a-hive-query-using-spark-sql since it doesn't exist in the target article -->
Ayrıntılı yönergeler için konudaki [bir Spark SQL sorgusu çalıştırma](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). 

### <a name="troubleshooting"></a>Sorun giderme
Getdebugınfo kümesine çünkü **her zaman**, blob kapsayıcınızdaki pyFiles klasöründeki günlük alt görürsünüz. Günlük klasörü günlük dosyasına ek bilgi sağlar. Bu günlük dosyası bir hata olduğunda özellikle yararlıdır. Bir üretim ortamında ayarlamak isteyebilirsiniz **hatası**.

Sorunu gidermek için aşağıdaki adımları uygulayın:


1. `https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster` kısmına gidin.

    ![YARN kullanıcı Arabiriminde uygulama](media/data-factory-spark/yarnui-application.png)

1. Seçin **günlükleri** çalıştırma biri için çalışır.

    ![Uygulama sayfası](media/data-factory-spark/yarn-applications.png)

1. Aşağıdaki ek hata bilgileri günlüğü sayfasında bakın:

    ![Günlük hata](media/data-factory-spark/yarnui-application-error.png)

Aşağıdaki bölümlerde, data factory'de Spark kümesi ve Spark etkinliği kullanmak için data factory varlıkları hakkında bilgi sağlar.

## <a name="spark-activity-properties"></a>Spark etkinliği özellikleri
Bir Spark etkinliği ile işlem hattı JSON tanımını örnek aşağıda verilmiştir: 

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

Aşağıdaki tabloda JSON tanımında kullanılan JSON özellikleri açıklanmaktadır.

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| name | İşlem hattındaki bir etkinliğin adı. | Evet |
| description | Etkinliğin ne yaptığını açıklayan metin. | Hayır |
| type | Bu özellik için HDInsightSpark ayarlamanız gerekir. | Evet |
| linkedServiceName | Spark programının çalıştığı HDInsight bağlı hizmetin adı. | Evet |
| rootPath | Blob kapsayıcısını ve Spark dosyasını içeren klasör. Dosya adı büyük/küçük harfe duyarlıdır. | Evet |
| entryFilePath | Spark kodun/paketin kök klasörünün göreli yolu. | Evet |
| className | Uygulamanın Java/Spark temel sınıfı. | Hayır |
| arguments | Spark programı için komut satırı bağımsız değişkenleri listesi. | Hayır |
| proxyUser | Spark programının yürütülecek kimliğine bürünmek için kullanıcı hesabı. | Hayır |
| sparkConfig | Listelenen Spark yapılandırma özellikleri için değerleri belirtin [Spark yapılandırması: Uygulama özellikleri](https://spark.apache.org/docs/latest/configuration.html#available-properties). | Hayır |
| getDebugInfo | HDInsight küme tarafından kullanılan depolama Spark günlük dosyalarının ne zaman kopyalanır belirtir (veya) sparkJobLinkedService belirtilir. İzin verilen değerler, her zaman veya hata yok. Varsayılan değer, Yok'tur. | Hayır |
| sparkJobLinkedService | Depolama bağlı iş dosyası, bağımlılıklar ve günlükleri Spark tutan hizmeti. Bu özellik için bir değer belirtmezseniz, HDInsight kümesi ile ilişkili depolama kullanılır. | Hayır |

## <a name="folder-structure"></a>klasör yapısı
Spark etkinliği bir satır içi betik Pig olarak desteklemez ve Hive etkinlikleri. Spark işleri de Pig/Hive işlerini daha fazla genişletilebilir. Spark işleri için çoklu bağımlılıklar gibi sağlayabilirsiniz (java sınıf yolu yerleştirilen) paketleri (ise PYTHONPATH üzerinde yerleştirilen) Python dosyaları ve diğer dosyaları jar.

HDInsight bağlı hizmeti tarafından başvurulan blob depolama alanındaki aşağıdaki klasör yapısını oluşturun. Ardından, uygun alt tarafından temsil edilen kök klasöründe bağımlı dosya yükleme **entryFilePath**. Örneğin, Python dosyaları pyFiles alt klasöre yüklemek ve jar dosyaları dışındaki kök klasörün alt dosyalarını jar. Çalışma zamanında Data Factory hizmeti blob depolamada aşağıdaki klasör yapısına bekliyor: 

| `Path` | Açıklama | Gerekli | Tür |
| ---- | ----------- | -------- | ---- |
| . | Spark işi depolama bağlı hizmeti kök yolu. | Evet | Klasör |
| &lt;Kullanıcı tanımlı &gt; | Spark işi giriş dosyasına işaret eden yol. | Evet | Dosya |
| . / jar'lar | Bu klasör altındaki tüm dosyaları karşıya ve küme Java sınıf yolu yerleştirilir. | Hayır | Klasör |
| ./pyFiles | Bu klasör altındaki tüm dosyaları karşıya ve küme ise PYTHONPATH üzerinde yerleştirilir. | Hayır | Klasör |
| . / dosyaları | Bu klasör altındaki tüm dosyaları karşıya ve yürütücü çalışma dizinine yerleştirilir. | Hayır | Klasör |
| . / arşivleri | Bu klasör altındaki tüm dosyaları sıkıştırılmamış. | Hayır | Klasör |
| . / günlüğe kaydeder | Spark kümesinden günlüklerinin depolandığı klasör.| Hayır | Klasör |

Blob depolama HDInsight bağlı hizmeti tarafından başvurulan iki Spark işi dosyaları içeren depolama alanı için bir örnek aşağıda verilmiştir:

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
