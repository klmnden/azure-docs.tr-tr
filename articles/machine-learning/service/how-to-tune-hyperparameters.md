---
title: Azure Machine Learning kullanarak modeliniz için ayarlama hiperparametreleri
description: Azure Machine Learning hizmetini kullanarak derin öğrenme / makine öğrenimi modeliniz için hiperparametreleri nasıl ayarlanacağını öğrenin. Parametre arama alanı tanımlayın, iyileştirin ve erken kötü performans gösteren yapılandırmaları gerçekleştirme sonlandırmak için birincil bir ölçüm belirtin nasıl görürsünüz.
ms.author: swatig
author: swatig007
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 205a4d454be02d64058e3d0fcffda35df5b831a5
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971686"
---
# <a name="tune-hyperparameters-for-your-model"></a>İçin model ayarlama hiperparametreleri

Bu makalede, hiperparametreleri modeliniz için verimli bir şekilde ayarlamak öğrenin. Parametre arama alanı tanımlayın, iyileştirin ve erken kötü performans gösteren yapılandırmaları gerçekleştirme sonlandırmak için birincil bir ölçüm belirtin nasıl görürsünüz. Ayrıca, çeşitli eğitim çalıştırmalarının görselleştirmek ve en iyi performansa sahip modelinizi için yapılandırmayı seçin.

## <a name="what-are-hyperparameters"></a>Hiperparametreleri nelerdir?
Hiperparametreleri Eğitim işlemini yöneten bir modeli eğitimi önce seçilen ayarlanabilir parametrelerdir. Örneğin, bir derin sinir ağı eğitiliyor önce gizli Ağ katmanlarında sayısı ve her katmandaki düğüm sayısını karar gerekecektir. Bu değerler bir eğitim işlemi sırasında genellikle sabit kalır.

Derin öğrenme / machine learning senaryolarda model performansını yoğun olarak seçili hiper parametre değerlerine bağlıdır. Amacı, Hiper parametre araştırma yer alan çeşitli hiper parametre yapılandırmalara istenen performansla sonuçlanır bir yapılandırma bulmak için arama gerçekleştirmektir. Genellikle, Hiper parametre araştırma arama alanı geniş ve her yapılandırma değerlendirmesinin hesaplıdır koşuluyla, büyük bir titizlikle el ile işlemidir.

Azure Machine Learning, bu hiper parametre araştırma verimli bir şekilde otomatik hale getirmek önemli zamandan ve kaynaklardan tasarruf sağlar. Araştırılacak hiper parametre değerleri aralığı belirtebilirsiniz ve bu İnceleme için eğitim en fazla çalıştırır. Sistem sonra otomatik olarak farklı parametre yapılandırmaları ile birden çok eşzamanlı eğitim çalıştırmalarının başlatır ve kullanıcı tarafından seçilen bir ölçüme göre ölçülen en iyi performansı sonuçları yapılandırma bulur. Zayıf performanslı eğitim çalıştırmalarının otomatik olarak erken sonlandırıldı, bilgi işlem kaynaklarının gereksiz azaltır. Bu kaynaklar, diğer hiper parametre yapılandırmalarını keşfetmek için bunun yerine kullanılır.

Azure Machine Learning hizmetini kullanarak modeliniz için hiperparametreleri ayarlamak için - aşağıdakileri yapmanız gerekir
* Hiper parametre arama alanı tanımlayın
* En iyi duruma getirmek için birincil bir ölçüm belirtin
* Bir erken sonlandırma ilkesini belirtin
* Hiper parametre ayarı için kaynakları ayırın.
* Yukarıdaki yapılandırma ile bir deneme başlatın

## <a name="define-the-hyperparameter-search-space"></a>Hiper parametre arama alanı tanımlayın
Azure Machine Learning hizmeti her hiper parametre için tanımlanan değerleri aralığı inceleyerek hiperparametreleri otomatik olarak ayarlar.

### <a name="types-of-hyperparameters"></a>Hiperparametreleri türleri
Her bir hiper parametre ayrık veya sürekli olarak ya da olabilir.

#### <a name="discrete-hyperparameters"></a>Ayrık hiperparametreleri 
Ayrık hiperparametreleri olarak belirtilebilir bir `choice` ayrık değerler arasında. `choice` ya da bir veya daha fazla virgülle ayrılmış değerler, bir `range` nesnesi, veya rastgele `list` nesne. Örneğin:  

```Python
    {    
        "batch_size": choice(16, 32, 64, 128)
        "number_of_hidden_layers": choice(range(1,5))
    }
```

Bu durumda, batch_size [16, 32, 64, 128] değerlerden birini alabilir ve number_of_hidden_layers [1, 2, 3, 4] değerlerden birini alabilir.

Bir dağıtım kullanarak gelişmiş ayrık hiperparametreleri de belirtilebilir. Aşağıdaki dağıtımlar desteklenir-
* `quniform(low, high, q)` -Hepsini gibi bir değer döndürür (Tekdüzen (düşük, yüksek) / s) * q
* `qloguniform(low, high, q)` -Hepsini gibi bir değeri döndürür (exp (tek düzen (düşük, yüksek)) / s) * q
* `qnormal(mu, sigma, q)` -Hepsini gibi bir değeri döndürür (normal (mu, sigma) / s) * q
* `qlognormal(mu, sigma, q)` -Hepsini gibi bir değeri döndürür (exp (normal (mu, sigma)) / s) * q

#### <a name="continuous-hyperparameters"></a>Sürekli hiperparametreleri 
Sürekli hiperparametreleri sürekli bir değerler aralığı içinde bir dağıtım belirtilebilir. Desteklenen dağıtımlar dahil-
* `uniform(low, high)` -Düşük ve yüksek arasında birörnek dağıtılmış bir değer döndürür
* `loguniform(low, high)` -Böylece dönüş değeri logaritmasını birörnek dağıtılmış exp (tek düzen (düşük, yüksek)) göre çizilmiş bir değer döndürür
* `normal(mu, sigma)` -Normalde ortalama mu ve standart sapma sigma ile dağıtılan gerçek bir değer döndürür
* `lognormal(mu, sigma)` -Dönüş değeri logaritmasını dağıtılması için normalde exp (normal (mu, sigma)) göre çizilmiş bir değer döndürür

İşte bir örnek parametre alanı tanımı-

```Python
    {    
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1)
    }
```

Bu örnek, bir arama alanı ile iki parametre - learning_rate ve keep_probability tanımlar. Ortalama değer 10 ve 3 standart sapmasını içeren bir normal dağıtım learning_rate olacaktır. keep_probability 0,05 ve en yüksek değeri 0,1 Tekdüzen bir dağıtım ile en az bir değere sahip olur.

### <a name="sampling-the-hyperparameter-space"></a>Hiper parametre alanı örnekleme
Kullanıcı ayrıca içinde belirtilen hiper parametre alanı tanımı kullanılacak parametre örnekleme yöntemini belirtir. Azure Machine Learning hizmeti rastgele örnekleme, kılavuz örnekleme ve Bayes örnekleme destekler.

#### <a name="random-sampling"></a>Rastgele örnekleme
Rasgele örnekleme, Hiper parametre değerleri tanımlanan arama alanının rastgele seçilir. Rastgele örnekleme, hem ayrık hem de sürekli hiperparametreleri içerecek şekilde arama alanı sağlar. Örneğin:

```Python
from azureml.train.hyperdrive import RandomParameterSampling
param_sampling = RandomParameterSampling( {
        "learning_rate": normal(10, 3),
        "keep_probability": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

#### <a name="grid-sampling"></a>Kılavuz örnekleme
Kılavuz örnekleme, tanımlı bir arama alanı, tüm uygun değerleri üzerinde bir basit kılavuz arama gerçekleştirir. Yalnızca kullanarak belirtilen hiperparametreleri ile kullanılabilir `choice`. Örneğin, aşağıdaki alana toplam altı örneği vardır-

```Python
from azureml.train.hyperdrive import GridParameterSampling
param_sampling = GridParameterSampling( {
        "num_hidden_layers": choice(1, 2, 3),
        "batch_size": choice(16, 32)
    }
)
```

#### <a name="bayesian-sampling"></a>Bayes örnekleme
Bayes örnekleme Bayes iyileştirme algoritması üzerinde temel alır ve ardından örneklemek için hiper parametre değerleri akıllı seçimlerini yapar. Bu, bu örnek nasıl önceki örnekleri gerçekleştirilen, gibi yeni örnek bildirilen birincil Metrik artırır göre seçer.

Bayes örnekleme kullanırken, eş zamanlı çalıştırma sayısını ayarlama işleminin verimliliğini üzerinde bir etkisi yoktur. Genellikle, daha az sayıda eşzamanlı çalışmanın için daha iyi örnekleme yakınsama neden olabilir. Bu durum, daha küçük bir paralellik derecesi önceden tamamlanmış çalıştırmalardan fayda çalıştırmalarının sayısı artar çünkü.

Bayes örneklemeyi destekleyip yalnızca `choice` ve `uniform` arama alanı üzerinden dağıtımları. Örneğin: 

```Python
from azureml.train.hyperdrive import BayesianParameterSampling
param_sampling = BayesianParameterSampling( {
        "learning_rate": uniform(0.05, 0.1),
        "batch_size": choice(16, 32, 64, 128)
    }
)
```

> [!NOTE]
> Bayes örnekleme, herhangi bir erken sonlandırma ilke şu anda desteklemiyor (bkz [bir erken sonlandırma ilkesini belirtin](#specify-an-early-termination-policy)). Bayes parametre örnekleme kullanıyorsanız, ilke ayarlamalısınız `None`. Bir sonlandırma ilke ile Bayes örnekleme belirtmek değil, aynı etkiye sahiptir.
>
> ```Python
> early_termination_policy = None
> ```

## <a name="specify-a-primary-metric-to-optimize"></a>En iyi duruma getirmek için birincil bir ölçüm belirtin
Ayarlama hiperparametreleri, denemeyi en iyi duruma getirmeyi ayarlama hiper parametre istediğiniz birincil ölçümü belirtmenize gerek. Her bir eğitim çalıştırmanın bu birincil bir ölçüm için değerlendirilir ve düzgün çalışır (burada birincil ölçüm ve Erken sonlandırma ilke tarafından ayarlanan ölçütlere uymuyor) gerçekleştirme sonlandırılacak. Birincil ölçüm adı belirterek yanı sıra, ayrıca amacı, en iyi duruma getirme - belirtmenize gerek en üst düzeye çıkarmak veya birincil ölçüm en aza indirin.
* `primary_metric_name`: En iyi duruma getirmek için birincil ölçüm adı. Birincil ölçüm adı bir eğitim betiği tarafından günlüğe ölçüm adı tam olarak eşleşmesi gerekir. Bkz: [oturum hiper parametre ayarı için ölçümleri](#log-metrics-for-hyperparameter-tuning).
* `primary_metric_goal`: Kaynaklar şunlar olabilir bu `PrimaryMetricGoal.MAXIMIZE` veya `PrimaryMetricGoal.MINIMIZE` ve birincil Metrik ekranı veya kaldırılacak çalıştırmalar değerlendirirken simge durumuna küçültülmüş olup olmadığını belirler. 

Örneğin-
```Python
primary_metric_name="accuracy",
primary_metric_goal=PrimaryMetricGoal.MAXIMIZE
```
Bu, "doğruluğu" en üst düzeye çıkarmak için çalışmalardan iyileştirir.

### <a name="log-metrics-for-hyperparameter-tuning"></a>Hiper parametre ayarı için ölçümleri günlüğe kaydedin
Azure Machine Learning hizmetinin hiper parametre ayarı için kullanmak için eğitim betiği modeliniz için model çalışırken ilgili ölçümleri raporu gerekecektir. Kullanıcının çalışma performans değerlendirmesi için kullanılacak hizmeti istedikleri birincil Metrik belirtir ve eğitim betiğini Bu ölçüm bağlanmanız gerekir. Bkz: [iyileştirmek için birincil bir ölçüm belirtin](#specify-a-primary-metric-to-optimize).

Bu ölçüm oturum eğitim betiğinizi güncelleştirebilirsiniz aşağıdaki örnek kod parçacığını - kullanarak

```Python
from azureml.core.run import Run
run_logger = Run.get_submitted_run()
run_logger.log("accuracy", float(val_accuracy))
```

Bu örnekte, eğitim betiğini hesaplar `val_accuracy` ve "birincil ölçüm kullanılan bu doğruluk" günlüğe kaydeder. Bu, genellikle bu ölçüm bildirme belirlemek için model geliştiricisi kadar kadar.

## <a name="specify-an-early-termination-policy"></a>Bir erken sonlandırma ilkesini belirtin
Ayarlama hiperparametreleri, hatalı çalıştırmalar gerçekleştirmek için Azure Machine Learning hizmetini kullanarak olduğunda otomatik olarak erken sonlandırıldı. Bu kaynakların atık azalır ve bunun yerine bu kaynakları diğer parametre yapılandırmalarını keşfetmek için kullanır.

Bir erken sonlandırma ilkesini kullanarak, bir kullanıcı bir ilke uygulandığında denetleyen aşağıdaki parametreleri yapılandırabilirsiniz-
* `evaluation_interval`: ilkeyi uygulamak için sıklığı. Her birincil Metrik eğitim betiği oturum açtığında bir aralık olarak sayılır. Bu nedenle bir `evaluation_interval` birincil Metrik eğitim betiğini raporları her zaman 1 ilke uygulanır. Bir `evaluation_interval` 2 eğitim betiğini birincil Metrik raporları her zaman ilke uygulanır. Bu isteğe bağlı bir parametredir ve belirtilmezse `evaluation_interval` varsayılan olarak 1 olarak ayarlanır.
* `delay_evaluation`: belirtilen sayıda aralıkları için ilk ilke değerlendirmesi geciktirir. Bu eğitim çalıştırmaları, erken sonlandırma önleme aralıkları, ilk en düşük numaralı çalıştırmak tüm yapılandırmaları sağlayan isteğe bağlı bir parametredir. Bu seçenek belirtilmişse, büyüktür veya eşittir delay_evaluation evaluation_interval her birden çok ilke uygulanır.

Azure Machine Learning hizmeti aşağıdaki erken sonlandırma ilkelerini destekler-

### <a name="bandit-policy"></a>Bandit İlkesi
Bandit ilke slack çarpanı/slack tutar ve değerlendirme aralığına dayalı bir sonlandırma ilkesidir. Bu ilkeyi burada birincil Metrik içinde belirtilen slack etken olmadığı durumlar / slack miktarı en iyi performansa sahip eğitim göre çalıştır her çalıştırma erken sona erer. Aşağıdaki yapılandırma parametrelerini sürdüğünü-
* `slack_factor` veya `slack_amount`: en iyi performansa sahip çalıştırma eğitim göre izin verilen slack. `slack_factor` izin verilen slack oranı belirtir. `slack_amount` izin verilen slack oranı yerine bir mutlak miktarını belirtir.

    Örneğin, aralığı 10 uygulanmakta Bandit ilke göz önünde bulundurun. En iyi 10 aralıkla çalışması gerçekleştiren bir birincil Metrik 0.8 birincil ölçüm en üst düzeye çıkarmak için bir hedef bildirilen varsayılır. İlke ile belirtildiyse bir `slack_factor` 0.2 herhangi bir eğitim çalıştığında, 10 aralığında olan en iyi ölçüm 0.66 küçüktür (0,8 / (1 +`slack_factor`)) sonlandırılacak. Bunun yerine, ilke ile belirtilmemişse bir `slack_amount` 0.2 herhangi bir eğitim çalıştığında, 10 aralığında olan en iyi ölçüm 0,6 küçüktür (0,8 - `slack_amount`) sonlandırılacak.
* `evaluation_interval`: (isteğe bağlı parametresi) ilkesini uygulamak için sıklığı.
* `delay_evaluation`: belirtilen sayıda aralıkları (isteğe bağlı parametresi) için ilk ilke değerlendirmesi geciktirir.

Bu örnekte düşünün:

```Python
from azureml.train.hyperdrive import BanditPolicy
early_termination_policy = BanditPolicy(slack_factor = 0.1, evaluation_interval=1, delay_evaluation=5)
```

Bu örnekte, ölçümleri bildirildiğinde 5 değerlendirmesi aralıkta başlangıç erken sonlandırma ilke her aralıkta uygulanır. Herhangi çalıştırın, en iyi bir ölçümdür değerinden (1/(1+0.1) veya 91 sonlandırılması % en iyi performansa çalışma olacaktır.

### <a name="median-stopping-policy"></a>ORTANCA ilke durduruluyor
ORTANCA durdurma ortalamalar çalıştırıcıları tarafından bildirilen birincil ölçüm üzerinde çalışan temel bir erken sonlandırma İlkesi ilkesidir. Bu ilke, tüm eğitim çalıştırmaları arasında çalışan ortalamaları hesaplar ve çalışan ortalamalar, Orta başarımını kötüsü çalıştırmaları sonlandırır. Bu ilke aşağıdaki yapılandırma parametrelerini alır-
* `evaluation_interval`: (isteğe bağlı parametresi) ilkesini uygulamak için sıklığı.
* `delay_evaluation`: belirtilen sayıda aralıkları (isteğe bağlı parametresi) için ilk ilke değerlendirmesi geciktirir.

Bu örnekte düşünün:

```Python
from azureml.train.hyperdrive import MedianStoppingPolicy
early_termination_policy = MedianStoppingPolicy(evaluation_interval=1, delay_evaluation=5)
```

Bu örnekte, değerlendirme aralığı 5 başlayarak her bir aralıkta erken sonlandırma ilke uygulanır. Kendi en iyi birincil Metrik aralık 1:5 tüm eğitim çalıştırmaları arasında çalışan ortalamalar, Orta kötüsü, bir farklı çalıştır 5 aralığında sonlandırılacak.

### <a name="truncation-selection-policy"></a>Kesme seçimi ilkesi
Her değerlendirme aralıklarla, en düşük performanslı belirtilen bir yüzdenin çalıştıran kesilmesi seçimi ilke iptal eder. Çalıştırmaları performanslarını birincil Metrik üzerinde göre karşılaştırılır ve en düşük %x sonlandırılır. Aşağıdaki yapılandırma parametrelerini sürdüğünü-
* `truncation_percentage`: her değerlendirme aralıkta sonlandırmak için en düşük performanslı yüzdesi çalıştırır. Bu, 1 ile 99 arasında bir tamsayı değeri olmalıdır.
* `evaluation_interval`: (isteğe bağlı parametresi) ilkesini uygulamak için sıklığı.
* `delay_evaluation`: belirtilen sayıda aralıkları (isteğe bağlı parametresi) için ilk ilke değerlendirmesi geciktirir.

Bu örnekte düşünün:

```Python
from azureml.train.hyperdrive import TruncationSelectionPolicy
early_termination_policy = TruncationSelectionPolicy(evaluation_interval=1, truncation_percentage=20, delay_evaluation=5)
```

Bu örnekte, değerlendirme aralığı 5 başlayarak her bir aralıkta erken sonlandırma ilke uygulanır. 5 aralığında tüm çalıştırmalar performansının düşük %20 performansını 5 aralığında ise, bir farklı çalıştır 5 aralığında sonlandırılacak.

### <a name="no-termination-policy"></a>Sonlandırma İlkesi yok
Tüm eğitim çalıştırmalarının çalıştırılıp, NoTerminationPolicy kullanmak istiyorsanız. Bu, herhangi bir erken sonlandırma ilke uygulanarak değil bir etkisi olmaz. Örneğin: 

```Python
from azureml.train.hyperdrive import NoTerminationPolicy
early_termination_policy = NoTerminationPolicy()
```

### <a name="default-policy"></a>Varsayılan ilke
İlke yok belirtilirse, hizmet ayarlama hiper parametre ORTANCA durdurma ilkesiyle kullanacağı `evaluation_interval` 1 ve `delay_evaluation` varsayılan olarak 5. Yaklaşık % 25-%35 tasarruf kaybı olmadan ile birincil ölçüm (değerlendirme verilerimizi göre) üzerinde sağlayabilen koruyucu ayarları şunlardır.

## <a name="allocate-resources-for-hyperparameter-tuning"></a>Hiper parametre ayarı için kaynakları ayırın.
Eğitim çalıştırmalarının ve isteğe bağlı olarak, en uzun süreyi (dakika cinsinden) deneme ayarlama, Hiper parametre için maksimum toplam sayısını belirleyerek deneme ayarlama, Hiper parametre için Kaynak bütçenizi kontrol edebilirsiniz. 
* `max_total_runs`: Oluşturulacak eğitim çalıştırmalarının en fazla toplam sayısı. Bu bir üst sınır - hiper parametre alanı sınırlıdır ve daha az örnek varsa biz daha az çalıştığında, örneğin, olabilir. 1 ile 1000 arasında bir sayı olmalıdır.
* `max_duration_minutes`: Deneme dakikalar içinde ayarlama hiper parametre en uzun süresi. Bu isteğe bağlı bir parametredir ve varsa, bu süreden sonra çalışıyor olabilecek tüm çalıştırmalar otomatik olarak iptal edilir.

>[!NOTE] 
>Her iki `max_total_runs` ve `max_duration_minutes` belirtilirse, bu iki eşik ilk ulaşıldığında, deneme ayarlama hiper parametre sonlandırılır.

Ayrıca, arama ayarlama eşzamanlı olarak, Hiper parametre sırasında çalıştırılacak çalıştırma eğitim maksimum sayısı belirtebilirsiniz.
* `max_concurrent_runs`: Bu çalıştırma sayısı üst sınırı olan eşzamanlı olarak belirli bir andaki çalıştırılacak. Hiçbiri belirtilmişse, tüm `max_total_runs` paralel olarak başlatılır. Belirtilirse, 1 ile 100 arasında bir sayı olmalıdır.

>[!NOTE] 
>Belirtilen işlem hedefte kullanılabilir kaynaklar eş zamanlı çalıştırma sayısını Geçitli. Bu nedenle, işlem hedef istenen eşzamanlılık için kullanılabilir kaynaklar olduğundan emin olmak gerekir.

Bu örnekte gösterildiği gibi hiper parametre ayarı için kaynakları ayırabilirsiniz-
```Python
max_total_runs=20,
max_concurrent_runs=4
```
Bu, en fazla 4 yapılandırmaları aynı anda çalışan 20 toplam çalıştırma kullanmak için deneme ayarlama hiper parametre yapılandıracaksınız.

## <a name="configure-your-hyperparameter-tuning-experiment"></a>Deneme ayarlama, Hiper parametre yapılandırma
Yukarıdaki bölümlerde tanımlanan hyperpameter arama alanı, erken sonlandırma İlkesi, birincil ölçüm ve kaynak ayırmayı kullanarak deneme ayarlama, Hiper parametre yapılandırabilirsiniz. Ayrıca, sağlamak ihtiyacınız olacak bir `estimator` ile örneklenen hiperparametreleri çağrılır. `estimator` Eğitim betiği çalıştırarak, kaynakları (tek veya birden çok gpu) iş başına ve kullanmak için işlem hedef açıklanmaktadır. Deneme ayarlama, Hiper parametre kullanılabilir kaynakları Geçitli için eşzamanlılık beri işlem hedef içinde belirtilen emin olmak ihtiyacınız olacak `estimator` istenen eşzamanlılık yeterli kaynaklara sahip. (Bkz [bağlantı](/how-to-train-ml-models.md) estimators hakkında daha fazla bilgi için).

Aşağıda, Hiper parametre ayarlama deneyinizi nasıl yapılandırabileceğiniz bir örnek-

```Python
from azureml.train.hyperdrive import HyperDriveRunConfig
hyperdrive_run_config = HyperDriveRunConfig(estimator=estimator,
                          hyperparameter_sampling=param_sampling, 
                          policy=early_termination_policy,
                          primary_metric_name="accuracy", 
                          primary_metric_goal=PrimaryMetricGoal.MAXIMIZE,
                          max_total_runs=100,
                          max_concurrent_runs=4)
```

## <a name="submit-your-hyperparameter-tuning-experiment"></a>Deneme ayarlama, Hiper parametre gönderin
Yapılandırma ayarlamalarına, Hiper parametre tanımladıktan sonra bir denemeyi göndermek bu yapılandırma - kullanma

```Python
from azureml.core.experiment import Experiment
experiment = Experiment(workspace, experiment_name)
hyperdrive_run = experiment.submit(hyperdrive_run_config)
```

Burada `experiment_name` keşfedin, ayarlama, Hiper parametre atamak istediğiniz adıdır ve `workspace` istediğiniz bir deneme oluşturmak çalışma alanı (bkz [bağlantı](/concept-azure-machine-learning-architecture.md) denemeler hakkında daha fazla bilgi için).

## <a name="visualize-your-hyperparameter-tuning-experiment"></a>Deneme ayarlama, Hiper parametre görselleştirin
Azure Machine Learning SDK, eğitim çalıştırmalarının ilerleme durumunu görselleştirmek için kullanılan bir not defteri pencere öğesi sağlar. Aşağıdaki kod parçacığı, tüm hiper parametre ayarlama çalıştırmalarınızı tek bir yerden görselleştirmek için kullanılabilir-

```Python
from azureml.train.widgets import RunDetails
RunDetails(hyperdrive_run).show()
```

Eğitim çalıştırmalarının ayrıntılarını içeren bir tablo her hiper parametre yapılandırmaları için bu görüntüler. Örneğin:

![Hiper parametre ayar tablosu](media/how-to-tune-hyperparameters/HyperparameterTuningTable.png)

Ayrıca, her birinin çalıştırmalar performans eğitim ilerledikçe görselleştirebilirsiniz. Örneğin:

![Hiper parametre ayarlama çizimi](media/how-to-tune-hyperparameters/HyperparameterTuningPlot.png)

Son olarak, performans ve bir paralel koordinatları çizim kullanarak tek tek hiperparametreleri değerleri arasındaki bağıntıyı görsel olarak belirleyebilirsiniz. Örneğin: 

![Hiper parametre paralel koordinatları ayarlama](media/how-to-tune-hyperparameters/HyperparameterTuningParallelCoordinates.png)

Alternatif olarak, Azure web portalında da çalışır ayarlama tüm, Hiper parametre görselleştirebilirsiniz. Bkz: [bağlantı](/how-to-track-experiments.md/#view-the-experiment-in-the-web-portal) deneme web portalında görüntüleme hakkında daha fazla bilgi için. Örneğin,-

![Hiper parametre ayarlama portalı](media/how-to-tune-hyperparameters/HyperparameterTuningPortal.png)

## <a name="find-the-configuration-that-resulted-in-the-best-performance"></a>En iyi performans sonuçlanan yapılandırması bulma
Tüm çalıştırmalar ayarlama hiper parametre tamamladıktan sonra en iyi performansa sahip yapılandırma ve aşağıdakileri kullanarak karşılık gelen hiper parametre değerlerini tanımlayabilirsiniz. kod parçacığı -

```Python
best_run = hyperdrive_run.get_best_run_by_primary_metric()
best_run_metrics = best_run.get_metrics()
parameter_values = best_run.get_details()['runDefinition']['Arguments']

print('Best Run Id: ', best_run.id)
print('\n Accuracy:', best_run_metrics['accuracy'])
print('\n learning rate:',parameter_values[3])
print('\n keep probability:',parameter_values[5])
print('\n batch size:',parameter_values[7])
```

## <a name="sample-notebook"></a>Örnek Not Defteri
Başvurun 
* `training/03.train-hyperparameter-tune-deploy-with-tensorflow/03.train-hyperparameter-tune-deploy-with-tensorflow.ipynb` hiperparametreleri Tensorflow modeli için ayarlama hakkında bir öğretici için. 

Bu not alın:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Bir deney izleyin](/how-to-track-experiments.md)
* [Eğitilen model dağıtma](/how-to-deploy-and-where.md)
