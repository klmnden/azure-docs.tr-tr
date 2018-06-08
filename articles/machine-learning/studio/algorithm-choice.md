---
title: Machine learning algoritmaları seçme | Microsoft Docs
description: Azure Machine Learning algoritmaları denetimli ve Denetimsiz öğrenme için kümeleme, sınıflandırma veya regresyon denemeler seçmek nasıl.
services: machine-learning
documentationcenter: ''
author: pakalra
ms.author: pakalra
manager: cgronlun
editor: cgronlun
tags: ''
ms.assetid: a3b23d7f-f083-49c4-b6b1-3911cd69f1b4
ms.service: machine-learning
ms.component: studio
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 12/18/2017
ms.openlocfilehash: 79b2cc3951fa8a48282f42f7180ec831050508f8
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34834391"
---
# <a name="how-to-choose-algorithms-for-microsoft-azure-machine-learning"></a>Microsoft Azure Machine Learning için algoritma seçme
"Ne makine öğrenme algoritmasının kullanmalıyım?" sorusunun yanıtını her zaman "Bu bağlıdır." olur Boyut, kalite ve veri yapısını bağlıdır. Bu Yanıtla yapmak istediğiniz yere bağlıdır. Bu, nasıl algoritma matematik yönergeler için kullanmakta olduğunuz bilgisayar içine çevrilmiştir üzerinde bağlıdır. Ve bağlı olduğu üzerinde ne kadar süre sahip. Hatta çoğu deneyimli veri bilimcilerine algoritmayı en iyi denemeden önce gerçekleştirecek bildiremez.

## <a name="the-machine-learning-algorithm-cheat-sheet"></a>Machine Learning algoritmasını kopya sayfası
**Microsoft Azure Machine Learning algoritmasını kopya sayfası** sağa seçtiğiniz yardımcı makine öğrenme algoritmasını Microsoft Azure Machine Learning algoritmaları kitaplığından Tahmine dayalı analiz çözümleriniz için.
Bu makalede aracılığıyla nasıl kullanılacağı anlatılmaktadır.

> [!NOTE]
> Kopya sayfası karşıdan yüklemek ve bu makaleyi izlemek için Git [makine için Microsoft Azure Machine Learning Studio'da algoritması bilgi sayfası öğrenme](algorithm-cheat-sheet.md).
> 
> 

Bu kopya sayfası çok belirli bir hedef kitle göz önünde sahiptir: Azure Machine Learning Studio'da başlamak algoritma seçme çalışılırken başına veri Bilimcisi lisans Öğrencisi düzeyi machine learning ile. Bazı Genelleştirme ve oversimplifications kolaylaştırır, ancak güvenli bir yönde işaret eden anlamına gelir. Ayrıca, çok sayıda burada listelenmeyen algoritmaları olduğu anlamına gelir. Azure Machine Learning kullanılabilir yöntemleri daha eksiksiz bir kümesini kapsayacak şekilde büyüdükçe, bunları ekleyeceğiz.

Bu, derlenmiş geri bildirim ve çok sayıda veri bilimcilerine ve makine öğrenme uzmanlar ipuçlarından önerilerdir. Biz her şeyi kabul alamadık, ancak bizim görüşlerini kaba anlaşma harmonize çalıştınız. "Bağlı olduğu ile..." çözünürlüğünün bilgilerinin çoğunu başlayın

### <a name="how-to-use-the-cheat-sheet"></a>Kopya sayfası kullanma
Grafik yolu ve algoritma etiketleri okuma "için  *&lt;yol etiketinin&gt;*, kullanın  *&lt;algoritması&gt;*." Örneğin, "için *hızı*, kullanın *iki Lojistik regresyon sınıf*." Bazen birden fazla dalı geçerlidir.
Bazen bunları bir mükemmel yok. Bunlar tam olan bu konuda endişelenmeyin böylece Flash kural öneriler olarak kullanılması.
Çeşitli veri bilimcilerine t ile konusu açıklandı en iyi algoritması bulmak için yalnızca emin bunların tümünün denemek için yoludur.

Bir örnek şudur [Azure AI galeri](http://gallery.cortanaintelligence.com/) aynı verilere göre çeşitli algoritmalar çalışır ve sonuçları karşılaştırır deneme: [çok sınıfı sınıflandırıcı karşılaştırın: Harf tanıma](http://gallery.cortanaintelligence.com/Details/a635502fc98b402a890efe21cec65b92).

> [!TIP]
> Machine Learning Studio'nun işlevlerine genel bir bakış sağlayan bir diyagram indirmek ve yazdırmak için bkz. [Azure Machine Learning Studio'nun işlevlerine genel bakış diyagramı](studio-overview-diagram.md).
> 
> 

## <a name="flavors-of-machine-learning"></a>Machine learning özellikleri
### <a name="supervised"></a>Denetimli
Denetimli öğrenme algoritmaları örnekler kümesine göre tahminlerde. Örneğin, geçmiş hisse senedi fiyatları gelecekteki fiyatlarla hasar tahmin için kullanılabilir. Eğitim için kullanılan her örnek ilgi değeriyle etiketli — Bu hisse senedi fiyatı durumda. Bu değer etiketleri düzenleri denetimli öğrenme algoritmasını arar. İlgili olabilecek herhangi bir bilgi kullanabilirsiniz — haftanın, mevsimin, şirketin finansal verileri, endüstri türünü, kesintiye uğratan jeopolitik olay gününü — ve her algoritması desenleri farklı türleri için görünür. Algoritma en iyi düzeni bulduktan sonra bunu, etiketlenmemiş test verileri için tahminde yapmak için bu deseni kullanır — yarın'ın fiyatlar.

Denetimli öğrenme machine learning popüler ve kullanışlı bir türde değil. Bunun tek istisnası tüm modüllerdeki Azure Machine Learning algoritmaları öğrenme denetimli. Azure Machine Learning içinde temsil birkaç belirli denetimli öğrenme tür vardır: Sınıflandırma, regresyon ve anomali algılama.

* **Sınıflandırma**. Verileri bir kategori tahmin etmek için kullanılıyorsa, denetimli öğrenme sınıflandırma da adlandırılır. Bu görüntü resmini 'Kat' veya 'köpek' atarken durumdur. Yalnızca iki seçenek olduğunda çağrılır **iki sınıflı** veya **iki terimli sınıflandırma**. Olarak olduğunda daha fazla kategori NCAA Mart Madness TURNUVASI kazanan tahmin etmeye olduğunda, bu sorunu olarak bilinir **çok sınıfı sınıflandırma**.
* **Regresyon**. Bir değer, gibi stok fiyatlarla tahmin denetimli öğrenme regresyon denir.
* **Anomali algılama**. Bazı durumlarda yalnızca olağan dışı veri noktalarını tanımlamak için belirtilir. Sahtekarlık algılama, örneğin, şüpheli tüm oldukça olağandışıdır kredi kartı harcama desenleri alır. Olası değişimler kadar çok sayıda ve bu nedenle birkaç, sahte hangi etkinlik öğrenmek için uygun olmadığı, eğitim örnekleri görünür gibi. Anomali algılama alan sadece (Geçmiş sahte olmayan işlemleri kullanarak gibi) hangi normal etkinlik arar öğrenin ve önemli ölçüde farklı olan her şeyi tanımlamak için yaklaşımdır.

### <a name="unsupervised"></a>Denetimsiz
Denetimsiz öğrenme içinde veri noktaları ilişkili hiçbir etiket vardır. Bunun yerine, bir Denetimsiz öğrenme algoritmasını verileri bazı şekilde düzenlemek için veya yapısını açıklamak için hedefidir. Kümeler halinde gruplandırmak veya basit ya da daha düzenli görünmesi karmaşık veri arayan farklı yöntemler bulma anlamına gelebilir.

### <a name="reinforcement-learning"></a>Öğrenmeyi öğrenme
Öğrenme öğrenmeyi içinde yanıt her veri noktası için bir eylem seçmenizi algoritma alır. Öğrenme algoritmasını de ödül sinyal kısa ne kadar iyi kararı olduğunu belirten bir süre sonra alır.
Algoritma bunu temel alarak, yüksek ödül elde etmek için kendi stratejisi değiştirir. Şu anda Azure Machine Learning'de algoritma modülleri öğrenme hiçbir öğrenmeyi vardır. Burada zaman içinde bir noktada sensör okumaları veri noktası kümesidir ve algoritma robot kullanıcının bir sonraki eylem seçmeniz gerekir robotics öğrenmeyi öğrenme yaygındır. Ayrıca, bir doğal uygulamaları şeyler Internet için uygun değildir.

## <a name="considerations-when-choosing-an-algorithm"></a>Bir algoritma seçerken dikkat edilecek noktalar
### <a name="accuracy"></a>Doğruluk
En doğru yanıt olası alma her zaman gerekli değildir.
Bazen yaklaşık kullanmak istediğinize bağlı olarak, yeterlidir. Bu durumda, işleme süresini önemli ölçüde daha fazla yaklaşık yöntemleriyle kalmanız tarafından kesme mümkün olabilir. Daha fazla yaklaşık yöntemleri başka bir avantajı bunlar doğal olarak önlemek için eğilimindedir olan [overfitting](https://youtu.be/DQWI1kvmwRg).

### <a name="training-time"></a>Eğitim süresini
Kaç dakika veya saat bir modeli eğitmek için gereken büyük miktarda algoritmaları arasında değişir. Zaman eğitim genellikle yakından bağlı tutarlılık — bir genellikle eşlik eden diğer. Ayrıca, bazı algoritmaları diğerlerinden daha fazla veri noktası sayısı duyarlıdır.
Zaman sınırlı olduğunda özellikle veri kümesi büyük olduğunda algoritması seçiminde sürücü.

### <a name="linearity"></a>Doğrusallık
Machine learning algoritmaları çok sayıda olun Doğrusallık kullanın. Doğrusal sınıflandırma algoritmaları bir çizgide (veya daha yüksek boyutlu analog) sınıfları ayrılabilir varsayalım. Bunlar Lojistik regresyon ve vektör makineler (Azure Machine Learning ile uygulanan gibi) destek içerir.
Doğrusal regresyon algoritması, veri eğilimleri bir çizgide izleyin varsayalım. Bu varsayımları bazı sorunlar için hatalı değildir, ancak bazılarında bunlar doğruluğu getir.

![Doğrusal Dışı sınıfı sınır][1]

***Doğrusal Dışı sınıfı sınır*** *-bir doğrusal sınıflandırma algoritmasıdır bağlı olan düşük doğruluk sonuçlanacak*

![Doğrusal eğilim verileri][2]

***Doğrusal eğilim verilerle*** *-doğrusal regresyon yöntemini kullanarak gereken daha büyük kadar hataları oluşturmak*

Kendi tehlikeleri rağmen doğrusal algoritmaları çok saldırı ilk satır yaygındır. Bunlar algorithmically basit ve hızlı eğitmek için olma eğilimindedir.

### <a name="number-of-parameters"></a>Parametre sayısı
Bir algoritma ayarlarken açmak için bir veri Bilimcisi alır düğmelerini parametreleridir. Bunlar hata toleransı veya yineleme veya algoritma nasıl davranacağını, çeşitler arasında seçenekleri sayısı gibi algoritması'nin davranışını etkilemek numaralarıdır. Algoritma doğruluğunu ve eğitim saat bazen yalnızca hakkı ayarlarını almak için oldukça önemli olabilir. Genellikle, çok sayıda parametre algoritmalarıyla iyi bir kombinasyonu bulmak için çoğu deneme ve hata gerektirir.

Alternatif olarak, var olan bir [parametresi Süpürme](algorithm-parameters-optimize.md) modülü bloğunda Azure Machine Learning'de, seçtiğiniz herhangi bir ayrıntı düzeyi, tüm parametresi birleşimleri otomatik olarak çalışır. Bu parametre alan yayılmış emin olmak için kullanışlı bir yoludur olmakla birlikte, bir modeli eğitmek için gereken süreyi katlanarak parametre sayısı artar.

Baş fazla parametre genellikle sahip bir algoritma esneklik olduğunu gösteren sayıdır. Genellikle çok iyi doğruluğu elde edebilirsiniz. Sağlanan parametre ayarları doğru bileşimini bulabilirsiniz.

### <a name="number-of-features"></a>Özellikleri sayısı
Belirli veri türleri, özellik sayısı veri noktası sayısı kıyasla çok büyük olabilir. Bu genellikle genetics veya metin veri durumdur. Özellikler çok sayıda unfeasibly uzun zaman eğitim yapmadan bazı learning algoritmaları bog. Destek vektör makineler bu durumda (aşağıya bakın) özellikle uygundur.

### <a name="special-cases"></a>Özel durumlar
Bazı learning algoritmaları veri ya da istenen sonuçları yapısı hakkında belirli varsayımlar olun. Gereksinimlerinize uyan bir fark ederseniz, bu, daha yararlı sonuçlar, daha doğru tahminleri ya da daha hızlı eğitim süreleri verebilirsiniz.

| **Algoritma** | **Doğruluk** | **Eğitim süresini** | **Doğrusallık** | **Parametreler** | **Notlar** |
| --- |:---:|:---:|:---:|:---:| --- |
| **İki sınıflı sınıflandırma** | | | | | |
| [Lojistik regresyon](https://msdn.microsoft.com/library/azure/dn905994.aspx) | |● |● |5 | |
| [karar orman](https://msdn.microsoft.com/library/azure/dn906008.aspx) |● |○ | |6 | |
| [karar jungle](https://msdn.microsoft.com/library/azure/dn905976.aspx) |● |○ | |6 |Düşük bellek alanı |
| [Artırılmış karar ağacı](https://msdn.microsoft.com/library/azure/dn906025.aspx) |● |○ | |6 |Büyük bellek alanı |
| [sinir ağı](https://msdn.microsoft.com/library/azure/dn905947.aspx) |● | | |9 |[Ek özelleştirme mümkündür](http://go.microsoft.com/fwlink/?LinkId=402867) |
| [Ortalama perceptron](https://msdn.microsoft.com/library/azure/dn906036.aspx) |○ |○ |● |4 | |
| [vektör makinesi desteği](https://msdn.microsoft.com/library/azure/dn905835.aspx) | |○ |● |5 |Büyük özellik kümeleri için iyi |
| [yerel olarak ayrıntılı destek vektör makinesi](https://msdn.microsoft.com/library/azure/dn913070.aspx) |○ | | |8 |Büyük özellik kümeleri için iyi |
| [Bayes noktası makinesinin](https://msdn.microsoft.com/library/azure/dn905930.aspx) | |○ |● |3 | |
| **Birden çok sınıf sınıflandırma** | | | | | |
| [Lojistik regresyon](https://msdn.microsoft.com/library/azure/dn905853.aspx) | |● |● |5 | |
| [karar orman](https://msdn.microsoft.com/library/azure/dn906015.aspx) |● |○ | |6 | |
| [karar jungle ](https://msdn.microsoft.com/library/azure/dn905963.aspx) |● |○ | |6 |Düşük bellek alanı |
| [sinir ağı](https://msdn.microsoft.com/library/azure/dn906030.aspx) |● | | |9 |[Ek özelleştirme mümkündür](http://go.microsoft.com/fwlink/?LinkId=402867) |
| [biri v tümü](https://msdn.microsoft.com/library/azure/dn905887.aspx) |- |- |- |- |Seçilen iki sınıflı yöntemi özelliklerini bakın |
| **regresyon** | | | | | |
| [Doğrusal](https://msdn.microsoft.com/library/azure/dn905978.aspx) | |● |● |4 | |
| [Bayesian doğrusal](https://msdn.microsoft.com/library/azure/dn906022.aspx) | |○ |● |2 | |
| [karar orman](https://msdn.microsoft.com/library/azure/dn905862.aspx) |● |○ | |6 | |
| [Artırılmış karar ağacı](https://msdn.microsoft.com/library/azure/dn905801.aspx) |● |○ | |5 |Büyük bellek alanı |
| [Hızlı orman quantile](https://msdn.microsoft.com/library/azure/dn913093.aspx) |● |○ | |9 |Noktası tahminleri yerine dağıtımları |
| [sinir ağı](https://msdn.microsoft.com/library/azure/dn905924.aspx) |● | | |9 |[Ek özelleştirme mümkündür](http://go.microsoft.com/fwlink/?LinkId=402867) |
| [Poisson](https://msdn.microsoft.com/library/azure/dn905988.aspx) | | |● |5 |Teknik olarak günlük doğrusal. Sayıları etmede |
| [sıra sayısı](https://msdn.microsoft.com/library/azure/dn906029.aspx) | | | |0 |RANK sıralama etmede |
| **Anormallik algılama** | | | | | |
| [vektör makinesi desteği](https://msdn.microsoft.com/library/azure/dn913103.aspx) |○ |○ | |2 |Büyük özellik kümeleri için özellikle iyi |
| [PCA tabanlı anomali algılama](https://msdn.microsoft.com/library/azure/dn913102.aspx) | |○ |● |3 | |
| [K-ortalamaları](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/) | |○ |● |4 |Bir kümeleme algoritması |

**Algoritma özellikleri:**

**●** -mükemmel doğruluğu, hızlı eğitim kez ve Doğrusallık kullanımını gösterir

**○** -iyi doğruluk ve orta eğitim süreleri gösterir

## <a name="algorithm-notes"></a>Algoritma notları
### <a name="linear-regression"></a>Çizgisel regresyon
Daha önce belirtildiği gibi [doğrusal regresyon](https://msdn.microsoft.com/library/azure/dn905978.aspx) bir satırı (veya düzlemi veya hyperplane) veri kümesine uyan. Basit ve hızlı, workhorse olduğu, ancak bazı sorunlar için aşırı simplistic olabilir.
Burada denetle bir [doğrusal regresyon Öğreticisi](linear-regression-in-azure.md).

![Doğrusal eğilim verileri][3]

***Doğrusal eğilim verileri***

### <a name="logistic-regression"></a>Lojistik regresyon
Confusingly adlarında 'regresyon' içeren Lojistik regresyon gerçekten güçlü bir araç için olsa da [iki sınıflı](https://msdn.microsoft.com/library/azure/dn905994.aspx) ve [veya çoklu sınıflar](https://msdn.microsoft.com/library/azure/dn905853.aspx) sınıflandırma. Hızlı ve kolay bir işlemdir. Kullandığı gerçek bir kişinin '-şekilli eğri bir çizgide yerine gruplar halinde veri bölmek için doğal bir uyum sağlar. Kullandığınızda Lojistik regresyon verir doğrusal sınıfı sınırlar, bu nedenle doğrusal yaklaşık ile canlı bir şey olduğundan emin olun.

![Tek bir özellik olan iki sınıflı verilere Lojistik regresyon][4]

***Tek bir özellik olan iki sınıflı verilere Lojistik regresyon*** *-sınıfı sınır Lojistik eğri olduğu yalnızca her iki sınıflar olarak yakın noktasıdır*

### <a name="trees-forests-and-jungles"></a>Ağaçları, ormanlar ve ormanları
Karar ormanları ([regresyon](https://msdn.microsoft.com/library/azure/dn905862.aspx), [iki sınıflı](https://msdn.microsoft.com/library/azure/dn906008.aspx), ve [veya çoklu sınıflar](https://msdn.microsoft.com/library/azure/dn906015.aspx)), karar ormanları ([iki sınıflı](https://msdn.microsoft.com/library/azure/dn905976.aspx) ve [ veya çoklu sınıflar](https://msdn.microsoft.com/library/azure/dn905963.aspx)) ve karar ağaçları boosted ([regresyon](https://msdn.microsoft.com/library/azure/dn905801.aspx) ve [iki sınıflı](https://msdn.microsoft.com/library/azure/dn906025.aspx)) tüm karar ağaçları, temel machine learning kavram temel alır. Karar ağaçları, çok sayıda çeşitlemesi vardır, ancak bunların tümü aynı işlevi görür — çoğunlukla aynı etiketle bölgeleri özellik alanı ayırabilir. Bu bölgeler tutarlı kategori veya sınıflandırma veya regresyon yapmakta olduğunuz bağlı olarak sabit değer olabilir.

![Karar ağacında bir özellik alanı subdivides][5]

***Karar ağacında bir özellik alanı kabaca Tekdüzen değerleri bölgelere subdivides***

Bir özellik alanı rasgele küçük bölgelere bölünmüştür çünkü yeterince ince bölge başına veri noktası için bölme düşünün kolaydır. Bu aşırı overfitting örneğidir. Bu durumu önlemek için çok sayıda ağaçları oluşturulur geçen özel matematiksel dikkatli ağaçları değil bağıntılı. Bu "karar ormanı" ortalama overfitting engelleyen bir ağacıdır. Karar ormanları çok miktarda bellek kullanabilir. Karar ormanları biraz daha uzun bir eğitim süresini ödün verme pahasına daha az bellek tüketir bir VARIANT ' dir.

Artırılmış karar ağaçları kaç kez ayırabilir ve her bölgede nasıl birkaç veri noktası izin verilir sınırlayarak overfitting kaçının. Algoritma ağaçları, her biri için önce ağacı tarafından sol hata dengelemek için öğrenir dizisi oluşturur. Çok miktarda bellek kullanmak eğilimindedir çok doğruysa bir öğrenen sonucudur. Tam teknik açıklamasını kullanıma [Friedman'ın özgün kağıt](http://www-stat.stanford.edu/~jhf/ftp/trebst.pdf).

[Hızlı orman quantile regresyon](https://msdn.microsoft.com/library/azure/dn913093.aspx) karar ağaçları, bölge, aynı zamanda, dağıtım quantiles biçiminde içindeki verilere yalnızca tipik (ORTANCA) değerini bilmek istediğiniz özel bir durum için bir çeşididir.

### <a name="neural-networks-and-perceptrons"></a>Sinir ağları ve perceptrons
Sinir ağları beyin-neden olacak kapsayan algoritmaları öğrenme [veya çoklu sınıflar](https://msdn.microsoft.com/library/azure/dn906030.aspx), [iki sınıflı](https://msdn.microsoft.com/library/azure/dn905947.aspx), ve [regresyon](https://msdn.microsoft.com/library/azure/dn905924.aspx) sorunları. Sonsuz bir çeşitlilik gelir, ancak Azure Machine Learning içinde sinir ağları yönlendirilmiş Çevrimsiz grafik form tümü. Giriş özellikleri ileri (hiçbir zaman geri) katmanları bir dizi çıkışları açık önce aktarılmasını anlamına gelir. Her katmanda girişleri çeşitli bileşimlerde ağırlıklı, toplanır ve sonraki katmana geçirildi. Bu basit hesaplamalar bileşimi Gelişmiş sınıfı sınırları ve veri eğilimleri görünen tarafından Sihirli öğrenin olanağı sonuçlanır. Çok katmanlı ağlar bu tür "çok teknik raporlama ve Bilim Kurgu fuels derin öğrenme" gerçekleştirin.

Bu yüksek performanslı ücretsiz, ancak gelmez. Sinir ağları özelliklerinin çok büyük veri kümeleri için özellikle eğitmek için uzun sürebilir. Bunlar ayrıca parametre Süpürme eğitim süresini büyük miktarda genişletir anlamına gelir çoğu algoritmaları sayısından daha fazla parametre vardır.
Ve isteyen bu overachievers [kendi ağ yapısı belirtmek](http://go.microsoft.com/fwlink/?LinkId=402867), olanakları inexhaustible.

![Sınırları öğrenilen sinir ağları tarafından][6]
***sinir ağları tarafından öğrenilen sınırları karmaşık ve düzensiz olabilir***

[Perceptron iki sınıflı ortalaması](https://msdn.microsoft.com/library/azure/dn906036.aspx) skyrocketing eğitim saatler sinir ağları yanıt. Doğrusal sınıfı sınırları sağlayan bir ağ yapısını kullanır. Günümüzün standartlarıyla neredeyse ilkel, ancak yerine çalışma uzun bir geçmişi vardır ve hızla bilgi edinmek için küçük.

### <a name="svms"></a>SVMs
Destek vektör makineleri (SVMs) geniş bir kenar boşluğu mümkün olduğunca olarak sınıfları tarafından ayıran sınır bulun. İki sınıf açıkça ayrılmış olamaz algoritmaları yapabilir en iyi sınır bulur. Azure Machine Learning ile yazılmış olarak [iki sınıflı SVM](https://msdn.microsoft.com/library/azure/dn905835.aspx) bunu yalnızca düz bir çizgi ile yapar. (SVM seslendir içinde doğrusal çekirdek kullandığı.) Bu Doğrusal yaklaşık yaptığından oldukça hızlı bir şekilde çalıştırabilir. Burada gerçekten inanılmaz özelliği yoğun metin gibi veya genomic ile veridir. Bu durumlarda SVMs daha hızlı ve yalnızca uygun miktarda bellek gerektiren ek olarak çoğu diğer algoritmalar, daha az overfitting sınıflarını ayırmak mümkün.

![Destek vektör makinesi sınıfı sınır][7]

***Normal destek vektör makinesi sınıfı sınır iki sınıf ayırarak kenar boşluğu en üst düzeye çıkarır.***

Microsoft Research'ün başka bir ürün [iki sınıflı yerel olarak derin SVM](https://msdn.microsoft.com/library/azure/dn913070.aspx) doğrusal olmayan bir doğrusal sürüm hızı ve bellek verimliliğini çoğunu korur SVM çeşididir. Doğrusal yaklaşım yeterince doğru yanıtlar burada vermediğinin durumları için idealdir. Geliştiriciler, bir grup küçük doğrusal SVM sorunları sorunla hızlı bölmek tarafından tutulur. Okuma [tam açıklama](http://proceedings.mlr.press/v28/jose13.html) nasıl bunlar bu eli çekilen hakkında bilgi.

Doğrusal SVMs, akıllı uzantısını kullanarak [bir sınıf SVM](https://msdn.microsoft.com/library/azure/dn913103.aspx) sıkı bir şekilde tüm veri kümesinin özetlenmektedir bir sınır çizer. Anomali algılama için yararlı olacaktır. Şu ana kadar bu sınırının dışında kalan herhangi bir yeni veri noktaları dikkate değer olacak şekilde alışılmadık.

### <a name="bayesian-methods"></a>Bayesian yöntemleri
Bayesian yöntemlerine sahip yükseltebilirsiniz kalitesi: overfitting kaçının. Önceden yanıt büyük olasılıkla dağıtılması hakkında bazı varsayımlar yaparak bunu. Bu yaklaşımın başka bir byproduct çok az parametrelere sahip olur. Azure Machine Learning sahip iki sınıflandırma için her iki Bayesian algoritmalar ([iki sınıflı Bayes noktası makinesi](https://msdn.microsoft.com/library/azure/dn905930.aspx)) ve gerileme ([Bayesian doğrusal regresyon](https://msdn.microsoft.com/library/azure/dn906022.aspx)).
Bu veri yüklenebilir bölme veya bir çizgide uygun olduğunu varsayın unutmayın.

Geçmiş bir not üzerinde Bayes noktası makineler Microsoft Research'te geliştirilen. Bazı olağanüstü güzel teorik iş arkasına sahiptirler. İlginizi Öğrenci yönlendirildiği [JMLR özgün makaleye](http://jmlr.org/papers/volume1/herbrich01a/herbrich01a.pdf) ve bir [Chris Güneş tarafından ayrıntılı blog](http://blogs.technet.com/b/machinelearning/archive/2014/10/30/embracing-uncertainty-probabilistic-inference.aspx).

### <a name="specialized-algorithms"></a>Özelleştirilmiş algoritmaları
Belirli bir hedefe sahip değilse Şanslar olabilir. Azure Machine Learning koleksiyonunda içinde specialize algoritmaları vardır:

- RANK tahmin ([sıralı regresyon](https://msdn.microsoft.com/library/azure/dn906029.aspx)),
- Tahmin sayısı ([Poisson regresyon](https://msdn.microsoft.com/library/azure/dn905988.aspx)),
- anomali algılama (birini temel alarak [asıl bileşenlerini analiz](https://msdn.microsoft.com/library/azure/dn913102.aspx) ve temel bir [destek vektör makinesi](https://msdn.microsoft.com/library/azure/dn913103.aspx)s)
- Kümeleme ([K-ortalamaları](https://msdn.microsoft.com/library/azure/5049a09b-bd90-4c4e-9b46-7c87e3a36810/))

![PCA tabanlı anomali algılama][8]

***PCA tabanlı anomali algılama*** *-stereotipik dağıtım veri çoğunluğu döner; bu dağıtım noktasından önemli ölçüde deviating şüpheli noktalarıdır*

![Veri kümesi K-ortalamaları kullanarak gruplandırılmış][9]

***Bir veri kümesi K-ortalamaları kullanarak beş kümeler halinde gruplandırılır***

Ayrıca bir ensemble olan [biri v tümü çok sınıflı sınıflandırıcı](https://msdn.microsoft.com/library/azure/dn905887.aspx), N-1 iki sınıflı sınıflandırma sorunlarla N sınıfı sınıflandırma sorunu keser. Doğruluğu, eğitim süresini ve Doğrusallık özellikleri kullanılan iki sınıflı sınıflandırıcı tarafından belirlenir.

![Üç sınıfı sınıflandırıcı oluşturmak için bir araya iki sınıflı sınıflandırıcı][10]

***İki sınıflı sınıflandırıcı çifti birleştirmek üç sınıfı sınıflandırıcı oluşturmak için***

Azure Machine Learning de güçlü machine learning framework başlığı altında erişimi içerir [Vowpal Wabbit](https://msdn.microsoft.com/library/azure/8383eb49-c0a3-45db-95c8-eb56a1fef5bf).
Sınıflandırma ve regresyon sorunları öğrenebilirsiniz ve kısmen etiketlenmemiş verilerden bile öğrenebilirsiniz VW burada kategori Koşullarımız. Algoritmalar, kaybı işlevleri ve en iyi duruma getirme algoritmaları öğrenme sayısı herhangi birini kullanacak şekilde yapılandırabilirsiniz. Bu sıfırdan yukarı verimli, paralel ve son derece hızlı olması için tasarlanmıştır. Gerçekten büyük özellik kümeleri az belirgin çabayla işler.
Başlatıldı ve Microsoft Research's kendi John Langford tarafından neden VW hisse senedi araba algoritmalarının bir alandaki formülü tek bir giriştir. Her sorun VW uygun ancak sizin varsa, kendi arabirimde öğrenme eğrisi tırmanan, while değer olabilir. Ayrıca olarak kullanılabilir [tek başına açık kaynak kodu](https://github.com/JohnLangford/vowpal_wabbit) çeşitli dillerde.

## <a name="more-help-with-algorithms"></a>Daha fazla yardıma algoritmaları
* Algoritmaları açıklar ve örnekler sağlayan bir indirilebilir bilgi grafiği için bkz: [indirilebilir bilgi grafiği: Makine öğrenme algoritmasını örneklerle temel](basics-infographic-with-algorithm-examples.md).
* Azure Machine Learning Studio'da kullanılabilen tüm makine öğrenimi algoritma kategoriye göre bir listesi için bkz: [modeli Başlat] [ initialize-model] Machine Learning Studio algoritması ve modülü Yardımı'nda.
* Bir tam alfabetik listesi algoritmaları ve Azure Machine Learning Studio'daki modüller için bkz: [Machine Learning Studio modülleri A-Z listesi] [ a-z-list] Machine Learning Studio algoritması ve modülü Yardımı'nda.
* Karşıdan yükle ve Azure Machine Learning Studio özelliklerine genel bakış sağlayan bir diyagram yazdırmak için bkz: [Azure Machine Learning Studio işlevlerine genel bakış diyagramı](studio-overview-diagram.md).


<!-- Reference links -->
[initialize-model]: https://msdn.microsoft.com/library/azure/dn905812.aspx
[a-z-list]: https://msdn.microsoft.com/library/azure/dn906033.aspx

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
