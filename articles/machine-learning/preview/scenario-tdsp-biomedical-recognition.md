---
title: "Biomedical varlık tanıma - takım veri bilimi işlem - Azure Machine Learning | Microsoft Docs"
description: "Azure Machine Learning çalışma ekranındaki biomedical varlık tanıma için derin öğrenme kullanan bir takım veri bilimi işlemi Proje Hızlı Başlangıç."
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
ms.date: 09/10/2017
ms.author: bradsev
ms.openlocfilehash: 21f8f66d8b78c2b536792bc96e9233d5739fde81
ms.sourcegitcommit: 4ed3fe11c138eeed19aef0315a4f470f447eac0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2017
---
# <a name="biomedical-entity-recognition-using-team-data-science-process-tdsp-template"></a>Takım veri bilimi işlem (TDSP) şablonu kullanarak biomedical varlık tanıma

Varlık ayıklama olan bilgi ayıklama görevinin (olarak da bilinen [adlandırılmış varlık tanıma (NER)](https://en.wikipedia.org/wiki/Named-entity_recognition), varlık Öbekleme ve varlık kimliği). Azure Machine Learning çalışma ekranı varlık ayıklama yapılandırılmamış metinden gibi karmaşık doğal dil işleme (NLP) görev çözmek için nasıl kullanılacağını vurgulamak için bu gerçek dünya senaryoları amacı şöyledir:

1. Sinir word eğitmek için eklerinin kullanma hakkında 18 milyon PubMed özetleri üzerinde bir metin gövde modeli nasıl [Spark Word2Vec uygulama](https://spark.apache.org/docs/latest/mllib-feature-extraction.html#word2vec).
2. Derin bir uzun kısa vadeli bellek (LSTM) yinelenen sinir ağı modeli varlık ayıklama üzerinde bir GPU etkin Azure veri bilimi sanal makine (GPU DS VM) için Azure üzerinde oluşturma.
2. Bu etki alanına özgü word eklerinin modeli genel word eklerinin varlık tanıma görev modellerinde daha iyi performans gösterir göstermektedir. 
3. Eğitmek ve Azure Machine Learning çalışma ekranı kullanarak derin learning modellerini faaliyete geçirmek nasıl ekleyebileceğiniz gösterilmektedir.

4. Azure Machine Learning çalışma ekranının içinden aşağıdaki özellikleri gösterilmektedir:

    * Eşlemesinin [takım veri bilimi işlem (TDSP) yapısı ve şablonları](how-to-use-tdsp-in-azure-ml.md).
    * İndirme ve yükleme de dahil olmak üzere, proje bağımlılıklarını otomatik yönetimi
    * Python komut differetn yürütülmesi ortamları işlem.
    * Python komut dosyaları için geçmişin çalıştırın.
    * İşleri uzaktan Spark üzerinde yürütme bağlamı Hdınsight Spark 2.1 kümeleri kullanarak işlem.
    * Azure üzerinde uzak GPU VM'ler işleri yürütme.
    * Web hizmetleri Azure kapsayıcı Hizmetleri (ACS) üzerinde derin öğrenme modellerin kolay operationalization.

## <a name="use-case-overview"></a>Kullanım örneği'ne genel bakış
Biomedical adlandırılmış varlık tanıma gibi karmaşık biomedical NLP görevleri için kritik bir adımı şöyledir: 
* Adlandırılmış varlıklar belirtilenlerden böyle diseases, İlaçlar, chemicals ve Belirtiler elektronik sağlık veya sistem durumu kayıtları ayıklanıyor.
* Uyuşturucu bulma
* Farklı varlık arasındaki etkileşimler anlama gibi Uyuşturucu Uyuşturucu etkileşim, Uyuşturucu Hastalık ilişki ve gene protein ilişki türleri.

Bizim kullanım senaryosu nasıl yapılandırılmamış verileri gövde Medline PubMed özetleri gibi büyük miktarda modeli katıştırma bir sözcük eğitmek için çözümlenebilir üzerine odaklanır. Ardından çıktı eklerinin sinir varlık ayıklayıcısı eğitmek için otomatik olarak oluşturulan özellikleri kabul edilir.

Bizim sonuçları özellikleri katıştırma etki alanına özgü Word'ün üzerinde biomedical varlık ayıklama model eğitim genel özellik türüne eğitilmiş model sürümlerden üstünlüğü gösterir. Etki alanına özgü model doğru (dışında 9475) 7012 varlıklar 0.61 genel modeli için F1 puanı 5274 varlıklara karşılaştırıldığında 0.73 F1 puanı algılayabilir.

Aşağıdaki şekilde veri işleme ve modeli eğitmek için kullanılan mimari gösterilmektedir.

![Mimari](./media/scenario-tdsp-biomedical-recognition/architecture.png)

## <a name="data-description"></a>Veri açıklaması

### <a name="1-word2vec-model-training-data"></a>1. Word2Vec model eğitim verileri
Biz öncelikle ham MEDLINE özet verileri indirilen [MEDLINE](https://www.nlm.nih.gov/pubs/factsheets/medline.html). Verileri, XML dosyalarını biçiminde herkese açık kendi [FTP sunucusu](https://ftp.ncbi.nlm.nih.gov/pubmed/baseline). Yok 892 XML dosyalarını sunucu üzerinde ve XML dosyalarının her birinin 30.000 makalelerin bilgiler içerir. Veri toplama adımı hakkında daha fazla ayrıntı proje yapısını bölümünde verilmiştir. Her dosya içine mevcut alanlar 
        
        abstract
        affiliation
        authors
        country 
        delete: boolean if False means paper got updated so you might have two XMLs for the same paper.
        file_name   
        issn_linking    
        journal 
        keywords    
        medline_ta: this is abbreviation of the journal nam 
        mesh_terms: list of MeSH terms  
        nlm_unique_id   
        other_id: Other IDs 
        pmc: Pubmed Central ID  
        pmid: Pubmed ID
        pubdate: Publication date
        title

### <a name="2-lstm-model-training-data"></a>2. LSTM model eğitim verileri

Sinir varlık ayıklama modeli eğitilmiş ve publiclly kullanılabilir veri kümelerinin değerlendirilir. Bu veri kümeleri hakkında ayrıntılı bir açıklama almak için aşağıdaki kaynaklara bakın:
 * [Varlığa biyografisi tanıma görevini BioNLP/NLPBA 2004](http://www.nactem.ac.uk/tsujii/GENIA/ERtask/report.html)
 * [BioCreative V CDR görev gövde](http://www.biocreative.org/tasks/biocreative-v/track-3-cdr/)
 * [Semeval 2013 - görev 9.1 (Uyuşturucu tanıma)](https://www.cs.york.ac.uk/semeval-2013/task9/)

## <a name="link-to-the-azure-gallery-github-repository"></a>Azure galerisinde GitHub deponuza bağlayın
Aşağıdaki kodu içeren gerçek senaryosunun genel GitHub depo bağlantıdır ve daha ayrıntılı açıklaması: 

[https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction)


## <a name="prerequisites"></a>Ön koşullar 

* Bir Azure [abonelik](https://azure.microsoft.com/free/)
* Azure Machine Learning çalışma ekranı. Bkz: [Yükleme Kılavuzu](quickstart-installation.md). Şu anda Azure Machine Learning çalışma ekranı yalnızca aşağıdaki işletim sistemlerinde yüklenebilir: 
    * Windows 10 veya Windows Server 2016
    * macOS Sierra (veya daha yeni)


### <a name="azure-services"></a>Azure hizmetleri
* [Hdınsight Spark kümesinde](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql) sürüm Spark 2.1 (HDI 3.6) Linux'ta genişleme hesaplama için. Aşağıda açıklanan MEDLINE özetleri tam miktarını işlemek için minimum yapılandırması gerekir: 
    * Baş düğüm: [D13_V2](https://azure.microsoft.com/pricing/details/hdinsight/) boyutu
    * Alt düğümleri: en az 4 [D12_V2](https://azure.microsoft.com/pricing/details/hdinsight/). Bizim işlerinde D12_V2 boyutunun 11 çalışan düğümleri kullandık.
* [NC6 veri bilimi sanal makine (DSVM)](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-linux-dsvm-intro) büyütme hesaplama için.

### <a name="python-packages"></a>Python paketlerini

Tüm gerekli bağımlılıkları senaryo proje klasörü altında aml_config/conda_dependencies.yml dosyasında tanımlanır. Bu dosyada tanımlanan bağımlılıklar çalıştırmaları için otomatik olarak sağlanacak docker, VM ve HDI karşı küme hedefler. Conda ortam dosyası biçimi hakkında daha fazla ayrıntı için başvurmak [burada](https://conda.io/docs/using/envs.html#create-environment-file-by-hand).

* [TensorFlow](https://www.tensorflow.org/install/)
* [CNTK 2.0](https://docs.microsoft.com/cognitive-toolkit/using-cntk-with-keras)
* [Keras](https://keras.io/#installation)
* NLTK
* Fastparquet

### <a name="basic-instructions-for-azure-machine-learning-aml-workbench"></a>Azure Machine Learning (AML) çalışma ekranı için temel yönergeleri
* [Genel Bakış](overview-what-is-azure-ml.md)
* [Yükleme](quickstart-installation.md)
* [TDSP kullanma](how-to-use-tdsp-in-azure-ml.md)
* [Dosyaları okuma ve yazma nasıl](how-to-read-write-files.md)
* [Jupyter not defterlerini kullanma](how-to-use-jupyter-notebooks.md)
* [GPU kullanma](how-to-use-gpu.md)

## <a name="scenario-structure"></a>Senaryo yapısı
Senaryo için takip eden TDSP Proje yapısı ve belgeleri şablonları (Şekil 1) kullanıyoruz [TDSP yaşam döngüsü](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md). Proje sağlanan yönergeleri göre oluşturulur [burada](./how-to-use-tdsp-in-azure-ml.md).


![Proje bilgileri doldurun](./media/scenario-tdsp-biomedical-recognition/instantiation-3.png) 

Adım adım veri bilimi akışı aşağıdaki gibidir:

### <a name="1-data-acquisition-and-understanding"></a>1. Veri edinme ve anlama

Bkz: [veri alımı ve anlama](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/blob/master/code/01_data_acquisition_and_understanding/ReadMe.md).

Yaklaşık 10 milyon makaleleri boş bir Özet alan sahip olduğu 27 milyon özetleri toplam ham MEDLINE gövde var. Azure Hdınsight Spark tek bir makineye belleğe yüklenemiyor büyük verileri işlemek için kullanılan bir [Pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html). İlk olarak, verileri Spark kümesi yüklenir. Üzerinde aşağıdaki adımları yürütüldükten sonra [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html): 
* Medline XML Ayrıştırıcı kullanarak XML dosyalarını ayrıştırma
* tümce bölme, simgeleştirme ve büyük/küçük harf normalleştirmesi dahil olmak üzere Özet metni önişle.
* Özet alanı boş veya kısa metin yüklü olduğu makaleleri Dışla 
* word sözlük eğitim özetleri oluşturma
* sinir modeli katıştırma word eğitmek. Daha fazla ayrıntı için başvurmak [GitHub kodu bağlantı](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/blob/master/code/01_data_acquisition_and_understanding/ReadMe.md) başlamak için.


XML dosyaları ayrıştırma sonra verileri aşağıdaki biçime sahiptir: 

![Veri örneği](./media/scenario-tdsp-biomedical-recognition/datasample.png)

Sinir varlık ayıklama modeli eğitilmiş ve publiclly kullanılabilir veri kümelerinin değerlendirilir. Bu veri kümeleri hakkında ayrıntılı bir açıklama almak için aşağıdaki kaynaklara bakın:
 * [Varlığa biyografisi tanıma görevini BioNLP/NLPBA 2004](http://www.nactem.ac.uk/tsujii/GENIA/ERtask/report.html)
 * [BioCreative V CDR görev gövde](http://www.biocreative.org/tasks/biocreative-v/track-3-cdr/)
 * [Semeval 2013 - görev 9.1 (Uyuşturucu tanıma)](https://www.cs.york.ac.uk/semeval-2013/task9/)

### <a name="2-modeling"></a>2. Modelleme

Bkz: [modelleme](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling).

Modelleme, nasıl modeli katıştırma kendi word eğitim için önceki bölümde indirilen veriler kullanın ve aşağı akış diğer görevler için kullanın burada gösteriyoruz aşamasıdır. PubMed veri kullanıyoruz olsa da eklerinin oluşturmak için ardışık düzen geneldir ve başka bir etki alanı için word eklerinin eğitmek için yeniden kullanılabilir. Verileri doğru bir temsili olarak eklerinin için word2vec büyük miktarda veri eğitildi gereklidir.
Biz word eklerinin hazır olduktan sonra biz öğrenilen eklerinin katıştırma katman başlatmak için kullandığı bir derin sinir ağı modeli eğitmek. Biz katıştırma katman trainable olmayan olarak işaretleme, ancak bu zorunlu değildir. Eğitim modeli katıştırma word'ün Denetimsiz ve bu nedenle biz etiketlenmemiş metinlerinin yararlanamazsınız. Bununla birlikte, varlık tanıma modeli eğitim denetimli öğrenme bir görevdir ve doğruluğunu tutar ve el ile Açıklama veri kalitesi bağlıdır. 


#### <a name="21-feature-generation"></a>2.1. Özellik oluşturma

Bkz: [özellik oluşturma](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/01_feature_engineering).

Word2Vec etiketlenmemiş eğitim gövde sinir ağı modelden eğitir Denetimsiz öğrenme algoritmasını katıştırma sözcüğüdür. Her sözcüğün anlamsal bilgilerini temsil eden gövde için sürekli vektör üretir. Bu modeller basit sinir tek bir gizli katmanla ağlardır. Word vektörlerinin/eklerinin backpropagation ve stokastik gradyan düşüşü tarafından öğrenilen. Word2vec modeller, yani, Atla-Gram ve sürekli-paketi-in-sözcükleri iki tür vardır (denetleyin [burada](https://arxiv.org/pdf/1301.3781.pdf) daha fazla ayrıntı için). Atla-gram modelini destekler, word2vec Mllib'i'nın uyarlamasını kullanıyoruz beri biz kısaca, burada açıklayın (alınan görüntü [bağlantı](https://ahmedhanibrahim.wordpress.com/2017/04/25/thesis-tutorials-i-understanding-word2vec-for-word-embedding-i/)): 

![Atla Gram modeli](./media/scenario-tdsp-biomedical-recognition/skip-gram.png)

Performansı iyileştirmek için hiyerarşik Softmax ve negatif örnekleme modelini kullanır. Hiyerarşik SoftMax (S-SoftMax) tarafından ikili ağaçlara esin bir yaklaşık bir değeridir. H SoftMax ayrıldığında sözcükleri hiyerarşik bir katmanla temelde düz SoftMax katman yerini alır. Bu bir sözcük olasılık bize pahalı normalleştirme tüm sözcükleri hesaplamak gereğini ortadan kaldırır olasılık hesaplamaları dizisi içine hesaplama parçalayın olanak tanır. Dengeli bir ikili ağacı log2 derinliği olduğundan (| V |) (V olduğundan sözlük), yalnızca en fazla log2 değerlendirmek ihtiyacımız (| V |) bir sözcük son olasılığını elde etmek için düğümleri. Bağlam c verilen word w olasılığını sonra yalnızca sağ almaya olasılıklar ürünüdür ve sol sırasıyla yaprak düğümü o müşteri adayına kapatır. Biz daha sık sözcükleri daha kısa Beyanları aldığından emin olmak için veri kümesi sözcükleri sıklığını göre Huffman ağacı oluşturabilirsiniz. Daha fazla bilgi için bkz [bu bağlantıyı](http://sebastianruder.com/word-embeddings-softmax/).
Alınan görüntü [burada](https://ahmedhanibrahim.wordpress.com/2017/04/25/thesis-tutorials-i-understanding-word2vec-for-word-embedding-i/).

##### <a name="visualization"></a>Görselleştirme

Word eklerinin sahibiz sonra bunları görselleştirmek ve anlam olarak benzer sözcükleri arasındaki ilişkiyi görmek isteriz. 

![W2V benzerlik](./media/scenario-tdsp-biomedical-recognition/w2v-sim.png)

Biz eklerinin görselleştirmenin iki farklı şekilde göstermiştir. Birinci bir PCA yüksek boyutlu vektör 2B vektör alanına proje için kullanır. Bu bilgi önemli kaybına yol açar ve görselleştirme doğru değil. İkinci PCA ile kullanmaktır [t SNE](https://distill.pub/2016/misread-tsne/). t-SNE alanına bir dağılım çizim canlandırılabilir iki veya üç boyut yüksek boyutlu bir veri katıştırmak için oldukça uygun olan bir doğrusal boyut azaltma tekniğidir.  Bir iki veya three dimensional noktası tarafından benzer nesneleri yakındaki noktaları tarafından modellenir ve farklı nesneleri uzak noktaları tarafından modellenir şekilde yüksek boyutlu her nesnenin modeller. İki parça halinde çalışır. İlk olarak, daha yüksek boyutlu alanı çekilen, yüksek olasılık benzer nesneleri olan bir şekilde çiftlerinde üzerinden olasılık dağılımının oluşturur ve çekme düşük olasılık farklı noktaları sahip. İkinci olarak, düşük boyutlu eşlemesindeki noktaları üzerinden benzer bir olasılık dağılımının tanımlar ve harita noktalarında konumunu göre iki dağıtımlar arasında KL Geçitler en aza indirir. Düşük boyutundaki noktaların konumunu gradyan düşüşü kullanarak KL Geçitler en aza indirerek elde edilir. Ancak t SNE her zaman güvenilir olmayabilir. Uygulama Ayrıntıları bulunabilir [burada](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/01_feature_engineering). 


Aşağıdaki çizimde gösterildiği gibi t-SNE görselleştirme word vektörlerinin ve olası kümeleme desenlerini daha fazla ayrımı sağlar. 


* PCA Görselleştirme

![PCA](./media/scenario-tdsp-biomedical-recognition/pca.png)

* T-SNE Görselleştirme

![t-SNE](./media/scenario-tdsp-biomedical-recognition/tsne.png)

* "Kanseri" en yakın noktaları (Kanseri, tüm alt oldukları)

![Noktaları Kanseri için en yakın](./media/scenario-tdsp-biomedical-recognition/nearesttocancer.png)

#### <a name="22-train-the-neural-entity-extractor"></a>2.2. Tren sinir varlık ayıklayıcısı

Bkz: [sinir varlık ayıklayıcısı eğitmek](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/02_model_creation/ReadMe.md).

Akış İleri sinir ağı mimarisi her giriş kabul eder ve diğer girişleri ve çıkışları bağımsız çıkış sorunu yaşar. Bu mimari makine çevirisi ve varlık ayıklama gibi sırası dizisi etiketleme görevleri model olamaz. Bir sonraki düğüme kadar için şimdi hesaplanan bilgi geçirirken yinelenen sinir ağı modelleri bu sorunu çözmek. Bu özellik aşağıdaki şekilde gösterildiği gibi önceden hesaplanan bilgi kullanmanız mümkün olduğundan bellek ağında sahip çağrılır:

![RNN](./media/scenario-tdsp-biomedical-recognition/rnn-expanded.png)

Temel alınan RNNs gerçekten yaşar [kaybolmasını gradyan sorun](https://en.wikipedia.org/wiki/Vanishing_gradient_problem) nedeniyle, bunlar bunlar görülen önce tüm bilgileri kullanmak mümkün değildir. Bağlam, büyük bir miktarını tahminde bulunmak için yalnızca gerekli olduğunda sorun korumalı olur. Ancak modelleri LSTM gibi böyle bir sorunu karşılaşmaz, uzun vadeli bağımlılıkları anımsaması aslında tasarlanmıştır. Tek bir sinir ağı sahip RNNs vanilla farklı olarak, her hücre için dört sinir ağları arasındaki etkileşimler LSTMs sahip. LSTM nasıl çalıştığını ayrıntılı açıklaması için başvurmak [bu post](http://colah.github.io/posts/2015-08-Understanding-LSTMs/).

![LSTM hücre](./media/scenario-tdsp-biomedical-recognition/lstm-cell.png)

Şimdi kendi LSTM tabanlı yinelenen sinir ağı araya deneyin ve varlık türleri Uyuşturucu, Hastalık ve belirti belirtilenlerden gibi PubMed verileri ayıklamak deneyin. Büyük miktarda etiketli veri almak için ilk adımdır ve tahmin gibi bu kolay değil! Sağlık verilerinin en çok sayıda kişi hakkında hassas bilgi içeriyor ve bu nedenle genel kullanıma açık değildir. Biz, genel kullanıma açık iki farklı veri kümesi bir birleşimini kullanır. İlk veri kümesini Semeval 2013 - görev 9.1 (Uyuşturucu tanıma) ve diğer BioCreative V CDR görevden. Biz birleştirme olan ve böylece biz İlaçlar ve tıbbi metinleri gelen diseases algılamak ve bizim word eklerinin değerlendirmek bu iki veri kümesi etiketleme otomatik. Uygulama ayrıntılarını başvurmak [GitHub kodu bağlantı](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/02_model_creation).

Tüm kodları üzerinden ve karşılaştırma için kullandık modeli mimari aşağıda sunulmuştur. Farklı veri kümeleri için değişiklikler parametre, en büyük sıra uzunluğu (burada 613) değil.

![LSTM modeli](./media/scenario-tdsp-biomedical-recognition/d-a-d-model.png)

#### <a name="23-model-evaluation"></a>2.3. Model değerlendirme
Bkz: [Model değerlendirme](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/03_model_evaluation/ReadMe.md).

Paylaşılan görev değerlendirme betikten kullanırız [biyografisi NLP/NLPBA 2004 biyografisi varlık tanıma görevini](http://www.nactem.ac.uk/tsujii/GENIA/ERtask/report.html) duyarlık geri çağırma ve model F1 puanı değerlendirmek için. 

#### <a name="in-domain-versus-generic-word-embedding-models"></a>Etki alanı genel word modelleri katıştırma karşılaştırması

İki özellik türleri doğruluğunu arasında bir karşılaştırma şudur: (1) word eklerinin PubMed özetleri ve (2) word eklerinin eğitilmiş eğitilmiş Google haber üzerinde. Etki alanındaki modeli genel model sürümlerden üstünlüğü açıkça bakın. Bu nedenle genel bir kullanarak çok daha faydalıdır yerine model katıştırma belirli bir sözcük sahip. 

* Görev #1: İlaçlar ve Diseases algılama

![Model karşılaştırma 1](./media/scenario-tdsp-biomedical-recognition/mc1.png)

Biz benzer şekilde diğer veri kümeleri üzerinde word eklerinin değerlendirmesi gerçekleştirmek ve bu etki alanındaki her zaman daha iyi modeldir bakın.

* Görev #2: Proteins, hücre satır, hücre türü, DNA ve RNA algılama

![Model karşılaştırma 2](./media/scenario-tdsp-biomedical-recognition/mc2.png)

* Görev #3: Chemicals ve Diseases algılama

![Model karşılaştırma 3](./media/scenario-tdsp-biomedical-recognition/mc3.png)

* Görev #4: İlaçlar algılama

![Model karşılaştırma 4](./media/scenario-tdsp-biomedical-recognition/mc4.png)

* Görev #5: Genes algılama

![Model karşılaştırma 5](./media/scenario-tdsp-biomedical-recognition/mc5.png)

#### <a name="tensorflow-versus-cntk"></a>TensorFlow CNTK karşılaştırması
Bildirilen tüm model eğitilmiş Keras TensorFlow ile arka ucu olarak kullanma. "Bu iş yapıldığı anda ters" Keras CNTK arka ucu ile desteklemez. Bu nedenle, karşılaştırma amacıyla, biz bir tek yönlü LSTM modeli CNTK arka ucu ile eğitilmiş ve tek yönlü bir LSTM modeli TensorFlow arka ucu ile karşılaştırılan. Keras CNTK 2.0 Yükle [burada](https://docs.microsoft.com/cognitive-toolkit/using-cntk-with-keras). 

![Model karşılaştırma 6](./media/scenario-tdsp-biomedical-recognition/mc6.png)

CNTK olarak gerçekleştirir sonuçları dönem (CNTK ve Tensorflow 75 saniye 60 saniye) ve algılanan test varlıkların sayısı harcanan eğitim süre bakımından Tensorflow hem olarak iyi. Değerlendirme için tek yönlü Katmanlar kullanıyoruz.


### <a name="3-deployment"></a>3. Dağıtım

Bkz: [dağıtım](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/03_deployment).

Bir küme üzerinde bir web hizmeti dağıttığımız [Azure kapsayıcı Hizmeti'ni (ACS)](https://azure.microsoft.com/services/container-service/). Operationalization ortam Docker ve Kubernetes web hizmeti dağıtımını yönetmek için kümedeki sağlar. Daha fazla operationalization işlemi hakkında bilgi bulabilirsiniz [burada](model-management-service-deploy.md ).


## <a name="conclusion"></a>Sonuç

Nasıl Spark Word2Vec algoritmasını kullanarak bir word katıştırma modeli eğitmek ve sonra ayıklanan eklerinin varlık ayıklama derin sinir ağı eğitmek için özellikleri kullanmak ayrıntılarını üzerinden geçti. Biz eğitim ardışık düzen biomedical etki alanında uyguladınız. Ancak, ardışık düzen başka bir etki alanı özel varlık türlerini algılamak için uygulanacak genel bir çözümdür. Yeterli veri yeterlidir ve farklı bir etki alanı için Burada sunulan iş akışı kolayca uyarlayabilirsiniz.

## <a name="references"></a>Başvurular

* Tomas Mikolov, Kai Chen, Greg Corrado ve Gamze Deniz. 2013a. Word Beyanları vektör alanda verimli tahmini. İçinde bildirileri ICLR biri.
* Tomas Mikolov, Ilya Sutskever, Kai Chen, Greg S Corrado ve Jeff Deniz. 2013b. Sözcükler ve tümcecikleri ve bunların compositionality dağıtılmış gösterimlerini. NIPS bildirileri 3111 – 3119 sayfaları.
* Billy Chiu, Gamal Crichton, Gamze Korhonen ve Sampo Pyysalo. 2016. [Tren iyi Word eklerinin Biomedical NLP için nasıl](http://aclweb.org/anthology/W/W16/W16-2922.pdf), içinde bildirileri 15 Atölye Biomedical doğal dil işleme üzerinde sayfalar 166 – 174.
* [Vektör gösterimlerini sözcükler](https://www.tensorflow.org/tutorials/word2vec)
* [Yinelenen sinir ağları](https://www.tensorflow.org/tutorials/recurrent)
* [Spark ml Word2Vec karşılaşılan sorunları](https://intothedepthsofdataengineering.wordpress.com/2017/06/26/problems-encountered-with-spark-ml-word2vec/)
* [Spark Word2Vec: öğrenilen dersler](https://intothedepthsofdataengineering.wordpress.com/2017/06/26/spark-word2vec-lessons-learned/)

