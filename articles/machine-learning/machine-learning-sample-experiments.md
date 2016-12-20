---
title: "Örnek makine öğrenimi denemelerini kopyalama | Microsoft Belgeleri"
description: "Cortana Intelligence Gallery ve Microsoft Azure Machine Learning ile yeni denemeler oluşturmak için örnek makine öğrenimi denemelerini nasıl kullanacağınızı öğrenin."
services: machine-learning
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: 81e6c1d8-682c-4db3-bfd5-d7bfb1150ff3
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/11/2016
ms.author: cgronlun;olgali
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 9c710e6f959afd8a4f4b965931ed4868d94c6d25


---
# <a name="copy-sample-experiments-to-create-new-machine-learning-experiments"></a>Örnek denemeleri kopyalayarak yeni makine öğrenimi denemeleri oluşturma
Sıfırdan makine öğrenimi denemeleri oluşturmayı öğrenmek yerine [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/)’deki örnek denemelerle nasıl çalışmaya başlayacağınızı öğrenin. Örnekleri kullanarak kendi makine öğrenimi çözümünüzü oluşturabilirsiniz.

Galeride Microsoft Azure Machine Learning ekibi tarafından sağlanan örnek denemelerin yanı sıra Machine Learning topluluğu tarafından paylaşılan örnek denemeler yer almaktadır. Ayrıca, denemeler hakkında soru sorabilir veya yorum yazabilirsiniz.

Galeriyi nasıl kullanacağınızı görmek için [Yeni Başlayanlar için Veri Bilimi](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) serisinden [Veri bilimi yapmak için başka insanların çalışmalarını kopyalama](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) başlıklı 3 dakikalık videoyu izleyin.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="find-an-experiment-to-copy-in-cortana-intelligence-gallery"></a>Cortana Intelligence Gallery’de kopyalanacak bir deneme bulma
Hangi denemelerin kullanılabildiğini görmek için [Galeri](https://gallery.cortanaintelligence.com/)'ye gidin ve sayfanın en üst kısmında **Denemeler**'e tıklayın.

### <a name="find-the-newest-or-most-popular-experiments"></a>En yeni ve en popüler denemeleri bulma
Bu sayfada **Son eklenen** denemeleri görüntüleyebilir, aşağı kaydırarak **Popüler olanlar**'a bakabilir veya en son **Popüler Microsoft denemeleri**’ni görebilirsiniz.

### <a name="look-for-an-experiment-that-meets-specific-requirements"></a>Belirli gereksinimleri karşılayan bir deneme arama
Tüm denemelere gözatmak için:

1. Sayfanın üstündeki **Tümüne gözat**'a tıklayın.
2. Galeri'deki tüm denemeleri görmek için **Daraltma ölçütü** seçeneğinin altından **Denemeler**'i seçin.
3. Gereksinimlerinize uyan denemeleri birkaç farklı yolla bulabilirsiniz:
   * **Soldaki filtreleri seçin.** Örneğin, PCA tabanlı bir anomali algılama algoritması kullanan denemelere göz atmak için, **Kategoriler**'in altında **Denemler**'i ve **Kullanılan Algoritmalar**'ın altında **PCA Tabanlı Anomali Algılama**'yı seçin. (Bu algoritmayı görmüyorsanız listenin en altında **Tümünü göster**'e tıklayın.)<br></br>
     ![Filtreleri seçin](./media/machine-learning-sample-experiments/refine-the-view.png)
   * **Arama kutusunu kullanın.** Örneğin, Microsoft tarafından paylaşılan iki sınıflı destekli vektör makinesi algoritması kullanan rakam tanıma ile ilgili denemeleri bulmak için arama kutusuna "rakam tanıma" yazın. Daha sonra, **Deneme**, **Yalnızca Microsoft içeriği** ve **İki Sınıflı Destekli Vektör Makinesi**'ni seçin: ![Arama kutusunu kullanın](./media/machine-learning-sample-experiments/search-for-experiments.png)
4. Hakkında daha fazla bilgi almak için bir denemeye tıklayın.
5. Denemeyi çalıştırmak ve/veya değiştirmek için, deneme sayfasında **Studio'da aç** seçeneğine tıklayın.

   > [!NOTE]
   > Bir denemeyi Machine Learning Studio'da açmak için Microsoft hesabı kimlik bilgilerinizle oturum açmanız gerekir. Machine Learning çalışma alanınız henüz yoksa ücretsiz bir deneme sürümü çalışma alanı oluşturulur. [Machine Learning ücretsiz deneme sürümüne nelerin dahil olduğunu öğrenin](https://azure.microsoft.com/pricing/details/machine-learning/)
   >
   >

    ![Örnek deneme](./media/machine-learning-sample-experiments/example-experiment.png)

## <a name="use-a-template-in-machine-learning-studio"></a>Machine Learning Studio'da bir şablon kullanma
Ayrıca, bir Galeri örneğini bir şablon olarak kullanıp Machine Learning Studio'da yeni bir deneme oluşturabilirsiniz.

1. Microsoft hesabı kimlik bilgilerinizle [Studio](https://studio.azureml.net)'da oturum açın ve ardından yeni bir deneme oluşturmak için **Yeni** seçeneğine tıklayın.
2. Örnek içeriğine gözatın ve birine tıklayın.

Örnek deneme bir şablon olarak kullanılarak çalışma alanınızda yeni bir deneme oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar
* [Verilerinizi hazırlama](machine-learning-data-science-import-data.md)
* [Denemenizde R kullanmayı deneme](machine-learning-r-quickstart.md)
* [Örnek R denemelerini gözden geçirme](machine-learning-r-csharp-web-service-examples.md)
* [Web hizmeti API’si oluşturma](machine-learning-publish-a-machine-learning-web-service.md)



<!--HONumber=Dec16_HO1-->


