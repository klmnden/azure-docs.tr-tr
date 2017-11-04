---
title: "Azure Data Factory - sık sorulan sorular"
description: "Azure Data Factory hakkında sık sorulan sorular."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 532dec5a-7261-4770-8f54-bfe527918058
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
robots: noindex
ms.openlocfilehash: 47ebebbd838d245c2559bff8d8750e80dbcc3fd8
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="azure-data-factory---frequently-asked-questions"></a>Azure Data Factory - sık sorulan sorular
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [sık sorulan soru - Data Factory sürüm 2](../frequently-asked-questions.md).

## <a name="general-questions"></a>Genel sorular
### <a name="what-is-azure-data-factory"></a>Azure Data Factory nedir?
Veri Fabrikası olan bulut tabanlı bir veri tümleştirme hizmeti **taşınmasını ve dönüştürülmesini veri otomatikleştirir**. Ham maddeleri alıp bunları tamamlanmış ürünlere dönüştürmek için donanım çalıştıran yalnızca bir fabrikası gibi Data Factory ham verileri toplayan ve kullanıma hazır bilgilere dönüştürür var olan hizmetleri düzenler.

Veri fabrikası, hem şirket içi ve bulut veri depolarına yanı sıra Azure Hdınsight ve Azure Data Lake Analytics gibi işlem hizmetleri kullanarak işlem/dönüştürme verileri arasında verileri taşımak için veri tabanlı iş akışları oluşturmanıza olanak sağlar. Gereksinim duyduğunuz bir eylem gerçekleştiren bir işlem hattını oluşturduktan sonra (saatlik, günlük, haftalık vb.) düzenli aralıklarla çalışacak şekilde zamanlayabilirsiniz.   

Daha fazla bilgi için bkz: [genel bakış & anahtar kavramlar](data-factory-introduction.md).

### <a name="where-can-i-find-pricing-details-for-azure-data-factory"></a>Nereden bulabilirim Azure Data Factory için fiyatlandırma ayrıntıları?
Bkz: [veri fabrikası fiyatlandırma ayrıntıları sayfası] [ adf-pricing-details] Azure Data Factory için fiyatlandırma ayrıntıları için.  

### <a name="how-do-i-get-started-with-azure-data-factory"></a>Azure Data Factory ile çalışmaya nasıl?
* Azure Data Factory genel bakış için bkz: [Azure Data Factory'ye giriş](data-factory-introduction.md).
* Bir öğretici için nasıl **copy/move veri** kopyalama etkinliği'ni kullanarak bkz [kopyalama verileri Azure Blob depolama alanından Azure SQL veritabanına](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
* Bir öğretici için nasıl **verileri** Hdınsight Hive etkinliği kullanma. Bkz: [Hadoop kümesindeki Hive betiği çalıştırılarak verileri işlemek](data-factory-build-your-first-pipeline.md)

### <a name="what-is-the-data-factorys-region-availability"></a>Veri fabrikasının bölge kullanılabilirliği nedir?
Veri Fabrikası sağlanmıştır **BİZE Batı** ve **Kuzey Avrupa**. Veri fabrikaları tarafından kullanılan işlem ve depolama hizmetleri başka bölgelerde olabilir. Bkz: [desteklenen bölgeler](data-factory-introduction.md#supported-regions).

### <a name="what-are-the-limits-on-number-of-data-factoriespipelinesactivitiesdatasets"></a>Veri fabrikaları/işlem hatları/etkinlikleri/veri kümeleri sayısının sınırları nelerdir?
Bkz: **Azure veri fabrikası sınırları** bölümünü [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../../azure-subscription-service-limits.md#data-factory-limits) makalesi.

### <a name="what-is-the-authoringdeveloper-experience-with-azure-data-factory-service"></a>Azure Data Factory hizmetiyle yazma geliştirici deneyimi nedir?
Yazar/aşağıdaki araçlar/Sdk'lardan birini kullanarak veri fabrikaları oluşturabilirsiniz:

* **Azure portal** Azure Portal'daki Data Factory dikey penceresi, veri fabrikaları bağlı ad hizmetleri oluşturmak zengin bir kullanıcı arabirimi sağlar. **Data Factory düzenleyici**, aynı zamanda portal parçası olan kolayca bağlı hizmetler, tablolar, veri kümelerini ve ardışık düzen bu yapılar için JSON tanımları belirterek oluşturmanıza olanak sağlar. Bkz: [Azure portalını kullanarak ilk veri işlem hattınızı oluşturma](data-factory-build-your-first-pipeline-using-editor.md) oluşturmak ve bir veri fabrikası dağıtmak için portal/Düzenleyicisi'ni kullanarak bir örnek için.
* **Visual Studio** bir Azure data factory oluşturmak için Visual Studio'yu kullanabilirsiniz. Bkz: [Visual Studio kullanarak ilk veri işlem hattınızı oluşturma](data-factory-build-your-first-pipeline-using-vs.md) Ayrıntılar için.
* **Azure PowerShell** bkz [oluşturma ve İzleyici Azure PowerShell kullanarak Azure Data Factory](data-factory-build-your-first-pipeline-using-powershell.md) bir öğretici/PowerShell kullanarak bir veri fabrikası oluşturmaya yönelik kılavuz. Bkz: [Data Factory Cmdlet başvurusu] [ adf-powershell-reference] Data Factory cmdlet'lerini ilişkin kapsamlı belgeler için MSDN kitaplığında içerik.
* **.NET sınıf kitaplığı** Data Factory .NET SDK kullanarak program aracılığıyla veri fabrikaları oluşturabilirsiniz. Bkz: [oluşturun, izlemek ve .NET SDK kullanarak veri fabrikaları yönetmek](data-factory-create-data-factories-programmatically.md) .NET SDK kullanarak bir veri fabrikası oluşturma kılavuz. Bkz: [Data Factory sınıf kitaplığı başvurusu] [ msdn-class-library-reference] veri fabrikası .NET SDK'sının kapsamlı belgeler.
* **REST API** oluşturmak ve veri fabrikaları dağıtmak için Azure Data Factory hizmeti tarafından sunulan REST API de kullanabilirsiniz. Bkz: [Data Factory REST API Başvurusu] [ msdn-rest-api-reference] veri fabrikası REST API kapsamlı belgeler.
* **Azure Resource Manager şablonu** bkz [Öğreticisi: Azure Resource Manager şablonu kullanarak ilk Azure data factory'nizi derleme](data-factory-build-your-first-pipeline-using-arm.md) fo ayrıntıları.

### <a name="can-i-rename-a-data-factory"></a>Veri Fabrikası yeniden adlandırabilir miyim?
Hayır. Diğer Azure kaynaklarına gibi bir Azure data factory adı değiştirilemez.

### <a name="can-i-move-a-data-factory-from-one-azure-subscription-to-another"></a>I data factory bir Azure aboneliğinden diğerine taşıyabilir miyim?
Evet. Kullanım **taşıma** aşağıdaki çizimde gösterildiği gibi veri fabrikası dikey penceresinde düğmesi:

![Veri Fabrikası taşıma](media/data-factory-faq/move-data-factory.png)

### <a name="what-are-the-compute-environments-supported-by-data-factory"></a>Data Factory ile desteklenen bilgi işlem ortamları nelerdir?
Aşağıdaki tabloda, bilgi işlem ortamları Data Factory ve bunlar üzerinde çalışan etkinlikleri tarafından desteklenen bir listesini sağlar.

| İşlem ortamı | etkinlikler |
| --- | --- |
| [İsteğe bağlı Hdınsight kümesi](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) veya [kendi Hdınsight kümenizi](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) |[DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Pig](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [Hadoop akış](data-factory-hadoop-streaming-activity.md) |
| [Azure toplu işlem](data-factory-compute-linked-services.md#azure-batch-linked-service) |[DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](data-factory-compute-linked-services.md#azure-machine-learning-linked-service) |[Machine Learning etkinlikleri: Toplu Yürütme ve Kaynak Güncelleştirme](data-factory-azure-ml-batch-execution-activity.md) |
| [Azure Data Lake Analytics](data-factory-compute-linked-services.md#azure-data-lake-analytics-linked-service) |[Data Lake Analytics U-SQL](data-factory-usql-activity.md) |
| [Azure SQL](data-factory-compute-linked-services.md#azure-sql-linked-service), [Azure SQL veri ambarı](data-factory-compute-linked-services.md#azure-sql-data-warehouse-linked-service), [SQL Server](data-factory-compute-linked-services.md#sql-server-linked-service) |[Saklı Yordam](data-factory-stored-proc-activity.md) |

### <a name="how-does-azure-data-factory-compare-with-sql-server-integration-services-ssis"></a>Azure Data Factory SQL Server Integration Services (SSIS) ile nasıl karşılaştırmak? 
Bkz: [Azure Data Factory vs. SSIS](http://www.sqlbits.com/Sessions/Event15/Azure_Data_Factory_vs_SSIS) bizim MVP (en değerli uzmanları) birini sunudan: Reza Rad. Veri Fabrikası'nda son değişikliklerden bazıları slayt paketiyle listelenmeyebilir. Daha fazla yetenekleri sürekli olarak Azure Data Factory'ye ekliyorsunuz. Daha fazla yetenekleri sürekli olarak Azure Data Factory'ye ekliyorsunuz. Biz bu güncelleştirmeleri Microsoft'tan veri tümleştirme teknolojileri karşılaştırması içine süre daha sonra bu yıl dahil.   

## <a name="activities---faq"></a>Etkinlikler - SSS
### <a name="what-are-the-different-types-of-activities-you-can-use-in-a-data-factory-pipeline"></a>Bir Data Factory işlem hattı kullanabileceğiniz etkinlikler farklı türleri nelerdir?
* [Veri taşıma etkinlikleri](data-factory-data-movement-activities.md) verileri taşımak için.
* [Veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) işlem/veri dönüştürme için.

### <a name="when-does-an-activity-run"></a>Bir etkinlik çalıştırıldığında?
**Kullanılabilirlik** çıkış veri tablosuna yapılandırma ayarı, etkinlik çalıştırıldığında belirler. Giriş veri kümeleri belirtilirse, etkinlik tüm giriş verilerini bağımlılıkların olup olmadığını denetler (diğer bir deyişle, **hazır** durumu) çalıştıran başlatılmadan önce.

## <a name="copy-activity---faq"></a>Kopya etkinliği - SSS
### <a name="is-it-better-to-have-a-pipeline-with-multiple-activities-or-a-separate-pipeline-for-each-activity"></a>Birden çok etkinliği ile işlem hattı ya da her etkinlik için ayrı bir işlem hattı için daha iyi mi?
Ardışık düzen, ilgili etkinlikler paketini beklenir. Bunları bağlamak veri kümeleri ardışık dışında herhangi bir etkinlik tarafından tüketilen değil, bir işlem hattında etkinlikleri tutabilirsiniz. Böylece birbirleri ile Hizala bu şekilde, zinciri ardışık düzen etkin dönem için ihtiyaç duymaz. Ayrıca, ardışık düzeni güncellenirken ardışık düzene iç tablolardaki veri bütünlüğü daha iyi korunur. Ardışık Düzen güncelleştirme temelde Ardışık düzenin içindeki tüm etkinlikleri durdurur, bunları kaldırır ve bunları yeniden oluşturur. Perspektif geliştirme, ayrıca bir JSON dosyası ardışık düzeni için ilgili etkinlikler içinde veri akışını daha kolay olabilir.

### <a name="what-are-the-supported-data-stores"></a>Desteklenen veri depoları nelerdir?
Data Factory’deki Kopyalama Etkinliği bir kaynak veri deposundan havuz veri deposuna verileri kopyalar. Data Factory aşağıdaki veri depolarını destekler. Herhangi bir kaynaktan gelen veriler herhangi bir havuza yazılabilir. Bir depoya veya depodan veri kopyalama hakkında bilgi edinmek için veri deposuna tıklayın.

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> * taşıyan veri depoları şirket içi veya Azure IaaS üzerinde olabilir ve bir şirket içi/Azure IaaS makinesine [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) yüklemenizi gerektirir.

### <a name="what-are-the-supported-file-formats"></a>Desteklenen dosya biçimleri nelerdir?
[!INCLUDE [data-factory-file-format](../../../includes/data-factory-file-format.md)]

### <a name="where-is-the-copy-operation-performed"></a>Kopyalama işlemi gerçekleştirildiği?
Bkz: [genel olarak kullanılabilir veri taşıma](data-factory-data-movement-activities.md#global) ayrıntıları bölümü. Kısacası, bir şirket içi veri deposu söz konusu olduğunda, veri yönetimi ağ geçidi, şirket içi ortamınızda kopyalama işlemi gerçekleştirilir. Ve veri taşıma iki bulut depoları arasında olduğunda, kopyalama işlemi aynı Coğrafya havuz konumda en yakın bölgede gerçekleştirilir.

## <a name="hdinsight-activity---faq"></a>Hdınsight etkinliği - SSS
### <a name="what-regions-are-supported-by-hdinsight"></a>Hangi bölgeleri Hdınsight tarafından destekleniyor mu?
Aşağıdaki makalede coğrafi kullanılabilirlik bölümüne bakın: veya [Hdınsight fiyatlandırma ayrıntıları][hdinsight-supported-regions].

### <a name="what-region-is-used-by-an-on-demand-hdinsight-cluster"></a>Hangi bölge isteğe bağlı Hdınsight kümesi tarafından kullanılır?
İsteğe bağlı Hdınsight kümesi ile küme kullanılacak belirtilen depolama bulunduğu aynı bölgede oluşturulur.    

### <a name="how-to-associate-additional-storage-accounts-to-your-hdinsight-cluster"></a>Ek depolama hesapları Hdınsight kümenize ilişkilendirmek nasıl?
Kendi Hdınsight kümenizi (BYOC - Getir bilgisayarınızı kendi küme) kullanıyorsanız, aşağıdaki konulara bakın:

* [Alternatif depolama hesapları ve meta Depolarla Hdınsight kümesi kullanma][hdinsight-alternate-storage]
* [Ek depolama hesapları ile Hdınsight Hive kullanma][hdinsight-alternate-storage-2]

Data Factory hizmeti tarafından oluşturulan bir isteğe bağlı küme kullanıyorsanız, böylece bunları sizin adınıza kaydedebilirsiniz Data Factory hizmetinin Hdınsight için ek depolama hesapları hizmeti bağlı belirtin. İsteğe bağlı hizmet JSON tanımında kullanmak **additionalLinkedServiceNames** özelliği aşağıdaki JSON parçacığında gösterildiği gibi diğer depolama hesaplarını belirtin:

```JSON
{
    "name": "MyHDInsightOnDemandLinkedService",
    "properties":
    {
        "type": "HDInsightOnDemandLinkedService",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "LinkedService-SampleData",
            "additionalLinkedServiceNames": [ "otherLinkedServiceName1", "otherLinkedServiceName2" ]
        }
    }
}
```
Yukarıdaki örnekte, Hdınsight kümesi alternatif depolama hesapları erişmesi gereken kimlik bilgilerini, tanımlarını içeren bağlı hizmetler otherLinkedServiceName1 ve otherLinkedServiceName2 temsil eder.

## <a name="slices---faq"></a>Dilimler - SSS
### <a name="why-are-my-input-slices-not-in-ready-state"></a>Neden my girdi dilimi hazır durumda değil misiniz?
Genel bir hata değildir ayarı **dış** özelliğine **true** girdi verileri data factory (data factory tarafından üretilen değil) dış olduğunda giriş veri kümesi üzerinde.

Aşağıdaki örnekte, yalnızca ayarlamanız gerekir **dış** true üzerinde **dataset1**.  

**DataFactory1** kanal 1: dataset1 activity1 -> -> dataset2 -> activity2 dataset3 ardışık düzen 2 ->: dataset3 -> activity3 ancak dataset4 ->

Başka bir data factory (2 1 veri fabrikasında ardışık düzen tarafından üretilen) dataset4 alan bir işlem hattı ile varsa, veri kümesi bir farklı veri fabrikası (DataFactory1, değil DataFactory2) oluşturduğundan dataset4 dış bir veri kümesi işaretleyin.  

**DataFactory2**    
Ardışık Düzen 1: dataset4 -> activity4 dataset5 ->

Dış özelliği doğru olarak ayarlanırsa, girdi verileri girdi veri kümesi tanımında belirtilen konumda var olup olmadığını doğrulayın.

### <a name="how-to-run-a-slice-at-another-time-than-midnight-when-the-slice-is-being-produced-daily"></a>Zaman dilimi günlük üretilen gece'den başka bir zaman dilimi çalıştırmak nasıl?
Kullanım **uzaklık** özelliği üretilecek dilim istediğiniz süreyi belirtin. Bkz: [Dataset kullanılabilirliği](data-factory-create-datasets.md#dataset-availability) bu özellik hakkında ayrıntılı bilgi için bölüm. Kısa bir örnek aşağıda verilmiştir:

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
Günlük dilimler Başlat **6'da** varsayılan gece yerine.     

### <a name="how-can-i-rerun-a-slice"></a>Bir dilim nasıl yeniden çalıştırabilir miyim?
Bir dilim aşağıdaki yollardan biriyle çalıştırabilirsiniz:

* Bir etkinlik penceresini veya dilimi yeniden çalıştırmak için izleme ve yönetme uygulaması'nı kullanın. Bkz: [seçilen yeniden çalıştır etkinliği windows](data-factory-monitor-manage-app.md#perform-batch-actions) yönergeler için.   
* Tıklatın **çalıştırmak** komut çubuğunda **veri DİLİMİ** dikey Azure portalında dilim için.
* Çalıştırma **Set-AzureRmDataFactorySliceStatus** durum cmdlet'iyle kümesine **bekleyen** ve dilim için.   

    ```PowerShell
    Set-AzureRmDataFactorySliceStatus -Status Waiting -ResourceGroupName $ResourceGroup -DataFactoryName $df -TableName $table -StartDateTime "02/26/2015 19:00:00" -EndDateTime "02/26/2015 20:00:00"
    ```
Bkz: [Set-AzureRmDataFactorySliceStatus] [ set-azure-datafactory-slice-status] cmdlet hakkında ayrıntılı bilgi için.

### <a name="how-long-did-it-take-to-process-a-slice"></a>Bir dilim işlemek için süresini?
Veri dilimi işlemek için ne kadar geçen bilmek için izleme ve yönetme uygulaması etkinlik penceresini Gezgini'ni kullanın. Bkz: [etkinlik penceresini Explorer](data-factory-monitor-manage-app.md#activity-window-explorer) Ayrıntılar için.

Ayrıca Azure portalında aşağıdakileri yapabilirsiniz:  

1. Tıklatın **veri kümeleri** döşemesinin **DATA FACTORY** veri fabrikanızın dikey.
2. Belirli veri kümesi tıklayın **veri kümeleri** dikey.
3. Gelen ilgilendiğiniz dilimi seçin **son dilimler** listesini **tablo** dikey.
4. Çalıştırma etkinliği tıklatın **etkinlik çalışması** listesini **veri DİLİMİ** dikey.
5. Tıklatın **özellikleri** döşemesinin **etkinlik çalışma ayrıntıları** dikey.
6. Görmeniz gerekir **süresi** alan bir değere sahip. Dilim işlenmesi için geçen süre değerdir.   

### <a name="how-to-stop-a-running-slice"></a>Çalışan bir dilim durdurmak nasıl?
Ardışık Düzen yürütülmesini Durdur ihtiyacınız varsa, kullanabileceğiniz [Suspend-AzureRmDataFactoryPipeline](/powershell/module/azurerm.datafactories/suspend-azurermdatafactorypipeline) cmdlet'i. Şu anda, ardışık düzen askıya sürüyor dilim yürütmeleri durdurmaz. Devam eden yürütmeleri tamamladıktan sonra ek bir dilim çekilir.

Gerçekten tüm yürütmeleri hemen durdurmak istiyorsanız, tek yolu ardışık silip yeniden oluşturmanız olacaktır. Ardışık düzeni silerek seçerseniz, tablolar ve ardışık düzen tarafından kullanılan bağlantılı hizmetler silmek gerekmez.

[create-factory-using-dotnet-sdk]: data-factory-create-data-factories-programmatically.md
[msdn-class-library-reference]: /dotnet/api/microsoft.azure.management.datafactories.models
[msdn-rest-api-reference]: /rest/api/datafactory/

[adf-powershell-reference]: /powershell/resourcemanager/azurerm.datafactories/v2.3.0/azurerm.datafactories
[azure-portal]: http://portal.azure.com
[set-azure-datafactory-slice-status]: /powershell/resourcemanager/azurerm.datafactories/v2.3.0/set-azurermdatafactoryslicestatus

[adf-pricing-details]: http://go.microsoft.com/fwlink/?LinkId=517777
[hdinsight-supported-regions]: http://azure.microsoft.com/pricing/details/hdinsight/
[hdinsight-alternate-storage]: http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx
[hdinsight-alternate-storage-2]: http://blogs.msdn.com/b/cindygross/archive/2014/05/05/use-additional-storage-accounts-with-hdinsight-hive.aspx
