---
title: "Soru- cevap Azure Machine Learning çalışma ekranı kullanarak bir eşleşen | Microsoft Docs"
description: "Çeşitli etkili machine learning yöntemleri açık eşleştirmek için kullanılacak önceden varolan sorguları sona erdi nasıl SSS soru/çiftleri yanıtlar."
services: machine-learning
documentationcenter: 
author: mezmicrosoft
editor: mezmicrosoft
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: mez
ms.manager: tihazen
ms.openlocfilehash: 8edc21fb8f42ee5897c4e938045cc1f42aedb3ce
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
#  <a name="q--a-matching-using-azure-machine-learning-workbench"></a>Soru- cevap Azure Machine Learning çalışma ekranı kullanarak bir eşleştirme
Açık sona erdi sorulara yanıt verilmesi zordur ve genellikle konu uzmanları (SME) gelen el ile çaba gerektirir. İç SME taleplerini azaltmaya yardımcı olmak için şirketler genellikle kullanıcılar yardımcı olan bir araç olarak sık sorulan sorular (SSS) listesi oluşturun. Bu örnek SSS soru/yanıt çiftleri önceden varolan açık sona erdi sorguları eşleştirmek için çeşitli etkili makine öğrenme yöntemlerini gösterir. Bu örnek, Azure Machine Learning çalışma ekranı kullanarak çözüm oluşturmak için bir kolay geliştirme sürecini gösterir. 

## <a name="link-to-the-gallery-github-repository"></a>Galeri GitHub deponuza bağlayın
[https://github.com/Azure/MachineLearningSamples-QnAMatching](https://github.com/Azure/MachineLearningSamples-QnAMatching)

## <a name="overview"></a>Genel Bakış

Bu örnekte genellikle sık sorulan sorular (diğer bir deyişle, SSS) listesi veya Q sağlanan soru ve yanıt (Q & A) çiftleri önceden varolan kullanıcı sorularını eşleme sorunu giderir & Web sitelerinde mevcut bir çiftleri ister [yığını Taşma](https://stackoverflow.com/). Bir soru en benzer soruya yanıt bulma gibi doğru yanıta eşleştirmek için birçok yaklaşım vardır. Her yanıt SSS birden çok anlam olarak eşdeğer soruları yanıtlayabilir varsayılarak ancak, bu örnekte açık sona erdi soruları tarafından daha önce sorulan sorulara eşleştirilir.

Bu çözümü sağlamak için gerekli anahtar adımlar aşağıdaki gibidir:

1. Temiz ve işlem metin verileri.
2. Bağımsız olarak kabul olduğunda daha dizisi görüntülendiğinde daha fazla bilgi sağlayan çok word dizileri bilgilendirici tümcecikleri öğrenin.
3. Özellik metin verileri ayıklayın.
4. Metin sınıflandırma modelleri eğitme ve model performansını değerlendirmek.


## <a name="prerequisites"></a>Ön koşullar

Bu örneği çalıştırmak için gereken önkoşullar aşağıdaki gibidir:

1. Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz deneme kullanılabilir).
2. Yüklü bir kopyasını [Azure Machine Learning çalışma ekranı](./overview-what-is-azure-ml.md) aşağıdaki [hızlı başlangıç Yükleme Kılavuzu](./quickstart-installation.md) programı yüklemek ve bir çalışma alanı oluşturmak için.
3. Bu örnekte her işlem bağlam çalıştırabilir. Ancak, çalıştırmak için bir çok çekirdekli makinesinde en az önerilir 16GB bellek ve 5GB disk alanı.

## <a name="create-a-new-workbench-project"></a>Yeni bir çalışma ekranı projesi oluşturma

Bu örnek bir şablon kullanarak yeni bir proje oluşturun:
1.  Açık Azure Machine Learning çalışma ekranı
2.  Üzerinde **projeleri** sayfasında,  **+**  oturum ve seçin **yeni proje**
3.  İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri doldurun
4.  İçinde **arama proje şablonları** arama kutusu, "Q & A eşleşen" yazın ve şablonu seçin
5.  **Oluştur**'a tıklayın

## <a name="data-description"></a>Veri açıklaması

Bu örnekte kullanılan veri kümesi bir yığın Exchange verileri depolandığı döküm olduğu [archive.org](https://archive.org/details/stackexchange). Bu verileri bir anonim kullanıcı katkıda bulunan tüm içerik üzerinde dökümüdür [yığın Exchange ağ](https://stackexchange.com/). Her sitede ağ üzerinden XML dosyaları oluşan ayrı bir arşiv daraltılmış olarak biçimlendirilmiş 7-zip bzıp2 sıkıştırma kullanılarak. Her site arşiv gönderileri, kullanıcılar, oyları, yorumlar, PostHistory ve PostLinks içerir. 

### <a name="data-source"></a>Veri kaynağı

Bu örnekte [yazılarını veri (10 GB)](https://archive.org/download/stackexchange/stackoverflow.com-Posts.7z) ve [PostLinks verileri (515 MB)](https://archive.org/download/stackexchange/stackoverflow.com-PostLinks.7z) yığın taşması sitesinden. Tam şeması bilgi için bkz: [readme.txt](https://ia800500.us.archive.org/22/items/stackexchange/readme.txt). 

`PostTypeId` Gönderileri veri alanına gösteren bir post olup bir `Question` veya bir `Answer`. `PostLinkTypeId` PostLinks veri alanında iki gönderileri bağlantılı veya yinelenen gösterir. Soru postalar genellikle bir soru diğer benzer/yinelenen sorularını kategorilere ayırma sözcükler bazı etiketler içerir. Bazı etiket vardır yüksek sıklıkta gibi `javascript`, `java`, `c#`, `php` vb., soru postalar artık kullanılmayan çok sayıda oluşur. Bu örnek, bir alt kümesini Q & çiftleri ile `javascript` etiketi ayıklanır.

Additionionally, soru post birden çok yanıt gönderileri veya yinelenen soru gönderileri ilgili olabilir. Bu iki veri kümesi ile ilgili SSS listesinden oluşturmak için bazı veri toplama ölçütlerini olarak kabul edilir. Bu üç derlenmiş veri kümesi, bu örnekte bulunmayan bir SQL betiği kullanarak seçilir. Sonuçta elde edilen veri açıklaması aşağıdaki gibidir:

- `Original Questions (Q)`: Soru gönderileri sorular ve yığın taşması sitesinde yanıt verdi.
- `Duplications (D)`: Soru yazılarını yinelenen önceden mevcut olan diğer sorular (`PostLinkTypeId = 3`), özgün sorular verilmiştir. Yinelenen özgün soru sağlanan yanıt ayrıca yeni yinelenen soru yanıtlar algılama özgün sorulara anlam olarak eşdeğer olarak kabul edilir.
- `Answers (A)`: Birden fazla yanıt gönderimleri bağlayın her özgün soru ve kendi yinelenen. Bu örnek yalnızca özgün yazar tarafından ya da kabul yanıt post seçer (tarafından filtre `AcceptedAnswerId`) veya en yüksek skoru (tarafından derece `Score`) özgün soruları içinde. 

Bu üç veri kümeleri birleşimi burada bir yanıt (A) bir özgün soru (Q) ve birden çok yinelenen soruları (D) tarafından aşağıdaki veri diyagramda gösterildiği gibi eşlenmiş soru- cevap çiftleri oluşturur:

<table><tr><td><img id ="data_diagram" src="media/scenario-qna-matching/data_diagram.png"></td></tr></table>

### <a name="data-structure"></a>Veri yapısı

Veri şeması ve üç veri kümelerini, doğrudan indirme bağlantıları aşağıdaki tabloda bulunabilir:

| Veri kümesi | Alan | Tür | Açıklama
| ----------|------------|------------|--------
| [Sorular](https://bostondata.blob.core.windows.net/stackoverflow/orig-q.tsv.gz) | Kimlik | Dize | Benzersiz soru kimliği (birincil anahtar)
|  | AnswerId | Dize | Soru her benzersiz yanıt kimliği
|  | Text0 | Dize | Sorusunun başlık ve gövde dahil olmak üzere ham metin verileri
|  | CreationDate | zaman damgası | Sorunun ne zaman istedi, zaman damgası
| [yinelenenler](https://bostondata.blob.core.windows.net/stackoverflow/dup-q.tsv.gz) | Kimlik | Dize | Benzersiz çoğaltma kimliği (birincil anahtar)
|  | AnswerId | Dize | Çoğaltma ile ilişkili yanıt kimliği
|  | Text0 | Dize | Çoğaltma 's başlık ve gövde dahil olmak üzere ham metin verileri
|  | CreationDate | zaman damgası | Çoğaltma zaman istedi, zaman damgası
| [yanıtlar](https://bostondata.blob.core.windows.net/stackoverflow/ans.tsv.gz)  | Kimlik | Dize | Benzersiz yanıt kimliği (birincil anahtar)
|  | text0 | Dize | Yanıt ham metin verileri


## <a name="scenario-structure"></a>Senaryo yapısı

Soru- cevap eşleşen örnek dosyaları üç tür sunulur. İlk tür, tüm iş akışının adım adım açıklamaları Göster Jupyter Not dizisidir. Python dosyaları kümesini içeren özel Python modülleri tümcecik öğrenme ve özelliği ayıklama için ikinci türüdür. Bu Python modülleri değil yalnızca bu örnek aynı zamanda diğer kullanım örnekleri sunmak için genel. Hyper-parametreleri ayarlamak ve Azure Machine Learning çalışma ekranı kullanarak modeli performansını izlemek için Python dosyaları kümesi üçüncü türüdür.

Bu örnekte dosyaları şu şekilde düzenlenmiştir.

| Dosya adı | Tür | Açıklama
| ----------|------------|--------
| `Image` | Klasör | Görüntüler için Benioku dosyasını kaydetmek için kullanılan klasörü
| `notebooks` | Klasör | Jupyter not defterleri klasörü
| `modules` | Klasör | Python modülleri klasörü
| `scripts` | Klasör | Python dosyaları klasörü
| `notebooks/Part_1_Data_Preparation.ipynb` | Jupyter Notebook | Örnek veri erişim, metin önceden işleyebilir ve eğitim hazırlamak ve veri kümelerini test
| `notebooks/Part_2_Phrase_Learning.ipynb` | Jupyter Notebook | Metin sözcük paketi (BOWs) Beyanları simgeleştirilecek ve bilgilendirici tümcecikleri öğrenin
| `notebooks/Part_3_Model_Training_and_Evaluation.ipynb` | Jupyter Notebook | Özellikleri ayıklamak, metin sınıflandırma modelleri ve değerlendirme modeli performans eğitme
| `modules/__init__.py` | Python dosyası | Python paket init dosyası
| `modules/phrase_learning.py` | Python dosyası | Ham verileri dönüştürün ve bilgilendirici tümcecikleri bilgi almak için kullanılan Python modülleri
| `modules/feature_extractor.py` | Python dosyası | Özellikler model eğitim için Python modülleri Ayıkla
| `scripts/naive_bayes.py` | Python dosyası | Python Naive Bayes modelinde Hyper-parametreleri ayarlamak için
| `scripts/random_forest.py` | Python dosyası | Hyper-parametrelerinde rastgele orman modeli ayarlamak için Python
| `README.md` | Markdown dosyası | Benioku markdown dosyası

> [!NOTE]
> Not defterlerini dizi Python 3.5 altında oluşturulur.
> 

### <a name="data-ingestion-and-transformation"></a>Veri alımı ve dönüştürme

Üç derlenmiş veri kümelerini bir Blob depolama alanına depolanır ve içinde alınan `Part_1_Data_Preparation.ipynb` dizüstü bilgisayar.  

Metin sınıflandırma modelleri eğitim önce soruları metinde temizlendi ve kod parçacıkları dışlamak için ön işlemesi yapılan. Denetimsiz tümcecik öğrenme bilgilendirici çok word sıraları öğrenmek için eğitim malzemesi uygulanır. Şu tümceciklerden metin sınıflandırma modelleri tarafından kullanılan aşağı akış sözcükler paketi (BOWs) featurization tek bileşik word birimleri olarak temsil edilir.

Metin ön işleme ve tümcecik öğrenme ayrıntılı adım adım açıklamalarını not defterlerinde bulunabilir `Part_1_Data_Preparation.ipynb` ve `Part_2_Phrase_Learning.ipynb`sırasıyla.

### <a name="modeling"></a>Modelleme

Bu örnek önceden varolan soru- cevap bir çiftleri karşı yeni sorular eğitim metin tarafından burada önceden mevcut olan her soru- cevap çifti benzersiz bir sınıf, yinelenen soruların her soru- cevap çifti için bir alt kullanılabilir olarak sınıflandırma modelleri Puanlama amacıyla tasarlanmıştır eğitim malzemesi. Örnekte, özgün sorular ve yinelenen soruların %75 için eğitim korunur ve en son gönderilen % 25'ini yinelenen sorular tutulan Değerlendirme verileri olarak genişletme.

Sınıflandırma modeli de dahil olmak üzere üç temel sınıflandırıcı toplamak için bir ensemble yöntemini kullanır **Naive Bayes**, **destek vektör makinesi**, ve **rastgele orman**. Her bir temel sınıflandırıcı içinde `AnswerId` sınıfı etiket ve BOWs Beyanları kullanılabilir olarak özellikleri olarak kullanılır.

Model eğitimi işlem gösterilmiştir `Part_3_Model_Training_and_Evaluation.ipynb`.

### <a name="evaluation"></a>Değerlendirme

İki farklı değerlendirme ölçümleri performansını değerlendirmek için kullanılır. 
1. `Average Rank (AR)`: Burada doğru yanıt bulundu (dışında 103 yanıt sınıfları tam kümesi) alınan soru- cevap çiftleri listesinde ortalama konumu belirtir. 
2. `Top 3 Percentage`: test yüzdesi sorularını doğru yanıt döndürülen ilk üç seçenek de listesi derece alınabilir olduğunu gösterir. 

Değerlendirme içinde gösterilen `Part_3_Model_Training_and_Evaluation.ipynb`.


## <a name="conclusion"></a>Sonuç

Bu örnek iyi bilinen metin analytics teknikler, tümcecik öğrenme ve metin sınıflandırma gibi sağlam bir modeli oluşturmak için nasıl kullanılacağını vurgular. Ayrıca, Azure Machine Learning çalışma ekranı ile etkileşimli çözüm geliştirme ve izleme modeli performansı nasıl yardımcı olabileceğini gösterir. 

Bu örnekte anahtar bazı önemli şunlardır:

- Etkili bir şekilde tümcecik öğrenme ve metin sınıflandırma modelleri ile soru ve yanıt eşleşen sorun çözülebilir.
- Azure Machine Learning çalışma ekranı ve Jupyter not defteri ile etkileşimli modeli geliştirme gösterimi.
- Azure Machine Learning çalışma ekranı çalıştırma geçmişi ve karşılaştırma amaçları için değerlendirme ölçümleri günlüğü ile öğrenilen modelleri yönetir. Bu özellikler hızlı parametre hyper ayarlama etkinleştirir ve modelleri gerçekleştirmek için en iyi belirlenmesine yardımcı olur.


## <a name="references"></a>Başvurular

Attila J. Hazen, Gamze Uludağ [ _için kısıtlanmış tümcecikleri ağacı Multiword tümcecikleri modelleme geliştirilmiş konu modelleme konuşma konuşma_](http://people.csail.mit.edu/hazen/publications/Hazen-SLT-2012.pdf). Dil teknolojisi Atölye (SLT) 2012 IEEE söylenir. IEEE, 2012.

Attila J. Hazen [ _MCE eğitim teknikler konuşulan ses belgeleri konu tanımlaması için_ ](http://ieeexplore.ieee.org/abstract/document/5742980/) ses, konuşma ve dil, Cilt 19, Hayır işleme IEEE işlemlerde. 8, pp. 2451-2460, Kasım 2011.
