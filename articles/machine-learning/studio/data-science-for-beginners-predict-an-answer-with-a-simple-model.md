---
title: Basit regresyon modeli - Azure Machine Learning ile bir yanıt tahmin | Microsoft Docs
description: Video 4 yeni başlayanlar için veri bilimi fiyatına tahmin etmek için bir basit regresyon modeli oluşturma Hedef verilerle doğrusal regresyon içerir.
keywords: bir model, basit modeli, fiyat tahmini, basit regresyon modeli oluşturma
services: machine-learning
documentationcenter: na
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cjgronlund
ms.assetid: a28f1fab-e2d8-4663-aa7d-ca3530c8b525
ms.service: machine-learning
ms.component: studio
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2018
ms.openlocfilehash: ad1b8369358f7811a02d344fdc0306662413a404
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34833834"
---
# <a name="predict-an-answer-with-a-simple-model"></a>Basit model ile yanıtı tahmin etme
## <a name="video-4-data-science-for-beginners-series"></a>Video 4: Yeni başlayanlar seri için veri bilimi
Veri bilimi video 4 yeni başlayanlar için de bir Karo fiyatını tahmin etmek için bir basit regresyon modeli oluşturmayı öğrenin. Biz hedef verilerle bir regresyon modeli çekersiniz.

Serinin en dışında almak için tümünü izleyin. [Videolar listesine Git](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/data-science-for-beginners-series-predict-an-answer-with-a-simple-model/player]
>
>

## <a name="other-videos-in-this-series"></a>Bu serideki diğer videolar
*Yeni başlayanlar için veri bilimi* veri bilimi beş kısa videolar içindeki bir giriş değil.

* Video 1: [5 veri bilimi yanıtlar sorular](data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 dakika 14 saniye)*
* Video 2: [verileriniz için veri bilimi hazır?](data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min 56 sn)*
* Video 3: [verilerle yanıt soru](data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sn)*
* Video 4: basit bir modelle bir yanıt tahmin etme
* Video 5: [veri bilimi yapmak için diğer kişilerin çalışma kopyalama](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 dakika 18 saniye)*

## <a name="transcript-predict-an-answer-with-a-simple-model"></a>Dökümü: basit bir modelle bir yanıt tahmin
"Veri bilimi için yeni başlayanlar" dördüncü videoda serisi Hoş Geldiniz. Bunu, biz basit bir model oluşturmak ve tahminde bulunmak.

A *modeli* verilerimizi ilgili basitleştirilmiş bir öykü olduğu. I ı anlamı göstereceğiz.

## <a name="collect-relevant-accurate-connected-enough-data"></a>İlgili, doğru bağlı, yeterli veri toplama
Alışverişi yapmak için bir Karo istiyorum söyleyin. My adı 1,35 ayar elmas için bir ayar ile ait bir halka sahip ve ne kadar maliyetlidir hakkında bir fikir edinmek istiyorum. I not defteri ve Kalem mücevher deposuna alın ve ı tüm durum ve bunların içinde carats ne kadar tartmanız Karo fiyat yazın. İlk baklava - cihazın 1.01 carats ve $7,366 başlatılıyor.

Şimdi geçtikleri ve diğer Karo deposundaki için bunu yap.

![Karo veri sütunlarının](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

Bizim listesi iki sütuna sahip olmadığına dikkat edin. Farklı bir öznitelik her sütununda - ağırlık carats ve fiyat - her satırda tek bir Karo temsil eden bir tek veri noktası.

Küçük bir gerçekte oluşturduk veri kümesi burada - bir tablo. Kalite bizim ölçütlerini karşılayan dikkat edin:

* Veri **ilgili** -ağırlık kesinlikle ilgili fiyatı
* Bunun **doğru** -biz biz yazma fiyatlar yeniden denetlediniz
* Bunun **bağlı** -boş boşluk yoktur ya bu sütunlar
* Ve anlatıldığı gibi var. **yeterli** bizim soruyu yanıtlamak için veri

## <a name="ask-a-sharp-question"></a>NET bir soru sorun
Şimdi biz bizim soru NET bir şekilde neden: "nasıl maliyeti 1,35 ayar elmas satın almak için?"

Biz yanıtını almak için verilerimizi kalan kullanmanız gerekir böylece bizim listesi 1,35 ayar elmas içinde yok.

## <a name="plot-the-existing-data"></a>Var olan verileri Çiz
Biz gerçekleştirirsiniz ilk ağırlıkları grafik eksen adı verilen yatay numarası çizgi, çizme şeydir. Biz, aralık ve yarı her ayar için çizgilerine put kapsayan bir satır çekersiniz şekilde ağırlıkları 0-2 aralığındadır.

Sonraki biz fiyat kaydetmek ve yatay ağırlık eksene bağlanmak için dikey eksen çekersiniz. Bu, ABD Doları birimlerinde olacaktır. Şimdi biz koordinat eksenleri kümesine sahiptir.

![Ağırlık ve fiyat eksenler](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

Şimdi bu verileri alabilir ve içine kapatma oluşturacağız bir *dağılım çizim*. Sayısal veri kümelerini görselleştirmek için harika bir yoludur.

İlk veri noktası için bir dikey satırında 1.01 carats eyeball. Ardından, $7,366 yatay satırında eyeball. Karşıladıklarından burada size bir nokta çizin. Bu bizim ilk elmas temsil eder.

Şimdi Biz bu listede her elmas gidin ve aynı işlevi görür. Biz işiniz bittiğinde, biz alma budur: bir grup noktalar, her Karo.

![Çizim dağılım](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-the-model-through-the-data-points"></a>Modelin veri noktaları aracılığıyla çizme
Nokta ve squint bakın, artık koleksiyon fat, benzer bir satır gibi durur. Bizim işaret alın ve düz bir çizgi geçen çizin.

Oluşturduğumuz bir satır çizerek bir *modeli*. Bu gerçek dünya alma ve bir simplistic çizgi sürümü yaparak olarak düşünün. Şimdi çizgi yanlış - satırın tüm veri noktaları aracılığıyla Git değil. Ancak yararlı basitleştirme değil.

![Doğrusal regresyon satır](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

Tüm nokta tam olarak satırın geçmez, olgu normaldir. Veri bilimcilerine açıklayan Bu model - satır - ve ardından her nokta bazı olduğundan söyleyerek *gürültü* veya *farkı* kendisiyle ilişkilendirilmiş. Temel alınan kusursuz ilişkisi yoktur ve ardından gürültü ve belirsizlik ekler Görünüşün, gerçek dünya yok.

Soruyu yanıtlamak çalıştığınız için *ne kadar?* bu adlı bir *regresyon*. Ve bir çizgide kullanmakta olduğunuz çünkü bu bir *doğrusal regresyon*.

## <a name="use-the-model-to-find-the-answer"></a>Model yanıt bulmak için kullanın
Bir model sahibiz ve biz bizim soru sorun artık: ne kadar 1,35 ayar elmas maliyetini?

Bizim soruyu yanıtlamak için 1,35 carats göz Küresi ve dikey çizgi çizme. Burada model satır kestiği biz dolar eksende yatay çizgi eyeball. Sağ taraftaki 10.000 kadardır. Müzik! Yanıt olan: 1,35 ayar elmas yaklaşık 10.000 $ maliyetleri.

![Model üzerinde yanıt Bul](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a>Güvenirlik aralığını oluşturma
Bu tahmin nasıl hassas olduğunu merak ediyor doğal. 1,35 ayar elmas çok $10.000 yakın olup olmayacağını bilmek yararlı veya çok daha yüksek veya düşük. Bunu bulmak için şimdi nokta çoğunu içeren regresyon satırı geçici Zarf çizin. Bu Zarf adlı bizim *güvenirlik aralığını*: çünkü fiyatlar bu Zarf içinde kalan oldukça emin ki bunların geçmiş çoğunda sahip. Üst ve alt bu zarfın 1,35 ayar satır burada kestiği biz iki daha yatay çizgiler çizebilirsiniz.

![Güvenirlik aralığını](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

Şimdi biz bir şeyler bizim güvenirlik aralığını hakkında söyleyin: güvenle 1,35 ayar elmas fiyat hakkında $10.000 - olur ancak 8000 kadar düşük olabilir ve 12.000 yüksek olabilir dediğimiz.

## <a name="were-done-with-no-math-or-computers"></a>Matematik ya da bilgisayarları bitmek
Hangi veri bilimcilerine yapmak için ödeme yaptığımız ve onu yalnızca çizerek yaptığımız:

* Biz biz verilerle yanıt bir soru sorular
* Oluşturduğumuz bir *modeli* kullanarak *doğrusal regresyon*
* Biz yapılan bir *tahmin*ile tam bir *güvenirlik aralığını*

Ve matematik veya bilgisayarlar yapmak için kullanırız alamadık.

Şimdi biz daha fazla bilgi olsaydı, ister...

* Karo Kes
* renk değişimleri (nasıl elmas beyaz olmaya yaklaştığını)
* elmas içindeki içermeler sayısı

...then biz olan daha fazla sütun. Bu durumda, Matematik yardımcı olur. İkiden fazla sütun varsa, kağıda noktalar çizmek zordur. Matematik, o satırdaki veya bu uçak verilerinize çok iyi sığacak olanak tanır.

Ayrıca, yalnızca birkaç Karo varsa, yerine biz iki bin veya iki milyon vardı sonra çalışan bir bilgisayarla çok daha hızlı yapabilirsiniz.

Bugün, doğrusal regresyon yapmak hakkında açıklandı ve biz verileri kullanarak bir tahmini yapılır.

"Veri bilimi için yeni başlayanlar" Microsoft Azure Machine learning'in diğer videoları kullanıma emin olun.

## <a name="next-steps"></a>Sonraki adımlar
* [İlk veri bilimi denemeyi Machine Learning Studio'da deneyin](create-experiment.md)
* [Microsoft Azure Machine Learning giriş Al](what-is-machine-learning.md)
