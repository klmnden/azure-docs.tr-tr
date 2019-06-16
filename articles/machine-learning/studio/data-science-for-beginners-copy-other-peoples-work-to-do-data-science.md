---
title: Başkalarının veri bilimi örneği kopyalayın
titleSuffix: Azure Machine Learning Studio
description: 'Veri bilimi, ticari sır: Diğer iş sizin için gerçekleştirmesini istemeniz alın. Machine learning örnekleri, Azure yapay ZEKA Galeriden alın.'
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: sdgilley
ms.author: sgilley
ms.custom: seodec18
ms.date: 03/22/2019
ms.openlocfilehash: 8bacc3940cebaf9c62179cee0788e5903e56a310
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60751863"
---
# <a name="copy-other-peoples-work-to-do-data-science"></a>Veri bilimi için başkalarının çalışmalarını kopyalama
## <a name="video-5-data-science-for-beginners-series"></a>Video 5: Seri yeni başlayanlar için veri bilimi
Bir veri bilimi, ticari sırlar, başkalarının çalışmanıza sizin için gerçekleştirmesini istemeniz almaktır. Kendi makine öğrenimi denemesi için kullanılacak Azure AI Gallery'de bir kümeleme algoritması örnek bulun.

> [!IMPORTANT]
> **Cortana Intelligence Galerisi** yeniden adlandırıldı **Azure AI Gallery**. Sonuç olarak, metin ve görüntüleri Bu dökümdeki biraz daha eski adı kullanan videodan değişir.
>

En yetersiz serisi almak için tüm bunları izleyin. [Videoları listesine Git](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/data-science-for-beginners-series-copy-other-peoples-work-to-do-data-science/player]
>
>

## <a name="other-videos-in-this-series"></a>Bu serideki diğer videolar
*Yeni başlayanlar için veri bilimi* beş kısa videoyu veri bilimine hızlı bir giriş niteliğindedir.

* Video 1: [5 veri biliminin yanıtladığı sorular](data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 en az 14 sn)*
* Video 2: [Verileriniz veri bilimi için hazır mı?](data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 dk 56 sn)*
* Video 3: [Yanıt verileri ile soru](data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 dk 17 sn)*
* Video 4: [Basit bir model ile yanıtı tahmin etme](data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 dk 42 sn)*
* Video 5: Veri bilimi için başkalarının çalışmalarını kopyalama

## <a name="transcript-copy-other-peoples-work-to-do-data-science"></a>Transkript: Veri bilimi için başkalarının çalışmalarını kopyalama
Serinin beşinci videoda "Yeni başlayanlar için veri bilimi." Hoş Geldiniz

Bu tek bir yerde, gelen bir başlangıç noktası olarak kendi çalışma kullanırım Bul örnekleri keşfedeceksiniz. Öncelikle bu serideki önceki videolar izleyin, en iyi bu videoyu alabilirsiniz.

Bir veri bilimi, ticari sırlar, başkalarının çalışmanıza sizin için gerçekleştirmesini istemeniz almaktır.

## <a name="find-examples-in-the-azure-ai-gallery"></a>Azure AI Gallery'de örnekler bulabilirsiniz

Sahip adı verilen bir bulut tabanlı hizmeti Microsoft [Azure Machine Learning Studio](https://azure.microsoft.com/services/machine-learning-studio/) ücretsiz denemeye Hoş Geldiniz. Size bir çalışma alanıyla farklı makine öğrenimi algoritmalarıyla denemeler yapabilir ve, çözümünüzü çalışılan süreyi bulduğunuzda, sağlar, web hizmeti olarak başlatın.

Bu hizmetin bir parçası olan bir şey adlı  **[Azure AI Gallery](https://gallery.azure.ai/)** . Bu, Azure Machine Learning Studio denemeleri veya kişilerin oluşturduğu ve başkalarının kullanması için katkıda bulunan modelleri koleksiyonu gibi kaynakları içerir. Bu denemeler, kendi çözümlerinizi başlamanıza yardımcı olmak için başkalarının çalışmalarını sabit ve düşünce yararlanmak için harika bir yoludur. Herkesin göz atmak Hoş Geldiniz.

![Azure Yapay Zeka Galerisi](./media/data-science-for-beginners-copy-other-peoples-work-to-do-data-science/azure-ai-gallery.png)

Tıklarsanız **denemeleri** galeri'deki en son ve popüler denemeleri sayısı üst kısmında görürsünüz. Tıklayarak denemeleri rest üzerinden arayabilirsiniz **tümüne Gözat** ekranın üst kısmındaki ve orada girdiğiniz arama terimlerini ve arama filtrelerini seçin.

## <a name="find-and-use-a-clustering-algorithm-example"></a>Bulma ve bir kümeleme algoritması örnek kullanma
Bu nedenle, örneğin, araması için kümeleme nasıl çalıştığına ilişkin bir örnek görmek istediğiniz varsayalım **"Süpürme Kümeleme"** denemeleri.

![Denemeleri kümeleme için arama](./media/data-science-for-beginners-copy-other-peoples-work-to-do-data-science/search-for-clustering-experiments.png)

Birisi Galeri'ye katkıda ilgi çekici bir İşte.

![Deneme kümeleme](./media/data-science-for-beginners-copy-other-peoples-work-to-do-data-science/clustering-experiment.png)

Deneme ve bazı sonuçları ile birlikte bu katkıda bulunan, yaptığınız çalışmayı açıklayan bir web sayfası alma tıklayın.

![Deneme açıklama sayfasında kümeleme](./media/data-science-for-beginners-copy-other-peoples-work-to-do-data-science/clustering-experiment-description-page.png)

Bildirim bağlantısına **Studio'da Aç**.

![Studio düğmesi Aç](./media/data-science-for-beginners-copy-other-peoples-work-to-do-data-science/open-in-studio.png)

Benim için doğru sürer ve üzerinde tıklayabilirsiniz **Azure Machine Learning Studio**. Bu denemenin bir kopyasını oluşturur ve bunu kendi çalışma alanına yerleştirir. Bu, katkıda bulunan kişinin veri kümesi, yaptıkları işlemin, tüm işleme tüm kullandıkları algoritmalar ve sonuçları kaydedilme içerir.

![Machine Learning Studio'da - kümeleme algoritması örnek bir galeri deneme açın](./media/data-science-for-beginners-copy-other-peoples-work-to-do-data-science/cluster-experiment-open-in-studio.png)

Ve bir başlangıç noktası artık sahibim. Ben için kendi verilerini takas ve kendi modelini ince ayar yapma yapın. Bana bir çalıştırma başlangıç sağlar ve bana gerçekten ne yaptıklarını bilmeniz kişilerin çalışmalarını yapı olanak tanır.

## <a name="find-experiments-that-demonstrate-machine-learning-techniques"></a>Makine öğrenimi tekniklerinden gösteren denemeleri bulma
İçindeki diğer denemeleri vardır [Azure AI Gallery](https://gallery.azure.ai) , katkıda özellikle veri bilimi için yeni olan kişiler için nasıl yapılır örnekleri sağlamak için. Örneğin, bir deneme eksik değerleri nasıl ele alınacağını gösteren galeri yoktur ([eksik değerleri işlemek için yöntemler](https://gallery.azure.ai/Experiment/Methods-for-handling-missing-values-1)). Boş değerleri değiştirerek 15 farklı yollar için size ve her yöntem ve ne zaman kullanacağınız avantajları hakkında konuşuyor.

![Machine Learning Studio'da - yöntemleri eksik değerler için Galeri denemeleri açın](./media/data-science-for-beginners-copy-other-peoples-work-to-do-data-science/experiment-methods-for-handling-missing-values.png)

[Azure AI Gallery](https://gallery.azure.ai) kendi çözümlerinizi için başlangıç noktası olarak kullanabileceğiniz çalışma denemeleri bulmak için bir yerdir.

"Veri bilimi için yeni başlayanlar" Microsoft Azure Machine Learning Studio'dan diğer videoları kullanıma emin olun.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Machine Learning Studio'da ilk veri bilimi denemenizi deneyin](create-experiment.md)
* [Microsoft Azure'da Machine learning'e giriş yapın](/azure/machine-learning/preview/overview-what-is-azure-ml)
