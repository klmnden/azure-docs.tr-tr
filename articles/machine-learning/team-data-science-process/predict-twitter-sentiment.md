---
title: "Takım veri bilimi işlem - Azure kullanarak word eklerinin ile twitter düşüncelerini tahmin | Microsoft Docs"
description: "Veri bilimi projelerinizi yürütmek için gerekli olan adımları"
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: bradsev;
ms.openlocfilehash: fe1c87df40102a62e1e0c8873b25fa3df7d743bc
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2017
---
# <a name="predict-twitter-sentiment-with-word-embeddings-using-the-team-data-science-process"></a>Takım veri bilimi işlemi kullanarak word eklerinin ile twitter düşüncelerini tahmin

Bu makalede etkin kullanırken işbirliği gösterilmektedir **Word2Vec** algoritmayla ekleme word ve **düşünceleri belirli Word katıştırma (SSWE) algoritması** ile Twitter düşüncelerini tahmin etmek için [Azure Machine Learning](../preview/index.yml). Tahmin etmeye yönelik görevi hakkında daha fazla ayrıntı düşünceleri polarite twitter için bkz: [depo](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction). Veri bilimi projelerde etkili takım işbirliği kolaylaştırmanın için yerleşik veri bilimi yaşam döngüsü projelerle belgelerine ve yapısı standart hale getirmek için anahtardır. [Takım veri bilimi işlem (TDSP)](overview.md) yalnızca böyle bir yapılandırılmış sağlar [yaşam döngüsü](lifecycle.md). 

Veri bilimi projelerle oluşturma **TDSP şablonu** Azure Machine Learning projelerde bu standartlaştırılmış bir çerçeve sağlar. TDSP takım daha önce yayımlanmış bir [TDSP proje yapısını ve şablonları için GitHub depo](https://github.com/Azure/Azure-TDSP-ProjectTemplate). Şimdi ile örneği Azure Machine Learning projeleri oluşturma [TDSP yapısı ve belgeleri şablonları Azure Machine Learning için](https://github.com/amlsamples/tdsp) etkinleştirildi. Azure Machine Learning TDSP yapısı ve şablonları kullanma hakkında daha fazla yönerge için bkz: [yapısı projeleri takım veri bilimi işlem şablonu ile](../preview/how-to-use-tdsp-in-azure-ml.md). 


## <a name="the-sentiment-polarity-sample"></a>Düşünceleri polarite örnek

Örneği ve machine learning ekibi veri bilimi işlemi yapısını kullanarak proje ve Azure Machine Learning çalışma karşılaştırma şablonlarında sağlamış bu yürütmek nasıl oluşturulduğunu gösteren bir örnek [izlenecek](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/blob/master/docs/deliverable_docs/Step_By_Step_Tutorial.md). Modelleme düşünceleri polarite (olumlu veya olumsuz) tahmin etmek için bir görevdir tweet'leri metinden kullanma. Bu makalede kılavuzda ele görevleri modelleme verileri ana hatlarını vermektedir. İzlenecek yol aşağıdaki görevleri içerir:

- Veri keşfi, eğitim ve kullanım durumu genel bakış içinde açıklanan tahmin soruna yönelik bir makine öğrenimi modeline dağıtımı. Bu amaçla [Twitter düşünceleri veri](http://cs.stanford.edu/people/alecmgo/trainingandtestdata.zip) kullanılır.
- Bu proje için Azure Machine Learning ekibi veri bilimi işlem (TDSP) şablonu kullanarak Azure Machine Learning projesinde yürütme. Proje yürütme ve raporlama için TDSP yaşam döngüsü kullanılır.
- Azure Machine Learning Azure kapsayıcı Services'de çözümden doğrudan operationalization.

Proje çeşitli özelliklerini Azure Machine Learning, bu tür TDSP yapısı örneklemesi ve kullanım, Azure Machine Learning çalışma karşılaştırma kodda ve Docker ve Kubernetes kullanarak Azure kapsayıcı Services içinde kolay operationalization yürütülmesini vurgular.

## <a name="team-data-science-process"></a>Team Data Science Process
Bu örnek yürütmek için TDSP Proje yapısı ve belgeleri şablonları kullanın. Kendisini izleyen [TDSP yaşam döngüsü]((https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md)). Proje sağlanan yönergeleri temel alınarak oluşturulur [burada](https://github.com/amlsamples/tdsp/blob/master/docs/how-to-use-tdsp-in-azure-ml.md).

![TDSP yaşam döngüsü](./media/predict-twitter-sentiment/tdsp-lifecycle.PNG)

![TDSP örneği](./media/predict-twitter-sentiment/tdsp-instantiation.PNG) 


## <a name="data-acquisition-and-understandinghttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode01dataacquisitionandunderstanding"></a>[Veri alımı ve anlama](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/01_data_acquisition_and_understanding)
Bu örnek ilk adımda sentiment140 dataset indirmek ve eğitim ve test veri kümeleri halinde bölmek için ' dir. Sentiment140 dataset tweet gerçek içeriği (ile ifadeler kaldırıldı) yanı sıra her bir tweet polarite içerir (negatif = 0, pozitif = 4), kaldırılan nötr tweet'leri ile. Bölünmüş, sonuçta elde edilen eğitim verileri 1.3 milyon satır ve test verilerinin 320 k satırları sahiptir.


## <a name="modelinghttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling"></a>[Modelleme](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling)

Bu örnek parçası daha fazla üç kısma ayrılmıştır:
 
- **Özellik Mühendisliği** algoritmaları katıştırma farklı sözcük kullanarak özellik nesil karşılık gelir. 
- **Model oluşturma** eğitim anlaşmalar başka modellerinin ister _Lojistik regresyon_ ve _gradyan artırmanın_ giriş metni düşünceleri tahmin etmek için. 
- **Model değerlendirme** eğitilen modeli sınama verilerinde geçerlidir.


### <a name="feature-engineeringhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling01featureengineering"></a>[Özellik Mühendisliği](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/01_FeatureEngineering)

Word2Vec ve SSWE burada word eklerinin oluşturmak için kullanılan algoritmalar katıştırma word markalarıdır. 


#### <a name="word2vec"></a>Word2Vec

Word2Vec algoritması Skipgram modda kullanılır. Word eklerinin oluşturmanın bu şekilde kağıt Tomas Mikolov et al. tarafından açıklanmıştır: [Beyanları sözcük dağıtılmış ve tümcecikleri ve bunların Compositionality. Sinir bilgi systems işleme gelişmeler. 2013.](https://arxiv.org/abs/1310.4546).

Atla-gram basit bir sinir ağı ' dir. Kendi giriş sözcükler tahmin etmek için kullandığı tek bir dinamik vektör olarak kodlanmış hedef sözcüğüdür. Varsa **V** çıkış katman boyutu şu şekilde olacaktır sözlük boyutudur __C * V__ burada C, bağlam penceresinin boyutu. Atla-gram tabanlı mimari aşağıdaki şekilde gösterilmiştir:

![Atla-gram modeli](./media/predict-twitter-sentiment/skip-gram-model.PNG)

***Atla-gram modeli***

Ayrıntılı mekanizması word2vec algoritması ve atlama-gram modeli, bu örnek kapsamı dışındadır ' dir. Okuyucu daha fazla ilgi kendi işleyişini ayrıntılarını aşağıdaki kaynaklara bakın:

- [Kod 02_A_Word2Vec.py TensorFlow örnekler başvurulan](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/word2vec/word2vec_basic.py)
- [Sözcükler vektör gösterimi](https://www.tensorflow.org/tutorials/word2vec)
- [Word2vec tam olarak nasıl çalışır?](http://www.1-4-5.net/~dmm/ml/how_does_word2vec_work.pdf)
- [Gürültü Contrastive tahmin ve negatif örnekleme ile ilgili notlar](http://demo.clab.cs.cmu.edu/cdyer/nce_notes.pdf)


#### <a name="sswe"></a>SSWE
**Düşünceleri belirli Word katıştırma (SSWE) algoritması** benzer Bağlamlar ile ve polarite ters sözcükleri word vektörleri olabilir Word2vec algoritmasının bir zayıflık aşmak çalışır. Bu Word2vec doğru şekilde düşünceleri analiz gibi görevler için çalışmayabilir, anlamına gelir. Bu zayıflık cümle polarite ve word'ün bağlam kaybı işlevini ekleme işlemek SSWE algoritması çalışır.

Bu örnek bir türevi SSWE kullanır. SSWE hem özgün ngram ve giriş ve olarak bozuk ngram kullanır bir derecelendirme stil söz dizimi kaybı ve anlam kaybı kaybı işlevi desteği. Ultimate kaybı işlevi söz dizimi kaybı ve anlam kaybı ağırlıklı birleşimidir. Kolaylık olması amacıyla, yalnızca anlamsal entropi çapraz kaybı işlevi olarak kullanılır. Daha sonra gördüğünüz gibi daha basit kaybı işlevi ile bile bu SSWE katıştırma performans Word2Vec katıştırma daha iyidir.

Bu örnekte kullanmak SSWE esin sinir ağı modelinin aşağıdaki şekilde gösterilmiştir:

![ROC modeli karşılaştırması](./media/predict-twitter-sentiment/embedding-model-2.PNG)

***Düşünceleri özgü word katıştırma oluşturmak için convolutional modeli.***


Eğitim işlemini gerçekleştirdikten sonra iki katıştırma dosyaları TSV biçiminde modelleme aşaması için oluşturulur.

Kağıt tarafından Duyu Tang et al. SSWE algoritmalar hakkında daha fazla bilgi için bkz: [öğrenme düşünceleri özgü Word katıştırma Twitter düşünceleri sınıflandırması için. ACL (1). 2014.](http://www.aclweb.org/anthology/P14-1146) 


### <a name="model-creationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling02modelcreation"></a>[Model oluşturma](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/02_ModelCreation)
Word vektörlerinin SSWE veya Word2vec algoritmaları birini kullanarak oluşturulmuş sonra sıradaki adım gerçek düşünceleri polarite tahmin etmek için sınıflandırma modelleri eğitme olacak. İki tür özellikleri, Word2Vec ve SSWE, iki modeli, GBM modeli ve lojistik regresyon modeli uygulanıyor. Bu nedenle dört farklı modelleri eğitilmiş olup.


### <a name="model-evaluationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling03modelevaluation"></a>[Model değerlendirme](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/03_ModelEvaluation)
Şimdi modelin performansını değerlendirmek için verileri test etme, önceki adımda eğitilmiş dört modelleri kullanın. 

1. SSWE Katıştırma üzerinden gradyan Boosting
2. SSWE Katıştırma üzerinden Lojistik regresyon
3. Word2Vec Katıştırma üzerinden gradyan Boosting
4. Word2Vec Katıştırma üzerinden Lojistik regresyon

![ROC modeli karşılaştırması](./media/predict-twitter-sentiment/roc-model-comparison.PNG)

AUC ölçümü kullanarak en iyisi SSWE özellikleriyle GBM modelidir.


## <a name="deploymenthttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode03deployment"></a>[Dağıtım](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/03_deployment)

Bu bölümü, bir kümede Azure kapsayıcı Hizmeti'ni (ACS) bir web hizmetine (SSWE katıştırma + GBM modeli) eğitilen düşünceleri tahmin modeli dağıtır. Operationalization ortam Docker ve Kubernetes web hizmeti dağıtımını yönetmek için kümedeki sağlar. Daha fazla operationalization süreci hakkında bilgiler bulabilirsiniz [burada](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-service-deploy).

![kubenetes_dashboard](./media/predict-twitter-sentiment/kubernetes-dashboard.PNG)


## <a name="conclusion"></a>Sonuç

Word2Vec ve SSWE algoritmaları kullanarak word katıştırma bir modeli eğitmek ayrıntılı açıklanan ve Twitter metin veri düşünceleri puanlarını tahmin etmek için birkaç modeli eğitmek için kullanılan ayıklanan eklerinin özellikleri. Gradyan Boosted ağacı modeli düşünceleri belirli ifadesi Embeddings(SSWE) özelliğiyle en iyi performansı verdi. Bu model, gerçek zamanlı web hizmeti olarak Azure Machine Learning çalışma karşılaştırma kullanarak Azure kapsayıcı Services sonra dağıtıldı.


## <a name="references"></a>Başvurular

* [Takım veri bilimi işlemi](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview) 
* [Azure Machine Learning ekibi veri bilimi işlem (TDSP) kullanma](https://aka.ms/how-to-use-tdsp-in-aml)
* [Azure Machine Learning için TDSP proje şablonu](https://aka.ms/tdspamlgithubrepo)
* [Azure ML çalışma karşılaştırma](https://docs.microsoft.com/en-us/azure/machine-learning/preview/)
* [Veri kümesi UCI ML depodan ABD gelir](https://archive.ics.uci.edu/ml/datasets/adult)
* [Biomedical varlık TDSP şablonu kullanarak tanıma](https://docs.microsoft.com/en-us/azure/machine-learning/preview/scenario-tdsp-biomedical-recognition)
* [Mikolov, Tomas, et al. Sözcükler ve tümcecikleri ve bunların compositionality dağıtılmış gösterimlerini. Sinir bilgi systems işleme gelişmeler. 2013.](https://arxiv.org/abs/1310.4546)
* [Tang, Duyu, et al. "Twitter düşünceleri sınıflandırma için düşünceleri özgü Word katıştırma öğrenme." ACL (1). 2014.](http://www.aclweb.org/anthology/P14-1146)