---
title: "Başkalarının veri bilimi örnekler – Azure Machine Learning kopyalama | Microsoft Docs"
description: "Veri bilimi, ticari sır: çalışmanızı bunu başkalarına alın. Machine learning örnekler Azure AI Galerisi'nden alın."
keywords: "Veri bilimi örnekler, algoritma örnek kümeleme algoritması, kümeleme machine learning örnek"
services: machine-learning
documentationcenter: na
author: cjgronlund
manager: jhubbard
editor: cjgronlund
ms.assetid: ec2be823-c325-4ad8-b8b2-3e664f1a44b4
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2018
ms.author: cgronlun
ms.openlocfilehash: f63911d8b48393ec8b0f041b1340bec40c768f38
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="copy-other-peoples-work-to-do-data-science"></a>Veri bilimi için başkalarının çalışmalarını kopyalama
## <a name="video-5-data-science-for-beginners-series"></a>Video 5: Yeni başlayanlar seri için veri bilimi
Veri bilimi, ticari sır birini iş yaptığınız için diğer kişileri almaktır. Kendi makine öğrenimi denemesinin için kullanılacak Azure AI Galerisi'ndeki kümeleme algoritması örnek bulun.

> [!IMPORTANT]
> Cortana Intelligence Galerisi adlandırıldı **Azure AI galeri**. Sonuç olarak, metin ve görüntüler bu dökümü biraz eski adı kullanan video farklı.
>

Serinin en dışında almak için tümünü izleyin. [Videolar listesine Git](#other-videos-in-this-series)
<br>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/data-science-for-beginners-series-copy-other-peoples-work-to-do-data-science/player]
>
>

## <a name="other-videos-in-this-series"></a>Bu serideki diğer videolar
*Yeni başlayanlar için veri bilimi* veri bilimi beş kısa videolar içindeki bir giriş değil.

* Video 1: [5 veri bilimi yanıtlar sorular](data-science-for-beginners-the-5-questions-data-science-answers.md) *(5 dakika 14 saniye)*
* Video 2: [verileriniz için veri bilimi hazır?](data-science-for-beginners-is-your-data-ready-for-data-science.md) *(4 min 56 sn)*
* Video 3: [verilerle yanıt soru](data-science-for-beginners-ask-a-question-you-can-answer-with-data.md) *(4 min 17 sn)*
* Video 4: [basit bir modelle bir yanıt tahmin](data-science-for-beginners-predict-an-answer-with-a-simple-model.md) *(7 min 42 sn)*
* Video 5: veri bilimi yapmak için diğer kişilerin çalışma kopyalayın

## <a name="transcript-copy-other-peoples-work-to-do-data-science"></a>Dökümü: veri bilimi yapmak için diğer kişilerin iş kopyalama
Serideki beşinci video "Yeni başlayanlar için veri bilimi." Hoş Geldiniz

Bu tek bir yerde, gelen bir başlangıç noktası olarak kendi iş için kullanırım Bul örnekler için öğreneceksiniz. Bu serideki önceki videoları ilk izleyin, en iyi bu videoyu alabilirsiniz.

Veri bilimi, ticari sır birini iş yaptığınız için diğer kişileri almaktır.

## <a name="find-examples-in-the-azure-ai-intelligence-gallery"></a>Azure AI Intelligence Galerisi'nde örnekleri Bul

Microsoft adlı bulut tabanlı bir hizmete sahip [Azure Machine Learning Studio](https://azure.microsoft.com/services/machine-learning-studio/) ücretsiz deneyin Hoş Geldiniz. Bu, bir çalışma alanıyla farklı machine learning algoritmaları ile deneyebilirsiniz ve yerdir, çalışılan çözümünüzün var olduğunda sağlar, web hizmeti olarak başlatın.

Bu hizmetin parçası olan bir şey adlı  **[Azure AI galeri](https://gallery.cortanaintelligence.com/)**. Azure Machine Learning denemeleri veya kişiler yerleşik ve diğerleri için kullanılacak katkıda modelleri koleksiyonu gibi kaynakları içerir. Bu denemeler düşünce ve diğer kendi çözümlerini başlamanıza yardımcı olmak için sabit iş yararlanmak için harika bir yoludur. Herkes üzerinden Gözat Hoş Geldiniz.

![Azure AI Galerisi](./media/data-science-for-beginners-copy-other-peoples-work-to-do-data-science/azure-ai-gallery.png)

Tıklatırsanız **denemeler** en üstte en son ve popüler denemeler galerideki sayısı görürsünüz. Tıklatarak denemeler kullanılmadıkları arayabilirsiniz **tümüne Gözat** ekranın üstünde ve orada girdiğiniz arama terimleri ve arama filtrelerini seçin.

## <a name="find-and-use-a-clustering-algorithm-example"></a>Bulma ve kümeleme algoritması örneği kullanın
Bu nedenle, örneğin, arama için kümeleme nasıl çalıştığını, bir örnek görmek istediğiniz diyelim ki **"Tarama Kümeleme"** denemeler.

![Denemeler kümeleme için arama](./media/data-science-for-beginners-copy-other-peoples-work-to-do-data-science/search-for-clustering-experiments.png)

Birisi Galeriye katkıda ilginç bir İşte.

![Deneme kümeleme](./media/data-science-for-beginners-copy-other-peoples-work-to-do-data-science/clustering-experiment.png)

Üzerinde deneme ve bazı sonuçları yanı sıra bu katkıda bulunan, yaptığınız iş açıklayan bir web sayfası Al'ı tıklatın.

![Deneme açıklama sayfa kümeleme](./media/data-science-for-beginners-copy-other-peoples-work-to-do-data-science/clustering-experiment-description-page.png)

Bildirim bağlantısını **Studio'da Aç**.

![Studio düğmesi Aç](./media/data-science-for-beginners-copy-other-peoples-work-to-do-data-science/open-in-studio.png)

Üzerinde tıklayabilir ve bana sağdan sürdüğünü **Azure Machine Learning Studio**. Denemeyi bir kopyasını oluşturur ve çalışma Alanım kendi yerleştirir. Bu, Katkıda Bulunanlar veri kümesi, yaptıkları işlemin, tüm işlemler tüm kullandıkları algoritmaları ve sonuçları kaydedilme içerir.

![Machine Learning Studio - kümeleme algoritması örnek bir galeri deneme açın](./media/data-science-for-beginners-copy-other-peoples-work-to-do-data-science/cluster-experiment-open-in-studio.png)

Ve bir başlangıç noktası şimdi sahibim. I için kendi verilerini takas ve kendi modelini uyguladıkça yapın. Bu bana bir çalışan başlangıç verir ve bana bilen gerçekten ne yaptıklarını kişilerin çalışmalarını yapı olanak tanır.

## <a name="find-experiments-that-demonstrate-machine-learning-techniques"></a>Machine learning teknikleri göstermek denemeler Bul
Diğer denemeler vardır [Azure AI galeri](https://gallery.cortanaintelligence.com) , katkıda bulunan özellikle veri bilimi yeni kişiler için nasıl yapılır örnekler sağlamak için. Örneğin, bir deneme eksik değerleri nasıl ele alınacağını gösteren galerisinde yoktur ([eksik değerleri işlemek için yöntemleri](https://gallery.cortanaintelligence.com/Experiment/Methods-for-handling-missing-values-1)). Boş değerleri değiştirerek 15 farklı yöntemler size yol gösterir ve her yöntem ve ne zaman kullanılmalı avantajları hakkında ettiği.

![Machine Learning Studio'da - eksik değerleri için yöntemleri galeri denemeler açın](./media/data-science-for-beginners-copy-other-peoples-work-to-do-data-science/experiment-methods-for-handling-missing-values.png)

[Azure AI galeri](https://gallery.cortanaintelligence.com) kendi çözümleri için bir başlangıç noktası olarak kullanabileceğiniz çalışma denemeleri bulmak için bir yerdir.

"Veri bilimi için yeni başlayanlar" Microsoft Azure Machine learning'in diğer videoları kullanıma emin olun.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Machine Learning ile ilk veri bilimi denemenizi deneyin](create-experiment.md)
* [Microsoft Azure Machine Learning giriş Al](what-is-machine-learning.md)
