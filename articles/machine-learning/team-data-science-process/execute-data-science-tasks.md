---
title: Veri bilimi görevleri - Team Data Science Process yürütme
description: Bir veri Bilimcisi bir veri bilimi proje nasıl izlenebilir, yürütebilir sürüm denetimli ve işbirliğine dayalı yolu.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/28/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 9d8ae3a95262b1554e7e97fac8375a44743bf4df
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60344674"
---
# <a name="execute-data-science-tasks-exploration-modeling-and-deployment"></a>Veri bilimi görevleri yürütme: keşfi, modelleme ve dağıtım

Veri keşfi, modelleme ve dağıtım tipik veri bilimi görevleri içerir. Bu makalede nasıl kullanılacağını gösterir **etkileşimli veri keşfi, analiz ve Raporlama (IDEAR)** ve **otomatik modelleme ve Raporlama (AMAR)** çeşitli genel veri bilimi görevlerini tamamlamak için yardımcı programlar Etkileşimli veri keşfi, veri analizi, raporlama ve modeli oluşturma gibi. Ayrıca, bir model çeşitli aşağıdaki gibi araç setleri ve veri platformları kullanarak bir üretim ortamına dağıtmak için seçenekler özetlenmektedir:

- [Azure Machine Learning](../service/index.yml)
- [SQL Server ML Hizmetleri ile](https://docs.microsoft.com/sql/advanced-analytics/r/r-services)
- [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server)


## 1. <a name='DataQualityReportUtility-1'></a> Araştırma 

Bir veri Bilimcisi keşfi ve çeşitli yollarla raporlama gerçekleştirebilirsiniz: kitaplıkları ve paketleri kullanılabilir Python (örneğin matplotlib) kullanarak veya R (ggplot veya örneğin kafes). Veri bilimcileri bu tür kod belirli senaryolar için veri İnceleme gereksinimlerine uyacak şekilde özelleştirebilirsiniz. Yapılandırılmış verileri ilgilenmekten sorumlu gereksinimlerini farklıdır, metin veya resimler gibi yapılandırılmamış veriler için. 

Azure Machine Learning hizmeti gibi ürünleri de sağlamak [gelişmiş veri hazırlama](../service/how-to-transform-data.md) veri denetimi ve araştırma, özellik oluşturma dahil. Kullanıcı Araçlar, kitaplıklar ve suite en iyi paketleri ihtiyaçlarını karar vermeniz gerekir. 

Bu aşamanın sonunda teslim edilebilir bir veri araştırma rapor eder. Rapor, veri modelleme için kullanılmak üzere oldukça kapsamlı bir görünüm ve veri modelleme adıma ilerlemek uygun olup'in bir değerlendirme sağlamanız gerekir. Yarı otomatik keşfi için aşağıdaki bölümlerde açıklanan Team Data Science işlem (TDSP) yardımcı programlar, modelleme ve raporlama da standartlaştırılmış veri keşfi ve modelleme raporlar sağlar. 

### <a name="interactive-data-exploration-analysis-and-reporting-using-the-idear-utility"></a>Etkileşimli veri keşfi, analiz ve raporlama IDEAR yardımcı programını kullanma

Bu markdown tabanlı R veya Python not defteri tabanlı bir hizmet değerlendirmek ve veri kümeleri keşfetmek için esnek ve etkileşimli bir araç sağlar. Kullanıcılar, hızlı bir şekilde minimal bir kodlama ile veri kümesinde raporlar oluşturabilirsiniz. Kullanıcıların etkileşimli aracında araştırma sonuçları istemciye teslim veya modelleme sonraki adımda eklenecek hangi değişkenler hakkında kararlar almak için kullanılan bir son rapor vermek için düğmeler tıklayabilirsiniz.

Şu anda aracı yalnızca veri çerçeveleri bellekte üzerinde çalışır. Bir YAML dosyası incelenecek veri kümesi parametrelerini belirtmek için gereklidir. Daha fazla bilgi için [TDSP veri bilimi yardımcı programları içinde IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/DataReport-Utils).


## 2. <a name='ModelingUtility-2'></a> Modelleme

Çok sayıda araç setleri ve çeşitli dillerde eğitim modellerinde paketleri vardır. Veri bilimcileri, doğruluk ve gecikme süresi ile ilgili performans değerlendirmeleri için ilgili iş memnun olduğu sürece, rahat oldukları tıklayacağınız olanları durumları ve üretim senaryolarında kullanmak ücretsiz gelecektir.

Sonraki bölümde, yarı otomatik modelleme için R tabanlı TDSP yardımcı kullanmayı gösterir. Bu AMAR yardımcı programı, taban çizgisinin modelleri hızla yanı sıra daha iyi gerçekleştirilmesi sağlamak üzere ayarlanması için gereken parametreler model oluşturmak için kullanılabilir.
Aşağıdaki model Yönetimi bölümünde, kaydetme ve birden çok modeli yönetmek için bir sistem olan işlemi gösterilmektedir.


### <a name="model-training-modeling-and-reporting-using-the-amar-utility"></a>Eğitim modeli: modelleme ve AMAR yardımcı programını kullanarak raporlama

[Otomatik modelleme ve Raporlama (AMAR) yardımcı programı](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/Modeling) hiper parametreli Süpürme ile model oluşturma ve bu modellerin kesinlik karşılaştırılacak bir özelleştirilebilir, yarı otomatik araç sağlar. 

Modeli oluşturma, farklı bölümler arasında kolay gezinebilmenizi içindekiler tablosu ile müstakil HTML çıktı oluşturmak için çalıştırılabilir bir R Markdown dosyası programıdır. Markdown dosyası (birbirine bağlı) çalıştırıldığında üç algoritmalar çalıştırılır: glmnet kullanarak regularized regresyon paketini, randomForest paketini kullanarak ve xgboost paketini kullanarak ağaçları artırma rastgele orman). Bu algoritmalar her eğitilen bir modelin üretir. Bu modellerin kesinlik ardından karşılaştırılır ve göreli özellik önem çizimleri raporlanır. Şu anda iki yardımcı programı vardır: biri için bir ikili sınıflandırma görevi ve regresyon görev için biridir. Bunlar arasındaki farklar olduğu şekilde denetim parametrelerini ve doğruluk ölçümler için bu öğrenme görevleri belirtilir. 

Bir YAML dosyası belirtmek için kullanılır:

- veri girişi (SQL kaynağı veya bir R-veri dosyası) 
- verilerin hangi kısmını, eğitim ve test etmek için hangi kısmını için kullanılır
- çalıştırmak için hangi algoritmaları 
- Denetim parametreleri modeli iyileştirme Seçimi:
    - Çapraz doğrulama 
    - önyükleme
    - Çapraz doğrulama, hatları
- Her bir algoritmanın için hiper parametreli ayarlar. 

Ayrıca algoritmaları, büyük Katlama sayısı için en iyi duruma getirme, hyper-parametreleriyle sayısı ve üzerinden süpürmek için hiper parametreli kümesi sayısı modelleri hızla çalıştırmak için Yaml dosyası içinde değiştirilebilir. Örneğin, daha az sayıda CV hatları, daha az sayıda parametre kümeleri ile çalıştırılabilir. Bunu verilmişse, bunlar ayrıca daha kapsamlı CV kat sayısı daha yüksek bir sayı veya çok sayıda parametre kümeleri ile çalıştırılabilir.

Daha fazla bilgi için [otomatik modelleme ve raporlama yardımcı programı'nda TDSP veri bilimi yardımcı programları](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/Modeling).

### <a name="model-management"></a>Model yönetimi
Birden çok modeli oluşturduktan sonra genellikle kaydetmek ve yönetim modellerine yönelik bir sistem olması gerekir. Genelde betiklerini veya API'leri ve arka uç veritabanı veya sürüm oluşturma sistemi gerekir. Bu yönetim görevleri için göz önünde bulundurun birkaç seçenek vardır:

1. [Azure Machine Learning - model Yönetimi Hizmeti](../service/index.yml)
2. [MIT gelen ModelDB](https://mitdbg.github.io/modeldb/) 
3. [Bir model yönetim sistemi olarak SQL server](https://blogs.technet.microsoft.com/dataplatforminsider/2016/10/17/sql-server-as-a-machine-learning-model-management-system/)
4. [Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

## 3. <a name='Deployment-3'></a> Dağıtım

Üretim dağıtımı, etkin bir rol bir iş yürütmek bir model sağlar. Dağıtılan bir modelde tahminleri iş kararları için kullanılabilir.

### <a name="production-platforms"></a>Üretim platformları
Çeşitli yaklaşımlar ve modelleri üretime koymak için Platform vardır. Bazı seçenekler şunlardır:


- [Azure Machine Learning hizmetindeki model dağıtımı](../service/how-to-deploy-and-where.md)
- [Bir modeli SQL Server dağıtımı](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

> [!NOTE]
> Dağıtımdan önce bir üretim ortamında kullanmak için düşük Puanlama modeli, gecikme süresi sağlamak üzere vardır.
>
>

Daha fazla örnek de işlem için tüm adımları gösteren talimatlara kullanılabilir **belirli senaryoları**. Listelenen ve küçük resim açıklamasında ile bağlantılı [örnek izlenecek yollar](walkthroughs.md) makalesi. Bunlar, bulut, şirket içi araçları ve Hizmetleri, bir iş akışı veya akıllı bir uygulama oluşturmak için işlem hattı birleştirme işlemini göstermektedir.

> [!NOTE]
> Azure Machine Learning Studio'yu kullanarak bir dağıtım için bkz [bir Azure Machine Learning web hizmetini dağıtma](../studio/publish-a-machine-learning-web-service.md).
>
>

### <a name="ab-testing"></a>A / B testi
Birden çok modelleri üretimde olduğunda yapmak yararlı olabilir [A / B testi](https://en.wikipedia.org/wiki/A/B_testing) modelleri performansını karşılaştırmak için. 

 
## <a name="next-steps"></a>Sonraki adımlar

[Veri bilimi projeleri ilerlemesini](track-progress.md) nasıl bir veri Bilimcisi bir veri bilimi projenizin ilerlemesini izleyebilirsiniz gösterir.

[Model işlemi ve CI/CD](ci-cd-flask.md) nasıl CI/CD ile geliştirilen modelleri gerçekleştirilebilir gösterir.


