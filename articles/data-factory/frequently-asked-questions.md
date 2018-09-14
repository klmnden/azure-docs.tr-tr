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
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: shlo
ms.openlocfilehash: 73bc8b6954470d11d6369bc733bb7c6f794ce892
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45577141"
---
# <a name="azure-data-factory-faq"></a>Azure veri fabrikası ile ilgili SSS
Bu makalede Azure Data Factory hakkında sık sorulan soruların yanıtlarını sağlar.  

## <a name="what-is-azure-data-factory"></a>Azure Data Factory nedir? 
Veri Fabrikası taşımayı ve dönüştürmeyi otomatikleştiren bir tam olarak yönetilen, bulut tabanlı veri tümleştirme hizmetidir. Ham madde tamamlanmış ürünlere dönüştürmek için donanım çalıştıran bir Fabrika gibi Azure Data Factory, ham verileri toplayan ve kullanıma hazır bilgilere dönüştürün var olan hizmetleri düzenler. 

Azure Data Factory kullanarak verileri şirket içi ile bulut arasında taşımak için veri odaklı iş akışları oluşturabilirsiniz veri depoları. İşleyebilir ve tümleştirme çalışma zamanı, Azure HDInsight, Azure Data Lake Analytics ve SQL Server Integration Services (SSIS) gibi hizmetleri kullanarak verileri dönüştürme işlem. 

Data Factory ile veri işleme, Azure tabanlı bir bulut hizmeti veya kendi şirket içinde barındırılan işlem ortamı SSIS, SQL Server veya Oracle gibi yürütebilir. Gereken bir eylem gerçekleştiren bir işlem hattı oluşturduktan sonra düzenli aralıklarla (örneğin, saatlik, günlük veya haftalık) çalıştırma, zaman penceresi zamanlama veya tetikleyici işlem hattı bir olay örnekten zamanlayabilirsiniz. Daha fazla bilgi için bkz. [Azure Data Factory'ye giriş](introduction.md).

### <a name="control-flows-and-scale"></a>Denetim akışı ve ölçeklendirme 
Modern veri ambarında çeşitli tümleştirme akışlarını ve desenlerini desteklemek için Data Factory programlama paradigmalarını koşullu yürütme dahil olmak üzere tam denetim akışı içeren, esnek veri işlem hattı modelleme veri komut zincirini, dallanma sağlar ve Bu akışların içinde ve arasında parametreleri açıkça geçirebilirsiniz. Denetim akışı dış yürütme motorları ve veri akışı özellikleri, uygun ölçekte, kopyalama etkinliği ile verileri taşıma dahil olmak üzere etkinlik dağıtma verilerine dönüştürme de kapsar.

Veri fabrikası, veri tümleştirmesi için gerekli ve isteğe bağlı veya bir zamanlamaya göre tekrarlanarak gönderilen bir akış stilini modelleme özgürlüğüne sağlar. Bu model sağlayan birkaç yaygın akış şunlardır:   

- Denetim akışı:
    - Bir işlem hattı içindeki sıralı etkinlikleri zinciri.
    - Bir işlem hattı içindeki etkinlikleri dal.
    - Parametreler
        - İşlem hattı düzeyinde parametre tanımlayın ve işlem hattı talep üzerine ya da bir tetikleyiciden çağırma sırasında bağımsız değişkenler geçirebilirsiniz.
        - Etkinlikler işlem hattına geçirilen bağımsız değişkenleri kullanabilir.
    - Özel durum geçirme
        - Durum da dahil olmak üzere etkinlik çıktıları, işlem hattının sonraki bir etkinliği tarafından kullanılabilir.
    - Döngü kapsayıcıları
        - For-each 
- Tetikleyici temelli akışlar:
    - İşlem hatları talep üzerine veya duvar saati zamanı tarafından tetiklenebilir.
- Delta akışlar:
    - Parametreleri kullanın ve bir ilişkisel mağazadan şirket içinde veya bulutta verileri göle yüklemek için boyut ya da başvuru tablolarını taşırken, yüksek su işareti delta kopya için tanımlayın. 

Daha fazla bilgi için [öğretici: Denetim Akışları](tutorial-control-flow.md).

### <a name="transform-your-data-at-scale-with-code-free-pipelines"></a>Uygun ölçekte kod ücretsiz işlem hatları ile veri dönüştürme
Yeni bir tarayıcı tabanlı bir araç deneyimi, kod gerektirmeyen bir işlem hattı yazma ve dağıtım ile bir modern, etkileşimli web tabanlı deneyim sağlar.

Görsel verileri geliştiriciler ve veri mühendisleri için ADF Web kullanıcı arabirimini, komut zincirleri oluşturmak için kullanacağınız kod gerektirmeyen bir tasarım ortamıdır. Visual Studio Online Git ile tamamen tümleşiktir ve CI/CD tümleştirmesi ve hata ayıklama seçenekleri ile yinelemeli geliştirme sağlar.

### <a name="rich-cross-platform-sdks-for-advanced-users"></a>Zengin platform SDK'larını İleri düzey kullanıcılar için çapraz
İleri düzey bir kullanıcıysanız ve bir programlama arabirimi arıyorsanız, ADF V2 sağlayan zengin bir SDK'ları oluşturmak, yönetmek amacıyla, sık kullandığınız IDE'yi kullanarak işlem hatlarını izleyin
1.  Python SDK'sı
2.  PowerShell CLI
3.  C# SDK'sı kullanıcıları arabirimi ADF V2 ile belgelenmiş için REST API'lerini de yararlanabilir

### <a name="iterative-development-and-debugging-using-visual-tools"></a>Yinelemeli geliştirme ve görsel araçları kullanarak hata ayıklama
Azure Data Factory (ADF) görsel araçları yinelemeli geliştirme ve hata ayıklama yapmanıza olanak sağlar. Test çalıştırmaları tek satırlık bir kod yazmadan işlem hattı tuvalinde hata ayıklama özelliğini kullanarak ve işlem hatlarınızı oluşturabilirsiniz. İşlem hattı tuvalinize çıkış penceresinde test çalıştırmalarınızın sonuçlarını görüntüleyebilirsiniz. Test çalıştırması başarılı olduktan sonra daha fazla etkinlik ardışık düzeninize ekleme ve yinelemeli bir şekilde hata ayıklamaya devam et. Devam eden sonra ayrıca test çalıştırmalarınızın iptal edebilirsiniz. Hata ayıklama tıklamadan önce değişikliklerinizi data factory hizmetinde yayımlamak için gerekli değildir. Bu, yeni eklemeler veya değişiklikler iş geliştirme, veri fabrikası akışlarınızda güncelleştirmeden önce beklendiği gibi test veya üretim ortamları emin olmak için istediğiniz senaryolarda faydalıdır. 

### <a name="deploy-ssis-packages-to-azure"></a>SSIS paketlerini Azure'a dağıtma 
SSIS iş yüklerinizi taşımak istiyorsanız, bir veri fabrikası oluşturma ve bir Azure-SSIS tümleştirme çalışma zamanı sağlama. Azure-SSIS Integration runtime, Azure bulutta SSIS paketlerinizi çalıştırmaya ayrılmış VM'lerin (düğümler) tam olarak yönetilen bir kümesidir. Adım adım yönergeler için bkz: [dağıtma SSIS paketlerini azure'a](tutorial-create-azure-ssis-runtime-portal.md) öğretici. 
 
### <a name="sdks"></a>SDK’lar
İleri düzey bir kullanıcıysanız ve bir programlama arabirimi arıyorsanız, ADF zengin bir SDK kümesi sağlayan yazmak, yönetmek veya sık kullandığınız IDE'yi kullanarak işlem hatlarını izlemek için kullanabilirsiniz. Dil desteği, .NET, PowerShell, Python ve REST içerir.

### <a name="monitoring"></a>İzleme
PowerShell, SDK veya Visual izleme araçları tarayıcı kullanıcı arabirimi aracılığıyla, veri Fabrikalarını izleyebilirsiniz. İzleme ve isteğe bağlı, tetikleyici tabanlı ve verimli ve etkili bir şekilde özel akış temelli saat yönetme. Var olan görevleri iptal, bkz: hataları bir bakışta ayrıntılı hata iletileri almak ve tümünü tek bir cam bölmeyle'dan geçiş veya geri ve İleri ekranlar arasında gezinme bağlamı olmadan ayıklanır detayına gidin. 

### <a name="new-features-for-ssis-in-adf"></a>ADF SSIS için yeni özellikler
2017'de ilk genel Önizleme sürümünden sonra Data Factory, SSIS için aşağıdaki özellikleri eklemiştir:

-   Üç için daha fazla yapılandırmaları/çeşitleri konak SSIS kataloğuna projelerini/paketlerini (SSISDB), Azure SQL veritabanı (DB), destekler:
-   Azure SQL DB ile sanal ağ hizmet uç noktaları
-   Yönetilen örnek (mı)
-   Elastik havuz
-   Azure Resource Manager sanal ağı (VNet) gelecekte – kullanım dışı bırakılacak Klasik VNet üzerinde desteği bu sayesinde, Azure-SSIS Integration Runtime (IR) için Azure SQL DB sanal ağ hizmet uç noktaları/mı ile yapılandırılmış bir sanal ağa ekleme/Birleştir / Şirket içinde veri erişimi için bkz: https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network 
-   Destek üstünde, SSISDB için - bağlanmak için SQL kimlik doğrulaması Azure Active Directory (AAD) kimlik doğrulaması için bu, ADF yönetilen hizmet kimliği (MSI) ile AAD kimlik doğrulama kullanmanıza olanak tanır
-   Azure hibrit Avantajı'nı (AHB) seçeneğinden önemli maliyet tasarrufları kazanmak için kendi şirket içi SQL Server lisansınızı getirmek için destek
-   Enterprise Edition'ın Azure-SSIS sağlayan IR için destek, Gelişmiş premium özellikler, 3. taraf ekosisteminin yanı sıra ek bileşenleri ve uzantıları yüklemek için bkz özel kurulum kullanın: https://blogs.msdn.microsoft.com/ssis/2018/04/27/enterprise-edition-custom-setup-and-3rd-party-extensibility-for-ssis-in-adf/ 
-   Birinci sınıf SSIS paketi yürütme etkinlikleri ADF işlem hatları çağırma/tetikleyici ve bunları SSMS zamanlama sağlayan ADF içinde ssıs'nin derin tümleştirme bakın: https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/ 


## <a name="what-is-integration-runtime"></a>Tümleştirme çalışma zamanı nedir?
Integration runtime, çeşitli ağ ortamlarında aşağıdaki veri tümleştirme özellikleri sağlamak için Azure Data Factory tarafından kullanılan işlem altyapısıdır:

- **Veri taşıma**: yerleşik bağlayıcılar, biçim dönüştürme, sütun eşleme ve yüksek performanslı ve ölçeklenebilir veri aktarımı için destek sağlarken kaynak ve hedef veri depoları arasında veri taşıma işlemi için tümleştirme çalışma zamanı verileri taşır.
- **Gönderme etkinlikleri**:, dönüştürme işlemi için tümleştirme çalışma zamanı, SSIS paketlerini yerel olarak yürütülebilmesi özelliği sağlar.
- **SSIS paketlerini yürütme**: SSIS paketlerini yönetilen bir Azure işlem ortamında yerel olarak yürütür. Integration Runtime ayrıca Azure HDInsight, Azure Machine Learning, Azure SQL Veritabanı, SQL Server ve diğer bazı işlem hizmetlerinde çalıştırılan dönüştürme etkinliklerinin dağıtılması ve izlenmesini de destekler.

Bir veya birçok Integration runtime örneği veri taşımak ve dönüştürmek için gerektiği şekilde dağıtabilirsiniz. Integration runtime, Azure genel ağında veya özel bir ağda (şirket içi, Azure sanal ağ veya Amazon Web Hizmetleri sanal özel bulutundaki [VPC]) çalıştırılabilir. 

Daha fazla bilgi için bkz. [Azure Data Factory'de tümleştirme çalışma zamanı](concepts-integration-runtime.md).

## <a name="what-is-the-limit-on-the-number-of-integration-runtimes"></a>Tümleştirme çalışma zamanları sayısına bir sınır nedir?
Tümleştirme çalışma zamanı örneği, bir veri fabrikasında olabilir, sayısına sabit bir sınır yoktur. Ancak, tümleştirme çalışma zamanının SSIS paketi yürütme için abonelik başına kullanabileceğiniz VM çekirdek sayısına bir sınır yoktur. Daha fazla bilgi için [veri fabrikası sınırları](../azure-subscription-service-limits.md#data-factory-limits).

## <a name="what-are-the-top-level-concepts-of-azure-data-factory"></a>Azure Data Factory en üst düzey kavramlarını nelerdir?
Azure aboneliğinin bir veya birden çok Azure Data Factory örneği (veya veri fabrikası) olabilir. Azure Data Factory, taşıma ve dönüştürme adımları ile veri odaklı iş akışları oluşturabileceğiniz platformu olarak birlikte çalışan başlıca dört bileşenden içerir.

### <a name="pipelines"></a>İşlem hatları
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. Bir işlem hattı, bir iş birimini gerçekleştirmeye yönelik etkinlikler mantıksal bir gruplandırmasıdır. İşlem hattındaki etkinlikler birlikte bir görev gerçekleştirir. Örneğin, bir işlem hattı bir grup Azure blobundan verileri alıp sonra verileri bölümlemek için bir HDInsight kümesinde Hive sorgusu çalıştırarak etkinlik içerebilir. Bir işlem hattı her etkinliği ayrı ayrı yönetmek zorunda kalmak yerine bir küme olarak etkinliklerini yönetmek için kullanabileceğiniz avantajdır. Etkinlikler, sırayla çalışmak için bir işlem hattı araya zincirleyebilirsiniz veya bağımsız olarak, paralel olarak çalışabilir.

### <a name="activity"></a>Etkinlik
Etkinlikler bir işlem hattındaki işleme adımını temsil eder. Örneğin, kullanabileceğiniz bir *kopyalama* bir veri deposundan başka bir veri deposuna veri kopyalamak için etkinlik. Benzer şekilde, dönüştürmek veya verilerinizi analiz etmek amacıyla Azure HDInsight kümesinde bir Hive sorgusu çalıştıran bir Hive etkinliği kullanabilirsiniz. Data Factory üç tür etkinliği destekler: veri taşıma etkinlikleri, veri dönüştürme etkinlikleri ve denetim etkinlikleri.

### <a name="datasets"></a>Veri kümeleri
Veri kümeleri, veri depoları içinde etkinliklerinizde giriş veya çıkış olarak kullanmak istediğiniz verilere işaret eden veya başvuruda bulunan veri yapılarını temsil eder. 

### <a name="linked-services"></a>Bağlı hizmetler
Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. Bunu şöyle düşünün: bağlı hizmet veri kaynağıyla bağlantıyı tanımlar ve bir veri kümesi verilerin yapısını temsil eder. Örneğin, bir Azure depolama bağlı hizmeti Azure depolama hesabına bağlanacak bağlantı dizesini belirtir. Ve bir Azure Blob veri kümesi blob kapsayıcıyı ve verileri içeren klasörü belirtir.

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

### <a name="control-flows"></a>Denetim akışı
Denetim akışı zincirleme etkinlikleri sıradaki dallanma, bir işlem hattı düzeyinde tanımladığınız parametreleri içeren işlem hattı etkinliklerinin düzenleyin ve işlem hattı talep üzerine ya da bir tetikleyiciden geçirdiğiniz bağımsız değişkenler çağırın. Denetim akışı, özel durum geçirme ve döngü kapsayıcılarını (diğer bir deyişle for-each yineleyicilerini) de içerir.


Data Factory kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Veri kümesi ve bağlı hizmetler](concepts-datasets-linked-services.md)
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
Zamanlayıcı tetikleyicisi veya zaman pencere tetikleyicisi, işlem hattı zamanlamak için kullanabilirsiniz. Duvar saati takvim zamanlama tetikleyicisini kullanır ve düzenli aralıklarla veya yinelenen desenleri Takvim tabanlı (örneğin, her hafta Pazartesi, 18: 00 ve Perşembe saat 21: 00,) kullanarak işlem hatlarını zamanlamak için kullanabilirsiniz. Daha fazla bilgi için bkz. [İşlem hattı yürütme ve tetikleyiciler](concepts-pipeline-execution-triggers.md).

### <a name="can-i-pass-parameters-to-a-pipeline-run"></a>Bir işlem hattı çalıştırması için parametreleri geçirmek?
Evet, birinci sınıf, üst düzey bir kavram ADF, parametrelerdir. İşlem hattı düzeyinde parametre tanımlayabilir ve bağımsız değişkenler isteğe bağlı veya bir tetikleyici kullanarak işlem hattı yürütme olarak geçirin.  

### <a name="can-i-define-default-values-for-the-pipeline-parameters"></a>Ben, işlem hattı parametrelerinin varsayılan değerleri tanımlayabilir misiniz? 
Evet. İşlem hatlarında parametrelerinin varsayılan değerleri tanımlayabilirsiniz. 

### <a name="can-an-activity-in-a-pipeline-consume-arguments-that-are-passed-to-a-pipeline-run"></a>Bir işlem hattındaki bir etkinliğin bir işlem hattı çalıştırması için geçirilen bağımsız değişkenleri kullanabilir? 
Evet. İşlem hattı içindeki her bir etkinlik ile çalıştırın ve işlem hattına geçirilen parametre değeri tüketebileceği `@parameter` oluşturun. 

### <a name="can-an-activity-output-property-be-consumed-in-another-activity"></a>Bir etkinlik çıkışı özelliğinin başka bir etkinliği tarafından kullanılabilir? 
Evet. Bir etkinlik çıkışı ile sonraki bir etkinliği kullanılabilir `@activity` oluşturun.
 
### <a name="how-do-i-gracefully-handle-null-values-in-an-activity-output"></a>Bir etkinlik çıkışı null değerleri düzgün bir şekilde nasıl yapabilirim? 
Kullanabileceğiniz `@coalesce` null değerlerini düzgün biçimde işlemesi için ifadelerinde oluşturun. 

## <a name="next-steps"></a>Sonraki adımlar
Veri Fabrikası oluşturmak adım adım yönergeler için aşağıdaki öğreticilere bakın:

- [Hızlı Başlangıç: veri fabrikası oluşturma](quickstart-create-data-factory-dot-net.md)
- [Öğretici: buluta veri kopyalama](tutorial-copy-data-dot-net.md)
