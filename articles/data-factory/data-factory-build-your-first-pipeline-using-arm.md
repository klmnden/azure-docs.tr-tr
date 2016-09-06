<properties
    pageTitle="İlk veri fabrikanızı derleme (Resource Manager şablonu) | Microsoft Azure"
    description="Bu öğreticide Azure Resource Manager şablonu kullanarak örnek bir Azure Data Factory işlem hattı oluşturacaksınız."
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
    ms.date="08/01/2016"
    ms.author="spelluru"/>

# Öğretici: Azure Resource Manager şablonu kullanarak ilk Azure veri fabrikanızı derleme
> [AZURE.SELECTOR]
- [Öğreticiye Genel Bakış](data-factory-build-your-first-pipeline.md)
- [Data Factory Düzenleyici’yi kullanma](data-factory-build-your-first-pipeline-using-editor.md)
- [PowerShell’i kullanma](data-factory-build-your-first-pipeline-using-powershell.md)
- [Visual Studio’yu kullanma](data-factory-build-your-first-pipeline-using-vs.md)
- [Resource Manager Şablonunu kullanma](data-factory-build-your-first-pipeline-using-arm.md)
- [REST API kullanma](data-factory-build-your-first-pipeline-using-rest-api.md)


Bu makalede Azure Resource Manager şablonu kullanarak ilk Azure veri fabrikanızı oluşturmayı öğrenirsiniz. 


## Ön koşullar
Öğreticiye Genel Bakış konusunda listelenen önkoşullar dışında aşağıdakileri yüklemeniz gerekir:

- Başka bir işlem yapmadan önce [Öğreticiye Genel Bakış](data-factory-build-your-first-pipeline.md) makalesinin tamamını okuyun ve önkoşul adımlarını tamamlayın. 
- **Azure PowerShell'i yükleme**. Bilgisayarınıza Azure PowerShell’in en son sürümünü yüklemek için [Azure PowerShell’i yükleme ve yapılandırma](../powershell-install-configure.md) makalesindeki yönergeleri izleyin.
- Bu makale, Azure Data Factory hizmetine kavramsal bir genel bakış sağlamaz. Hizmet hakkında ayrıntılı bir genel bakış için [Azure Data Factory'ye giriş](data-factory-introduction.md) makalesini okuyun. 
- Azure Resource Manager şablonları hakkında bilgi için bkz. [Azure Resource Manager Şablonları Yazma](../resource-group-authoring-templates.md). 

> [AZURE.IMPORTANT]
> Bu makaledeki kılavuzu uygulamak için [Öğreticiye Genel Bakış](data-factory-build-your-first-pipeline.md) bölümündeki önkoşul adımlarını tamamlayın. 

## Resource Manager şablonu oluşturma

**C:\ADFGetStarted** klasöründe aşağıdaki içeriğe sahip **ADFTutorialARM.json** adlı bir JSON dosyası oluşturun: 

Şablon aşağıdaki Data Factory varlıklarını oluşturmanıza olanak sağlar.

1. **TutorialDataFactoryARM** adlı bir **veri fabrikası**. Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. İşlem hattında bir veya daha fazla etkinlik olabilir. Örneğin, verileri bir kaynaktan bir hedef veri deposuna kopyalamak için Kopyalama Etkinliği, girdi verilerini ürün çıktı verilerine dönüştürecek Hive betiğini çalıştırmak için de HDInsight Hive etkinliği. 
2. İki **bağlı hizmet**: **StorageLinkedService** ve **HDInsightOnDemandLinkedService**. Bu bağlı hizmetler Azure Storage hesabınızı ve isteğe bağlı Azure HDInsight kümesini veri fabrikanıza bağlar. Azure Depolama hesabı, bu örnekteki işlem hattı için girdi ve çıktı verilerini tutar. HDInsight bağlı hizmeti, bu örnekte işlem hattının etkinliğinde belirtilen Hive betiğini çalıştırmak için kullanılır. Senaryonuzda hangi veri deposu/işlem hizmetlerinin kullanılacağını belirtmek ve bağlı hizmetler oluşturarak bu hizmetleri veri fabrikanıza bağlamak için gereklidir. 
3. İki (girdi/çıktı) **veri kümesi**: **AzureBlobInput** ve **AzureBlobOutput**. Bu veri kümeleri Hive işlemesi için girdi ve çıktı verilerini temsil eder. Bu veri kümeleri, bu öğreticide daha önce oluşturduğunuz **StorageLinkedService** öğesine başvurur. Bağlı hizmet Azure Storage hesabını belirtirken, veri kümeleri de girdi ve çıktı verilerini tutan depolama biriminde kapsayıcı, klasör, dosya adı belirtir.   

Bu şablonda kullanılan JSON özelliklerine ilişkin ayrıntıları içeren makaleye geçmek için **Data Factory Düzenleyici’yi kullanma** sekmesine tıklayın.

> [AZURE.IMPORTANT] **storageAccountName** ve **storageAccountKey** değişkenlerinin değerlerini değiştirin. Adın benzersiz olması gerektiğinden **dataFactoryName** değerini de değiştirin.


    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
        },
        "variables": {
            "dataFactoryName":  "TutorialDataFactoryARM",
            "storageAccountName":  "<AZURE STORAGE ACCOUNT NAME>" ,
            "storageAccountKey":  "<AZURE STORAGE ACCOUNT KEY>",
            "apiVersion": "2015-10-01",
            "storageLinkedServiceName": "AzureStorageLinkedService",
            "hdInsightOnDemandLinkedServiceName": "HDInsightOnDemandLinkedService",
            "blobInputDataset": "AzureBlobInput",
            "blobOutputDataset": "AzureBlobOutput",
            "singleQuote": "'"
        },
        "resources": [
            {
                "name": "[variables('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "westus",
                "resources": [
                    {
                        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                        "type": "linkedservices",
                        "name": "[variables('storageLinkedServiceName')]",
                        "apiVersion": "[variables('apiVersion')]",
                        "properties": {
                            "type": "AzureStorage",
                            "typeProperties": {
                                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',variables('storageAccountName'),';AccountKey=',variables('storageAccountKey'))]"
                            }
                        }
                    },
                    {
                        "dependsOn": [
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]",
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"
                        ],
                        "type": "linkedservices",
                        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
                        "apiVersion": "[variables('apiVersion')]",
                        "properties": {
                            "type": "HDInsightOnDemand",
                            "typeProperties": {
                                "clusterSize": 4,
                                "version":  "3.2",
                                "timeToLive": "00:05:00",
                                "osType": "windows",
                                "linkedServiceName": "[variables('storageLinkedServiceName')]",
                            }
                        }
                    },
                    {
                        "dependsOn": [
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]",
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
                        ],
                        "type": "datasets",
                        "name": "[variables('blobInputDataset')]",
                        "apiVersion": "[variables('apiVersion')]",
                            "properties": {
                                "type": "AzureBlob",
                                "linkedServiceName": "[variables('storageLinkedServiceName')]",
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
                        },
                    {
                        "dependsOn": [
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]",
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
                        ],
                        "type": "datasets",
                        "name": "[variables('blobOutputDataset')]",
                        "apiVersion": "[variables('apiVersion')]",
                            "properties": {
                                "published": false,
                                "type": "AzureBlob",
                                "linkedServiceName": "[variables('storageLinkedServiceName')]",
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
                        },
                        {
                            "dependsOn": [
                                "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]",
                                "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]",
                                "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/linkedServices/', variables('hdInsightOnDemandLinkedServiceName'))]",
                                "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/datasets/', variables('blobInputDataset'))]",
                                "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/datasets/', variables('blobOutputDataset'))]"
                            ],
                            "type": "datapipelines",
                            "name": "[variables('dataFactoryName')]",
                            "apiVersion": "[variables('apiVersion')]",
                            "properties": {
                                "description": "My first Azure Data Factory pipeline",
                                "activities": [
                                    {
                                        "type": "HDInsightHive",
                                        "typeProperties": {
                                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                                            "scriptLinkedService": "[variables('storageLinkedServiceName')]",
                                            "defines": {
                                                "inputtable": "[concat('wasb://adfgetstarted@', variables('storageAccountName'), '.blob.core.windows.net/inputdata')]",
                                                "partitionedtable": "[concat('wasb://adfgetstarted@', variables('storageAccountName'), '.blob.core.windows.net/partitioneddata')]"
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
                ]
            }
        ]
    }

Bu şablonda kullanılan JSON özelliklerine ilişkin ayrıntıları içeren makaleye geçmek için **Data Factory Düzenleyici’yi kullanma** sekmesine tıklayın.

Şunlara dikkat edin: 

- Data Factory **Windows tabanlı** bir HDInsight kümesini yukarıdaki JSON ile sizin için oluşturur. **Linux tabanlı** bir HDInsight kümesi de oluşturabilirsiniz. Ayrıntılar için bkz. [İsteğe Bağlı HDInsight Bağlı Hizmeti](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service). 
- İsteğe bağlı HDInsight kümesi yerine **kendi HDInsight kümenizi** kullanabilirsiniz. Ayrıntılar için bkz. [HDInsight Bağlı Hizmeti](data-factory-compute-linked-services.md#azure-hdinsight-linked-service).
- HDInsight kümesi JSON’da belirttiğiniz blob depolamada (**linkedServiceName**) bir **varsayılan kapsayıcı** oluşturur. HDInsight, küme silindiğinde bu kapsayıcıyı silmez. Bu davranış tasarım gereğidir. İsteğe bağlı HDInsight bağlı hizmetiyle, HDInsight kümesi her oluşturulduğunda, burada mevcut canlı bir küme (**timeToLive**) olmadıkça bir dilim gerekir ve işlem bittiğinde silinir.

    Daha fazla dilim işlendikçe, Azure blob depolamanızda çok sayıda kapsayıcı görürsünüz. İşlerin sorunları giderilmesi için bunlara gerek yoksa, depolama maliyetini azaltmak için bunları silmek isteyebilirsiniz. Bu kapsayıcıların adları şu deseni izler: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp". Azure blob depolamada kapsayıcı silmek için [Microsoft Storage Gezgini](http://storageexplorer.com/) gibi araçları kullanın.

Ayrıntılar için bkz. [İsteğe Bağlı HDInsight Bağlı Hizmeti](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service).

> [AZURE.NOTE] Azure veri fabrikası oluşturmaya yönelik Resource Manager şablonunun başka bir örneğini [Github](https://github.com/Azure/azure-quickstart-templates/blob/master/101-data-factory-blob-to-sql/azuredeploy.json) üzerinde bulabilirsiniz.  

## Veri fabrikası oluşturma

1. **Azure PowerShell**’i başlatın ve aşağıdaki komutu çalıştırın. 
    - **Login-AzureRmAccount** komutunu çalıştırın ve Azure Portal’da oturum açmak için kullandığınız kullanıcı adı ve parolayı girin.  
    - İçinde veri fabrikasını oluşturmak istediğiniz aboneliği seçmek için aşağıdaki komutu çalıştırın.
            Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
1. 1 Adımda oluşturduğunuz Resource Manager şablonunu kullanarak Data Factory varlıklarını dağıtmak için aşağıdaki komutu çalıştırın. 

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json

## İşlem hattını izleme
 
1.  [Azure Portal](https://portal.azure.com/)’da oturum açtıktan sonra **Gözat**’a tıklayın ve **Veri fabrikaları**’nı seçin.
        ![Gözat->Veri fabrikaları](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)
2.  **Veri Fabrikaları** dikey penceresinde oluşturduğunuz veri fabrikasına (**TutorialFactoryARM**) tıklayın.   
2.  Veri fabrikanızın **Veri Fabrikası** dikey penceresinde **Diyagram**’a tıklayın.
        ![Diyagram Kutucuğu](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4.  **Diyagram Görünümü**nde, işlem hatlarına ve bu öğreticide kullanılan veri kümelerine bir genel bakış görürsünüz.
    
    ![Diyagram Görünümü](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
8. Diyagram Görünümü’nde **AzureBlobOutput** veri kümesine çift tıklayın. Dilimin işlenmekte olduğunu görürsünüz.

    ![Veri kümesi](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
9. İşlem tamamlandığında dilimi **Hazır** durumunda göreceksiniz. İsteğe bağlı HDInsight kümesinin oluşturulması genellikle biraz zaman alır (yaklaşık 20 dakika). 

    ![Veri kümesi](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png) 
10. Dilim **Hazır** durumunda olduğunda çıktı verileri için blob depolama alanınızın **adfgetstarted** kapsayıcısında **partitioneddata** klasörünü denetleyin.  

Bu öğreticide oluşturduğunuz işlem hattını ve veri kümelerini izlemek üzere Azure portalı dikey pencerelerinin kullanımına ilişkin yönergeler için bkz. [Veri kümelerini ve işlem hatlarını izleme](data-factory-monitor-manage-pipelines.md).

Veri işlem hatlarınızı izlemek için İzleme ve Yönetme Uygulaması’nı da kullanabilirsiniz. Uygulamanın kullanımına ilişkin ayrıntılar için bkz. [İzleme Uygulaması kullanarak Azure Data Factory işlem hatlarını izleme ve yönetme](data-factory-monitor-manage-app.md). 

> [AZURE.IMPORTANT] Dilim başarıyla işlendiğinde girdi dosyası silinir. Bu nedenle, dilimi yeniden çalıştırmak veya öğreticiyi yeniden uygulamak isterseniz girdi dosyasını (input.log) adfgetstarted kapsayıcısının inputdata klasörüne yükleyin.

## Bir ağ geçidi oluşturmak için Resource Manager şablonu
Arka planda mantıksal bir ağ geçidi oluşturmaya yönelik örnek bir Resource Manager şablonu aşağıda verilmiştir. Şirket içi bilgisayarınıza ya da Azure IaaS sanal makinesine bir ağ geçidi yüklemeniz ve ağ geçidini bir anahtar kullanarak Data Factory hizmetine kaydetmeniz gerekir. Ayrıntılar için bkz. [Şirket içi ile bulut arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md).

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
        },
        "variables": {
            "dataFactoryName":  "GatewayUsingArmDF",
            "apiVersion": "2015-10-01",
            "singleQuote": "'"
        },
        "resources": [
            {
                "name": "[variables('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "eastus",
                "resources": [
                    {
                        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                        "type": "gateways",
                        "apiVersion": "[variables('apiVersion')]",
                        "name": "GatewayUsingARM",
                        "properties": {
                            "description": "my gateway"
                        }
                    }            
                ]
            }
        ]
    }

Bu şablon GatewayUsingArmDF adlı bir veri fabrikasını GatewayUsingARM adlı bir ağ geçidi ile oluşturur. 

## Ayrıca Bkz.
| Konu | Açıklama |
| :---- | :---- |
| [Veri Dönüştürme Etkinlikleri](data-factory-data-transformation-activities.md) | Bu makalede, Azure Data Factory’nin desteklediği veri dönüştürme etkinliklerinin (bu öğreticide kullandığınız HDInsight Hive dönüştürmesi gibi) bir listesi sağlanmaktadır. |
| [Zamanlama ve yürütme](data-factory-scheduling-and-execution.md) | Bu makalede Azure Data Factory uygulama modelinin zamanlama ve yürütme yönleri açıklanmaktadır. |
| [İşlem hatları](data-factory-create-pipelines.md) | Bu makale, Azure Data Factory’de işlem hatlarının ve etkinliklerini anlamanıza ve senaryonuz ya da işletmeniz için uçtan uca veri odaklı iş akışları oluşturmak amacıyla bunları nasıl kullanacağınızı anlamanıza yardımcı olur. |
| [Veri kümeleri](data-factory-create-datasets.md) | Bu makale, Azure Data Factory’deki veri kümelerini anlamanıza yardımcı olur.
| [İzleme Uygulaması kullanılarak işlem hatlarını izleme ve yönetme](data-factory-monitor-manage-app.md) | Bu makalede İzleme ve Yönetim Uygulaması kullanılarak işlem hatlarını izleme, yönetme ve hatalarını ayıklama işlemleri açıklanmaktadır. 

  




<!--HONumber=Aug16_HO4-->


