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
ms.topic: article
ms.date: 01/15/2018
ms.author: shlo
ms.openlocfilehash: 8c240e1a654c80c34f6b612d9126058e5d67c4c2
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-data-factory-faq"></a>Azure Data Factory hakkında SSS
Bu makalede Azure Data Factory hizmetinin 2 sürümü için geçerlidir. Data Factory hakkında sık sorulan soruların yanıtlarını sağlar.  

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [Data Factory sürüm 1 için sık sorulan sorular](v1/data-factory-faq.md).

## <a name="what-is-azure-data-factory"></a>Azure Data Factory nedir? 
Veri Fabrikası taşınmasını ve dönüştürülmesini veri otomatikleştiren bir tam olarak yönetilen, bulut tabanlı veri tümleştirme hizmetidir. Hammaddeleri tamamlanmış ürünlere dönüştürmek için donanım çalıştıran bir fabrikası gibi Azure Data Factory ham verileri toplayan ve kullanıma hazır bilgilere dönüştürür var olan hizmetleri düzenler. 

Azure Data Factory kullanarak, şirket içi ve bulut arasında veri taşımak için veri tabanlı iş akışları oluşturabilirsiniz verileri depolar. İşleyebilmesi için ve Azure Hdınsight, Azure Data Lake Analytics ve SQL Server Integration Services (SSIS) Tümleştirmesi çalışma zamanı gibi hizmetleri kullanarak dönüştürme veri işlem. 

Data Factory ile veri işleme Azure tabanlı bulut hizmeti veya SSIS, SQL Server ve Oracle gibi kendi kendini barındıran işlem ortamınızda yürütebilir. Gereksinim duyduğunuz bir eylem gerçekleştiren bir ardışık düzen oluşturduktan sonra düzenli aralıklarla (örneğin, saatlik, günlük veya haftalık) çalıştırabilir veya bir olayın gerçekleşmesi ardışık düzen tetiklemek üzere zamanlayabilirsiniz. Daha fazla bilgi için bkz. [Azure Data Factory'ye giriş](introduction.md).

## <a name="whats-different-in-version-2"></a>Sürüm 2’nin farkları nelerdir?
Azure Data Factory sürüm 2, özgün Azure Data Factory veri taşıma ve dönüştürme hizmetinin üzerine kurulmuştur ve daha fazla bulut öncelikli veri tümleştirme senaryosunu kapsar. Azure Data Factory sürüm 2 aşağıdaki özellikleri sunar:

- Denetim akışı ve ölçeklendirme
- Azure’da SSIS paketlerini dağıtma ve çalıştırma

Sürüm 1 sürüm, müşterilerin tasarım karmaşık veri hareketlerini ve bulutta şirket içi ve bulut sanal makinelerde (VM'ler) işleme gerektiren karma veri tümleştirme senaryolarına gerekir tanınmıyor. Bu gereksinimleri aktarmak ve verileri güvenli sanal ağ ortamlar içinde ve isteğe bağlı işlem gücü ile ölçek genişletme için işleme gerek getirin.

Veri ardışık İş analizi stratejisinin daha önemli bir parçası olma gibi Biz bu etkinlikler artımlı verileri yükler ve olay tetiklenen yürütmeleri desteklemek için esnek zamanlama gerektiren gözlenen. Karmaşıklık arttıkça, bu nedenle çok hizmetinin dallanma, döngü ve koşullu işleme dahil ortak iş akışı örneklerinde desteklemek gereksinim yapar.

Sürüm 2 ile var olan SSIS paketlerini de buluta geçirebilirsiniz. Bu eylem kaldırır ve yeni bir özellik Integration zamanının yararlanan Data Factory içinde yönetilen bir Azure hizmeti olarak SSIS kaydırır. Sürüm 2 bir SSIS tümleştirmesi çalışma zamanı yukarı dönen tarafından yürütmek, yönetmek, izlemek ve bulutta SSIS paketleri oluşturmak.

### <a name="control-flows-and-scale"></a>Denetim akışı ve ölçeklendirme 
Modern veri ambarında desenleri ve farklı tümleştirme akışları desteklemek için veri fabrikası için zaman serisi veri artık bağlı bir yeni, esnek ve veri ardışık düzen modeli etkinleştirdi. Bu sürümle birlikte, koşulları, bir veri kanalının denetim akışında şube model ve açıkça parametreleri içinde ve bu akışlar arasında geçirin.

Artık, veri tümleştirmesi için gerekli ve isteğe bağlı veya sürekli olarak bir zamanlamaya göre dağıtılan herhangi bir akış stil model özgürlüğü var. Daha önce mümkün olmayıp şu anda etkin olan birkaç yaygın akış şunlardır:   

- Denetim akışı:
    - Bir işlem hattı içinde sırayla zinciri etkinlikler.
    - Bir işlem hattı'nda şube etkinlikler.
    - Parametreler
        - Ardışık düzen düzeyinde parametrelerini tanımlayın ve isteğe bağlı veya bir tetikleyiciden ardışık düzen çağırma sırasında bağımsız değişkenleri geçirin.
        - Etkinlikler işlem hattına geçirilen bağımsız değişkenleri kullanabilir.
    - Özel durum geçirme
        - Durum da dahil olmak üzere etkinlik çıktıları, işlem hattının sonraki bir etkinliği tarafından kullanılabilir.
    - Döngü kapsayıcıları
        - For-each 
- Tetikleyici tabanlı akışları:
    - Ardışık düzen, isteğe bağlı veya duvar saati süresi tetiklenebilir.
- Delta akışları:
    - Parametreleri kullanın ve ilişkisel deposundan, hem şirket içinde veya bulutta lake verileri yüklemek için boyut veya başvuru tabloları taşırken, yüksek su işareti delta kopya tanımlayın. 

Daha fazla bilgi için bkz: [Öğreticisi: denetim akışı](tutorial-control-flow.md).

### <a name="deploy-ssis-packages-to-azure"></a>SSIS paketlerini Azure'a dağıtma 
SSIS iş yüklerinizi taşımak istiyorsanız, bir Data Factory sürüm 2 oluşturun ve bir Azure SSIS tümleştirmesi çalışma zamanı sağlayın. Azure SSIS tümleştirmesi çalışma zamanı Azure SSIS paketleri bulutta çalıştırmak için adanmış VM'nin (düğümler) tam olarak yönetilen bir kümedir. Adım adım yönergeler için bkz: [azure'a dağıtma SSIS paketleri](tutorial-create-azure-ssis-runtime-portal.md) Öğreticisi. 
 

### <a name="sdks"></a>SDK’lar
İleri düzey bir kullanıcı varsa ve bu programa dayalı bir arabirim için baktığınızda, sürüm 2 zengin bir SDK'ları sağlayan yazar, yönetme veya sık kullanılan IDE kullanılarak işlem hatlarını izlemek için kullanabilirsiniz.

- **.NET SDK**: .NET SDK, sürüm 2 için güncelleştirilmiştir. 
- **PowerShell**: PowerShell cmdlet'leri sürüm 2 için güncelleştirilmiştir. Sürüm 2 cmdlet'lerinin adında *DataFactoryV2* vardır. Örneğin, *Get-AzureRmDataFactoryV2*. 
- **Python SDK**: Bu SDK sürüm 2 içindir.
- **REST API**: REST API, sürüm 2 için güncelleştirilmiştir.  

Sürüm 2 için güncelleştirilmiş olan SDK'lar sürüm 1 istemcileriyle uyumlu değildir. 

### <a name="monitoring"></a>İzleme
Şu anda sürüm 2 veri fabrikalarının yalnızca SDK kullanılarak izlenmesini desteklemektedir. Portal henüz sürüm 2 veri fabrikalarını izleme desteği sunmamaktadır. 

## <a name="what-is-integration-runtime"></a>Tümleştirme çalışma zamanı nedir?
Tümleştirme çalışma zamanı tarafından Azure Data Factory çeşitli ağ ortamlar genelinde aşağıdaki veri tümleştirme özellikleri sağlamak için kullanılan bilgi işlem altyapısı şöyledir:

- **Veri taşıma**: özel bir ağda (şirket içi veya sanal özel ağ) genel bir ağ ve veri depolarını üzerinde veri depoları arasında verileri taşıyın. Yerleşik bağlayıcılar, biçim dönüştürme, sütun eşleme, performanslı ve ölçeklenebilir veri aktarımı desteği sunar.
- **Gönderme etkinlikleri**: çeşitli üzerinde çalışan dağıtma ve izleme dönüştürme etkinliklerinin işlem Azure Hdınsight, Azure Machine Learning, Azure SQL veritabanı, SQL Server ve daha fazlası gibi hizmetleri.
- **SSIS paketleri yürütme**: yerel bir yönetilen Azure işlem ortamında SSIS paketleri yürütür.

Bir veya daha çok örnekleri Integration zamanının verileri taşımak ve dönüştürmek için gerektiği şekilde dağıtabilirsiniz. Tümleştirme çalışma zamanı, Azure ortak ağ veya özel bir ağda (şirket içi, Azure sanal ağ veya Amazon Web Hizmetleri sanal özel bulut [VPC]) çalıştırabilirsiniz. 

Daha fazla bilgi için bkz. [Azure Data Factory'de tümleştirme çalışma zamanı](concepts-integration-runtime.md).

## <a name="what-is-the-limit-on-the-number-of-integration-runtimes"></a>Tümleştirme çalışma zamanları sayısı sınırı nedir?
Veri fabrikasında olabilir tümleştirmesi çalışma zamanı örnekleri sayısının sabit sınırı yoktur. Ancak, tümleştirme çalışma abonelik başına SSIS paketi yürütme için kullanabileceği VM çekirdek sayısına bir sınır yoktur. Daha fazla bilgi için bkz: [Data Factory sınırlar](../azure-subscription-service-limits.md#data-factory-limits).

## <a name="when-should-i-use-version-2-rather-than-version-1"></a>Sürüm 1 yerine sürüm 2 ne zaman kullanmalıyım? 
Azure Data Factory yeniyseniz, doğrudan sürüm 2'ile başlayın. Sürüm 1 zaten kullanıyorsanız, veri fabrikaları sürüm 2 üzerinde yeniden oluşturun.

> [!WARNING]
> 2 Data Factory Önizleme sürümüdür ve genel kullanılabilirlik (GA) değil. Bu nedenle, bu sürüm 1 içinde İST Data Factory, olarak aynı Azure hizmet düzeyi sözleşmesi (SLA) taahhüt altında kalan değil 

## <a name="what-are-the-top-level-concepts-of-version-2"></a>Sürüm 2 en üst düzey kavramlarını nelerdir?
Azure aboneliğinin bir veya birden çok Azure Data Factory örneği (veya veri fabrikası) olabilir. Azure Data Factory veri temelli iş akışlarını taşıyabilir ve veri dönüştürme için gerekli adımları içeren oluşturan bir platform olarak birlikte çalışan dört anahtar bileşenleri içerir.

### <a name="pipelines"></a>İşlem hatları
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. Ardışık etkinlikler bir iş birimine gerçekleştirmek için mantıksal bir gruplandırmasıdır. İşlem hattındaki etkinlikler birlikte bir görev gerçekleştirir. Örneğin, bir işlem hattı verileri Azure blob alma ve ardından verileri bölümlemek için bir Hdınsight kümesinde bir Hive sorgusu çalıştırmak etkinliklerin bir grubu içerebilir. Her etkinlik tek tek yönetmek zorunda kalmak yerine bir küme olarak etkinliklerini yönetmek için bir işlem hattı kullanabilirsiniz avantajdır. Sıralı olarak çalışması için bir ardışık düzende etkinlik zincir veya bağımsız olarak, paralel olarak işleyebilir.

### <a name="activity"></a>Etkinlik
Etkinlikler bir işlem hattındaki işleme adımını temsil eder. Örneğin, kullanabileceğiniz bir *kopyalama* veri bir veri deposundan başka bir veri deposuna kopyalamak için kopyalama etkinliği. Benzer şekilde, dönüştürme ya da verilerinizi çözümlemek için bir Azure Hdınsight kümesinde bir Hive sorgusu çalıştıran bir Hive etkinliğini kullanabilirsiniz. Data Factory üç tür etkinliği destekler: veri taşıma etkinlikleri, veri dönüştürme etkinlikleri ve denetim etkinlikleri.

### <a name="data-sets"></a>Veri kümeleri
Veri kümeleri yalnızca işaret veya etkinliklerinizde giriş veya çıkış kullanacağınız veri başvuru veri depoları içindeki veri yapılarını temsil eder. 

### <a name="linked-services"></a>Bağlı hizmetler
Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. Bunu bu şekilde düşünün: bağlı hizmet, veri kaynağına bağlantıyı tanımlar ve bir veri kümesi verilerin yapısını temsil eder. Örneğin, bir Azure Storage bağlı hizmeti Azure Storage hesabınıza bağlanmak için bağlantı dizesini belirtir. Ve bir Azure Blob veri kümesi blob kapsayıcısında ve verileri içeren klasörü belirtir.

Bağlı hizmetler Data Factory'de iki amaca sahiptir:

- Temsil etmek için bir *veri deposu* , içerir, ancak bir şirket içi SQL Server örneği, Oracle veritabanı örneği, bir dosya paylaşımı veya bir Azure Blob Depolama hesabı için sınırlı değildir. Desteklenen veri depoları listesi için bkz: [kopya etkinliği Azure Data factory'de](copy-activity-overview.md).
- Etkinlik yürütülmesini barındırabilen *işlem kaynağını* temsil etmek için. Örneğin, Hdınsight Hive etkinliği bir Hdınsight Hadoop kümesinde çalışır. Dönüştürme etkinlikleri ve desteklenen bilgi işlem ortamlarını listesi için bkz: [dönüştürme Azure Data Factory veri](transform-data.md).

### <a name="triggers"></a>Tetikleyiciler
Tetikleyiciler ardışık düzen yürütmesi zaman koparılan belirlemek işleme birimleri temsil eder. Farklı etkinlik türleri için farklı tetikleyici türleri vardır. Önizleme için bir duvar saati Zamanlayıcı tetikleyicisi destekliyoruz. 


### <a name="pipeline-runs"></a>İşlem hattı çalıştırmaları
Çalıştıran bir ardışık düzen ardışık düzen yürütmesi örneğidir. Ardışık düzeninde tanımlanan parametrelere bağımsız değişkenleri geçirme çalıştırmak bir ardışık düzen genellikle örneği oluşturur. El ile veya tetikleyici tanımındaki bağımsız değişkenler geçirebilirsiniz.

### <a name="parameters"></a>Parametreler
Anahtar-değer çiftlerinin bir salt okunur yapılandırmasında parametreleridir. Ardışık düzeninde parametrelerini tanımlayın ve yürütme sırasında bir çalışma bağlamında tanımlanan parametrelerin bağımsız değişkenleri geçirin. Çalışma bağlamı, tetikleyici veya el ile yürütme ardışık oluşturulur. İşlem hattındaki etkinlikler parametre değerlerini kullanır.

Bir veri türü kesin belirlenmiş bir parametre ve yeniden veya başvuru bir varlığın kümesidir. Veri kümesi bir etkinlik başvurusu yapabilir ve veri kümesi tanımında tanımlanan özellikler kullanmasını sağlayabilirsiniz.

Bağlı hizmet ayrıca bir veri deposu ya da bir bilgi işlem ortamı bağlantı bilgilerini içeren bir kesin türü belirtilmiş parametredir. Bu aynı zamanda yeniden kullanabilir veya başvuru bir varlıktır.

### <a name="control-flows"></a>Denetim akışı
Denetim akışı etkinlikleri sırayla dallanma, ardışık düzen düzeyinde tanımlayan parametreleri zincirleme dahil ardışık etkinlikleri düzenlemek ve geçirdiğiniz bağımsız değişkenler isteğe bağlı veya bir tetikleyiciden ardışık düzen çağırma. Denetim akışı özel durumu geçirme ve kapsayıcıları (diğer bir deyişle, için-her yineleyiciler) döngü da kapsar.


Data Factory kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Veri kümesi ve bağlantılı Hizmetleri](concepts-datasets-linked-services.md)
- [İşlem hatları ve etkinlikler](concepts-pipelines-activities.md)
- [Tümleştirme çalışma zamanı](concepts-integration-runtime.md)

## <a name="what-is-the-pricing-model-for-data-factory"></a>Veri Fabrikası için fiyatlandırma modeli nedir?
Fiyatlandırma ayrıntıları olan Azure Data Factory için bkz: [Data Factory fiyatlandırma ayrıntıları](https://azure.microsoft.com/pricing/details/data-factory/).

## <a name="what-regions-support-azure-data-factory-version-2"></a>Hangi bölgeleri Azure Data Factory sürüm 2 destekleniyor mu?
Şu anda veri fabrikaları sürüm 2, Doğu ABD, Doğu ABD 2 ve Batı Avrupa bölgelerde oluşturabilirsiniz. Ancak, veri depoları, işlem Hizmetleri veya gönderme SSIS paketleri karşı gönderme etkinlikler arasında veri taşımak için bir veri fabrikası tümleştirmesi çalışma zamanı başka bir bölgede kullanabilirsiniz. Daha fazla bilgi için bkz: [Data Factory konumları](concepts-integration-runtime.md#integration-runtime-location).

## <a name="how-can-i-stay-up-to-date-with-information-about-data-factory"></a>Nasıl ı Data Factory hakkında bilgi içeren güncelliği?
Azure Data Factory hakkında en güncel bilgiler için aşağıdaki sitelere bakın:

- [Blog](https://azure.microsoft.com/blog/tag/azure-data-factory/)
- [Belge giriş sayfası](/azure/data-factory)
- [Ürün giriş sayfası](https://azure.microsoft.com/services/data-factory/)

## <a name="technical-deep-dive"></a>Teknik derinlemesine bakış 

### <a name="can-version-1-and-version-2-pipelines-run-side-by-side"></a>Sürüm 1 ve yan yana çalıştırma sürüm 2 ardışık düzen kullanabilir?
Hayır. Sürüm 2 ve sürüm 1 data Factory varlıkları (örneğin, bağlı hizmetler, veri kümeleri veya işlem hatları) diğer sürümünün içeremez.   

### <a name="do-i-still-need-to-define-data-sets-in-version-2"></a>Sürüm 2 veri kümelerini tanımlayın gerekiyor mu?
Bir veri kümesi artık çoğu etkinlikler için zorunlu bir varlık değildir. Kopyalama, machine learning, arama, doğrulama ve şeması ve diğer meta veri bilgileri veri kümesinde dönüşümü için kullanın. özel etkinlikler için gereklidir. Kalan etkinliklerin artık veri kümeleri gerektirir.

### <a name="can-i-chain-two-activities-without-a-data-set-in-version-2"></a>Sürüm 2 veri kümesinde olmadan iki etkinlik zincir?
Evet. Veri kümeleri gerek kalmadan sürüm 2 aktivitelerde zincir. Etkinlikleri kullanarak zincir **dependsOn** hattınızı JSON tanımını özelliği. 

### <a name="are-all-the-version-1-activities-supported-in-version-2"></a>Tüm sürüm 2 sürümünde desteklenen 1 etkinlikleri hangileri? 
Evet, tüm sürüm 1 etkinlikleri sürüm 2 desteklenir.

### <a name="how-can-i-schedule-a-version-2-pipeline"></a>Sürüm 2 ardışık düzen nasıl zamanlayabilir miyim? 
Zamanlayıcı tetikleyicisi sürüm 2 ardışık düzen zamanlamak için kullanabilirsiniz. Tetikleyici duvar saati takvim zamanlama kullanır ve düzenli aralıklarla veya (örneğin, haftalık 6 PM adresindeki Pazartesi ve Salı günleri 9 PM adresindeki üzerinde) takvim tabanlı yinelenen desenler kullanılarak işlem hatlarını zamanlamak için kullanabilirsiniz. Daha fazla bilgi için bkz. [İşlem hattı yürütme ve tetikleyiciler](concepts-pipeline-execution-triggers.md).

### <a name="can-i-pass-parameters-to-a-pipeline-run-in-version-2"></a>Parametreleri sürüm 2 çalıştıran bir ardışık düzene geçirebilirsiniz?
Evet, 2 sürümündeki birinci sınıf, üst düzey bir kavram parametreleridir. Ardışık düzen düzeyinde parametrelerini tanımlayın ve bağımsız değişkenler isteğe bağlı veya bir tetikleyici kullanarak çalıştırmak ardışık düzen yürütmek olarak geçirin.  

### <a name="can-i-define-default-values-for-the-pipeline-parameters"></a>Ardışık Düzen parametrelerinin varsayılan değerleri tanımlayın? 
Evet. Ardışık düzenlerinde parametrelerinin varsayılan değerleri tanımlayabilirsiniz. 

### <a name="can-an-activity-in-a-pipeline-consume-arguments-that-are-passed-to-a-pipeline-run"></a>Ardışık düzeninde bir aktivite çalıştırmak bir ardışık düzene geçirilen bağımsız değişkenlerini tüketebileceği? 
Evet. Ardışık Düzen içindeki her etkinliği ardışık düzene geçirilen ve çalıştırmalarına parametre değeri tüketebileceği `@parameter` oluşturun. 

### <a name="can-an-activity-output-property-be-consumed-in-another-activity"></a>Bir etkinlik çıkış özelliği başka bir etkinliğin tüketilebilir? 
Evet. Bir etkinlik çıkışı ile sonraki bir aktivite içinde kullanılabilecek `@activity` oluşturun.
 
### <a name="how-do-i-gracefully-handle-null-values-in-an-activity-output"></a>Nasıl bir etkinlik çıkışı null değerleri kolaylıkla idare? 
Kullanabileceğiniz `@coalesce` null değerler kolaylıkla idare edecek ifadelerinde oluşturun. 

### <a name="can-i-use-retry-and-timeout-at-the-activity-level-in-version-2"></a>Sürüm 2 etkinlik düzeyinde yeniden deneme ve zaman aşımı kullanabilir miyim?
Evet. Sürüm 1 yaptığınız gibi sürüm 2 etkinliklerin yürütülmesi yönetmek üzere etkinlik düzeyinde yeniden deneme ve zaman aşımı yapılandırabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
2. sürümün data factory oluşturmak adım adım yönergeler için aşağıdaki öğreticiler bakın:

- [Hızlı Başlangıç: bir veri fabrikası oluşturun](quickstart-create-data-factory-dot-net.md)
- [Öğretici: verileri bulutta kopyalayın.](tutorial-copy-data-dot-net.md)

