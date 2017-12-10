---
title: "Azure Data Factory sürüm 1 ve 2’nin karşılaştırması | Microsoft Docs"
description: "Bu makalede, Azure Data Factory’nin 1 ve 2 numaralı sürümleri karşılaştırılır."
services: data-factory
documentationcenter: 
author: kromerm
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/20/2017
ms.author: makromer
ms.openlocfilehash: f55348e9974de17c6d7e704e185f1ea9b21ff66e
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="compare-azure-data-factory-versions-1-and-2"></a>Azure Data Factory sürüm 1 ve 2’nin karşılaştırması
Bu makalede, Azure Data Factory’nin 1 (V1) ve 2 (V2) numaralı sürümleri karşılaştırılır. Hizmetin V1 sürümüne giriş için bkz. [Data Factory sürüm 1 - giriş](v1/data-factory-introduction.md) ve V2 sürümü için bkz. [Data Factory sürüm 2 - giriş](introduction.md).

## <a name="feature-comparison"></a>Özellik karşılaştırması
Aşağıdaki tabloda, V1 ve V2 sürümlerinin özellikleri karşılaştırılır. 

| Özellik | V1 | V2 | 
| ------- | --------- | --------- | 
| Veri kümeleri | Etkinliklerinizde girdi ve çıktı olarak kullanmak istediğiniz verilere başvuran verilerin adlandırılmış bir görünümüdür. Veri kümeleri tablolar, dosyalar, klasörler ve belgeler gibi farklı veri depolarındaki verileri tanımlar. Örneğin Azure Blob veri kümesi, etkinliğin verileri okuması için gereken blob kapsayıcısını ve Blob depolama klasörünü belirtir.<br/><br/>**Kullanılabilirlik**, veri kümesi (saatlik, günlük gibi) için model dilimleme işleme penceresini tanımlar. | Veri kümeleri, V2 içinde aynıdır. Ancak, veri kümeleri için **kullanılabilirlik** zaman çizelgelerini tanımlamanız gerekmez. İşlem hatlarını bir zaman çizelgesi oluşturma paradigmasından zamanlayabilen bir tetikleyici kaynak belirleyebilirsiniz. Daha fazla bilgi için [Tetikleyiciler](concepts-pipeline-execution-triggers.md#triggers) ve [Veri Kümeleri](concepts-datasets-linked-services.md) bölümlerine bakın. | 
| Bağlı hizmetler | Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. | Data Factory V1 ile aynı, ancak Data Factory V2 Integration Runtime bilgi işlem ortamından yararlanmak için yeni bir **connectVia** özelliği de bulunur. Daha fazla bilgi için bkz. [Tümleştirme çalışma zamanları](concepts-integration-runtime.md) ve [Azure Blob Depolama için bağlı hizmeti özellikleri](connector-azure-blob-storage.md#linked-service-properties). |
| İşlem hatları | Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. İşlem hattı, bir araya geldiğinde bir görev gerçekleştiren mantıksal etkinlik grubudur. İşlem hatlarını zamanlamak ve çalıştırmak için startTime, endTime, isPaused kullandınız. | İşlem hatları, verilerde uygulanacak etkinlik gruplarıdır. Ancak işlem hattındaki etkinliklerin zamanlanması, tetikleyici kaynaklarına ayrılmıştır. Data Factory V2 üzerindeki işlem hatlarını, tetikleyiciler aracılığıyla ayrı olarak zamanladığınız “iş akışı birimleri” olarak düşünebilirsiniz. <br/><br/>Data Factory V2’de, işlem hatlarının yürütüleceği zaman “pencereleri” yoktur. Data Factory V1’in startTime, endTime, ve isPaused kavramları, Data Factory V2’de bulunmaz. Daha fazla bilgi bkz. [İşlem hattı yürütme ve tetikleyicileri](concepts-pipeline-execution-triggers.md) ve [İşlem hatları ve etkinlikler](concepts-pipelines-activities.md). |
| Etkinlikler | Etkinlikler, bir işlem hattındaki verilerinizde gerçekleştirilecek eylemleri tanımlar. Veri taşıma (kopyalama etkinliği) ve veri dönüştürme etkinlikleri (Hive, Pig ve MapReduce gibi) desteklenir. | Data Factory V2'de de etkinlikler, işlem hattı içindeki tanımlanmış eylemlerdir. V2’de yeni [denetim akışı etkinlikleri](concepts-pipelines-activities.md#control-activities) sunulur. Bu etkinlikleri denetim akışında (döngü ve dal oluşturma) kullanırsınız. V1’de desteklenen veri taşıma ve veri dönüştürmek etkinlikleri, V2’de de desteklenir. V2’de dönüşüm etkinliklerini veri kümelerini kullanmadan da tanımlayabilirsiniz. |
| Karma veri taşıma ve etkinlik dağıtma | Şirket içi ve bulut arasında veri taşıma desteği (eski adıyla [Veri Yönetimi Ağ Geçidi](v1/data-factory-data-management-gateway.md)). Yeni adıyla Integration Runtime.| Veri Yönetimi Ağ Geçidi, artık Şirket İçinde Barındırılan Integration Runtime olarak adlandırılır. V1’deki özelliklerin aynısını sunar. <br/><br/> V2’deki Azure-SSIS Integration Runtime, bulutta SQL Server Integration Services (SSIS) dağıtımını ve çalıştırılmasını destekler. Ayrıntılar için [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) makalesine bakın.|
| Parametreler | NA | Parametreler, işlem hatlarında tanımlanan salt okunur yapılandırma ayarlarının anahtar-değer çiftleridir. İşlem hattını el ile yürütürken, bağımsız değişkenleri parametrelere geçirebilirsiniz. Bir zamanlayıcı tetikleyicisi kullanıyorsanız, tetikleyici de parametrelere ilişkin değerleri geçirebilir. İşlem hattındaki etkinlikler parametre değerlerini kullanır.  |
| İfadeler | Data Factory V1, veri seçimi sorgularında ve etkinlik/veri kümesi özelliklerinde işlevleri ve sistem değişkenlerini kullanmanızı sağlar. | Data Factory V2’de, ifadeleri JSON dizesi değerinin tüm kısımlarında kullanabilirsiniz. Daha fazla bilgi için bkz. [V2’de İfadeler ve İşlevler](control-flow-expression-language-functions.md).|
| İşlem hattı çalıştırmaları | NA | İşlem hattı yürütmesinin tek örneği. Örneğin, 08:00, 09:00 ve 10:00'da çalışan bir işlem hattınız olduğunu kabul edelim. Bu durumda işlem hattının üç ayrı çalıştırması (işlem hattı çalıştırması) olacaktır. Her işlem hattı çalıştırmasında belirli işlem hattı çalıştırmasını benzersiz bir şekilde tanımlayan GUID olan benzersiz bir işlem hattı çalıştırması kimliği vardır. İşlem hattı çalıştırmaları örneği genelde bağımsız değişkenlerin işlem hatlarında tanımlanan parametrelere iletilmesiyle oluşturulur. |
| Etkinlik Çalıştırmaları | NA | Bir işlem hattı içinde bir etkinlik yürütmesi örneği. | 
| Tetikleyici çalıştırmaları | NA | Bir tetikleyici yürütmesi örneği. Daha fazla bilgi için bkz. [Tetikleyiciler](concepts-pipeline-execution-triggers.md). |
| Zamanlama | Zamanlama, işlem hattının başlangıç/bitiş zamanlarına ve veri kümesinin kullanılabilirliğine dayanır. | Zamanlayıcı tetikleyicisi veya dış zamanlayıcı aracılığıyla yürütme. Daha fazla bilgi için bkz. [İşlem hattı yürütme ve tetikleyiciler](concepts-pipeline-execution-triggers.md). |

Aşağıdaki bölümlerde sürüm 2’nin özellikleri hakkında daha fazla bilgi sağlanır: 

## <a name="control-flow"></a>Denetim akışı  
Data Factory V2, modern veri ambarında çeşitli tümleştirme akışlarını ve desenlerini desteklemek için artık zaman dizisi verilerine bağlı olmayan yeni bir esnek veri işlem hattı modelini etkinleştirmiştir. Daha önce mümkün olmayıp şu anda etkin olan birkaç yaygın akış şunlardır:   

### <a name="chaining-activities"></a>Zincirleme etkinlikleri
Sürüm 1’de, etkinlikleri zincirlemek için bir etkinliğin çıktısını, başka bir etkinliğin girdisi olarak yapılandırmanız gerekiyordu. V2’de ise bir işlem hattı içindeki sıralı etkinlikleri zincirleyebilirsiniz. Bir etkinliği yukarı akış etkinliğiyle zincirlemek için etkinliğin içindeki **dependsOn** özelliğini kullanabilirsiniz. Daha fazla bilgi almak ve örnekleri incelemek için [İşlem hatları ve etkinlikler](concepts-pipelines-activities.md#multiple-activities-in-a-pipeline) ve [Dal oluşturma ve zincirleme etkinlikleri](tutorial-control-flow.md) makalelerine bakın. 

### <a name="branching-activities"></a>Dal oluşturma etkinlikleri
V2’de artık bir işlem hattı içindeki etkinlikleri dallara ayırabilirsiniz. [If-condition etkinliği](control-flow-if-condition-activity.md), programlama dillerindeki `if` deyimiyle aynı işlevselliği sağlar. Koşul `true` sonucunu verdiğinde bir dizi etkinliği, `false` sonucu verdiğinde ise başka bir dizi etkinliği değerlendirmeye alır. Dal oluşturma etkinliklerinin bir örneğini görmek için [Dal oluşturma ve zincirleme etkinlikleri](tutorial-control-flow.md) öğreticisine bakın.

### <a name="parameters"></a>Parametreler 
İşlem hattı düzeyinde parametre tanımlayabilir ve işlem hattı talep üzerine ya da bir tetikleyici ile çağrılırken bağımsız değişkenler geçirebilirsiniz. Etkinlikler işlem hattına geçirilen bağımsız değişkenleri kullanabilir. Daha fazla bilgi için bkz. [İşlem hatları ve tetikleyiciler](concepts-pipeline-execution-triggers.md). 

### <a name="custom-state-passing"></a>Özel durum geçirme
Durum da dahil olmak üzere etkinlik çıktıları, işlem hattının sonraki bir etkinliği tarafından kullanılabilir. Örneğin, bir etkinliğin JSON tanımlamasında, aşağıdaki söz dizimini kullanarak bir önceki etkinliğin çıktısına erişebilirsiniz: `@activity('NameofPreviousActivity').output.value`. Bu özellik, değerlerin etkinliklerden geçebileceği iş akışları oluşturmanıza imkan tanır.

### <a name="looping-containers"></a>Döngü kapsayıcıları
[ForEach etkinliği](control-flow-for-each-activity.md), işlem hattınızda yinelenen bir denetim akışını tanımlar. Bu etkinlik bir koleksiyon üzerinde yinelemek için kullanılır ve bir döngüde belirtilen etkinlikleri yürütür. Bu etkinliğin döngü uygulaması, programlama dillerindeki Foreach döngü yapısına benzer. 

[Until](control-flow-until-activity.md) etkinliği, programlama dillerindeki do-until döngü yapısıyla aynı işlevselliği sağlar. Etkinlikle ilişkilendirilmiş olan koşul doğru sonucunu verene kadar bir dizi etkinliği döngüsel olarak yürütür. Data Factory'de bitiş etkinliği için bir zaman aşımı değeri belirtebilirsiniz.  

### <a name="trigger-based-flows"></a>Tetikleyici temelli akışlar
İşlem hatları talep üzerine veya duvar saati zamanlamasına göre tetiklenebilir. [İşlem hatları ve tetikleyiciler](concepts-pipeline-execution-triggers.md) makalesi, tetikleyiciler hakkında ayrıntılı bilgiler sunar. 

### <a name="invoke-a-pipeline-from-another-pipeline"></a>Bir işlem hattını başka bir işlem hattından çağırma
[İşlem Hattı Yürütme etkinliği](control-flow-execute-pipeline-activity.md) bir Data Factory işlem hattının başka bir işlem hattını çağırmasını sağlar.

### <a name="delta-flows"></a>Delta akışlar
ETL düzenlerindeki bir anahtar kullanım örneği, “değişiklik yüklemeleri” veya yalnızca bir işlem hattının son yinelenmesinden itibaren değiştirilen verileri yüklemedir. Sürüm 2’deki [arama etkinliği](control-flow-lookup-activity.md), esnek zamanlama ve denetim akışı gibi yeni özellikler, bu kullanım örneğini doğal bir biçimde sunar. Adım adım yönergeler içeren bir öğretici için bkz. [Öğretici: Artımlı kopya](tutorial-incremental-copy-powershell.md).

### <a name="other-control-flow-activities"></a>Diğer denetim akışı etkinlikleri
Data Factory V2 tarafından desteklenen denetim akışı etkinliklerinin bazıları şunlardır: 

Denetim etkinliği | Açıklama
---------------- | -----------
[ForEachActivity](control-flow-for-each-activity.md) | ForEach Etkinliği, işlem hattınızda yinelenen bir denetim akışını tanımlar. Bu etkinlik bir koleksiyon üzerinde yinelemek için kullanılır ve bir döngüde belirtilen etkinlikleri yürütür. Bu etkinliğin döngü uygulaması, programlama dillerindeki Foreach döngü yapısına benzer.
[WebActivity](control-flow-web-activity.md) | Web Etkinliği bir Data Factory işlem hattından özel bir REST uç noktasını çağırmak için kullanılabilir. Etkinlik tarafından kullanılacak ve erişilecek veri kümelerini ve bağlı hizmetleri geçirebilirsiniz. 
[Arama Etkinliği](control-flow-lookup-activity.md) | Arama Etkinliği herhangi bir dış kaynaktan bir record/ table name/ değerini okumak veya aramak için kullanılabilir. Sonraki etkinliklerde bu çıktıya daha fazla başvurulabilir. 
[Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md) | GetMetadata etkinliği, Azure Data Factory içindeki herhangi bir verinin meta verilerini almak için kullanılabilir. 
[Bekleme Etkinliği](control-flow-wait-activity.md) | İşlem hattı (duraklatılır), belirtilen süre boyunca bekler.

## <a name="deploy-ssis-packages-to-azure"></a>SSIS paketlerini Azure'a dağıtma 
SQL Server Integration Services (SSIS) iş yüklerinizi buluta taşımak istiyorsanız, Data Factory V2 oluşturabilir ve bir Azure SSIS Integration Runtime (IR) sağlayabilirsiniz. Azure-SSIS IR, bulutta SSIS paketlerinizi çalıştırmaya ayrılmış Azure VM'lerin (düğümler) tam yönetilen bir kümesidir. Azure-SSIS IR sağladıktan sonra, SSIS paketlerini şirket içi SSIS ortamına dağıtmak için kullandığınız araçları kullanabilirsiniz. Örneğin, SQL Server Veri Araçları (SSDT) veya SQL Server Management Studio’yu (SSMS) kullanarak Azure’da bu çalışma zamanına SSIS paketleri dağıtabilirsiniz. Adım adım yönergeler için bkz. [SQL Server Integration Services paketlerini Azure'a dağıtma](tutorial-deploy-ssis-packages-azure.md). 

## <a name="flexible-scheduling"></a>Esnek zamanlama
Data Factory V2’de veri kümesi kullanılabilirlik zaman çizelgelerini tanımlamanız gerekmez. İşlem hatlarını bir zaman çizelgesi oluşturma paradigmasından zamanlayabilen bir tetikleyici kaynak belirleyebilirsiniz. Esnek bir zamanlama / yürütme modeli için, parametreleri tetikleyiciden işlem hatlarına geçirebilirsiniz. Data Factory V2’de, işlem hatlarının yürütüleceği zaman “pencereleri” yoktur. Data Factory V1’in startTime, endTime, ve isPaused kavramları, Data Factory V2’de bulunmaz. Data Factory V2’de işlem hattı oluşturma ve ardından bunu zamanlama: [İşlem hattı yürütme ve tetikleyiciler](concepts-pipeline-execution-triggers.md).

## <a name="support-for-more-data-stores"></a>Daha fazla veri deposu desteği
V2’de V1’e kıyasla daha fazla veri deposuna ve daha fazla veri deposundan veri kopyalanabilir. Desteklenen veri depoları listesi için aşağıdaki makalelere bakın:

- [V1 - Desteklenen veri depoları](v1/data-factory-data-movement-activities.md#supported-data-stores-and-formats)
- [V2 - Desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats)

## <a name="support-for-on-demand-spark-cluster"></a>İsteğe bağlı Spark kümesi desteği
V2, isteğe bağlı HDInsight Spark kümesi oluşturulmasını destekler. İsteğe bağlı bir Spark kümesi oluşturmak için, steğe bağlı HDInsight bağlı servis tanımınızdaki küme türünü Spark olarak belirtin. Ardından, işlem hattınızdaki Spark etkinliğini bu bağlı hizmeti kullanacak biçimde yapılandırabilirsiniz. Çalışma zamanında, etkinlik yürütüldüğünde, Data Factory hizmeti otomatik olarak Spark kümesini oluşturur. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [V2’de Spark Etkinliği](transform-data-using-spark.md)
- [İsteğe bağlı Azure HDInsight bağlı hizmeti](compute-linked-services.md#azure-hdinsight-on-demand-linked-service)

## <a name="custom-activities"></a>Özel etkinlikler
V1’de, IDotNetActivity arabiriminin Execute yöntemini uygulayan bir sınıfa sahip olan bir .Net Class Library projesi oluşturarak (Özel) DotNet Etkinliği kodu uygularsınız. Bu nedenle özel kodunuzun .Net Framework 4.5.2 kapsamında yazılması ve Windows tabanlı Azure Batch Havuzu düğümlerinde yürütülmesi gerekir. 

V2 Özel Etkinlik’te .Net arabirimi uygulamanız gerekmez. Artık doğrudan komutlarla betikler çalıştırabilir ve yürütülmeye uygun olan kendi özel kodunuzu çalıştırabilirsiniz. 

Daha fazla bilgi için bkz. [V1 ve V2’de özel etkinlikler arasındaki farklar](transform-data-using-dotnet-custom-activity.md#difference-between-custom-activity-in-azure-data-factory-v2-and-custom-dotnet-activity-in-azure-data-factory-v1).

## <a name="sdks"></a>SDK’lar
Data Factory Sürüm 2, işlem hatlarını yazmak, yönetmek ve izlemek için kullanılabilen daha geniş SDK seçenekleri sunar.

- **.NET SDK**: .NET SDK, V2 için güncelleştirilmiştir. 
- **PowerShell**: PowerShell cmdlet'leri V2 için güncelleştirilmiştir. V2 cmdlet'lerinin adında **V2** vardır. Örneğin: Get-AzureRmDataFactoryV2. 
- **Python SDK**: Bu SDK V2 içindir.
- **REST API**: REST API, V2 için güncelleştirilmiştir. 

V2 için güncelleştirilmiş olan SDK'lar sürüm 1 istemcileriyle uyumlu değildir. 

## <a name="authoring-experience"></a>Yazma deneyimi
Data Factory V1, Azure Portal'da Data Factory Düzenleyicisi’ni kullanarak işlem hatları yazmanızı sağlar. Şu anda Data Factory V2, yalnızca programlama yoluyla (.NET SDK, REST API, PowerShell, Python vb.) veri fabrikası oluşturmayı destekler. Henüz kullanıcı arabirimi desteği sunulmamaktadır.  Data Factory V1; SDK, REST ve PowerShell ile yazma desteği de sağlar.

## <a name="monitoring-experience"></a>İzleme deneyimi
V2'de, [Azure İzleyici](monitor-using-azure-monitor.md)’yi kullanarak veri fabrikalarını izleyebilirsiniz. Yeni PowerShell cmdlet’leri, [tümleştirme çalışma zamanlarını](monitor-integration-runtime.md) izlemeyi destekler. V1 ve V2, Azure portalından başlatılabilen izleme uygulaması aracılığıyla görsel izleme desteği sunar.


## <a name="next-steps"></a>Sonraki adımlar
Veri fabrikası oluşturma hakkında bilgi edinmek için şu Hızlı Başlangıçlarda verilen adım adım yönergeleri izleyin: [PowerShell](quickstart-create-data-factory-powershell.md), [.NET](quickstart-create-data-factory-dot-net.md), [Python](quickstart-create-data-factory-python.md), [REST API](quickstart-create-data-factory-rest-api.md). 
