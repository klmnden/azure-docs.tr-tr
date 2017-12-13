---
title: Koleksiyon analiz - Azure belge | Microsoft Docs
description: "Özetlemek ve belgeleri, tümcecik öğrenme, modelleme konu ve Azure ML çalışma ekranı kullanarak konu modeli çözümlemesi gibi teknikler de dahil olmak üzere büyük topluluğu analiz etme."
services: machine-learning
author: kehuan
ms.author: kehuan
ms.reviewer: garyericson, jasonwhowell, MicrosoftDocs/mlreview
ms.service: machine-learning
ms.topic: article
ms.date: 09/20/2017
ms.openlocfilehash: 49e215e723728f54a34f7c4e3a89217f16250002
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="document-collection-analysis"></a>Belge koleksiyonu çözümleme

Bu senaryo, özetlemek ve belgeleri, tümcecik öğrenme, modelleme konu ve Azure ML çalışma ekranı kullanarak konu modeli çözümlemesi gibi teknikler de dahil olmak üzere büyük topluluğu çözümlemek gösterilmiştir. Azure Machine Learning çalışma ekranı çok büyük belge koleksiyonunu kaydınızı kolay ölçek sağlar ve eğitmek ve işlem bağlamı, yerel işlem için veri bilimi sanal makinelere Spark kümesi arasında değişen birçok içinde modelleri ayarlamak için mekanizma sağlar. Kolay geliştirme Jupyter not defterleri Azure Machine Learning çalışma ekranının içinden sağlanır.

## <a name="link-to-the-gallery-github-repository"></a>Galeri GitHub deponuza bağlayın

Bu gerçek dünya senaryoları için ortak GitHub depo kod örnekleri, bu örnek için gerekli dahil olmak üzere tüm malzemeleri içerir:

[https://github.com/Azure/MachineLearningSamples-DocumentCollectionAnalysis](https://github.com/Azure/MachineLearningSamples-DocumentCollectionAnalysis)

## <a name="overview"></a>Genel Bakış

Her gün toplanan bir miktarda (özellikle yapılandırılmamış metin verileri) ile önemli bir sınama düzenlemek, arama ve bu metinleri büyük miktarlarda anlamak için kullanılır. Bu belge koleksiyonu analiz senaryosu büyük belge koleksiyonunu çözümlemek ve aşağı akış NLP görevleri etkinleştirmek için bir verimli ve otomatik uçtan uca iş akışı gösterilmektedir.

Bu senaryo tarafından teslim temel öğeleri şunlardır:

1. Belgelerden belirgin çok sözcükler tümcecik öğrenme.

1. Belge koleksiyonundaki sunulan temel konuları bulma.

1. Belgeleri temsil eden göre güncel dağıtım.

1. Düzenleme, arama ve belgeleri tasarlamayla içeriğe göre özetlemeye yöntemleri sunmak.

Bu senaryoda sunulan yöntemleri konu eğilimleri anomali, belge koleksiyonu özetleme ve benzer belge arama bulma gibi kritik endüstriyel iş yüklerinin sağlayabilir. Belge analiz, devlet Yasama, haberleri, ürün incelemeleri, müşteri geribildirimler ve bilimsel araştırma makaleleri gibi birçok farklı türlerdeki uygulanabilir.

Machine learning teknikleri/Bu senaryoda kullanılan algoritmalar şunları içerir:

1. Metin işleme ve temizleme

1. Tümcecik öğrenme

1. Konu modelleme

1. Gövde özetleme

1. Güncel eğilimleri ve anomali algılama

## <a name="prerequisites"></a>Ön koşullar

Bu örneği çalıştırmak için gereken önkoşullar aşağıdaki gibidir:

* Düzgün yüklediğinizden emin olun [Azure Machine Learning çalışma ekranı](./overview-what-is-azure-ml.md) izleyerek [yükleyin ve hızlı başlangıç oluşturma](quickstart-installation.md).

* Bu örnekte her işlem bağlam çalıştırabilir. Ancak, çalıştırmak için bir çok çekirdekli makinesinde en az önerilir 16GB bellek ve 5GB disk alanı.

## <a name="create-a-new-workbench-project"></a>Yeni bir çalışma ekranı projesi oluşturma

Bu örnek bir şablon kullanarak yeni bir proje oluşturun:
1.  Açık Azure Machine Learning çalışma ekranı
2.  Üzerinde **projeleri** sayfasında,  **+**  oturum ve seçin **yeni proje**
3.  İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri doldurun
4.  İçinde **arama proje şablonları** arama kutusu, "Belge koleksiyonu analiz" yazın ve şablonu seçin
5.  **Oluştur**'a tıklayın

## <a name="data-description"></a>Veri açıklaması

Bu senaryoda kullanılan dataset metin özetler ve BİZE Kongre tarafından yapılan her legislative eylemi için ilişkili meta verileri içerir. Veriler toplanır [GovTrack.us](https://www.govtrack.us/), Amerika Birleşik Devletleri Kongre etkinliklerini izler ve yardımcı Americans kendi Ulusal legislative işleminde katılın. Toplu veri aracılığıyla indirilebilir [bu bağlantıyı](https://www.govtrack.us/data/congress/) bu senaryoya dahil edilmeyen el ile bir betik kullanarak. Veri yüklemek nasıl ayrıntılarını bulunamadı [GovTrack API belgelerine](https://www.govtrack.us/developers/api).

### <a name="data-source"></a>Veri kaynağı

Bu senaryoda, toplanan ham verileri BİZE Kongre tarafından (önerilen faturaları ve çözümlemeleri) 1973 tarihli Haziran 2017 sunulan legislative Eylemler dizisidir (93rd 115th Congresses için). Toplanan veriler JSON biçiminde ve zengin bir legislative eylemler hakkında bilgi içerir. Başvurmak [bu GitHub bağlantıyı](https://github.com/unitedstates/congress/wiki/bills) veri alanları ayrıntılı bir açıklaması için. Bu senaryo içinde tanıtım amaç için yalnızca bir alt veri alanlarının ayıklanan JSON dosyaları. Önceden derlenmiş TSV dosyası `CongressionalDataAll_Jun_2017.tsv` kayıtları legislative işlemleri içeren bu senaryoda sağlanır. TSV dosyası otomatik olarak ya da dizüstü indirilebilir `1_Preprocess_Text.ipynb` Not Defteri klasörü altında veya `preprocessText.py` Python paketindeki.

### <a name="data-structure"></a>Veri yapısı

Veri dosyasında dokuz veri alanları vardır. Veri alan adları ve açıklamaları aşağıda listelenmiştir.

| Alan adı | Tür | Açıklama | Eksik değeri içermelidir |
|------------|------|-------------|---------------|
| `ID` | Dize | Fatura/çözüm kimliği. Bu alan [bill_type] [sayı] biçimidir-[Kongre]. Örneğin, "hconres1-93" fatura türü "hconres" anlamına gelir (temsil eden için ev eşzamanlı çözümleme başvurmak için [bu belgeyi](https://github.com/unitedstates/congress/wiki/bills#basic-information)), fatura numarası ' 1'dir 've ' 93 Kongre sayıdır'. | Hayır |
| `Text` | Dize | Fatura/çözümleme içeriği. | Hayır |
| `Date` | Dize | Fatura/çözümleme Başlangıçta önerilen tarih. Bir 'yyyy-aa-gg' biçiminde. | Hayır |
| `SponsorName` | Dize | Fatura/çözümleme önerilen birincil sponsoru adı. | Evet |
| `Type` | Dize | Birincil başlık türünü sponsorluğunu üstlenmek, 'rep' (temsilcisi) ya da 'ğ' (senator). | Evet |
| `State` | Dize | Birincil sponsoru durumu. | Evet |
| `District` | Tamsayı | Sponsoru başlığı bir temsilcisi ise birincil sponsoru bölge sayısı. | Evet |
| `Party` | Dize | Birincil sponsoru taraf. | Evet |
| `Subjects` | Dize | Üst üste fatura Kongre kitaplığı tarafından eklenen konu koşulları. Terimleri virgülle birleşir. Bu koşulları Kongre kitaplığı içinde bir insan tarafından yazılan ve fatura hakkında bilgi ilk yayımlandığında genellikle mevcut değildir. Herhangi bir zamanda eklenebilir. Bu nedenle bir fatura ömrünü sonu bazı konu artık ilgili olmayabilir. | Evet |

## <a name="scenario-structure"></a>Senaryo yapısı

Belge koleksiyonu analiz örneği iki tür sonuçlara düzenlenmiştir. İlk tür iPython tüm iş akışının adım adım açıklamaları Göster not defterlerini dizisidir. İkinci bir Python paket yanı sıra bu paketin nasıl kullanılacağı bazı kod örnekleri türüdür. Birçok kullanım durumlarına genel Python paketidir.

Bu örnekte dosyaları şu şekilde düzenlenmiştir.

| Dosya Adı | Tür | Açıklama |
|-----------|------|-------------|
| `aml_config` | Klasör | Azure Machine Learning çalışma ekranı yapılandırma klasörü başvurmak [bu belge](./experimentation-service-configuration-reference.md) ayrıntılı deneme yürütme yapılandırması için |
| `Code` | Klasör | Python betiği ve Python paketini kaydetmek için kullanılan kod klasörü |
| `Data` | Klasör | Ara dosyaları kaydetmek için kullanılan veri klasörü |
| `notebooks` | Klasör | Jupyter not defterlerini klasörü |
| `Code/documentAnalysis/__init__.py` | Python dosyası | Python paket init dosyası |
| `Code/documentAnalysis/configs.py` | Python dosyası | Önceden tanımlanmış sabitleri dahil olmak üzere belge analiz Python paketi tarafından kullanılan yapılandırma dosyası |
| `Code/documentAnalysis/preprocessText.py` | Python dosyası | Aşağı Akış görevler için verileri ön işlemek için kullanılan Python dosyası |
| `Code/documentAnalysis/phraseLearning.py` | Python dosyası | Verileri tümcecikleri öğrenin ve ham verileri dönüştürmek için kullanılan Python dosyası |
| `Code/documentAnalysis/topicModeling.py` | Python dosyası | Görünmeyen Dirichlet ayırma (LDA) konu modeli eğitmek için kullanılan Python dosyası |
| `Code/step1.py` | Python dosyası | Belge koleksiyonu analiz adım 1: metin önişle |
| `Code/step2.py` | Python dosyası | Belge koleksiyonu analiz adım 2: tümceyi öğrenme |
| `Code/step3.py` | Python dosyası | Belge koleksiyonu analiz adım 3: eğitmek ve LDA konu modelini değerlendir |
| `Code/runme.py` | Python dosyası | Örnek bir dosyada tüm adımları çalıştırın |
| `notebooks/1_Preprocess_Text.ipynb` | iPython not defteri | Metin önişle ve ham verileri dönüştürme |
| `notebooks/2_Phrase_Learning.ipynb` | iPython not defteri | Metin verilerden tümcecikleri (sonra veri dönüştürme) öğrenin |
| `notebooks/3_Topic_Model_Training.ipynb` | iPython not defteri | Tren LDA konu modeli |
| `notebooks/4_Topic_Model_Summarization.ipynb` | iPython not defteri | Eğitilmiş bir LDA konu modelini temel alan belge koleksiyonunun içeriğini özetler |
| `notebooks/5_Topic_Model_Analysis.ipynb` | iPython not defteri | Metin belgeleri koleksiyonu tasarlamayla içeriğini çözümleyebilir ve belge koleksiyonla ilişkili diğer meta verileri karşı güncel bilgi ilişkilendirmek |
| `notebooks/6_Interactive_Visualization.ipynb` | iPython not defteri | Öğrenilen konu modelinin etkileşimli Görselleştirme |
| `notebooks/winprocess.py` | Python dosyası | Dizüstü bilgisayarlar tarafından kullanılan çoklu işlem için python komut dosyası |
| `README.md` | Markdown dosyası | Benioku markdown dosyası |

### <a name="data-ingestion-and-transformation"></a>Veri alımı ve dönüştürme

Derlenmiş dataset `CongressionalDataAll_Jun_2017.tsv` Blob depolama alanına kaydedilir ve erişilebilir olduğunu hem de dizüstü bilgisayarlar ve Python komut dosyaları içinde. Veri alımı ve dönüştürme için iki adımı vardır: metin verileri ön işleme ve tümceyi öğrenme.

#### <a name="preprocess-text-data"></a>Metin verileri ön işleme

Önişlem adım temizleyin ve ham metin verileri hazırlamak için doğal dil işleme teknikleri uygulanır. Denetimsiz tümcecik öğrenme ve görünmeyen konu modelleme için bir precursor görür. Ayrıntılı adım adım açıklama not defterinde bulunabilir `1_Preprocess_Text.ipynb`. Ayrıca bir Python komut dosyası olan `step1.py` bu not defterini karşılık gelir.

Bu adımda, TSV veri dosyası Azure Blob depolama alanından indirilir ve Pandas DataFrame içeri aktarıldı. Her satır DataFrame tek bir bağlı uzun dize metin ya da 'belge' öğedir. Her bir belgenin sonra tümcelerin, tümce veya subphrases büyük olasılıkla olan parçalara metnin ayrılır. Bölme arası cümleyi veya çapraz tümceciği sözcük dizelerini tümcecikleri öğrenme kullandığınızda işlemin öğrenme tümcecik önlemek üzere tasarlanmıştır.

Hem Not Defteri ve Python paket önişlem adımı için tanımlanan birden çok işlevleri vardır. İşin çoğunu bu iki satırlar kodlarının çağırarak elde edilebilir.

```python
# Read raw data into a Pandas DataFrame
textDF = getData()

# Write data frame with preprocessed text out to TSV file
cleanedDataFrame = CleanAndSplitText(textDF, saveDF=True)
```

#### <a name="phrase-learning"></a>Tümcecik öğrenme

Tümcecik öğrenme adım belgeleri büyük topluluğu arasında anahtar tümcecikleri öğrenmek için temel bir çerçeve uygular. Başlıklı teknik incelemeye açıklanan "[Multiword tümcecikleri geliştirilmiş konu modelleme, konuşma konuşma için kısıtlı tümcecikleri ağacı modelleme](http://people.csail.mit.edu/hazen/publications/Hazen-SLT-2012.pdf)", hangi ilk olarak sunulan konuşulan dil 2012 IEEE Atölye içinde Teknoloji. Tümcecik öğrenme adımın ayrıntılı uygulama iPython not defteri gösterilen `2_Phrase_Learning.ipynb` ve Python komut dosyası `step2.py`.

Bu adım, temizlenen metin giriş olarak alır ve büyük belgeleri koleksiyonda mevcut en belirgin tümcecikleri öğrenir. Tümcecik öğrenme, üç görevlere bölünür yinelemeli bir işlemdir: n-gram sayısı, rank ağırlıklı Pointwise karşılıklı bilgilere göre bağlı sözcüklerin olası tümcecikleri ve metin tümceciği yeniden yazın. Belirtilen tümcecikleri öğrenilen kadar üç bu görevleri birden çok yinelemelerini sıralı olarak yürütülür.

Belge analiz Python paket Python sınıfı `PhraseLearner` tanımlanan `phraseLearning.py` dosya. Tümcecikleri öğrenmek için kullanılan kod parçacığı aşağıdadır.

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
> Tümcecik öğrenme adım ile çoklu işlem uygulanır. Ancak, daha fazla CPU çekirdekleri yapmak **değil** daha hızlı bir yürütme süresi anlamına gelir. Bizim testlerinde performans, çoklu işlem yükü nedeniyle birden fazla sekiz çekirdeği ile geliştirilmiş değil. Sekiz Çekirdeği (3,6 GHz) olan bir makinede 25.000 tümcecikleri öğrenmek için yaklaşık iki ve bir yarım saat sürdü.
>

### <a name="topic-modeling"></a>Konu modelleme

Bir görünmeyen konu model kullanımı öğrenme LDA üçüncü Bu senaryoda bir adımdır. [Gensim](https://radimrehurek.com/gensim/) Python paket öğrenmek için bu adım gereklidir bir [LDA konu modeli](https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation). Bu adım için karşılık gelen not defteri `3_Topic_Model_Training.ipynb`. Ayrıca başvurabilirsiniz `step3.py` belge analiz paketin nasıl kullanılacağı için.

Bu adımda, ana LDA konu modeli öğrenin ve hiper parametrelerini ayarlamak için bir görevdir. Vardır ayarlanması gereken birden çok parametre ne zaman bir LDA modeli eğitmek, ancak en önemli parametre konuları sayısıdır. Çok az konuları Insight belge koleksiyonuna sahip olmayabilir; çok seçerken birçok "gövde çok sayıda küçük, yüksek oranda benzer konulara aşırı kümelemesinde" neden olur. Bu senaryo amacı konuların sayısını ayarlamak nasıl göstermektir. Azure Machine Learning çalışma ekranı denemeler farklı parametre yapılandırmayla farklı işlem bağlamları üzerinde çalıştırmak özgürlüğü sağlar.

Belge analiz Python paket yardımcı olmak için birkaç işlevleri tanımlanmış olan kullanıcılar konu en iyi sayısı şekil. İlk konu modeli tutarlılık değerlendirerek bir yaklaşımdır. Desteklenen dört değerlendirme matrisleri vardır: `u_mass`, `c_v`, `c_uci`, ve `c_npmi`. Bu dört ölçümün ayrıntılarını ele alınmıştır [bu kağıt](http://svn.aksw.org/papers/2015/WSDM_Topic_Evaluation/public.pdf). Tutulan çıkış gövde üzerinde perplexity değerlendirmek için ikinci bir yaklaşım kullanılır.

Perplexity değerlendirme için en iyi konu sayısı bulmak için 'U' Şekil eğri beklenir. Ve dirsek konumu en iyi seçimdir. Birden çok kez farklı rastgele kök ile değerlendirmek ve ortalama almak için tavsiye edilir. Tutarlılık değerlendirmek bir 'n' olması bekleniyor şekil, konu sayısı artan tutarlılık artar anlamına gelir ve ardından azaltın. Bir örnek çizim perplexity, ve `c_v` tutarlılık şu şekilde gösteriliyor.

![Perplexity](./media/scenario-document-collection-analysis/Perplexity_Value.png)

![c_v tutarlılık](./media/scenario-document-collection-analysis/c_v_Coherence.png)

Bu senaryoda, tutarlılık değeri de 200 konuları sonra önemli ölçüde azaltır perplexity 200 konuları sonra önemli ölçüde artırır. Bu grafikler ve karşı daha fazla genel konularına desire kümelenmiş konuları bağlı olarak, iyi bir seçenek 200 konuları olmalıdır seçin.

Bir deneme LDA konu modelinde Çalıştır veya eğitmek ve farklı konu sayı çalıştıran tek bir deneme yapılandırmalarında LDA modelleriyle değerlendirmek eğitim yapabilirsiniz. Bir yapılandırma için birden çok kez çalıştırmak ve ortalama tutarlılık ve/veya perplexity değerlendirmeleri almak için tavsiye edilir. Belge analiz paketin nasıl kullanılacağı ayrıntılarını bulunabilir `step3.py` dosya. Bir örnek kod parçacığını aşağıdaki gibidir.

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
> Bir LDA konu modeli eğitmek için yürütme süresi, makinedeki çekirdek sayısının yanı sıra gövde, Hiper parametre yapılandırma boyutu gibi birden çok faktöre bağlıdır. Birden çok CPU çekirdeği kullanarak daha hızlı bir model eğitir. Ancak, aynı hiper parametresiyle daha fazla sayıda çekirdek ayarını daha az güncelleştirmeler eğitim sırasında anlamına gelir. İçin önerilen **yakınsanmış LDA modeli eğitmek için en az 100 güncelleştirmeleri**. Güncelleştirme sayısı ve hiper parametreleri arasındaki ilişki içinde ele alınmıştır [bu post](https://groups.google.com/forum/#!topic/gensim/ojySenxQHi4) ve [bu post](http://miningthedetails.com/blog/python/lda/GensimLDA/). Bizim testlerinde yapılandırmasını kullanarak 200 konularda LDA modeli eğitmek için yaklaşık 3 saat sürdü `workers=15`, `passes=10`, `chunksize=1000` 16 çekirdek (2,0 GHz) olan bir makinede.
>

### <a name="topic-summarization-and-analysis"></a>Konu özetleme ve çözümleme

İki dizüstü bilgisayarlar belge analiz paketinde karşılık gelen hiçbir işlevleri varken bir konu özetleme ve analiz oluşur.

İçinde `4_Topic_Model_Summarization.ipynb`, eğitilen LDA konu modelini temel alan belgesinin içeriğini özetlemek nasıl gösterir. Özetleme, adım 3'te öğrenilen LDA konu modeline uygulanır. Önem derecesi ya da belge olmak üzere ölçü konuya kullanarak bir konu kalitesi ölçmek nasıl gösterir. Bu olmak üzere ölçü göründükleri belgeleri baskındır görünmeyen konuları zayıf görünmeyen konuları birçok belgeler arasında yayılan daha fazla anlamsal olarak önemli olduğunu varsayar. Bu kavram yazıda sunulmuştur "[ses gövde özetleme için görünmeyen konu modelleme](http://people.csail.mit.edu/hazen/publications/Hazen-Interspeech11.pdf)."

Not Defteri `5_Topic_Model_Analysis.ipynb` belgeleri topluluğu güncel içeriği çözümlemek ve belge koleksiyonla ilişkili diğer meta verileri karşı güncel bilgi ilişkilendirmek gösterilmektedir. Birkaç çizimleri kullanıcıların öğrenilen konu ve belge koleksiyonu daha iyi anlamasına yardımcı olmak için bu not defterinde sunulur.

Not Defteri `6_Interactive_Visualization.ipynb` öğrenilen konu modeli etkileşimli görselleştirme gösterilmektedir. Dört etkileşimli görselleştirme görevleri içerir.

## <a name="conclusion"></a>Sonuç

Bu gerçek dünya senaryosu sağlam bir modeli oluşturmak için bilinen metin analytics teknikleri (Bu durumda, tümcecik öğrenme ve LDA konu modelleme) kullanmayı ve Azure Machine Learning çalışma ekranı modeli performansını izlemek ve sorunsuz bir şekilde çalıştırmak nasıl yardımcı olabileceği önemli noktalar yüksek ölçekli öğrencileriyle. Daha ayrıntılı olarak:

* Tümcecik öğrenme ve konu modelleme belgeleri topluluğu işlemek ve sağlam bir model oluşturmak için kullanın. Belgeleri topluluğu çok büyük ise, Azure Machine Learning çalışma ekranı kolayca onu ölçeklendirebilirsiniz ayarlama ve çıkış. Ayrıca, kullanıcıların farklı işlem bağlamı kolayca Azure Machine Learning çalışma ekranının içinden denemeleri çalıştırmak için serbestçe.

* Azure Machine Learning çalışma ekranı not defterlerini bir adım adım şekilde ve tüm bir denemeyi çalıştırmak için Python betiği çalıştırmak için her iki seçenek sağlar.

* Konu en iyi sayısı bulmak için Azure Machine Learning çalışma ekranı kullanarak Hyper-parametre ayarlama öğrenmek gerekli. Azure Machine Learning çalışma ekranı modeli performansını izlemek ve sorunsuz bir şekilde yüksek ölçekte farklı öğrencileriyle çalıştırmak yardımcı olabilir.

* Azure Machine Learning çalışma ekranı çalıştırma geçmişi ve öğrenilen modelleri yönetebilirsiniz. Veri bilimcilerine modelleri gerçekleştirmek için en iyi hızlı bir şekilde tanımlamak ve komut dosyalarını ve bunları oluşturmak için kullanılan veri bulmak için etkinleştirir.

## <a name="references"></a>Başvurular

* **Attila J. Hazen, Gamze Uludağ**, [ _için kısıtlanmış tümcecikleri ağacı Multiword tümcecikleri modelleme geliştirilmiş konu modelleme konuşma konuşma_](http://people.csail.mit.edu/hazen/publications/Hazen-SLT-2012.pdf). Dil teknolojisi Atölye (SLT) 2012 IEEE söylenir. IEEE, 2012.

* **Attila J. Hazen**, [ _ses gövde özetleme görünmeyen konu modelleme_](http://people.csail.mit.edu/hazen/publications/Hazen-Interspeech11.pdf). Uluslararası konuşma iletişimi ilişki 12 yıllık konferans. 2011.

* **Michael Roder, her iki Andreas, Alexander Hinneburg**, [ _konu tutarlılık ölçüleri alanı keşfetme_](http://svn.aksw.org/papers/2015/WSDM_Topic_Evaluation/public.pdf). Sekizinci ACM uluslararası konferans Web araması ve veri madenciliği bildirileri. ACM, 2015'İ TIKLATIN.
