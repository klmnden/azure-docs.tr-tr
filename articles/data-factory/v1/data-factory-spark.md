---
title: Azure Data Factory Spark programlardan çağırma | Microsoft Docs
description: MapReduce etkinliğini kullanarak bir Azure data factory Spark programlardan çağırma öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: ''
editor: ''
ms.assetid: fd98931c-cab5-4d66-97cb-4c947861255c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: e775798dbaaf93d5a9b497323a3b2fa365820550
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37046477"
---
# <a name="invoke-spark-programs-from-azure-data-factory-pipelines"></a>Azure Data Factory işlem hatlarını Spark programlardan çağırma

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive etkinliği](data-factory-hive-activity.md)
> * [Pig etkinliği](data-factory-pig-activity.md)
> * [MapReduce etkinliği](data-factory-map-reduce.md)
> * [Hadoop akış etkinliği](data-factory-hadoop-streaming-activity.md)
> * [Spark etkinliği](data-factory-spark.md)
> * [Machine Learning toplu iş yürütme etkinliği](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning güncelleştirme kaynağı etkinliği](data-factory-azure-ml-update-resource-activity.md)
> * [Saklı yordam etkinliği](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL etkinliği](data-factory-usql-activity.md)
> * [.NET özel etkinliği](data-factory-use-custom-activities.md)

> [!NOTE]
> Bu makale, Azure Data Factory’nin genel kullanıma açık olan 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [veri fabrikasında Apache Spark etkinliğini kullanarak veri dönüştürme](../transform-data-using-spark.md).

## <a name="introduction"></a>Giriş
Spark etkinlik biri [veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) Data Factory ile desteklenen. Bu etkinlik, Azure Hdınsight Spark kümesinde belirtilen Spark programı çalıştırır. 

> [!IMPORTANT]
> - Spark etkinliği Azure Data Lake Store birincil depolama alanı olarak kullanma Hdınsight Spark kümeleri desteklemez.
> - Spark etkinlik (kendi) Hdınsight Spark kümeleri yalnızca varolan destekler. Bir isteğe bağlı Hdınsight bağlı hizmeti desteklemiyor.

## <a name="walkthrough-create-a-pipeline-with-a-spark-activity"></a>İzlenecek yol: bir Spark etkinliği ile işlem hattı oluşturma
Spark etkinliği ile bir data factory işlem hattı oluşturmak için tipik adımları şunlardır: 

* Veri fabrikası oluşturma.
* Hdınsight Spark kümenize data factory ile ilişkili depolama alanınızın bağlamak için bir Azure depolama bağlantılı hizmeti oluşturun.
* Hdınsight'ta Spark kümenizin data factory'ye bağlamak için bir Hdınsight bağlantılı hizmeti oluşturun.
* Depolama bağlı hizmeti başvuran bir veri kümesi oluşturun. Şu anda, üretilen hiçbir çıktı olsa bile bir çıkış veri kümesi bir etkinlik için belirtmeniz gerekir. 
* Oluşturduğunuz Hdınsight bağlı hizmeti başvuruyor Spark etkinliği ile işlem hattı oluşturacaksınız. Etkinlik, bir çıkış veri kümesi önceki adımda oluşturduğunuz veri kümesi ile yapılandırılır. Çıktı veri kümesi ne zamanlama (saatlik, günlük) sürücüleri ' dir. Bu nedenle, etkinlik çıktısı gerçekten oluşturmuyor olsa da, çıktı veri kümesi belirtmeniz gerekir.

### <a name="prerequisites"></a>Önkoşullar
1. Yönergeleri izleyerek genel amaçlı depolama hesabı oluşturma [depolama hesabı oluşturma](../../storage/common/storage-create-storage-account.md#create-a-storage-account).

2. Öğreticide yönergeleri izleyerek Hdınsight'ta Spark kümesi oluşturma [Hdınsight'ta Spark kümesi oluşturma](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). Bu küme ile 1. adımda oluşturduğunuz depolama hesabı ilişkilendirin.

3. Karşıdan yükle ve Python komut dosyasını gözden **test.py** konumunda bulunan [ https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py ](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).

4. Karşıya yükleme **test.py** için **pyFiles** klasöründe **adfspark** blob depolama alanınızın kapsayıcısında. Yoksa kapsayıcıyı ve klasör oluşturun.

### <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
Veri fabrikası oluşturmak için bu adımları izleyin:

1. [Azure Portal](https://portal.azure.com/) oturum açın.

2. **Yeni** > **Veri ve Analiz** > **Data Factory**’yi seçin.

3. Üzerinde **yeni data factory** dikey altında **adı**, girin **SparkDF**.

   > [!IMPORTANT]
   > Azure veri fabrikasının adı genel olarak benzersiz olmalıdır. "Veri fabrikası adı SparkDF kullanılabilir değil" hatasını görürseniz, data factory adını değiştirin. Örneğin, yournameSparkDFdate kullanın ve veri fabrikası yeniden oluşturun. Adlandırma kuralları hakkında daha fazla bilgi için bkz. [Data Factory: Adlandırma kuralları](data-factory-naming-rules.md).

4. Abonelik bölümünde, veri fabrikasını oluşturmak istediğiniz **Azure aboneliğini** seçin.

5. Varolan bir kaynak grubu seçin veya bir Azure kaynak grubu oluşturun.

6. **Panoya sabitle** onay kutusunu seçin.

7. **Oluştur**’u seçin.

   > [!IMPORTANT]
   > Data Factory örnekleri oluşturmak için abonelik/kaynak grubu düzeyinde [Data Factory katılımcısı](../../role-based-access-control/built-in-roles.md#data-factory-contributor) rolünün üyesi olmanız gerekir.

8. Azure portal panosunda oluşturulduğunda, veri fabrikası görürsünüz.

9. Veri fabrikası oluşturulduktan sonra veri fabrikasının içeriğinin gösterildiği **Veri fabrikası** sayfasını görürsünüz. Görmüyorsanız, **veri fabrikası** sayfasında, Panoda veri fabrikanızın döşeme seçin.

    ![Data Factory dikey penceresi](./media/data-factory-spark/data-factory-blade.png)

### <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Bu adımda, iki bağlı hizmet oluşturun. Bir hizmet Spark kümenizin veri fabrikanıza bağlar ve diğer hizmet depolama alanınızın veri fabrikanıza bağlar. 

#### <a name="create-a-storage-linked-service"></a>Depolama bağlı hizmeti oluşturma
Bu adımda, depolama hesabınızı veri fabrikanıza bağlarsınız. Bu kılavuzda daha sonra bir adımda oluşturduğunuz bir veri kümesi bu bağlı hizmete başvuruyor. Sonraki adımda tanımladığınız Hdınsight bağlı hizmeti bu bağlantılı hizmeti çok ifade eder. 

1. Üzerinde **veri fabrikası** dikey penceresinde, select **yazar ve dağıtma**. Data Factory Düzenleyici görüntülenir.

2. **Yeni veri deposu**’na tıklayın ve **Azure Depolama**’yı seçin.

   ![Yeni veri deposu](./media/data-factory-spark/new-data-store-azure-storage-menu.png)

3. JSON komut dosyası Düzenleyicisi'nde bir depolama bağlantılı hizmeti görünür oluşturmak için kullanın.

   ![AzureStorageLinkedService](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)

4. Değiştir **hesap adı** ve **hesap anahtarı** depolama hesabınızın adını ve erişim anahtarına sahip. Depolama erişim anahtarınızı nasıl alabileceğinizi öğrenmek için [Depolama hesabınızı yönetme](../../storage/common/storage-create-storage-account.md#manage-your-storage-account) sayfasındaki depolama erişim anahtarlarını görüntüleme, kopyalama ve yeniden oluşturma yönergelerine bakın.

5. Bağlı hizmeti dağıtmak için seçin **dağıtma** komut çubuğunda. Bağlı hizmet başarıyla dağıtıldıktan sonra Draft-1 penceresi kaybolur. Soldaki ağaç görünümünde **AzureStorageLinkedService** öğesini görürsünüz.

#### <a name="create-an-hdinsight-linked-service"></a>HDInsight bağlı hizmeti oluşturma
Bu adımda, Hdınsight Spark kümenizin data factory'ye bağlamak için bir Hdınsight bağlantılı hizmeti oluşturun. Hdınsight kümesi, bu örnekteki ardışık Spark etkinliğinde belirtilen Spark programı çalıştırmak için kullanılır. 

1. Data Factory düzenleyici seçin **daha fazla** > **yeni işlem** > **Hdınsight kümesi**.

    ![HDInsight bağlı hizmeti oluşturma](media/data-factory-spark/new-hdinsight-linked-service.png)

2. Aşağıdaki kod parçacığını kopyalayıp Taslak-1 penceresine yapıştırın. JSON Düzenleyicisi'nde, aşağıdaki adımları uygulayın:

    a. Hdınsight Spark kümesi URI'sini belirtin. Örneğin: `https://<sparkclustername>.azurehdinsight.net/`.

    b. Spark kümesi erişimi olan kullanıcı adını belirtin.

    c. Kullanıcının parolasını belirtin.

    d. Hdınsight Spark kümesi ile ilişkili depolama bağlı hizmeti belirtin. Bu örnekte, AzureStorageLinkedService değil.

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
    > - Spark etkinliği Azure Data Lake Store birincil depolama alanı olarak kullanma Hdınsight Spark kümeleri desteklemez.
    > - Spark etkinlik (kendi) Hdınsight Spark kümeleri yalnızca varolan destekler. Bir isteğe bağlı Hdınsight bağlı hizmeti desteklemiyor.

    Hdınsight bağlı hizmeti hakkında daha fazla bilgi için bkz: [Hdınsight bağlantılı hizmeti](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).

3. Bağlı hizmeti dağıtmak için seçin **dağıtma** komut çubuğunda. 

### <a name="create-the-output-dataset"></a>Çıktı veri kümesini oluşturma
Çıktı veri kümesi ne zamanlama (saatlik, günlük) sürücüleri ' dir. Bu nedenle, etkinlik herhangi bir çıktı oluşturmuyor olsa da ardışık düzeninde bir çıkış veri kümesi için Spark etkinlik belirtmeniz gerekir. Bir giriş veri kümesi etkinliğin belirlenmesi isteğe bağlıdır.

1. Data Factory Düzenleyicisi’nde **Diğer** > **Yeni veri kümesi** > **Azure Blob depolama**’yı seçin.

2. Aşağıdaki kod parçacığını kopyalayıp Taslak-1 penceresine yapıştırın. JSON parçacığı adlı veri kümesini tanımlamaktadır **OutputDataset**. Ayrıca, sonuçlar adlı blob kapsayıcısında depolanır belirttiğiniz **adfspark** ve adlı klasörde **pyFiles/çıkış**. Daha önce belirtildiği gibi bu veri kümesi kukla bir veri kümesi ' dir. Bu örnekte Spark program herhangi bir çıktı oluşturmuyor. **Kullanılabilirlik** bölümü belirtiyor çıktı veri kümesi günlük üretilir. 

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
3. Veri kümesi dağıtmak için seçin **dağıtma** komut çubuğunda.


### <a name="create-a-pipeline"></a>İşlem hattı oluşturma
Bu adımda, bir HDInsightSpark etkinliği ile işlem hattı oluşturun. Şu anda, zamanlama çıktı veri kümesi tarafından yönetildiğinden, etkinlik hiçbir çıktı oluşturmasa bile bir çıktı veri kümesi oluşturmanız gerekir. Etkinlik herhangi bir girdi almazsa, girdi veri kümesi oluşturma işlemini atlayabilirsiniz. Bu nedenle, hiçbir girdi veri kümesi bu örnekte belirtilir.

1. Data Factory Düzenleyicisi’nde **Diğer** > **Yeni işlem hattı**’nı seçin.

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

    a. **Türü** özelliği ayarlanmış **HDInsightSpark**.

    b. **RootPath** özelliği ayarlanmış **adfspark\\pyFiles** burada adfspark blob kapsayıcısında ve pyFiles kapsayıcıdaki dosya klasörü. Bu örnekte, blob depolama Spark kümesi ile ilişkili adrestir. Farklı bir depolama hesabı için dosyayı karşıya yükleyebilirsiniz. Bunu yaparsanız, bu depolama hesabı data factory'ye bağlamak için bir depolama bağlantılı hizmeti oluşturun. Ardından, bağlı hizmetin adı için bir değer olarak belirtin **sparkJobLinkedService** özelliği. Bu özellik ve Spark etkinlik tarafından desteklenen diğer özellikler hakkında daha fazla bilgi için bkz: [Spark etkinlik özellikleri](#spark-activity-properties).

    c. **EntryFilePath** özelliği ayarlanmış **test.py**, Python dosyası değil.

    d. **Getdebugınfo** özelliği ayarlanmış **her zaman**, günlük dosyaları her zaman anlamına gelir (başarılı veya başarısız) oluşturulur.

    > [!IMPORTANT]
    > Bu özelliği ayarlamak değil, öneririz `Always` bir üretim ortamında bir sorunu gidermeye çalışıyorsanız sürece.

    e. **Çıkarır** bölümü bir çıkış veri kümesi yok. Spark program herhangi bir çıktı oluşturmuyor olsa bile bir çıkış veri kümesi belirtmeniz gerekir. Çıktı veri kümesi (saatlik, günlük) ardışık düzeni için zamanlama sürücüleri. 

    Bölüm Spark etkinlik tarafından desteklenen özellikler hakkında daha fazla bilgi için bkz: [Spark etkinlik özellikleri](#spark-activity-properties).

3. İşlem hattını dağıtmak için seçin **dağıtma** komut çubuğunda.

### <a name="monitor-a-pipeline"></a>İşlem hattını izleme
1. Üzerinde **veri fabrikası** dikey penceresinde, select **İzleyici & Yönet** başka bir sekmede izleme uygulamayı başlatmak için.

    ![İzleme ve Yönetme kutucuğu](media/data-factory-spark/monitor-and-manage-tile.png)

2. Değişiklik **başlangıç zamanı** en üstte filtre **1/2/2017**seçip **Uygula**.

3. Yalnızca bir gününü (2017-02-01) başlangıç ve bitiş zamanları (2017-02-02) ardışık arasındaki olduğundan yalnızca bir etkinlik penceresi görüntülenir. Veri dilimi olduğunu onaylayın **hazır** durumu.

    ![İşlem hattını izleme](media/data-factory-spark/monitor-and-manage-app.png)

4. İçinde **etkinlik windows** listesinde, ilgili ayrıntıları görmek için bir etkinlik seçin. Bir hata varsa, sağ bölmede hakkındaki ayrıntıları bakın.

### <a name="verify-the-results"></a>Sonuçları doğrulayın

1. Hdınsight Spark kümenizin Jupyter not defteri adresine giderek başlatın [bu Web sitesi](https://CLUSTERNAME.azurehdinsight.net/jupyter). İçin Hdınsight Spark küme panosu da açabilirsiniz küme ve Jupyter not defteri başlatın.

2. Seçin **yeni** > **PySpark** yeni bir not defteri başlatmak için.

    ![Yeni Jupyter not defteri](media/data-factory-spark/jupyter-new-book.png)

3. Kopyalama ve metin yapıştırma ve ikinci ifade sonunda SHIFT + Enter tuşlarına basarak aşağıdaki komutu çalıştırın:

    ```sql
    %%sql

    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```
4. Hvac tablodan veri gördüğünüzü onaylayın. 

    ![Jupyter sorgu sonuçları](media/data-factory-spark/jupyter-notebook-results.png)

<!-- Removed bookmark #run-a-hive-query-using-spark-sql since it doesn't exist in the target article --> Ayrıntılı yönergeler için bkz [bir Spark SQL sorgusu çalıştırmanız](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). 

### <a name="troubleshooting"></a>Sorun giderme
Getdebugınfo kümesine çünkü **her zaman**, blob kapsayıcınızdaki pyFiles klasöründe bir günlük alt bakın. Günlük klasöründeki günlük dosyası ek bilgiler sağlar. Bu günlük dosyası bir hata olduğunda özellikle yararlıdır. Bir üretim ortamında ayarlamak isteyebilirsiniz **hatası**.

Daha fazla sorun giderme için aşağıdaki adımları uygulayın:


1. `https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster` kısmına gidin.

    ![YARN kullanıcı arabirimini uygulama](media/data-factory-spark/yarnui-application.png)

2. Seçin **günlükleri** Çalıştır biri için çalışır.

    ![Uygulama sayfası](media/data-factory-spark/yarn-applications.png)

3. Günlük sayfasında aşağıdaki ek hata bilgileri görebilirsiniz:

    ![Günlük hatası](media/data-factory-spark/yarnui-application-error.png)

Aşağıdaki bölümler, Spark kümesi ve Spark etkinlik data factory'nizi kullanmak için data factory varlıklarını hakkında bilgi sağlar.

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

Aşağıdaki tabloda JSON tanımında kullanılan JSON özellikleri açıklanmaktadır.

| Özellik | Açıklama | Gerekli |
| -------- | ----------- | -------- |
| ad | İşlem hattında etkinlik adı. | Evet |
| açıklama | Etkinlik yaptığı açıklayan metin. | Hayır |
| type | Bu özellik için HDInsightSpark ayarlamanız gerekir. | Evet |
| linkedServiceName | Spark programın çalıştığı Hdınsight bağlı hizmetin adı. | Evet |
| rootPath | Blob kapsayıcısı ve Spark dosyasını içeren klasör. Dosya adı büyük/küçük harfe duyarlıdır. | Evet |
| entryFilePath | Spark kod/paketi kök klasörüne göreli yolu. | Evet |
| className | Uygulamanın Java/Spark ana sınıfı. | Hayır |
| bağımsız değişkenler | Spark programın komut satırı bağımsız değişkenleri listesi. | Hayır |
| proxyUser | Spark program yürütülmeye kimliğine bürünmek için kullanıcı hesabı. | Hayır |
| sparkConfig | Listelenen Spark yapılandırma özellikleri için değerleri belirtin [Spark yapılandırma: uygulama özellikleri](https://spark.apache.org/docs/latest/configuration.html#available-properties). | Hayır |
| Getdebugınfo | Hdınsight küme tarafından kullanılan depolama Spark günlük dosyalarının ne zaman kopyalanır belirtir (veya) sparkJobLinkedService tarafından belirtilen. İzin verilen değerler, her zaman veya hata yok. Varsayılan değer Yok'tur. | Hayır |
| sparkJobLinkedService | Depolama Spark iş dosyası, bağımlılıklar ve günlükleri tutan hizmeti bağlı. Bu özellik için bir değer belirtmezseniz, Hdınsight kümesi ile ilişkili depolama kullanılır. | Hayır |

## <a name="folder-structure"></a>Klasör yapısı
Spark etkinlik Pig olarak bir satır içi betiği desteklemez ve Hive etkinlikleri yapın. Spark işleri de Pig/Hive işleri daha fazla genişletilebilir. Spark işleri, birden çok bağımlılıkları gibi sağlayabilirsiniz jar (java sınıf yerleştirilen) paketleri, Python dosyaları (PYTHONPATH yerleştirilmiş) ve diğer dosyaları.

Hdınsight bağlı hizmeti tarafından başvurulan blob depolamada aşağıdaki klasör yapısını oluşturun. Ardından, bağımlı dosyaları tarafından temsil edilen kök klasöründe uygun alt karşıya **entryFilePath**. Örneğin, Python dosyaları pyFiles alt klasöre yüklemek ve kök klasörünün Kavanoz alt dosyalara jar. Çalışma zamanında Data Factory hizmetinin blob depolama alanına aşağıdaki klasör yapısını bekler: 

| Yol | Açıklama | Gerekli | Tür |
| ---- | ----------- | -------- | ---- |
| . | Depolama bağlantılı hizmeti Spark işinde kök yolu. | Evet | Klasör |
| &lt;Kullanıcı tanımlı &gt; | Spark iş girişi dosyasına işaret yolu. | Evet | Dosya |
| . / jar'lar | Bu klasörü altındaki tüm dosyaları karşıya ve küme Java sınıf yolu yerleştirilir. | Hayır | Klasör |
| . / pyFiles | Bu klasörü altındaki tüm dosyaları karşıya ve küme PYTHONPATH üzerinde yerleştirilir. | Hayır | Klasör |
| . / dosyaları | Bu klasörü altındaki tüm dosyaları karşıya ve yürütücü çalışma dizini yerleştirilir. | Hayır | Klasör |
| . / arşivler | Bu klasörü altındaki tüm dosyaları sıkıştırılmamış. | Hayır | Klasör |
| . / günlükleri | Spark kümesi günlüklerinden depolandığı klasörü.| Hayır | Klasör |

Hdınsight bağlı hizmeti tarafından başvurulan blob depolama alanında iki Spark iş dosyalarını içeren depolama için örnek aşağıda verilmiştir:

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
