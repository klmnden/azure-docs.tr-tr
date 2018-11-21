---
title: Biyomedikal varlık tanıma - Team Data Science Process - Azure Machine Learning | Microsoft Docs
description: Azure Machine Learning workbench'te biyomedikal varlık tanıma için ayrıntılı öğrenme kullanan Team Data Science Process Proje Hızlı.
services: machine-learning
documentationcenter: ''
author: deguhath
ms.author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 0d31fc0ecb06727aa44d31d832b0bfd5145b7c7d
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52262101"
---
# <a name="biomedical-entity-recognition-using-team-data-science-process-tdsp-template"></a>Team Data Science işlem (TDSP) şablonu kullanarak biyomedikal varlık tanıma

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



Varlık ayıklama, bilgi ayıklama bir alt görevi olan (diğer adıyla [adlandırılmış varlık tanıma (NER)](https://en.wikipedia.org/wiki/Named-entity_recognition), varlık Öbekleme ve varlık kimliği). Yapılandırılmamış metinden varlık ayıklama gibi karmaşık bir doğal dil işlemeyi (NLP) görev çözmek için Azure Machine Learning Workbench'i kullanma vurgulamak için bu gerçek dünya senaryoları amacı şöyledir:

1. Gömmeleri sinir word eğitmek için kullanma hakkında 18 milyon PubMed özetleri üzerinde bir metin gövde modeli nasıl [Spark Word2Vec uygulama](https://spark.apache.org/docs/latest/mllib-feature-extraction.html#word2vec).
2. Azure üzerinde derin uzun kısa vadeli bellek (LSTM) yinelenen sinir ağı model varlık ayıklama üzerinde bir GPU özellikli Azure veri bilimi sanal makinesi (GPU DS VM) için derleme yapma.
2. Bu etki alanına özgü word Gömmeleri modeli varlık tanıma göreve genel word Gömmeleri modelleri daha iyi performans gösterir gösterir. 
3. Eğitmek ve Azure Machine Learning Workbench'i kullanarak ayrıntılı öğrenme modelleri kullanıma hazır hale getirmek nasıl ekleyebileceğiniz gösterilmektedir.

4. Azure Machine Learning Workbench içinde aşağıdaki özellikleri gösterilmektedir:

    * Örneğinin [Team Data Science işlem (TDSP) yapısını ve şablonları](how-to-use-tdsp-in-azure-ml.md)
    * İndirme ve yükleme dahil olmak üzere proje bağımlılıklarınızı otomatik yönetimi
    * Farklı işlem ortamlarında Python betiklerini yürütme
    * Çalıştırma geçmişi Python betikleri için izleme
    * İşlem bağlamları HDInsight Spark 2.1 kümelerini kullanarak uzak Spark işleri yürütülmesi
    * Azure'da uzaktan GPU Vm'lerine işlerinin yürütülmesi
    * Derin öğrenme modelleri web hizmetleri üzerinde Azure kapsayıcı Hizmetleri (ACS) olarak kolay kullanıma hazır hale getirme

## <a name="use-case-overview"></a>Kullanım örneği genel bakış
Biyomedikal adlandırılmış varlık tanıma gibi karmaşık biyomedikal NLP görevler için kritik bir adım şöyledir: 
* Adlandırılmış varlık bahsetmeleri elektronik sağlık veya sistem durumu kayıtları gibi hastalıklara, dışı uyuşturucu madde, chemicals ve belirtileri ayıklanıyor.
* İlaç bulma
* Farklı varlık arasındaki etkileşimler anlama gibi ilaç ilaç etkileşim, ilaç Hastalık ilişki ve gene protein ilişki türleri.

Bizim kullanım örneği senaryosu nasıl büyük miktarda yapılandırılmamış veri topluluğunuza Medline PubMed Özet gibi bir sözcük model ekleme eğitmek için çözümlenebilir üzerinde odaklanır. Ardından çıktı Gömmeleri sinir varlık ayıklayıcı eğitmek için otomatik olarak oluşturulan özellikler kabul edilir.

Biyomedikal varlık ayıklama modeli eğitimi özelliklerini gömme etki alanına özgü sözcüğü genel özellik türüne eğitilmiş modelin daha iyi bizim sonuçları gösterir. Etki alanına özgü model doğru (dışında 9475) 7012 varlıkları 5274 varlıklara 0.61 genel modeli için F1 puanı ile karşılaştırıldığında 0.73 F1 puanı algılayabilir.

Aşağıdaki şekilde, verileri işlemek ve eğitme modelleri için kullanılan bir mimari gösterilmektedir.

![Mimari](./media/scenario-tdsp-biomedical-recognition/architecture.png)

## <a name="data-description"></a>Veri açıklaması

### <a name="1-word2vec-model-training-data"></a>1. Word2Vec model eğitim verileri
Ham MEDLINE soyut verilerden ilk indirilen [MEDLINE](https://www.nlm.nih.gov/pubs/factsheets/medline.html). Dosyaları XML biçiminde veriler genel kullanıma açıktır, [FTP sunucusu](https://ftp.ncbi.nlm.nih.gov/pubmed/baseline). Kullanılabilir 892 XML dosyaları sunucuda ve her biri XML dosyalarını 30.000 makale bilgiler içerir. Veri toplama adımı hakkında daha fazla ayrıntı Proje yapısı bölümünde sağlanır. Her dosyada mevcut alanlar 
        
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

Sinir varlık ayıklama modeli, eğitim ve genel kullanıma açık veri kümeleri üzerinde değerlendirilir. Bu veri kümeleri hakkında ayrıntılı bir açıklama almak için aşağıdaki kaynaklarla başvurabileceğiniz:
 * [Biyografi varlık tanıma BioNLP/NLPBA 2004 görevi](http://www.nactem.ac.uk/tsujii/GENIA/ERtask/report.html)
 * [BioCreative V CDR görev gövde](http://www.biocreative.org/tasks/biocreative-v/track-3-cdr/)
 * [Semeval 2013 - görev 9.1 (ilaç tanıma)](https://www.cs.york.ac.uk/semeval-2013/task9/)

## <a name="link-to-the-azure-gallery-github-repository"></a>Azure Galerisi GitHub deponuza bağlayın
Aşağıdaki kodu içeren gerçek senaryosunun ortak GitHub deposu bağlantısı olan ve daha ayrıntılı açıklaması: 

[https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction)


## <a name="prerequisites"></a>Önkoşullar 

* Bir Azure [abonelik](https://azure.microsoft.com/free/)
* Azure Machine Learning Workbench'i. Bkz: [Yükleme Kılavuzu](quickstart-installation.md). Şu anda Azure Machine Learning Workbench yalnızca şu işletim sistemlerine yüklenebilir: 
    * Windows 10 veya Windows Server 2016
    * macOS Sierra (veya daha yeni)


### <a name="azure-services"></a>Azure hizmetleri
* [HDInsight Spark kümesi](https://docs.microsoft.com/azure/hdinsight/hdinsight-apache-spark-jupyter-spark-sql) sürüm Spark 2.1 (HDI 3.6) Linux'ta genişleme hesaplama için. Aşağıda açıklanan MEDLINE özetleri kredisinin tamamı işlemek için en düşük yapılandırmasını gerekir: 
    * Baş düğüm: [D13_V2](https://azure.microsoft.com/pricing/details/hdinsight/) boyutu
    * Çalışan düğümlerinin: en az 4 [D12_V2](https://azure.microsoft.com/pricing/details/hdinsight/). Bizim işlerinde D12_V2 boyutu 11 çalışan düğümü kullandık.
* [NC6 veri bilimi sanal makinesi (DSVM)](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-linux-dsvm-intro) ölçek büyütme hesaplama için.

### <a name="python-packages"></a>Python paketleri

Tüm gerekli bağımlılıkları senaryo proje klasörü altında aml_config/conda_dependencies.yml dosyasında tanımlanır. Bu dosyada tanımlanan bağımlılıklar çalıştırmaları için otomatik olarak sağlanan karşı docker, VM ve HDI küme hedefler. Conda ortam dosyası biçimi hakkında daha fazla ayrıntı için başvurmak [burada](https://conda.io/docs/using/envs.html#create-environment-file-by-hand).

* [TensorFlow](https://www.tensorflow.org/install/)
* [CNTK 2.0](https://docs.microsoft.com/cognitive-toolkit/using-cntk-with-keras)
* [Keras](https://keras.io/#installation)
* NLTK
* Fastparquet

### <a name="basic-instructions-for-azure-machine-learning-aml-workbench"></a>Azure Machine Learning (AML) workbench'in temel yönergeler
* [Genel Bakış](../service/overview-what-is-azure-ml.md)
* [Yükleme](quickstart-installation.md)
* [TDSP kullanma](how-to-use-tdsp-in-azure-ml.md)
* [Nasıl dosyalarını okuma ve yazma](how-to-read-write-files.md)
* [Jupyter not defterlerini kullanma](how-to-use-jupyter-notebooks.md)
* [GPU kullanma](how-to-use-gpu.md)

## <a name="scenario-structure"></a>Senaryo yapısı
Senaryo için kullanıyoruz, takip eden TDSP proje yapısını ve belgelerini şablonları (Şekil 1) [TDSP yaşam döngüsü](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/lifecycle-detail.md). Proje sağlanan yönergeler göre oluşturulan [burada](./how-to-use-tdsp-in-azure-ml.md).


![Proje bilgileri doldurun](./media/scenario-tdsp-biomedical-recognition/instantiation-3.png) 

Adım adım veri bilimi iş akışı aşağıdaki gibidir:

### <a name="1-data-acquisition-and-understanding"></a>1. Veri edinme ve anlama

Bkz: [veri edinme ve anlama](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/blob/master/code/01_data_acquisition_and_understanding/ReadMe.md).

Ham MEDLINE topluluğunuza 27 milyon özetleri toplamda yaklaşık 10 milyon makaleleri soyut boş alan sahip olduğu sahiptir. Azure HDInsight Spark tek bir makinenin belleğe yüklenemiyor büyük verileri işlemek için kullanılan bir [Pandas DataFrame](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html). İlk olarak, verileri Spark kümesine yüklenir. Aşağıdaki adımlar üzerinde yürütülen sonra [Spark DataFrame](https://spark.apache.org/docs/latest/sql-programming-guide.html): 
* Medline XML ayrıştırıcısını kullanarak XML dosyalarını ayrıştırma
* soyut metni tümce bölme, simgeleştirme ve büyük/küçük harf normalleştirmesi dahil olmak üzere önceden işlenir.
* Burada soyut alanı boş veya kısa metin olan makaleler Dışla 
* sözcük dilbilgisi eğitim Özet oluşturma
* sinir model ekleme word eğitin. Daha fazla bilgi için [GitHub kodu bağlantısı](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/blob/master/code/01_data_acquisition_and_understanding/ReadMe.md) kullanmaya başlamak için.


XML dosyaları ayrıştırma sonra veri biçimi aşağıdaki gibidir: 

![Veri örneği](./media/scenario-tdsp-biomedical-recognition/datasample.png)

Sinir varlık ayıklama modeli, eğitim ve genel kullanıma açık veri kümeleri üzerinde değerlendirilir. Bu veri kümeleri hakkında ayrıntılı bir açıklama almak için aşağıdaki kaynaklarla başvurabileceğiniz:
 * [Biyografi varlık tanıma BioNLP/NLPBA 2004 görevi](http://www.nactem.ac.uk/tsujii/GENIA/ERtask/report.html)
 * [BioCreative V CDR görev gövde](http://www.biocreative.org/tasks/biocreative-v/track-3-cdr/)
 * [Semeval 2013 - görev 9.1 (ilaç tanıma)](https://www.cs.york.ac.uk/semeval-2013/task9/)

### <a name="2-modeling"></a>2. Modelleme

Bkz: [modelleme](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling).

Model oluşturma, modeli katıştırma kendi sözcük eğitimi için önceki bölümde indirilen verileri kullanarak ve diğer görevleri için kullanmak nasıl burada göstereceğiz aşamadır. PubMed verileri kullanıyoruz ancak Gömmeleri oluşturmak için işlem hattı geneldir ve diğer etki alanı için word Gömmeleri eğitmek için yeniden kullanılabilir. Verileri doğru bir temsilini olacak şekilde katıştırma için word2vec büyük miktarda veri eğitildi gereklidir.
Biz word Gömmeleri hazır olduktan sonra biz öğrenilen Gömmeleri katıştırma Katmanı'nı başlatmak için kullandığı bir derin sinir ağı modeli eğitebilirsiniz. Biz katıştırma katmanı trainable olmayan olarak işaretleyin, ancak zorunlu değildir. Model ekleme word'ün eğitim Denetimsiz ve bu nedenle etiketlenmemiş metinleri yararlanmak yükümlü. Ancak, varlık tanıma modeli eğitimi denetimli öğrenmede bir görevdir ve doğruluğunun tutar ve el ile açıklanan veri kalitesini bağlıdır. 


#### <a name="21-feature-generation"></a>2.1. Özellik oluşturma

Bkz: [özellik nesil](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/01_feature_engineering).

Word2Vec etiketlenmemiş eğitim topluluğunuza sinir ağı modelden eğitir Denetimsiz öğrenme algoritmasını katıştırma sözcüktür. Bu, her bir sözcüğün anlam bilgilerini temsil eden bir yapı için sürekli bir vektör üretir. Bu modeller basit sinir ağları ile tek bir gizli katman var. Word vektörleri/Gömmeleri yayılımı ve stokastik aşama tarafından öğrenilen. İki tür word2vec modelleri, yani, Skip-Gram ve sürekli-paketi--sözcük vardır (denetleyin [burada](https://arxiv.org/pdf/1301.3781.pdf) daha fazla ayrıntı için). Skip-gram modelini destekler, word2vec MLlib'ın uygulaması kullandığımızdan kısaca, burada açıklanmaktadır (alınan görüntü [bağlantı](https://ahmedhanibrahim.wordpress.com/2017/04/25/thesis-tutorials-i-understanding-word2vec-for-word-embedding-i/)): 

![Skip Gram modeli](./media/scenario-tdsp-biomedical-recognition/skip-gram.png)

Performansı iyileştirmek için hiyerarşik Softmax ve negatif örnekleme modelini kullanır. Hiyerarşik SoftMax (SoftMax H) bir ikili ağaçlara göre ilham değeridir. H SoftMax ayrıldığında sözcükleri hiyerarşik bir katmanla temelde düz SoftMax katman yerini alır. Bu, bize pahalı normalleştirme tüm sözcükleri hesaplamak zorunda kalmaktan kurtarır olasılık hesaplamaları dizisini bir sözcük olasılığını hesaplama parçalayın olanak tanır. Log2 derinliğini dengeli bir ikili ağacı sahip olduğundan (| V |) (V olduğundan kelime), yalnızca en fazla log2 değerlendirmek ihtiyacımız (| V |) bir sözcük son olasılığını almak için düğümleri. Kendi bağlam c verilen sözcük w olasılığını sonra yalnızca doğru alınmasına olasılıklar ürünüdür ve sol sırasıyla yaprak düğümü yol açan kapatır. Biz daha sık sözcükleri daha kısa gösterimleri aldığından emin olmak için veri kümesinde bir kelimelerin sıklığına göre Huffman ağacı oluşturabilirsiniz. Daha fazla bilgi için [bu bağlantıyı](http://ruder.io/word-embeddings-softmax/).
Alınan görüntü [burada](https://ahmedhanibrahim.wordpress.com/2017/04/25/thesis-tutorials-i-understanding-word2vec-for-word-embedding-i/).

##### <a name="visualization"></a>Görselleştirme

Word Gömmeleri biz oluşturduktan sonra bunları görselleştirin ve anlamsal olarak benzer sözcükler arasındaki ilişkiyi görmek istiyoruz. 

![W2V benzerlik](./media/scenario-tdsp-biomedical-recognition/w2v-sim.png)

Biz Gömmeleri görselleştirme iki farklı yollar gösterilmiştir. İlki bir PCA yüksek boyutlu vektör 2B vektör alanına proje için kullanır. Bu bir önemli bilgi kaybına yol açar ve görselleştirme doğru değil. İkinci PCA ile kullanmaktır [t SNE](https://distill.pub/2016/misread-tsne/). t-SNE içinde bir dağılım grafiğinde noktalara görselleştirilebilir, iki veya üç boyutlu bir alana yüksek boyutlu veri eklemek için uygun olan bir doğrusal işlenemez azaltma tekniğidir.  Yüksek boyutlu her nesne bir iki veya three dimensional noktası tarafından benzer nesneler yakındaki noktaları tarafından modellenir ve benzer olmayan nesneler uzak noktaları tarafından modellenir şekilde modeller. Bu iki parça halinde çalışır. İlk olarak, benzer nesneler çekilen bir yüksek olasılığı olan bir şekilde daha büyük boyutlu uzayda çiftlerinde üzerine bir olasılık dağılımının oluşturur ve teslim olasılığı düşük benzemez noktalarına sahip. İkinci olarak, benzer bir olasılık dağıtım noktalarının düşük boyutlu bir eşlem içindeki üzerine tanımlar ve KL farklılıklara göre harita üzerinde noktaları konumunu iki dağıtım arasında en aza indirir. Düşük boyutundaki noktaları konumunu gradyan düşüşü kullanarak KL Geçitler en aza indirerek elde edilir. Ancak, t-SNE her zaman güvenilir olmayabilir. Uygulama Ayrıntıları bulunabilir [burada](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/01_feature_engineering). 


Aşağıdaki resimde gösterildiği gibi t-SNE görselleştirme daha fazla sözcük vektörler ve olası kümeleme desenleri ayrımı sağlar. 


* PCA Görselleştirme

![PCA](./media/scenario-tdsp-biomedical-recognition/pca.png)

* T-SNE Görselleştirme

![t-SNE](./media/scenario-tdsp-biomedical-recognition/tsne.png)

* "Kanser için" en yakın noktaları (bunlar Kanser, tüm alt türleri olan)

![Noktaları Kanser için en yakın](./media/scenario-tdsp-biomedical-recognition/nearesttocancer.png)

#### <a name="22-train-the-neural-entity-extractor"></a>2.2. Sinir varlık ayıklayıcı eğitin

Bkz: [sinir varlık ayıklayıcı eğitme](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/02_model_creation/ReadMe.md).

Akış iletme sinir ağı mimarisi, her bir giriş kabul et ve diğer girişler ve çıkışlar bağımsız olarak çıktı sorunu düşer. Bu mimari, makine çevirisi ve varlık ayıklama gibi etiketleme görevler dizisi dizisi modeli olamaz. Yinelenen sinir ağı modelleri, artık kadar sonraki düğümü için hesaplanan bilgi geçirirken bu sorunu çözmek. Bu özellik, daha önce hesaplanan bilgileri aşağıdaki şekilde gösterildiği gibi kullanmanız mümkün olduğundan ağdaki belleğe sahip çağrılır:

![RNN](./media/scenario-tdsp-biomedical-recognition/rnn-expanded.png)

Temel alınan Rnn'ler gerçekten etkilese [kaybolmasını gradyan sorun](https://en.wikipedia.org/wiki/Vanishing_gradient_problem) nedeniyle, bunlar bunlar görülen önce tüm bilgileri kullanmak mümkün değildir. Bağlam, büyük bir miktarını tahminde bulunmak için yalnızca gerekli olduğunda sorunu yetkisiz değiştirmeye karşı korumalı hale gelir. Ancak modelleri LSTM gibi böyle bir sorunu karşılaşmaz, uzun vadeli bağımlılıkları hatırlamak aslında tasarlanmıştır. Tek bir sinir ağı sahip Rnn'ler vanilla Lstm'ler her hücre için dört sinir ağları arasındaki etkileşimler vardır. LSTM nasıl çalıştığı hakkında ayrıntılı bir açıklama için başvurmak [yazıya](http://colah.github.io/posts/2015-08-Understanding-LSTMs/).

![LSTM hücresi](./media/scenario-tdsp-biomedical-recognition/lstm-cell.png)

Şimdi kendi LSTM tabanlı yinelenen sinir ağı bir araya deneyin ve ilaç ve hastalık belirti Bahsetme gibi varlık türleri PubMed verileri ayıklamayı deneyin. İlk adım etiketli verileri büyük miktarda elde edilir ve tahmin gibi bu kolay değil! Tıbbi verilerden en iyi şekilde birçok kişi hakkında hassas bilgi içeriyor ve bu nedenle genel kullanıma açık değildir. Biz genel kullanıma açık olan iki farklı veri kümeleri üzerinde bileşimini kullanır. İlk veri kümesini Semeval 2013 - görev 9.1 (ilaç tanıma) ve diğer BioCreative V CDR görevden. Biz birleştiriyorsanız ve otomatik olarak bu iki veri kümesi etiketleme, böylece biz ilaçları hem hastalıklara gelen tıbbi metinleri algılayın ve bizim word Gömmeleri değerlendirin. Uygulama ayrıntılarını başvurmak [GitHub kodu bağlantısı](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/02_model_creation).

Tüm kodlar ve karşılaştırma için kullandığımız modeli mimarisi, aşağıda sunulmuştur. Farklı veri kümeleri için değişiklikleri en büyük sıra uzunluğu (burada 613) parametredir.

![LSTM modeli](./media/scenario-tdsp-biomedical-recognition/d-a-d-model.png)

#### <a name="23-model-evaluation"></a>2.3. Modeli değerlendirme
Bkz: [Model değerlendirme](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/02_modeling/03_model_evaluation/ReadMe.md).

Paylaşılan görev değerlendirme betikten kullandığımız [biyografi NLP/NLPBA 2004 biyografi varlık tanıma görevi](http://www.nactem.ac.uk/tsujii/GENIA/ERtask/report.html) duyarlılık, geri çağırma ve F1 puan modeli değerlendirilecek. 

#### <a name="in-domain-versus-generic-word-embedding-models"></a>Etki alanı modelleri ekleme genel word karşılaştırması

İki özellik türleri doğruluğunu arasında bir karşılaştırma şudur: (1) word Gömmeleri PubMed özetleri ve (2) word Gömmeleri eğitilmiş Google News eğitim. Etki alanındaki model genel model daha iyi açıkça bakın. Bu nedenle belirli bir sözcüğün modeli kullanarak genel bir tane daha faydalıdır yerine ekleme sahip. 

* #1. Görev: İlaçları ve hastalıklara algılama

![1 modeli karşılaştırması](./media/scenario-tdsp-biomedical-recognition/mc1.png)

Biz benzer şekilde diğer veri kümelerinde word Gömmeleri değerlendirmesi gerçekleştirmek ve bu etki alanındaki modeli her zaman daha iyi olduğunu görün.

* #2. Görev: Geliyor, hücre satırını, hücresi türü, DNA ve RNA algılama

![2 modeli karşılaştırması](./media/scenario-tdsp-biomedical-recognition/mc2.png)

* #3. Görev: Chemicals ve hastalıklara algılama

![3 modeli karşılaştırması](./media/scenario-tdsp-biomedical-recognition/mc3.png)

* #4. Görev: İlaçları algılama

![4 modeli karşılaştırması](./media/scenario-tdsp-biomedical-recognition/mc4.png)

* #5. Görev: Genes algılama

![5 modeli karşılaştırması](./media/scenario-tdsp-biomedical-recognition/mc5.png)

#### <a name="tensorflow-versus-cntk"></a>TensorFlow, CNTK karşılaştırması
Keras TensorFlow ile kullanarak bir arka uç bildirilen tüm modelleri eğitilir. Keras CNTK arka ucu ile "Bu iş yapıldığı anda ters" desteklemez. Bu nedenle, karşılaştırma için biz sahip CNTK arka ucu ile tek yönlü bir LSTM modeli eğitilir ve tek yönlü bir LSTM modeli TensorFlow arka ucu ile karşılaştırılan. Keras için CNTK 2. 0'ı yükleme [burada](https://docs.microsoft.com/cognitive-toolkit/using-cntk-with-keras). 

![6 modeli karşılaştırması](./media/scenario-tdsp-biomedical-recognition/mc6.png)

CNTK olarak gerçekleştiren tamamlanmış dönem (CNTK ve Tensorflow 75 saniye 60 saniye) ve algılanan test varlıkların sayısı geçen eğitim süre açısından her iki Tensorflow olarak iyi. Değerlendirme için tek yönlü katmanları kullanıyoruz.


### <a name="3-deployment"></a>3. Dağıtım

Bkz: [dağıtım](https://github.com/Azure/MachineLearningSamples-BiomedicalEntityExtraction/tree/master/code/03_deployment).

Biz bir web hizmeti bir kümede dağıtılan [Azure Container Service (ACS)](https://azure.microsoft.com/services/container-service/). Kullanıma hazır hale getirme ortamı, Docker ve Kubernetes kümesini web hizmeti dağıtımı Yönet sağlar. Daha fazla kullanıma hazır hale getirme işlemi hakkında bilgi bulabilirsiniz [burada](model-management-service-deploy.md ).


## <a name="conclusion"></a>Sonuç

Üzerinde Spark Word2Vec algoritması kullanılarak bir sözcük ekleme modeli eğitmek ve sonra varlık ayıklama derin sinir ağı eğitmek için özellikleri ayıklanan Gömmeleri kullanmak ayrıntılarını üzerinden geçti. Eğitim işlem hattı biyomedikal etki uyguladığımız. Ancak, işlem hattının herhangi bir etki alanının özel varlık türlerini algılamak için uygulanacak genel olur. Yeterli veri yeterlidir ve farklı bir etki alanı için Burada sunulan iş akışı kolayca uyarlayabilirsiniz.

## <a name="references"></a>Başvurular

* Tomas Mikolov, Kai Chen, Greg Corrado ve Jeffrey Deniz. 2013a. Word gösterimleri vektör alanında verimli tahmini. İçinde daha fazla Study of ICLR.
* Tomas Mikolov ve Ilya Sutskever, Kai Chen, Greg S Corrado ve Jeff Deniz. 2013b. Sözcük ve tümcecikleri ve bunların compositionality dağıtılmış temsillerini. NIPS Study 3111 – 3119 sayfaları.
* Billy Chiu, Gamal Crichton, Anna'nın Korhonen ve Sampo Pyysalo. 2016. [Biyomedikal NLP eğitme iyi Word katıştırma için nasıl](http://aclweb.org/anthology/W/W16/W16-2922.pdf), içinde Study, on beşinci Atölyesi Biyomedikal doğal dil işleme üzerinde 166 – 174 sayfaları.
* [Vektör temsillerini sözcükler](https://www.tensorflow.org/tutorials/word2vec)
* [Yinelenen sinir ağları](https://www.tensorflow.org/tutorials/recurrent)
* [Spark ml Word2Vec ile karşılaştığınız sorunları](https://intothedepthsofdataengineering.wordpress.com/2017/06/26/problems-encountered-with-spark-ml-word2vec/)
* [Spark Word2Vec: öğrenilen dersler](https://intothedepthsofdataengineering.wordpress.com/2017/06/26/spark-word2vec-lessons-learned/)

