---
title: Senaryoları tanımlama ve analytics süreci - planı Team Data Science Process | Azure Machine Learning
description: Senaryolar ve Gelişmiş analiz verileri işlemeyi planlama birtakım önemli sorular dikkate alarak belirleyin.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: d8eed4f2425cdbfec7d3addad11ddaba57e5370e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64704497"
---
# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Senaryoları tanımlama ve gelişmiş analiz verileri işlemeyi planlama

Hangi kaynakların gerçekleştirebileceği bir ortam oluşturmak için gerekli olan bir veri kümesi üzerinde Gelişmiş analiz işleme? Bu makalede, bir dizi görevler ve kaynakların ilgili senaryonuz belirlemeye yardımcı olabilecek olan soru sormak için önerir.

Tahmine dayalı analiz için üst düzey adımları sırası hakkında bilgi edinmek için [Team Data Science işlem (TDSP) nedir](overview.md). Her adım, özel senaryonuzla ilgili görevler için özel kaynakları gerektirir.

Senaryonuzu tanımlama için aşağıdaki alanlarda önemli sorularını yanıtlayın:

* Veri lojistiğini
* veri özellikleri
* veri kümesi kalite
* tercih edilen araçları ve dilleri

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Lojistik sorular: veri konumları ve taşıma

Aşağıdaki öğeler Lojistik soruları kapsar:

* veri kaynağı konumu
* Hedef Azure
* zamanlama, tutar ve ilgili kaynaklar dahil olmak üzere, verileri taşımak için gereksinimler

Veri analizi işlemi sırasında birkaç kez taşımanız gerekebilir. Azure'da ve Machine Learning Studio'ya yerel veri depolama bazı forma taşıma yaygın bir senaryodur.

### <a name="what-is-your-data-source"></a>Veri kaynağı nedir?

Verilerinizi yerel mi yoksa bulutta mı? Olası yerler şunlardır:

* Genel kullanıma açık bir HTTP adresi
* Yerel veya ağ dosya konumu
* bir SQL Server veritabanı
* bir Azure depolama kapsayıcısı

### <a name="what-is-the-azure-destination"></a>Azure hedef nedir?

Burada verilerinizi işleme veya modelleme olması gerekiyor mu? 

* Azure Blob Depolama
* SQL Azure veritabanı
* Azure VM’lerde SQL Server
* HDInsight (Hadoop azure'da) ya da Hive tabloları
* Azure Machine Learning
* Azure sanal sabit diskleri bağlanabilir

### <a name="how-are-you-going-to-move-the-data"></a>Verileri taşımak için nasıl oluşturacağınız?

Yordamlar ve alma ya da farklı depolama ve işleme ortamları çeşitli verileri yüklemek için bkz:

* [Analiz için depolama ortamlarına veri yükleme](ingest-data.md)
* [Eğitim verilerinizi çeşitli veri kaynaklarından Azure Machine Learning Studio'ya alma](../studio/import-data.md)

### <a name="does-the-data-need-to-be-moved-on-a-regular-schedule-or-modified-during-migration"></a>Veri düzenli bir zamanlamaya göre taşınamaz veya geçiş sırasında değiştirilen gerekiyor mu?

Sürekli olarak geçirilecek verilere ihtiyaç duyduğunda, Azure Data Factory (ADF) kullanmayı düşünün. ADF için yararlı olabilir:

* her ikisi de kapsayan karma bir senaryoda şirket içi ve bulut kaynakları
* Burada veri işlem temelli, değiştiren veya geçirme sırasında iş mantığı değiştiren bir senaryo

Daha fazla bilgi için bkz: [veri taşıma bir şirket içi SQL Server'dan Azure Data Factory ile SQL Azure için](move-sql-azure-adf.md).

### <a name="how-much-of-the-data-is-to-be-moved-to-azure"></a>Verilerin ne kadar Azure'a taşınabilir mi?

Son derece büyük veri kümeleri belirli ortamları depolama kapasitesini aşabilir. Bir örnek için boyutu sınırları hakkında ayrıntılı bilgi için Machine Learning Studio'da sonraki bölüme bakın. Böyle durumlarda, analiz sırasında bir örnek veri kullanabilirsiniz. Aşağı-örnek bir veri kümesi çeşitli Azure ortamlarında konusunda ayrıntılar için bkz [örnek veriler Team Data Science Process içinde](sample-data.md).

## <a name="data-characteristics-questions-type-format-and-size"></a>Veri özellikleri soruları: türü, biçimi ve boyutu

Bu sorular depolama planlama ve ortamları işleme için anahtar vardır. Bunlar, veri türü için uygun senaryo seçin ve herhangi bir kısıtlama anlamanıza yardımcı olur.

### <a name="what-are-the-data-types"></a>Veri türleri nelerdir?

* Sayısal
* Kategorik
* Dizeler
* binary

### <a name="how-is-your-data-formatted"></a>Verilerinizi nasıl biçimlendirildiğini?

* Virgülle ayrılmış (CSV) veya (TSV) düz dosyaları sekmeyle ayrılmış
* Sıkıştırılmış veya sıkıştırılmamış
* Azure BLOB'ları
* Hadoop Hive tabloları
* SQL Server tabloları

### <a name="how-large-is-your-data"></a>Verilerinizi ne kadar büyük?

* Küçük: En az 2 GB
* Orta: 2 GB ve boyutu 10 GB'tan büyük
* Büyük: 10 GB değerinden fazla

Örneğin, Azure Machine Learning Studio'da ortamı uygulayın:

* Veri biçimleri ve Azure Machine Learning Studio tarafından desteklenen türleri listesi için bkz. [veri biçimlerini ve desteklenen veri türleri](../studio/import-data.md#supported-data-formats-and-data-types) bölümü.
* Analytics işleminde kullanılan diğer Azure Hizmetleri sınırlamaları hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Veri Kalitesi soruları: inceleme ve ön işleme

### <a name="what-do-you-know-about-your-data"></a>Verileriniz hakkında ne biliyor musunuz?

Verileriniz hakkında temel özelliklerini anlamak:

* Hangi desenleri veya bu eğilimler gösteriyor
* Bulunan hangi aykırı değerleri
* Kaç değer eksik

Yardımcı olması bu adım önemlidir:

* Ne kadar ön işleme gerek belirleyin
* En uygun özellikler veya çözümleme türü Öner hipotezi düzenleme
* Ek veri toplama planlarını düzenleme

Veri İnceleme için kullanışlı teknikler kullanılabilir açıklayıcı istatistikleri hesaplama ve görselleştirme çizer. Çeşitli Azure ortamlarında bir veri kümesini araştırmak nasıl ayrıntıları için bkz: [Team Data Science Process verileri araştırın](explore-data.md).

### <a name="does-the-data-require-preprocessing-or-cleaning"></a>Veri ön işleme veya temizleme gerektiriyor mu?

Önceden işleme ve machine learning için etkili bir şekilde veri kümesi kullanmadan önce verilerinizi temizleme gerekebilir. Ham verileri, genellikle gürültülü ve güvenilir. Değerleri eksik olabilir. Modelleme için bu verileri kullanarak, yanıltıcı sonuçlara neden olabilir. Bir açıklama için bkz. [için Gelişmiş veri hazırlama görevleri makine öğrenimi](prepare-data.md).

## <a name="tools-and-languages-questions"></a>Araçları ve dilleri ile ilgili sorular

Diller, geliştirme ortamları ve araçları için birçok seçenek vardır. Gereksinimlerinize ve tercihlerinize unutmayın.

### <a name="what-languages-do-you-prefer-to-use-for-analysis"></a>Hangi dillerin analiz için kullanılacak tercih ediyorsunuz?

* R
* Python
* SQL

### <a name="what-tools-should-you-use-for-data-analysis"></a>Veri analizi için hangi araçları kullanmalısınız?

* [Microsoft Azure Powershell](/powershell/azure/overview) -komut dosyası dili Azure kaynaklarınızı yönetmek için kullanılan bir komut dosyası dili
* [Azure Machine Learning Studio](../studio/what-is-ml-studio.md)
* [Revolution Analytics](https://www.microsoft.com/sql-server/machinelearningserver)
* [RStudio](https://www.rstudio.com)
* [Visual Studio için Python Araçları](https://aka.ms/ptvsdocs)
* [Anaconda](https://www.continuum.io/why-anaconda)
* [Jupyter Not Defterleri](https://jupyter.org/)
* [Microsoft Power BI](https://powerbi.microsoft.com)

## <a name="identify-your-advanced-analytics-scenario"></a>Gelişmiş analiz senaryonuzu tanımlama

Önceki bölümde sorulara verdiğiniz yanıtlara sonra uygun en iyi senaryoyu durumunuzu belirlemek hazır olursunuz. Örnek senaryolar özetlenen [Azure Machine learning'de Gelişmiş analiz senaryoları](plan-sample-scenarios.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Team Data Science işlem (TDSP) nedir?](overview.md)
