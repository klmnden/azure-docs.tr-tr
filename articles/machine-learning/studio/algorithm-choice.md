---
title: Algoritmaları seçme
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio algoritmaları için denetimli ve Denetimsiz öğrenme kümeleme, sınıflandırma veya regresyon denemeleri nasıl seçeceğinizi öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: previous-ms.author=pakalra, previous-author=pakalra
ms.date: 12/18/2017
ms.openlocfilehash: d60c99349fef26fc1ead7f6ea4b77d0c364c4abb
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55868149"
---
# <a name="how-to-choose-algorithms-for-azure-machine-learning-studio"></a>Azure Machine Learning Studio için algoritma seçme

"Hangi makine öğrenme algoritmasına kullanmam gerekir?" sorusuna verilen yanıt her zaman "Duruma göre değişir." şeklindedir. Bu boyut, kalite ve verilerin niteliğine bağlıdır. Bu, Yanıtla yapmak istediğiniz üzerinde bağlıdır. Bu, nasıl algoritmasının matematik kullanmakta olduğunuz bilgisayar için yönergeler içine çevrilmiştir üzerinde bağlıdır. Ve duruma göre değişir üzerinde ne kadar süre sahip. Hatta en deneyimli veri uzmanları, denemeden önce en iyi algoritmayı gerçekleştirecek bildiremez.

## <a name="the-machine-learning-algorithm-cheat-sheet"></a>Machine Learning algoritmasını kural sayfası

**Microsoft Azure Machine Learning algoritması kural sayfası** makine öğrenme algoritmasını algoritmaları Azure Machine Learning Studio kitaplığından Tahmine dayalı analiz çözümleriniz için sağ seçtiğiniz yardımcı olur.
Bu makalede aracılığıyla nasıl kullanılacağı gösterilmektedir.

> [!NOTE]
> İpuçlarını indirin ve bu makaleyi izlemek için Git [makine öğrenimi algoritma bilgi sayfasına için Microsoft Azure Machine Learning Studio](algorithm-cheat-sheet.md).
> 
> 

Bu kural sayfası çok belirli bir kitleyi göz önünde vardır: algoritma Azure Machine Learning Studio'da çalışmaya başlamak seçme çalışılırken başına veri uzmanı Lise düzeyi machine learning ile. Bu, bazı genelleştirmesi ve oversimplifications kolaylaştırır, ancak güvenli bir yönde işaret eden anlamına gelir. Ayrıca, burada listelenmeyen algoritmaları çok sayıda olduğu anlamına gelir. Azure Machine Learning, metotların daha eksiksiz bir kümesini kapsayacak şekilde büyüdükçe, bunları ekleyeceğiz.

Bu, derlenmiş geri bildirim ve ipuçları birçok veri bilimcileri ve machine learning uzmanlar önerilerdir. Biz üzerinde her şeyi kabul etmememiz ancak kaba bir fikir birliğine varılmış müşterilerimizin düşünceleri son derece harmonize girişimi yaptınız. "Duruma göre değişir ile..." çözünürlüğünün bilgilerinin çoğunu başlayın

### <a name="how-to-use-the-cheat-sheet"></a>Kopya kağıdı kullanma

Grafik olarak yolu ve algoritma etiketleri okuyun "için  *&lt;yol etiketinin&gt;*, kullanın  *&lt;algoritması&gt;*." Örneğin, "için *hızı*, kullanın *lojistik regresyon iki sınıf*." Bazen birden fazla dal için geçerlidir.
Bazen bunların hiçbiri tam olarak uygun değil. Bunlar tam olan bu konuda endişelenmeyin için thumb kural önerileri olacak şekilde tasarlanmıştır.
I ile söz konusu açıklandı birkaç veri bilimcileri en iyi algoritmayı bulmak için yalnızca emin tümünü denemek için yoludur.

İşte bir örnek [Azure AI Gallery](http://gallery.cortanaintelligence.com/) aynı verilere karşı çeşitli algoritmalar çalışır ve sonuçları karşılaştıran bir deneme: [Çok sınıflı sınıflandırıcılar karşılaştırın: Harfli tanıma](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

> [!TIP]
> Anlaşılması kolay bilgi grafiği machine learning temel bilgileri yaygın machine learning soruları yanıtlamak için kullanılan popüler algoritmaları hakkında bilgi edinmek için genel bakış indirmek için bkz [makine öğrenimi algoritma örnekleri ile Temelleri](basics-infographic-with-algorithm-examples.md).

## <a name="flavors-of-machine-learning"></a>Machine Learning model olarak sağlanır

### <a name="supervised"></a>Denetimli

Denetimli öğrenme algoritmalarını örnekler kümesine göre tahminlerde. Örneğin, geçmiş hisse senedi fiyatlarına tehlike tahmin gelecekteki fiyatlarla kullanılabilir. Eğitim için kullanılan her bir örnekte ilgi değeri ile etiketlenir — Bu hisse senedi fiyatının durumda. Bu değer etiketleri desenleri denetimli öğrenme algoritmasını arar. İlgili olabilecek herhangi bir bilgi kullanabilirsiniz — gün haftanın, döneminde, şirketin finansal verileri, sektör türünü, jeopolitik aksatıcı olayları varlığını — ve her bir algoritmanın desenleri farklı türleri için görünür. Algoritma en iyi düzeni bulduktan sonra alabilir, etiketsiz sınama veri tahmininde bulunmak amacıyla bu düzeni kullanır: yarının fiyatlar.

Denetimli öğrenme, machine learning, popüler ve kullanışlı bir türüdür. Bunun tek istisnası tüm modülleri Azure Machine Learning algoritmaları öğrenme denetimli olduğu. Azure Machine Learning içinde temsil edilen birkaç belirli denetimli öğrenme tür vardır: Sınıflandırma, regresyon ve anomali algılama.

* **Sınıflandırma**. Verileri bir kategori tahmin etmek için kullanılan, denetimli öğrenme sınıflandırma da adlandırılır. Bu görüntü resmini 'cat' veya 'dog' atarken durumdur. Yalnızca iki seçenek olduğunda çağrılır **iki sınıflı** veya **binom sınıflandırma**. Olarak olduğunda daha fazla kategori NCAA Mart TURNUVASI Turnuva kazanan tahmin olduğunda, bu sorunu olarak bilinir **çok sınıflı sınıflandırma**.
* **Regresyon**. Bir değer, hisse senedi fiyatlarına gibi ile tahmin edildiğinde denetimli öğrenme regresyon çağrılır.
* **Anomali algılama**. Bazen yalnızca olağan dışı veri noktalarını tanımlamak üzere hedeftir. Sahtekarlık algılama, örneğin, şüpheli tüm son derece alışılmışın dışında kredi kartı harcama desenleri alır. Olası çeşitlemeleri kadar çok sayıda ve bu nedenle birkaç sahte hangi etkinlik öğrenmek için uygun değil, eğitim örnekleri gibi görünen. Anomali algılama alan yalnızca normal hangi etkinlik (bir geçmiş sahte olmayan işlemleri kullanarak gibi) görünümünü öğrenin ve önemli ölçüde farklı olan her şeyi tanımlamak için bir yaklaşımdır.

### <a name="unsupervised"></a>Denetimsiz

Denetimsiz öğrenme içinde bunlarla ilgili hiçbir etiket veri noktanız olması. Bunun yerine, Denetimsiz öğrenme algoritmasının bir şekilde verileri düzenleme veya yapısını tanımlamak için hedeftir. Bu kümeler halinde gruplandırmak veya basit ya da daha düzenli görünür bir karmaşık verilere baktığımızda, farklı yöntemleri sürekli düzende bulmak anlamına gelebilir.

### <a name="reinforcement-learning"></a>Pekiştirmeye dayalı öğrenme

Öğrenme pekiştirmeye içinde yanıt her veri noktası için bir eylem seçmek üzere algoritma alır. Öğrenme algoritmasını ödül sinyal kısa kararı ne kadar iyi olduğunu gösteren bir süre daha da alır.
Algoritma bunu temel alarak, yüksek ödül elde etmek için kendi stratejisi değiştirir. Şu anda Azure Machine Learning algoritması modüllerde öğrenme hiçbir pekiştirmeye vardır. Burada zaman içinde bir noktadaki sensör okumaları kümesini bir veri noktasıdır ve algoritma robot'ın sonraki eylem seçmelisiniz robotlara ilişkin pekiştirmeye dayalı öğrenme yaygındır. Ayrıca, bir doğal uygulamaları nesnelerin Internet'için uygun değildir.

## <a name="considerations-when-choosing-an-algorithm"></a>Bir algoritma seçerken dikkat edilmesi gerekenler

### <a name="accuracy"></a>Doğruluk

En doğru yanıta olası alma her zaman gerekli değildir.
Bazen bir yaklaştırma kullanmak istediğinize bağlı olarak, yeterli olur. Bu durumda, işleme süresini önemli ölçüde daha fazla yaklaşık yöntemleriyle kalmanız tarafından Kes mümkün olabilir. Daha fazla yaklaşık yöntemlerden başka bir avantajı, doğal olarak önlemek için eğilimindedir olan [overfitting](https://youtu.be/DQWI1kvmwRg).

### <a name="training-time"></a>Eğitim süresini

Kaç dakika veya saat bir modeli eğitmek için gereken büyük miktarda algoritmaları arasında değişir. Zaman eğitim genellikle yakından bağlı tutarlılık — bir genellikle eşlik eden diğer. Ayrıca, bazı algoritmalar diğerlerinden daha hassas veri noktası sayısı.
Zaman sınırlı olduğunda özellikle veri kümesi büyük olduğunda seçimi algoritması yönlendirebilirsiniz.

### <a name="linearity"></a>Doğrusallık

Makine öğrenimi algoritmaları çok sayıda olun Doğrusallık kullanın. Düz çizgi (veya daha büyük boyutlu kendi analog) sınıfları ayrılabilir doğrusal sınıflandırma algoritmaları varsayılır. Bunlar Lojistik regresyon ve vektör makineleri (Azure Machine Learning'de uygulanmış gibi) desteği içerir.
Doğrusal regresyon algoritmaları, veri eğilimleri düz bir çizgi izleyin varsayılır. Bu varsayımları bazı sorunlar için hatalı değildir, ancak bazılarında, doğruluk kapatmaya.

![Doğrusal olmayan sınıf sınırı][1]

***Doğrusal olmayan sınıf sınır*** *-bağlı olan bir doğrusal bir sınıflandırma algoritmasıdır üzerinde düşük doğruluk sonuçlanır*

![Doğrusal bir eğilim verileri][2]

***Doğrusal bir eğilim verilerle*** *-bir doğrusal regresyon yöntemi kullanarak daha büyük kadar hataları oluşturmak*

Kendi tehlikeleri rağmen doğrusal algoritmaları bir saldırı ilk satırı olarak çok fazla kullanılır. Bunlar algorithmically basit ve hızlı eğitmek için olma eğilimindedir.

### <a name="number-of-parameters"></a>Parametre sayısı

Bir algoritma ayarlarken açmak için bir veri Bilimcisi alır düğmelerini parametrelerdir. Bunlar, hata toleransı veya yineleme veya seçenekleri, algoritmanın nasıl davranacağını çeşitler arasında sayısı gibi algoritması'nın davranışını etkileyen sayılardır. Eğitim süresini ve algoritma doğruluğunu bazen yalnızca doğru ayarları almak için oldukça önemli olabilir. Genelde çok sayıda parametre algoritmalarıyla iyi kombinasyonu bulmak için çoğu deneme ve hata gerektirir.

Alternatif olarak, var olan bir [parametresi Süpürme](algorithm-parameters-optimize.md) modülü blok Azure Machine learning'de, hangi ayrıntı düzeyi seçtiğiniz tüm parametre birleşimlerini otomatik olarak çalışır. Bu parametre alanı yayılmış emin olmak için harika bir yoludur ancak, bir modeli eğitmek için gereken süreyi, parametre sayısıyla birlikte katlanarak artar.

Baş genellikle birçok parametreleri olan bir algoritma daha fazla esnekliğe sahip olduğunu gösteren sayıdır. Genellikle çok iyi doğruluk elde edebilirsiniz. Sağlanan parametre ayarlarını doğru bileşimini bulabilirsiniz.

### <a name="number-of-features"></a>Birçok özellik

Belirli veri türleri, birçok özellik veri noktalarının sayısını karşılaştırıldığında çok büyük olabilir. Genetik veya metin verilerini durum genellikle budur. Çok sayıda özellik unfeasibly uzun zaman eğitim yaparak bazı öğrenme algoritmalarını bog. Destek vektörü makineler bu durumda (aşağıya bakın) uygundur.

### <a name="special-cases"></a>Özel durumlar

Bazı öğrenme algoritmalarını, veri ya da istenen sonuçları yapısı hakkında belirli varsayımlar. İhtiyaçlarınıza uygun bir fark ederseniz, bu, daha yararlı sonuçlar, daha doğru tahminler elde etmek veya daha hızlı eğitim süreleri verebilirsiniz.

| **Algoritma** | **Doğruluğu** | **Eğitim süresini** | **Doğrusallık** | **Parametreler** | **Notlar** |
| --- |:---:|:---:|:---:|:---:| --- |
| **İki sınıflı sınıflandırma** | | | | | |
| [Lojistik regresyon](https://msdn.microsoft.com/library/azure/dn905994.aspx) | |● |● |5 | |
| [karar ormanı](https://msdn.microsoft.com/library/azure/dn906008.aspx) |● |○ | |6 | |
| [karar jungle](https://msdn.microsoft.com/library/azure/dn905976.aspx) |● |○ | |6 |Düşük bellek Ayak izi |
| [artırmalı karar ağacı](https://msdn.microsoft.com/library/azure/dn906025.aspx) |● |○ | |6 |Büyük bellek Ayak izi |
| [sinir ağı](https://msdn.microsoft.com/library/azure/dn905947.aspx) |● | | |9 |[Ek özelleştirme mümkündür](https://go.microsoft.com/fwlink/?LinkId=402867) |
| [Ortalama perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx) |○ |○ |● |4 | |
| [Destekli vektör makinesi](https://msdn.microsoft.com/library/azure/dn905835.aspx) | |○ |● |5 |Büyük özellik kümeleri için iyidir |
| [yerel olarak kapsamlı destek vektör makinesi](https://msdn.microsoft.com/library/azure/dn913070.aspx) |○ | | |8 |Büyük özellik kümeleri için iyidir |
| [Bayes noktası makinesinin](https://msdn.microsoft.com/library/azure/dn905930.aspx) | |○ |● |3 | |
| **Çok sınıflı sınıflandırma** | | | | | |
| [Lojistik regresyon](https://msdn.microsoft.com/library/azure/dn905853.aspx) | |● |● |5 | |
| [karar ormanı](https://msdn.microsoft.com/library/azure/dn906015.aspx) |● |○ | |6 | |
| [karar jungle ](https://msdn.microsoft.com/library/azure/dn905963.aspx) |● |○ | |6 |Düşük bellek Ayak izi |
| [sinir ağı](https://msdn.microsoft.com/library/azure/dn906030.aspx) |● | | |9 |[Ek özelleştirme mümkündür](https://go.microsoft.com/fwlink/?LinkId=402867) |
| [bir-v-all](https://msdn.microsoft.com/library/azure/dn905887.aspx) |- |- |- |- |Seçilen iki sınıflı yöntemi özelliklerini bakın |
| **Regresyon** | | | | | |
| [Doğrusal](https://msdn.microsoft.com/library/azure/dn905978.aspx) | |● |● |4 | |
| [Bayes doğrusal](https://msdn.microsoft.com/library/azure/dn906022.aspx) | |○ |● |2 | |
| [karar ormanı](https://msdn.microsoft.com/library/azure/dn905862.aspx) |● |○ | |6 | |
| [artırmalı karar ağacı](https://msdn.microsoft.com/library/azure/dn905801.aspx) |● |○ | |5 |Büyük bellek Ayak izi |
| [Hızlı orman quantile](https://msdn.microsoft.com/library/azure/dn913093.aspx) |● |○ | |9 |Noktası Öngörüler yerine dağıtımları |
| [sinir ağı](https://msdn.microsoft.com/library/azure/dn905924.aspx) |● | | |9 |[Ek özelleştirme mümkündür](https://go.microsoft.com/fwlink/?LinkId=402867) |
| [Poisson](https://msdn.microsoft.com/library/azure/dn905988.aspx) | | |● |5 |Teknik olarak günlük doğrusal. Sayıları tahmin etmek için |
| [Sıra](https://msdn.microsoft.com/library/azure/dn906029.aspx) | | | |0 |Sıra sıralama tahmin etmek için |
| **Anormallik algılama** | | | | | |
| [Destekli vektör makinesi](https://msdn.microsoft.com/library/azure/dn913103.aspx) |○ |○ | |2 |Özellikle büyük özellik kümeleri için iyi |
| [PCA tabanlı anomali algılama](https://msdn.microsoft.com/library/azure/dn913102.aspx) | |○ |● |3 | |
| [K-ortalamaları](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/) | |○ |● |4 |Bir kümeleme algoritması |

**Algoritma özellikleri:**

**●** -mükemmel doğruluk, hızlı eğitim kez ve Doğrusallık kullanımını gösterir.

**○** -iyi doğruluk ve orta eğitim süreleri gösterir.

## <a name="algorithm-notes"></a>Algoritma notları

### <a name="linear-regression"></a>Çizgisel regresyon

Daha önce de belirtildiği [doğrusal regresyon](https://msdn.microsoft.com/library/azure/dn905978.aspx) veri kümesi için bir satır (veya düz veya hyperplane) uyar. Basit ve hızlı, workhorse olduğunu ancak bazı sorunlar için aşırı alıyormuş olabilir.
Burada denetle bir [doğrusal regresyon öğretici](linear-regression-in-azure.md).

![Doğrusal bir eğilim verileri][3]

***Doğrusal bir eğilim verileri***

### <a name="logistic-regression"></a>Lojistik regresyon

Confusingly adlarında 'regresyon' içerse de, lojistik regresyon gerçekten yönelik güçlü bir araç olan [iki sınıflı](https://msdn.microsoft.com/library/azure/dn905994.aspx) ve [veya çoklu sınıflar](https://msdn.microsoft.com/library/azure/dn905853.aspx) sınıflandırması. Bu hızlı ve basit olur. Kullandığı gerçek bir kişinin '-düz bir çizgi yerine şekillendirilmiş eğri veri gruplara bölmek için doğal bir şekilde uyum sağlar. Kullandığınızda, lojistik regresyon verir doğrusal sınıfı sınırlar, bu nedenle doğrusal bir yaklaştırma ile canlı şeydir emin olun.

![Tek bir özellik ile iki sınıflı verilere Lojistik regresyon][4]

***Tek bir özellik ile iki sınıflı verilere Lojistik regresyon*** *-sınıfı sınır Lojistik eğri olduğu gibi her iki sınıfları yalnızca yakın noktasıdır*

### <a name="trees-forests-and-jungles"></a>Harikası ağaçlar ve ormanları

Karar ormanları ([regresyon](https://msdn.microsoft.com/library/azure/dn905862.aspx), [iki sınıflı](https://msdn.microsoft.com/library/azure/dn906008.aspx), ve [veya çoklu sınıflar](https://msdn.microsoft.com/library/azure/dn906015.aspx)), karar harikası ([iki sınıflı](https://msdn.microsoft.com/library/azure/dn905976.aspx) ve [ veya çoklu sınıflar](https://msdn.microsoft.com/library/azure/dn905963.aspx)) ve Artırılmış karar ağaçları ([regresyon](https://msdn.microsoft.com/library/azure/dn905801.aspx) ve [iki sınıflı](https://msdn.microsoft.com/library/azure/dn906025.aspx)) tüm karar ağaçları, temel makine öğrenme kavramını temel alır. Karar ağaçları birçok çeşidini vardır, ancak bunların tümü aynı şeyi yapmak — bölgeleri çoğunlukla aynı etikete sahip özellik alanı ayırabilir. Bu bölgeler tutarlı kategorisinin veya sınıflandırma veya regresyon yapmakta olduğunuz bağlı olarak sabit değerinin olabilir.

![Karar ağacı kesen ve onun özellik alanı][5]

***Karar ağacı özellik alanı kabaca Tekdüzen değerleri bölgelere kesen ve onun***

Özellik alanı rasgele olarak küçük bölgelere alt bölümlere ayrılabilir, çünkü yeterince iyi bölge başına bir veri noktası için bölme Imagine kolaydır. Bu aşırı overfitting örneğidir. Bunu önlemek için çok sayıda ağaçları uzamayan geçen özel matematik dikkatli ağaçları değil bağıntılı olan. Bu "karar ormanı" overfitting engelleyen bir ağaç ortalamasıdır. Karar ormanları, çok miktarda bellek kullanabilirsiniz. Karar harikası biraz daha uzun bir eğitim süresini çoğaltamaz daha az bellek tüketen bir değişken var.

Artırmalı karar ağaçları, kaç kez ayırabilir ve her bölgede nasıl birkaç veri noktası izin verilen sınırlayarak overfitting kaçının. Algoritma, her biri için önce ağacı tarafından sol hata dengelemek için öğrenir ağaçları, bir dizi oluşturur. Çok miktarda bellek kullanma eğilimindedir çok doğru bir learner sonucudur. Tam teknik açıklaması için kullanıma [Friedman'ın özgün belgede](http://www-stat.stanford.edu/~jhf/ftp/trebst.pdf).

[Hızlı orman quantile regresyon](https://msdn.microsoft.com/library/azure/dn913093.aspx) karar ağaçları verilerin bir bölge, aynı zamanda quantiles biçiminde, dağıtım içinde yalnızca Normal (ORTANCA) değerini bilmek istediğiniz özel bir durum için bir çeşididir.

### <a name="neural-networks-and-perceptrons"></a>Sinir ağları ve perceptrons

Sinir ağları beyin ilham alınarak tasarlanan öğrenimi algoritmaları kapsayan [veya çoklu sınıflar](https://msdn.microsoft.com/library/azure/dn906030.aspx), [iki sınıflı](https://msdn.microsoft.com/library/azure/dn905947.aspx), ve [regresyon](https://msdn.microsoft.com/library/azure/dn905924.aspx) sorunları. Sonsuz bir çeşitli geldikleri, ancak Azure Machine Learning içinde sinir ağları yönlendirilmiş Çevrimsiz grafikler biçiminin tümü. Giriş özellikleri ileri (hiçbir zaman geri) katmanları sırasıyla çıkışları açık önce aktarılmasını anlamına gelir. Her katmanda girişleri çeşitli birleşimler ağırlıklı, toplamı ve sonraki katmana geçirildi. Bu basit hesaplamalar birleşimi Gelişmiş sınıfı sınırları ve veri eğilimleri görünüşte tarafından Sihirli öğrenin olanağı sonuçlanır. Çok katmanlı ağlar bu tür "çok fazla teknik raporlama ve Bilim Kurgu artırıyor derin öğrenme" gerçekleştirin.

Bu yüksek performanslı ücretsiz, ancak gelmez. Sinir ağları, özellikle büyük veri kümeleri çok sayıda özellikleri için eğitmek için uzun sürebilir. Ayrıca parametre Süpürme eğitim süresini büyük ölçüde genişletir anlamına gelir çoğu algoritmaları sayısından daha fazla parametre sahiptirler.
Ve isteyen bu overachievers [kendi ağ yapısı belirtmek](https://go.microsoft.com/fwlink/?LinkId=402867), olasılık inexhaustible.

![Sınırları öğrenilen sinir ağları tarafından][6]
***sinir ağları tarafından öğrenilen sınırları, karmaşık ve düzensiz olabilir.***

[İki sınıflı perceptron ortalama](https://msdn.microsoft.com/library/azure/dn906036.aspx) oldukça eğitim kez sinir ağları yanıtı. Doğrusal sınıfı sınırlar sağlayan bir ağ yapısını kullanır. Günümüzün standartlarıyla neredeyse temel ancak yerine çalışma uzun bir geçmişe sahiptir ve hızlı bir şekilde öğrenmek için küçük.

### <a name="svms"></a>SVMs

Destek vektörü makineler (SVMs) mümkün olduğunca geniş bir kenar boşluğu olarak sınıfları tarafından ayıran sınır bulun. İki sınıf açıkça ayrılamayan, bunlar için en iyi sınır algoritmalar bulun. Azure Machine Learning'de yazıldığı gibi [iki sınıflı SVM](https://msdn.microsoft.com/library/azure/dn905835.aspx) yalnızca düz bir çizgi ile bunu yapar. (SVM konuşurken doğrusal bir çekirdek kullanır.) Bu doğrusal bir yaklaştırma getirdiği için oldukça hızlı bir şekilde çalıştırabilirsiniz. Burada gerçekten çıkar özellik güçlü metin gibi veya genetik ile verilerdir. Bu gibi durumlarda SVMs sınıfları daha hızlı bir şekilde ve yalnızca uygun miktarda bellek gerektiren ek olarak çoğu diğer algoritmalar, daha az overfitting ayrı olanağına sahip olursunuz.

![Destek vektör makinesi sınıfı sınır][7]

***Bir normal destek vektör makinesi sınıfı sınır iki sınıf ayırarak kenar boşluğu en üst düzeye çıkarır.***

Microsoft Research'ün başka bir ürün [iki sınıflı yerel olarak derin SVM](https://msdn.microsoft.com/library/azure/dn913070.aspx) doğrusal olmayan bir doğrusal sürüm hız ve bellek verimliliğini çoğunu korur SVM çeşididir. Burada doğrusal bir yaklaşım yeterince doğru yanıtlar vermek olmayan durumlar için idealdir. Geliştiriciler bir sürü küçük doğrusal SVM sorunları sorunla hızlı bölmek tarafından tutulur. Okuma [tam açıklama](http://proceedings.mlr.press/v28/jose13.html) nasıl bunlar Bu ipucunu çekilen ilişkin ayrıntılar için.

Akıllı bir doğrusal SVMs uzantısı [bir sınıf SVM](https://msdn.microsoft.com/library/azure/dn913103.aspx) sıkı bir şekilde tüm veri kümesini özetleyen bir sınır çizer. Anomali algılama için kullanışlıdır. Şu ana kadar bu sınırları dışında kalan tüm yeni veri noktaları, kayda değer olmasını olağan değildir.

### <a name="bayesian-methods"></a>Bayes yöntemleri

Yükseltebilirsiniz kalite Bayes yöntemi vardır: Bunlar overfitting kaçının. Bunlar önceden yanıt büyük olasılıkla dağıtımı hakkında bazı varsayımlarda bulunarak yapın. Bu yaklaşımın başka bir byproduct, çok az sayıda parametre sahip olduğunu belirtir. Azure Machine Learning iki sınıflandırma için her iki Bayes algoritmalar içerir ([iki sınıflı Bayes noktası makinesi](https://msdn.microsoft.com/library/azure/dn905930.aspx)) ve gerileme ([Bayes doğrusal regresyon](https://msdn.microsoft.com/library/azure/dn906022.aspx)).
Bu veri bölebilir veya düz bir çizgi uygun olduğunu varsayın unutmayın.

Geçmiş bir not üzerinde Bayes noktası makineleri Microsoft Research'te geliştirilen. Bazı olağanüstü güzel teorik iş arkasına sahiptirler. İsteyen Öğrenci yönlendirildiği [JMLR özgün makalesinde](http://jmlr.org/papers/volume1/herbrich01a/herbrich01a.pdf) ve [Chris Güneş tarafından bilgilendirici blog](http://blogs.technet.com/b/machinelearning/archive/2014/10/30/embracing-uncertainty-probabilistic-inference.aspx).

### <a name="specialized-algorithms"></a>Özel algoritmaların
Belirli bir hedefe varsa günümüzdeyiz! tam da olabilir. Azure Machine Learning koleksiyonda olarak uzmanlaşmış algoritmaları vardır:

- Tahmin Rank ([sıralı regresyon](https://msdn.microsoft.com/library/azure/dn906029.aspx)),
- Tahmin sayısı ([Poisson regresyon](https://msdn.microsoft.com/library/azure/dn905988.aspx)),
- anomali algılama (bir temel alarak [asıl bileşenlerini analiz](https://msdn.microsoft.com/library/azure/dn913102.aspx) ve bir temel [vektör makineler Destek](https://msdn.microsoft.com/library/azure/dn913103.aspx))
- Kümeleme ([K-ortalamaları](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/))

![PCA tabanlı anomali algılama][8]

***PCA tabanlı anomali algılama*** *-veri büyük çoğunluğu stereotipik dağıtım döner; bu dağıtım noktasından önemli ölçüde deviating şüpheli noktalarıdır*

![Veri kümesi K-ortalamaları kullanarak gruplanır.][9]

***Bir veri kümesi K-ortalamaları kullanarak beş kümeler halinde gruplandırılır.***

Ayrıca bir topluluğu vardır [bir v tüm çok sınıflı sınıflandırıcı](https://msdn.microsoft.com/library/azure/dn905887.aspx), N-1 iki sınıflı sınıflandırma sorunlarla karşılaşırsanız N sınıflı sınıflandırma problemi keser. Doğruluk, eğitim süresini ve Doğrusallık özellikleri kullanılan iki sınıflı sınıflandırıcılar tarafından belirlenir.

![Üç düzeyde sınıflandırıcı oluşturmak için bir araya iki sınıflı sınıflandırıcı][10]

***Bir çift iki sınıflı sınıflandırıcılar birleştiren üç sınıf sınıflandırıcı oluşturmak için***

Azure Machine Learning bir güçlü makine öğrenimi framework başlığının altında erişim yöntemlerine [Vowpal Wabbit](https://msdn.microsoft.com/library/azure/8383eb49-c0a3-45db-95c8-eb56a1fef5bf).
VW Burada, kategori Koşullarımız, Sınıflandırma ve regresyon hem sorunları edinebilirsiniz ve hatta kısmen etiketlenmemiş verilerden bilgi edinebilirsiniz. Öğrenme algoritmaları, kayıp işlevleri ve iyileştirme algoritmalarını sayısı herhangi birini kullanmak üzere yapılandırabilirsiniz. Bunu baştan yukarı verimli, paralel ve son derece hızlı olacak şekilde tasarlanmıştır. Bu, gerçekten büyük özellik kümeleri görünen çok az çabayla işler.
Başlatılan ve Microsoft Research'ün kendi John Langford tarafından yürütülen VW hisse senedi araba algoritmaları alanındaki formülü bir bir giriştir. Her sorun VW uyar, ancak Sizinkinde varsa, kendi arabiriminde öğrenme eğrisini tırmanan, while değer olabilir. Ayrıca kullanılabilir olarak [tek başına bir açık kaynak kod](https://github.com/JohnLangford/vowpal_wabbit) çeşitli dillerde.

## <a name="next-steps"></a>Sonraki Adımlar

* Anlaşılması kolay bilgi grafiği machine learning temel bilgileri yaygın machine learning soruları yanıtlamak için kullanılan popüler algoritmaları hakkında bilgi edinmek için genel bakış indirmek için bkz [makine öğrenimi algoritma örnekleri ile Temelleri](basics-infographic-with-algorithm-examples.md).

* Machine Learning Studio'da kullanılabilen tüm makine öğrenimi algoritma kategoriye göre bir listesi için bkz [modeli Başlat](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/machine-learning-initialize-model) Machine Learning Studio algoritma ve modül Yardımı.

* Bir tam alfabetik listesi algoritmaları ve Machine Learning Studio'daki modüller için bkz: [Machine Learning Studio modüllerinin A-Z listesi](https://docs.microsoft.com/azure/machine-learning/studio-module-reference/a-z-module-list) Machine Learning Studio algoritma ve modül Yardımı.

* İndirin ve Machine Learning Studio özelliklerine genel bir fikir veren bir diyagramını Yazdır: [Microsoft Azure Machine Learning Studio işlevlerine genel bakış (PDF)](https://download.microsoft.com/download/C/4/6/C4606116-522F-428A-BE04-B6D3213E9E52/ml_studio_overview_v1.1.pdf).

<!-- Media -->

[1]: ./media/algorithm-choice/image1.png
[2]: ./media/algorithm-choice/image2.png
[3]: ./media/algorithm-choice/image3.png
[4]: ./media/algorithm-choice/image4.png
[5]: ./media/algorithm-choice/image5.png
[6]: ./media/algorithm-choice/image6.png
[7]: ./media/algorithm-choice/image7.png
[8]: ./media/algorithm-choice/image8.png
[9]: ./media/algorithm-choice/image9.png
[10]: ./media/algorithm-choice/image10.png
