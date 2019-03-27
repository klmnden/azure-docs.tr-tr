---
title: Azure Data Factory - sık sorulan sorular
description: Sık sorulan Azure Data Factory hakkında sorular.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 532dec5a-7261-4770-8f54-bfe527918058
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: shlo
robots: noindex
ms.openlocfilehash: 518e3fa842c5283dc20a6111773bd55451f026b6
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58485858"
---
# <a name="azure-data-factory---frequently-asked-questions"></a>Azure Data Factory - sık sorulan sorular
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [sık sorulan soru - Data Factory](../frequently-asked-questions.md).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="general-questions"></a>Genel sorular
### <a name="what-is-azure-data-factory"></a>Azure Data Factory nedir?
Data Factory, bulut tabanlı veri tümleştirme hizmeti **taşımayı ve dönüştürmeyi otomatikleştiren**. Ham maddeleri alıp bunları tamamlanmış ürünlere dönüştürmek için donanım çalıştıran yalnızca bir fabrikası gibi Data Factory, ham verileri toplayan ve kullanıma hazır bilgilere dönüştürün var olan hizmetleri düzenler.

Veri fabrikası, hem şirket içi ve bulut veri depolarının yanı sıra Azure HDInsight ve Azure Data Lake Analytics gibi işlem hizmetlerini kullanarak işlem/dönüştürme verileri arasında veri taşımak için veri odaklı iş akışları oluşturmanıza olanak sağlar. İhtiyacınız olan eylemi gerçekleştiren bir işlem hattı oluşturduktan sonra (saatlik, günlük, haftalık vb.) düzenli aralıklarla çalışacak şekilde zamanlayabilirsiniz.   

Daha fazla bilgi için [genel bakış ve temel kavramlar](data-factory-introduction.md).

### <a name="where-can-i-find-pricing-details-for-azure-data-factory"></a>Nereden bulabilirim fiyatlandırma ayrıntıları için Azure Data Factory?
Bkz: [Data Factory fiyatlandırma ayrıntıları sayfasına] [ adf-pricing-details] Azure Data Factory için fiyatlandırma ayrıntıları.  

### <a name="how-do-i-get-started-with-azure-data-factory"></a>Azure Data factory'yi kullanmaya nasıl başlayabilirim?
* Azure Data Factory'ye genel bakış için bkz. [Azure Data Factory'ye giriş](data-factory-introduction.md).
* Hakkında bir öğretici için **veri kopyalama/taşıma** kullanarak kopyalama etkinliği bkz [verileri Azure Blob depolama alanından Azure SQL veritabanına kopyalama](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
* Hakkında bir öğretici için **verileri dönüştürme** kullanarak HDInsight Hive etkinliği. Bkz: [Hadoop kümesinde Hive betiği çalıştırarak verileri işleyin](data-factory-build-your-first-pipeline.md)

### <a name="what-is-the-data-factorys-region-availability"></a>Veri fabrikasının bölge kullanılabilirliği nedir?
Data Factory, kullanılabilir **ABD Batı** ve **Kuzey Avrupa**. Veri fabrikası tarafından kullanılan işlem ve depolama hizmetleri, başka bölgelerde olabilir. Bkz: [desteklenen bölgeler](data-factory-introduction.md#supported-regions).

### <a name="what-are-the-limits-on-number-of-data-factoriespipelinesactivitiesdatasets"></a>Veri fabrikaları/işlem hatları/etkinlik/veri kümeleri sayısı sınırları nelerdir?
Bkz: **Azure veri fabrikası sınırları** bölümünü [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../../azure-subscription-service-limits.md#data-factory-limits) makalesi.

### <a name="what-is-the-authoringdeveloper-experience-with-azure-data-factory-service"></a>Azure Data Factory hizmetiyle yazma geliştirici deneyimi nedir?
Yazar/aşağıdaki araçlardan/Sdk'lardan birini kullanarak veri fabrikaları oluşturabilirsiniz:

* **Azure portalında** Azure Portal'daki Data Factory dikey pencereleri veri fabrikaları ad bağlantılı hizmetler oluşturmak için zengin kullanıcı arabirimi sağlar. **Data Factory düzenleyici**, ayrıca Portalı'nın parçası olduğu kolayca bağlı hizmetler, tablolar, veri kümeleri ve işlem hatlarını bu yapılar için JSON tanımları belirterek oluşturmanıza imkan tanır. Bkz: [Azure portalını kullanarak ilk veri işlem hattınızı](data-factory-build-your-first-pipeline-using-editor.md) oluşturmak ve veri fabrikası dağıtmak için portal/Düzenleyicisi'ni kullanarak bir örnek için.
* **Visual Studio** bir Azure veri fabrikası oluşturmak için Visual Studio'yu kullanabilirsiniz. Bkz: [Visual Studio kullanarak ilk veri işlem hattınızı](data-factory-build-your-first-pipeline-using-vs.md) Ayrıntılar için.
* **Azure PowerShell** bkz [oluşturma ve Azure PowerShell kullanarak Azure Data factory'yi izleme](data-factory-build-your-first-pipeline-using-powershell.md) PowerShell kullanarak veri fabrikası oluşturmak için öğretici/kılavuz. Bkz: [Data Factory Cmdlet başvurusu] [ adf-powershell-reference] kapsamlı belgeler Data Factory cmdlet'leri için MSDN Kitaplığı içeriği.
* **.NET sınıf kitaplığı** Data Factory .NET SDK'sını kullanarak programlama yoluyla veri fabrikaları oluşturabilirsiniz. Bkz [oluşturma, izleme ve yönetme .NET SDK kullanarak veri fabrikaları](data-factory-create-data-factories-programmatically.md) kılavuz .NET SDK kullanarak veri fabrikası oluşturma. Bkz: [Data Factory sınıf kitaplığı başvurusu] [ msdn-class-library-reference] Data Factory .NET SDK'ın kapsamlı belgeler için.
* **REST API** oluşturup veri fabrikaları dağıtmak için Azure Data Factory hizmeti tarafından kullanıma sunulan REST API de kullanabilirsiniz. Bkz: [Data Factory REST API Başvurusu] [ msdn-rest-api-reference] Data Factory REST API, kapsamlı belgeler için.
* **Azure Resource Manager şablonu** bkz [Öğreticisi: Azure Resource Manager şablonu kullanarak ilk Azure data factory'nizi derleme](data-factory-build-your-first-pipeline-using-arm.md) fo ayrıntıları.

### <a name="can-i-rename-a-data-factory"></a>Veri Fabrikası yeniden adlandırabilir miyim?
Hayır. Diğer Azure kaynakları gibi Azure veri fabrikası adı değiştirilemez.

### <a name="can-i-move-a-data-factory-from-one-azure-subscription-to-another"></a>Veri Fabrikası bir Azure aboneliğine ait diğerine taşıyabilirim?
Evet. Kullanım **taşıma** düğme Aşağıdaki diyagramda gösterildiği gibi data factory dikey penceresinde:

![Veri Fabrikası Taşı](media/data-factory-faq/move-data-factory.png)

### <a name="what-are-the-compute-environments-supported-by-data-factory"></a>Data Factory tarafından desteklenen işlem ortamlarının nelerdir?
Aşağıdaki tabloda Data Factory ve bunlar üzerinde çalışan etkinlikler tarafından desteklenen işlem ortamlarının listesi sağlar.

| İşlem ortamı | etkinlikler |
| --- | --- |
| [İsteğe bağlı HDInsight kümesi](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) veya [kendi HDInsight kümenizi](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) |[DotNet](data-factory-use-custom-activities.md), [Hive](data-factory-hive-activity.md), [Pig](data-factory-pig-activity.md), [MapReduce](data-factory-map-reduce.md), [Hadoop akış](data-factory-hadoop-streaming-activity.md) |
| [Azure Batch](data-factory-compute-linked-services.md#azure-batch-linked-service) |[DotNet](data-factory-use-custom-activities.md) |
| [Azure Machine Learning](data-factory-compute-linked-services.md#azure-machine-learning-linked-service) |[Machine Learning etkinlikleri: Toplu yürütme ve kaynak güncelleştirme](data-factory-azure-ml-batch-execution-activity.md) |
| [Azure Data Lake Analytics'i](data-factory-compute-linked-services.md#azure-data-lake-analytics-linked-service) |[Data Lake Analytics U-SQL](data-factory-usql-activity.md) |
| [Azure SQL](data-factory-compute-linked-services.md#azure-sql-linked-service), [Azure SQL veri ambarı](data-factory-compute-linked-services.md#azure-sql-data-warehouse-linked-service), [SQL Server](data-factory-compute-linked-services.md#sql-server-linked-service) |[Saklı Yordam](data-factory-stored-proc-activity.md) |

### <a name="how-does-azure-data-factory-compare-with-sql-server-integration-services-ssis"></a>Nasıl Azure Data Factory, SQL Server Integration Services (SSIS) arasındaki fark nedir? 
Bkz: [Azure Data factory'yi vs. SSIS](https://www.sqlbits.com/Sessions/Event15/Azure_Data_Factory_vs_SSIS) sunudan bizim Professionals (en değerli uzmanları) biri: Reza Rad. Son değişiklikleri Data factory'de bazıları PowerPoint'te slayt Destesi listelenmeyebilir. Daha fazla özellik sürekli olarak Azure Data Factory'ye ekliyorsunuz. Daha fazla özellik sürekli olarak Azure Data Factory'ye ekliyorsunuz. Biz bu güncelleştirmeler Microsoft gelen veri tümleştirme teknolojileri karşılaştırması içine süre bu yıl birleştirecektir.   

## <a name="activities---faq"></a>Etkinlikleri - SSS
### <a name="what-are-the-different-types-of-activities-you-can-use-in-a-data-factory-pipeline"></a>Bir Data Factory işlem hattında kullanabileceğiniz etkinlikler farklı türleri nelerdir?
* [Veri taşıma etkinlikleri](data-factory-data-movement-activities.md) verileri taşımak için.
* [Veri dönüştürme etkinlikleri](data-factory-data-transformation-activities.md) işlem/veri dönüştürme için.

### <a name="when-does-an-activity-run"></a>Bir etkinlik çalıştırıldığında?
**Kullanılabilirlik** yapılandırma ayarı çıktı veri tablosundaki etkinlik çalıştırıldığında belirler. Etkinlik giriş veri kümeleri belirtilirse, tüm giriş verilerini bağımlılıkların olup olmadığını denetler (diğer bir deyişle, **hazır** durumu) çalışmaya başlamadan önce.

## <a name="copy-activity---faq"></a>Kopyalama etkinliği - SSS
### <a name="is-it-better-to-have-a-pipeline-with-multiple-activities-or-a-separate-pipeline-for-each-activity"></a>Birden çok etkinliği ile işlem hattı veya etkinlikleri için ayrı bir işlem hattı için daha iyi mi?
İşlem hatları ilgili etkinlikleri paket kaydetmeleri beklenir. Bağlantı veri kümeleri işlem hattı dışında başka herhangi bir etkinliği tarafından tüketilen değil, tek işlem hattında etkinlikleri tutabilirsiniz. Böylece birbirleri ile hizalamak bu şekilde, zincir işlem hattı etkin nokta ihtiyaç duymaz. Ayrıca, işlem hattı güncelleştirirken tablolarda işlem hattının iç veri bütünlüğü daha iyi korunur. İşlem hattı güncelleştirme temelde işlem hattındaki tüm etkinlikleri durdurur, bunları kaldırır ve yeniden oluşturur. Perspektif geliştirme, bu da ilgili etkinlikler, işlem hattı için bir JSON dosyası içinde veri akışını görmek daha kolay olabilir.

### <a name="what-are-the-supported-data-stores"></a>Desteklenen veri depoları nelerdir?
Data Factory’deki Kopyalama Etkinliği bir kaynak veri deposundan havuz veri deposuna verileri kopyalar. Data Factory aşağıdaki veri depolarını destekler. Herhangi bir kaynaktan gelen veriler herhangi bir havuza yazılabilir. Bir depoya veya depodan veri kopyalama hakkında bilgi edinmek için veri deposuna tıklayın.

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> \* taşıyan veri depoları şirket içi veya Azure IaaS üzerinde olabilir ve bir şirket içi/Azure IaaS makinesine [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md) yüklemenizi gerektirir.

### <a name="what-are-the-supported-file-formats"></a>Desteklenen dosya biçimleri nelerdir?
[!INCLUDE [data-factory-file-format](../../../includes/data-factory-file-format.md)]

### <a name="where-is-the-copy-operation-performed"></a>Kopyalama işleminin gerçekleştirildiği?
Bkz: [küresel olarak kullanılabilir veri taşıma](data-factory-data-movement-activities.md#global) ayrıntıları bölümü. Kısacası, bir şirket içi veri deposuna söz konusu olduğunda, veri yönetimi ağ geçidi, şirket içi ortamınızda kopyalama işlemi gerçekleştirilir. Ayrıca, iki bulut deposu arasında veri taşıma olduğunda, aynı coğrafyadaki havuz konumu en yakın bölgede kopyalama işlemi gerçekleştirilir.

## <a name="hdinsight-activity---faq"></a>HDInsight etkinliği - SSS
### <a name="what-regions-are-supported-by-hdinsight"></a>HDInsight tarafından hangi bölgeler desteklenir?
Coğrafi kullanılabilirlik bölümüne şu makaleye bakın: veya [HDInsight fiyatlandırma ayrıntıları][hdinsight-supported-regions].

### <a name="what-region-is-used-by-an-on-demand-hdinsight-cluster"></a>Hangi bölge, bir isteğe bağlı HDInsight kümesi tarafından kullanılır?
İsteğe bağlı HDInsight kümesi kümeyle birlikte kullanılmak üzere belirtilen depolama bulunduğu bölgede oluşturulur.    

### <a name="how-to-associate-additional-storage-accounts-to-your-hdinsight-cluster"></a>Ek depolama hesapları HDInsight kümenize ilişkilendirmek nasıl?
Kendi HDInsight kümenizi (BYOC - bilgisayarınızı kendi küme Getir) kullanıyorsanız, aşağıdaki konulara bakın:

* [Alternatif depolama hesapları ve meta Depolarla HDInsight kümesi kullanma][hdinsight-alternate-storage]
* [Ek depolama hesapları HDInsight Hive'ı kullanın.][hdinsight-alternate-storage-2]

Data Factory hizmeti tarafından oluşturulan bir talep üzerine küme kullanıyorsanız, Data Factory hizmetinin sizin adınıza kaydedebilmesi ek depolama hesapları için HDInsight bağlı hizmeti belirtin. İsteğe bağlı hizmet JSON tanımında **additionalLinkedServiceNames** özelliğini aşağıdaki JSON kod parçacığında gösterildiği gibi diğer depolama hesaplarını belirtin:

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
Yukarıdaki örnekte, bağlı hizmetler, tanımları HDInsight küme alternatif depolama hesapları erişmesi gereken kimlik bilgilerini içeren otherLinkedServiceName1 ve otherLinkedServiceName2 temsil eder.

## <a name="slices---faq"></a>Dilimler - SSS
### <a name="why-are-my-input-slices-not-in-ready-state"></a>Neden benim girdi dilimi hazır durumda değil misiniz?
Sıkça ayarlamıyor **dış** özelliğini **true** girdi verileri data factory (data factory tarafından üretilen değil) dışında olduğunda giriş veri kümesi üzerinde.

Aşağıdaki örnekte, yalnızca ayarlamanız gerekir **dış** üzerinde true **dataset1**.  

**DataFactory1** 1 işlem hattı: dataset1 activity1 -> -> dataset2 -> activity2 dataset3 işlem hattı 2 ->: dataset3 -> activity3 ancak dataset4 ->

Başka bir veri fabrikası dataset4 (2 1 data factory'de işlem hattı tarafından üretilen) alan bir işlem hattıyla varsa, veri kümesi farklı bir veri fabrikası (DataFactory1, DataFactory2 değil) tarafından oluşturulduğundan dataset4 dış bir veri kümesi olarak işaretleyin.  

**DataFactory2**    
İşlem hattı 1: dataset4 -> activity4 dataset5 ->

Dış özelliği düzgün şekilde ayarlarsanız, giriş verilerini giriş veri kümesi tanımında belirtilen konumda var olup olmadığını doğrulayın.

### <a name="how-to-run-a-slice-at-another-time-than-midnight-when-the-slice-is-being-produced-daily"></a>Nasıl daha gece yarısı zaman dilimi günlük üretilmekte olan başka bir zaman dilimi çalıştırılır?
Kullanım **uzaklığı** özelliği üretilecek dilim istediğiniz süreyi belirtin. Bkz: [veri kümesinin kullanılabilirliğine](data-factory-create-datasets.md#dataset-availability) bölümü bu özellik hakkında ayrıntılı bilgi için. Hızlı bir örnek aşağıda verilmiştir:

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
Günlük dilimleri başlar **6 AM** varsayılan gece yarısı yerine.     

### <a name="how-can-i-rerun-a-slice"></a>Bir dilim nasıl yeniden çalıştırabilir miyim?
Bir dilim aşağıdaki yollardan birinde çalıştırabilirsiniz:

* Bir etkinlik penceresi veya dilimi yeniden çalıştırmak için izleme ve yönetme uygulaması'nı kullanın. Bkz: [seçilen yeniden çalıştır etkinliği windows](data-factory-monitor-manage-app.md#perform-batch-actions) yönergeler için.   
* Tıklayın **çalıştırma** komut çubuğunda **veri DİLİMİ** dilimin Azure portalındaki dikey.
* Çalıştırma **kümesi AzDataFactorySliceStatus** durumu cmdlet'iyle kümesine **bekleniyor** dilim için.   

    ```powershell
    Set-AzDataFactorySliceStatus -Status Waiting -ResourceGroupName $ResourceGroup -DataFactoryName $df -TableName $table -StartDateTime "02/26/2015 19:00:00" -EndDateTime "02/26/2015 20:00:00"
    ```
  Bkz: [kümesi AzDataFactorySliceStatus] [ set-azure-datafactory-slice-status] cmdlet'i hakkında daha fazla ayrıntı için.

### <a name="how-long-did-it-take-to-process-a-slice"></a>Ne kadar bir dilimi işlemesi için sürer?
Veri dilimi işlemesi için ne kadar sürdüğünü öğrenmek için etkinlik penceresi Gezgini'nde izleme ve yönetme uygulamasını kullanın. Bkz: [etkinlik penceresi Gezgini](data-factory-monitor-manage-app.md#activity-window-explorer) Ayrıntılar için.

Ayrıca Azure portalında aşağıdakileri yapabilirsiniz:  

1. Tıklayın **veri kümeleri** kutucuğundan **DATA FACTORY** veri fabrikanızın dikey.
2. Belirli bir veri kümesi tıklayarak **veri kümeleri** dikey penceresi.
3. Gelen ilginizi çeken dilimi seçin **son dilimler** listesini **tablo** dikey penceresi.
4. Çalıştırılan etkinlik tıklayın **etkinlik çalıştırmalarını** listesini **veri DİLİMİ** dikey penceresi.
5. Tıklayın **özellikleri** kutucuğundan **etkinlik çalışma ayrıntıları** dikey penceresi.
6. Görmelisiniz **süresi** alan değerine sahip. Bu değer dilimi işlemesi için geçen süredir.   

### <a name="how-to-stop-a-running-slice"></a>Çalışan bir dilim durdurmak nasıl?
İşlem hattının yürütülmesini durdurmak gerekiyorsa, kullanabileceğiniz [Suspend-AzDataFactoryPipeline](/powershell/module/az.datafactory/suspend-azdatafactorypipeline) cmdlet'i. Şu anda, işlem hattı askıya sürmekte olan dilim yürütme durdurulmaz. Devam eden yürütmeler tamamladıktan sonra ek dilim çekilir.

Gerçekten tüm yürütmeleri hemen durdurmak istiyorsanız, tek yolu, işlem hattını silip yeniden oluşturmanız olacaktır. İşlem hattı silmeyi seçerseniz, tablolar ve işlem hattı tarafından kullanılan bağlı hizmet silme gerekmez.

[create-factory-using-dotnet-sdk]: data-factory-create-data-factories-programmatically.md
[msdn-class-library-reference]: /dotnet/api/microsoft.azure.management.datafactories.models
[msdn-rest-api-reference]: /rest/api/datafactory/

[adf-powershell-reference]: /powershell/module/az.datafactory/
[azure-portal]: https://portal.azure.com
[set-azure-datafactory-slice-status]: /powershell/module/az.datafactory/set-Azdatafactoryslicestatus

[adf-pricing-details]: https://go.microsoft.com/fwlink/?LinkId=517777
[hdinsight-supported-regions]: https://azure.microsoft.com/pricing/details/hdinsight/
[hdinsight-alternate-storage]: https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx
[hdinsight-alternate-storage-2]: https://blogs.msdn.com/b/cindygross/archive/2014/05/05/use-additional-storage-accounts-with-hdinsight-hive.aspx
