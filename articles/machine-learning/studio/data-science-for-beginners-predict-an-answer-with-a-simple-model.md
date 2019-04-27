---
title: Regresyon modellerini yanıtlarla tahmin edin
titleSuffix: Azure Machine Learning Studio
description: Nasıl bir veri bilimi video 4 yeni başlayanlar için fiyatı tahmin etmek için bir basit bir regresyon modeli oluşturun. Bir doğrusal regresyon hedef verilerle içerir.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: sdgilley
ms.author: sgilley
ms.custom: seodec18
ms.date: 03/22/2019
ms.openlocfilehash: 9165e51d07cf97756408c7f73720931abe067bb2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60751611"
---
# <a name="predict-an-answer-with-a-simple-model"></a>Basit model ile yanıtı tahmin etme
## <a name="video-4-data-science-for-beginners-series"></a>Video 4: Seri yeni başlayanlar için veri bilimi
Veri bilimi video 4 yeni başlayanlar için Karo fiyatı tahmin etmek için bir basit bir regresyon modeli oluşturmayı öğrenin. Biz bir regresyon modeli hedef verilerle çekersiniz.

En yetersiz serisi almak için tüm bunları izleyin. [Videoları listesine Git](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/data-science-for-beginners-series-predict-an-answer-with-a-simple-model/player]
>
>

## <a name="other-videos-in-this-series"></a>Bu serideki diğer videolar
*Yeni başlayanlar için veri bilimi* beş kısa videoyu veri bilimine hızlı bir giriş niteliğindedir.

* Video 1: [5 veri biliminin yanıtladığı sorular](data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 en az 14 sn)*
* Video 2: [Verileriniz veri bilimi için hazır mı?](data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 dk 56 sn)*
* Video 3: [Yanıt verileri ile soru](data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 dk 17 sn)*
* Video 4: Basit model ile yanıtı tahmin etme
* Video 5: [Veri bilimi için başkalarının çalışmalarını kopyalama](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) *(3 dk 18 sn)*

## <a name="transcript-predict-an-answer-with-a-simple-model"></a>Transkript: Basit model ile yanıtı tahmin etme
"Veri bilimi için yeni başlayanlar" içinde dördüncü video serisine Hoş Geldiniz. Bunu, biz basit bir model oluşturun ve tahminde bulunmak.

A *modeli* verilerimizi hakkında basitleştirilmiş bir hikaye. Ben ne demek istediğimi anlıyor göstereceğiz.

## <a name="collect-relevant-accurate-connected-enough-data"></a>İlgili, doğru bağlı, yeterli veri topla
Söyleyin için elmas alışverişi yapmak istiyorum. My NO@LOC 1,35 ayar elmas için bir ayar ile ait bir halka sahibim ve ne kadara mal hakkında bir fikir edinmek istiyorum. Ben bir not defteri ve Kalem mücevher özelliklerine deposuna yararlanın ve tüm durum ve bunlar carats içinde ne kadar Tart Karo fiyatını aşağı yazdığım. İlk baklava - onun 1.01 carats ve $7,366 başlatılıyor.

Artık Git ve depodaki tüm diğer Karo için bunu.

![Baklava veri sütunları](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/diamond-data.png)

Listemize iki sütun olduğuna dikkat edin. Her sütunda farklı bir öznitelikle - ağırlık carats ve fiyat - her satırda tek bir baklava temsil eden bir tek bir veri noktasıdır.

Küçük gerçekten oluşturduk veri kümesi burada - bir tablo. Bizim kalite ölçütlerini karşıladığını dikkat edin:

* Veriler **ilgili** -ağırlık kesinlikle ilgili fiyatı
* Sahip **doğru** -biz size yazma fiyatları yeniden denetlediniz
* Sahip **bağlı** -boş boşluk yoktur ya da bu sütunların
* Ve anlatıldığı gibi sahip **yeterli** bizim cevap

## <a name="ask-a-sharp-question"></a>NET bir soru sorun
Şimdi biz bizim soru NET bir şekilde konusunda sizi uyarmayı: "Ne kadar bu 1,35 ayar elmas satın alma maliyeti ne olacak?"

Verilerimizi geri kalanını sorusuna bir cevap almak için kullanılacak gerekir böylece listemize 1,35 ayar elmas içinde yok.

## <a name="plot-the-existing-data"></a>Mevcut veri çizimi
Şimdi ilk şey, yatay numarası ağırlıkları grafiğe bağlı olarak, bir eksen adı, çizgi çizme ' dir. Ağırlıkları aralığının 0-2 olduğundan biz aralığı ve saat döngüsü yarım her ayar için put kapsayan bir satır çekersiniz.

Sonraki biz fiyat kaydetmek ve Ağırlık yatay eksene bağlamak için dikey eksen çekersiniz. Bu birimleri cinsinden ABD Doları olacaktır. Artık bir dizi koordinat eksenleri sahibiz.

![Ağırlık ve fiyat eksenler](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/weight-and-price-axes.png)

Bu veriler artık alıp oturum açmak için yapacağız bir *dağılım grafiğinde noktalara*. Bu, sayısal veri kümeleri görselleştirmek için harika bir yoludur.

İlk veri noktası için size bir dikey çizgi 1.01 carats eyeball. Ardından, $7,366 yatay bir çizgi eyeball. Bunlar uygun olduğunda, size bir nokta çizin. Bu, bizim ilk elmas temsil eder.

Şimdi Biz bu listede her elmas gidin ve aynı şeyi yapmak. Biz işiniz bittiğinde elde ederiz budur: bir sürü noktalar, her elmas.

![Dağılım](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/scatter-plot.png)

## <a name="draw-the-model-through-the-data-points"></a>Modeli aracılığıyla veri noktalarını çizin
Şimdi noktalar ve squint bakarsanız, koleksiyonu gibi fat, benzer bir satır görünür. Bizim işaretçisi olması ve onun üzerinden düz bir çizgi çizin.

Oluşturduğumuz bir çizgi çizerek bir *model*. Gerçek dünyada almak ve bir alıyormuş çizgi sürümü yaparak olarak düşünün. Şimdi çizgi yanlış - satırı tüm veri noktalarına Git değil. Ancak, kullanışlı bir basitleştirme.

![Doğrusal regresyon satır](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/linear-regression-line.png)

Tüm noktaların tam olarak bir çizgi üzerinden gerçekleştirilmeyen olgu kurulabilir. Veri bilimcileri açıklayan bu - çizgidir - model ve bazı her nokta olduğundan diyerek *gürültü* veya *varyansı* ilişkili. Temel alınan mükemmel bir ilişki yoktur ve ardından gürültü ve belirsizlik ekler Görünüşün ve gerçek dünyadaki yoktur.

Soruyu yanıtlamak deniyoruz çünkü *ne kadar?* bu adlı bir *regresyon*. Ve düz bir çizgi kullandığımızdan olduğu bir *doğrusal regresyon*.

## <a name="use-the-model-to-find-the-answer"></a>Model yanıt bulmak için kullanın
Şimdi bir model sunuyoruz ve bizim soru isteriz: 1,35 ayar elmas maliyeti ne kadar?

Bizim soruyu yanıtlamak için biz 1,35 carats göz Küresi ve bir dikey çizgi çizin. Burada model satır kesme noktası biz dolar ekseni yatay çizgi eyeball. Bunu, sağ taraftaki 10.000 denk gelir. Müzik! Yanıt olmasıdır: 10.000 ABD Doları hakkında 1,35 ayar elmas maliyet.

![Model üzerinde yanıt bulun](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/find-the-answer.png)

## <a name="create-a-confidence-interval"></a>Olasılık aralığı oluşturma
Bu tahmin nasıl kesin mi merak ediyorsunuz doğaldır. 1,35 ayar elmas çok yakın 10.000 $ olup olmayacağını bilmek yararlı veya çok daha yüksek veya düşük olduğu. Bu şekil, şimdi Zarf nokta çoğunu içeren regresyon satırına yakın bir yerde çizin. Bu Zarf adlı bizim *olasılık aralığı*: Çünkü fiyatlar bu Zarf içinde kalan oldukça emin olmamıza son çoğu, sahip. Üst ve alt kısmındaki bu zarfın 1,35 simgeyi seçtiğinizde satırın nereye aştığında size iki yatay olarak daha fazla satır çizebilirsiniz.

![Olasılık aralığı](./media/data-science-for-beginners-predict-an-answer-with-a-simple-model/confidence-interval.png)

Artık bir şey hakkında bizim olasılık aralığı dediğimiz:  Biz güvenle 1,35 ayar elmas fiyatı hakkında $ 10. 000 - olan ancak 8000 kadar düşük olabilir ve 12.000 kadar yüksek olabilir söyleyebilirsiniz.

## <a name="were-done-with-no-math-or-computers"></a>Hiçbir matematik veya bilgisayarlar ile tamamlandı
Hangi veri bilimcileri yapmak için ödeme yaptığımız ve onu yalnızca çizerek yaptık:

* Biz size verilerle yanıt soru sorular
* Oluşturduğumuz bir *modeli* kullanarak *doğrusal regresyon*
* Yaptık bir *tahmin*ile tam bir *olasılık aralığı*

Ve matematik veya bilgisayarları yapmak için kullanırız kaydetmedi.

Size daha fazla bilgi olsaydı, şimdi nasıl görünüyor...

* Elmasın Kes
* renk değişimleri (ne kadar yakın elmas beyaz olarak değer)
* eklenen elmas içindeki sayısı

...then biz olan daha fazla sütun. Bu durumda, matematik yararlı olur. İkiden fazla sütununuz varsa, noktalar kağıda çizmek zordur. Matematik çok düzgün bir şekilde bu satırı veya bu uçak verilerinize uygun olanak sağlar.

Ayrıca, yalnızca birkaç Karo ise, yerine iki bin veya milyon iki vardı sonra çalışan bir bilgisayarla çok daha hızlı yapabilirsiniz.

Bugün, doğrusal regresyon yapmak hakkında konuştuk ve verileri kullanarak tahmin yaptık.

"Veri bilimi için yeni başlayanlar" Microsoft Azure Machine Learning Studio'dan diğer videoları kullanıma emin olun.

## <a name="next-steps"></a>Sonraki adımlar
* [Bir Machine Learning Studio'da ilk veri bilimi deneyinde deneyin](create-experiment.md)
* [Microsoft Azure'da Machine learning'e giriş yapın](/azure/machine-learning/preview/overview-what-is-azure-ml)
