---
title: Senaryoları tanımlama ve Team Data Science Process analytics süreci - planı
description: Senaryolar ve Gelişmiş analiz verileri işlemeyi planlama birtakım önemli sorular dikkate alarak belirleyin.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 5faa7a58a252a5d3b8cc044f9e81a6d7cb2df7d5
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53138050"
---
# <a name="how-to-identify-scenarios-and-plan-for-advanced-analytics-data-processing"></a>Senaryoları tanımlama ve gelişmiş analiz verileri işlemeyi planlama
Hangi kaynakların bir ortamı ayarlama, Gelişmiş analiz bir veri kümesi üzerinde işlem yapmak için ayarlarken içerecek şekilde planlamanız gerekir? Bu makalede bir dizi soru sormak ilgili Yardım için önerdiği görevler ve kaynaklarla ilgili senaryonuzu tanımlama. Tahmine dayalı analiz için üst düzey adımları sırasını açıklandığı [Team Data Science işlem (TDSP) nedir?](overview.md). Bu adımların her biri, özel senaryonuzla ilgili görevler için özel kaynakları gerektirir. Senaryonuzu tanımlama için anahtar soruları veri lojistiğini, özellikleri, veri kümeleri ve araçları ve dilleri analiz yapmak için tercih ettiğiniz kalitesini ilgilendiriyor.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="logistic-questions-data-locations-and-movement"></a>Lojistik sorular: veri konumları ve taşıma
Konumunu Lojistik soruları ilgilendiriyor **veri kaynağı**, **hedef** Azure ve verileri taşımak için gereksinimleri, zamanlama, tutar ve ilgili kaynaklar dahil olmak üzere. Veri analizi işlemi sırasında birkaç kez taşınması gerekebilir. Azure'da ve Machine Learning Studio'ya yerel veri depolama bazı forma taşıma yaygın bir senaryodur.

1. **Veri kaynağı nedir?** Yerel mi yoksa bulutta mı? Örneğin:
   
   * Verileri bir HTTP adresinde herkes tarafından kullanılabilir.
   * Verileri bir yerel/ağ dosya konumunda yer alıyor.
   * Bir SQL Server veritabanında verilerdir.
   * Bir Azure depolama kapsayıcısında depolanan veri
2. **Azure hedef nedir?** Burada işleme veya modelleme olması gerekiyor mu? Örneğin:
   
   * Azure Blob Depolama
   * SQL Azure veritabanı
   * Azure VM’lerde SQL Server
   * HDInsight (Hadoop azure'da) ya da Hive tabloları
   * Azure Machine Learning
   * Bağlanabilir Azure sanal sabit diskler.
3. **Verileri taşımak için nasıl oluşturacağınız?** Aşağıdaki makaleler de yordamları ve alma ya da farklı depolama ve işleme ortamları çeşitli veri yükleme kullanılabilir kaynakları özetlenmiştir:
   
   * [Analiz için depolama ortamlarına veri yükleme](ingest-data.md)
   * [Eğitim verilerinizi çeşitli veri kaynaklarından Azure Machine Learning Studio'ya alma](../studio/import-data.md).
4. **Veri düzenli bir zamanlamaya göre taşınamaz veya geçiş sırasında değiştirilen gerekiyor mu?** Azure Data Factory (ADF), hem şirket içi erişen ve bulut kaynakları eklendiyse veya veri işlem temelli veya değiştirilmesi veya eklenen iş mantığına sahip gerekiyor özellikle karma bir senaryoda, sürekli olarak geçirilmesi verilere ihtiyaç duyduğunda kullanmayı düşünün Bu geçiş sırasında. Daha fazla bilgi için bkz: [taşımanız veri bir şirket içi SQL Server'dan Azure Data Factory ile SQL Azure](move-sql-azure-adf.md)
5. **Verilerin ne kadar Azure'a taşınabilir mi?** Son derece büyük veri kümeleri belirli ortamları depolama kapasitesini aşabilir. Bir örnek için boyutu sınırları hakkında ayrıntılı bilgi için Machine Learning Studio'da sonraki bölüme bakın. Böyle durumlarda, bir örnek veri analiz sırasında kullanılabilir. Aşağı-örnek bir veri kümesi çeşitli Azure ortamlarında konusunda ayrıntılar için bkz [örnek veriler Team Data Science Process içinde](sample-data.md).

## <a name="data-characteristics-questions-type-format-and-size"></a>Veri özellikleri soruları: türü, biçimi ve boyutu
Bu sorular depolama planlama ve her biri çeşitli veri türleri için uygun olan ve her biri belirli kısıtlamalara sahip ortamlar, işleme için anahtar vardır.

1. **Veri türleri nelerdir?** Örneğin:
   
   * Sayısal
   * Kategorik
   * Dizeler
   * İkili
2. **Verilerinizi nasıl biçimlendirildiğini?** Örneğin:
   
   * Virgülle ayrılmış (CSV) veya (TSV) düz dosyaları sekmeyle ayrılmış
   * Sıkıştırılmış veya sıkıştırılmamış
   * Azure BLOB'ları
   * Hadoop Hive tabloları
   * SQL Server tabloları
3. **Verilerinizi ne kadar büyük?**
   
   * Küçük: en az 2 GB
   * Orta: 2 GB ve boyutu 10 GB'tan büyük
   * Büyük: 10 GB değerinden fazla

Örneğin, Azure Machine Learning Studio'da ortamı uygulayın:

* Veri biçimleri ve Azure Machine Learning Studio tarafından desteklenen türleri listesi için bkz. [veri biçimlerini ve desteklenen veri türleri](../studio/import-data.md#data-formats-and-data-types-supported) bölümü.
* Azure Machine Learning Studio için data sınırlamaları hakkında daha fazla bilgi için bkz: **ne kadar büyük veri kümesi modüllerim için da olabilir?** bölümünü [alma ve Machine Learning için verileri dışarı aktarma](../studio/faq.md#machine-learning-studio-questions)

Analytics işleminde kullanılan diğer Azure Hizmetleri sınırlamaları hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../../azure-subscription-service-limits.md).

## <a name="data-quality-questions-exploration-and-pre-processing"></a>Veri Kalitesi soruları: inceleme ve ön işleme
1. **Verileriniz hakkında ne biliyor musunuz?** Temel özelliklerine bir anlayın kazanmak için verileri keşfedin. Hangi desenleri ya da bunu gösteriyor, eğilimleri var. hangi aykırı değerleri veya kaç değer eksik. Bu adım, en uygun özellikler önerme veya analizini yazın hipotezi formulating ve ek veri toplama planları formulating gerekli ön işleme kapsamını belirlemek için son derece önemlidir. Açıklayıcı istatistikleri hesaplama ve görselleştirmeler çizmek için veri İnceleme yararlı tekniklerdir. Çeşitli Azure ortamlarında bir veri kümesini araştırmak nasıl ayrıntıları için bkz: [Team Data Science Process verileri araştırın](explore-data.md).
2. **Veri ön işleme veya temizleme gerektiriyor mu?**
   Ön işleme ve verileri temizleme veri kümesi machine learning için etkili bir şekilde kullanılabilmesi için önce genellikle gerçekleştirilmesi gereken önemli görevlerdir. Ham veriler genellikle gürültülü ve güvenilmeyen ve değerleri eksik olabilir. Modelleme için bu verileri kullanarak, yanıltıcı sonuçlara neden olabilir. Bir açıklama için bkz. [için Gelişmiş veri hazırlama görevleri makine öğrenimi](prepare-data.md).

## <a name="tools-and-languages-questions"></a>Araçları ve dilleri ile ilgili sorular
Birçok hangi dil ve geliştirme ortamları veya araçları, veya en conformable kullanarak bağlı olarak burada seçenek vardır.

1. **Hangi dillerin analiz için kullanılacak tercih ediyorsunuz?**  
   
   * R
   * Python
   * SQL
2. **Veri analizi için hangi araçları kullanmalısınız?**
   
   * [Microsoft Azure Powershell](/powershell/azure/overview) -komut dosyası dili Azure kaynaklarınızı yönetmek için kullanılan bir komut dosyası dili.
   * [Azure Machine Learning Studio](../studio/what-is-ml-studio.md)
   * [Revolution Analytics](https://www.microsoft.com/sql-server/machinelearningserver)
   * [RStudio](http://www.rstudio.com)
   * [Visual Studio için Python Araçları](https://aka.ms/ptvsdocs)
   * [Anaconda](https://www.continuum.io/why-anaconda)
   * [Jupyter Not Defterleri](http://jupyter.org/)
   * [Microsoft Power BI](https://powerbi.microsoft.com)

## <a name="identify-your-advanced-analytics-scenario"></a>Gelişmiş analiz senaryonuzu tanımlama
Önceki bölümde sorulara verdiğiniz yanıtlara sonra uygun en iyi senaryoyu durumunuzu belirlemek hazır olursunuz. Örnek senaryolar özetlenen [Azure Machine learning'de Gelişmiş analiz senaryoları](plan-sample-scenarios.md).

