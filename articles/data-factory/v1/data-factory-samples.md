---
title: Azure Data Factory - örnekleri
description: Azure Data Factory hizmetiyle birlikte örnekleri hakkında ayrıntılar sağlar.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: c0538b90-2695-4c4c-a6c8-82f59111f4ab
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 1e85d5f48ce998ebaf4ccaa231bb75449e2bab16
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-data-factory---samples"></a>Azure Data Factory - örnekleri
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [PowerShell örnekleri sürüm 2 veri fabrikasında](../samples-powershell.md) ve [kod örnekleri Azure Kod örnekleri galerisinde](https://azure.microsoft.com/en-us/resources/samples/?service=data-factory).


## <a name="samples-on-github"></a>Github'da örnekleri
[GitHub Azure-DataFactory depo](https://github.com/azure/azure-datafactory) yardımcı birkaç örnekleri kolayca artırma Azure Data Factory hizmetiyle içerir (veya) komut dosyaları değiştirebilirsiniz ve kendi uygulamada kullanabilirsiniz. Samples\JSON klasörü ortak senaryolar için JSON parçacıklarını içerir.

| Örnek | Açıklama |
|:--- |:--- |
| [ADF gözden geçirme](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFWalkthrough) |Bu örnek veri günlük dosyalarında ınsights'a kapatmak için Azure Data Factory kullanarak günlük dosyalarını işlemek için bir uçtan uca kılavuz sağlar. <br/><br/>Bu kılavuzda, Data Factory işlem hattı örnek günlükleri, işlemleri toplar ve başvuru verileri günlüklerinden verilerle aşağıdakilere zenginleştirir ve kısa süre önce başlatılan bir pazarlama kampanyası verimliliğini değerlendirilecek verileri dönüştürür. |
| [JSON örnekleri](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON) |Bu örnek için genel senaryolar JSON örnekler verilmektedir. |
| [HTTP veri yükleyici örneği](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) |Bu örnek showcases özel .NET etkinlik kullanarak Azure Blob Depolama için bir HTTP uç noktası verileri yükleniyor. |
| [AppDomain Dot Net etkinlik örnek arası](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |Bu örnek, derleme sürümlerini ADF Başlatıcısı (örneğin, WindowsAzure.Storage v4.3.0, Newtonsoft.Json v6.0.x, vb.) tarafından kullanılan kısıtlı değil özel bir .NET etkinlik Yazar olanak sağlar. |
| [R betiği çalıştırın](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) |Bu örnek RScript.exe çağırmak için kullanılan veri fabrikası özel etkinlik içerir. Bu örnek yalnızca R yüklü üzerinde zaten kendi (değil isteğe bağlı) Hdınsight kümesi ile çalışır. |
| [Hdınsight Hadoop kümesi üzerinde Spark işlerinin çağırma](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) |Bu örnek MapReduce etkinliği bir Spark programı çağırmak için nasıl kullanılacağını gösterir. Spark programın yalnızca verileri bir Azure Blob kapsayıcısından başka bir konuma kopyalar. |
| [Azure Machine Learning toplu iş Puanlama etkinliği kullanılarak twitter İnceleme](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-AzureMLBatchScoringActivity) |Bu örnek AzureMLBatchScoringActivity Puanlama, tahmin vb. twitter düşünceleri analiz gerçekleştiren bir Azure Machine Learning modelini çağırmak için nasıl kullanılacağını gösterir. |
| [Özel Etkinlik kullanılarak twitter İnceleme](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |Bu örnek özel bir .NET etkinliği Puanlama, tahmin vb. twitter düşünceleri analiz gerçekleştiren bir Azure Machine Learning modelini çağırmak için nasıl kullanılacağını gösterir. |
| [Azure Machine Learning için parametreli komut zincirleri](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ParameterizedPipelinesForAzureML/) |Örnek Puanlama ve her bölgelerin listesi ile bu örnek bulunan bir dosyadan parameters.txt geldiği yere farklı bir bölgeye parametresiyle yeniden eğitme N ardışık düzen dağıtmak için bir uçtan uca C# kodu sağlar. |
| [Başvuru verileri yenilemek için Azure Stream Analytics işleri](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs) |Bu örnek, Azure Data Factory ve Azure akış analizi birlikte başvuru verileri ile sorgular çalıştırın ve bir zamanlamaya göre başvuru verileri için yenileme kurulumu için nasıl kullanılacağını gösterir. |
| [Şirket içi Hortonworks Hadoop ile karma ardışık düzen](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HybridPipelineWithOnPremisesHortonworksHadoop) |Örnek bir şirket içi Hadoop kümesi yalnızca bir Hdınsight Hadoop kümesinde bulut tabanlı gibi diğer işlem hedefleri eklediğiniz gibi işleri veri fabrikasında çalıştırmak için bir işlem hedefi olarak kullanır. |
| [JSON dönüştürme aracı](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSONConversionTool) |Bu araç 2015-07-01-Önizleme son veya 2015-07-01-Önizleme (varsayılan) önce sürümünden JSONs dönüştürmenize olanak sağlar. |
| [U-SQL örnek giriş dosyası](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/U-SQL%20Sample%20Input%20File) |Bu dosya, U-SQL etkinlik tarafından kullanılan örnek bir dosyadır. |
| [BLOB dosya silme](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity) | Bu örnek ADF özel .net etkinlik bir parçası olarak dosyaları kopyalandıktan sonra Azure Blob konumu kaynak dosyaları silmek için kullanılan bir C# dosyasının gösterir.|

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları
Veri Fabrikası github'da aşağıdaki Azure Resource Manager şablonları bulabilirsiniz.

| Şablon | Açıklama |
| --- | --- |
| [Azure Blob depolama alanından Azure SQL veritabanına kopyalamak](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy) |Bu şablon dağıtımı Azure data factory belirtilen Azure blob depolama alanından Azure SQL veritabanına verileri kopyalayan bir işlem hattı oluşturur |
| [Salesforce Azure Blob depolama alanına kopyalama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy) |Bu şablon dağıtımı Azure data factory verileri Azure blob depolama alanına belirtilen Salesforce hesabındaki kopyalayan bir işlem hattı oluşturur. |
| [Bir Azure Hdınsight kümesinde Hive betiği çalıştırılarak verileri dönüştürün](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation) |Bu şablon dağıtımı Azure data factory Azure Hdınsight Hadoop kümesinde örnek Hive betiği çalıştırılarak verileri dönüştüren bir işlem hattı oluşturur. |

## <a name="samples-in-azure-portal"></a>Azure portalında örnekleri
Kullanabileceğiniz **örnek ardışık düzen** döşeme örnek ardışık düzen ve bunların ilişkili varlıkların (veri kümeleri ve bağlantılı Hizmetleri) veri fabrikanıza dağıtmanız için veri fabrikanızın giriş sayfasında.

1. Veri Fabrikası oluşturun veya varolan bir veri fabrikası açın. Bkz: [veri kopyalama Blob depolama alanından SQL Data Factory kullanarak veritabanına](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) veri fabrikası oluşturma adımları için.
2. İçinde **DATA FACTORY** data factory dikey penceresine tıklayın **örnek ardışık düzen** döşeme.

    ![Örnek komut zincirleri döşeme](./media/data-factory-samples/SamplePipelinesTile.png)
3. İçinde **örnek ardışık düzen** dikey penceresinde tıklatın **örnek** dağıtmak istediğiniz.

    ![Örnek komut zincirleri dikey penceresi](./media/data-factory-samples/SampleTile.png)
4. Örnek yapılandırma ayarlarını belirtin. Örneğin, Azure depolama hesabı adı ve hesap anahtarını, Azure SQL sunucu adı, veritabanı, kullanıcı kimliği ve parola, vb.

    ![Örnek dikey penceresi](./media/data-factory-samples/SampleBlade.png)
5. Yapılandırma ayarları belirterek tamamladıktan sonra tıklatın **oluşturma** oluşturun / örnek ardışık düzen ve ardışık düzen tarafından kullanılan bağlı hizmetler/tablolar dağıtmak için.
6. Daha önce üzerinde tıkladığınız örnek kutucuğu dağıtım durumunu görmek **örnek ardışık düzen** dikey.

    ![Dağıtım durumu](./media/data-factory-samples/DeploymentStatus.png)
7. Gördüğünüzde **dağıtım başarılı** örnek, kapatmak için döşeme iletide **örnek ardışık düzen** dikey.  
8. Üzerinde **DATA FACTORY** dikey penceresinde, gördüğünüz bağlı hizmetler, veri kümelerini ve ardışık düzen veri fabrikanıza eklenir.  

    ![Data Factory dikey penceresi](./media/data-factory-samples/DataFactoryBladeAfter.png)

## <a name="samples-in-visual-studio"></a>Visual Studio Örnekleri
### <a name="prerequisites"></a>Önkoşullar
Bilgisayarınızda şunların yüklü olması gerekir:

* Visual Studio 2013 veya Visual Studio 2015
* Visual Studio 2013 veya Visual Studio 2015 için Azure SDK’sını indirin. [Azure İndirme Sayfası](https://azure.microsoft.com/downloads/)’na gidin ve **.NET** bölümündeki **VS 2013** veya **VS 2015**’e tıklayın.
* Visual Studio için en son Azure Data Factory eklentisini indirin: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) veya [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Visual Studio 2013 kullanıyorsanız, aşağıdaki adımları uygulayarak eklentiyi güncelleştirebilirsiniz: Menüde **Araçlar** -> **Uzantılar ve Güncelleştirmeler** -> **Çevrimiçi** -> **Visual Studio Galerisi** -> **Visual Studio için Microsoft Azure Data Factory Araçları** -> **Güncelleştir**’e tıklayın.

### <a name="use-data-factory-templates"></a>Veri Fabrikası şablonları kullanın.
1. Tıklatın **dosya** menüsünde işaret **yeni**, tıklatıp **proje**.
2. İçinde **yeni proje** iletişim kutusunda, aşağıdaki adımları uygulayın:

   1. Seçin **DataFactory** altında **şablonları**.
   2. Seçin **veri fabrikası şablonları** sağ bölmede.
   3. Girin bir **adı** projesi için.
   4. Seçin bir **konumu** projesi için.
   5. **Tamam**’a tıklayın.

      ![Yeni proje iletişim kutusu](./media/data-factory-samples/vs-new-project-adf-templates.png)
3. İçinde **veri fabrikası şablonları** iletişim kutusunda, örnek şablonu seçin **kullanım örneği şablonları** bölümünde ve tıklatın **sonraki**. Aşağıdaki adımlar kullanılarak yol **müşteri profil** şablonu. Diğer örnekler için adımları benzerdir.

    ![Veri Fabrikası Şablonları iletişim kutusu](./media/data-factory-samples/vs-data-factory-templates-dialog.png)
4. İçinde **veri fabrikası Yapılandırması** iletişim kutusunda, tıklatın **sonraki** üzerinde **veri fabrikası Temelleri** sayfası.
5. Üzerinde **data factory Yapılandır** sayfasında, aşağıdaki adımları uygulayın:
   1. Seçin **yeni veri fabrikası oluşturma**. Öğesini de seçebilirsiniz **mevcut veri fabrikası kullanmak**.
   2. Girin bir **adı** data factory için.
   3. Seçin **Azure aboneliği** oluşturulacak data factory istediğiniz içinde.
   4. Seçin **kaynak grubu** data factory için.
   5. Seçin **Batı ABD**, **Doğu ABD**, veya **Kuzey Avrupa** için **bölge**.
   6. **İleri**’ye tıklayın.
6. İçinde **yapılandırma veri depolarına** sayfasında, varolan bir belirtin **Azure SQL veritabanı** ve **Azure depolama hesabı** (veya) veritabanı/depolama alanı oluşturun ve İleri'yi tıklatın.
7. İçinde **işlem yapılandırma** sayfasında, Varsayılanları seçin ve tıklayın **sonraki**.
8. İçinde **Özet** sayfasında, tüm ayarları gözden geçirin ve tıklayın **sonraki**.
9. İçinde **dağıtım durumu** sayfasında, dağıtım işlemi tamamlanana kadar bekleyin ve tıklayın **son**.
10. Çözüm Gezgini’nde projeye sağ tıklayın ve ardından **Yayımla**’ya tıklayın.
11. **Microsoft hesabınızda oturum açın** iletişim kutusunu görmezseniz, Azure aboneliğindeki kimlik bilgilerini hesap için girin ve **oturum aç**’a tıklayın.
12. Aşağıdaki iletişim kutusunu göreceksiniz:

    ![Yayımla iletişim kutusu](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)
13. **Data Factory’yi yapılandırma** sayfasında aşağıdaki adımları uygulayın:

    1. Onaylayın **mevcut veri fabrikası kullanmak** seçeneği.
    2. Seçin **veri fabrikası** şablon kullanırken seçin vardı.
    3. **Öğeleri Yayımla** sayfasına geçmek için **İleri**’ye tıklayın. (Ad alanından çıkmak için, **İleri** düğmesi devre dışıysa **SEKME** tuşuna basın.)
14. **Öğeleri Yayımla** sayfasında tüm Data Factory varlıklarının işaretli olmasını sağlayın ve **Özet** sayfasına geçmek için **İleri**’ye tıklayın.     
15. Özeti gözden geçirin, dağıtım işlemini başlatmak ve **Dağıtım Durumu**’nu görüntülemek için **İleri**’ye tıklayın.
16. **Dağıtım Durumu** sayfasında dağıtım sürecinin durumunu görmelisiniz. Dağıtımını gerçekleştirdikten sonra Son'a tıklayın.

Bkz: [ilk data factory'nizi (Visual Studio) derleme](data-factory-build-your-first-pipeline-using-vs.md) Data Factory varlıklarını yazmak için Visual Studio kullanarak ve bunları Azure'a yayımlama hakkında ayrıntılar için.          
