---
title: Veri bilimi görevleri - Azure Machine Learning yürütme | Microsoft Docs
description: Bir veri Bilimcisi veri bilimi projesi nasıl trackable, yürütebilir sürüm denetimli ve işbirliği yolu.
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: deguhath
ms.openlocfilehash: 5c617c60ff51d0b1e7717b28b0372efe63c1ee18
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="execute-data-science-tasks-exploration-modeling-and-deployment"></a>Veri bilimi görevleri yürütün: keşfi, model ve dağıtım

Genel veri bilimi görevler veri keşfi, model oluşturma ve dağıtımını içerir. Bu makalede nasıl kullanılacağını gösterir **etkileşimli veri keşfi, analiz ve Raporlama (IDEAR)** ve **otomatik modelleme ve Raporlama (AMAR)** birçok ortak veri bilimi görevleri tamamlamak için yardımcı programlar Etkileşimli veri keşfi, veri analizi, raporlama ve model oluşturma gibi. Ayrıca, bir model çeşitli platformlardan araç takımları ve verileri, aşağıdaki gibi kullanarak bir üretim ortamında dağıtmak için seçenekleri özetlenmektedir:

- [Azure Machine Learning](../service/index.yml)
- [ML Hizmetleri ile SQL Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-services#in-database-analytics-with-sql-server)
- [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server)


## 1. <a name='DataQualityReportUtility-1'></a> Araştırması 

Bir veri Bilimcisi araştırması ve çeşitli şekillerde raporlama görevlerini gerçekleştirebilir: Python (örneğin matplotlib) için kitaplıkları ve paketleri kullanılabilir kullanarak veya r (ggplot veya örneğin kafes). Veri bilimcilerine kodun belirli senaryolar için veri keşfi gereksinimlerine uyacak şekilde özelleştirebilirsiniz. Yapılandırılmış verileri postalarla gereksinimlerini farklı metin veya görüntüler gibi yapılandırılmamış veriler için. 

Azure Machine Learning çalışma ekranı gibi ürünler de sağlar [veri hazırlığı Gelişmiş](../desktop-workbench/tutorial-bikeshare-dataprep.md) wrangling verileri ve özellik oluşturma dahil araştırması için. Kullanıcı araçlarını, kitaplıklarını ve paketi en iyi paketleri gereksinimlerine karar vermeniz gerekir. 

Bu aşama, sonunda teslim edilebilir veri araştırması rapor eder. Rapor model için kullanılacak veri oldukça kapsamlı bir görünüm ve verilerin modelleme adıma devam etmek uygun olup'in bir değerlendirme sağlaması gerekir. Yarı otomatik araştırması için aşağıdaki bölümlerde ele alınan takım veri bilimi işlem (TDSP) yardımcı programları modelleme ve raporlama ayrıca standartlaştırılmış veri keşfi ve raporları modelleme sağlar. 

### <a name="interactive-data-exploration-analysis-and-reporting-using-the-idear-utility"></a>Etkileşimli veri keşfi, analiz ve IDEAR yardımcı programını kullanarak raporlama

Bu R markdown tabanlı veya Python dizüstü bilgisayar tabanlı bir hizmet değerlendirmek ve veri kümelerini keşfetmek için esnek ve etkileşimli bir araç sağlar. Kullanıcıların hızlı bir şekilde minimal kodlama ile veri kümesinden raporları da oluşturabilirsiniz. Kullanıcıların etkileşimli aracı araştırması sonuçlarında istemcilere teslim veya sonraki model adımda eklemek için hangi değişkenleri hakkında kararlar almak için kullanılan bir son rapor vermek için düğmelerini tıklatabilirsiniz.

Şu anda aracı yalnızca veri çerçevelerini bellekte üzerinde çalışır. YAML dosya incelenecek veri kümesi parametrelerini belirtmek için gereklidir. Daha fazla bilgi için bkz: [TDSP veri bilimi yardımcı programları IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/DataReport-Utils).


## 2. <a name='ModelingUtility-2'></a> Modelleme

Çok sayıda araç takımları ve çeşitli dillerde eğitim modellerinde paketleri vardır. Veri bilimcilerine ve ever olanları doğruluğunu ve gecikme süresi ile ilgili performans değerlendirmeleri için ilgili iş karşılanır sürece, rahat oldukları durumlarda ve üretim senaryoları kullandığınız çekinmeyin.

Sonraki bölümde bir R tabanlı TDSP yardımcı programının yarı otomatik modelleme için nasıl kullanılacağı gösterilir. Bu AMAR yardımcı programı, taban çizgisinin modelleri hızla yanı sıra daha iyi gerçekleştirme sağlamak için ayarlanması gereken parametreleri model oluşturmak için kullanılabilir.
Aşağıdaki model yönetim bölümünden sistemi kaydetme ve birden fazla modeli yönetmek için nasıl gösterir.


### <a name="model-training-modeling-and-reporting-using-the-amar-utility"></a>Eğitim modeli: modelleme ve AMAR yardımcı programını kullanarak raporlama

[Otomatik modelleme ve Raporlama (AMAR) yardımcı programı](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/Modeling) özelleştirilebilir, yarı otomatik aracı modeli oluşturma parametresi hyper Süpürme ile gerçekleştirmek ve bu modelleri doğruluğunu karşılaştırmak için sağlar. 

Model oluşturma farklı bölümleri aracılığıyla kolay gezinme için içindekiler tablosu ile kendi içinde bulunan HTML çıkışını üretmek için çalıştırabilirsiniz bir R Markdown dosyası programıdır. Markdown dosyası (birbirine bağlı) çalıştırıldığında üç algoritmalar çalıştırılır: glmnet kullanarak regularized regresyon paketini, randomForest paketini kullanarak ve xgboost paketini kullanarak ağaçları artırmanın rastgele orman). Bu algoritmalar her bir modeli oluşturur. Bu modeller doğruluğunu karşılaştırılır ve göreli özelliği önem çizimleri bildirilir. Şu anda iki yardımcı programları vardır: biri için bir ikili sınıflandırma görev ve regresyon görev için biridir. Bunları birincil farklarını olduğu şekilde denetim parametrelerini ve doğruluk ölçümleri bu öğrenme görevler için belirtilir. 

YAML dosyası belirtmek için kullanılır:

- veri girişi (SQL kaynağı veya bir R veri dosyası) 
- verilerin hangi kısmını eğitim ve test etmek için ne bölümü için kullanılır
- çalıştırmak için hangi algoritmaları 
- model iyileştirme denetim parametrelerini Seçimi:
    - Çapraz doğrulama 
    - önyükleme eklemesi
    - Çapraz doğrulama Katlama
- hyper-parametresi için her algoritmasını ayarlar. 

Ayrıca algoritmalar, en iyi duruma getirme için Katlama sayısı, hyper-parametreleri sayısı ve üzerinden sweep için hyper-parametre kümesi sayısı modelleri hızlı bir şekilde çalıştırmak için Yaml dosyasında değiştirilebilir. Örneğin, daha az sayıda MS Katlama, parametre kümeleri daha düşük bir sayı ile çalıştırılabilir. Bunu verilmişse, bunlar ayrıca daha kapsamlı MS Katlama daha yüksek bir sayı veya çok sayıda parametre kümeleri ile çalıştırılabilir.

Daha fazla bilgi için bkz: [otomatik modelleme ve raporlama yardımcı programı'nda TDSP veri bilimi yardımcı programları](https://github.com/Azure/Azure-TDSP-Utilities/tree/master/DataScienceUtilities/Modeling).

### <a name="model-management"></a>Model yönetimi
Birden fazla modeli oluşturduktan sonra genellikle kaydetme ve modelleri yönetmek için bir sisteme sahip olmanız gerekir. Genellikle, komut dosyaları veya API'ler ve arka uç veritabanı veya sürüm oluşturma sistemi bileşimini gerekir. Bu yönetim görevleri için göz önünde bulundurabilirsiniz birkaç seçenek vardır:

1. [Azure Machine Learning - model yönetim hizmeti](../service/index.yml)
2. [ModelDB MIT gelen](https://mitdbg.github.io/modeldb/) 
3. [Bir model yönetim sistemi olarak SQL seerver](https://blogs.technet.microsoft.com/dataplatforminsider/2016/10/17/sql-server-as-a-machine-learning-model-management-system/)
4. [Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

## 3. <a name='Deployment-3'></a> Dağıtım

Üretim dağıtımı etkin bir rol bir işletmede yürütmek bir model sağlar. Dağıtılan modelden tahminleri iş kararları için kullanılabilir.

### <a name="production-platforms"></a>Üretim platformları
Çeşitli yaklaşımlar ve platformları, modelleri üretime sokmak için vardır. Bazı seçenekler şunlardır:


- [Azure Machine Learning modeli dağıtımında](../desktop-workbench/model-management-overview.md)
- [SQL Server'daki bir model dağıtımı](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)

>
>
>Not: dağıtımdan önce modeli Puanlama gecikme süresi üretimde kullanmak üzere düşük güvence altına almaya sahip.
>

Daha ayrıntılı örnekler için işlemdeki tüm adımlar gösteren talimatlara bulunan **belirli senaryolar**. Listelenen ve küçük resim açıklamasında ile bağlantılı [örnek izlenecek yollar](walkthroughs.md) makalesi. Bunlar, bulut, şirket içi araçları ve Hizmetleri bir iş akışı veya akıllı bir uygulama oluşturmak için ardışık düzen birleştirmek nasıl koruduğu gösterilmiştir.

Not: Azure Machine Learning Studio kullanarak dağıtım için bkz. [bir Azure Machine Learning web hizmetini dağıtma](../studio/publish-a-machine-learning-web-service.md).

### <a name="ab-testing"></a>A / B testi
Birden fazla modeli üretimde olduğunda gerçekleştirmek yararlı olabilir [A / B testi](https://en.wikipedia.org/wiki/A/B_testing) modelleri performansını karşılaştırmak için. 

 
## <a name="next-steps"></a>Sonraki adımlar

[Veri bilimi projeleri ilerlemesini izlemek](track-progress.md) nasıl veri Bilimcisi veri bilimi proje ilerlemesini izleyebilirsiniz gösterir.
 


