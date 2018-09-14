---
title: Azure Machine Learning (AML) paketiyle metin analizi (AMLPTA) ve Team Data Science işlem (TDSP) için duygu sınıflandırmasını twitter | Microsoft Docs
description: TDSP (Team Data Science Process) ve AMLPTA için duygu sınıflandırmasını kullanımını açıklar.
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
ms.openlocfilehash: 9e5018bc4c7b90897f7f8c91169410284217b172
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45577023"
---
# <a name="twitter-sentiment-classification-with-azure-machine-learning-aml-package-for-text-analytics-amlpta-and-team-data-science-process-tdsp"></a>Azure Machine Learning (AML) paketi için metin analizi (AMLPTA) ve Team Data Science işlem (TDSP) ile twitter duygu sınıflandırmasını

## <a name="introduction"></a>Giriş
Standartlaştırma yapısı ve belgeleri veri bilimi projeleri, diğer bir deyişle kurulan bir bağlantılı [veri bilimi yaşam döngüsünü](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md), veri bilimi ekipleri etkin işbirliği etkinleştirme için anahtardır.

Daha önce yayımladık bir [TDSP Proje yapısı ve şablonlar için GitHub deposu](https://github.com/Azure/Azure-TDSP-ProjectTemplate). Azure Machine Learning projelerin ile örneği oluşturma artık etkinleştirdik [TDSP yapısı ve belgeler şablonları Azure Machine Learning için](https://github.com/amlsamples/tdsp). TDSP yapısı ve şablonları Azure Machine Learning'de kullanmak hakkında yönergeler sağlanır [burada](https://docs.microsoft.com/azure/machine-learning/preview/how-to-use-tdsp-in-azure-ml). 

Bu örnekte, metin analizi ve TDSP geliştirmesini ve Twitter duygu sınıflandırmasını yönelik Tahmine dayalı modeller Azure Machine Learning paketi kullanımını göstermek için kullanacağız.


## <a name="use-case"></a>Kullanım örneği
### <a name="twitter-sentiment-polarity-sample"></a>Twitter yaklaşım kutup örnek
Bu makalede, örneği ve bir Machine Learning projesi yürütme işlemini göstermek için bir örnek kullanılmıştır. Örnek, Azure Machine Learning Workbench'te TDSP yapısı ve şablonlar kullanır. Bu izlenecek yolda tam bir örnek sağlanmaktadır. Modelleme görev yaklaşım kutup (pozitif veya negatif), tweet metni kullanarak tahmin eder. Bu makalede, bu kılavuzda açıklanan veri modelleme görevleri özetlenmektedir. İzlenecek yol aşağıdaki görevleri kapsar:

1. Veri araştırma, eğitim ve makine öğrenme modeli dağıtımını, kullanım örneği genel bakış içinde açıklanan tahmin sorunu çözün. Twitter duygu verilerini aşağıdaki görevler için kullanılır.
2. Bu proje için Azure Machine Learning TDSP şablonu kullanarak proje yürütme. TDSP yaşam döngüsü, proje yürütme ve raporlama için kullanılır.
3. Azure Machine Learning, Azure Container Service'te doğrudan çözümün kullanıma hazır hale getirme

Proje, Azure Machine Learning için metin analizi paket kullanımını vurgular.


## <a name="link-to-github-repository"></a>GitHub deponuza bağlayın
GitHub deposu bağlantısına [burada](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction). 

### <a name="purpose"></a>Amaç
Bu örnek birincil amacı, örneği ve bir machine learning Azure Machine Learning çalışma tezgahı içinde Team Data Science işlem (TDSP) yapısını ve şablonları kullanarak proje yürütmek nasıl göstermektir. Bu amaç için kullandığımız [Twitter duygu verilerini](http://cs.stanford.edu/people/alecmgo/trainingandtestdata.zip). Modelleme görev yaklaşım kutup (pozitif veya negatif) tahmin etmektir tweet metni kullanma.

### <a name="scope"></a>Kapsam
- Veri araştırma, eğitim ve kullanım çalışması genel bakış içinde açıklanan tahmin soruna yönelik bir makine öğrenme modelinin dağıtımı.
- Bu proje için Azure Machine Learning Team Data Science işlem (TDSP) şablonu kullanarak Azure Machine Learning projede yürütme. Proje yürütme ve raporlama için TDSP yaşam döngüsü kullanmayı ekleyeceğiz.
- Azure Machine Learning, Azure kapsayıcı Hizmetleri'ndeki doğrudan çözümün kullanıma hazır hale getirme

Proje çeşitli özellikler, Azure Machine Learning gibi TDSP yapısı örnekleme ve kullanımı, Docker ve Kubernetes kullanarak Azure Container Services kolay kullanıma hazır hale getirme ve Azure Machine Learning çalışma tezgahı kodları yürütülmesini vurgular.

## <a name="team-data-science-process-tds"></a>Team Data Science Process'i (akışı TDS)
Bu örnek yürütülecek TDSP proje yapısını ve belgelerini şablonları kullanın. Takip eden [TDSP yaşam döngüsü](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/lifecycle). Proje sağlanan yönergeler dayanarak oluşturulan [burada](https://github.com/amlsamples/tdsp/blob/master/docs/how-to-use-tdsp-in-azure-ml.md).


<img src="./media/predict-twitter-sentiment-amltextpackage/tdsp-lifecycle2.png" alt="tdsp-lifecycle" width="800" height="600">


## <a name="use-case-overview"></a>Kullanım örneği genel bakış
Her twitter'ın yaklaşım ikili kutup twitter metinden ayıklanan word Gömmeleri özelliklerini kullanarak tahmin etmek için bir görevdir. Ayrıntılı bir açıklaması için lütfen şuna bakın [depo](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction).

### <a name="data-acquisition-and-understandinghttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode01dataacquisitionandunderstanding"></a>[Veri edinme ve anlama](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/01_data_acquisition_and_understanding)
Bu örnek ilk adımda sentiment140 veri kümesini indirin ve eğitme bölme ve veri kümeleri test sağlamaktır. Sentiment140 veri kümesini içeren tweet gerçek içeriği (ile ifadeler kaldırıldı) yanı sıra her tweet kutup (negatif = 0, pozitif = 4) ile de nötr tweetleri kaldırıldı. Sonuçta elde edilen eğitim verilerini 1,3 milyon satır ve veri sınama 320 bin satır vardır.

### <a name="modelinghttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode02modeling"></a>[Modelleme](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/02_modeling)
Örneğinin bu parçası üç alt bölümleri kopyalanıyor daha ayrılmıştır: 
- **Özellik Mühendisliği** algoritması, yani Word2Vec katıştırma farklı sözcük kullanarak özellik oluşturulmasını karşılık gelir. 
- **Model oluşturma** farklı modellerin eğitimi anlaşmalar ister _Lojistik regresyon_ ve _gradyan artırma_ giriş metinlerdeki tahmin etmek için. 
- **Model değerlendirme** sınama verilerinde eğitim modeli uygular.

#### <a name="feature-engineering"></a>Özellik mühendisliği
Kullandığımız <b>Word2Vec</b> word Gömmeleri oluşturulacak. İlk Word2Vec algoritma Skipgram modda kağıt açıklandığı gibi kullanıyoruz [Mikolov, Tomas, tarayıcılarınızda. Sözcük ve tümcecikleri ve bunların compositionality dağıtılmış temsillerini. Sinir bilgi işleme sistemlerinin gelişmeler. 2013.](https://arxiv.org/abs/1310.4546) Word Gömmeleri oluşturmak için.

Skip-gram, giriş olarak sık erişimli bir vektörü olarak kodlanan ve sözcükler tahmin etmek için kullanarak hedef sözcük alma yüzeysel bir sinir ağdır. V sözlük boyutu olduğundan sonra çıktı katman boyutu olacaktır __C * V__ C bağlam penceresinin boyutunu olduğu. Aşağıdaki şekilde skip-gram tabanlı mimari gösterilmektedir.
 
<table class="image" align="center">
<caption align="bottom">Skip-gram modeli</caption>
<tr><td><img src="https://s3-ap-south-1.amazonaws.com/av-blog-media/wp-content/uploads/2017/06/05000515/Capture2-276x300.png" alt="Skip-gram model"/></td></tr>
</table>

Word2Vec algoritması ve atlama-gram modelinin ayrıntıları bu örnek kapsamı dışındadır ve ilginizi okuyucular daha fazla ayrıntı için aşağıdaki bağlantıları inceleyin istenir. Başvurulan kod 02_A_Word2Vec.py [tensorflow örnekleri](https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/tutorials/word2vec/word2vec_basic.py)

* [Vektör temsili sözcük](https://www.tensorflow.org/tutorials/word2vec)
* [Word2vec tam olarak nasıl çalışır?](http://www.1-4-5.net/~dmm/ml/how_does_word2vec_work.pdf)
* [Gürültü Contrastive tahmini ve negatif örnekleme ilgili notlar](http://demo.clab.cs.cmu.edu/cdyer/nce_notes.pdf)

Eğitim işlemini gerçekleştirdikten sonra TSV biçiminde iki katıştırma dosyalar modelleme aşama için oluşturulur.

#### <a name="model-training"></a>Modeli eğitimi
Word vektörleri SSWE veya Word2vec algoritması kullanılarak oluşturulmuş sonra sonraki adıma gerçek yaklaşım kutup tahmin etmek için sınıflandırma modellerini eğitin sağlamaktır. Şu özellikler iki tür uygulama: Word2Vec ve iki modeli içine SSWE: Lojistik regresyon ve Evrişimsel sinir ağları (CNN). 

#### <a name="model-evaluation"></a>Modeli değerlendirme
Kod yükleme ve test veri kümesi üzerinde birden fazla eğitilen modellerin değerlendirmesi için sunuyoruz.


### <a name="deploymenthttpsgithubcomazuremachinelearningsamples-amltextpackage-twittersentimentpredictiontreemastercode03deployment"></a>[Dağıtım](https://github.com/Azure/MachineLearningSamples-AMLTextPackage-TwitterSentimentPrediction/tree/master/code/03_deployment)
Bu bölümü, bir Azure Container Service (AKS) kümesi üzerinde bir web hizmeti için önceden eğitilmiş yaklaşım tahmin modeli kullanıma hazır hale getirme konusunda yönergeler için işaretçiler sunuyoruz. Kullanıma hazır hale getirme ortamı, Docker ve Kubernetes kümesini web hizmeti dağıtımı Yönet sağlar. Daha fazla kullanıma hazır hale getirme işlemi hakkında bilgi bulabilirsiniz [burada](https://docs.microsoft.com/azure/machine-learning/preview/model-management-service-deploy).

## <a name="conclusion"></a>Sonuç
Biz Word2Vec kullanarak bir word katıştırma modeli eğitmek ve sonra metin veri Twitter yaklaşım puanını tahmin etmek için iki farklı modelleri eğitmek için özellikleri ayıklanan Gömmeleri kullanmak hakkında ayrıntılara geçti. Bu modellerden biri, Azure Container Services (AKS) dağıtılır. 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla ilgili belgeleri okuyun [Azure Machine Learning paket için metin analizi (AMLPTA)](https://docs.microsoft.com/python/api/overview/azure-machine-learning/textanalytics?view=azure-ml-py-latest) ve [Team Data Science işlem (TDSP)](https://aka.ms/tdsp) kullanmaya başlamak için.

## <a name="references"></a>Başvurular
* [Team Data Science Process](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview) 
* [Azure Machine Learning'de Team Data Science işlem (TDSP) kullanma](https://aka.ms/how-to-use-tdsp-in-aml)
* [Azure Machine Learning için TDSP projesi şablonu](https://aka.ms/tdspamlgithubrepo)
* [Azure ML çalışma tezgahı](https://docs.microsoft.com/azure/machine-learning/preview/)
* [Mikolov, Tomas, v.d. Sözcük ve tümcecikleri ve bunların compositionality dağıtılmış temsillerini. Sinir bilgi işleme sistemlerinin gelişmeler. 2013.](https://arxiv.org/abs/1310.4546)
