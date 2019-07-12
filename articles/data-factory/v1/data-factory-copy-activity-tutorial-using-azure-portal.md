---
title: 'Öğretici: Verileri (Azure portalı) kopyalamak için bir Azure Data Factory işlem hattı oluşturma | Microsoft Docs'
description: Bu öğreticide, Azure blob depolama alanından Azure SQL veritabanına veri kopyalamak için Azure portalını kullanarak Kopyalama Etkinliği içeren Azure Data Factory işlem hattı oluşturursunuz.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: ''
editor: ''
ms.assetid: d9317652-0170-4fd3-b9b2-37711272162b
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 82e8fe26cc58117bc6249f8a7e87612dabc5f438
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839504"
---
# <a name="tutorial-use-azure-portal-to-create-a-data-factory-pipeline-to-copy-data"></a>Öğretici: Verileri kopyalamak için bir Data Factory işlem hattı oluşturmak için Azure portalını kullanma 
> [!div class="op_single_selector"]
> * [Genel bakış ve önkoşullar](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Kopyalama Sihirbazı](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager şablonu](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API’si](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz. [kopyalama etkinliği öğreticisi](../quickstart-create-data-factory-dot-net.md). 

> [!WARNING]
> JSON düzenleyicisini yazma ve işlem hatlarını olacaktır ADF v1 dağıtmak için Azure Portal'da, 31 Temmuz 2019 üzerinde kapatıldı. 31 Temmuz 2019 sonra kullanmaya devam edebilirsiniz [ADF v1 Powershell cmdlet'leri](https://docs.microsoft.com/powershell/module/az.datafactory/?view=azps-2.4.0&viewFallbackFrom=azps-2.3.2), [ADF v1 .net SDK'sı](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.datafactories.models?view=azure-dotnet), [ADF v1 REST API'leri](https://docs.microsoft.com/rest/api/datafactory/) author &, ADF v1 dağıtmak için işlem hatları.

Bu makalede, Azure blob depolama alanından Azure SQL veritabanına veri kopyalayan bir işlem hattıyla veri fabrikası oluşturmak için [Azure portalını](https://portal.azure.com) nasıl kullanacağınızı öğreneceksiniz. Azure Data Factory’yi ilk kez kullanıyorsanız bu öğreticiyi tamamlamadan önce [Azure Data Factory’ye Giriş](data-factory-introduction.md) makalesini okuyun.   

Bu öğreticide, içinde bir etkinlik ile işlem hattı oluşturun: Kopyalama etkinliği. Kopyalama etkinliği, verileri, desteklenen bir veri deposundan desteklenen bir havuz veri deposuna kopyalar. Kaynak ve havuz olarak desteklenen veri depolarının listesi için bkz. [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Etkinlik, çeşitli veri depolama alanları arasında güvenli, güvenilir ve ölçeklenebilir bir yolla veri kopyalayabilen genel olarak kullanılabilir bir hizmet tarafından desteklenir. Kopyalama Etkinliği hakkında daha fazla bilgi için bkz. [Veri Taşıma Etkinlikleri](data-factory-data-movement-activities.md).

Bir işlem hattında birden fazla etkinlik olabilir. Bir etkinliğin çıkış veri kümesini diğer etkinliğin giriş veri kümesi olarak ayarlayarak iki etkinliği zincirleyebilir, yani bir etkinliğin diğerinden sonra çalıştırılmasını sağlayabilirsiniz. Daha fazla bilgi için bkz. [bir işlem hattında birden fazla etkinlik](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline). 

> [!NOTE] 
> Bu öğreticideki veri işlem hattı, bir kaynak veri deposundaki verileri hedef veri deposuna kopyalar. Azure Data Factory kullanarak verileri dönüştürme hakkında bir öğretici için bkz. [Öğreticisi: Hadoop kümesi kullanarak verileri dönüştürmek için bir işlem hattı oluşturma](data-factory-build-your-first-pipeline.md).

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi uygulamadan önce [öğretici önkoşulları](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) makalesinde listelenen önkoşulları tamamlayın.

## <a name="steps"></a>Adımlar
Bu eğitimin bir parçası olarak gerçekleştireceğiniz adımlar şunlardır:

1. Azure **veri fabrikası** oluşturma. Bu adımda, ADFTutorialDataFactory adlı bir veri fabrikası oluşturursunuz. 
2. Veri fabrikasında **bağlı hizmetler** oluşturun. Bu adımda iki bağlı hizmet türü oluşturun: Azure depolama ve Azure SQL veritabanı. 
    
    AzureStorageLinkedService, Azure depolama hesabınızı veri fabrikasına bağlar. Bir kapsayıcı oluşturup verileri [ön koşulların](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) parçası olarak bu depolama hesabına yüklediniz.   

    AzureSqlLinkedService, Azure SQL veritabanınızı veri fabrikasına bağlar. Blob depolama alanından kopyalanan veriler bu veritabanında depolanır. Bu veritabanındaki SQL tablosunu, [ön koşulların](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) parçası olarak oluşturdunuz.   
3. Veri fabrikasında girdi ve çıktı **veri kümesi oluşturma**.  
    
    Azure Depolama bağlı hizmeti, Data Factory hizmetinin Azure depolama hesabınıza bağlanmak için çalışma zamanında kullandığı bağlantı dizesini belirtir. Giriş blob veri kümesi ise kapsayıcıyı ve girdi verilerini içeren klasörü belirtir.  

    Benzer şekilde, Azure SQL Veritabanı bağlı hizmeti, Data Factory hizmetinin Azure SQL veritabanınıza bağlanmak için çalışma zamanında kullandığı bağlantı dizesini belirtir. Çıktı SQL tablosu veri kümesi ise blob depolama alanındaki verilerin kopyalandığı veritabanında tabloyu belirtir.
4. Veri fabrikasında **işlem hattı** oluşturun. Bu adımda, kopyalama etkinliği ile bir işlem hattı oluşturursunuz.   
    
    Kopyalama etkinliği, verileri Azure blob depolama alanındaki bir blobdan Azure SQL veritabanındaki tabloya kopyalar. Verileri desteklenen herhangi bir kaynaktan desteklenen herhangi bir hedefe kopyalamak için bir işlem hattındaki kopyalama etkinliğini kullanabilirsiniz. Desteklenen veri depolarının bir listesi için [veri taşıma etkinlikleri](data-factory-data-movement-activities.md#supported-data-stores-and-formats) makalesine bakın. 
5. İşlem hattını izleme. Bu adımda, girdi ve çıktı veri kümelerinin dilimlerini Azure portalını kullanarak **izlersiniz**. 

## <a name="create-a-data-factory"></a>Veri fabrikası oluşturma
> [!IMPORTANT]
> Henüz yapmadıysanız, [öğreticinin ön koşullarını](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) tamamlayın.   

Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. İşlem hattında bir veya daha fazla etkinlik olabilir. Örneğin, verileri bir kaynaktan bir hedef veri deposuna kopyalamak için Kopyalama Etkinliği, giriş verilerini ürün çıkış verilerine dönüştürecek Hive betiğini çalıştırmak için de HDInsight Hive etkinliği. Bu adımda data factory oluşturmayla başlayalım.

1. [Azure portalında](https://portal.azure.com/) oturum açtıktan sonra sol taraftaki menüde **Kaynak oluştur**’a, **Veri ve Analiz**’e ve **Data Factory**'ye tıklayın. 
   
   ![Yeni->DataFactory](./media/data-factory-copy-activity-tutorial-using-azure-portal/NewDataFactoryMenu.png)    
2. **Yeni data factory** dikey penceresinde:
   
   1. **Ad** için **ADFTutorialDataFactory** girin. 
      
         ![Yeni veri fabrikası dikey penceresi](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-new-data-factory.png)
      
       Azure data factory adı **küresel olarak benzersiz** olmalıdır. Aşağıdaki hatayı alırsanız veri fabrikasının adını değiştirin (örneğin adınızADFTutorialDataFactory) ve oluşturmayı yeniden deneyin. Data Factory yapıtlarının adlandırma kuralları için [Data Factory - Adlandırma Kuralları](data-factory-naming-rules.md) konusuna bakın.
      
           Data factory name “ADFTutorialDataFactory” is not available  
      
       ![Data Factory adı yok](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-not-available.png)
   2. Veri fabrikasını oluşturmak istediğiniz Azure **aboneliğini** seçin. 
   3. **Kaynak Grubu** için aşağıdaki adımlardan birini uygulayın:
      
      - **Var olanı kullan**’ı seçin ve ardından açılır listeden var olan bir kaynak grubu belirleyin. 
      - **Yeni oluştur**’u seçin ve bir kaynak grubunun adını girin.   
         
          Bu öğreticideki adımlardan bazıları adı kullandığınızı varsayar: **ADFTutorialResourceGroup** kaynak grubu için. Kaynak grupları hakkında daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../../azure-resource-manager/resource-group-overview.md).  
   4. Data factory için **konum** seçin. Açılır listede yalnızca Data Factory hizmeti tarafından desteklenen bölgeler gösterilmektedir.
   5. **Panoya sabitle**’yi seçin.     
   6.           **Oluştur**'a tıklayın.
      
      > [!IMPORTANT]
      > Data Factory örnekleri oluşturmak için abonelik/kaynak grubu düzeyinde [Data Factory Katılımcısı](../../role-based-access-control/built-in-roles.md#data-factory-contributor) rolünün üyesi olmanız gerekir.
      > 
      > Veri fabrikasının adı gelecekte bir DNS adı olarak kaydedilmiş ve herkese görünür hale gelmiş olabilir.                
      > 
      > 
3. Panoda durumuna sahip aşağıdaki kutucuğu görürsünüz: **Veri Fabrikası dağıtılıyor**. 

    ![veri fabrikası dağıtılıyor kutucuğu](media/data-factory-copy-activity-tutorial-using-azure-portal/deploying-data-factory.png)
4. Oluşturma işlemi tamamlandıktan sonra, görüntüde gösterildiği gibi **Data Factory** dikey penceresini görürsünüz.
   
   ![Data factory giriş sayfası](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-data-factory-home-page.png)

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma
Veri depolarınızı ve işlem hizmetlerinizi veri fabrikasına bağlamak için veri fabrikasında bağlı hizmetler oluşturursunuz. Bu öğreticide, Azure HDInsight veya Azure Data Lake Analytics gibi herhangi bir işlem hizmeti kullanmazsınız. Azure Depolama (kaynak) ve Azure SQL Veritabanı (hedef) türünde iki veri deposu kullanırsınız. 

Bu nedenle, AzureStorageLinkedService ve AzureSqlLinkedService türlerinin adlı iki bağlı hizmet oluşturun: AzureStorage ve AzureSqlDatabase.  

AzureStorageLinkedService, Azure depolama hesabınızı veri fabrikasına bağlar. Bu depolama hesabı, kapsayıcı oluşturduğunuz ve verileri [ön koşulların](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) parçası olarak yüklediğiniz hesaptır.   

AzureSqlLinkedService, Azure SQL veritabanınızı veri fabrikasına bağlar. Blob depolama alanından kopyalanan veriler bu veritabanında depolanır. Bu veritabanındaki emp tablosunu, [önkoşulların](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) parçası olarak oluşturdunuz.  

### <a name="create-an-azure-storage-linked-service"></a>Azure Depolama bağlı hizmeti oluşturma
Bu adımda, Azure depolama hesabınızı veri fabrikanıza bağlarsınız. Bu bölümde Azure depolama hesabınızın adını ve anahtarını belirtirsiniz.  

1. **Data Factory** dikey penceresinde **Geliştir ve dağıt** kutucuğuna tıklayın.
   
   ![Geliştir ve Dağıt Kutucuğu](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-author-deploy-tile.png) 
2. Bu resimdeki gibi **Data Factory Düzenleyicisi**'ni göreceksiniz: 

    ![Data Factory Düzenleyicisi](./media/data-factory-copy-activity-tutorial-using-azure-portal/data-factory-editor.png)
3. Düzenleyici'de, araç çubuğundaki **Yeni veri deposu** düğmesine tıklayın ve açılan menüden **Azure depolama**'yı seçin. Sağ bölmede Azure depolama bağlı hizmeti oluşturmak için JSON şablonunu görmeniz gerekir. 
   
    ![Düzenleyici Yeni veri deposu düğmesi](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-newdatastore-button.png)    
3. Burada, `<accountname>` ve `<accountkey>` sözcüklerini Azure depolama hesabınıza ait hesap adı ve hesap anahtarı değerleriyle değiştirin. 
   
    ![Düzenleyici Blob Storage JSON](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-json.png)    
4. Araç çubuğunda **Dağıt**’a tıklayın. Dağıtılan **AzureStorageLinkedService** öğesini şu anda ağaçta görmeniz gerekir. 
   
    ![Düzenleyici Blob Storage Dağıtma](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-editor-blob-storage-deploy.png)

    Bağlı hizmet tanımındaki JSON özellikleri hakkında daha fazla bilgi için [Azure Blob Depolama bağlayıcısı](data-factory-azure-blob-connector.md#linked-service-properties) makalesine bakın.

### <a name="create-a-linked-service-for-azure-sql-database"></a>Azure SQL veritabanı için bağlı hizmet oluşturma
Bu adımda, Azure SQL veritabanınızı veri fabrikanıza bağlarsınız. Bu bölümde Azure SQL sunucu adı, veritabanı adı, kullanıcı adı ve kullanıcı parolasını belirtirsiniz. 

1. **Data Factory Düzenleyici**’de, araç çubuğundaki **Yeni veri deposu** düğmesine tıklayın ve açılan menüden **Azure SQL Veritabanı**’nı seçin. Sağ bölmede Azure SQL bağlı hizmeti oluşturmak için JSON şablonunu görmeniz gerekir.
2. `<servername>`, `<databasename>`, `<username>@<servername>` ve `<password>` öğesini Azure SQL sunucusu, veritabanı, kullanıcı hesabı ve parolası ile değiştirin. 
3. **AzureSqlLinkedService**’i oluşturmak ve dağıtmak için araç çubuğunda **Dağıt**’a tıklayın.
4. Ağaç görünümünde **Bağlı hizmetler** bölümünde **AzureSqlLinkedService** öğesini gördüğünüzü onaylayın.  

    Bu JSON özellikleri hakkında daha fazla bilgi için [Azure SQL Veritabanı bağlayıcısı](data-factory-azure-sql-connector.md#linked-service-properties) makalesine bakın.

## <a name="create-datasets"></a>Veri kümeleri oluşturma
Önceki adımda, Azure Depolama hesabınızı ve Azure SQL veritabanınızı veri fabrikanıza bağlamak için bağlı hizmetler oluşturdunuz. Bu adımda, sırasıyla AzureStorageLinkedService ve AzureSqlLinkedService tarafından başvurulan veri depolarında depolanan girdi ve çıktı verilerini temsil eden InputDataset ve OutputDataset adlı iki veri kümesini tanımlarsınız.

Azure Depolama bağlı hizmeti, Data Factory hizmetinin Azure depolama hesabınıza bağlanmak için çalışma zamanında kullandığı bağlantı dizesini belirtir. Giriş blobu veri kümesi (InputDataset) ise kapsayıcıyı ve girdi verilerini içeren klasörü belirtir.  

Benzer şekilde, Azure SQL Veritabanı bağlı hizmeti, Data Factory hizmetinin Azure SQL veritabanınıza bağlanmak için çalışma zamanında kullandığı bağlantı dizesini belirtir. Çıktı SQL tablosu veri kümesi (OutputDataset) ise blob depolama alanındaki verilerin kopyalandığı veritabanında tabloyu belirtir. 

### <a name="create-an-input-dataset"></a>Girdi veri kümesi oluşturma
Bu adımda, InputDataset adlı bir veri kümesi oluşturursunuz. Bu veri kümesi, AzureStorageLinkedService bağlı hizmetiyle temsil edilen Azure Depolama’daki bir blob kapsayıcısının (adftutorial) kök klasöründe bulunan blob dosyasını (emp.txt) işaret eder. fileName için bir değer belirtmezseniz (veya bu adımı atlarsanız) girdi klasöründe bulunan tüm blob’lardaki veriler hedefe kopyalanır. Bu öğreticide, fileName için bir değer belirtirsiniz. 

1. Data Factory **Düzenleyici**’de açılır listeden **... Daha fazla**, **Yeni veri kümesi** ve **Azure Blob depolama** öğelerine tıklayın. 
   
    ![Yeni veri kümesi menüsü](./media/data-factory-copy-activity-tutorial-using-azure-portal/new-dataset-menu.png)
2. Sağ bölmedeki JSON ifadesini aşağıdaki JSON parçacığıyla değiştirin: 
   
    ```json
    {
      "name": "InputDataset",
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
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
          "folderPath": "adftutorial/",
          "fileName": "emp.txt",
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
    ```   

    Aşağıdaki tabloda, kod parçacığında kullanılan JSON özellikleri için açıklamalar verilmektedir:

    | Özellik | Açıklama |
    |:--- |:--- |
    | türü | Veriler Azure blob depolama alanında yer aldığından type özelliği **AzureBlob** olarak ayarlanmıştır. |
    | linkedServiceName | Daha önce oluşturduğunuz **AzureStorageLinkedService**’e başvurur. |
    | folderPath | blob **kapsayıcıyı** ve girdi blob’larını içeren **klasörü** belirtir. Bu öğreticide adftutorial, blob kapsayıcısıdır ve klasör, kök klasördür. | 
    | fileName | Bu özellik isteğe bağlıdır. Bu özelliği atarsanız tüm folderPath dosyaları alınır. Bu öğreticide fileName için **emp.txt** belirtilir, bu nedenle işlem için yalnızca bu dosya seçilir. |
    | format -> type |Girdi dosyası metin biçiminde olduğundan **TextFormat**'ı kullanırız. |
    | columnDelimiter | Girdi dosyasındaki sütunlar, **virgül (`,`)** ile ayrılmıştır. |
    | frequency/interval | frequency **Saat**, interval da **1** olarak ayarlanmıştır. Bu, girdi dilimlerinin **saatlik** olarak kullanılabileceğini belirtir. Başka bir deyişle, Data Factory hizmeti belirttiğiniz blob kapsayıcısının (**adftutorial**) kök klasöründe girdi verilerini saatte bir kere arar. İşlem hattı başlangıç ve bitiş zamanlarındaki verileri arar, bu zamanlardan önceki veya sonraki verileri aramaz.  |
    | external | Bu özellik, veriler bu işlem hattı tarafından oluşturulmazsa **true** olarak ayarlanır. Bu öğreticideki girdi verileri, bu işlem hattı tarafından oluşturulmayan emp.txt dosyasında bulunur, bu nedenle bu özelliği true olarak ayarlarız. |

    Bu JSON özellikleri hakkında daha fazla bilgi için bkz. [Azure Blob bağlayıcısı makalesi](data-factory-azure-blob-connector.md#dataset-properties).      
3. **InputDataset** veri kümesini oluşturmak ve dağıtmak için araç çubuğunda **Dağıt**’a tıklayın. **InputDataset** öğesini ağaç görünümünde gördüğünüzü onaylayın.

### <a name="create-an-output-dataset"></a>Çıktı veri kümesi oluşturma
Azure SQL Veritabanı bağlı hizmeti, Data Factory hizmetinin Azure SQL veritabanınıza bağlanmak için çalışma zamanında kullandığı bağlantı dizesini belirtir. Bu adımda oluşturduğunuz çıktı SQL tablosu veri kümesi (OututDataset), blob depolama alanındaki verilerin kopyalandığı veritabanında tabloyu belirtir.

1. Data Factory **Düzenleyici**’de açılır listeden **... Daha fazla**, **Yeni veri kümesi** ve **Azure SQL** öğelerine tıklayın. 
2. Sağ bölmedeki JSON ifadesini aşağıdaki JSON parçacığıyla değiştirin:

    ```json   
    {
      "name": "OutputDataset",
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
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
          "tableName": "emp"
        },
        "availability": {
          "frequency": "Hour",
          "interval": 1
        }
      }
    }
    ```     

    Aşağıdaki tabloda, kod parçacığında kullanılan JSON özellikleri için açıklamalar verilmektedir:

    | Özellik | Açıklama |
    |:--- |:--- |
    | türü | type özelliği, veriler Azure SQL veritabanındaki bir tabloya kopyalandığından **AzureSqlTable** olarak ayarlanır. |
    | linkedServiceName | Daha önce oluşturduğunuz **AzureSqlLinkedService**’e başvurur. |
    | tableName | Verilerin kopyalandığı **tabloyu** belirtir. | 
    | frequency/interval | frequency **Saatlik** ve interval **1** olarak ayarlanır. Bu durumda çıktı dilimleri, işlem hattı başlangıç ve bitiş zamanları arasında **saatlik** olarak üretilir, bu zamanlardan önce veya sonra üretilmez.  |

    Veritabanındaki emp tablosunda üç sütun vardır: **ID**, **FirstName** ve **LastName**. ID bir kimlik sütunu olduğundan, burada yalnızca **FirstName** ve **LastName** değerlerini belirtmeniz gerekir.

    Bu JSON özellikleri hakkında daha fazla bilgi için bkz. [Azure SQL bağlayıcısı makalesi](data-factory-azure-sql-connector.md#dataset-properties).
3. **OutputDataset** veri kümesini oluşturmak ve dağıtmak için araç çubuğunda **Dağıt**’a tıklayın. **OutputDataset** öğesini ağaç görünümünde **Veri kümeleri** altında gördüğünüzü onaylayın. 

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma
Bu adımda, girdi olarak **InputDataset** ve çıktı olarak **OutputDataset** kullanan **kopyalama etkinliği**’ne sahip bir işlem hattı oluşturursunuz.

Şu anda zamanlamayı çıktı veri kümesi yürütmektedir. Bu öğreticide, çıktı veri kümesi saatte bir dilim oluşturacak şekilde yapılandırılır. İşlem hattının başlangıç zamanı ve bitiş zamanı arasında bir gün, yani 24 saat vardır. Bu nedenle işlem hattı, çıktı veri kümesinden 24 dilim oluşturur. 

1. Data Factory **Düzenleyici**’de açılır listeden **... Daha fazla** ve **Yeni işlem hattı** öğelerine tıklayın. Alternatif olarak, ağaç görünümünde **İşlem hatları**’na sağ tıklayın ve**Yeni işlem hattı**’na tıklayın.
2. Sağ bölmedeki JSON ifadesini aşağıdaki JSON parçacığıyla değiştirin: 

    ```json   
    {
      "name": "ADFTutorialPipeline",
      "properties": {
        "description": "Copy data from a blob to Azure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "type": "Copy",
            "inputs": [
              {
                "name": "InputDataset"
              }
            ],
            "outputs": [
              {
                "name": "OutputDataset"
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
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
        ],
        "start": "2017-05-11T00:00:00Z",
        "end": "2017-05-12T00:00:00Z"
      }
    } 
    ```   
    
    Aşağıdaki noktalara dikkat edin:
   
   - Etkinlikler bölümünde, **türü** **Copy** olarak ayarlanmış yalnızca bir etkinlik vardır. Kopyalama etkinliği hakkında daha fazla bilgi için bkz. [veri taşıma etkinlikleri](data-factory-data-movement-activities.md). Data Factory çözümlerinde, [veri dönüştürme etkinliklerini](data-factory-data-transformation-activities.md) de kullanabilirsiniz.
   - Etkinlik girdisi **InputDataset** olarak, etkinlik çıktısı ise **OutputDataset** olarak ayarlanmıştır. 
   - **typeProperties** bölümünde **BlobSource** kaynak türü, **SqlSink** de havuz türü olarak belirtilir. Kaynak ve havuz olarak kopyalama etkinliği tarafından desteklenen veri depolarının eksiksiz listesi için bkz. [desteklenen veri depoları](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Kaynak/havuz olarak desteklenen belirli bir veri deposunu nasıl kullanacağınızı öğrenmek için tablodaki bağlantıya tıklayın.
   - Başlangıç ve bitiş tarih saatleri [ISO biçiminde](https://en.wikipedia.org/wiki/ISO_8601) olmalıdır. Örneğin: 2016-10-14T16:32:41Z. **End** zamanı isteğe bağlıdır; ancak bu öğreticide bunu kullanacağız. **end** özelliği için değer belirtmezseniz "**start + 48 hours**" olarak hesaplanır. İşlem hattını süresiz olarak çalıştırmak için **end** özelliği değerini **9999-09-09** olarak ayarlayın.
     
     Önceki örnekte, her veri dilimi saatlik oluşturulduğundan 24 veri dilimi vardır.

     İşlem hattı tanımındaki JSON özelliklerinin açıklamaları için [işlem hatları oluşturma](data-factory-create-pipelines.md) makalesine bakın. Kopyalama etkinliği tanımındaki JSON özelliklerinin açıklamaları için bkz. [veri taşıma etkinlikleri](data-factory-data-movement-activities.md). BlobSource tarafından desteklenen JSON özelliklerinin açıklamaları için bkz. [Azure Blob bağlayıcısı makalesi](data-factory-azure-blob-connector.md). SqlSink tarafından desteklenen JSON özelliklerinin açıklamaları için bkz. [Azure SQL Veritabanı bağlayıcısı makalesi](data-factory-azure-sql-connector.md).
3. **ADFTutorialPipeline** tablosunu oluşturmak ve dağıtmak için araç çubuğunda **Dağıt**’a tıklayın. İşlem hattını ağaç görünümünde gördüğünüzü onaylayın. 
4. Şimdi, **Düzenleyici** dikey penceresini **X** işaretine tıklayarak kapatın. **X** simgesine yeniden tıklayarak **ADFTutorialDataFactory** için **Data Factory** giriş sayfasını görüntüleyin.

**Tebrikler!** Azure blob depolamadan bir Azure SQL veritabanına veri kopyalamak üzere işlem hattına sahip bir Azure veri fabrikasını başarıyla oluşturdunuz. 


## <a name="monitor-the-pipeline"></a>İşlem hattını izleme
Bu adımda, Azure data factory’de neler olduğunu izlemek için Azure Portal kullanacaksınız.    

### <a name="monitor-pipeline-using-monitor--manage-app"></a>İzleme ve Yönetme Uygulamasını kullanarak işlem hattını izleme
Aşağıdaki adımlar İzleme ve Yönetme uygulamasını kullanarak veri fabrikanızdaki işlem hatlarını nasıl izleyeceğinizi göstermektedir: 

1. Data factory’nin giriş sayfasındaki **İzleme ve Yönetme** kutucuğuna tıklayın.
   
    ![İzleme ve Yönetme kutucuğu](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-manage-tile.png) 
2. **İzleme ve Yönetme uygulaması** ayrı bir sekmede açılmalıdır. 

    > [!NOTE]
    > Web tarayıcısının "Yetkilendiriliyor..." durumunda takıldığını görürseniz **Üçüncü taraf tanımlama bilgilerini ve site verilerini engelle** ayarının işaretini kaldırın (ya da) **login.microsoftonline.com** için bir özel durum oluşturun ve ardından uygulamayı yeniden başlatmayı deneyin.

    ![İzleme ve Yönetme Uygulaması](./media/data-factory-copy-activity-tutorial-using-azure-portal/monitor-and-manage-app.png)
3. **Başlangıç saati** ve **Bitiş saati**'ni işlem hattınızın başlangıç (2017-05-11) ve bitiş saatlerini (2017-05-12) içerecek şekilde değiştirin ve **Uygula**'ya tıklayın.       
3. İşlem hattı başlangıç ve bitiş saatleri arasındaki saatlerle ilişkilendirilmiş **etkinlik pencerelerini** orta bölmede görebilirsiniz. 
4. Bir etkinlik penceresinin ayrıntılarını görmek için **Etkinlik Pencereleri** listesinde seçin. 
    ![Etkinlik penceresi ayrıntıları](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-window-details.png)

    Sağ taraftaki Etkinlik Penceresi Gezgini'nde geçerli saate (20:12) kadar olan dilimlerin işlendiğini (yeşil renkli olduğunu) göreceksiniz. 20-21, 21-22, 22-23 ve 23-00 dilimleri henüz işlenmemiştir.

    Sağ bölmedeki **Denemeler**bölümünde veri dilimi için çalıştırılan etkinlik hakkında bilgiler yer alır. Bir hata varsa onunla ilgili bilgiler de eklenir. Örneğin girdi klasörü veya kapsayıcı mevcut değilse ve dilim işleme başarısız olursa kapsayıcının veya klasörün bulunmadığını belirten bir hata iletisi görürsünüz.

    ![Etkinlik çalıştırma denemeleri](./media/data-factory-copy-activity-tutorial-using-azure-portal/activity-run-attempts.png) 
4. **SQL Server Management Studio**’yu başlatın, Azure SQL Veritabanı’na bağlanın ve veritabanındaki **emp** tablosuna satırların eklenmiş olduğunu doğrulayın.
    
    ![sql sorgu sonuçları](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)

Bu uygulamanın kullanımına ilişkin ayrıntılı bilgi için bkz. [İzleme ve Yönetme Uygulamasını kullanarak Azure Data Factory işlem hatlarını izleme ve yönetme](data-factory-monitor-manage-app.md).

### <a name="monitor-the-pipeline-using-diagram-view"></a>Diyagram görünümünü kullanarak işlem hattını izleme
Veri işlem hatlarını diyagram görünümüyle de izleyebilirsiniz.  

1. **Data Factory** dikey penceresinde **Diyagram**’a tıklayın.
   
    ![Data Factory Dikey Penceresi - Diyagram Kutucuğu](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-datafactoryblade-diagramtile.png)
2. Aşağıdaki görüntüye benzer bir diyagram görmeniz gerekir: 
   
    ![Diyagram görünümü](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-diagram-blade.png)  
5. Diyagram görünümünde **InputDataset**'e çift tıklayarak veri kümesinin dilimlerini görüntüleyebilirsiniz.  
   
    ![InputDataset seçiliyken veri kümeleri](./media/data-factory-copy-activity-tutorial-using-azure-portal/DataSetsWithInputDatasetFromBlobSelected.png)   
5. Tüm veri dilimlerini görmek için **Daha fazlasını gör** bağlantısına tıklayın. İşlem hattı başlangıç ve bitiş saatleri arasında 24 saatlik dilim göreceksiniz. 
   
    ![Tüm girdi veri dilimleri](./media/data-factory-copy-activity-tutorial-using-azure-portal/all-input-slices.png)  
   
    **emp.txt** dosyası her zaman **adftutorial\input** blob kapsayıcısında yer aldığından geçerli UTC saatine kadar olan tüm veri dilimleri **Hazır**'dır. Geleceğe yönelik dilimler hazır değil durumundadır. Alttaki **En son başarısız olan dilimler** bölümünde hiç dilim gösterilmediğini onaylayın.
6. Diyagram görünümüne ulaşana kadar dikey pencereleri kapatın veya diyagram görünümüne geçmek için sola kaydırın. Ardından **OutputDataset** öğesine çift tıklayın. 
8. Tüm dilimleri görmek için **OutputDataset** öğesine ait **Tablo** dikey penceresinde **Daha fazlasını gör** bağlantısına tıklayın.

    ![veri dilimleri dikey penceresi](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslices-blade.png) 
9. Geçerli UTC saatine kadar olan tüm dilimlerin durumunun **bekleyen yürütme** yerine => **Sürüyor** ==> **Hazır** durumuna geçtiğine dikkat edin. Geçmiş dilimler (geçerli saat öncesi) varsayılan olarak en yeniden en eskiye doğru işlenir. Örneğin geçerli saat 20:12 UTC ise 19-20 dilimi 18-19 diliminden önce işlenir. 20-21 dilimi varsayılan olarak zaman aralığın sonunda,yani 21 sonrasında işlenir.  
10. Listeden herhangi bir veri dilimine tıklayın; **Veri dilimi** dikey penceresini görmeniz gerekir. Bir etkinlik penceresiyle ilişkilendirilmiş veri parçasına dilim adı verilir. Bir dilim bir veya birden çok dosya olabilir.  
    
     ![veri dilimi dikey penceresi](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-dataslice-blade.png)
    
     Dilim **Hazır** durumunda değilse, Hazır olmayan ve geçerli dilimin yürütülmesini engelleyen yukarı akış dilimlerini **Hazır olmayan yukarı akış dilimleri** listesinde görebilirsiniz.
11. **VERİ DİLİMİ** dikey penceresinde, alttaki listede tüm etkinlik çalıştırmalarını görmelisiniz. **Etkinlik çalışma ayrıntıları** dikey penceresini görmek için bir **etkinlik çalışması**’na tıklayın. 
    
    ![Etkinlik Çalışma Ayrıntıları](./media/data-factory-copy-activity-tutorial-using-azure-portal/ActivityRunDetails.png)

    Bu dikey pencerede kopyalama işleminin ne kadar sürdüğünü, aktarım hızını, kaç bayt veri okunup yazıldığını, çalışma başlangıç zamanını, çalışma bitiş zamanını vs. görebilirsiniz.  
12. **ADFTutorialDataFactory** giriş dikey penceresine dönene kadar tüm dikey pencereleri kapatmak için **X** işaretine tıklayın.
13. (isteğe bağlı) Önceki adımlarda gördüğünüz dikey pencerelere ulaşmak için **Veri kümeleri** kutucuğuna veya **İşlem hatları** kutucuğuna tıklayın. 
14. **SQL Server Management Studio**’yu başlatın, Azure SQL Veritabanı’na bağlanın ve veritabanındaki **emp** tablosuna satırların eklenmiş olduğunu doğrulayın.
    
    ![sql sorgu sonuçları](./media/data-factory-copy-activity-tutorial-using-azure-portal/getstarted-sql-query-results.png)


## <a name="summary"></a>Özet
Bu öğreticide Azure blob’undan Azure SQL veritabanına veri kopyalamak üzere Azure data factory oluşturdunuz. Data factory, bağlı hizmetler, veri kümeleri ve işlem hattı oluşturmak için Azure Portal’ı kullandınız. Bu öğreticide gerçekleştirilen üst düzey adımlar şunlardır:  

1. Azure **data factory** oluşturuldu.
2. **Bağlı hizmetler** oluşturuldu:
   1. Girdi verilerini tutan Azure Storage hesabınıza bağlamak için **Azure Storage** bağlı hizmeti.     
   2. Çıktı verilerini tutan Azure SQL veritabanınıza bağlamak için **Azure SQL** bağlı hizmeti. 
3. İşlem hatları için girdi verilerini ve çıktı verilerini açıklayan oluşturulan **veri kümeleri**.
4. Kaynak olarak **BlobSource**’u, havuz olarak da **SqlSink**’i kapsayan **Kopyalama Etkinliği**’ne sahip oluşturulan **işlem hattı**.  

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir kopyalama işleminde kaynak veri deposu olarak Azure blob depolama alanını ve hedef veri deposu olarak Azure SQL veritabanını kullandınız. Aşağıdaki tabloda, kopyalama etkinliği tarafından kaynak ve hedef olarak desteklenen veri depolarının listesi sağlanmıştır: 

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

Veri deposundan/veri deposuna veri kopyalama hakkında bilgi edinmek için tablodaki veri deposunun bağlantısına tıklayın.
