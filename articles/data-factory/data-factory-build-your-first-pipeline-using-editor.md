---
title: "İlk data factory’nizi derleme (Azure portalı) | Microsoft Belgeleri"
description: "Bu öğreticide, Azure Portal&quot;daki Data Factory Düzenleyiciyi kullanarak örnek bir Azure Data Factory işlem hattı oluşturursunuz."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d5b14e9e-e358-45be-943c-5297435d402d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/06/2016
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: cfbfccfe09e6f2b3826223a779a5ff478c1f804f
ms.openlocfilehash: 64250a0b37488eb165bd13e727f365bd391794b7


---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a>Öğretici: Azure portal kullanarak ilk Azure data factory’nizi derleme
> [!div class="op_single_selector"]
> * [Genel bakış ve önkoşullar](data-factory-build-your-first-pipeline.md)
> * [Azure portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Resource Manager Şablonu](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)
>
>

Bu makalede, ilk Azure data factory’nizi oluşturmak için [Azure Portal](https://portal.azure.com/) kullanmayı öğrenirsiniz.

## <a name="prerequisites"></a>Önkoşullar
1. [Öğreticiye Genel Bakış](data-factory-build-your-first-pipeline.md) makalesinin tamamını okuyun ve **ön koşul** adımlarını tamamlayın.
2. Bu makale, Azure Data Factory hizmetine kavramsal bir genel bakış sağlamaz. Hizmet hakkında ayrıntılı bir genel bakış için [Azure Data Factory'ye giriş](data-factory-introduction.md) makalesine gitmenizi öneririz.  

## <a name="create-data-factory"></a>Veri fabrikası oluşturma
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. İşlem hattında bir veya daha fazla etkinlik olabilir. Örneğin, verileri bir kaynaktan bir hedef veri deposuna kopyalamak için Kopyalama Etkinliği, girdi verilerini ürün çıktı verilerine dönüştürecek Hive betiğini çalıştırmak için de HDInsight Hive etkinliği. Bu adımda data factory oluşturmayla başlayalım.

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
2. Soldaki menüde **YENİ**, **Veri + Analiz** ve **Data Factory** öğesine tıklayın.

   ![Dikey pencere oluşturma](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)
3. **Yeni data factory** dikey penceresinde, Ad olarak **GetStartedDF** girin.

   ![Yeni veri fabrikası dikey penceresi](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

   > [!IMPORTANT]
   > Azure data factory adı **küresel olarak benzersiz** olmalıdır. **Veri fabrikası adı "GetStartedDF" kullanılamıyor** hatasını alırsanız. Veri fabrikasının adını (örneğin adınızBaşlarkenVF) değiştirin ve yeniden oluşturmayı deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.
   >
   > Data factory adı gelecekte bir **DNS** adı olarak kaydedilmiş olabilir; bu nedenle herkese görünür hale gelmiştir.
   >
   >
4. Data factory’yi oluşturmak istediğiniz **Azure aboneliği**’ni seçin.
5. Mevcut bir **kaynak grubu** seçin ya da bir kaynak grubu oluşturun. Öğreticide kullanmak için şu adla bir kaynak grubu oluşturun: **ADFGetStartedRG**.
6. **Yeni data factory** dikey penceresinde **Oluştur**’a tıklayın.

   > [!IMPORTANT]
   > Data Factory örnekleri oluşturmak için abonelik/kaynak grubu düzeyinde [Data Factory Katılımcısı](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) rolünün üyesi olmanız gerekir.
   >
   >
7. Oluşturulmakta olan data factory’yi Azure Portal’ın **Başlangıç panosu**’nda şöyle görürsünüz:   

   ![Data factory durumu oluşturma](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
8. Tebrikler! İlk data factory’nizi başarıyla oluşturdunuz. Data factory sorunsuz oluşturulduktan sonra data factory sayfasını görürsünüz, burada size data factory içeriği gösterilir.     

    ![Data Factory dikey penceresi](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

Data factory içinde bir işlem hattı oluşturmadan önce birkaç Data Factory varlığı oluşturmanız gerekir. Önce veri depolarını/işlemleri veri deponuza bağlamak için bağlı hizmetler oluşturun, bağlı veri depolarında giriş/çıkış verilerini temsil etmek üzere giriş ve çıkış veri kümeleri tanımlayın, sonra da bu veri kümelerini kullanan bir etkinlikle işlem hattını oluşturun.

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Bu adımda, Azure Depolama hesabınızı ve isteğe bağlı Azure HDInsight kümesini data factory’nize bağlarsınız. Azure Depolama hesabı, bu örnekteki işlem hattı için girdi ve çıktı verilerini tutar. HDInsight bağlı hizmeti, bu örnekte işlem hattının etkinliğinde belirtilen Hive betiğini çalıştırmak için kullanılır. Senaryonuzda hangi [veri deposu](data-factory-data-movement-activities.md)/[işlem hizmetlerinin](data-factory-compute-linked-services.md) kullanılacağını belirleyin ve bağlı hizmetler oluşturarak bu hizmetleri data factory’ye bağlayın.  

### <a name="create-azure-storage-linked-service"></a>Azure Storage bağlı hizmeti oluşturma
Bu adımda, Azure Depolama hesabınızı veri fabrikanıza bağlarsınız. Bu öğreticide, giriş/çıkış verilerini ve HQL betik dosyasını depolamak için aynı Azure Depolama hesabını kullanırsınız.

1. **GetStartedDF** için **DATA FACTORY** dikey penceresinde **Geliştir ve dağıt**’a tıklayın. Data Factory Düzenleyicisi’ni görmeniz gerekir.

   ![Geliştir ve dağıt kutucuğu](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2. **Yeni data store**’a tıklayın ve **Azure depolama**’yı seçin.

   ![Yeni veri deposu - Azure Depolama - menü](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)
3. Düzenleyicide Azure Storage bağlı hizmeti oluşturmak için JSON betiğini görmeniz gerekir.

   ![Azure Storage bağlı hizmeti](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. **accountname** sözcüğünü Azure depolama hesabınızın adıyla, **accountkey** sözcüğünü de Azure depolama hesabının erişim anahtarıyla değiştirin. Depolama erişim anahtarınızı nasıl alabileceğinizi öğrenmek için [Depolama hesabınızı yönetme](../storage/storage-create-storage-account.md#manage-your-storage-account) sayfasındaki depolama erişim anahtarlarını görüntüleme, kopyalama ve yeniden oluşturma bilgilerine bakın.
5. Bağlı hizmeti dağıtmak için komut çubuğunda **Dağıt**’a tıklayın.

    ![Dağıt düğmesi](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   Bağlı hizmet sorunsuz dağıtıldıktan sonra **Taslak-1** penceresi artık görünmemelidir; soldaki ağaç görünümünde **AzureStorageLinkedService** görürsünüz.
    ![Menüde Depolama Bağlı Hizmeti](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)    

### <a name="create-azure-hdinsight-linked-service"></a>Azure HDInsight bağlı hizmeti oluşturma
Bu adımda, isteğe bağlı HDInsight kümesini data factory’nize bağlarsınız. HDInsight kümesi çalışma zamanında otomatik olarak oluşturulur ve işlenmesi bittiğinde ve belirtilen sürede boşta kalırsa silinir.

1. **Data Factory Düzenleyicisi**’nde komut çubuğundaki **... Diğer**, **Yeni işlem** öğelerine tıklayın ve **İsteğe bağlı HDInsight kümesi**’ni seçin.

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
   |:--- |:--- |
   | Sürüm |Oluşturulan HDInsight sürümünün 3.2 olması gerektiğini belirtir. |
   | ClusterSize |HDInsight kümesi boyutunu belirtir. |
   | TimeToLive |Silinmeden önce HDInsight kümesinin boşta kalma süresini belirtir. |
   | linkedServiceName |HDInsight tarafından oluşturulan günlükleri depolamak için kullanılan depolama hesabını belirtir. |

    Aşağıdaki noktalara dikkat edin:

   * Data Factory, sizin için JSON ile **Windows tabanlı** bir HDInsight kümesi oluşturur. **Linux tabanlı** bir HDInsight kümesi de oluşturabilirsiniz. Ayrıntılar için bkz. [İsteğe Bağlı HDInsight Bağlı Hizmeti](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).
   * İsteğe bağlı HDInsight kümesi yerine **kendi HDInsight kümenizi** kullanabilirsiniz. Ayrıntılar için bkz. [HDInsight Bağlı Hizmeti](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).
   * HDInsight kümesi JSON’da belirttiğiniz blob depolamada (**linkedServiceName**) bir **varsayılan kapsayıcı** oluşturur. HDInsight, küme silindiğinde bu kapsayıcıyı silmez. Bu davranış tasarım gereğidir. İsteğe bağlı HDInsight bağlı hizmeti kullanıldığında, mevcut canlı bir küme olmadığı sürece bir dilim her işlendiğinde bir HDInsight kümesi oluşturulur (**timeToLive**). Küme, işlem tamamlandığında otomatik olarak silinir.

       Daha fazla dilim işlendikçe, Azure blob depolamanızda çok sayıda kapsayıcı görürsünüz. İşlerin sorunları giderilmesi için bunlara gerek yoksa, depolama maliyetini azaltmak için bunları silmek isteyebilirsiniz. Bu kapsayıcıların adları şu deseni izler: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp". Azure blob depolamada kapsayıcı silmek için [Microsoft Storage Gezgini](http://storageexplorer.com/) gibi araçları kullanın.

     Ayrıntılar için bkz. [İsteğe Bağlı HDInsight Bağlı Hizmeti](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).
3. Bağlı hizmeti dağıtmak için komut çubuğunda **Dağıt**’a tıklayın.

    ![İsteğe bağlı HDInsight bağlı hizmetini dağıtma](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)
4. Hem **AzureStorageLinkedService**, hem de **HDInsightOnDemandLinkedService** öğesinin soldaki ağaç görünümünde olduğunu onaylayın.

    ![Bağlı hizmetlerin bulunduğu ağaç görünümü](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a>Veri kümeleri oluşturma
Bu adımda, Hive işlenmesi için girdi ve çıktı verilerini temsil edecek veri kümeleri oluşturursunuz. Bu veri kümeleri, bu öğreticide daha önce oluşturduğunuz **AzureStorageLinkedService** öğesine başvurur. Bağlı hizmet Azure Storage hesabını belirtirken, veri kümeleri de girdi ve çıktı verilerini tutan depolama biriminde kapsayıcı, klasör, dosya adı belirtir.   

### <a name="create-input-dataset"></a>Girdi veri kümesi oluşturma
1. **Data Factory Düzenleyicisi**’nde komut çubuğundaki **... Diğer**, **Yeni veri kümesi** öğelerine tıklayın ve **Azure Blob depolama** öğesini seçin.

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
   |:--- |:--- |
   | type |Veriler Azure blob depolamada yer aldığından type özelliği AzureBlob olarak ayarlanmıştır. |
   | linkedServiceName |daha önce oluşturduğunuz AzureStorageLinkedService’e başvurur. |
   | fileName |Bu özellik isteğe bağlıdır. Bu özelliği atarsanız, tüm folderPath dosyaları alınır. Bu durumda, yalnızca input.log işlenir. |
   | type |Günlük dosyaları metin biçiminde olduğundan TextFormat kullanacağız. |
   | columnDelimiter |Günlük dosyalarındaki sütunlar virgül (,) ile ayrılmıştır. |
   | frequency/interval |frequency Ay, interval de 1 olarak ayarlanmıştır; girdi dilimlerinin aylık olarak kullanılabileceğini belirtir. |
   | external |bu özellik, girdi verileri Data Factory hizmetiyle oluşturulmadıysa true olarak ayarlanır. |
3. Yeni oluşturulan veri kümesini dağıtmak için komut çubuğunda **Dağıt**’a tıklayın. Veri kümesini soldaki ağaç görünümünde görmeniz gerekir.

### <a name="create-output-dataset"></a>Çıktı veri kümesi oluşturma
Şimdi, Azure Blob depolamada depolanan çıktı verilerini göstermek için çıktı veri kümesi oluşturursunuz.

1. **Data Factory Düzenleyicisi**’nde komut çubuğundaki **... Diğer**, **Yeni veri kümesi** öğelerine tıklayın ve **Azure Blob depolama** öğesini seçin.  
2. Aşağıdaki kod parçacığını kopyalayıp Taslak-1 penceresine yapıştırın. JSON parçacığında, **AzureBlobOutput** adlı bir veri kümesi oluşturur ve Hive betiğinin oluşturacağı verilerin yapısını belirtirsiniz. Ek olarak, sonuçların **adfgetstarted** adlı blob kapsayıcısında ve **partitioneddata** adlı klasörde depolandığını belirtin. Burada, **availability** bölümü çıktı veri kümesinin aylık tabanda oluşturulduğunu belirtiyor.

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

## <a name="create-pipeline"></a>İşlem hattı oluşturma
Bu adımda, **HDInsightHive** etkinliğiyle ilk işlem hattınızı oluşturursunuz. Girdi diliminin ayda bir (frequency: Month, interval: 1) kullanılabilir, çıktı dilimi ayda bir oluşturulur ve etkinlik zamanlayıcı özelliği de ayda bir olacak şekilde ayarlanır. Çıktı veri kümesi ve etkinlik zamanlayıcı ayarlarının eşleşmesi gerekir. Şu anda, çıktı veri kümesi zamanlamayı yönetendir; bu nedenle etkinlik hiçbir çıktı oluşturmasa bile sizin bir çıktı veri kümesi oluşturmanız gerekir. Etkinlik herhangi bir girdi almazsa, girdi veri kümesi oluşturma işlemini atlayabilirsiniz. Aşağıdaki JSON’da kullanılan özellikler bu bölümün sonunda anlatılmaktadır.

1. **Data Factory Düzenleyicisi**’nde, **Ellipsis (…) More commands**’e (Üç nokta (…) Daha fazla komut’a) sonra da **Yeni işlem hattı**’na tıklayın.

    ![yeni işlem hattı düğmesi](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. Aşağıdaki kod parçacığını kopyalayıp Taslak-1 penceresine yapıştırın.

   > [!IMPORTANT]
   > **storageaccountname**’i JSON’daki depolama adınızla değiştirin.
   >
   >

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

   > [!NOTE]
   > Örnekte kullanılan JSON özellikleri hakkında ayrıntılı bilgi için [Azure Data Factory’deki işlem hatları ve etkinlikler](data-factory-create-pipelines.md) sayfasındaki “JSON İşlem Hatları” bölümüne bakın.
   >
   >
3. Şunları onaylayın:

   1. Azure blob depolamada **adfgetstarted** kapsayıcısının **inputdata** klasöründe **input.log** dosyasının olduğunu
   2. Azure blob depolamada **adfgetstarted** kapsayıcısının **script** klasöründe **partitionweblogs.hql** dosyasının olduğunu. Bu dosyaları görmüyorsanız lütfen [Öğreticiye Genel Bakış](data-factory-build-your-first-pipeline.md)’taki önkoşul adımları tamamlayın.
   3. **storageaccountname**’i JSON işlem hattındaki depolama adınızla değiştirdiğinizi onaylayın.
4. İşlem hattını dağıtmak için komut çubuğunda **Dağıt**’a tıklayın. **start** ve **end** zamanları geçmişe ayarlanmış ve **isPaused** yanlış olarak ayarlanmış olduğundan işlem hattı (işlem hattında etkinlik) dağıtıldıktan hemen sonra çalışır.
5. İşlem hattını ağaç görünümünde gördüğünüzü onaylayın.

    ![İşlem hattının bulunduğu ağaç görünümü](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
6. Tebrikler, ilk işlem hattınızı başarıyla oluşturdunuz.

## <a name="monitor-pipeline"></a>İşlem hattını izleme
### <a name="monitor-pipeline-using-diagram-view"></a>Diyagram Görünümünü kullanarak işlem hattını izleme
1. Data Factory Düzenleyici dikey penceresini kapatmak ve Data Factory dikey penceresine dönmek için **X** işaretine, sonra da **Diyagram**’a tıklayın.

    ![Diyagram kutucuğu](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
2. Diyagram Görünümü’nde, işlem hatlarına ve bu öğreticide kullanılan veri kümelerine bir genel bakış görürsünüz.

    ![Diyagram Görünümü](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png)
3. İşlem hattındaki tüm etkinlikleri görüntülemek için diyagramdaki işlem hattına sağ tıklayın ve Açık İşlem Hattı’na tıklayın.

    ![İşlem hattı menüsünü açma](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
4. İşlem hattında HDInsightHive etkinliğini gördüğünüzü onaylayın.

    ![İşlem hattı görünümünü açma](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    Önceki görünüme dönmek için en üstteki içerik haritası menüsünde **Data factory**’ye tıklayın.
5. **Diyagram Görünümü**’nde **AzureBlobInput** veri kümesine çift tıklayın. Dilimin **Hazır** durumunda olduğunu onaylayın. Dilimin Hazır durumda gösterilmesi birkaç dakika alabilir. Bir süre bekledikten sonra bu gerçekleşmiyorsa, girdi dosyasının (input.log) doğru kapsayıcıda (adfgetstarted) ve klasörde (inputdata) olup olmadığına bakın.

   ![Girdi dilimi hazır durumda](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
6. **AzureBlobInput** dikey penceresini kapatmak için **X** işaretine tıklayın.
7. **Diyagram Görünümü**’nde **AzureBlobOutput** veri kümesine çift tıklayın. Dilimin işlenmekte olduğunu görürsünüz.

   ![Veri kümesi](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
8. İşlem tamamlandığında dilimi **Hazır** durumunda görürsünüz.

   ![Veri kümesi](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png)  

   > [!IMPORTANT]
   > İsteğe bağlı HDInsight kümesinin oluşturulması genellikle biraz zaman alır (yaklaşık 20 dakika). Bu nedenle, işlem hattının dilimi işlemesi için **yaklaşık 30 dakika** bekleyin.
   >
   >

9. Dilim **Hazır** durumunda olduğunda çıktı verileri için blob depolama alanınızın **adfgetstarted** kapsayıcısında **partitioneddata** klasörünü denetleyin.  

   ![çıktı verileri](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
10. Dilimin ayrıntılarını bir **Veri dilimi** dikey penceresinde görmek için dilime tıklayın.

   ![Veri dilimi ayrıntıları](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
11. Bir etkinlik çalışmasına ilişkin ayrıntıları (bu senaryoda Hive etkinliği) bir **Etkinlik çalışma ayrıntıları** penceresinde görmek için **Etkinlik çalışma listesi** içinden bir etkinlik çalışmasına tıklayın.   

   ![Etkinlik çalışma ayrıntıları](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)    

   Yürütülen Hive sorgusunu ve durum bilgilerini günlük dosyalarında görebilirsiniz. Bu günlükler her türlü sorunu gidermek için kullanışlıdır.
   Daha fazla ayrıntı için [Azure portal dikey penceresi kullanılarak işlem hatlarını izleme ve yönetme](data-factory-monitor-manage-pipelines.md) makalesine bakın.

> [!IMPORTANT]
> Dilim başarıyla işlendiğinde girdi dosyası silinir. Bu nedenle, dilimi yeniden çalıştırmak veya öğreticiyi yeniden uygulamak isterseniz girdi dosyasını (input.log) adfgetstarted kapsayıcısının inputdata klasörüne yükleyin.
>
>

### <a name="monitor-pipeline-using-monitor--manage-app"></a>İzleme ve Yönetme Uygulamasını kullanarak işlem hattını izleme
İşlem hatlarınızı izlemek için İzleme ve Yönetme uygulamasını da kullanabilirsiniz. Bu uygulamanın kullanımına ilişkin ayrıntılı bilgi için bkz. [İzleme ve Yönetme Uygulamasını kullanarak Azure Data Factory işlem hatlarını izleme ve yönetme](data-factory-monitor-manage-app.md).

1. Data factory’nin giriş sayfasındaki **İzleme ve Yönetme** kutucuğuna tıklayın.

    ![İzleme ve Yönetme kutucuğu](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png)
2. **İzleme ve Yönetme uygulaması**’nı görmeniz gerekir. **Başlangıç saati** ve **Bitiş saati** değerlerini işlem hattınızın başlangıç (04-01-2016 12:00 AM) ve bitiş saatleri (04-02-2016 12:00 AM) ile eşleşecek şekilde değiştirin ve **Uygula**’ya tıklayın.

    ![İzleme ve Yönetme Uygulaması](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png)
3. Ayrıntılarını görmek için **Etkinlik Pencereleri** listesinden bir etkinlik penceresi seçin.
    ![Etkinlik penceresi ayrıntıları](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)

## <a name="summary"></a>Özet
Bu öğreticide, HDInsight hadoop kümesindeki Hive betiği çalıştırılarak verileri işlemek için bir Azure data factory oluşturdunuz. Aşağıdaki adımları uygulamak için Azure Portal’da Data Factory Düzenleyici’yi kullandınız:  

1. Oluşturulan Azure **data factory**.
2. Oluşturulan iki **bağlı hizmet**:
   1. Girdi/çıktı dosyalarını tutan Azure blob depolamanızı data factory’ye bağlamak için **Azure Storage** bağlı hizmeti.
   2. İsteğe bağlı HDInsight Hadoop kümesini data factory’ye bağlamak için isteğe bağlı **Azure HDInsight** bağlı hizmeti. Azure Data Factory, girdi verilerini işlemek, çıktı verilerini de oluşturmak için tam zamanında HDInsight Hadoop kümesi oluşturur.
3. İşlem hattındaki HDInsight Hive etkinliğiyle ilgili girdi ve çıktı verilerini açıklayan oluşturulan iki **veri kümesi**.
4. **HDInsight Hive** etkinliğine sahip oluşturulan bir **işlem hattı**.

## <a name="next-steps"></a>Sonraki Adımlar
Bu makalede, isteğe bağlı HDInsight kümesinde bir Hive betiği çalıştıran dönüştürme etkinliğine (HDInsight Etkinliği) sahip işlem hattı oluşturdunuz. Verileri Azure Blob’tan Azure SQL’e kopyalamak için Kopyalama Etkinliği’nin kullanılması hakkında bilgi için bkz. [Öğretici: Verileri Azure blob’tan Azure SQL’e kopyalama](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>Ayrıca Bkz.
| Konu | Açıklama |
|:--- |:--- |
| [Veri Dönüştürme Etkinlikleri](data-factory-data-transformation-activities.md) |Bu makalede, Azure Data Factory’nin desteklediği veri dönüştürme etkinliklerinin (bu öğreticide kullandığınız HDInsight Hive dönüştürmesi gibi) bir listesi sağlanmaktadır. |
| [Zamanlama ve yürütme](data-factory-scheduling-and-execution.md) |Bu makalede Azure Data Factory uygulama modelinin zamanlama ve yürütme yönleri açıklanmaktadır. |
| [İşlem hatları](data-factory-create-pipelines.md) |Bu makale, Azure Data Factory’de işlem hatlarının ve etkinliklerini anlamanıza ve senaryonuz ya da işletmeniz için uçtan uca veri odaklı iş akışları oluşturmak amacıyla bunları nasıl kullanacağınızı anlamanıza yardımcı olur. |
| [Veri kümeleri](data-factory-create-datasets.md) |Bu makale, Azure Data Factory’deki veri kümelerini anlamanıza yardımcı olur. |
| [İzleme Uygulaması kullanılarak işlem hatlarını izleme ve yönetme](data-factory-monitor-manage-app.md) |Bu makalede İzleme ve Yönetim Uygulaması kullanılarak işlem hatlarını izleme, yönetme ve hatalarını ayıklama işlemleri açıklanmaktadır. |



<!--HONumber=Dec16_HO1-->


