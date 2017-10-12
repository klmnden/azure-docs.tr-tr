---
title: "Örnek makine öğrenimi denemelerini kopyalama - Azure | Microsoft Docs"
description: "Cortana Intelligence Gallery ve Microsoft Azure Machine Learning ile yeni denemeler oluşturmak için örnek makine öğrenimi denemelerini nasıl kullanacağınızı öğrenin."
keywords: "machine learning örnekleri, örnek deneme, machine learning örneği"
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
ms.date: 07/28/2017
ms.author: cgronlun
ms.openlocfilehash: f798ac1b46d702dbb96a2384d96f2d14eb3cac0e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="copy-example-experiments-to-create-new-machine-learning-experiments"></a>Örnek denemeleri kopyalayarak yeni makine öğrenimi denemeleri oluşturma
Sıfırdan makine öğrenimi denemeleri oluşturmayı öğrenmek yerine [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/)’deki örnek denemelerle nasıl çalışmaya başlayacağınızı öğrenin. Örnekleri kullanarak kendi makine öğrenimi çözümünüzü oluşturabilirsiniz.

Galeride Microsoft Azure Machine Learning ekibi tarafından sağlanan örnek denemelerin yanı sıra Machine Learning topluluğu tarafından paylaşılan örnek denemeler yer almaktadır. Ayrıca, denemeler hakkında soru sorabilir veya yorum yazabilirsiniz.

Galeriyi nasıl kullanacağınızı görmek için [Yeni Başlayanlar için Veri Bilimi](data-science-for-beginners-the-5-questions-data-science-answers.md) serisinden [Veri bilimi yapmak için başka insanların çalışmalarını kopyalama](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) başlıklı 3 dakikalık videoyu izleyin.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="find-an-experiment-to-copy-in-cortana-intelligence-gallery"></a>Cortana Intelligence Gallery’de kopyalanacak bir deneme bulma
Hangi denemelerin kullanılabildiğini görmek için [Galeri](https://gallery.cortanaintelligence.com/)'ye gidin ve sayfanın en üst kısmında **Denemeler**'e tıklayın.

### <a name="find-the-newest-or-most-popular-experiments"></a>En yeni ve en popüler denemeleri bulma
Bu sayfada **Son eklenen** denemeleri görüntüleyebilir, aşağı kaydırarak **Popüler olanlar**'a bakabilir veya en son **Popüler Microsoft denemeleri**’ni görebilirsiniz.

### <a name="look-for-an-experiment-that-meets-specific-requirements"></a>Belirli gereksinimleri karşılayan bir deneme arama
Tüm denemelere gözatmak için:

1. Sayfanın üstündeki **Tümüne gözat**'a tıklayın.
2. Galeri'deki tüm denemeleri görmek için sol tarafta bulunan **Kategoriler** bölümündeki **Daraltma ölçütü** seçeneğinin altından **Denemeler**'i seçin.
3. Gereksinimlerinize uyan denemeleri birkaç farklı yolla bulabilirsiniz:
   * **Soldaki filtreleri seçin.** Örneğin, PCA tabanlı bir anomali algılama algoritması kullanan denemelere göz atmak için: **Kategoriler**'in altında **Denemeler** seçiliyken **Tümünü göster**'e tıklayın. Ardından, **Kullanılan Algoritmalar** altında **PCA Tabanlı Anomali Algılama**’yı seçin. <br></br>
     ![Filtreleri seçin](./media/sample-experiments/refine-the-view.png)
   * **Arama kutusunu kullanın.** Örneğin, Microsoft tarafından paylaşılan iki sınıflı destekli vektör makinesi algoritması kullanan rakam tanıma ile ilgili denemeleri bulmak için arama kutusuna "rakam tanıma" yazın. Daha sonra, **Deneme**, **Yalnızca Microsoft içeriği** ve **İki Sınıflı Destekli Vektör Makinesi**'ni seçin:<br></br>
     ![Arama kutusunu kullanma](./media/sample-experiments/search-for-experiments.png)
4. Hakkında daha fazla bilgi almak için bir denemeye tıklayın.
5. Denemeyi çalıştırmak ve/veya değiştirmek için, deneme sayfasında **Studio'da aç** seçeneğine tıklayın. <br></br>

    ![Örnek deneme](./media/sample-experiments/example-experiment.png)

    > [!NOTE]
    > Machine Learning Studio’da bir denemeyi ilk kez açtığınızda ücretsiz olarak deneyebilir veya bir Azure aboneliği satın alabilirsiniz. [Machine Learning Studio ücretsiz denemesi ile ücretli hizmetin karşılaştırması hakkında bilgi edinin](https://azure.microsoft.com/pricing/details/machine-learning/)
    >
    >

## <a name="create-a-new-experiment-using-an-example-as-a-template"></a>Şablon olarak bir örnek kullanarak yeni deneme oluşturma
Ayrıca, bir Galeri örneğini bir şablon olarak kullanıp Machine Learning Studio'da yeni bir deneme oluşturabilirsiniz.

1. Microsoft hesabı kimlik bilgilerinizle [Studio](https://studio.azureml.net)'da oturum açın ve ardından bir deneme oluşturmak için **Yeni** seçeneğine tıklayın.
2. Örnek içeriğine gözatın ve birine tıklayın.

Örnek deneme bir şablon olarak kullanılarak Machine Learning Studio çalışma alanınızda yeni bir deneme oluşturulur.

## <a name="next-steps"></a>Sonraki adımlar
* [Çeşitli kaynaklardan veri alma](import-data.md)
* [Machine Learning için R programlama diline yönelik hızlı başlangıç öğreticisi](r-quickstart.md)
* [Machine Learning web hizmeti dağıtma](publish-a-machine-learning-web-service.md)
