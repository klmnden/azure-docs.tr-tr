---
title: 'Azure Data Factory: Sık sorulan sorular | Microsoft Docs'
description: Azure Data Factory hakkında sık sorulan soruların yanıtlarını alın.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.assetid: 532dec5a-7261-4770-8f54-bfe527918058
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: shlo
ms.openlocfilehash: d704c32ee7417c6460ad6cc880e451adddfa61de
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61345763"
---
# <a name="azure-data-factory-faq"></a>Azure veri fabrikası ile ilgili SSS
Bu makalede Azure Data Factory hakkında sık sorulan soruların yanıtlarını sağlar.  

## <a name="what-is-azure-data-factory"></a>Azure Data Factory nedir? 
Veri Fabrikası taşımayı ve dönüştürmeyi otomatikleştiren bir tam olarak yönetilen, bulut tabanlı veri tümleştirme hizmetidir. Ham madde tamamlanmış ürünlere dönüştürmek için donanım çalıştıran bir Fabrika gibi Azure Data Factory, ham verileri toplayan ve kullanıma hazır bilgilere dönüştürün var olan hizmetleri düzenler. 

Azure Data Factory kullanarak verileri şirket içi ile bulut arasında taşımak için veri odaklı iş akışları oluşturabilirsiniz veri depoları. İşleme ve Azure HDInsight, Azure Data Lake Analytics ve SQL Server Integration Services (SSIS) Integration runtime gibi hizmetleri kullanarak verileri dönüştürme işlem. 

Data Factory ile veri işleme, Azure tabanlı bir bulut hizmeti veya kendi şirket içinde barındırılan işlem ortamı SSIS, SQL Server veya Oracle gibi yürütebilir. Gereksinim duyduğunuz eylem gerçekleştiren bir işlem hattı oluşturduktan sonra düzenli aralıklarla (saatlik, günlük veya haftalık örneğin), zaman penceresi zamanlama çalıştırmak üzere ya da bir olayın gerçekleşmesi işlem hattını tetikler. Daha fazla bilgi için bkz. [Azure Data Factory'ye giriş](introduction.md).

### <a name="control-flows-and-scale"></a>Denetim akışı ve ölçeklendirme 
Data Factory sağlar modern veri ambarında çeşitli tümleştirme akışlarını ve desenlerini desteklemek için modelleme esnek veri işlem hattı. Bu, veri işlem hatlarına ve bu akışların içinde ve arasında parametreleri açıkça geçirebilirsiniz olanağı dallanma, koşullu yürütme içeren paradigmalarını programlama tam denetim akışı kapsar. Denetim akışı dış yürütme motorları ve veri akışı özellikleri, uygun ölçekte, kopyalama etkinliği ile verileri taşıma dahil olmak üzere etkinlik dağıtma verilerine dönüştürme de kapsar.

Veri fabrikası, veri tümleştirmesi için gerekli ve isteğe bağlı veya bir zamanlamaya göre tekrarlanarak gönderilen bir akış stilini modelleme özgürlüğüne sağlar. Bu model sağlayan birkaç yaygın akış şunlardır:   

- Denetim akışı:
    - Etkinlikler, bir işlem hattı içindeki sıralı birbirine zincirlenebilir.
    - Bir işlem hattı etkinlikleri dal.
    - Parametreler:
        - Parametreler işlem hattı düzeyinde tanımlanabilir ve işlem hattı talep üzerine ya da bir tetikleyiciden çağırma sırasında bağımsız değişkenler geçirilebilir.
        - Etkinlikler işlem hattına geçirilen bağımsız değişkenleri kullanabilir.
    - Özel durum geçirme:
        - Durumu dahil olmak üzere etkinlik çıktıları, işlem hattının sonraki bir etkinliği tarafından tüketilebilir.
    - Döngü kapsayıcıları:
        - Foreach etkinliği, etkinliklerin bir döngüde belirtilen bir koleksiyon üzerinden yineleme. 
- Tetikleyici temelli akışlar:
    - İşlem hatları talep üzerine veya duvar saati zamanı tarafından tetiklenebilir.
- Delta akışlar:
    - Parametreler, bir ilişkisel mağazadan şirket içinde veya bulutta verileri göle yüklemek için boyut ya da başvuru tablolarını taşırken, yüksek su işareti delta kopya için tanımlamak için kullanılabilir. 

Daha fazla bilgi için [Öğreticisi: Denetim Akışları](tutorial-control-flow.md).

### <a name="data-transformed-at-scale-with-code-free-pipelines"></a>Dönüştürülen uygun ölçekte kod gerektirmeyen işlem hatları ile veri
Yeni bir tarayıcı tabanlı bir araç deneyimi, kod gerektirmeyen bir işlem hattı yazma ve dağıtım ile bir modern, etkileşimli web tabanlı deneyim sağlar.

Görsel verileri geliştiriciler ve veri mühendisleri, Data Factory web kullanıcı Arabirimi komut zincirleri oluşturmak için kullanacağınız kod gerektirmeyen bir tasarım ortamıdır. Visual Studio Online Git ile tamamen tümleşiktir ve CI/CD tümleştirmesi ve hata ayıklama seçeneği ile yinelemeli geliştirme sağlar.

### <a name="rich-cross-platform-sdks-for-advanced-users"></a>İleri düzey kullanıcılar için zengin çoklu platform SDK'ları
Data Factory V2 sağlayan zengin bir SDK'ları yazmak, yönetmek ve sık kullandığınız IDE'yi kullanarak işlem hatlarını izlemek için kullanılan dahil olmak üzere:
* Python SDK'sı
* PowerShell CLI
* C# SDK’sı

Kullanıcılar, Data Factory V2 ile arabirim oluşturmak için belgelenmiş REST API de kullanabilirsiniz.

### <a name="iterative-development-and-debugging-by-using-visual-tools"></a>Yinelemeli geliştirme ve görsel araçları kullanarak hata ayıklama
Azure Data Factory'ye görsel Araçlar yinelemeli geliştirme ve hata ayıklamayı etkinleştirin. İşlem hatlarınızı oluşturabilir ve test çalıştırmaları kullanarak **hata ayıklama** tek satırlık bir kod yazmadan işlem hattı tuvalinde yeteneği. Test çalıştırmalarınızın sonuçlarını görüntüleyebilirsiniz **çıkış** , işlem hattı tuvaline penceresi. Test çalıştırması başarılı olduktan sonra daha fazla etkinlik ardışık düzeninize ekleme ve yinelemeli bir şekilde hata ayıklamaya devam et. Devam eden sonra ayrıca test çalıştırmalarınızın iptal edebilirsiniz. 

Değişikliklerinizi seçmeden önce data factory hizmetinde yayımlayın gerekmez **hata ayıklama**. Bu, yeni eklemeler veya değişiklikler veri fabrikası akışlarınızı geliştirme, test ve üretim ortamlarında güncelleştirmeden önce beklendiği gibi çalıştığından emin olmak için istediğiniz senaryolarda faydalıdır. 

### <a name="ability-to-deploy-ssis-packages-to-azure"></a>SSIS paketlerini Azure'a dağıtma olanağı 
SSIS iş yüklerinizi taşımak istiyorsanız, bir veri fabrikası oluşturma ve bir Azure-SSIS tümleştirme çalışma zamanı sağlama. Bir Azure-SSIS tümleştirme çalışma zamanı, Azure bulutta SSIS paketlerinizi çalıştırmaya ayrılmış VM'lerin (düğümler) tam olarak yönetilen bir kümesidir. Adım adım yönergeler için bkz: [dağıtma SSIS paketlerini azure'a](tutorial-create-azure-ssis-runtime-portal.md) öğretici. 
 
### <a name="sdks"></a>SDK’lar
İleri düzey bir kullanıcıysanız ve bir programlama arabirimi arıyorsanız, Data Factory zengin bir SDK kümesi sağlayan yazmak, yönetmek veya sık kullandığınız IDE'yi kullanarak işlem hatlarını izlemek için kullanabilirsiniz. Dil desteği, .NET, PowerShell, Python ve REST içerir.

### <a name="monitoring"></a>İzleme
PowerShell, SDK veya Visual izleme araçları tarayıcı kullanıcı arabirimi aracılığıyla, veri Fabrikalarını izleyebilirsiniz. İzleyin ve isteğe bağlı, tetikleyici tabanlı ve saat temelli özel akışlarınızı verimli ve etkili bir şekilde yönetin. Var olan görevleri iptal, bkz: hataları bir bakışta ayrıntılı hata iletileri almak ve cam geçiş veya geri ve İleri ekranlar arasında gezinme bağlamı olmadan tüm tek bölmesinden sorunlarında hata ayıklama detayına gidin. 

### <a name="new-features-for-ssis-in-data-factory"></a>SSIS Data factory'de yeni özellikler
İlk genel Önizleme sürümü 2017'de olduğundan, Data Factory, SSIS için aşağıdaki özellikleri eklemiştir:

-   Üç için daha fazla yapılandırmaları/seçeneklerinde projelerini/paketlerini (SSISDB) SSIS veritabanını barındırmak için Azure SQL veritabanı'nın destekler:
-   SQL veritabanı ile sanal ağ hizmet uç noktaları
-   Yönetilen örnek
-   Elastik havuz
-   İmkan tanıyan gelecekte kullanım için Klasik sanal ağ üzerinde bir Azure Resource Manager sanal ağı için destek ekleme/JOIN SQL veritabanı için sanal ağ hizmeti ile yapılandırılmış bir sanal ağ, Azure-SSIS tümleştirme çalışma zamanı uç noktalar/mı/şirket içi verilere erişim. Daha fazla bilgi için Ayrıca bkz: [bir Azure-SSIS tümleştirme çalışma zamanını bir sanal ağa katılın](join-azure-ssis-integration-runtime-virtual-network.md).
-   Azure Active Directory (Azure AD) kimlik doğrulaması ve Azure kaynakları için yönetilen Data Factory kimliğinizle Azure AD kimlik doğrulaması sağlayan SSISDB bağlanmak için SQL kimlik doğrulaması için destek
-   Azure hibrit avantajı seçeneğinden önemli maliyet tasarrufları kazanmak için kendi şirket içi SQL Server lisansınızı getirmek için destek
-   Destek sağlayan Azure-SSIS Integration runtime'nın Enterprise Edition için Gelişmiş premium özellikler, bir iş ortağı ekosisteminin yanı sıra ek bileşenleri ve uzantıları yüklemek için özel kurulum arabirimi kullanın. Daha fazla bilgi için Ayrıca bkz: [Enterprise Edition, özel kurulum ve ADF SSIS için 3 taraf genişletilebilirlik](https://blogs.msdn.microsoft.com/ssis/2018/04/27/enterprise-edition-custom-setup-and-3rd-party-extensibility-for-ssis-in-adf/). 
-   Daha derin tümleştirme ssıs'nin Data factory'de birinci sınıf SSIS paketi yürütme etkinlikleri Data Factory işlem hatları çağırma/tetikleyici ve bunları SSMS zamanlama olanak sağlar. Daha fazla bilgi için Ayrıca bkz: [Modernize ve ETL/ELT iş akışlarınızı SSIS etkinliklerle ADF işlem hatlarını genişletin](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/).


## <a name="what-is-the-integration-runtime"></a>Tümleştirme çalışma zamanı nedir?
Tümleştirme çalışma zamanının çeşitli ağ ortamlarında aşağıdaki veri tümleştirme özellikleri sağlamak için Azure Data Factory kullanan işlem altyapısıdır:

- **Veri taşıma**: Integration runtime, veri taşıma işlemi için yerleşik bağlayıcılar, biçim dönüştürme, sütun eşleme ve yüksek performanslı ve ölçeklenebilir veri aktarımı için destek sağlarken kaynak ve hedef veri depoları arasında veri taşır.
- **Gönderme etkinlikleri**: Tümleştirme çalışma zamanı, dönüştürme işlemi için SSIS paketlerinin yerel olarak yürütülebilmesi özelliği sağlar.
- **SSIS paketlerini yürütme**: Tümleştirme çalışma zamanı, SSIS paketlerini yönetilen bir Azure işlem ortamında yerel olarak yürütür. Tümleştirme çalışma zamanı, dağıtma ve Azure HDInsight, Azure Machine Learning, SQL veritabanı ve SQL Server gibi işlem hizmetlerini çeşitli çalıştırılan dönüştürme etkinliklerinin izleme de destekler.

Bir veya birçok Integration runtime örneği veri taşımak ve dönüştürmek için gerektiği şekilde dağıtabilirsiniz. Integration runtime, Azure genel ağında veya özel bir ağda (şirket içi, Azure sanal ağ veya Amazon Web Hizmetleri sanal özel bulutundaki [VPC]) çalıştırılabilir. 

Daha fazla bilgi için bkz. [Azure Data Factory'de tümleştirme çalışma zamanı](concepts-integration-runtime.md).

## <a name="what-is-the-limit-on-the-number-of-integration-runtimes"></a>Tümleştirme çalışma zamanları sayısına bir sınır nedir?
Tümleştirme çalışma zamanı örneği, bir veri fabrikasında olabilir, sayısına sabit bir sınır yoktur. Ancak, tümleştirme çalışma zamanının SSIS paketi yürütme için abonelik başına kullanabileceğiniz VM çekirdek sayısına bir sınır yoktur. Daha fazla bilgi için [veri fabrikası sınırları](../azure-subscription-service-limits.md#data-factory-limits).

## <a name="what-are-the-top-level-concepts-of-azure-data-factory"></a>Azure Data Factory en üst düzey kavramlarını nelerdir?
Azure aboneliğinin bir veya birden çok Azure Data Factory örneği (veya veri fabrikası) olabilir. Azure Data Factory, taşıma ve dönüştürme adımları ile veri odaklı iş akışları oluşturabileceğiniz platformu olarak birlikte çalışan başlıca dört bileşenden içerir.

### <a name="pipelines"></a>İşlem hatları
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. Bir işlem hattı, bir iş birimini gerçekleştirmeye yönelik etkinlikler mantıksal bir gruplandırmasıdır. İşlem hattındaki etkinlikler birlikte bir görev gerçekleştirir. Örneğin, bir işlem hattı bir grup Azure blobundan verileri alıp sonra verileri bölümlemek için bir HDInsight kümesinde Hive sorgusu çalıştırarak etkinlik içerebilir. Bir işlem hattı her etkinliği ayrı ayrı yönetmek zorunda kalmak yerine bir küme olarak etkinliklerini yönetmek için kullanabileceğiniz avantajdır. Etkinlikler, sırayla çalışmak için bir işlem hattı araya zincirleyebilirsiniz veya bağımsız olarak, paralel olarak çalışabilir.

### <a name="activities"></a>Etkinlikler
Etkinlikler bir işlem hattındaki işleme adımını temsil eder. Örneğin, bir veri deposundan başka bir veri deposuna veri kopyalamak için kopyalama etkinliğini kullanabilirsiniz. Benzer şekilde, dönüştürmek veya verilerinizi analiz etmek amacıyla Azure HDInsight kümesinde bir Hive sorgusu çalıştıran bir Hive etkinliği kullanabilirsiniz. Data Factory üç tür etkinliği destekler: veri taşıma etkinlikleri, veri dönüştürme etkinlikleri ve denetim etkinlikleri.

### <a name="datasets"></a>Veri kümeleri
Veri kümeleri, veri depoları içinde etkinliklerinizde giriş veya çıkış olarak kullanmak istediğiniz verilere işaret eden veya başvuruda bulunan veri yapılarını temsil eder. 

### <a name="linked-services"></a>Bağlı hizmetler
Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. Bunu, şöyle düşünün: Bağlı hizmet veri kaynağıyla bağlantıyı tanımlar ve bir veri kümesi verilerin yapısını temsil eder. Örneğin, bir Azure depolama bağlı hizmeti Azure depolama hesabına bağlanacak bağlantı dizesini belirtir. Ve bir Azure blob veri kümesi blob kapsayıcıyı ve verileri içeren klasörü belirtir.

Bağlı hizmetler Data Factory'de iki amacı vardır:

- Temsil etmek için bir *veri deposu* , buradakilerle, ancak şirket içi SQL Server örneği, Oracle veritabanı örneği, bir dosya paylaşımı veya bir Azure Blob Depolama hesabı için sınırlı değildir. Desteklenen veri depolarının listesi için bkz: [Azure veri fabrikasında kopyalama etkinliği](copy-activity-overview.md).
- Etkinlik yürütülmesini barındırabilen *işlem kaynağını* temsil etmek için. Örneğin, HDInsight Hive etkinliği bir HDInsight Hadoop kümesinde çalışır. Dönüştürme etkinlikleri ve desteklenen işlem ortamlarının listesi için bkz. [Azure Data factory'de veri dönüştürme](transform-data.md).

### <a name="triggers"></a>Tetikleyiciler
Tetikleyiciler bir işlem hattı çalıştırma ne zaman başlatılır belirlemek işleme birimleri temsil eder. Farklı etkinlik türleri için farklı tetikleyici türleri vardır. 

### <a name="pipeline-runs"></a>İşlem hattı çalıştırmaları
Bir işlem hattı çalıştırması, işlem hattı yürütme örneğidir. Genellikle bir işlem hattı çalıştırması bağımsız değişkenleri ardışık düzeninde tanımlanan parametrelere iletilmesiyle örneği oluşturur. El ile veya tetikleyici tanımı içinde bağımsız değişkenler geçirebilirsiniz.

### <a name="parameters"></a>Parametreler
Parametreler salt okunur yapılandırmasında anahtar-değer çiftleridir. Bir işlem hattında parametrelerini tanımlayın ve tanımlı parametrelerin bağımsız değişkenleri, yürütme sırasında bir çalışma bağlamında geçirin. Çalıştırma bağlamı veya el ile yürütülen işlem hattı bir tetikleyici tarafından oluşturulur. İşlem hattındaki etkinlikler parametre değerlerini kullanır.

Bir veri kümesi, türü kesin olarak belirtilmiş bir parametre ve yeniden kullanabilir veya başvuran bir varlığın ' dir. Bir etkinliğin veri kümelerine başvurabilir ve bu veri kümesi tanımında belirtilen özellikleri kullanabilir.

Bağlı hizmet de bir veri deposu ya da işlem ortamı ile bağlantı bilgilerini içeren bir türü kesin belirlenmiş parametredir. Bu ayrıca, yeniden kullanabilmek veya başvuru bir varlıktır.

### <a name="control-flows"></a>Denetim akışları
Denetim akışı zincirleme etkinlikleri sıradaki dallanma, bir işlem hattı düzeyinde tanımladığınız parametreleri içeren işlem hattı etkinliklerinin düzenleyin ve işlem hattı talep üzerine ya da bir tetikleyiciden geçirdiğiniz bağımsız değişkenler çağırın. Denetim akışı, geçirme ve döngü kapsayıcılarını (diğer bir deyişle, foreach yineleyicilerini) özel bir durum da içerir.


Data Factory kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Veri kümeleri ve bağlı hizmetler](concepts-datasets-linked-services.md)
- [İşlem hatları ve etkinlikler](concepts-pipelines-activities.md)
- [Tümleştirme çalışma zamanı](concepts-integration-runtime.md)

## <a name="what-is-the-pricing-model-for-data-factory"></a>Data Factory fiyatlandırma modeli nedir?
Fiyatlandırma ayrıntıları Azure Data Factory için bkz. [Data Factory fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/data-factory/).

## <a name="how-can-i-stay-up-to-date-with-information-about-data-factory"></a>Nasıl miyim Data Factory hakkında bilgi ile güncel kalmanız?
Azure Data Factory hakkında en güncel bilgiler için şu sitelere gidin:

- [Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
- [Belgeler giriş sayfası](/azure/data-factory)
- [Ürün giriş sayfası](https://azure.microsoft.com/services/data-factory/)

## <a name="technical-deep-dive"></a>Ayrıntılı Teknik İnceleme 

### <a name="how-can-i-schedule-a-pipeline"></a>Bir işlem hattı nasıl zamanlayabilir miyim? 
Zamanlayıcı tetikleyicisi veya zaman pencere tetikleyicisi, işlem hattı zamanlamak için kullanabilirsiniz. Tetikleyici, işlem hatları (Pazartesi, 18:00:00) ve Perşembe saat 21:00:00 üzerinde gibi zamanlayabilirsiniz düzenli aralıklarla veya yinelenen desenleri Takvim tabanlı bir duvar saati takvim zamanlama kullanır. Daha fazla bilgi için bkz. [İşlem hattı yürütme ve tetikleyiciler](concepts-pipeline-execution-triggers.md).

### <a name="can-i-pass-parameters-to-a-pipeline-run"></a>Bir işlem hattı çalıştırması için parametreleri geçirmek?
Evet, Data factory'de birinci sınıf, üst düzey bir kavram parametrelerdir. İşlem hattı düzeyinde parametre tanımlayabilir ve bağımsız değişkenler isteğe bağlı veya bir tetikleyici kullanarak işlem hattı yürütme olarak geçirin.  

### <a name="can-i-define-default-values-for-the-pipeline-parameters"></a>Ben, işlem hattı parametrelerinin varsayılan değerleri tanımlayabilir misiniz? 
Evet. İşlem hatlarında parametrelerinin varsayılan değerleri tanımlayabilirsiniz. 

### <a name="can-an-activity-in-a-pipeline-consume-arguments-that-are-passed-to-a-pipeline-run"></a>Bir işlem hattındaki bir etkinliğin bir işlem hattı çalıştırması için geçirilen bağımsız değişkenleri kullanabilir? 
Evet. İşlem hattı içindeki her bir etkinlik ile çalıştırın ve işlem hattına geçirilen parametre değeri tüketebileceği `@parameter` oluşturun. 

### <a name="can-an-activity-output-property-be-consumed-in-another-activity"></a>Bir etkinlik çıkışı özelliğinin başka bir etkinliği tarafından kullanılabilir? 
Evet. Bir etkinlik çıkışı ile sonraki bir etkinliği kullanılabilir `@activity` oluşturun.
 
### <a name="how-do-i-gracefully-handle-null-values-in-an-activity-output"></a>Bir etkinlik çıkışı null değerleri düzgün bir şekilde nasıl yapabilirim? 
Kullanabileceğiniz `@coalesce` null değerlerini düzgün biçimde işlemesi için ifadelerinde oluşturun. 

## <a name="mapping-data-flows"></a>Eşleme veri akışları

### <a name="which-data-factory-version-do-i-use-to-create-data-flows"></a>Bir veri akışı oluşturmak için hangi Data Factory sürüm kullanabilir?
Bir veri akışı oluşturmak için Data Factory V2 sürümü kullanın.
  
### <a name="i-was-a-previous-private-preview-customer-who-used-data-flows-and-i-used-the-data-factory-v2-preview-version-for-data-flows"></a>Ben bir veri akışı kullanan önceki özel Önizleme müşterisi olan ve Data Factory V2 Önizleme sürümü için bir veri akışı kullandım.
Bu sürüm artık kullanılmıyor. Data Factory V2 veri akışları için kullanın.
  
### <a name="what-has-changed-from-private-preview-to-limited-public-preview-in-regard-to-data-flows"></a>Bir veri akışı in regard to sınırlı genel Önizleme için hangi özel Önizlemesi'nden değişti mi?
Artık kendi Azure Databricks kümeleri getirmek gerekir. Veri Fabrikası küme oluşturma ve kapatmayı yönetebilir. BLOB veri kümeleri ve Azure Data Lake depolama Gen2'ye veri kümeleri, sınırlandırılmış metin ve Apache Parquet veri kümeleri halinde ayrılır. Data Lake depolama Gen2'ye ve Blob Depolama dosyaları depolamak için kullanmaya devam edebilirsiniz. Bu depolama altyapıları için uygun bağlı hizmetini kullanın.

### <a name="can-i-migrate-my-private-preview-factories-to-data-factory-v2"></a>Data Factory V2 için kendi özel Önizleme fabrikaları geçirebilir miyim?

Evet. [Yönergeleri izleyerek](https://www.slideshare.net/kromerm/adf-mapping-data-flow-private-preview-migration).

### <a name="i-need-help-troubleshooting-my-data-flow-logic-what-info-do-i-need-to-provide-to-get-help"></a>My veri akışı mantığı sorun gidermek için yardıma ihtiyacım var. Yardım sağlamak hangi bilgileri gerekiyor?

Microsoft Yardım veya veri akışları ile sorun giderme sağladığında, lütfen DSL kod planı belirtin. Bunu yapmak için şu adımları uygulayın:

1. Veri Akışı Tasarımcısı'ndan seçin **kod** sağ üst köşedeki. Bu veri akışı için düzenlenebilir JSON kodunu görüntüler.
2. Kod görünümünden seçim **planlama** sağ üst köşedeki. Bu iki durumlu JSON'dan salt okunur biçimlendirilmiş DSL betik plana geçiş yapar.
3. Kopyalayın ve bu betiği yapıştırın veya bir metin dosyasına kaydedin.

### <a name="how-do-i-access-data-by-using-the-other-80-dataset-types-in-data-factory"></a>Verileri Data Factory'de diğer 80 veri kümesi türleri kullanarak nasıl erişebilir?

Veri akışı eşleme özelliği, şu anda Azure SQL veritabanı, Azure SQL veri ambarı, Azure Blob Depolama veya Azure Data Lake depolama Gen2 metin dosyalarından ve Blob Depolama veya Data Lake depolama Gen2 Parquet dosyalarını yerel olarak kaynak ve havuz için ayrılmış sağlar. 

Kopyalama etkinliği için veri hazırlamak herhangi diğer bağlayıcıları kullanın ve hazırlanmış sonra verileri dönüştürmek için bir veri akışı etkinliği yürütebilirsiniz. Örneğin, işlem hattınızı önce Blob Depolama'ya kopyalar ve bir veri akışı etkinlik verileri dönüştürmek için kaynakta bir veri kümesi sonra kullanır.

## <a name="next-steps"></a>Sonraki adımlar
Veri Fabrikası oluşturmak adım adım yönergeler için aşağıdaki öğreticilere bakın:

- [Hızlı Başlangıç: Veri Fabrikası oluşturma](quickstart-create-data-factory-dot-net.md)
- [Öğretici: Bulutta veri kopyalama](tutorial-copy-data-dot-net.md)
