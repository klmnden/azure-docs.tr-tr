---
title: "Azure Data Factory’de Spark kullanarak verileri dönüştürme | Microsoft Docs"
description: "Bu öğretici, Azure Data Factory'de Spart Etkinliğini kullanarak verileri dönüştürmeye ilişkin adım adım yönergeler sağlar."
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/10/2018
ms.author: shengc
ms.openlocfilehash: c2ec6706c92f229bb05ad9a19246c6ffe5f615c9
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="transform-data-in-the-cloud-by-using-spark-activity-in-azure-data-factory"></a>Azure Data Factory'de Spark etkinliğini kullanarak verileri bulutta dönüştürme
Bu öğreticide, Azure portalını kullanarak verileri Spark Etkinliği ve isteğe bağlı bir HDInsight bağlı hizmeti ile dönüştüren bir Data Factory işlem hattı oluşturursunuz. Bu öğreticide aşağıdaki adımları gerçekleştireceksiniz:

> [!div class="checklist"]
> * Veri fabrikası oluşturma. 
> * Spark etkinliği ile işlem hattı oluşturma
> * İşlem hattı çalıştırmasını tetikleme
> * İşlem hattı çalıştırmasını izleme.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Data Factory hizmetinin genel kullanıma açık (GA) 1. sürümünü kullanıyorsanız [Data Factory sürüm 1 belgeleri](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) konusunu inceleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="prerequisites"></a>Ön koşullar
* **Azure Depolama hesabı**. Bir python betiği ve giriş dosyası oluşturup Azure depolama alanına yükleyin. Spark programının çıktısı bu depolama hesabında depolanır. İsteğe bağlı Spark kümesi, birincil depolama alanıyla aynı depolama hesabını kullanır.  
* **Azure PowerShell**. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/install-azurerm-ps) bölümündeki yönergeleri izleyin.


### <a name="upload-python-script-to-your-blob-storage-account"></a>Python betiğini Blob Depolama hesabınıza yükleme
1. Aşağıdaki içeriğe sahip **WordCount_Spark.py** adlı bir python dosyası oluşturun: 

    ```python
    import sys
    from operator import add
    
    from pyspark.sql import SparkSession
    
    def main():
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
            
        lines = spark.read.text("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/inputfiles/minecraftstory.txt").rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' ')) \
            .map(lambda x: (x, 1)) \
            .reduceByKey(add)
        counts.saveAsTextFile("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/outputfiles/wordcount")
        
        spark.stop()
    
    if __name__ == "__main__":
        main()
    ```
2. **&lt;storageAccountName&gt;**’i Azure Depolama hesabınızın adıyla değiştirin. Ardından dosyayı kaydedin. 
3. Azure Blob depolama alanınızda henüz yoksa **adftutorial** adlı bir kapsayıcı oluşturun. 
4. **Spark** adlı bir klasör oluşturun.
5. **Spark** klasörünün altında **script** adlı bir alt klasör oluşturun. 
6. **WordCount_Spark.py** dosyasını **script** alt klasörüne yükleyin. 


### <a name="upload-the-input-file"></a>Girdi dosyasını yükleme
1. Bazı metinlerle **minecraftstory.txt** adlı bir dosya oluşturun. Spark programı bu metindeki sözcükleri sayar. 
2. `spark` klasöründe `inputfiles` adlı bir alt klasör oluşturun. 
3. `minecraftstory.txt` dosyasını `inputfiles` alt klasörüne yükleyin. 

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma

1. Soldaki menüde **Yeni**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın. 
   
   ![Yeni->DataFactory](./media/tutorial-transform-data-spark-portal/new-azure-data-factory-menu.png)
2. **Yeni veri fabrikası** sayfasında **ad** için **ADFTutorialDataFactory** girin. 
      
     ![Yeni veri fabrikası sayfası](./media/tutorial-transform-data-spark-portal/new-azure-data-factory.png)
 
   Azure data factory adı **küresel olarak benzersiz** olmalıdır. Ad alanı için aşağıdaki hatayı görürseniz veri fabrikasının adını değiştirin (örneğin, adınızADFTutorialDataFactory). Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](naming-rules.md) makalesine bakın.
  
     ![Ad yok - hata](./media/tutorial-transform-data-spark-portal/name-not-available-error.png)
3. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
4. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
     
      - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
      - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
      Bu hızlı başlangıçtaki adımlardan bazıları kaynak grubu için şu adı kullandığınızı varsayar: **ADFTutorialResourceGroup**. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-overview.md).  
4. **Sürüm** için **V2 (Önizleme)** öğesini seçin.
5. Data factory için **konum** seçin. Data Factory V2 şu anda Doğu ABD, Doğu ABD 2 ve Batı Avrupa bölgelerinde veri fabrikası oluşturmanıza olanak sağlar. Veri fabrikası tarafından kullanılan verileri depoları (Azure Depolama, Azure SQL Veritabanı vb.) ve işlemler (HDInsight vb.) başka bölgelerde olabilir.
6. **Panoya sabitle**’yi seçin.     
7. **Oluştur**’a tıklayın.
8. Panoda şu kutucuğu ve üzerinde şu durumu görürsünüz: **Veri fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media//tutorial-transform-data-spark-portal/deploying-data-factory.png)
9. Oluşturma işlemi tamamlandıktan sonra, resimde gösterildiği gibi **Data Factory** sayfasını görürsünüz.
   
    ![Data factory giriş sayfası](./media/tutorial-transform-data-spark-portal/data-factory-home-page.png)
10. Data Factory kullanıcı arabirimi uygulamasını ayrı bir sekmede açmak için **Geliştir ve İzle** kutucuğuna tıklayın.

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Bu bölümde iki Bağlı Hizmet oluşturacaksınız: 
    
- Bir Azure Depolama hesabını veri fabrikasına bağlayan **Azure Depolama Bağlı Hizmeti**. Bu depolama alanı, isteğe bağlı HDInsight kümesi tarafından kullanılır. Ayrıca, yürütülecek Spark betiğini içerir. 
- **İsteğe bağlı HDInsight Bağlı Hizmeti**. Azure Data Factory otomatik olarak bir HDInsight kümesi oluşturur, Spark programını çalıştırır ve önceden yapılandırılmış bir süre boyunca boşta kaldıktan sonra HDInsight kümesini siler. 

### <a name="create-an-azure-storage-linked-service"></a>Azure Depolama bağlı hizmeti oluşturma

1. **Başlarken** sayfasında, aşağıdaki resimde gösterildiği gibi sol bölmede bulunan **Düzenle** sekmesine geçin: 

    ![İşlem hattı oluştur kutucuğu](./media/tutorial-transform-data-spark-portal/get-started-page.png)

2. Pencerenin en altından **Bağlantılar**’a ve sonra **+ Yeni**’ye tıklayın. 

    ![Yeni bağlantı düğmesi](./media/tutorial-transform-data-spark-portal/new-connection.png)
3. **Yeni Bağlı Hizmet** penceresinde **Azure Blob Depolama**’yı seçip **Devam**’a tıklayın. 

    ![Azure Blob Depolama’yı seçin](./media/tutorial-transform-data-spark-portal/select-azure-storage.png)
4. **Azure Depolama hesabınızın adını** seçip **Kaydet**’e tıklayın. 

    ![Blob Depolama hesabını belirtme](./media/tutorial-transform-data-spark-portal/new-azure-storage-linked-service.png)


### <a name="create-an-on-demand-hdinsight-linked-service"></a>İsteğe bağlı bir HDInsight bağlı hizmeti oluşturma

1. Bir kere daha **Yeni** düğmesine tıklayarak başka bir bağlı hizmet oluşturun. 
2. **Yeni Bağlı Hizmet** penceresinde **Azure HDInsight**’ı seçip **Devam**’a tıklayın. 

    ![Azure HDInsight’ı seçin](./media/tutorial-transform-data-spark-portal/select-azure-hdinsight.png)
2. **Yeni Bağlı Hizmet** penceresinde aşağıdaki adımları uygulayın: 

    1. **Ad** için **AzureHDInsightLinkedService** adını girin.
    2. **Tür** için **İsteğe bağlı HDInsight**’ın seçildiğini onaylayın.
    3. **Azure Depolama Bağlı Hizmeti** için **AzureStorage1**’i seçin. Bu bağlı hizmeti daha önce oluşturmuştunuz. Farklı bir ad kullandıysanız, bu alan için doğru adı belirtin. 
    4. **Küme türü** için **spark**’ı seçin.
    5. HDInsight kümesi oluşturma yetkisi olan **hizmet sorumlusunun kimliğini** girin. Bu hizmet sorumlusu, abonelikte ya da kümenin oluşturulduğu kaynak grubunda Katkıda Bulunan rolünün bir üyesi olmalıdır. Ayrıntılar için bkz. [Azure Active Directory uygulaması ve hizmet sorumlusu oluşturma](../azure-resource-manager/resource-group-create-service-principal-portal.md).
    6. **Hizmet sorumlusu anahtarını** girin. 
    7. **Kaynak grubu** için veri fabrikası oluştururken kullandığınız kaynak grubunu seçin. Spark kümesi bu kaynak grubunda oluşturulur. 
    8. **İşletim sistemi türü** seçeneğini genişletin.
    9. Küme **kullanıcısı** için bir **ad** girin. 
    10. Kullanıcının **parolasını** girin. 
    11. **Kaydet**’e tıklayın. 

        ![HDInsight bağlı hizmet ayarları](./media/tutorial-transform-data-spark-portal/azure-hdinsight-linked-service-settings.png)

> [!NOTE]
> Azure HDInsight, desteklediği her bir Azure bölgesinde kullanabileceğiniz toplam çekirdek sayısıyla ilgili sınırlamaya sahiptir. İsteğe Bağlı HDInsight Bağlı Hizmeti için HDInsight kümesi, birincil depolama olarak kullanılan Azure Depolama konumunda oluşturulur. Kümenin başarıyla oluşturulabilmesi için yeterince çekirdek kotanızın olduğundan emin olun. Daha fazla bilgi için bkz. [HDInsight’ta Hadoop, Spark, Kafka ve daha fazlası ile küme ayarlama](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md). 

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

2. + (artı) düğmesine tıklayın ve menüden **İşlem Hattı**’na tıklayın.

    ![Yeni işlem hattı menüsü](./media/tutorial-transform-data-spark-portal/new-pipeline-menu.png)
3. **Etkinlikler** araç kutusunda **HDInsight**’ı genişletin ve **Etkinlikler** araç kutusundan **Spark** etkinliğini sürükleyip işlem hattı tasarımcısının yüzeyine bırakın. 

    ![Spark etkinliğini sürükleyip bırakma](./media/tutorial-transform-data-spark-portal/drag-drop-spark-activity.png)
4. **Spark** etkinliği penceresinin altındaki özellikler bölümünden aşağıdaki adımları uygulayın: 

    1. **HDI Kümesi** sekmesine geçin.
    2. Önceki adımda oluşturduğunuz **AzureHDInsightLinkedService** hizmetini seçin. 
        
    ![HDInsight bağlı hizmeti belirtme](./media/tutorial-transform-data-spark-portal/select-hdinsight-linked-service.png)
5. **Betik/Jar** sekmesine geçin ve aşağıdaki adımları uygulayın: 

    1. **İş Bağlı Hizmeti** için **AzureStorage1**’i seçin.
    2. **Depolamaya Gözat**’a tıklayın. 
    3. **adftutorial/spark/script klasörüne** gidin, **WordCount_Spark.py** dosyasını seçin ve **Son**’a tıklayın.      

    ![Spark betiği belirtme](./media/tutorial-transform-data-spark-portal/specify-spark-script.png)
6. İşlem hattını doğrulamak için araç çubuğundaki **Doğrula** düğmesine tıklayın. Doğrulama penceresini kapatmak için **sağ ok** (>>) düğmesine tıklayın. 
    
    ![Doğrula düğmesi](./media/tutorial-transform-data-spark-portal/validate-button.png)
7. **Yayımla**’ta tıklayın. Data Factory kullanıcı arabirimi, varlıkları (bağlı hizmetler ve işlem hattı) Azure Data Factory hizmetinde yayımlar. 
    
    ![Yayımla düğmesi](./media/tutorial-transform-data-spark-portal/publish-button.png)

## <a name="trigger-a-pipeline-run"></a>İşlem hattı çalıştırmasını tetikleme
Araç çubuğunda **Tetikle**’ye tıklayıp **Şimdi Tetikle**’ye tıklayın. 

![Şimdi tetikle](./media/tutorial-transform-data-spark-portal/trigger-now-menu.png)

## <a name="monitor-the-pipeline-run"></a>İşlem hattı çalıştırmasını izleme

1. **İzleyici** sekmesine geçin. Bir işlem hattı çalıştırması gördüğünüzü onaylayın. Spark kümesi oluşturma işlemi yaklaşık 20 dakika sürer. 

    ![İşlem hattı çalıştırmalarını izleme](./media/tutorial-transform-data-spark-portal/monitor-tab.png)
2. Düzenli aralıklarla **Yenile**’ye tıklayarak işlem hattı çalıştırmasının durumunu denetleyin. 

    ![İşlem hattı çalıştırma durumu](./media/tutorial-transform-data-spark-portal/pipeline-run-succeeded.png)
1. İşlem hattı çalıştırmasıyla ilişkili etkinlik çalıştırmalarını görmek için Eylemler sütunundaki **Etkinlik Çalıştırmalarını Göster** eylemine tıklayın. Üstteki **İşlem Hatları** bağlantısına tıklayarak işlem hattı çalıştırmaları görünümüne dönebilirsiniz.

    ![Etkinlik Çalıştırmaları görünümü](./media/tutorial-transform-data-spark-portal/activity-runs.png)

## <a name="verify-the-output"></a>Çıktıyı doğrulama
adftutorial kapsayıcısının spark/otuputfiles/wordcount klasöründe çıktı dosyasının oluşturulduğunu doğrulayın. 

![Çıktıyı doğrulama](./media/tutorial-transform-data-spark-portal/verity-output.png)

Dosya, girdi metin dosyasındaki her bir sözcüğü ve sözcüğün dosyada görünme sayısını içermelidir. Örnek: 

```
(u'This', 1)
(u'a', 1)
(u'is', 1)
(u'test', 1)
(u'file', 1)
```

## <a name="next-steps"></a>Sonraki adımlar
Bu örnekteki işlem hattı, Spark etkinliğini ve isteğe bağlı bir HDInsight bağlı hizmetini kullanarak verileri dönüştürür. Şunları öğrendiniz: 

> [!div class="checklist"]
> * Veri fabrikası oluşturma. 
> * Spark etkinliği ile işlem hattı oluşturma
> * İşlem hattı çalıştırmasını tetikleme
> * İşlem hattı çalıştırmasını izleme.

Sanal ağdaki bir Azure HDInsight kümesinde Hive betiği çalıştırarak verileri dönüştürme hakkında bilgi almak için sonraki öğreticiye ilerleyin. 

> [!div class="nextstepaction"]
> [Öğretici: Azure Sanal Ağ’da Hive kullanarak verileri dönüştürme](tutorial-transform-data-hive-virtual-network-portal.md).





