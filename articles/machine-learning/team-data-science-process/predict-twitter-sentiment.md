---
title: "Azure'da takım veri bilimi işlemi kullanarak word eklerinin ile twitter düşüncelerini tahmin | Microsoft Docs"
description: "Veri bilimi projelerinizi yürütmek için gerekli adımlar."
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
ms.openlocfilehash: 20bc3f31897cec4a3cec9ca409062229133102f5
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2018
---
# <a name="predict-twitter-sentiment-with-word-embeddings-by-using-the-team-data-science-process"></a>Takım veri bilimi işlemi kullanarak word eklerinin ile twitter düşüncelerini tahmin etme

Bu makalede etkili bir şekilde kullanarak işbirliği yapmak gösterilmiştir _Word2Vec_ algoritmayla ekleme word ve _düşünceleri özgü Word katıştırma (SSWE)_ algoritması ile Twitter düşüncelerini tahmin etmek için [Azure Machine Learning](../preview/index.yml). Twitter düşünceleri polarite tahmin etmeye daha fazla bilgi için bkz: [MachineLearningSamples TwitterSentimentPrediction deposu](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction) github'da. Veri bilimi projelerde etkili takım işbirliği kolaylaştırmanın için yerleşik veri bilimi yaşam döngüsü projelerle belgelerine ve yapısı standart hale getirmek için anahtardır. [Takım veri bilimi işlem (TDSP)](overview.md) bu tür sağlayan yapılandırılmış [yaşam döngüsü](lifecycle.md). 

Veri bilimi projelerle oluşturma _TDSP şablonu_ Azure Machine Learning projeleri için standartlaştırılmış bir çerçeve sağlar. Daha önce TDSP takım yayımlanan bir [TDSP proje yapısını ve şablonları için GitHub depo](https://github.com/Azure/Azure-TDSP-ProjectTemplate). Şimdi ile örneği Machine Learning projeleri [TDSP şablonları Azure Machine Learning için](https://github.com/amlsamples/tdsp) etkinleştirilir. Nasıl kullanılacağı hakkında yönergeler için bkz [TDSP yapısı projeleri TDSP şablonuyla](../preview/how-to-use-tdsp-in-azure-ml.md) Azure Machine learning'de. 


## <a name="twitter-sentiment-polarity-sample"></a>Twitter düşünceleri polarite örnek

Bu makalede bir örnek örneği ve Machine Learning proje yürütmek nasıl göstermek için kullanır. Örnek, Azure Machine Learning çalışma TDSP yapısı ve şablonları kullanır. Tam örnek sağlanan [bu kılavuzda](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/blob/master/docs/deliverable_docs/Step_By_Step_Tutorial.md). Modelleme görev tweet'leri metinden kullanarak düşünceleri polarite (olumlu veya olumsuz) tahmin eder. Bu makalede kılavuzda açıklanan veri modellemesi görevleri özetlenmektedir. İzlenecek yol aşağıdaki görevleri içerir:

- Veri keşfi, eğitim ve machine learning modeli dağıtımını, kullanım örneği genel bakış içinde açıklanan tahmin sorunu çözün. [Twitter düşünceleri veri](http://cs.stanford.edu/people/alecmgo/trainingandtestdata.zip) bu görevler için kullanılır.
- Bu proje için Azure Machine Learning TDSP şablonu kullanarak proje yürütme. Proje yürütme ve raporlama için TDSP yaşam döngüsü kullanılır.
- Azure Machine Learning Azure kapsayıcı Hizmeti'nde çözümden doğrudan operationalization.

Projeyi Azure Machine Learning aşağıdaki özellikleri vurgular:

- Örnek oluşturma ve TDSP yapısının kullanın.
- Azure Machine Learning çalışma ekranındaki kodun yürütülmesini.
- Docker ve Kubernetes kullanarak kolay operationalization kapsayıcı hizmetinde.

## <a name="team-data-science-process"></a>Team Data Science Process
Bu örnek yürütmek için Azure Machine Learning çalışma TDSP Proje yapısı ve belgeleri şablonları kullanın. Örnek uygulayan [TDSP yaşam döngüsü](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md), aşağıdaki çizimde gösterildiği gibi:

![TDSP yaşam döngüsü](./media/predict-twitter-sentiment/tdsp-lifecycle.PNG)

Azure Machine Learning çalışma'ekranındaki göre TDSP Proje oluşturulduktan [bu yönergeleri](https://github.com/amlsamples/tdsp/blob/master/docs/how-to-use-tdsp-in-azure-ml.md), aşağıdaki çizimde gösterildiği gibi:

![TDSP içinde Azure Machine Learning çalışma ekranı oluşturma](./media/predict-twitter-sentiment/tdsp-instantiation.PNG) 


## <a name="data-acquisition-and-preparationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode01dataacquisitionandunderstanding"></a>[Veri alımı ve hazırlık](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/01_data_acquisition_and_understanding)
Bu örnek ilk adımda sentiment140 dataset indirmek ve verileri eğitim ve test veri kümeleri halinde bölmek için ' dir. Sentiment140 dataset tweet gerçek içeriği (ile ifadeler kaldırıldı) içerir. Veri kümesi de her tweet polarite içerir (negatif = 0, pozitif = 4) ile nötr tweet'leri kaldırıldı. Veri bölündükten sonra eğitim verileri 1.3 milyon satır ve test verilerinin 320,000 satırları sahiptir.


## <a name="model-developmenthttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling"></a>[Model geliştirme](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling)

Sonraki örnek veriler için bir modeli geliştirmek için adımdır. Modelleme görev, üç bölüme ayrılır:

- Özellik Mühendisliği: algoritmaları katıştırma farklı sözcük kullanarak model özellikleri oluşturma. 
- Model oluşturma: giriş metni düşünceleri tahmin etmek için farklı modelleri eğitme. Bu modeller örneklerindendir _Lojistik regresyon_ ve _gradyan artırmanın_.
- Model değerlendirme: eğitilmiş modeller sınama verilerinde değerlendirin.


### <a name="feature-engineeringhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling01featureengineering"></a>[Özellik Mühendisliği](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/01_FeatureEngineering)

Word2Vec ve SSWE algoritmalar word eklerinin oluşturmak için kullanılır. 


#### <a name="word2vec-algorithm"></a>Word2Vec algoritması

Word2Vec algoritması Atla-Gram modelinde kullanılır. Bu model Tomas Mikolov tarafından makalede açıklanan et al. "[Dağıtılmış sözcükleri ve tümcecikleri ve bunların Compositionality gösterimlerini. Sinir bilgi systems işleme gelişmeler. ](https://arxiv.org/abs/1310.4546)" 2013.

Basit bir sinir ağı Atla-Gram modelidir. Girdi, sözcük tahmin etmek için kullanılan bir tek hot vektörü olarak kodlanmış hedef sözcüktür. Varsa **V** çıkış katman boyutu ardından sözlük boyutudur __C * V__ burada C, bağlam penceresinin boyutu. Aşağıdaki şekilde Atla-Gram modelini temel alan bir mimari gösterilmektedir:

![Atla-Gram modeli](./media/predict-twitter-sentiment/skip-gram-model.PNG)

Ayrıntılı mekanizması Word2Vec algoritması ve atlama-Gram modeli, bu örnek kapsamı dışındadır ' dir. Daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Başvurulan TensorFlow örneklerle 02_A_Word2Vec.PY kodu](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/word2vec/word2vec_basic.py) 
- [Vektör gösterimlerini sözcükler](https://www.tensorflow.org/tutorials/word2vec)
- [Word2vec tam olarak nasıl çalışır?](http://www.1-4-5.net/~dmm/ml/how_does_word2vec_work.pdf)
- [Gürültü Contrastive tahmin ve negatif örnekleme ile ilgili notlar](http://demo.clab.cs.cmu.edu/cdyer/nce_notes.pdf)




#### <a name="sentiment-specific-word-embedding-algorithm"></a>Word düşünceleri özgü katıştırma algoritması
SSWE algoritması sözcükler polarite ters benzer bağlamları ile word vektörleri sahip olduğu Word2Vec algoritmasının bir zayıflık üstesinden dener. Benzerlikleri doğru şekilde için düşünceleri analiz gibi görevleri gerçekleştirmek değil Word2Vec algoritması neden olabilir. Bu zayıflık cümle polarite ve word'ün bağlam kaybı işlevini ekleme işlemek SSWE algoritması çalışır.

Örnek bir değişken SSWE algoritmasının aşağıdaki gibi kullanır:

- Her iki özgün _ngram_ ve bozuk _ngram_ girdi olarak kullanılır.
- Bir stil desteği kaybı işlevi sıralaması kullanılan söz dizimi kaybı ve anlam kaybı için.
- Ultimate kaybı işlevi söz dizimi kaybı ve anlam kaybı ağırlıklı birleşimidir.
- Kolaylık sağlamak için yalnızca anlamsal entropi çapraz kaybı işlev olarak kullanılır.

Örnek bile daha basit kaybı işleviyle SSWE katıştırma performansı Word2Vec katıştırma daha iyi olduğunu gösterir. Aşağıdaki şekilde düşünceleri özgü word katıştırma oluşturmak için kullanılan convolutional modeli gösterilmektedir:

![Sinir ağı modelinin SSWE tarafından esin](./media/predict-twitter-sentiment/embedding-model-2.PNG)

Eğitim işlemini gerçekleştirdikten sonra iki katıştırma dosyaları sekmeyle ayrılmış değerler (TSV) biçiminde modelleme aşaması için oluşturulur.

Kağıt Duyu Tang tarafından SSWE algoritmalar hakkında daha fazla bilgi için bkz: et al. "[Düşünceleri özgü öğrenme Word için Twitter düşünceleri sınıflandırma katıştırma](http://www.aclweb.org/anthology/P14-1146)." İlişkilendirme hesaplama Linguistics (1) için. 2014.


### <a name="model-creationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling02modelcreation"></a>[Model oluşturma](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/02_ModelCreation)
Word vektörlerinin SSWE veya Word2Vec algoritması kullanılarak üretildikten sonra gerçek düşünceleri polarite tahmin etmek için sınıflandırma modelleri eğitildi. İki tür özelliklerin: Word2Vec ve SSWE, iki modellerine uygulanır: gradyan artırmanın modeli ve lojistik regresyon modeli. Sonuç olarak, dört farklı modelleri eğitilmiş olup.


### <a name="model-evaluationhttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode02modeling03modelevaluation"></a>[Model değerlendirme](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/02_modeling/03_ModelEvaluation)
Modelleri eğitilmiş sonra modelleri test Twitter metin verileri ve her modelinin performansını değerlendirmek için kullanılır. Örnek aşağıdaki dört modeller değerlendirir:

- SSWE Katıştırma üzerinden gradyan Boosting.
- SSWE Katıştırma üzerinden Lojistik regresyon.
- Word2Vec Katıştırma üzerinden gradyan Boosting.
- Word2Vec Katıştırma üzerinden Lojistik regresyon.

Performans dört modellerin karşılaştırması aşağıdaki şekilde gösterilmiştir:

![Özellik (ROC) modeli karşılaştırması işletim alıcı](./media/predict-twitter-sentiment/roc-model-comparison.PNG)

Gradyan artırmanın modeli SSWE özelliğiyle modelleri eğri (AUC) ölçüm alanında kullanarak karşılaştırılırken en iyi performansı verir.


## <a name="deploymenthttpsgithubcomazuremachinelearningsamples-twittersentimentpredictiontreemastercode03deployment"></a>[Dağıtım](https://github.com/Azure/MachineLearningSamples-TwitterSentimentPrediction/tree/master/code/03_deployment)

Son adım, bir web hizmetine Azure kapsayıcı hizmeti kümesinde eğitilen düşünceleri tahmin modelinin dağıtımıdır. Örnek gradyan artırmanın modeli SSWE katıştırma algoritmayla eğitilen model olarak kullanır. Operationalization ortam Docker ve Kubernetes aşağıdaki resimde gösterildiği gibi web hizmeti dağıtımını yönetmek için kümedeki sağlar: 

![Kubernetes Panosu](./media/predict-twitter-sentiment/kubernetes-dashboard.PNG)

Operationalization işlemi hakkında daha fazla bilgi için bkz: [bir Azure Machine Learning modeli bir web hizmeti olarak dağıtma](https://docs.microsoft.com/en-us/azure/machine-learning/preview/model-management-service-deploy).

## <a name="conclusion"></a>Sonuç

Bu makalede, Word2Vec ve Word düşünceleri özgü katıştırma algoritmaları kullanarak word katıştırma modeli eğitmek öğrendiniz. Ayıklanan eklerinin özellikleri Twitter metin veri düşünceleri puanlarını tahmin etmek için birkaç modeli eğitmek için kullanılmıştır. Gradyan artırmanın modeliyle kullanılan SSWE özelliği en iyi performansı verdi. Model sonra kapsayıcı hizmetinde bir gerçek zamanlı web hizmeti olarak Azure Machine Learning çalışma ekranı kullanarak dağıtıldı.


## <a name="references"></a>Başvurular

* [Takım veri bilimi işlemi](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview) 
* [Azure Machine Learning ekibi veri bilimi işlem (TDSP) kullanma](https://aka.ms/how-to-use-tdsp-in-aml)
* [Azure Machine Learning için TDSP proje şablonları](https://aka.ms/tdspamlgithubrepo)
* [Azure Machine Learning çalışma ekranı](https://docs.microsoft.com/en-us/azure/machine-learning/preview/)
* [UCI ML depodan ABD gelir veri kümesi](https://archive.ics.uci.edu/ml/datasets/adult)
* [TDSP şablonları kullanarak biomedical varlık tanıma](https://docs.microsoft.com/en-us/azure/machine-learning/preview/scenario-tdsp-biomedical-recognition)
* [Mikolov, Tomas, et al. "Sözcükler ve tümcecikleri ve bunların Compositionality gösterimlerini dağıtılmış. İşleme sistemlerinin sinir bilgilerinde ilerletir." 2013.](https://arxiv.org/abs/1310.4546)
* [Tang, Duyu, et al. "Twitter düşünceleri sınıflandırma için düşünceleri özgü Word katıştırma öğrenme." ACL (1). 2014.](http://www.aclweb.org/anthology/P14-1146)
