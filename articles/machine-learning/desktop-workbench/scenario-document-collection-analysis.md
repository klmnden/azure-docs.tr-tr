---
title: Belge koleksiyonu çözümleme - Azure | Microsoft Docs
description: Özetleme ve büyük bir belge, tümcecik öğrenme konu modelleme ve Azure ML Workbench kullanarak konu model analizi gibi teknikler koleksiyonu analiz etme.
services: machine-learning
author: kehuan
ms.author: kehuan
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, MicrosoftDocs/mlreview, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/20/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 762955103aeb48eb8a9b62f4e3ffe193bba71a38
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46947226"
---
# <a name="document-collection-analysis"></a>Belge koleksiyonu çözümleme

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 



Bu senaryo özetlemek ve büyük bir belge, tümcecik öğrenme konu modelleme ve Azure ML Workbench kullanarak konu model analizi gibi teknikler koleksiyonu analiz etme gösterir. Azure Machine Learning Workbench için çok büyük bir belge koleksiyonu için kolay ölçek sağlar ve eğitme ve işlem bağlamı, yerel işlem için veri bilimi sanal makineleri için Spark kümesi arasında çeşitli Modellerinizi ayarlamak için bir mekanizma sağlar. Kolay geliştirme, Azure Machine Learning Workbench içinde Jupyter Notebook aracılığıyla sağlanır.

## <a name="link-to-the-gallery-github-repository"></a>Galeri GitHub deponuza bağlayın

Bu örnek için gerekli, kod örnekleri dahil olmak üzere tüm malzemeler, bu gerçek dünya senaryosu için ortak GitHub deposu içerir:

[https://github.com/Azure/MachineLearningSamples-DocumentCollectionAnalysis](https://github.com/Azure/MachineLearningSamples-DocumentCollectionAnalysis)

## <a name="overview"></a>Genel Bakış

Her gün toplanan büyük miktarda veri (özellikle yapılandırılmamış metin verilerini) ile önemli düzenlemek, arama ve bu metinleri miktardaki anlamak zordur. Bu belge koleksiyonu çözümleme senaryo büyük belge koleksiyonu çözümleme ve aşağı akış NLP görevlerini etkinleştirmek için bir verimli ve otomatik uçtan uca iş akışı gösterilmektedir.

Bu senaryo tarafından sunulan temel öğeleri şunlardır:

1. Belgelerden dikkat çekici çok sözcük tümcecik öğrenme.

1. Belge koleksiyonundaki sunulan temel konuları bulunuyor.

1. Belgeleri güncel dağıtım tarafından temsil eder.

1. Düzenleme, arama ve belgeleri güncel içeriğe göre özetlemek için yöntemleri sunmak.

Bu senaryoda sunulan yöntemleri kritik endüstriyel gibi iş yüklerini bulma konu eğilimleri anomali ve belge koleksiyonu özetleme benzer belge arama işlemi çeşitli sağlayabilir. Birçok farklı türdeki kamu Yasama, haberleri, ürün incelemeleri, müşteri geri bildirimler ve bilimsel araştırmaları makaleleri gibi belge analizi için uygulanabilir.

Makine öğrenme teknikleri/Bu senaryoda kullanılan algoritmalar şunlardır:

1. Metin işleme ve temizleme

1. Tümcecik öğrenme

1. Konu modelleme

1. Gövde özetleme

1. Güncel eğilimleri ve anomali algılama

## <a name="prerequisites"></a>Önkoşullar

Bu örneği çalıştırmak için gereken önkoşullar aşağıdaki gibidir:

* Düzgün bir şekilde yüklediğinizden emin olun [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) izleyerek [yükleme ve oluşturma Hızlı Başlangıç](quickstart-installation.md).

* Bu örnek, her işlem bağlam üzerinde çalıştırılabilir. Ancak, çalıştırmak için çok çekirdekli makine ile en az önerilir 16GB bellek ve 5GB disk alanı.

## <a name="create-a-new-workbench-project"></a>Workbench yeni bir proje oluşturun

Bu örnekte, şablon olarak kullanarak yeni bir proje oluşturun:
1.  Açık bir Azure Machine Learning Workbench'i
2.  Üzerinde **projeleri** sayfasında **+** açıp seçmek **yeni proje**
3.  İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri girin
4.  İçinde **proje şablonlarında Ara** arama kutusuna "Belge koleksiyonu Çözümleme" yazın ve şablonu seçin
5.  **Oluştur**'a tıklayın

## <a name="data-description"></a>Veri açıklaması

Bu senaryoda kullanılan veri kümesinin metin özetleri ve BİZE Kongre tarafından gerçekleştirilen her bir devletin eylemi ilişkili meta veriler içerir. Verilerin toplandığı [GovTrack.us](https://www.govtrack.us/), Amerika Birleşik Devletleri Kongre etkinliklerini izler ve katılmak, Ulusal devletin işlemde tan yardımcı olur. Toplu veri aracılığıyla indirilebilir [bu bağlantıyı](https://www.govtrack.us/data/congress/) bu senaryoya dahil edilmeyen el ile bir betik kullanarak. Nasıl veri yükleneceği ile ilgili ayrıntıları bulunamadı [GovTrack API belgeleri](https://www.govtrack.us/developers).

### <a name="data-source"></a>Veri kaynağı

Bu senaryoda, toplanan ham verileri BİZE Kongre tarafından (önerilen fatura ve çözümleri) 1973 tarihli Haziran 2017'ye sunulan devletin eylemleri dizisidir (93rd 115th Congresses için). Toplanan verileri JSON biçimindedir ve zengin devletin eylemler hakkında bilgi içerir. Başvurmak [bu GitHub bağlantı](https://github.com/unitedstates/congress/wiki/bills) veri alanları ayrıntılı bir açıklaması için. Bu senaryo içinde Tanıtım amaçlı için yalnızca bir alt veri alanlarının ayıklanan JSON dosyalarından. Önceden derlenmiş TSV dosyası `CongressionalDataAll_Jun_2017.tsv` devletin bu eylemlerin kayıtları içeren bu senaryoda sağlanır. TSV dosyası otomatik olarak not defterleri ile indirilebilir `1_Preprocess_Text.ipynb` Not Defteri klasörü altında veya `preprocessText.py` Python paketi olarak.

### <a name="data-structure"></a>Veri yapısı

Veri dosyasında dokuz veri alanları vardır. Veri alanı adları ve açıklamaları aşağıda listelenmiştir.

| Alan Adı | Tür | Açıklama | Eksik değer içermesi |
|------------|------|-------------|---------------|
| `ID` | Dize | Fatura/çözüm kimliği. Bu alan [bill_type] [sayı] biçimidir-[Kongre]. Örneğin, "93 hconres1" fatura türü "hconres" anlamına gelir (temsil eden ev eşzamanlı çözümlemesi için bkz [bu belgeyi](https://github.com/unitedstates/congress/wiki/bills#basic-information)), bill numarası ' 1'dir 've ' 93 Kongre sayıdır'. | Hayır |
| `Text` | Dize | Fatura/çözümleme içeriği. | Hayır |
| `Date` | Dize | Fatura/çözüm Başlangıçta önerilen tarih. Bir 'yyyy-aa-gg' biçiminde. | Hayır |
| `SponsorName` | Dize | Fatura/çözüm önerilen birincil sponsoru adı. | Evet |
| `Type` | Dize | Birincil başlık türünü toplanma, 'Temsilcisi' (temsilcisi) ya da 'Gönder' (senator). | Evet |
| `State` | Dize | Birincil sponsor durumu. | Evet |
| `District` | Tamsayı | Birincil sponsor sponsoru başlığının bir temsilcisi ise bölge sayısı. | Evet |
| `Party` | Dize | Birincil sponsor taraf. | Evet |
| `Subjects` | Dize | Üst üste fatura kitaplığı Kongre tarafından eklenen konu koşulları. Terimleri virgülle bitiştirilir. Bu koşulları Kongre kitaplığı içinde bir insan tarafından yazılmış ve fatura bilgileri ilk yayımlandığında genellikle mevcut değildir. Herhangi bir zamanda eklenebilir. Bu nedenle fatura ömrünün sonunda bazı konu artık uygun olmayabilir. | Evet |

## <a name="scenario-structure"></a>Senaryo yapısı

Belge koleksiyonu çözümleme örnek teslim edilebilirleri iki tür düzenlenmiştir. İlk tür, Ipython iş akışının tamamını adım adım açıklamaları Göster not defterlerini dizisidir. İkinci bir Python paketi yanı sıra bu paketin nasıl kullanılacağı bazı kod örnekleri türüdür. Birçok kullanım durumlarına göre genel Python paketidir.

Bu örnekte dosyalar gibi düzenlenir.

| Dosya Adı | Tür | Açıklama |
|-----------|------|-------------|
| `aml_config` | Klasör | Azure Machine Learning Workbench yapılandırma klasörü başvurmak [bu belgeleri](./experimentation-service-configuration-reference.md) ayrıntılı deneme yürütme yapılandırması |
| `Code` | Klasör | Python betiği ve Python paketini kaydetmek için kullanılan kod klasörü |
| `Data` | Klasör | Ara dosyaları kaydetmek için kullanılan veri klasörü |
| `notebooks` | Klasör | Jupyter not defterleri klasörü |
| `Code/documentAnalysis/__init__.py` | Soubor Pythonu | Python paket init dosyası |
| `Code/documentAnalysis/configs.py` | Soubor Pythonu | Yapılandırma dosyası Python paketi, önceden tanımlanmış sabitleri de dahil olmak üzere belge analizi tarafından kullanılır. |
| `Code/documentAnalysis/preprocessText.py` | Soubor Pythonu | Aşağı Akış görevlerinde için verileri ön işleme için kullanılan bir Python dosyası |
| `Code/documentAnalysis/phraseLearning.py` | Soubor Pythonu | İfadeleri verilerden bilgi edinin ve ham verileri dönüştürmek için kullanılan bir Python dosyası |
| `Code/documentAnalysis/topicModeling.py` | Soubor Pythonu | Görünmeyen Dirichlet ayırma (LDA) konu modeli eğitmek için kullanılan bir Python dosyası |
| `Code/step1.py` | Soubor Pythonu | Adım 1 / belge koleksiyonu çözümleme: metin ön işleme |
| `Code/step2.py` | Soubor Pythonu | Adım 2 / belge koleksiyonu çözümleme: tümcecik öğrenme |
| `Code/step3.py` | Soubor Pythonu | Adım 3 / belge koleksiyonu çözümleme: eğitme ve değerlendirme LDA konu modeli |
| `Code/runme.py` | Soubor Pythonu | Örnek bir dosya içinde tüm adımları çalıştırın |
| `notebooks/1_Preprocess_Text.ipynb` | Ipython Notebook | Ham verileri dönüştürme ve metin ön işleme |
| `notebooks/2_Phrase_Learning.ipynb` | Ipython Notebook | (Sonra veri dönüştürme) ifadeleri metin verilerden bilgi edinin |
| `notebooks/3_Topic_Model_Training.ipynb` | Ipython Notebook | LDA konu modeli eğitme |
| `notebooks/4_Topic_Model_Summarization.ipynb` | Ipython Notebook | Eğitilen bir LDA konu modelini temel belge koleksiyonu içeriğini özetleme |
| `notebooks/5_Topic_Model_Analysis.ipynb` | Ipython Notebook | Metin belgeleri koleksiyonunu tasarlamayla içeriğini çözümleyebilir ve güncel bilgiler belge koleksiyonu ile ilişkili diğer meta verileri ilişkilendirin |
| `notebooks/6_Interactive_Visualization.ipynb` | Ipython Notebook | Öğrenilen konu modelinin etkileşimli Görselleştirme |
| `notebooks/winprocess.py` | Soubor Pythonu | Dizüstü bilgisayarlar tarafından kullanılan işlemci için python betiği |
| `README.md` | Markdown dosyası | Benioku markdown dosyası |

### <a name="data-ingestion-and-transformation"></a>Veri alımı ve dönüştürme

Derlenmiş dataset `CongressionalDataAll_Jun_2017.tsv` Blob depolama alanına kaydedilir ve erişilebilir olduğundan hem not defterlerini ve Python komut dosyaları. Veri alımı ve dönüştürme için iki adımı vardır: metin verileri ön işleme ve tümcecik öğrenme.

#### <a name="preprocess-text-data"></a>Metin verileri ön işleme

Ön işleme adımı, doğal dil işleme tekniklerini temizlemek ve ham metin verilerini hazırlamak için geçerlidir. Görünmeyen konu modelleme ve tümcecik Denetimsiz öğrenme için bir precursor görür. Ayrıntılı adım adım açıklama not defterinde bulunabilir `1_Preprocess_Text.ipynb`. Python betiğini de mevcuttur `step1.py` bu not defterini karşılık gelir.

Bu adımda, TSV veri dosyasını Azure Blob depolama alanından indirilir ve bir Pandas DataFrame içeri aktarıldı. Her bir satır veri çerçevesi öğesinde tek cohesive uzun metin veya bir 'document' dizesidir. Her belge ardından cümle, ifadeler veya subphrases olasılığı öbeklere metin ayrılır. Bölme arası cümle veya çapraz tümcecik sözcük dizelerini tümcecikleri öğrenme kullandığınızda işlemin öğrenme tümcecik engellemek için tasarlanmıştır.

Not Defteri ve Python paketini ön işleme adımı için tanımlanan birden çok işlev vardır. İşin çoğunu, bu iki satırı kodlarının çağırarak gerçekleştirilebilir.

```python
# Read raw data into a Pandas DataFrame
textDF = getData()

# Write data frame with preprocessed text out to TSV file
cleanedDataFrame = CleanAndSplitText(textDF, saveDF=True)
```

#### <a name="phrase-learning"></a>Tümcecik öğrenme

Tümcecik öğrenme adımı, anahtar ifadeleri büyük bir belge koleksiyonu arasında öğrenmek için temel bir çerçeve uygular. Başlıklı yazının açıklanan "[kısıtlı tümcecikleri ağaç geliştirilmiş konu modelleme, konuşma konuşma Multiword tümcecikleri modelleme](http://people.csail.mit.edu/hazen/publications/Hazen-SLT-2012.pdf)", hangi ilk kısmında sunulmuştur konuşulan dil 2012 IEEE Atölyesi Teknoloji. Tümcecik öğrenme adımın ayrıntılı uygulama Ipython Notebook gösterilen `2_Phrase_Learning.ipynb` ve Python betiğini `step2.py`.

Bu adım, temizlenmiş metin giriş olarak alır ve büyük bir belge koleksiyonu mevcut en dikkat çekici tümcecikleri öğrenir. Tümcecik öğrenme üç görevlere ayrılabilir, yinelemeli bir işlemdir: n-gram sayısı, rank ağırlıklı Pointwise karşılıklı bilgilere göre bağlı bir kelimelerin olası ifadeleri ve metin deyimi yeniden yazın. Belirtilen ifadeler öğrenilen kadar bu üç görevin birden çok yinelemelerde sırayla yürütülür.

Belge analizi Python paket Python sınıfı içinde `PhraseLearner` tanımlanan `phraseLearning.py` dosya. İfadeleri öğrenmek için kullanılan kod parçacığı aşağıdadır.

```python
# Instantiate a PhraseLearner and run a configuration
phraseLearner = PhraseLearner(cleanedDataFrame, "CleanedText", numPhrase,
                        maxPhrasePerIter, maxPhraseLength, minInstanceCount)

# The chunks of text in a list
textData = list(phraseLearner.textFrame['LowercaseText'])

# Learn most salient phrases present in a large collection of documents
phraseLearner.RunConfiguration(textData,
            phraseLearner.learnedPhrases,
            addSpace=True,
            writeFile=True,
            num_workers=cpu_count()-1)
```

> [!NOTE]
> Tümcecik öğrenme adımı çoklu işleme ile uygulanır. Ancak, bunun için daha fazla CPU çekirdeği **değil** daha hızlı bir yürütme süresi anlamına gelir. Testlerimiz, performans, çoklu işlem yükü nedeniyle sekizden fazla çekirdek içeren geliştirilmiş değil. Sekiz çekirdeğe (3.6 GHz) olan bir makinede 25.000 tümcecikleri öğrenmek için yaklaşık iki ve yarım saat sürdü.
>

### <a name="topic-modeling"></a>Konu modelleme

Görünmeyen konu model kullanımı öğrenme LDA üçüncü Bu senaryoda adımıdır. [Gensim](https://radimrehurek.com/gensim/) Python paketini öğrenmek için bu adım gereklidir bir [LDA konu modeli](https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation). Bu adım için karşılık gelen not defterine `3_Topic_Model_Training.ipynb`. Ayrıca başvurabilirsiniz `step3.py` belge analizi paketin nasıl kullanılacağı için.

Bu adımda, ana bir LDA konu modeli öğrenin ve hiper parametreler ayarlamak için görevdir. Vardır ayarlanması gereken birden çok parametre ne zaman bir LDA modeli eğitmek, ancak konu sayısı en önemli parametredir. Çok az sayıda konuları öngörülere ve belge koleksiyonu olmayabilir; çok seçerken birçok "bir gövde birçok küçük ve büyük ölçüde benzer konulara aşırı kümelemesindeki" sonuçlanır. Bu senaryonun amacı, konu sayısı'nı ayarlama göstermektir. Azure Machine Learning Workbench, denemeleri farklı parametre yapılandırmasıyla farklı işlem bağlamlarının üzerinde çalıştırmak için özgürlüğü sağlar.

Belge analizi Python paketine yardımcı olmak için bazı işlevleri tanımlanmış olan en iyi konuları sayısının ölçeğini kullanıcıları şekil. Konu modelinin modellenmiş değerlendirerek ilk yaklaşımdır. Desteklenen dört değerlendirme matrisi vardır: `u_mass`, `c_v`, `c_uci`, ve `c_npmi`. Bu dört ölçüm ayrıntıları ele alınmıştır [Bu yazıda](http://svn.aksw.org/papers/2015/WSDM_Topic_Evaluation/public.pdf). İkinci tutulan genişletme gövde üzerinde perplexity değerlendirilecek bir yaklaşımdır.

'U' şekli eğri konularda en iyi sayısının ölçeğini bulmak için perplexity değerlendirme için bekleniyor. Ve dirsek konumu en iyi bir seçimdir. Farklı rasgele çekirdek ile birden çok kez değerlendirmenize ve ortalamasını almak için tavsiye edilir. Modellenmiş değerlendirmek bir 'N' sayıda olması beklenir şekil, konu sayısı artan modellenmiş artış anlamına gelir ve ardından azaltın. Bir örnek çizim perplexity, ve `c_v` modellenmiş şu şekilde gösteriliyor.

![Perplexity](./media/scenario-document-collection-analysis/Perplexity_Value.png)

![c_v modellenmiş](./media/scenario-document-collection-analysis/c_v_Coherence.png)

Modellenmiş değeri de 200 konuları sonra önemli ölçüde azaltır. ancak bu senaryoda perplexity 200 konuları sonra önemli ölçüde artırır. Bu grafikler ve gizliliğinizi korumayı taahhüt eder daha genel konular için kümelenmiş konuları bağlı olarak, 200 konuları iyi bir seçenek seçin.

Bir denemede LDA konu modeli Çalıştır veya eğitmek ve farklı bir konu sayı yapılandırmaları Çalıştır tek bir denemede birden çok LDA modelleriyle değerlendirmek eğitebilirsiniz. Bir yapılandırmanın birden çok kez çalıştırın ve ardından ortalama modellenmiş ve/veya perplexity değerlendirmeleri almak için tavsiye edilir. Belge analizi paketin nasıl kullanılacağı ile ilgili ayrıntıları bulunabilir `step3.py` dosya. Bir örnek kod parçacığını aşağıdaki gibidir.

```python
topicmodeler = TopicModeler(docs,
        stopWordFile=FUNCTION_WORDS_FILE,
        minWordCount=MIN_WORD_COUNT,
        minDocCount=MIN_DOC_COUNT,
        maxDocFreq=MAX_DOC_FREQ,
        workers=cpu_count()-1,
        numTopics=NUM_TOPICS,
        numIterations=NUM_ITERATIONS,
        passes=NUM_PASSES,
        chunksize=CHUNK_SIZE,
        random_state=RANDOM_STATE,
        test_ratio=test_ratio)

# Train an LDA topic model
lda = topicmodeler.TrainLDA(saveModel=saveModel)

# Evaluate coherence metrics
coherence = topicmodeler.EvaluateCoherence(lda, coherence_types)

# Evaluate perplexity on a held-out corpus
perplex = topicmodeler.EvaluatePerplexity(lda)
```

> [!NOTE]
> Bir LDA konu modeli eğitmek için yürütme süresi, gövde, Hiper parametre yapılandırma yanı sıra, makinedeki çekirdek boyutu birden çok etkene bağlıdır. Birden çok CPU çekirdekleri kullanılarak daha hızlı bir modeli eğitir. Ancak, aynı hyper parametresi ile daha fazla çekirdek ayarını daha az güncelleştirmeleri eğitim sırasında anlamına gelir. İçin önerilen **yakınsanmış bir LDA modeli eğitmek için en az 100 güncelleştirmeleri**. Güncelleştirme sayısı ve hiper Parametreler arasındaki ilişki, içinde ele alınmıştır [yazıya](https://groups.google.com/forum/#!topic/gensim/ojySenxQHi4) ve [yazıya](http://miningthedetails.com/blog/python/lda/GensimLDA/). Testlerimiz, yaklaşık 3 saat yapılandırmasını kullanarak 200 konularla LDA modeli eğitmek için sürdüğünü `workers=15`, `passes=10`, `chunksize=1000` 16 çekirdek (2,0 GHz) olan bir makinede.
>

### <a name="topic-summarization-and-analysis"></a>Konu özetleme ve analiz

İki not defteri belge analizi pakette hiçbir ilgili işlevlerle varken bir konu özetleme ve analiz oluşur.

İçinde `4_Topic_Model_Summarization.ipynb`, eğitilen bir LDA konu modelini temel belgesinin içeriğini özetlenme biçimini gösterir. Özetleme, adım 3'te öğrenilen LDA konu modeline uygulanır. Bu önem ya da belge saflığa ölçü konuya kullanarak bir konu kalitesi ölçmek nasıl gösterir. Bu saflığa ölçü, göründükleri belgeleri baskındır görünmeyen konuları zayıf görünmeyen konuları birçok belgeler arasında yayılabilir. daha fazla anlamsal olarak önemli olduğunu varsayar. Bu kavramı kağıt kullanıma sunulmuştur "[ses topluluğunuza özetleme için görünmeyen konu modelleme](http://people.csail.mit.edu/hazen/publications/Hazen-Interspeech11.pdf)."

Not Defteri `5_Topic_Model_Analysis.ipynb` tasarlamayla içeriğini bir belge koleksiyonu çözümleme ve belge koleksiyonu ile ilişkili diğer meta verileri karşı güncel bilgi ilişkilendirmek gösterilmektedir. Birkaç çizimleri kullanıcıların öğrenilen konu ve belge koleksiyonu daha iyi anlamanıza yardımcı olmak için bu not defteri kullanıma sunulmuştur.

Not Defteri `6_Interactive_Visualization.ipynb` öğrenilen konu modeli etkileşimli görselleştirme gösterilmektedir. Bu dört etkileşimli görselleştirme görevleri içerir.

## <a name="conclusion"></a>Sonuç

İyi bilinen metin analizi teknikleri (Bu durumda, tümcecik öğrenme ve LDA konu modelleme) güçlü bir modeli oluşturmak için nasıl kullanılacağını ve Azure Machine Learning Workbench model performansını izlemek ve sorunsuz bir şekilde çalıştırma nasıl yardımcı olabileceğini Bu gerçek dünya senaryosu vurgulanıyor daha yüksek ölçekte öğrencileriyle. Daha ayrıntılı olarak:

* Bir belge koleksiyonu işlem ve sağlam bir model derlemek için ifade öğrenme ve konu modelleme kullanın. Belgelerin koleksiyonu çok büyük ise, Azure Machine Learning Workbench kolayca ölçeklendiremezsiniz ayarlama ve inceleyin. Ayrıca, kullanıcılar, Azure Machine Learning Workbench içinde farklı işlem bağlamı kolayca denemeleri çalıştırmak için özgürlüğüne sahipsiniz.

* Azure Machine Learning Workbench, not defterlerini bir adım adım bir şekilde ve tüm bir denemeyi çalıştırmak için Python betiğini çalıştırmak için iki seçenek sağlar.

* Bilgi edinmek en iyi konuları sayısını bulmak için Azure Machine Learning Workbench'i kullanarak Hyper parametresi ayarlama gerekli. Azure Machine Learning Workbench, model performansını izlemek ve sorunsuz bir şekilde farklı öğrencileriyle daha büyük bir ölçekte alıştırmanızı yardımcı olabilir.

* Azure Machine Learning Workbench'i çalıştırma geçmişi ve öğrenilen modelleri yönetebilirsiniz. Ancak, veri uzmanları için en iyi performansa sahip Modellerinizi hızla tanımlamanıza ve betikleri ve onları oluşturmak için kullanılan verileri bulmak için etkinleştirir.

## <a name="references"></a>Başvurular

* **Timothy J. Hazen, Fred Uludağ**, [ _geliştirilmiş damıtarak konuşma bağlamında kullanılabilen konuşma konu modelleme için kısıtlanmış bir ifade ağacı Multiword tümcecikleri modelleme_](http://people.csail.mit.edu/hazen/publications/Hazen-SLT-2012.pdf). Konuşulan dili teknoloji Workshop (SLT) 2012 IEEE. IEEE, 2012.

* **Timothy J. Hazen**, [ _ses topluluğunuza özetleme için görünmeyen konu modelleme_](http://people.csail.mit.edu/hazen/publications/Hazen-Interspeech11.pdf). Uluslararası konuşma iletişim ilişkilendirmenin 12 yıllık konferansı. 2011.

* **Michael Roder, her iki Andreas Alexander Hinneburg**, [ _konu modellenmiş ölçüleri alan araştırma_](http://svn.aksw.org/papers/2015/WSDM_Topic_Evaluation/public.pdf). Web araması ve veri madenciliği sekizinci ACM uluslararası konferans bildirileri. ACM, 2015'İ TIKLATIN.
