---
title: Metin analizi (AMLPTA) ve Team veri bilimi işlem (TDSP) Azure Machine Learning (AML) paketi düşünceleri sınıflandırmasıyla twitter | Microsoft Docs
description: TDSP (Takım veri bilimi işlemi) ve AMLPTA kullanımını düşünceleri sınıflandırma açıklar
services: machine-learning, team-data-science-process
documentationcenter: ''
author: deguhath
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: deguhath
ms.openlocfilehash: 559af47bcf41cd6af59f8ba1b27ff8e64e849925
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296172"
---
# <a name="twitter-sentiment-classification-with-azure-machine-learning-aml-package-for-text-analytics-amlpta-and-team-data-science-process-tdsp"></a>Metin analizi (AMLPTA) ve Team veri bilimi işlem (TDSP) Azure Machine Learning (AML) paketi ile twitter düşünceleri sınıflandırma

## <a name="introduction"></a>Giriş
Standartlaştırma yapısı ve veri bilimi belgelerine projeleri, yani kurulmuş bir bağlantılı [veri bilimi yaşam döngüsü](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md), veri bilimi ekipleri etkili işbirliği kolaylaştırmanın için anahtarıdır.

Daha önce yayımladık bir [TDSP proje yapısını ve şablonları için GitHub depo](https://github.com/Azure/Azure-TDSP-ProjectTemplate). Biz şimdi ile örneği Azure Machine Learning projeleri oluşturulmasını etkinleştirdiyseniz [TDSP yapısı ve belgeleri şablonları Azure Machine Learning için](https://github.com/amlsamples/tdsp). Azure Machine Learning TDSP yapısı ve şablonları kullanma konusunda yönergeler sağlanır [burada](https://docs.microsoft.com/en-us/azure/machine-learning/preview/how-to-use-tdsp-in-azure-ml). 

Bu örnekte, biz metin analizi ve geliştirmek ve Twitter düşünceleri sınıflandırması için Tahmine dayalı modelleri dağıtmak için TDSP Azure Machine Learning paketi kullanım göstermek için adımıdır.


## <a name="use-case"></a>Kullanım örneği
### <a name="twitter-sentiment-polarity-sample"></a>Twitter düşünceleri polarite örnek
Bu makalede bir örnek örneği ve Machine Learning proje yürütmek nasıl göstermek için kullanır. Örnek, Azure Machine Learning çalışma TDSP yapısı ve şablonları kullanır. Bu kılavuzda tam örnek sağlanır. Modelleme görev tweet'leri metinden kullanarak düşünceleri polarite (olumlu veya olumsuz) tahmin eder. Bu makalede kılavuzda açıklanan veri modellemesi görevleri özetlenmektedir. İzlenecek yol aşağıdaki görevleri içerir:

1. Veri keşfi, eğitim ve machine learning modeli dağıtımını, kullanım örneği genel bakış içinde açıklanan tahmin sorunu çözün. Twitter düşünceleri verileri bu görevler için kullanılır.
2. Bu proje için Azure Machine Learning TDSP şablonu kullanarak proje yürütme. Proje yürütme ve raporlama için TDSP yaşam döngüsü kullanılır.
3. Azure Machine Learning Azure kapsayıcı Hizmeti'nde çözümden doğrudan operationalization.

Projeyi Azure Machine Learning için metin Analytics paket kullanımını vurgular.


## <a name="link-to-github-repository"></a>GitHub deponuza bağlayın
GitHub depo bağlantıdır [burada](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction). 

### <a name="purpose"></a>Amaç
Bu örnek birincil amacı, örneği ve machine learning ekibi veri bilimi işlem (TDSP) yapısı ve şablonları Azure Machine Learning çalışma karşılaştırma içinde kullanarak proje yürütmek nasıl göstermektir. Bu amaç için kullandığımız [Twitter düşünceleri veri](http://cs.stanford.edu/people/alecmgo/trainingandtestdata.zip). Modelleme düşünceleri polarite (olumlu veya olumsuz) tahmin etmek için bir görevdir tweet'leri metinden kullanma.

### <a name="scope"></a>Kapsam
- Veri keşfi, eğitim ve kullanım durumu genel bakış içinde açıklanan tahmin soruna yönelik bir makine öğrenimi modeline dağıtımı.
- Bu proje için Azure Machine Learning ekibi veri bilimi işlem (TDSP) şablonu kullanarak Azure Machine Learning projesinde yürütme. Proje yürütme ve raporlama için TDSP yaşam döngüsü kullanmayı yapacağız.
- Azure Machine Learning Azure kapsayıcı Services'de çözümden doğrudan operationalization.

Proje çeşitli özelliklerini Azure Machine Learning, bu tür TDSP yapısı örneklemesi ve kullanım, Azure Machine Learning çalışma karşılaştırma kodda ve Docker ve Kubernetes kullanarak Azure kapsayıcı Services içinde kolay operationalization yürütülmesini vurgular.

## <a name="team-data-science-process-tds"></a>Takım veri bilimi işlemi (TDS)
Bu örnek yürütmek için TDSP Proje yapısı ve belgeleri şablonları kullanın. Kendisini izleyen [TDSP yaşam döngüsü](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/lifecycle). Proje sağlanan yönergeleri temel alınarak oluşturulur [burada](https://github.com/amlsamples/tdsp/blob/master/docs/how-to-use-tdsp-in-azure-ml.md).


<img src="./media/predict-twitter-sentiment-amltextpackage/tdsp-lifecycle2.png" alt="tdsp-lifecycle" width="800" height="600">


## <a name="use-case-overview"></a>Kullanım örneği'ne genel bakış
Her twitter's düşünceleri ikili polarite twitter metinden ayıklanan word eklerinin özelliklerini kullanarak tahmin etmek için bir görevdir. Ayrıntılı bir açıklaması için lütfen bu başvurun [depo](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction).

### <a name="data-acquisition-and-understandinghttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode01dataacquisitionandunderstanding"></a>[Veri alımı ve anlama](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/01_data_acquisition_and_understanding)
Bu örnek ilk adımda sentiment140 dataset indirin ve tren bölmek ve veri kümeleri sınamaktır. Sentiment140 veri kümesi içerdiğinden tweet gerçek içeriği (ile ifadeler kaldırıldı) yanı sıra her bir tweet polarite (negatif = 0, pozitif = 4) ile de nötr tweet'leri kaldırıldı. Sonuçta elde edilen eğitim verileri 1.3 milyon satır ve verileri test etme 320 k satırları sahiptir.

### <a name="modelinghttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode02modeling"></a>[Modelleme](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/02_modeling)
Bu örnek parçası daha fazla üç subparts ayrılır: 
- **Özellik Mühendisliği** algoritması, yani Word2Vec katıştırma farklı sözcük kullanarak özellik nesil karşılık gelir. 
- **Model oluşturma** eğitim anlaşmalar başka modellerinin ister _Lojistik regresyon_ ve _gradyan artırmanın_ giriş metni düşünceleri tahmin etmek için. 
- **Model değerlendirme** eğitilen modeli sınama verilerinde geçerlidir.

#### <a name="feature-engineering"></a>Özellik mühendisliği
Kullanırız <b>Word2Vec</b> word eklerinin oluşturmak için. İlk Word2Vec algoritma Skipgram modunda yazıda açıklandığı gibi kullanıyoruz [Mikolov, Tomas, et al. Sözcükler ve tümcecikleri ve bunların compositionality dağıtılmış gösterimlerini. Sinir bilgi systems işleme gelişmeler. 2013.](https://arxiv.org/abs/1310.4546) Word eklerinin oluşturmak için.

Atla-gram, giriş olarak bir etkin vektör olarak kodlanmış ve sözcükleri tahmin etmek için kullanarak hedef word alma basit bir sinir ağdır. V sözlüğünü boyutudur sonra çıktı katman boyutu olacak __C * V__ burada C, bağlam penceresinin boyutu. Atla-gram tabanlı mimari aşağıdaki şekilde gösterilir.
 
<table class="image" align="center">
<caption align="bottom">Atla-gram modeli</caption>
<tr><td><img src="https://s3-ap-south-1.amazonaws.com/av-blog-media/wp-content/uploads/2017/06/05000515/Capture2-276x300.png" alt="Skip-gram model"/></td></tr>
</table>

Bu örnek kapsamı dışındadır Word2Vec algoritması ve atlama-gram modeli ayrıntılarını olduğunu ve ilgi okuyucular daha fazla ayrıntı için aşağıdaki bağlantıları gitmesi istenir. Başvurulan kod 02_A_Word2Vec.py [tensorflow örnekleri](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/word2vec/word2vec_basic.py)

* [Sözcükler vektör gösterimi](https://www.tensorflow.org/tutorials/word2vec)
* [Word2vec tam olarak nasıl çalışır?](http://www.1-4-5.net/~dmm/ml/how_does_word2vec_work.pdf)
* [Gürültü Contrastive tahmin ve negatif örnekleme ile ilgili notlar](http://demo.clab.cs.cmu.edu/cdyer/nce_notes.pdf)

Eğitim işlemini gerçekleştirdikten sonra TSV biçiminde iki katıştırma dosyaları modelleme aşaması için oluşturulur.

#### <a name="model-training"></a>Model eğitimi
Word vektörlerinin SSWE veya Word2vec algoritmasını birini kullanarak oluşturulmuş sonra sıradaki adım gerçek düşünceleri polarite tahmin etmek için sınıflandırma modelleri eğitme olacak. Şu iki tür özelliklerin Uygula: Word2Vec ve iki modeli içine SSWE: Lojistik regresyon ve Convolutional sinir ağları (CNN). 

#### <a name="model-evaluation"></a>Model değerlendirme
Yük ve birden çok eğitilmiş modeller sınama veri kümesi üzerinde değerlendirmek nasıl kod sunuyoruz.


### <a name="deploymenthttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode03deployment"></a>[Dağıtım](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/03_deployment)
Bir web hizmeti bir kümede Azure kapsayıcı hizmeti (AKS) önceden eğitilen düşünceleri tahmin modeline faaliyete konusunda yönergeler işaretçiler sağladığımız bu bölümü. Operationalization ortam Docker ve Kubernetes web hizmeti dağıtımını yönetmek için kümedeki sağlar. Daha fazla operationalization süreci hakkında bilgiler bulabilirsiniz [burada](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-service-deploy).

## <a name="conclusion"></a>Sonuç
Biz ayrıntılarını Word2Vec kullanarak bir word katıştırma modeli eğitmek ve sonra ayıklanan eklerinin özellikleri Twitter metin veri düşünceleri puanı tahmin etmek için iki farklı modeli eğitmek için nasıl geçti. Bu modeller birini Azure kapsayıcı Hizmetleri (AKS) dağıtılır. 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla üzerinde belgeleri okuyun [Azure Machine Learning paket metin Analytics (AMLPTA) için](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/textanalytics?view=azure-ml-py-latest) ve [takım veri bilimi işlem (TDSP)](https://aka.ms/tdsp) başlamak için.

## <a name="references"></a>Başvurular
* [Team Data Science Process](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview) 
* [Azure Machine Learning ekibi veri bilimi işlem (TDSP) kullanma](https://aka.ms/how-to-use-tdsp-in-aml)
* [Azure Machine Learning için TDSP proje şablonu](https://aka.ms/tdspamlgithubrepo)
* [Azure ML çalışma karşılaştırma](https://docs.microsoft.com/en-us/azure/machine-learning/preview/)
* [Mikolov, Tomas, et al. Sözcükler ve tümcecikleri ve bunların compositionality dağıtılmış gösterimlerini. Sinir bilgi systems işleme gelişmeler. 2013.](https://arxiv.org/abs/1310.4546)
