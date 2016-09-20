<properties
    pageTitle="İlk data factory’nizi derleme (PowerShell) | Microsoft Azure"
    description="Bu öğreticide Azure PowerShell kullanarak örnek bir Azure Data Factory işlem hattı oluşturursunuz."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"
/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/16/2016"
    ms.author="spelluru"/>

# Öğretici: Azure PowerShell kullanarak ilk Azure data factory’nizi derleme
> [AZURE.SELECTOR]
- [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
- [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Resource Manager Şablonu](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)


[AZURE.INCLUDE [data-factory-tutorial-prerequisites](../../includes/data-factory-tutorial-prerequisites.md)] 

## Ek önkoşullar
Öğreticiye Genel Bakış konusunda listelenen önkoşullar dışında aşağıdakileri yüklemeniz gerekir:

- **Azure PowerShell**. Bilgisayarınıza Azure PowerShell’in en son sürümünü yüklemek için [Azure PowerShell’i yükleme ve yapılandırma](../powershell-install-configure.md) makalesindeki yönergeleri izleyin.
- (isteğe bağlı) Bu makalede, tüm Data Factory cmdlet'lerini kapsamaz. Data Factory cmdlet’leri hakkında kapsamlı bilgi için bkz. [Data Factory Cmdlet Başvurusu](https://msdn.microsoft.com/library/dn820234.aspx). 

Azure PowerShell **sürüm < 1.0** kullanıyorsanız, [burada](https://msdn.microsoft.com/library/azure/dn820234.aspx) belgelenen cmdlet’leri kullanmalısınız. Data Factory cmdlet’lerini kullanmadan önce de aşağıdaki komutları çalıştırmanız gerekir: 
 
1. Azure PowerShell’i başlatın ve aşağıdaki komutları çalıştırın. Bu öğreticide sonuna kadar Azure PowerShell’i açık tutun. Kapatıp yeniden açarsanız, bu komutları yeniden çalıştırmanız gerekir.
    1. `Add-AzureAccount` komutunu çalıştırın ve Azure portalda oturum açmak için kullandığınız kullanıcı adı ve parolayı girin.
    2. Bu hesapla ilgili tüm abonelikleri görmek için `Get-AzureSubscription` komutunu çalıştırın.
    3. Çalışmak isteğiniz aboneliği seçmek için `Get-AzureRmSubscription -SubscriptionName NameOfAzureSubscription | Set-AzureRmContext` komutunu çalıştırın. **NameOfAzureSubscription** değerini Azure aboneliğinizin adıyla değiştirin.
4. Azure Data Factory cmdlet’leri bu modda kullanılabildiğinden Azure Resource Manager moduna geçin: `Switch-AzureMode AzureResourceManager`.

## Veri fabrikası oluşturma

Bu adımda **FirstDataFactoryPSH** adlı bir Azure Data Factory oluşturmak için Azure PowerShell’i kullanırsınız. Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. İşlem hattında bir veya daha fazla etkinlik olabilir. Örneğin, verileri bir kaynaktan bir hedef veri deposuna kopyalamak için bir Kopyalama Etkinliği ve giriş verilerini dönüştürecek Hive betiğini çalıştırmak için bir HDInsight Hive etkinliği. Bu adımda data factory oluşturmayla başlayalım. 

1. Azure PowerShell’i başlatın ve aşağıdaki komutu çalıştırın. Bu öğreticide sonuna kadar Azure PowerShell’i açık tutun. Kapatıp yeniden açarsanız, bu komutları yeniden çalıştırmanız gerekir.
    - `Login-AzureRmAccount` komutunu çalıştırın ve Azure portalda oturum açmak için kullandığınız kullanıcı adı ve parolayı girin.  
    - Bu hesapla ilgili tüm abonelikleri görmek için `Get-AzureRmSubscription` komutunu çalıştırın.
    - Çalışmak isteğiniz aboneliği seçmek için `Select-AzureRmSubscription <Name of the subscription>` komutunu çalıştırın. Bu abonelik Azure portalında kullanılanla aynı olmalıdır.
3. Aşağıdaki komutu kullanarak şu adda bir Azure kaynak grubu oluşturun: **ADFTutorialResourceGroup**.

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Bu öğreticideki adımlardan bazıları ADFTutorialResourceGroup adlı kaynak grubunu kullandığınızı varsayar. Farklı bir kaynak grubu kullanıyorsanız, bu öğreticide ADFTutorialResourceGroup yerine onu kullanmanız gerekir.
4. **FirstDataFactoryPSH** adında bir data factory oluşturan **New-AzureRmDataFactory** cmdlet’ini çalıştırın.  

        New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH –Location "West US"


Şunlara dikkat edin:
 
- Azure Data Factory adı küresel olarak benzersiz olmalıdır. Şu hatayı alırsanız: ** “FirstDataFactoryPSH” veri fabrikası adı yok**, adı değiştirin (örneğin, yournameFirstDataFactoryPSH). Bu öğreticide adımları uygularken ADFTutorialFactoryPSH yerine bu adı kullanın. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.
- Data Factory örnekleri oluşturmak için, Azure aboneliğinde katılımcı/yönetici rolünüz olmalıdır
- Data factory adı gelecekte bir DNS adı olarak kaydedilmiş olabilir; bu nedenle herkese görünür hale gelmiştir.
- Şu hatayı alırsanız: "**Abonelik, Microsoft.DataFactory ad alanını kullanacak şekilde kaydedilmemiş**", aşağıdakilerden birini yapın ve yeniden yayımlamayı deneyin: 

    - Azure PowerShell’de Data Factory sağlayıcısını kaydetmek için aşağıdaki komutu çalıştırın. 
        
            Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    
        Data Factory sağlayıcısının kayıtlı olduğunu onaylamak için aşağıdaki komutu çalıştırabilirsiniz. 
    
            Get-AzureRmResourceProvider
    - Azure aboneliğini kullanarak [Azure portalında](https://portal.azure.com) oturum açın ve Data Factory dikey penceresine gidin (ya da) Azure portalında bir data factory oluşturun. Bu eylem sağlayıcıyı sizin için otomatik olarak kaydeder.

İşlem hattı oluşturmadan önce, öncelikle birkaç Data Factory varlığı oluşturmanız gerekir. Önce veri depolarını/işlemleri veri deponuza bağlamak için bağlı hizmetler oluşturun, bağlı veri depolarında giriş/çıkış verilerini temsil etmek üzere giriş ve çıkış veri kümeleri tanımlayın, sonra da bu veri kümelerini kullanan bir etkinlikle işlem hattını oluşturun. 

## Bağlı hizmetler oluşturma 
Bu adımda, Azure Depolama hesabınızı ve isteğe bağlı Azure HDInsight kümesini data factory’nize bağlarsınız. Azure Depolama hesabı, bu örnekteki işlem hattı için girdi ve çıktı verilerini tutar. HDInsight bağlı hizmeti, bu örnekte işlem hattının etkinliğinde belirtilen Hive betiğini çalıştırmak için kullanılır. Senaryonuzda hangi veri deposu/işlem hizmetlerinin kullanılacağını belirleyin ve bağlı hizmetler oluşturarak bu hizmetleri data factory’ye bağlayın.

### Azure Storage bağlı hizmeti oluşturma
Bu adımda, Azure Depolama hesabınızı veri fabrikanıza bağlarsınız. Giriş/çıkış verilerini ve HQL betik dosyasını depolamak için aynı Azure Depolama hesabını kullanırsınız.

1. C:\ADFGetStarted klasöründe aşağıdaki içeriğe sahip StorageLinkedService.json adlı bir JSON dosyası oluşturun: Henüz yoksa ADFGetStarted klasörünü oluşturun.

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

    **accountname** sözcüğünü Azure depolama hesabınızın adıyla, **accountkey** sözcüğünü de Azure depolama hesabının erişim anahtarıyla değiştirin. Depolama erişim anahtarınızı nasıl alacağınız hakkında bilgi için bkz. [Depolama erişim anahtarlarını görüntüleme, kopyalama ve yeniden oluşturma](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#view-copy-and-regenerate-storage-access-keys).

2. Azure PowerShell’de ADFGetStarted klasörüne geçin.
3. Bağlı bir hizmet oluşturan **New-AzureRmDataFactoryLinkedService** cmdlet’ini kullanabilirsiniz. Bu öğreticide kullandığınız bu cmdlet ve diğer Data Factory cmdlet’lerini *ResourceGroupName* ve *DataFactoryName* parametreleri için değerleri geçirmeniz gerekir. Alternatif olarak, **DataFactory** nesnesini almak ve cmdlet’i her çalıştırdığınızda *ResourceGroupName* ve *DataFactoryName* yazmadan nesneyi geçirmek için **Get-AzureRmDataFactory** kullanabilirsiniz. **Get-AzureRmDataFactory** cmdlet’inin çıktısını **$df** değişkenine atamak için aşağıdaki komutu çalıştırın.

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH

4. Şimdi de, **StorageLinkedService** bağlı hizmetini oluşturan **New-AzureRmDataFactoryLinkedService** cmdlet’ini çalıştırın.

        New-AzureRmDataFactoryLinkedService $df -File .\StorageLinkedService.json

    **Get-AzureRmDataFactory** cmdlet’ini çalıştırmadıysanız ve çıktıyı **$df** değişkenine atamadıysanız, *ResourceGroupName* ve *DataFactoryName* parametreleri için değerleri aşağıdaki gibi belirtmeniz gerekir.

        New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName FirstDataFactoryPSH -File .\StorageLinkedService.json

    Öğreticinin ortasında Azure PowerShell’i kapatırsanız, öğreticiyi tamamlamak için Azure PowerShell’i sonraki başlatışınızda **Get-AzureRmDataFactory** cmdlet’ini çalıştırmanız gerekir.

### Azure HDInsight bağlı hizmeti oluşturma
Bu adımda, isteğe bağlı HDInsight kümesini data factory’nize bağlarsınız. HDInsight kümesi çalışma zamanında otomatik olarak oluşturulur ve işlenmesi bittiğinde ve belirtilen sürede boşta kalırsa silinir. İsteğe bağlı HDInsight kümesi yerine kendi HDInsight kümenizi kullanabilirsiniz. Ayrıntılar için bkz. [İşlem Bağlı Hizmetleri](data-factory-compute-linked-services.md).  

1. **C:\ADFGetStarted** klasöründe aşağıdaki içeriğe sahip **HDInsightOnDemandLinkedService**.json adlı bir JSON dosyası oluşturun:

        {
          "name": "HDInsightOnDemandLinkedService",
          "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
              "version": "3.2",
              "clusterSize": 1,
              "timeToLive": "00:30:00",
              "linkedServiceName": "StorageLinkedService"
            }
          }
        }

    Aşağıdaki tabloda, kod parçacığında kullanılan JSON özellikleri için açıklamalar verilmektedir:

  	| Özellik | Açıklama |
  	| :------- | :---------- |
  	| Sürüm | Oluşturulan HDInsight sürümünün 3.2 olması gerektiğini belirtir. | 
  	| ClusterSize | HDInsight kümesi boyutunu belirtir. | 
  	| TimeToLive | Silinmeden önce HDInsight kümesinin boşta kalma süresini belirtir. |
  	| linkedServiceName | HDInsight tarafından oluşturulan günlükleri depolamak için kullanılan depolama hesabını belirtir. |

    Şunlara dikkat edin: 
    
    - Data Factory, sizin için JSON ile **Windows tabanlı** bir HDInsight kümesi oluşturur. **Linux tabanlı** bir HDInsight kümesi de oluşturabilirsiniz. Ayrıntılar için bkz. [İsteğe Bağlı HDInsight Bağlı Hizmeti](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). 
    - İsteğe bağlı HDInsight kümesi yerine **kendi HDInsight kümenizi** kullanabilirsiniz. Ayrıntılar için bkz. [HDInsight Bağlı Hizmeti](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).
    - HDInsight kümesi JSON’da belirttiğiniz blob depolamada (**linkedServiceName**) bir **varsayılan kapsayıcı** oluşturur. HDInsight, küme silindiğinde bu kapsayıcıyı silmez. Bu davranış tasarım gereğidir. İsteğe bağlı HDInsight bağlı hizmeti kullanıldığında, mevcut canlı bir küme olmadığı sürece bir dilim her işlendiğinde bir HDInsight kümesi oluşturulur (**timeToLive**). Küme, işlem tamamlandığında otomatik olarak silinir.
    
        Daha fazla dilim işlendikçe, Azure blob depolamanızda çok sayıda kapsayıcı görürsünüz. İşlerin sorunları giderilmesi için bunlara gerek yoksa, depolama maliyetini azaltmak için bunları silmek isteyebilirsiniz. Bu kapsayıcıların adları şu deseni izler: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp". Azure blob depolamada kapsayıcı silmek için [Microsoft Storage Gezgini](http://storageexplorer.com/) gibi araçları kullanın.

    Ayrıntılar için bkz. [İsteğe Bağlı HDInsight Bağlı Hizmeti](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). 
2. HDInsightOnDemandLinkedService adlı bağlı hizmeti oluşturan **New-AzureRmDataFactoryLinkedService** cmdlet’ini çalıştırın.

        New-AzureRmDataFactoryLinkedService $df -File .\HDInsightOnDemandLinkedService.json


## Veri kümeleri oluşturma
Bu adımda, Hive işlenmesi için girdi ve çıktı verilerini temsil edecek veri kümeleri oluşturursunuz. Bu veri kümeleri, bu öğreticide daha önce oluşturduğunuz **StorageLinkedService** öğesine başvurur. Bağlı hizmet Azure Storage hesabını belirtirken, veri kümeleri de girdi ve çıktı verilerini tutan depolama biriminde kapsayıcı, klasör, dosya adı belirtir.   

### Girdi veri kümesi oluşturma
1. **C:\ADFGetStarted** klasöründe aşağıdaki içeriğe sahip **InputTable.json** adlı bir JSON dosyası oluşturun:

        {
            "name": "AzureBlobInput",
            "properties": {
                "type": "AzureBlob",
                "linkedServiceName": "StorageLinkedService",
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

    Bu JSON, işlem hattındaki bir etkinliğin girdi verilerini temsil eden **AzureBlobInput** adlı veri kümesini tanımlamaktadır. Ek olarak, girdi verilerinin **adfgetstarted** adlı blob kapsayıcısında ve **inputdata** adlı klasörde bulunduğunu belirtir.

    Aşağıdaki tabloda, kod parçacığında kullanılan JSON özellikleri için açıklamalar verilmektedir:

  	| Özellik | Açıklama |
  	| :------- | :---------- |
  	| type | Veriler Azure blob depolamada yer aldığından type özelliği AzureBlob olarak ayarlanmıştır. |  
  	| linkedServiceName | daha önce oluşturduğunuz StorageLinkedService’e başvurur. |
  	| fileName | Bu özellik isteğe bağlıdır. Bu özelliği atarsanız, tüm folderPath dosyaları alınır. Bu durumda, yalnızca input.log işlenir. |
  	| type | Günlük dosyaları metin biçiminde olduğundan TextFormat kullanacağız. | 
  	| columnDelimiter | Günlük dosyalarındaki sütunlar virgül (,) ile ayrılmıştır. |
  	| frequency/interval | frequency Ay, interval de 1 olarak ayarlanmıştır; girdi dilimlerinin aylık olarak kullanılabileceğini belirtir. | 
  	| external | bu özellik, girdi verileri Data Factory hizmetiyle oluşturulmadıysa true olarak ayarlanır. | 

2. Data Factory veri kümesi oluşturmak için Azure PowerShell’de şu komutu çalıştırın.

        New-AzureRmDataFactoryDataset $df -File .\InputTable.json

### Çıktı veri kümesi oluşturma
Şimdi, Azure Blob depolamada depolanan çıktı verilerini göstermek için çıktı veri kümesi oluşturursunuz.

1. **C:\ADFGetStarted** klasöründe aşağıdaki içeriğe sahip **OutputTable.json** adlı bir JSON dosyası oluşturun:

        {
          "name": "AzureBlobOutput",
          "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
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

    Bu JSON, işlem hattındaki bir etkinliğin çıktı verilerini temsil eden **AzureBlobOutput** adlı veri kümesini tanımlamaktadır. Ek olarak, sonuçların **adfgetstarted** adlı blob kapsayıcısında ve **partitioneddata** adlı klasörde depolandığını belirtir. Burada, **availability** bölümü çıktı veri kümesinin aylık tabanda oluşturulduğunu belirtiyor.

2. Data Factory veri kümesi oluşturmak için Azure PowerShell’de şu komutu çalıştırın.

        New-AzureRmDataFactoryDataset $df -File .\OutputTable.json

## İşlem hattı oluşturma
Bu adımda, **HDInsightHive** etkinliğiyle ilk işlem hattınızı oluşturursunuz. Girdi diliminin ayda bir (frequency: Month, interval: 1) kullanılabilir, çıktı dilimi ayda bir oluşturulur ve etkinlik zamanlayıcı özelliği de ayda bir olacak şekilde ayarlanır. Çıktı veri kümesi ve etkinlik zamanlayıcı ayarlarının eşleşmesi gerekir. Şu anda, çıktı veri kümesi zamanlamayı yönetendir; bu nedenle etkinlik hiçbir çıktı oluşturmasa bile sizin bir çıktı veri kümesi oluşturmanız gerekir. Etkinlik herhangi bir girdi almazsa, girdi veri kümesi oluşturma işlemini atlayabilirsiniz. Aşağıdaki JSON’da kullanılan özellikler bu bölümün sonunda anlatılmaktadır. 


1. C:\ADFGetStarted klasöründe aşağıdaki içeriğe sahip MyFirstPipelinePSH.json adlı bir JSON dosyası oluşturun:

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
                            "scriptLinkedService": "StorageLinkedService",
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
    
    **partitionweblogs.hql** Hive betik dosyası Azure depolama hesabında (scriptLinkedService tarafından belirtilen **StorageLinkedService** adıyla) ve **adfgetstarted** kapsayıcısındaki **betik** klasöründe depolanır.

    Burada, **defines** bölümü hive betiğine Hive yapılandırma değerleri olarak (örn., ${hiveconf:inputtable}, ${hiveconf:partitionedtable}) geçirilecek çalışma zamanı ayarlarını belirtmek için kullanılır.

    İşlem hattının **start** ve **end** özellikleri işlem hattının etkin dönemini belirtir.

    JSON etkinliğinde, Hive betiğinin **linkedServiceName** – **HDInsightOnDemandLinkedService** tarafından belirtilen işlemde çalışacağını belirtirsiniz.

    > [ACOM.NOTE] Örnekte kullanılan JSON özellikleri hakkında ayrıntılı bilgi için bkz. [İşlem Hattı Anatomisi](data-factory-create-pipelines.md#anatomy-of-a-pipeline). 
2.  Azure blob depolamada **adfgetstarted/inputdata** klasöründeki **input.log** dosyasını gördüğünüzü doğrulayın ve işlem hattına dağıtmak için aşağıdaki komutu çalıştırın. **start** ve **end** zamanları geçmişe ayarlanmış ve **isPaused** yanlış olarak ayarlanmış olduğundan işlem hattı (işlem hattında etkinlik) dağıtıldıktan hemen sonra çalışır. 

        New-AzureRmDataFactoryPipeline $df -File .\MyFirstPipelinePSH.json
5. Tebrikler, Azure PowerShell kullanarak ilk işlem hattınızı başarıyla oluşturdunuz.

## İşlem hattını izleme
Bu adımda, Azure data factory’de neler olduğunu izlemek için Azure PowerShell kullanırsınız.

1. **Get-AzureRmDataFactory** komutunu çalıştırın ve çıktıyı **$df** değişkenine atayın.

        $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH

2. İşlem hattının çıktı tablosu olan **EmpSQLTable** tablosunun tüm dilimleri hakkında bilgi almak için **Get-AzureRmDataFactorySlice** komutunu çalıştırın.  

        Get-AzureRmDataFactorySlice $df -DatasetName AzureBlobOutput -StartDateTime 2014-02-01

    Burada belirttiğiniz StartDateTime ile JSON işlem hattında belirtilen başlangıç zamanıyla aynı olmasına özen gösterin. Aşağıdakine benzer bir çıktı görmeniz gerekir.

        ResourceGroupName : ADFTutorialResourceGroup
        DataFactoryName   : FirstDataFactoryPSH
        DatasetName       : AzureBlobOutput
        Start             : 2/1/2014 12:00:00 AM
        End               : 3/1/2014 12:00:00 AM
        RetryCount        : 0
        State             : InProgress
        SubState          :
        LatencyStatus     :
        LongRetryCount    : 0


3. Belirli bir dilimle ilgili etkinlik çalıştırmalarının ayrıntılarını almak için **Get-AzureRmDataFactoryRun** komutunu çalıştırın.

        Get-AzureRmDataFactoryRun $df -DatasetName AzureBlobOutput -StartDateTime 2014-02-01

    Aşağıdakine benzer bir çıktı görmeniz gerekir.
        
        Id                  : 0f6334f2-d56c-4d48-b427-d4f0fb4ef883_635268096000000000_635292288000000000_AzureBlobOutput
        ResourceGroupName   : ADFTutorialResourceGroup
        DataFactoryName     : FirstDataFactoryPSH
        DatasetName         : AzureBlobOutput
        ProcessingStartTime : 12/18/2015 4:50:33 AM
        ProcessingEndTime   : 12/31/9999 11:59:59 PM
        PercentComplete     : 0
        DataSliceStart      : 2/1/2014 12:00:00 AM
        DataSliceEnd        : 3/1/2014 12:00:00 AM
        Status              : AllocatingResources
        Timestamp           : 12/18/2015 4:50:33 AM
        RetryAttempt        : 0
        Properties          : {}
        ErrorMessage        :
        ActivityName        : RunSampleHiveActivity
        PipelineName        : MyFirstPipeline
        Type                : Script

    Dilimi **Hazır** durumunda veya **Başarısız** durumunda görene kadar bu cmdlet’i çalışır halde tutun. Dilim Hazır durumunda olduğunda, çıktı verileri için blob depolamanızın **adfgetstarted** klasöründe **partitioneddata** klasörünü denetleyin.  İsteğe bağlı HDInsight kümesinin oluşturulması genellikle biraz zaman alır.

    ![çıktı verileri](./media/data-factory-build-your-first-pipeline-using-powershell/three-ouptut-files.png)


> [AZURE.IMPORTANT] Dilim başarıyla işlendiğinde girdi dosyası silinir. Bu nedenle, dilimi yeniden çalıştırmak veya öğreticiyi yeniden uygulamak isterseniz girdi dosyasını (input.log) adfgetstarted kapsayıcısının inputdata klasörüne yükleyin.

## Özet 
Bu öğreticide, HDInsight hadoop kümesindeki Hive betiği çalıştırılarak verileri işlemek için bir Azure data factory oluşturdunuz. Aşağıdaki adımları uygulamak için Azure Portal’da Data Factory Düzenleyici’yi kullandınız:  

1.  Oluşturulan Azure **data factory**.
2.  Oluşturulan iki **bağlı hizmet**:
    1.  Girdi/çıktı dosyalarını tutan Azure blob depolamanızı data factory’ye bağlamak için **Azure Storage** bağlı hizmeti.
    2.  İsteğe bağlı HDInsight Hadoop kümesini data factory’ye bağlamak için isteğe bağlı **Azure HDInsight** bağlı hizmeti. Azure Data Factory, girdi verilerini işlemek, çıktı verilerini de oluşturmak için tam zamanında HDInsight Hadoop kümesi oluşturur. 
3.  İşlem hattındaki HDInsight Hive etkinliğiyle ilgili girdi ve çıktı verilerini açıklayan oluşturulan iki **veri kümesi**. 
4.  **HDInsight Hive** etkinliğine sahip oluşturulan bir **işlem hattı**. 

## Sonraki adımlar
Bu makalede, isteğe bağlı Azure HDInsight kümesinde bir Hive betiği çalıştıran dönüştürme etkinliğine (HDInsight Etkinliği) sahip işlem hattı oluşturdunuz. Verileri Azure Blob’tan Azure SQL’e kopyalamak için Kopyalama Etkinliği’nin kullanılması hakkında bilgi için bkz. [Öğretici: Verileri Azure Blob’dan Azure SQL’e kopyalama](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## Ayrıca Bkz.
| Konu | Açıklama |
| :---- | :---- |
| [Data Factory Cmdlet Başvurusu](https://msdn.microsoft.com/library/azure/dn820234.aspx) |  Data Factory cmdlet'leri hakkında kapsamlı belgelere bakma |
| [Veri Dönüştürme Etkinlikleri](data-factory-data-transformation-activities.md) | Bu makalede, Azure Data Factory’nin desteklediği veri dönüştürme etkinliklerinin (bu öğreticide kullandığınız HDInsight Hive dönüştürmesi gibi) bir listesi sağlanmaktadır. |
| [Zamanlama ve Yürütme](data-factory-scheduling-and-execution.md) | Bu makalede Azure Data Factory uygulama modelinin zamanlama ve yürütme yönleri açıklanmaktadır. |
| [İşlem hatları](data-factory-create-pipelines.md) | Bu makale, Azure Data Factory’de işlem hatlarının ve etkinliklerini anlamanıza ve senaryonuz ya da işletmeniz için uçtan uca veri odaklı iş akışları oluşturmak amacıyla bunları nasıl kullanacağınızı anlamanıza yardımcı olur. |
| [Veri kümeleri](data-factory-create-datasets.md) | Bu makale, Azure Data Factory’deki veri kümelerini anlamanıza yardımcı olur.
| [Azure portalı dikey penceresi kullanılarak İşlem Hatlarını İzleme ve Yönetme](data-factory-monitor-manage-pipelines.md) | Bu makalede Azure Portal dikey penceresi kullanılarak işlem hatlarınızı izleme, yönetme ve hatalarını ayıklama işlemleri açıklanmaktadır. |
| [İzleme Uygulaması kullanılarak işlem hatlarını izleme ve yönetme](data-factory-monitor-manage-app.md) | Bu makalede İzleme ve Yönetim Uygulaması kullanılarak işlem hatlarını izleme, yönetme ve hatalarını ayıklama işlemleri açıklanmaktadır. 




<!--HONumber=sep16_HO2-->


