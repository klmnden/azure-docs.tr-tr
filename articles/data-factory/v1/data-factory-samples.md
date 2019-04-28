---
title: Azure Data Factory - örnekleri
description: Azure Data Factory hizmetine gönderilen örnekler hakkında ayrıntılar sağlar.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: c0538b90-2695-4c4c-a6c8-82f59111f4ab
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 03127dc777588f669ef07af52c8f73d986bfe0ea
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61260470"
---
# <a name="azure-data-factory---samples"></a>Azure Data Factory - örnekleri
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [PowerShell örnekleri Data factory'de](../samples-powershell.md) ve [kod örneklerini Azure Kod örnekleri galerideki](https://azure.microsoft.com/resources/samples/?service=data-factory).


## <a name="samples-on-github"></a>GitHub’daki örnekler
[GitHub Azure-DataFactory depo](https://github.com/azure/azure-datafactory) yardımcı çeşitli örnekleri hızlı bir şekilde Azure Data Factory hizmetiyle artırma içeren (veya) komut dosyasını değiştirmek ve kendi uygulamada kullanın. Yaygın senaryolar için JSON kod parçacıkları Samples\JSON klasör içeriyor.

| Örnek | Açıklama |
|:--- |:--- |
| [ADF gözden geçirme](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ADFWalkthrough) |Bu örnek veri günlük dosyalarında ınsights'a kapatmak için Azure Data factory'yi kullanarak günlük dosyalarını işlemek için bir uçtan uca kılavuz sağlar. <br/><br/>Bu izlenecek yolda, Data Factory işlem hattı işlemleri örnek günlükleri toplar ve günlüklerinden verilerini başvuru verileriyle zenginleştirir ve son başlatılan bir pazarlama kampanyasının verimliliğini değerlendirmek için verileri dönüştürür. |
| [JSON örnekleri](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSON) |Bu örnek JSON örnekleri için genel senaryolar verilmiştir. |
| [HTTP veri yükleyici örneği](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HttpDataDownloaderSample) |Bu örnek nu tanıtarak özel bir .NET etkinliği kullanarak Azure Blob Depolama için bir HTTP uç noktasından veri yükleniyor. |
| [AppDomain Dot Net etkinlik örnek arası](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/CrossAppDomainDotNetActivitySample) |Bu örnek için ADF Başlatıcısı (örneğin, WindowsAzure.Storage verze 4.3.0, aynı zamanda Newtonsoft.Json v6.0.x, vb.) tarafından kullanılan derleme sürümlerini sınırlı değildir özel bir .NET etkinliği yazmanızı sağlar. |
| [R betiği çalıştırma](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) |Bu örnek RScript.exe çağırmak için kullanılan Data Factory özel etkinlik içerir. Bu örnek, (zaten yüklü R üzerinde sahip yalnızca kendi değil isteğe bağlı) HDInsight kümesi ile çalışır. |
| [HDInsight Hadoop kümesi üzerinde Spark işleri çağırma](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/Spark) |Bu örnek, MapReduce etkinliği Spark programının çağırmak için nasıl kullanılacağını gösterir. Spark programının yalnızca verileri bir Azure Blob kapsayıcısından diğerine kopyalar. |
| [Azure Machine Learning toplu iş Puanlama etkinliği kullanarak twitter çözümlemesi](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-AzureMLBatchScoringActivity) |Bu örnek AzureMLBatchScoringActivity twitter yaklaşım analizi, Puanlama, tahmin vb. gerçekleştiren bir Azure Machine Learning modeli çağırmak için nasıl kullanılacağını gösterir. |
| [Twitter özel etkinlik kullanma analizi](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TwitterAnalysisSample-CustomC%23Activity) |Bu örnek, özel bir .NET etkinliği twitter yaklaşım analizi, Puanlama, tahmin vb. gerçekleştiren bir Azure Machine Learning modeli çağırmak için nasıl kullanılacağını gösterir. |
| [Azure Machine Learning için parametreli işlem hatları](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ParameterizedPipelinesForAzureML/) |Örnek, bir uçtan uca sağlar C# dağıtmak için Puanlama ve her bölgelerin listesini nereden geldiğini Bu örnek ile birlikte bir dosyadan parameters.txt farklı bir bölgeye parametre ile yeniden eğitme N işlem hatları için kod. |
| [Başvuru verileri yenilemek için Azure Stream Analytics işleri](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs) |Bu örnek, Azure Data Factory ve Azure Stream Analytics birlikte başvuru verileri ile sorgular çalıştırın ve bir zamanlamaya göre başvuru verileri için yenileme için nasıl kullanılacağını gösterir. |
| [Şirket içi Hortonworks Hadoop ile karma işlem hattı](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/HybridPipelineWithOnPremisesHortonworksHadoop) |Örnek, bir HDInsight Hadoop kümesinin bulut dayanan storsimple çözümü gibi diğer işlem hedeflerine yalnızca eklediğiniz gibi işleri Data Factory'de çalıştırmak için bir işlem hedefi olarak bir şirket içi Hadoop kümesi kullanır. |
| [JSON dönüştürme aracı](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/JSONConversionTool) |Bu araç, önce bir 2015-07-01-preview en son veya 2015-07-01-preview (varsayılan) sürümünden Json'lerini dönüştürmenize olanak sağlar. |
| [U-SQL örnek giriş dosyası](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/U-SQL%20Sample%20Input%20File) |Bu dosya, bir U-SQL etkinliği tarafından kullanılan bir örnek dosyasıdır. |
| [BLOB Dosya Sil](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity) | Bu örnek gösteren bir C# ADF özel bir .net etkinliği bir parçası olarak dosyaları kopyaladıktan sonra Azure Blob konumu kaynak dosyaları silmek için kullanılan dosya.|

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager şablonları
Aşağıdaki Azure Resource Manager şablonları, Data Factory github'da bulabilirsiniz.

| Şablon | Açıklama |
| --- | --- |
| [Azure Blob depolama alanından Azure SQL veritabanına kopyalama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy) |Bu şablon dağıtımı bir Azure data factory belirtilen Azure blob depolama alanından Azure SQL veritabanına veri kopyalayan bir işlem hattı ile oluşturur |
| [Salesforce'tan Azure Blob depolamaya kopyalama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy) |Bir Azure data factory, bu şablonu dağıtma belirtilen Salesforce hesabındaki Azure blob depolama alanına veri kopyalayan bir işlem hattı ile oluşturur. |
| [Bir Azure HDInsight kümesinde Hive betiği çalıştırarak verileri dönüştürme](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation) |Bu şablon dağıtımı, bir Azure HDInsight Hadoop kümesinde örnek Hive betiği çalıştırarak verileri dönüştüren bir işlem hattı ile bir Azure data factory oluşturur. |

## <a name="samples-in-azure-portal"></a>Azure portalında örnekleri
Kullanabileceğiniz **örnek işlem hatları** örnek işlem hatları ve bunların ilişkili varlıkları (veri kümeleri ve bağlı hizmetler) veri fabrikanıza dağıtmak veri fabrikanızın giriş sayfasında kutucuk.

1. Veri Fabrikası oluşturma veya mevcut bir data factory açın. Bkz: [Blob depolamadan SQL veritabanına veri kopyalama kullanarak veri fabrikası için](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) veri fabrikası oluşturma adımları için.
2. İçinde **DATA FACTORY** data factory dikey penceresine tıklayın **örnek işlem hatları** Döşe.

    ![Örnek işlem hatları kutucuğu](./media/data-factory-samples/SamplePipelinesTile.png)
3. İçinde **örnek işlem hatları** dikey penceresinde tıklayın **örnek** dağıtmak istediğiniz.

    ![Örnek işlem hatları dikey penceresi](./media/data-factory-samples/SampleTile.png)
4. Örnek için yapılandırma ayarlarını belirtin. Örneğin, Azure depolama hesabı adını ve hesap anahtarı, Azure SQL sunucu adı, veritabanı, kullanıcı kimliği ve parola, vb.

    ![Örnek dikey penceresi](./media/data-factory-samples/SampleBlade.png)
5. Yapılandırma ayarlarını belirten ile bitirdikten sonra tıklayın **Oluştur** dağıtmak oluşturma / örnek işlem hatları ve ardışık düzen tarafından kullanılan bağlı hizmetler/tablolar için.
6. Dağıtım durumu örnek kutucuğa tıklandığında daha önce gördüğünüz **örnek işlem hatları** dikey penceresi.

    ![Dağıtım durumu](./media/data-factory-samples/DeploymentStatus.png)
7. Gördüğünüzde **dağıtım başarılı** kutucuğuna örnek, kapatmak için ileti **örnek işlem hatları** dikey penceresi.  
8. Üzerinde **DATA FACTORY** dikey penceresinde görürsünüz bağlı hizmetler, veri kümeleri ve işlem hatlarını veri fabrikanıza eklenir.  

    ![Data Factory dikey penceresi](./media/data-factory-samples/DataFactoryBladeAfter.png)

## <a name="samples-in-visual-studio"></a>Visual Studio Örnekleri
### <a name="prerequisites"></a>Önkoşullar
Bilgisayarınızda şunların yüklü olması gerekir:

* Visual Studio 2013 veya Visual Studio 2015
* Visual Studio 2013 veya Visual Studio 2015 için Azure SDK’sını indirin. [Azure İndirme Sayfası](https://azure.microsoft.com/downloads/)’na gidin ve **.NET** bölümündeki **VS 2013** veya **VS 2015**’e tıklayın.
* Visual Studio için en son Azure Data Factory eklentisini indirin: [VS 2013](https://visualstudiogallery.msdn.microsoft.com/754d998c-8f92-4aa7-835b-e89c8c954aa5) veya [VS 2015](https://visualstudiogallery.msdn.microsoft.com/371a4cf9-0093-40fa-b7dd-be3c74f49005). Visual Studio 2013 kullanıyorsanız, ayrıca aşağıdaki adımları uygulayarak eklentiyi güncelleştirebilirsiniz: Menüsünde **Araçları** -> **Uzantılar ve güncelleştirmeler** -> **çevrimiçi** -> **Visual Studio Galerisi**  ->  **Visual Studio için Microsoft Azure Data Factory Araçları** -> **güncelleştirme**.

### <a name="use-data-factory-templates"></a>Data Factory şablonlarını kullanma
1. Tıklayın **dosya** menüde işaret **yeni**, tıklatıp **proje**.
2. **Yeni Proje** iletişim kutusunda aşağıdaki adımları uygulayın:

   1. Seçin **DataFactory** altında **şablonları**.
   2. Seçin **veri fabrikası şablonları** sağ bölmede.
   3. Proje için bir **ad** girin.
   4. Seçin bir **konumu** projesi için.
   5. **Tamam** düğmesine tıklayın.

      ![Yeni proje iletişim kutusu](./media/data-factory-samples/vs-new-project-adf-templates.png)
3. İçinde **veri fabrikası şablonları** iletişim kutusunda, örnek şablonu seçin **kullanım örneği şablonları** bölümünde ve tıklayın **sonraki**. Aşağıdaki adımlar, kullanarak size yol **müşteri profili oluşturma** şablonu. Diğer örnekler için adımları benzerdir.

    ![Data Factory Şablonları iletişim kutusu](./media/data-factory-samples/vs-data-factory-templates-dialog.png)
4. İçinde **veri fabrikası Yapılandırması** iletişim kutusunda, tıklayın **sonraki** üzerinde **Data Factory Temelleri** sayfası.
5. Üzerinde **data factory Yapılandır** sayfasında, aşağıdaki adımları uygulayın:
   1. Seçin **yeni veri fabrikası oluşturma**. Belirleyebilirsiniz **var olan data factory'yi**.
   2. Girin bir **adı** data factory için.
   3. Seçin **Azure aboneliği** oluşturulacak veri fabrikasının istediğiniz.
   4. Seçin **kaynak grubu** data factory için.
   5. Seçin **Batı ABD**, **Doğu ABD**, veya **Kuzey Avrupa** için **bölge**.
   6. **İleri**’ye tıklayın.
6. İçinde **yapılandırma veri depoları** sayfasında, mevcut bir belirtin **Azure SQL veritabanı** ve **Azure depolama hesabı** (veya) veritabanı/depolama alanı oluşturun ve İleri'ye tıklayın.
7. İçinde **işlem yapılandırma** sayfasında, Varsayılanları seçin ve tıklayın **sonraki**.
8. İçinde **özeti** sayfasında tüm ayarları gözden geçirin ve tıklayın **sonraki**.
9. İçinde **dağıtım durumu** sayfasında, dağıtım işlemi tamamlanana kadar bekleyin ve tıklayın **son**.
10. Çözüm Gezgini’nde projeye sağ tıklayın ve ardından **Yayımla**’ya tıklayın.
11. **Microsoft hesabınızda oturum açın** iletişim kutusunu görmezseniz, Azure aboneliğindeki kimlik bilgilerini hesap için girin ve **oturum aç**’a tıklayın.
12. Aşağıdaki iletişim kutusunu göreceksiniz:

    ![Yayımla iletişim kutusu](./media/data-factory-build-your-first-pipeline-using-vs/publish.png)
13. **Data Factory’yi yapılandırma** sayfasında aşağıdaki adımları uygulayın:

    1. Onaylayın **var olan data factory'yi** seçeneği.
    2. Seçin **veri fabrikası** şablon kullanırken seçin vardı.
    3. **Öğeleri Yayımla** sayfasına geçmek için **İleri**’ye tıklayın. (Ad alanından çıkmak için, **İleri** düğmesi devre dışıysa **SEKME** tuşuna basın.)
14. **Öğeleri Yayımla** sayfasında tüm Data Factory varlıklarının işaretli olmasını sağlayın ve **Özet** sayfasına geçmek için **İleri**’ye tıklayın.     
15. Özeti gözden geçirin, dağıtım işlemini başlatmak ve **Dağıtım Durumu**’nu görüntülemek için **İleri**’ye tıklayın.
16. **Dağıtım Durumu** sayfasında dağıtım sürecinin durumunu görmelisiniz. Dağıtımını gerçekleştirdikten sonra Son'a tıklayın.

Bkz: [ilk veri fabrikanızı (Visual Studio) derleme](data-factory-build-your-first-pipeline-using-vs.md) Data Factory varlıklarını yazmak için Visual Studio kullanarak ve bunları Azure'a yayımlama hakkında ayrıntılar için.          
