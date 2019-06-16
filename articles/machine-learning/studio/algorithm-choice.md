---
title: Algoritmaları seçme
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio algoritmaları için denetimli ve Denetimsiz öğrenme kümeleme, sınıflandırma veya regresyon denemeleri nasıl seçeceğinizi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-ms.author=pakalra, previous-author=pakalra
ms.date: 03/04/2019
ms.openlocfilehash: 3bb88f2f9546ec25433061a0704bd144730bd34c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60752981"
---
# <a name="how-to-choose-algorithms-for-azure-machine-learning-studio"></a>Azure Machine Learning Studio için algoritma seçme

"Hangi makine öğrenme algoritmasına kullanmam gerekir?" sorusuna verilen yanıt her zaman "Duruma göre değişir." şeklindedir. Bu boyut, kalite ve verilerin niteliğine bağlıdır. Bu, Yanıtla yapmak istediğiniz üzerinde bağlıdır. Bu, nasıl algoritmasının matematik kullanmakta olduğunuz bilgisayar için yönergeler içine çevrilmiştir üzerinde bağlıdır. Ve duruma göre değişir üzerinde ne kadar süre sahip. Hatta en deneyimli veri uzmanları, denemeden önce en iyi algoritmayı gerçekleştirecek bildiremez.

Machine Learning Studio, Microsoft Research'te geliştirilen Ölçeklenebilir Artırılmış Karar ağaçları, Bayes Önerisi sistemleri, Derin Sinir Ağları ve Karar Ormanları gibi teknoloji harikası algoritmaları sağlar. Vowpal Wabbit gibi ölçeklenebilir açık kaynaklı makine öğrenimi paketleri de dahildir. Machine Learning Studio, çok sınıflı ve ikili sınıflandırma, regresyon ve kümeleme için makine öğrenimi algoritmalarını destekler. Tam listesi görmek [Machine Learning modüllerinin](/azure/machine-learning/studio-module-reference/index).
Belgeler, her bir algoritmanın ve parametreler, kullanımınız için algoritmayı en iyi duruma getirmek için nasıl ayarlanacağını hakkında bazı bilgiler sağlar.  


## <a name="the-machine-learning-algorithm-cheat-sheet"></a>Machine Learning algoritmasını kural sayfası

**[Microsoft Azure Machine Learning Studio algoritma kural sayfası](algorithm-cheat-sheet.md)** makine öğrenme algoritmasını Azure Machine Learning, Tahmine dayalı analiz çözümleri için sağdaki seçtiğiniz yardımcı olur Studio kitaplığı algoritmaları.
Bu makalede bu kopya kağıdı kullanma hakkında bilgi vermektedir.

> [!NOTE]
> İpuçlarını indirin ve bu makaleyi izlemek için Git [makine öğrenimi algoritma bilgi sayfasına için Microsoft Azure Machine Learning Studio](algorithm-cheat-sheet.md).
> 
> 

Bu kural sayfası çok belirli bir kitleyi göz önünde vardır: algoritma Azure Machine Learning Studio'da çalışmaya başlamak seçme çalışılırken başına veri uzmanı Lise düzeyi machine learning ile. Bu, bazı genelleştirmesi ve oversimplifications kolaylaştırır, ancak güvenli bir yönde işaret eden anlamına gelir. Ayrıca, burada listelenmeyen algoritmaları çok sayıda olduğu anlamına gelir.

Bu, derlenmiş geri bildirim ve ipuçları birçok veri bilimcileri ve machine learning uzmanlar önerilerdir. Biz üzerinde her şeyi kabul etmememiz ancak biz müşterilerimizin düşünceleri son derece kaba bir fikir birliğine varılmış harmonize girişimi yaptınız. "Duruma göre değişir ile..." çözünürlüğünün bilgilerinin çoğunu başlayın

### <a name="how-to-use-the-cheat-sheet"></a>Kopya kağıdı kullanma

Grafik olarak yolu ve algoritma etiketleri okuyun "için  *&lt;yol etiketinin&gt;* , kullanın  *&lt;algoritması&gt;* ." Örneğin, "için *hızı*, kullanın *lojistik regresyon iki sınıf*." Bazen birden fazla dal için geçerlidir.
Bazen bunların hiçbiri tam olarak uygun değil. Bunlar tam olan bu konuda endişelenmeyin için thumb kural önerileri olacak şekilde tasarlanmıştır.
İle söz konusu konuştuk birkaç veri bilimcileri en iyi algoritmayı bulmak için yalnızca emin tümünü denemek için yoludur.

İşte bir örnek [Azure AI Gallery](https://gallery.azure.ai/) aynı verilere karşı çeşitli algoritmalar çalışır ve sonuçları karşılaştıran bir deneme: [Çok sınıflı sınıflandırıcılar karşılaştırın: Harfli tanıma](https://gallery.azure.ai/Details/a635502fc98b402a890efe21cec65b92).

> [!TIP]
> Anlaşılması kolay bilgi grafiği machine learning temel bilgileri yaygın machine learning soruları yanıtlamak için kullanılan popüler algoritmaları hakkında bilgi edinmek için genel bakış indirmek için bkz [makine öğrenimi algoritma örnekleri ile Temelleri](basics-infographic-with-algorithm-examples.md).

## <a name="flavors-of-machine-learning"></a>Machine Learning model olarak sağlanır

### <a name="supervised"></a>Denetimli

Denetimli öğrenme algoritmalarını örnekler kümesine göre tahminlerde. Örneğin, geçmiş hisse senedi fiyatlarına gelecekteki fiyatları hakkında tahmin yapmak için kullanılabilir. Eğitim için kullanılan her bir örnekte ilgi değeri ile etiketlenir — Bu hisse senedi fiyatının durumda. Bu değer etiketleri desenleri denetimli öğrenme algoritmasını arar. İlgili olabilecek herhangi bir bilgi kullanabilirsiniz — gün haftanın, döneminde, şirketin finansal verileri, sektör türünü, jeopolitik aksatıcı olayları varlığını — ve her bir algoritmanın desenleri farklı türleri için görünür. Algoritma en iyi düzeni bulduktan sonra alabilir, etiketsiz sınama veri tahmininde bulunmak amacıyla bu düzeni kullanır: yarının fiyatlar.

Denetimli öğrenme, machine learning, popüler ve kullanışlı bir türüdür. Bunun tek istisnası, tüm modülleri Azure Machine Learning Studio'da öğrenimi algoritmaları denetimli olduğu. Azure Machine Learning Studio içinde temsil birkaç belirli denetimli öğrenme tür vardır: Sınıflandırma, regresyon ve anomali algılama.

* **Sınıflandırma**. Verileri bir kategori tahmin etmek için kullanılan, denetimli öğrenme sınıflandırma da adlandırılır. Bu görüntü resmini 'cat' veya 'dog' atarken durumdur. Yalnızca iki seçenek olduğunda çağrılır **iki sınıflı** veya **binom sınıflandırma**. Olarak olduğunda daha fazla kategori NCAA Mart TURNUVASI Turnuva kazanan tahmin olduğunda, bu sorunu olarak bilinir **çok sınıflı sınıflandırma**.
* **Regresyon**. Bir değer, hisse senedi fiyatlarına gibi ile tahmin edildiğinde denetimli öğrenme regresyon çağrılır.
* **Anomali algılama**. Bazen yalnızca olağan dışı veri noktalarını tanımlamak üzere hedeftir. Sahtekarlık algılama, örneğin, şüpheli tüm son derece alışılmışın dışında kredi kartı harcama desenleri alır. Olası çeşitlemeleri kadar çok sayıda ve bu nedenle birkaç sahte hangi etkinlik öğrenmek için uygun değil, eğitim örnekleri gibi görünen. Anomali algılama alan yalnızca normal hangi etkinlik (sahte olmayan işlemleri geçmişini kullanarak gibi) görünümünü öğrenin ve önemli ölçüde farklı olan her şeyi tanımlamak için bir yaklaşımdır.

### <a name="unsupervised"></a>Denetimsiz

Denetimsiz öğrenme içinde bunlarla ilgili hiçbir etiket veri noktanız olması. Bunun yerine, Denetimsiz öğrenme algoritmasının bir şekilde verileri düzenleme veya yapısını tanımlamak için hedeftir. Bu kümeler halinde gruplandırmak veya basit ya da daha düzenli görünür bir karmaşık verilere baktığımızda, farklı yöntemleri sürekli düzende bulmak anlamına gelebilir.

### <a name="reinforcement-learning"></a>Pekiştirmeye dayalı öğrenme

Öğrenme pekiştirmeye içinde yanıt her veri noktası için bir eylem seçmek üzere algoritma alır. Öğrenme algoritmasını ödül sinyal kısa kararı ne kadar iyi olduğunu gösteren bir süre daha da alır.
Algoritma bunu temel alarak, yüksek ödül elde etmek için kendi stratejisi değiştirir. Şu anda bir öğrenme algoritması modülleri Azure Machine Learning Studio'da yok pekiştirmeye vardır. Burada zaman içinde bir noktadaki sensör okumaları kümesini bir veri noktasıdır ve algoritma robot'ın sonraki eylem seçmelisiniz robotlara ilişkin pekiştirmeye dayalı öğrenme yaygındır. Ayrıca, bir doğal uygulamaları nesnelerin Internet'için uygun değildir.

## <a name="considerations-when-choosing-an-algorithm"></a>Bir algoritma seçerken dikkat edilmesi gerekenler

### <a name="accuracy"></a>Doğruluğu

En doğru yanıta olası alma her zaman gerekli değildir.
Bazen bir yaklaştırma kullanmak istediğinize bağlı olarak, yeterli olur. Bu durumda, işleme süresini önemli ölçüde daha fazla yaklaşık yöntemleriyle kalmanız tarafından Kes mümkün olabilir. Daha fazla yaklaşık yöntemlerden başka bir avantajı, doğal olarak overfitting önlemek için eğilimindedir olmasıdır.

### <a name="training-time"></a>Eğitim süresini

Kaç dakika veya saat bir modeli eğitmek için gereken büyük miktarda algoritmaları arasında değişir. Zaman eğitim genellikle yakından bağlı tutarlılık — bir genellikle eşlik eden diğer. Ayrıca, bazı algoritmalar diğerlerinden daha hassas veri noktası sayısı.
Zaman sınırlı olduğunda özellikle veri kümesi büyük olduğunda seçimi algoritması yönlendirebilirsiniz.

### <a name="linearity"></a>Doğrusallık

Makine öğrenimi algoritmaları çok sayıda olun Doğrusallık kullanın. Düz çizgi (veya daha büyük boyutlu kendi analog) sınıfları ayrılabilir doğrusal sınıflandırma algoritmaları varsayılır. Bunlar Lojistik regresyon ve vektör makineleri (Azure Machine Learning Studio'da uygulanmış gibi) desteği içerir.
Doğrusal regresyon algoritmaları, veri eğilimleri düz bir çizgi izleyin varsayılır. Bu varsayımları bazı sorunlar için hatalı değildir, ancak bazılarında, doğruluk kapatmaya.

![Doğrusal olmayan sınıf sınırı](./media/algorithm-choice/image1.png)

***Doğrusal olmayan sınıf sınır*** *-bağlı olan bir doğrusal bir sınıflandırma algoritmasıdır üzerinde düşük doğruluk sonuçlanır*

![Doğrusal bir eğilim verileri](./media/algorithm-choice/image2.png)

***Doğrusal bir eğilim verilerle*** *-bir doğrusal regresyon yöntemi kullanarak daha büyük kadar hataları oluşturmak*

Kendi tehlikeleri rağmen doğrusal algoritmaları bir saldırı ilk satırı olarak çok fazla kullanılır. Bunlar algorithmically basit ve hızlı eğitmek için olma eğilimindedir.

### <a name="number-of-parameters"></a>Parametre sayısı

Bir algoritma ayarlarken açmak için bir veri Bilimcisi alır düğmelerini parametrelerdir. Bunlar, hata toleransı veya yineleme veya seçenekleri, algoritmanın nasıl davranacağını çeşitler arasında sayısı gibi algoritması'nın davranışını etkileyen sayılardır. Eğitim süresini ve algoritma doğruluğunu bazen yalnızca doğru ayarları almak için oldukça önemli olabilir. Genellikle, çok sayıda parametre algoritmalar en iyi deneme ve iyi bir kombinasyonu bulmak için hata gerektirir.

Alternatif olarak, var olan bir [parametresi Süpürme](algorithm-parameters-optimize.md) Azure Machine Learning Studio'da hangi ayrıntı düzeyi seçtiğiniz tüm parametre birleşimlerini otomatik olarak çalışan modülü blok. Bu parametre alanı yayılmış emin olmak için harika bir yoludur ancak, bir modeli eğitmek için gereken süreyi, parametre sayısıyla birlikte katlanarak artar.

Baş genellikle birçok parametreleri olan bir algoritma daha fazla esnekliğe sahip olduğunu gösteren sayıdır. Sağlanan parametre ayarlarını doğru bileşimini bulabilirsiniz genellikle çok iyi doğruluk elde edebilirsiniz.

### <a name="number-of-features"></a>Birçok özellik

Belirli veri türleri, birçok özellik veri noktalarının sayısını karşılaştırıldığında çok büyük olabilir. Genetik veya metin verilerini durum genellikle budur. Çok sayıda özellik unfeasibly uzun zaman eğitim yaparak bazı öğrenme algoritmalarını bog. Destek vektörü makineler bu durumda (aşağıya bakın) uygundur.

### <a name="special-cases"></a>Özel durumlar

Bazı öğrenme algoritmalarını, veri ya da istenen sonuçları yapısı hakkında belirli varsayımlar. İhtiyaçlarınıza uygun bir fark ederseniz, bu, daha yararlı sonuçlar, daha doğru tahminler elde etmek veya daha hızlı eğitim süreleri verebilirsiniz.

| **Algoritma** | **Doğruluğu** | **Eğitim süresini** | **Doğrusallık** | **Parametreler** | **Notlar** |
| --- |:---:|:---:|:---:|:---:| --- |
| **İki sınıflı sınıflandırma** | | | | | |
| [Lojistik regresyon](/azure/machine-learning/studio-module-reference/two-class-logistic-regression) | |● |● |5 | |
| [karar ormanı](/azure/machine-learning/studio-module-reference/two-class-decision-forest) |● |○ | |6 | |
| [karar jungle](/azure/machine-learning/studio-module-reference/two-class-decision-jungle) |● |○ | |6 |Düşük bellek Ayak izi |
| [artırmalı karar ağacı](/azure/machine-learning/studio-module-reference/two-class-boosted-decision-tree) |● |○ | |6 |Büyük bellek Ayak izi |
| [sinir ağı](/azure/machine-learning/studio-module-reference/two-class-neural-network) |● | | |9 |[Ek özelleştirme mümkündür](azure-ml-netsharp-reference-guide.md) |
| [Ortalama perceptron](/azure/machine-learning/studio-module-reference/two-class-averaged-perceptron) |○ |○ |● |4 | |
| [Destekli vektör makinesi](/azure/machine-learning/studio-module-reference/two-class-support-vector-machine) | |○ |● |5 |Büyük özellik kümeleri için iyidir |
| [yerel olarak kapsamlı destek vektör makinesi](/azure/machine-learning/studio-module-reference/two-class-locally-deep-support-vector-machine) |○ | | |8 |Büyük özellik kümeleri için iyidir |
| [Bayes noktası makinesinin](/azure/machine-learning/studio-module-reference/two-class-bayes-point-machine) | |○ |● |3 | |
| **Çok sınıflı sınıflandırma** | | | | | |
| [Lojistik regresyon](/azure/machine-learning/studio-module-reference/multiclass-logistic-regression) | |● |● |5 | |
| [karar ormanı](/azure/machine-learning/studio-module-reference/multiclass-decision-forest) |● |○ | |6 | |
| [karar jungle](/azure/machine-learning/studio-module-reference/multiclass-decision-jungle) |● |○ | |6 |Düşük bellek Ayak izi |
| [sinir ağı](/azure/machine-learning/studio-module-reference/multiclass-neural-network) |● | | |9 |[Ek özelleştirme mümkündür](azure-ml-netsharp-reference-guide.md) |
| [bir-v-all](/azure/machine-learning/studio-module-reference/one-vs-all-multiclass) |- |- |- |- |Seçilen iki sınıflı yöntemi özelliklerini bakın |
| **Regresyon** | | | | | |
| [Doğrusal](/azure/machine-learning/studio-module-reference/linear-regression) | |● |● |4 | |
| [Bayes doğrusal](/azure/machine-learning/studio-module-reference/bayesian-linear-regression) | |○ |● |2 | |
| [karar ormanı](/azure/machine-learning/studio-module-reference/decision-forest-regression) |● |○ | |6 | |
| [artırmalı karar ağacı](/azure/machine-learning/studio-module-reference/boosted-decision-tree-regression) |● |○ | |5 |Büyük bellek Ayak izi |
| [Hızlı orman quantile](/azure/machine-learning/studio-module-reference/fast-forest-quantile-regression) |● |○ | |9 |Noktası Öngörüler yerine dağıtımları |
| [sinir ağı](/azure/machine-learning/studio-module-reference/neural-network-regression) |● | | |9 |[Ek özelleştirme mümkündür](azure-ml-netsharp-reference-guide.md) |
| [Poisson](/azure/machine-learning/studio-module-reference/poisson-regression) | | |● |5 |Teknik olarak günlük doğrusal. Sayıları tahmin etmek için |
| [Sıra](/azure/machine-learning/studio-module-reference/ordinal-regression) | | | |0 |Sıra sıralama tahmin etmek için |
| **Anormallik algılama** | | | | | |
| [Destekli vektör makinesi](/azure/machine-learning/studio-module-reference/one-class-support-vector-machine) |○ |○ | |2 |Özellikle büyük özellik kümeleri için iyi |
| [PCA tabanlı anomali algılama](/azure/machine-learning/studio-module-reference/pca-based-anomaly-detection) | |○ |● |3 | |
| [K-ortalamaları](/azure/machine-learning/studio-module-reference/k-means-clustering) | |○ |● |4 |Bir kümeleme algoritması |

**Algoritma özellikleri:**

**●** -mükemmel doğruluk, hızlı eğitim kez ve Doğrusallık kullanımını gösterir.

**○** -iyi doğruluk ve orta eğitim süreleri gösterir.

## <a name="algorithm-notes"></a>Algoritma notları

### <a name="linear-regression"></a>Çizgisel regresyon

Daha önce de belirtildiği [doğrusal regresyon](/azure/machine-learning/studio-module-reference/linear-regression) veri kümesi için bir satır (veya düz veya hyperplane) uyar. Basit ve hızlı, workhorse olduğunu ancak bazı sorunlar için aşırı alıyormuş olabilir.

![Doğrusal bir eğilim verileri](./media/algorithm-choice/image3.png)

***Doğrusal bir eğilim verileri***

### <a name="logistic-regression"></a>Lojistik regresyon

Adı 'regresyon' içerse Lojistik regresyon gerçekten yönelik güçlü bir araç olan [iki sınıflı](/azure/machine-learning/studio-module-reference/two-class-logistic-regression) ve [veya çoklu sınıflar](/azure/machine-learning/studio-module-reference/multiclass-logistic-regression) sınıflandırması. Bu hızlı ve basit olur. Kullandığı gerçek bir kişinin '-düz bir çizgi yerine şekillendirilmiş eğri veri gruplara bölmek için doğal bir şekilde uyum sağlar. Kullandığınızda, lojistik regresyon verir doğrusal sınıfı sınırlar, bu nedenle doğrusal bir yaklaştırma ile canlı şeydir emin olun.

![Tek bir özellik ile iki sınıflı verilere Lojistik regresyon](./media/algorithm-choice/image4.png)

***Tek bir özellik ile iki sınıflı verilere Lojistik regresyon*** *-sınıfı sınır Lojistik eğri olduğu gibi her iki sınıfları yalnızca yakın noktasıdır*

### <a name="trees-forests-and-jungles"></a>Harikası ağaçlar ve ormanları

Karar ormanları ([regresyon](/azure/machine-learning/studio-module-reference/decision-forest-regression), [iki sınıflı](/azure/machine-learning/studio-module-reference/two-class-decision-forest), ve [veya çoklu sınıflar](/azure/machine-learning/studio-module-reference/multiclass-decision-forest)), karar harikası ([iki sınıflı](/azure/machine-learning/studio-module-reference/two-class-decision-jungle) ve [ veya çoklu sınıflar](/azure/machine-learning/studio-module-reference/multiclass-decision-jungle)) ve Artırılmış karar ağaçları ([regresyon](/azure/machine-learning/studio-module-reference/boosted-decision-tree-regression) ve [iki sınıflı](/azure/machine-learning/studio-module-reference/two-class-boosted-decision-tree)) tüm karar ağaçları, temel makine öğrenme kavramını temel alır. Karar ağaçları birçok çeşidini vardır, ancak bunların tümü aynı şeyi yapmak — bölgeleri çoğunlukla aynı etikete sahip özellik alanı ayırabilir. Bu bölgeler tutarlı kategorisinin veya sınıflandırma veya regresyon yapmakta olduğunuz bağlı olarak sabit değerinin olabilir.

![Karar ağacı kesen ve onun özellik alanı](./media/algorithm-choice/image5.png)

***Karar ağacı özellik alanı kabaca Tekdüzen değerleri bölgelere kesen ve onun***

Özellik alanı rasgele olarak küçük bölgelere alt bölümlere ayrılabilir, çünkü yeterince iyi bölge başına bir veri noktası için bölme Imagine kolaydır. Bu aşırı overfitting örneğidir. Bunu önlemek için çok sayıda ağaçları uzamayan ağaçları değil bağıntılı sağlamak üzere özel matematik dikkatli. Bu "karar ormanı" overfitting engelleyen bir ağaç ortalamasıdır. Karar ormanları, çok miktarda bellek kullanabilirsiniz. Karar harikası biraz daha uzun bir eğitim süresini çoğaltamaz daha az bellek tüketen bir değişken var.

Artırmalı karar ağaçları, kaç kez ayırabilir ve her bölgede nasıl birkaç veri noktası izin verilen sınırlayarak overfitting kaçının. Algoritma, her biri için önce ağacı tarafından sol hata dengelemek için öğrenir ağaçları, bir dizi oluşturur. Çok miktarda bellek kullanma eğilimindedir çok doğru bir learner sonucudur. Tam teknik açıklaması için kullanıma [Friedman'ın özgün belgede](https://www-stat.stanford.edu/~jhf/ftp/trebst.pdf).

[Hızlı orman quantile regresyon](/azure/machine-learning/studio-module-reference/fast-forest-quantile-regression) karar ağaçları verilerin bir bölge, aynı zamanda quantiles biçiminde, dağıtım içinde yalnızca Normal (ORTANCA) değerini bilmek istediğiniz özel bir durum için bir çeşididir.

### <a name="neural-networks-and-perceptrons"></a>Sinir ağları ve perceptrons

Sinir ağları beyin ilham alınarak tasarlanan öğrenimi algoritmaları kapsayan [veya çoklu sınıflar](/azure/machine-learning/studio-module-reference/multiclass-neural-network), [iki sınıflı](/azure/machine-learning/studio-module-reference/two-class-neural-network), ve [regresyon](/azure/machine-learning/studio-module-reference/neural-network-regression) sorunları. Sonsuz bir çeşitli geldikleri, ancak Azure Machine Learning Studio içinde sinir ağları yönlendirilmiş Çevrimsiz grafikler biçiminin tümü. Giriş özellikleri ileri (hiçbir zaman geri) katmanları sırasıyla çıkışları açık önce aktarılmasını anlamına gelir. Her katmanda girişleri çeşitli birleşimler ağırlıklı, toplamı ve sonraki katmana geçirildi. Bu basit hesaplamalar birleşimi Gelişmiş sınıfı sınırları ve veri eğilimleri görünüşte tarafından Sihirli öğrenin olanağı sonuçlanır. Çok katmanlı ağlar bu tür "çok fazla teknik raporlama ve Bilim Kurgu artırıyor derin öğrenme" gerçekleştirin.

Bu yüksek performanslı ücretsiz, ancak gelmez. Sinir ağları, özellikle büyük veri kümeleri çok sayıda özellikleri için eğitmek için uzun sürebilir. Ayrıca parametre Süpürme eğitim süresini büyük ölçüde genişletir anlamına gelir çoğu algoritmaları sayısından daha fazla parametre sahiptirler.
Ve isteyen bu overachievers [kendi ağ yapısı belirtmek](azure-ml-netsharp-reference-guide.md), olasılık inexhaustible.

![Sinir ağları tarafından öğrenilen sınırları](./media/algorithm-choice/image6.png)

***Sinir ağları tarafından öğrenilen sınırları, karmaşık ve düzensiz olabilir.***

[İki sınıflı perceptron ortalama](/azure/machine-learning/studio-module-reference/two-class-averaged-perceptron) oldukça eğitim kez sinir ağları yanıtı. Doğrusal sınıfı sınırlar sağlayan bir ağ yapısını kullanır. Günümüzün standartlarıyla neredeyse temel ancak yerine çalışma uzun bir geçmişe sahiptir ve hızlı bir şekilde öğrenmek için küçük.

### <a name="svms"></a>SVMs

Destek vektörü makineler (SVMs) mümkün olduğunca geniş bir kenar boşluğu olarak sınıfları tarafından ayıran sınır bulun. İki sınıf açıkça ayrılamayan, bunlar için en iyi sınır algoritmalar bulun. Azure Machine Learning Studio'da yazıldığı gibi [iki sınıflı SVM](/azure/machine-learning/studio-module-reference/two-class-support-vector-machine) yalnızca düz bir çizgi ile yapar (SVM konuşurken doğrusal bir çekirdek kullanır).
Bu doğrusal bir yaklaştırma getirdiği için oldukça hızlı bir şekilde çalıştırabilirsiniz. Burada gerçekten çıkar metin veya genom veri gibi güçlü özelliği veriler değişkendir. Bu gibi durumlarda SVMs sınıfları daha hızlı bir şekilde ve yalnızca uygun miktarda bellek gerektiren ek olarak çoğu diğer algoritmalar, daha az overfitting ayrı olanağına sahip olursunuz.

![Destek vektör makinesi sınıfı sınır](./media/algorithm-choice/image7.png)

***Bir normal destek vektör makinesi sınıfı sınır iki sınıf ayırarak kenar boşluğu en üst düzeye çıkarır.***

Microsoft Research'ün başka bir ürün [iki sınıflı yerel olarak derin SVM](/azure/machine-learning/studio-module-reference/two-class-locally-deep-support-vector-machine) doğrusal olmayan bir doğrusal sürüm hız ve bellek verimliliğini çoğunu korur SVM çeşididir. Burada doğrusal bir yaklaşım yeterince doğru yanıtlar vermek olmayan durumlar için idealdir. Geliştiriciler bir dizi küçük doğrusal SVM sorun sorunla hızlı bölmek tarafından tutulur. Okuma [tam açıklama](http://proceedings.mlr.press/v28/jose13.html) nasıl bunlar Bu ipucunu çekilen ilişkin ayrıntılar için.

Akıllı bir doğrusal SVMs uzantısı [bir sınıf SVM](/azure/machine-learning/studio-module-reference/one-class-support-vector-machine) sıkı bir şekilde tüm veri kümesini özetleyen bir sınır çizer. Anomali algılama için kullanışlıdır. Şu ana kadar bu sınırları dışında kalan tüm yeni veri noktaları, kayda değer olmasını olağan değildir.

### <a name="bayesian-methods"></a>Bayes yöntemleri

Yükseltebilirsiniz kalite Bayes yöntemi vardır: Bunlar overfitting kaçının. Bunlar önceden yanıt büyük olasılıkla dağıtımı hakkında bazı varsayımlarda bulunarak yapın. Bu yaklaşımın başka bir byproduct, çok az sayıda parametre sahip olduğunu belirtir. Azure Machine Learning Studio iki sınıflandırma Bayes algoritmaları sahiptir ([iki sınıflı Bayes noktası makinesi](/azure/machine-learning/studio-module-reference/two-class-bayes-point-machine)) ve gerileme ([Bayes doğrusal regresyon](/azure/machine-learning/studio-module-reference/bayesian-linear-regression)).
Bu veri bölebilir veya düz bir çizgi uygun olduğunu varsayın unutmayın.

Geçmiş bir not üzerinde Bayes noktası makineleri Microsoft Research'te geliştirilen. Bazı olağanüstü güzel teorik iş arkasına sahiptirler. İsteyen Öğrenci yönlendirildiği [JMLR özgün makalesinde](http://jmlr.org/papers/volume1/herbrich01a/herbrich01a.pdf) ve [Chris Güneş tarafından bilgilendirici blog](https://blogs.technet.com/b/machinelearning/archive/2014/10/30/embracing-uncertainty-probabilistic-inference.aspx).

### <a name="specialized-algorithms"></a>Özel algoritmaların
Belirli bir hedefe varsa günümüzdeyiz! tam da olabilir. Azure Machine Learning Studio koleksiyonda olarak uzmanlaşmış algoritmaları vardır:

- Tahmin Rank ([sıralı regresyon](/azure/machine-learning/studio-module-reference/ordinal-regression)),
- Tahmin sayısı ([Poisson regresyon](/azure/machine-learning/studio-module-reference/poisson-regression)),
- anomali algılama (bir temel alarak [asıl bileşenlerini analiz](/azure/machine-learning/studio-module-reference/pca-based-anomaly-detection) ve bir temel [vektör makineler Destek](/azure/machine-learning/studio-module-reference/one-class-support-vector-machine))
- Kümeleme ([K-ortalamaları](/azure/machine-learning/studio-module-reference/k-means-clustering))

![PCA tabanlı anomali algılama](./media/algorithm-choice/image8.png)

***PCA tabanlı anomali algılama*** *-veri büyük çoğunluğu stereotipik dağıtım döner; bu dağıtım noktasından önemli ölçüde deviating şüpheli noktalarıdır*

![Veri kümesi K-ortalamaları kullanarak gruplanır.](./media/algorithm-choice/image9.png)

***Bir veri kümesi K-ortalamaları kullanarak beş kümeler halinde gruplandırılır.***

Ayrıca bir topluluğu vardır [bir v tüm çok sınıflı sınıflandırıcı](/azure/machine-learning/studio-module-reference/one-vs-all-multiclass), N-1 iki sınıflı sınıflandırma sorunlarla karşılaşırsanız N sınıflı sınıflandırma problemi keser. Doğruluk, eğitim süresini ve Doğrusallık özellikleri kullanılan iki sınıflı sınıflandırıcılar tarafından belirlenir.

![Üç düzeyde sınıflandırıcı oluşturmak için bir araya iki sınıflı sınıflandırıcı](./media/algorithm-choice/image10.png)

***Bir çift iki sınıflı sınıflandırıcılar birleştiren üç sınıf sınıflandırıcı oluşturmak için***

Azure Machine Learning Studio, bir güçlü makine öğrenimi framework başlığının altında erişimi de içerir [Vowpal Wabbit](/azure/machine-learning/studio-module-reference/train-vowpal-wabbit-version-7-4-model).
VW Burada, kategori Koşullarımız, Sınıflandırma ve regresyon hem sorunları edinebilirsiniz ve hatta kısmen etiketlenmemiş verilerden bilgi edinebilirsiniz. Öğrenme algoritmaları, kayıp işlevleri ve iyileştirme algoritmalarını sayısı herhangi birini kullanmak üzere yapılandırabilirsiniz. Bunu baştan yukarı verimli, paralel ve son derece hızlı olacak şekilde tasarlanmıştır. Bu, gerçekten büyük özellik kümeleri görünen çok az çabayla işler.
Başlatılan ve Microsoft Research'ün kendi John Langford tarafından yürütülen VW hisse senedi araba algoritmaları alanındaki formülü bir bir giriştir. Her sorun VW uyar, ancak Sizinkinde varsa, kendi arabiriminde öğrenme eğrisini tırmanan, while değer olabilir. Ayrıca kullanılabilir olarak [tek başına bir açık kaynak kod](https://github.com/JohnLangford/vowpal_wabbit) çeşitli dillerde.

## <a name="next-steps"></a>Sonraki Adımlar

* Anlaşılması kolay bilgi grafiği machine learning temel bilgileri yaygın machine learning soruları yanıtlamak için kullanılan popüler algoritmaları hakkında bilgi edinmek için genel bakış indirmek için bkz [makine öğrenimi algoritma örnekleri ile Temelleri](basics-infographic-with-algorithm-examples.md).

* Machine Learning Studio'da kullanılabilen tüm makine öğrenimi algoritma kategoriye göre bir listesi için bkz [modeli Başlat](/azure/machine-learning/studio-module-reference/machine-learning-initialize-model) Machine Learning Studio algoritma ve modül Yardımı.

* Bir tam alfabetik listesi algoritmaları ve Machine Learning Studio'daki modüller için bkz: [Machine Learning Studio modüllerinin A-Z listesi](/azure/machine-learning/studio-module-reference/a-z-module-list) Machine Learning Studio algoritma ve modül Yardımı.
